# Cloud Computing Fundamentals

A beginner-friendly introduction to core cloud concepts.

---

## Table of Contents
1. [What Is “the Cloud”](#1-what-is-the-cloud)
2. [Why Cloud: Benefits & Trade-offs](#2-why-cloud-benefits--trade-offs)
3. [Service Models: IaaS vs PaaS vs SaaS (+ Serverless)](#3-service-models-iaas-vs-paas-vs-saas--serverless)
4. [Deployment Models: Public, Private, Hybrid, Multi-cloud, Edge](#4-deployment-models-public-private-hybrid-multi-cloud-edge)
5. [Shared Responsibility Model](#5-shared-responsibility-model)
6. [Global Anatomy & Resource Hierarchy](#6-global-anatomy--resource-hierarchy)
7. [Naming, Tagging, and Environment Strategy](#7-naming-tagging-and-environment-strategy)
8. [Networking Essentials](#8-networking-essentials-cloud-agnostic)
9. [Identity & Access Management (IAM)](#9-identity--access-management-iam)
10. [Compute Options](#10-compute-options)
11. [Storage Building Blocks](#11-storage-building-blocks)
12. [Databases & Data Services](#12-databases--data-services)
13. [Messaging, Events, and Streams](#13-messaging-events-and-streams)
14. [Observability & Operations](#14-observability--operations)
15. [Security Foundations](#15-security-foundations)
16. [Reliability, Availability & Disaster Recovery](#16-reliability-availability--disaster-recovery)
17. [Cost Management & FinOps Basics](#17-cost-management--finops-basics)
18. [Governance, Compliance & Landing Zones](#18-governance-compliance--landing-zones)
19. [Performance & Sustainability](#19-performance--sustainability)
20. [Cross-Cloud Concept Mapping (Minimal)](#20-cross-cloud-concept-mapping-minimal)
21. [Choosing a Provider: Evaluation Criteria](#21-choosing-a-provider-evaluation-criteria)

---

## 1) What Is “the Cloud”
**Definition:** Working definition: Cloud computing is the on-demand delivery of computing resources (compute, storage, networking, and managed services) over the internet with pay-as-you-go pricing.

**Essential characteristics**
- **On-demand self-service:** Provision resources without human interaction from the provider.
- **Broad network access:** Accessible over standard networks and devices.
- **Resource pooling:** Multi-tenant, pooled capacity; location independence.
- **Rapid elasticity:** Scale up/down quickly; illusion of infinite capacity.
- **Measured service:** Usage is metered; you pay for what you consume.

**Why the cloud changed everything:** It turned hardware and software platforms into **utilities**—like electricity—shifting from capital expenditure (buy servers) to operational expenditure (rent capacity).

## 2) Why Cloud: Benefits & Trade-offs
**Benefits**
- **Speed:** Minutes to provision, not months.
- **Elasticity:** Match capacity to demand; avoid over/under-provisioning.
- **Managed services:** Offload undifferentiated heavy lifting (patching, HA).
- **Global reach:** Regions worldwide; lower latency to users.
- **Cost model:** OpEx, granular metering, reserved/committed discounts.

**Trade-offs**
- **Complex pricing:** Egress, storage tiers, cross-region traffic, etc.
- **Lock-in risk:** Proprietary services/APIs.
- **Operational blind spots:** Shared responsibility—not everything is “managed.”
- **Governance drift:** Easy to create sprawl without guardrails.

## 3) Service Models: IaaS vs PaaS vs SaaS (+ Serverless)
- **IaaS (Infrastructure as a Service):** You manage OS and above. Flexible, closest to raw VMs and networks.
- **PaaS (Platform as a Service):** Provider manages runtime/platform. You focus on apps & data.
- **SaaS (Software as a Service):** Provider delivers full apps. You configure and use.
- **Serverless/FaaS (Functions as a Service):** Event-driven, autoscaled, pay for execution; no server management.

**Fit-for-purpose intuition**
- Need full control or special OS? → IaaS.
- Standard web/API with minimal ops? → PaaS.
- Commodity business function (email, CRM)? → SaaS.
- Event-driven glue/microtasks? → Serverless.

## 4) Deployment Models: Public, Private, Hybrid, Multi-cloud, Edge
- **Public:** Shared provider infrastructure, logical isolation.
- **Private:** Dedicated environment, often on-prem.
- **Hybrid:** Integrated on-prem + public cloud.
- **Multi-cloud:** Two or more public clouds.
- **Edge:** Compute near users/devices to reduce latency.

## 5) Shared Responsibility Model
**Idea:** Security and operations are shared between you and the provider. The **line** moves depending on service model:
- **IaaS:** You secure the OS, apps, data; provider secures physical infra and hypervisor.
- **PaaS:** You secure applications, identities, data; provider secures OS/platform.
- **SaaS:** You manage users, configs, and data; provider runs the app.

## 6) Global Anatomy & Resource Hierarchy
**Common layers**
- **Organization/Tenant:** Top container for governance.
- **Account/Subscription/Project:** Billing and isolation boundary.
- **Region:** Geographic area with independent infrastructure.
- **Availability Zone (AZ)/Zone:** Separate data centers within a region for HA.
- **Edge locations/PoPs:** Caching and network ingress/egress points.

**SLA & locality**
- Multi-AZ for high availability; multi-region for disaster recovery and data residency needs.

## 7) Naming, Tagging, and Environment Strategy
**Naming**
- Encode environment, app, component, region, and sequence.  
  Example pattern: `env-app-component-region-###` (e.g., `prod-payments-api-ap-south-1-001`)

**Tagging/Labels**
- **Required keys:** `env`, `owner`, `cost-center`, `app`, `data-classification`, `lifecycle`.
- Enforce via policies to keep cost & compliance under control.

**Environment tiers**
- `sandbox` → `dev` → `test`/`qa` → `staging` → `prod`  
    Keep **strict separation** (network, identity, and billing).

## 8) Networking Essentials (cloud-agnostic)
**Virtual network (VPC/VNet)**
- Your private IP space in the cloud, carved into **subnets** (often public vs private).
- **CIDR planning** matters—leave room for growth.

**Routing & gateways**
- Route tables govern traffic flows.
- **Internet Gateway** for outbound/inbound internet; **NAT** for private outbound only.
- **Private endpoints/links** to consume managed services privately.

**Network security**
- **Security groups/ACLs/Firewalls:** Allow only what’s needed (least privilege for ports).
- **Zero-trust mindset:** Identity and context over implicit trust.

**Name resolution & delivery**
- **DNS** for service discovery.
- **Load balancing** spreads traffic across instances.
- **CDN** pushes static content to edge locations.

## 9) Identity & Access Management (IAM)
**Core terms**
- **Principal:** Human or workload identity.
- **Authentication vs authorization:** Prove who you are vs what you can do.
- **RBAC/ABAC:** Role- or attribute-based access control.
- **Policies/Permissions:** Machine-readable rules granting actions on resources.

**Good practice**
- **MFA for humans**, **short-lived tokens** for machines.
- **Least privilege** roles; **break-glass** highly privileged account kept offline.
- Centralized SSO; periodic access reviews; secrets rotation.

## 10) Compute Options
- **Virtual Machines (VMs):** Full OS control; consider images, patching, and autoscaling.
- **Containers & Orchestrators:** Package apps with dependencies; schedule across nodes.
- **Serverless Functions:** Event-driven, scale to zero; mind cold starts and limits.
- **Batch & HPC:** Optimized for parallel workloads (rendering, simulations).

**Cost & resilience concepts**
- **Instance families:** General purpose, compute-optimized, memory-optimized.
- **Preemptible/spot capacity:** Very cheap but revocable—design for interruptions.
- **Autoscaling:** Policy-based scale out/in; ensure statelessness.

## 11) Storage Building Blocks
- **Object storage:** Flat namespace, high durability, ideal for blobs, logs, backups; tiered by access frequency.
- **Block storage:** Disk attached to a compute instance; low-latency random I/O.
- **File storage:** Shared POSIX/SMB file systems for lift-and-shift or legacy apps.

**Design considerations**
- **Durability vs availability:** “Eleven 9s” durability ≠ app always reachable.
- **Lifecycle policies:** Move cold data to cheaper tiers; set retention.
- **Consistency models:** Eventual vs strong; know read-after-write guarantees.
- **Encryption:** At rest and in transit—prefer provider-managed keys unless you need customer-managed keys.
    
## 12) Databases & Data Services
- **Relational (SQL):** Strong consistency, transactions; vertical + read-replica scaling.
- **NoSQL (Key-Value, Document, Wide-Column, Graph):** Choose based on access pattern and scale needs.
- **Data warehouse vs data lake:** Structured analytics vs raw, large-scale storage.
- **Backup & recovery:** Point-in-time, automated backups, retention windows.
- **HA & DR:** Multi-AZ for HA; cross-region replicas for DR.
    
## 13) Messaging, Events, and Streams
- **Queues:** Point-to-point, work distribution, back-pressure.
- **Topics/Pub-Sub:** Fan-out to many subscribers.
- **Streams:** Ordered, replayable event logs for analytics/ETL.
- **Reliability patterns:** Idempotency keys, retries with backoff, dead-letter queues.

## 14) Observability & Operations
- **Logs:** Structured, centralized, searchable (include correlation IDs)
- **Metrics:** Time series (latency, error rate, saturation)
- **Traces:** End-to-end request flows
- **SLO/SLI/Error Budgets:** Reliability targets and trade-offs
- **Runbooks/Playbooks:** Standard responses to alerts

## 15) Security Foundations
- **Defense-in-depth:** IAM, network, app, and data layers all matter.
- **Data protection:** Classification, least access, encryption, key management (KMS/HSM), tokenization.
- **Secrets management:** Central store, rotation, short-lived credentials.
- **Perimeter & app security:** WAF, DDoS controls, input validation, patching cadence.
- **Continuous assurance:** Vulnerability scans, posture management, audit logs.
  
## 16) Reliability, Availability & Disaster Recovery
- **Availability vs reliability:** Uptime vs correctness over time.
- **HA inside a region:** Spread across zones; avoid single points of failure.
- **DR across regions:** Define **RTO** (time to recover) and **RPO** (data loss window).
- **Backups & tests:** Back up, replicate, and **rehearse restores**.
- **Failure modes:** Dependency mapping; chaos testing to surface brittle spots.
    
## 17) Cost Management & FinOps Basics
- **Cost drivers:** Compute hours, storage capacity, data egress, managed service premiums. 
- **Controls:** Budgets, alerts, quotas; tagging for chargeback/showback.
- **Optimization:** Right-size, autoscale, reserved/committed usage, turn off idle.
- **Architecture choices:** Caching, data locality, fewer cross-region hops.
- **Culture:** Engineers see price as a **feature**—measure cost per transaction/user.

## 18) Governance, Compliance & Landing Zones
- **Guardrails:** Policy-as-code to enforce regions, sizes, encryption, tagging.
- **Resource hierarchy:** Org → accounts/subscriptions/projects → folders/resource groups.
- **Access boundaries:** Separate prod from non-prod (network + identity).
- **Compliance posture:** Map controls to frameworks (ISO, SOC, HIPAA, PCI, GDPR, etc.).
- **Landing zone:** Pre-baked, secure multi-account/subscription foundation with networking, IAM, logging, and cost controls from day one.

## 19) Performance & Sustainability
- **Performance:** Choose the right compute/storage class; cache aggressively; minimize tail latency; prefer async patterns.
- **Sustainability:** Right-size, autoscale, pick efficient instance families, use serverless where sensible, minimize data movement; consolidate idle resources.

## 20) Cross-Cloud Concept Mapping (Minimal)
| Generic Concept | Typical Name You’ll See |
| --- | --- |
| Top-level org/tenant | Organization / Tenant |
| Billing/Isolation unit | Account / Subscription / Project |
| Global geography | Region |
| Fault domain | Availability Zone / Zone |
| Private network | VPC / VNet |
| Network isolation rules | Security Group / Network Security Group / Firewall |
| Public entry | Internet Gateway / Public IP / External LB |
| Private egress | NAT Gateway / NAT |
| Managed object store | Object/Bucket/Blob Storage |
| Managed relational DB | Managed SQL / Relational DB Service |
| Queue / Pub-Sub | Message Queue / Topic / Pub-Sub |
| CDN | Content Delivery Network |
| KMS | Key Management Service |
| Audit | Activity / Cloud / Audit Logs |

## 21) Choosing a Provider: Evaluation Criteria
- Regional presence & data residency
- Core service depth (compute, storage, network, DB, analytics)
- Managed services you plan to use (queues, functions, ML, CDN)
- Identity integration (SSO/federation), compliance attestations
- Pricing model & tooling; sustained-use/commitment discounts
- Ecosystem maturity (marketplace, partner support)
- Operational tooling (monitoring, logging, policy)
- Contract/SLA/support plans
