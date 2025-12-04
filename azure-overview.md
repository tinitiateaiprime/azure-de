# Azure - Overview

## Table of Contents
1. [What Azure Is](#1-what-azure-is)  
2. [Global Anatomy & Resource Hierarchy](#2-global-anatomy--resource-hierarchy)  
3. [Identity (Microsoft Entra ID) & Access](#3-identity-microsoft-entra-id--access)  
4. [Networking Basics (VNet)](#4-networking-basics-vnet)  
5. [Compute Options](#5-compute-options)  
6. [Storage Building Blocks](#6-storage-building-blocks)  
7. [Databases & Analytics (Essentials)](#7-databases--analytics-essentials)  
8. [Messaging & Eventing](#8-messaging--eventing)  
9. [Observability & Operations](#9-observability--operations)  
10. [Security & Shared Responsibility](#10-security--shared-responsibility)  
11. [Cost Management](#11-cost-management)  
12. [Naming, Tags & Environments](#12-naming-tags--environments)

---

## 1) What Azure Is
Microsoft Azure is a broad set of **public cloud** services. You’ll compose **Resource Groups** of services (compute, storage, DB, networking) governed by **Policies** and **RBAC**.

---

## 2) Global Anatomy & Resource Hierarchy
- **Entra ID Tenant** (formerly Azure AD) → identity boundary.
- **Management Groups** → govern multiple subscriptions.
- **Subscription** → **billing/isolation** unit.
- **Resource Group (RG)** → lifecycle boundary for resources.
- **Region** with **Availability Zones** for HA.

**Landing Zone**: opinionated blueprint of networking, identity, policy, logging, and security to start right.

---

## 3) Identity (Microsoft Entra ID) & Access
- **Users/Groups/Service principals/Managed identities**.
- **RBAC** at MG/Subscription/RG/Resource scopes; grant least privilege.
- **PIM** for just-in-time elevation; **MFA** mandatory.

---

## 4) Networking Basics (VNet)
- **VNet** → private IP space; **subnets** (often segregate app tiers).
- **NSGs** (Network Security Groups) → security rules at subnet/NIC.
- **UDR** (route tables), **Public IP**, **NAT Gateway**, **Bastion**.
- **Private Link** to consume PaaS privately; **VPN/ExpressRoute** for hybrid.
- **Azure Firewall / Application Gateway (WAF)** for perimeter control.

---

## 5) Compute Options
- **Virtual Machines** & **VM Scale Sets** (autoscale).
- **App Service** (PaaS web/API), **Functions** (serverless), **Container Apps** (serverless containers), **AKS** (Kubernetes).
- Placement across **Zones**; use **Availability Sets** for non-zoned SKUs.

---

## 6) Storage Building Blocks
- **Storage Account** → **Blob** (object), **File** (SMB), **Queue**, **Table**; tiers: Hot/Cool/Archive.
- **Managed Disks** for VMs.
- Encryption at rest by default; customer-managed keys via **Key Vault**.

---

## 7) Databases & Analytics (Essentials)
- **Azure SQL** (PaaS), **PostgreSQL/MySQL Flexible Server**.
- **Cosmos DB** (NoSQL, global distribution, multiple APIs).
- **Synapse Analytics** (DW + pipelines), **Data Factory** (ETL).

---

## 8) Messaging & Eventing
- **Service Bus** (queues/topics), **Event Grid** (events), **Event Hubs** (streams).
- Patterns: DLQ, retry, idempotency.

---

## 9) Observability & Operations
- **Azure Monitor** + **Log Analytics Workspace** (central logs/metrics).
- **Application Insights** (APM/traces).
- **Activity Log** (audit), **Change Analysis**.

---

## 10) Security & Shared Responsibility
- **Key Vault** (secrets/keys/certs), **Defender for Cloud** (posture, threat), **Policy** (guardrails), **Blueprints/Landing Zone Accelerator** (governance patterns).
- You: secure identities, data, configs; Azure: substrate & managed components.

---

## 11) Cost Management
- **Azure Cost Management + Billing**, budgets/alerts.
- **Reservations** (1/3-yr), **Savings Plan for Compute**.
- Right-size SKUs, zonal vs regional redundancy trade-offs.

---

## 12) Naming, Tags & Environments
- Suggested name: `env-app-component-region-###` (Azure examples often use `rg-`, `vnet-`, `vm-` prefixes).
- **Tags**: `env`, `owner`, `costCenter`, `app`, `dataClass`, `lifecycle`.
- Separate `dev`/`test`/`stg`/`prod` at least by **Resource Groups** (prefer **Subscriptions**).
