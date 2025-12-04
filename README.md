# Azure Cloud Training

© TINITIATE.COM

---

### Phase A — Cloud / Foundation (Beginner → Associate)

1. **Foundations & Account Setup**

   * Azure regions, region pairs, sovereign clouds.
   * Availability zones, SLAs, shared responsibility model, core resource providers.
   * Subscription setup, resource groups, naming & tagging standards.
   * Azure Portal, CLI (`az`), PowerShell basics; ARM resource model.
   * **Lab:** Create a resource group, configure Cloud Shell, set subscription default.

2. **Identity & Access (Microsoft Entra ID + RBAC)**

   * Tenants, users, groups, service principals.
   * RBAC scopes (Management Group → Subscription → Resource Group → Resource).
   * Managed identities + Key Vault integration.
   * **Lab:** Create a managed identity + Key Vault secret access.

3. **Organizations & Governance (Management Groups, Azure Policy)**

   * Management Groups hierarchy and multi-subscription strategy.
   * Azure Policy: deny, audit, deployIfNotExists; initiatives; compliance reporting.
   * Governance standards: allowed regions, tag enforcement, resource consistency.
   * **Lab:** Apply policy initiative for tagging + compliance.

4. **Networking (VNet)**

   * CIDR planning, VNets + subnets, NSGs, UDR, firewalls.
   * Load balancing: Load Balancer, Application Gateway, Front Door, Traffic Manager.
   * Hybrid: VPN Gateway, ExpressRoute, Private Endpoints + DNS Zones.
   * **Lab:** Create a 3-tier VNet topology + private endpoints.

5. **Compute (VMs, Scale Sets, App Service, Functions)**

   * VM families, Azure compute SKUs, fault/availability domains.
   * VM Scale Sets auto-scaling.
   * App Service plans and deployment slots.
   * Azure Functions (consumption vs premium).
   * **Lab:** Deploy VM scale set + autoscaling rule.

6. **Storage**

   * Storage account tiers: LRS/ZRS/GRS.
   * Blob vs ADLS Gen2 vs Azure Files; lifecycle management.
   * Snapshots, immutability, soft-delete.
   * **Lab:** Create ADLS Gen2 + lifecycle rule.

7. **Databases**

   * Relational: Azure SQL, PostgreSQL Flexible Server, MySQL.
   * NoSQL: Cosmos DB (Core/Mongo/Cassandra/Table).
   * Caching: Azure Cache for Redis.
   * **Lab:** Deploy Azure SQL + Cosmos DB with private networking.

8. **App Integration (Messaging/Eventing)**

   * Event Grid, Service Bus (queues/topics), Event Hubs.
   * DLQs, retries, idempotent messaging.
   * **Lab:** Build Function → Service Bus workflow.

9. **Serverless & Containers**

   * Functions deep dive.
   * Azure Container Registry, Container Apps, AKS.
   * Dapr for distributed microservices.
   * **Lab:** Deploy a container API using ACR + Container Apps.

10. **Observability & Operations**

    * Azure Monitor, Log Analytics, diagnostic settings.
    * App Insights + distributed tracing (OTel).
    * Dashboards, alerts, automated remediation.
    * **Lab:** Central log ingestion + alert.

11. **Security & Compliance**

    * Defender for Cloud, vulnerability scanning.
    * Conditional Access, MFA, PIM.
    * Key Vault (secrets, keys, certs) + purge protection.
    * **Lab:** Enable Defender + JIT access.

12. **Cost Management & FinOps**

    * Cost Management + Billing scopes, anomaly detection, budgets.
    * Reservations vs Savings Plans.
    * Chargeback/showback using tags.
    * **Lab:** Create cost budget + export report.

---

### Phase B — Professional / Production Patterns

13. **Landing Zone & Enterprise Patterns**

    * Azure Landing Zone architecture.
    * IaC deployment with Bicep/Terraform.
    * Deployment stacks + template specs.
    * **Lab:** Deploy Hub-and-Spoke Landing Zone.

14. **DevOps & CI/CD**

    * GitHub Actions vs Azure DevOps.
    * IaC pipelines with what-if and approvals.
    * Secure workloads with federated identity.
    * **Lab:** CI/CD for Bicep modules.

15. **Data, Analytics & AI**

    * Data Factory, Synapse, Microsoft Fabric.
    * Azure OpenAI + Cognitive Search.
    * **Lab:** Build RAG app using Azure OpenAI.

16. **Hybrid & Multi-Cloud (Azure Arc)**

    * Arc-enabled servers and Kubernetes.
    * Policy-as-code for hybrid.
    * **Lab:** Onboard cluster to Arc + enforce policy.

---

| © TINITIATE.COM |
