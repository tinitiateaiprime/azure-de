# Azure IAM

## Table of Contents
1. [Prerequisites & Setup](#prerequisites--setup)
2. [IAM Mental Model in Azure](#iam-mental-model-in-azure)
3. [Entra ID vs Azure RBAC](#entra-id-vs-azure-rbac)
4. [Azure Roles: Built-in & Custom](#azure-roles-built-in--custom)
5. [Role Assignments & Scopes](#role-assignments--scopes)
6. [Users, Groups, and Group-Based Access](#users-groups-and-group-based-access)
7. [Service Principals (App Registrations)](#service-principals-app-registrations)
8. [Managed Identities (System/User Assigned)](#managed-identities-systemuser-assigned)
9. [Data Plane vs Control Plane](#data-plane-vs-control-plane)
10. [Deny Assignments, Locks & Azure Policy](#deny-assignments-locks--azure-policy)
11. [Conditional Access & MFA](#conditional-access--mfa)
12. [Privileged Identity Management (PIM)](#privileged-identity-management-pim)
13. [Access Reviews & Entitlement Mgmt](#access-reviews--entitlement-mgmt)
14. [Auditing: Activity Logs, Sign-in Logs](#auditing-activity-logs-sign-in-logs)
15. [Best Practices Checklist](#best-practices-checklist)
16. [Glossary](#glossary)

---

## Prerequisites & Setup

- Azure subscription with permissions to create resources (Contributor or Owner for labs).
- **Azure CLI** installed.
- Sign in to Azure: `az login`.

### Install & Login
```bash
# macOS (Homebrew)
brew update && brew install azure-cli

# Ubuntu/Debian (script)
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# Login
az login --use-device-code
az account set --subscription "<SUBSCRIPTION_NAME_OR_ID>"
```
**Expected output:**
- A browser/device-code flow opens; after auth, JSON listing your subscriptions and `tenantId`.

---

## IAM Mental Model in Azure

- **Who are you?** → **Principal** (user, group, service principal, managed identity).
- **What can you do?** → **Role definitions** (sets of allowed actions) **assigned** at a **scope** (subscription, resource group, resource).
- **How evaluated?** → Default **no access**. **Role assignments** grant allows. **Deny assignments** (from some platform features) and **resource locks** can block actions even if role allows.

---

## Entra ID vs Azure RBAC

**Entra ID (Directory plane):**
- Identities (users, groups, devices), **directory roles** (e.g., Global Administrator), Conditional Access, MFA, PIM for directory roles.

**Azure RBAC (Resource plane):**
- Access to Azure resources via **role assignments** at a **scope**. Roles include `Reader`, `Contributor`, `Owner`, and many service-specific roles.

**Portal Steps**
- Entra admin center: manage **users/groups/CA**.
- Azure portal: manage **subscriptions/RGs/resources** and **role assignments**.

**CLI Steps**
```bash
# List subscriptions
az account list -o table
# List directory roles (requires sufficient permissions)
az role assignment list --all > /tmp/assignments.json
```
**Expected output:**
- Tables/JSON of subscriptions or role assignments.

---

## Azure Roles: Built-in & Custom

**Theory**
- A role definition has **Actions/NotActions** (management plane) and **DataActions/NotDataActions** (data plane).
- **Built-in roles** (e.g., Reader, Contributor, Owner, Storage Blob Data Reader).
- **Custom roles** let you tailor permissions.

### Portal Steps: View Built-in Roles
1. Azure portal → **Subscriptions** → select subscription → **Access control (IAM)** → **Roles**.
2. Search and open `Storage Blob Data Reader` to view allowed `DataActions`.

### CLI Steps: Create a Custom Role (read-only over specific provider)
```bash
# 1) Export a template of Reader to start from (optional)
az role definition list --name Reader > reader.json

# 2) Create a minimal custom role JSON
cat > custom-read-vnet.json <<'JSON'
{
  "Name": "Network Read Only (Custom)",
  "IsCustom": true,
  "Description": "Read-only on Microsoft.Network resources",
  "Actions": [
    "Microsoft.Network/*/read"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/<SUBSCRIPTION_ID>"
  ]
}
JSON

# 3) Create the custom role
az role definition create --role-definition custom-read-vnet.json
```
**Expected output:**
- JSON of the new role with an `id`, `name`, and `permissions` arrays.

---

## Role Assignments & Scopes

**Theory**
- **Scopes**: Subscription → Resource Group → Resource (inheritance flows down).
- **Role assignment** = `principalId` + `roleDefinitionId` + `scope`.

### Portal Steps: Assign Role at Resource Group
1. Create a resource group: **Resource groups** → **Create** → Name `rg-demo`.
2. Open `rg-demo` → **Access control (IAM)** → **Add** → **Add role assignment** → choose `Reader` → assign to user/group/service principal.

### CLI Steps
```bash
# Create a resource group
az group create -n rg-demo -l eastus

# Look up a principal (user or group) by display name
az ad user list --display-name "analyst1" -o table
# OR for a group
az ad group list --display-name "DataReaders" -o table

# Assign Reader at the RG scope
PRINCIPAL_ID=<objectId-of-user-or-group>
SUBS=<your-subscription-id>
az role assignment create \
  --assignee-object-id $PRINCIPAL_ID \
  --assignee-principal-type User \
  --role Reader \
  --scope /subscriptions/$SUBS/resourceGroups/rg-demo
```
**Expected output:**
- JSON with `roleDefinitionName`, `principalId`, `scope`.

---

## Users, Groups, and Group-Based Access

**Theory**
- Grant roles to **groups** and manage membership, not per-user.
- Supports **dynamic groups** (based on user attributes) for scale.

### Portal Steps
1. Entra admin center → **Groups** → **New group** → Security group `DataReaders`.
2. Add members (users) to `DataReaders`.
3. Azure portal → Assign resource roles to the **group** at the necessary scope.

### CLI Steps
```bash
# Create a group
az ad group create --display-name DataReaders --mail-nickname datareaders
# Add a member
USER_ID=$(az ad user show --id analyst1@contoso.com --query id -o tsv)
GROUP_ID=$(az ad group show --group DataReaders --query id -o tsv)
az ad group member add --group $GROUP_ID --member-id $USER_ID
# Assign role to the group
SUBS=<your-subscription-id>
az role assignment create \
  --assignee-object-id $GROUP_ID \
  --assignee-principal-type Group \
  --role "Storage Blob Data Reader" \
  --scope /subscriptions/$SUBS/resourceGroups/rg-demo
```
**Expected output:**
- JSON for group and role assignment; empty output for member add.

---

## Service Principals (App Registrations)

**Theory**
- **Application (App registration)** represents an app; a **Service Principal (SP)** is the instance of that app in a tenant used for authN/authZ.
- Use **client secret** or **certificate** credentials.

### Portal Steps: Quick App + Secret
1. Entra admin center → **App registrations** → **New registration** → Name `sp-demo` → Register.
2. **Certificates & secrets** → **New client secret** (copy value once).
3. Azure portal → Assign a role to the **service principal** at a scope.

### CLI Steps: Create SP with RBAC in one go
```bash
SUBS=<your-subscription-id>
az ad sp create-for-rbac \
  --name sp-demo \
  --role Reader \
  --scopes /subscriptions/$SUBS/resourceGroups/rg-demo \
  --sdk-auth
```
**Expected output:**
- JSON containing `clientId`, `clientSecret`, `tenantId`, `subscriptionId`. **Store securely**.

---

## Managed Identities (System/User Assigned)

**Theory**
- **System-assigned** identity lives with a resource (VM, Function, App Service). Lifecycle tied to the resource.
- **User-assigned** identity is standalone; attach to multiple resources.
- Use them to authenticate to Azure services **without secrets**.

### Portal Steps: VM with System-Assigned MI → Read Blobs
1. Create a VM (Linux) → **Management** tab → **Identity** → System assigned **On**.
2. Create/storage account & container.
3. Grant the VM’s identity **Storage Blob Data Reader** role at storage account scope.

### CLI Steps
```bash
# Create a storage account & container
SUBS=<your-subscription-id>
RG=rg-demo
LOC=eastus
ST=st${RANDOM}demo
az storage account create -n $ST -g $RG -l $LOC --sku Standard_LRS
az storage container create --account-name $ST -n reports

# Create a VM and enable system-assigned identity
az vm create -n vm-demo -g $RG --image Ubuntu2204 --generate-ssh-keys
az vm identity assign -g $RG -n vm-demo

# Get the VM's principalId
MI_PRINCIPAL=$(az vm show -g $RG -n vm-demo --query identity.principalId -o tsv)
# Assign Storage Blob Data Reader at storage account scope
az role assignment create \
  --assignee-object-id $MI_PRINCIPAL \
  --assignee-principal-type ServicePrincipal \
  --role "Storage Blob Data Reader" \
  --scope $(az storage account show -n $ST -g $RG --query id -o tsv)
```
**Expected output:**
- JSONs confirming identity assignment and role assignment; container creation returns `created: true/false`.

> **Test inside the VM** using IMDS token + Azure SDK/CLI to read blobs (no secrets required).

---

## Data Plane vs Control Plane

**Theory**
- **Control plane** actions manage Azure resources (e.g., create VM). Governed by **Actions/NotActions**.
- **Data plane** actions operate within a resource (e.g., read a blob). Governed by **DataActions/NotDataActions**.
- Some services use separate data plane auth models (e.g., Storage supports **Azure AD RBAC** and **account keys/SAS**).

### CLI Steps: Assign Storage Data Role
```bash
SUBS=<your-subscription-id>
RG=rg-demo
ST=<your-storage-account>
GROUP_ID=$(az ad group show --group DataReaders --query id -o tsv)
az role assignment create \
  --assignee-object-id $GROUP_ID \
  --assignee-principal-type Group \
  --role "Storage Blob Data Reader" \
  --scope $(az storage account show -n $ST -g $RG --query id -o tsv)
```
**Expected output:**
- Role assignment JSON with `roleDefinitionName: Storage Blob Data Reader`.

---

## Deny Assignments, Locks & Azure Policy

**Theory**
- **Deny assignments** (used by certain platform features) **block** actions even if a role allows them.
- **Resource locks** (`ReadOnly`, `CanNotDelete`) protect critical resources from modification/deletion.
- **Azure Policy** evaluates compliance and can **deny** non-compliant changes at request time.

### Portal Steps: Create a Lock
1. Open a resource group → **Locks** → **Add** lock → Name `protect-rg` → **Read-only** or **Delete**.

### CLI Steps: Lock & Policy Assignment
```bash
# Add a delete lock at RG scope
az lock create --name protect-rg --lock-type CanNotDelete \
  --resource-group rg-demo

# Create a built-in policy assignment to deny public IP on NICs (example)
POLICY_DEF=$(az policy definition list --query "[?contains(displayName, 'Public IP') && policyType=='BuiltIn']|[0].name" -o tsv)
az policy assignment create \
  --name deny-public-ip \
  --policy $POLICY_DEF \
  --scope /subscriptions/$SUBS/resourceGroups/rg-demo
```
**Expected output:**
- Lock JSON with `level: CanNotDelete`.
- Policy assignment JSON with `name: deny-public-ip`.

---

## Conditional Access & MFA

**Theory**
- **Conditional Access (CA)** enforces sign-in controls (MFA, device compliance, location). Critical for human users.

### Portal Steps
1. Entra admin center → **Security** → **Conditional Access** → **New policy**.
2. Target a group (e.g., `All_Employees`), apps (e.g., **All cloud apps**), **Grant** → **Require MFA**.

### CLI / API Note
- Creating CA policies is typically done via **Microsoft Graph** (PowerShell/Graph API). Azure CLI uses `az rest` to call Graph if needed.

```bash
# Example (advanced): create a CA policy via Microsoft Graph with az rest (requires Graph permissions)
# az rest --method POST --url https://graph.microsoft.com/beta/identity/conditionalAccess/policies --body @policy.json
```
**Expected output:**
- In portal, policy status shows **On**/**Report-only** with evaluation results.

---

## Privileged Identity Management (PIM)

**Theory**
- PIM provides **just-in-time (JIT)** elevation for **directory roles** and **Azure resource roles**, with approval, MFA, and time-bound access.

### Portal Steps
1. Entra admin center → **Privileged Identity Management**.
2. Choose **Azure AD roles** or **Azure resources** → Assign eligible roles, define activation settings.

**CLI Note**
- PIM automation is primarily via **Microsoft Graph**; Azure CLI has limited direct PIM commands.

**Expected output:**
- Eligible assignments show; activations are time-limited and audited.

---

## Access Reviews & Entitlement Mgmt

**Theory**
- **Access reviews**: periodic checks of group/app/resource access.
- **Entitlement management**: access packages bundling groups/apps/resources with approvals and lifecycle.

### Portal Steps
- Entra admin center → **Identity Governance** → **Access reviews** / **Entitlement management** → create review or package.

**Expected output:**
- Review results with decisions (Approve/Deny) and automated removals.

---

## Auditing: Activity Logs, Sign-in Logs

**Theory**
- **Activity Log** (Azure) for resource changes.
- **Sign-in logs** (Entra) for auth events, MFA, CA results.

### Portal Steps
- Azure portal → **Monitor** → **Activity Log** (filter by subscription/RG).
- Entra admin center → **Sign-in logs**.

### CLI Steps
```bash
# Activity log (last 1 hour)
az monitor activity-log list --status Succeeded --max-events 50 \
  --query "[].{time:eventTimestamp, op:operationName.value, sub:subscriptionId}" -o table
```
**Expected output:**
- Table of operations with timestamps and operation names.

---

## Best Practices Checklist

- [ ] Enforce **MFA** via Conditional Access for all users.
- [ ] Use **groups** for role assignments; avoid direct per-user grants.
- [ ] Prefer **managed identities** over client secrets.
- [ ] Grant **least privilege** at the **lowest necessary scope**.
- [ ] Separate **directory roles** (Entra) from **resource roles** (RBAC); limit Global Admin.
- [ ] Use **PIM** for admin elevation; require MFA & approvals.
- [ ] Apply **Azure Policy** for guardrails; protect critical resources with **locks**.
- [ ] Monitor **Activity** & **Sign-in logs**; export to **Log Analytics**/**Sentinel**.
- [ ] Rotate app **client secrets**; prefer **certificates** or **federated credentials**.
- [ ] Regular **access reviews** for groups/apps/roles.

---

## Glossary
- **Entra ID (Azure AD):** Microsoft’s cloud identity service for users, groups, apps, and policies.
- **Service Principal:** Enterprise application instance used by apps/services to authenticate.
- **Managed Identity:** Azure-provided identity for resources; no secrets.
- **Role Definition:** Set of allowed actions (and data actions) in Azure RBAC.
- **Role Assignment:** Binding of a role to a principal at a scope.
- **Scope:** Subscription, Resource Group, or Resource at which access is granted.
- **Azure Policy:** Governance engine to enforce/deny configuration.
- **PIM:** Just-in-time elevation for sensitive roles.
- **Conditional Access:** CA engine to enforce MFA, device, risk, etc.
- **Deny Assignment:** Blocks operations regardless of allow role assignments (applied by certain platform features).
