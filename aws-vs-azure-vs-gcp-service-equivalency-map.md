# AWS vs Azure vs GCP – Service Equivalency Map (2025)

A practical, side‑by‑side map of the “same thing, different name” across the big three clouds.

**How to read.** Each table row is a _capability_. Some capabilities have more than one plausible match; those are listed with a slash (/) and brief notes.

**Caveats.** Names and SKUs evolve. Some services are retired, renamed, or split into multiple offers. Treat this as a starting point; always confirm limits/SLAs/GA vs preview. (SLA = Service Level Agreement; GA = General Availability.)

---

## 1) Accounts, Projects, Organization Structure

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Org root & hierarchy | **AWS Organizations** → OUs → Accounts | **Management Groups** → **Subscriptions** | **Organization** → **Folders** → **Projects** | Isolation unit = Account / Subscription / Project. (OU = Organizational Unit.) |
| Landing zone scaffolding | **Control Tower** | **Azure Landing Zone** (CAF) | **Google Cloud Landing Zone** (blueprints) | Opinionated guardrails. (CAF = Cloud Adoption Framework.) |
| Policy engine (org) | **Service Control Policies (SCPs)** | **Azure Policy** | **Organization Policy Service** | Constraints at org/folder/project. (SCP = Service Control Policy.) |

---

## 2) Identity & Access

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| IAM (resource‑level) | **AWS IAM** | **Azure RBAC** | **Cloud IAM** | Role‑based permissions. (IAM = Identity and Access Management; RBAC = Role‑Based Access Control.) |
| Workforce SSO | **IAM Identity Center** (ex‑AWS SSO) | **Microsoft Entra ID** (Azure AD) | **Cloud Identity** / **Workforce Identity Federation** | SAML/OIDC to consoles & apps. (SSO = Single Sign‑On; SAML = Security Assertion Markup Language; OIDC = OpenID Connect; AD = Active Directory.) |
| Managed AD | **AWS Managed Microsoft AD** | **Azure AD Domain Services** | **Managed Microsoft AD** | Domain join/LDAP/Group Policy. (AD = Active Directory; LDAP = Lightweight Directory Access Protocol.) |
| Customer identity (CIAM) | **Cognito** | **Entra External ID** | **Identity Platform** | B2C sign‑in, social auth. (CIAM = Customer Identity and Access Management; B2C = Business‑to‑Consumer.) |
| Keys / HSM | **KMS** / **CloudHSM** | **Key Vault (Keys)** / **Dedicated HSM** | **Cloud KMS** / **Cloud HSM** | KMS = envelope encryption APIs. (KMS = Key Management Service; HSM = Hardware Security Module.) |
| Secrets | **Secrets Manager** / **SSM Parameter Store** | **Key Vault (Secrets)** | **Secret Manager** | Runtime secret storage/rotation. (SSM = Systems Manager.) |

---

## 3) Networking, Connectivity & Edge

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Private network | **VPC** | **Virtual Network (VNet)** | **VPC** | Subnets, routes, ACLs. (VPC = Virtual Private Cloud; VNet = Virtual Network; ACL = Access Control List.) |
| Global routing hub | **Transit Gateway** | **Virtual WAN** | **Network Connectivity Center** | Centralized hub‑and‑spoke. (WAN = Wide Area Network.) |
| Direct private link to DC | **Direct Connect** | **ExpressRoute** | **Cloud Interconnect** | Dedicated circuits. (DC = Data Center.) |
| Site‑to‑site VPN | **AWS VPN** | **VPN Gateway** | **Cloud VPN** | IPSec tunnels. (VPN = Virtual Private Network; IPSec = Internet Protocol Security.) |
| NAT service | **NAT Gateway** | **NAT Gateway** | **Cloud NAT** | Egress without public IPs. (NAT = Network Address Translation.) |
| Private service access | **PrivateLink** | **Private Link** | **Private Service Connect** | Private PaaS endpoints. (PaaS = Platform as a Service.) |
| Load balancing (L4/L7) | **ELB** (ALB/NLB/CLB) | **Azure Load Balancer**, **App Gateway** | **Cloud Load Balancing** | GCP has global anycast L7 LB. (L4/L7 = Layer 4/Layer 7; ELB = Elastic Load Balancing; ALB = Application Load Balancer; NLB = Network Load Balancer; CLB = Classic Load Balancer.) |
| DNS (authoritative) | **Route 53** | **Azure DNS** | **Cloud DNS** | Hosted zones/records. (DNS = Domain Name System.) |
| CDN / Front Door | **CloudFront** | **Front Door** / **Azure CDN** | **Cloud CDN** | Front Door also does WAF/acceleration. (CDN = Content Delivery Network; WAF = Web Application Firewall.) |
| DDoS/WAF | **AWS Shield / WAF** | **Azure DDoS Protection / WAF** | **Cloud Armor** | Cloud Armor = WAF + DDoS features. (DDoS = Distributed Denial of Service; WAF = Web Application Firewall.) |
| Web acceleration | **Global Accelerator** | **Front Door** | **Cloud Load Balancing (global)** | Anycast‑based acceleration. (Anycast = one IP announced from many edges.) |
| Hybrid/edge infra | **Outposts / Local Zones / Wavelength** | **Azure Stack HCI/Hub/Edge**, **Arc** | **Google Distributed Cloud (GDC)** / **Anthos** | Managed control plane to edge. (HCI = Hyper‑Converged Infrastructure; GDC = Google Distributed Cloud.) |

---

## 4) Compute, Containers & Serverless

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| VMs | **EC2** | **Virtual Machines** | **Compute Engine** | EC2 = Elastic Compute Cloud. (VM = Virtual Machine.) |
| Autoscaling groups | **ASG** | **VM Scale Sets** | **Managed Instance Groups** | Instance fleet scaling. (ASG = Auto Scaling Group; MIG = Managed Instance Group.) |
| Spot/preemptible | **EC2 Spot** | **Spot VMs** | **Spot VMs** | Transient capacity (preemptible/evictable instances). |
| Dedicated hosts | **Dedicated Hosts** | **Dedicated Host** | **Sole‑tenant Nodes** | HW isolation. (HW = Hardware.) |
| Batch/HPC | **AWS Batch** | **Azure Batch** | **Cloud Batch** | Managed batch schedulers. (HPC = High‑Performance Computing.) |
| PaaS web/apps | **Elastic Beanstalk** / **App Runner** | **App Service** | **App Engine** | PaaS for web APIs. (PaaS = Platform as a Service.) |
| Serverless functions | **Lambda** | **Azure Functions** | **Cloud Functions** | Event‑driven FaaS. (FaaS = Function as a Service.) |
| Serverless containers | **Fargate (ECS/EKS)** / **App Runner** | **Container Apps** | **Cloud Run** | Scale‑to‑zero HTTP/containers. (ECS = Elastic Container Service; EKS = Elastic Kubernetes Service.) |
| Containers (orchestration) | **ECS** / **EKS** | **AKS** | **GKE** | Managed Kubernetes. (AKS = Azure Kubernetes Service; GKE = Google Kubernetes Engine; K8s = Kubernetes.) |
| Container registry | **ECR** | **ACR** | **Artifact Registry** | Private OCI registries. (ECR = Elastic Container Registry; ACR = Azure Container Registry; OCI = Open Container Initiative.) |
| Dev workstations | **Cloud9** | **Dev Box** | **Cloud Workstations** | Cloud IDE/dev desktops. (IDE = Integrated Development Environment.) |

---

## 5) Storage & Data Movement

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Object storage | **S3** | **Blob Storage** | **Cloud Storage** | Multi‑tier (hot/cold/archive). (S3 = Simple Storage Service.) |
| Block storage | **EBS** | **Managed Disks** | **Persistent Disk / Hyperdisk** | VM‑attached volumes. (EBS = Elastic Block Store.) |
| File/NAS | **EFS** | **Azure Files** | **Filestore** | NFS/SMB. (EFS = Elastic File System; NFS = Network File System; SMB = Server Message Block.) |
| High‑perf shared FS | **FSx for Lustre/ONTAP/OpenZFS/Windows** | **Azure NetApp Files** | **NetApp Volumes** / **Filestore Enterprise** | HPC, SAP, Windows file shares. (FSx = Amazon FSx (brand); ONTAP = NetApp ONTAP; HPC = High‑Performance Computing; SAP = Systems, Applications & Products.) |
| Archive | **S3 Glacier (Instant/Flexible/Deep)** | **Blob Archive** | **Archive / Coldline** | Long‑term, low‑cost tiers. |
| Online transfer | **DataSync**, **Transfer Family (SFTP/FTPS/FTP)** | **Data Factory copy**, **Storage Explorer**, **SFTP for Azure Blob** | **Storage Transfer Service**, **SFTP** | Managed movers. (SFTP = SSH File Transfer Protocol; FTPS = FTP Secure; FTP = File Transfer Protocol.) |
| Offline transfer | **Snowcone/Snowball/Snowmobile** | **Data Box / Heavy / Gateway** | **Transfer Appliance** | Rugged shuttles. |
| Hybrid file cache | **Storage Gateway (File/Volume/Tape)** | **Azure File Sync** | **NetApp Volumes / partners** | Edge caching/backups. |

---

## 6) Databases (Relational, NoSQL, Cache, Graph)

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Relational (managed) | **Amazon RDS** (MySQL, PostgreSQL, SQL Server, MariaDB, Oracle) | **Azure SQL Database**, **Azure Database for MySQL/PostgreSQL** | **Cloud SQL** (MySQL, PostgreSQL, SQL Server) | Engine families. (RDS = Relational Database Service; SQL = Structured Query Language; RDBMS engines.) |
| Distributed relational | **Amazon Aurora** (MySQL/PostgreSQL‑compatible) | **Azure SQL Hyperscale** / **Cosmos DB for PostgreSQL (Citus)** | **Cloud Spanner** / **AlloyDB for PostgreSQL** | Global scale & HA characteristics vary. (HA = High Availability.) |
| Data warehouse | **Redshift** | **Synapse Analytics (SQL DW)** / **Microsoft Fabric (Warehouse)** | **BigQuery** | Serverless vs provisioned models. (SQL DW = SQL Data Warehouse (legacy name for Synapse dedicated SQL pool).) |
| Document DB | **Amazon DocumentDB (Mongo compatible)** | **Cosmos DB (Mongo API)** | **Firestore** / **MongoDB Atlas** | Different consistency/semantics. |
| Wide‑column | **DynamoDB** | **Cosmos DB (Cassandra API)** | **Bigtable** | Key‑value/column family. |
| In‑memory cache | **ElastiCache (Redis/Memcached)** | **Azure Cache for Redis** | **Memorystore (Redis/Memcached)** | In‑memory caching. |
| Graph | **Neptune** | **Cosmos DB (Gremlin API)** | *(no native)* — use **Neo4j Aura** or self‑managed | Graph databases. |
| Time series | **Timestream** | **Azure Data Explorer (ADX/Kusto)** | **Bigtable/BigQuery + Dataflow** | No direct 1:1 on GCP; solution pattern. (ADX = Azure Data Explorer.) |
| Search | **OpenSearch Service** | **Azure AI Search** | **Elastic Cloud** / **Vertex AI Search** | Elastic also available in marketplaces. |
| Vector search | **OpenSearch (vector) / Aurora pgvector / Neptune Analytics** | **Azure AI Search (vector)** / **Cosmos DB (vector)** | **Vertex AI Matching Engine / BigQuery Vector Search / AlloyDB AI** | Rapidly evolving; index types include HNSW/IVF. (HNSW = Hierarchical Navigable Small World; IVF = Inverted File index.) |

---

## 7) Analytics, Streaming, ETL & Governance

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Stream ingestion (pub/sub) | **Kinesis Data Streams** | **Event Hubs** | **Pub/Sub** | High‑throughput append. (Pub/Sub = Publish/Subscribe.) |
| Stream delivery | **Kinesis Firehose** | **Event Hubs Capture / ADF** | **Pub/Sub → GCS** / **Dataflow** | Durable fan‑out to storage. (ADF = Azure Data Factory; GCS = Google Cloud Storage.) |
| Stream processing | **Kinesis Data Analytics (Flink)** / **Managed Kafka (MSK)** | **Stream Analytics** / **HDInsight Kafka** | **Dataflow (Apache Beam)** / **Dataproc Kafka** | Stateful windowing (SQL or Beam). (MSK = Managed Streaming for Apache Kafka.) |
| Message queues (at‑least‑once) | **SQS** | **Service Bus / Queue Storage** | **Pub/Sub** | At‑least‑once semantics. (SQS = Simple Queue Service.) |
| Event bus (event router) | **EventBridge** | **Event Grid** | **Eventarc** | Cross‑service/event routing. |
| Batch/ETL serverless SQL | **Athena** | **Synapse Serverless SQL** | **BigQuery** | Query directly over data lake/warehouse. (ETL = Extract‑Transform‑Load.) |
| Managed Spark | **EMR** | **Synapse Spark** / **HDInsight** | **Dataproc** | Managed Spark clusters/jobs. (EMR = Elastic MapReduce.) |
| Visual ELT/ETL | **AWS Glue (Jobs/Studio)** | **Data Factory** | **Cloud Data Fusion** | Visual pipelines. (ELT = Extract‑Load‑Transform; CDAP = Cask Data Application Platform.) |
| Workflow orchestration | **Step Functions** / **MWAA (Airflow)** | **Logic Apps** / **ADF pipelines** | **Workflows** / **Cloud Composer (Airflow)** | State machines vs Airflow. (MWAA = Managed Workflows for Apache Airflow.) |
| Data catalog | **Glue Data Catalog** | **Microsoft Purview** | **Data Catalog (Dataplex)** | Discovery + lineage. |
| Lake governance | **Lake Formation** | **Microsoft Purview** | **Dataplex / BigLake** | Permissions/unified tables. |
| BI & dashboards | **QuickSight** | **Power BI** | **Looker / Looker Studio** | Business Intelligence. (BI = Business Intelligence.) |

---

## 8) AI/ML & GenAI

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| ML platform (build/train/deploy) | **SageMaker** | **Azure Machine Learning** | **Vertex AI** | SDKs, pipelines, endpoints. (ML = Machine Learning; SDK = Software Development Kit.) |
| Foundation models (hosted) | **Amazon Bedrock** | **Azure OpenAI Service** | **Vertex AI Model Garden / Gemini** | Managed access to FM APIs. (FM = Foundation Model; API = Application Programming Interface.) |
| Conversational AI | **Lex** | **Azure Bot Service** | **Dialogflow CX/ES** | NLU, bot orchestration. (NLU = Natural Language Understanding.) |
| Vision APIs | **Rekognition** | **Azure AI Vision** | **Vision AI / Vision API** | Image/video analysis. |
| NLP / text analytics | **Comprehend** | **Azure AI Language** | **Cloud Natural Language / Vertex** | Sentiment / entity / PII. (NLP = Natural Language Processing; PII = Personally Identifiable Information.) |
| Speech | **Transcribe / Polly** | **Azure AI Speech** | **Speech‑to‑Text / Text‑to‑Speech** | STT/TTS. (STT = Speech‑to‑Text; TTS = Text‑to‑Speech.) |
| Translation | **Translate** | **Azure AI Translator** | **Cloud Translation** | Machine translation. |
| Recommendations | **Personalize** | **Azure AI Personalizer** | **Vertex AI Recommendations / Retail** | |
| Feature store | **SageMaker Feature Store** | **Azure ML Feature Store** | **Vertex AI Feature Store** | |

---

## 9) DevOps, Code & Delivery

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Git repos | **CodeCommit** | **Azure Repos** | **Cloud Source Repositories (deprecated)** | On GCP prefer GitHub/Bitbucket + Cloud Build triggers. |
| CI/CD build | **CodeBuild** | **Azure Pipelines** | **Cloud Build** | CI/CD pipelines. (CI = Continuous Integration; CD = Continuous Delivery/Deployment.) |
| CD to K8s | **CodeDeploy / ArgoCD (self)** | **Pipelines / Environments** | **Cloud Deploy** | Progressive delivery to clusters. (K8s = Kubernetes; CD = Continuous Delivery/Deployment.) |
| Artifacts/packages | **CodeArtifact** | **Azure Artifacts** | **Artifact Registry** | Maven/NPM/containers. |
| IaC templates | **CloudFormation / CDK** | **ARM / Bicep** | **Config Connector / Config Controller / Infrastructure Manager** | Choose your IaC. (IaC = Infrastructure as Code; CDK = Cloud Development Kit; ARM = Azure Resource Manager.) |
| Config/state mgmt | **Systems Manager (SSM)** | **Azure Automation / Update Mgmt** | **OS Config / Ansible/ops‑agent** | Fleet ops for VMs. (SSM = Systems Manager; OS = Operating System; VM = Virtual Machine.) |

---

## 10) Monitoring, Logging, Reliability

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Metrics/alerts | **CloudWatch** | **Azure Monitor** | **Cloud Monitoring (Ops Suite)** | Multi‑signal dashboards. |
| Logs | **CloudWatch Logs** | **Log Analytics (Workspace)** | **Cloud Logging** | Central log stores. |
| Tracing | **X‑Ray** | **Application Insights (APM)** | **Cloud Trace** | Distributed tracing. (APM = Application Performance Monitoring.) |
| Error reporting | **CloudWatch / X‑Ray** | **App Insights** | **Error Reporting** | Exceptions aggregation. |
| DR & continuity | **Route 53 health checks**, **Pilot Light/Backup** | **Azure Site Recovery** | **Disaster Recovery options / regional patterns** | ASR = VM replication/orchestration. (DR = Disaster Recovery; ASR = Azure Site Recovery; VM = Virtual Machine.) |
| Backup | **Backup** | **Azure Backup** | **Backup and DR Service** | Managed backups. |

---

## 11) Integration, Messaging & Business Apps

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Managed Kafka | **MSK** | **HDInsight Kafka** / **Confluent Cloud** | **Confluent Cloud** / **Dataproc Kafka** | Often use Confluent SaaS. (MSK = Managed Streaming for Apache Kafka; SaaS = Software as a Service.) |
| iPaaS / integration | **AppFlow** / **Step Functions** | **Logic Apps** | **Workflows / Integration Connectors** | Connectors to SaaS apps. (iPaaS = Integration Platform as a Service.) |
| Email | **Amazon SES** | **Azure Communication Services – Email** | *(no native)* → **SendGrid/Twilio** | GCP uses partners/SaaS. (SES = Simple Email Service.) |
| Notifications (mobile/push) | **SNS / Pinpoint** | **Notification Hubs** | **Firebase Cloud Messaging** | Mobile push at scale. (SNS = Simple Notification Service.) |
| Contact center | **Amazon Connect** | **Dynamics 365 CC** / **ACS with partners** | **Contact Center AI Platform (CCAIP)** | CCAIP + telephony partners. (CCAIP = Contact Center AI Platform; ACS = Azure Communication Services.) |
| API management | **API Gateway** | **API Management** | **API Gateway / Apigee** | Apigee has enterprise API lifecycle/monetization. (API = Application Programming Interface.) |

---

## 12) IoT, Edge AI & OT

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Device registry/messaging | **IoT Core** | **IoT Hub** | *(retired)* **Cloud IoT Core** → use **Pub/Sub + partners** | GCP first‑party IoT Core retired; ref architectures remain. (IoT = Internet of Things; Pub/Sub = Publish/Subscribe.) |
| Device mgmt/ops | **IoT Device Management / Greengrass** | **IoT Hub Device Management / Azure IoT Edge** | **Edge + partners (NVIDIA/Siemens)** | Industrial/edge patterns. (OT = Operational Technology.) |

---

## 13) Media, Maps, Satellite & Specialty

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Media transcode/live | **Elemental MediaConvert/Live/Package** | *(Azure Media Services retired)* → **partners** | **Transcoder API / Media CDN / Live Stream** | Azure directs to partner ecosystem. (CDN = Content Delivery Network.) |
| CDN (media focus) | **CloudFront** | **Front Door / Azure CDN** | **Media CDN / Cloud CDN** | Large‑scale streaming. (CDN = Content Delivery Network.) |
| Maps/geo | **Amazon Location Service** | **Azure Maps** | **Google Maps Platform** | Maps is a separate Google business but pairs with GCP. |
| Satellite ground | **AWS Ground Station** | **Azure Orbital Ground Station** | *(no native)* → partners | |
| Quantum computing | **Amazon Braket** | **Azure Quantum** | *(research‑only, not GCP managed)* | |
| Blockchain | **Managed Blockchain** | *(Azure Blockchain retired)* | *(no native)* | Use partners/marketplaces. |

---

## 14) Cost, Billing & Governance

| Capability | AWS | Azure | GCP | Notes |
|---|---|---|---|---|
| Billing accounts | **Payer/Linked Accounts** | **Billing accounts + Subscriptions** | **Billing accounts + Projects** | Invoices/committed use discounts differ. |
| Budgets/alerts | **AWS Budgets** | **Cost Management + Budgets** | **Budgets** | Threshold alerts. |
| Cost analysis | **Cost Explorer / CUR** | **Cost Analysis / Exports** | **Billing reports / BigQuery export** | CUR → Athena/QuickSight; GCP → BigQuery. (CUR = Cost and Usage Report.) |
| FinOps tooling | **Compute Optimizer / Rightsizing** | **Advisor / Savings Plan** | **Recommender / CUD/SUD** | Rightsizing + commitment planning. (FinOps = Cloud Financial Operations; CUD = Committed Use Discounts; SUD = Sustained Use Discounts.) |

---

## 15) Common “Gotchas” When Mapping Services

1. **Not 1:1 parity.** “Equivalent” ≠ identical limits or SLAs. Example: BigQuery (serverless) vs Redshift/Synapse (provisioned) differ operationally. (SLA = Service Level Agreement.)
2. **Retired/legacy services.** Azure Media Services and Azure Blockchain are retired; GCP Cloud IoT Core is retired; GCP Cloud Source Repositories is deprecated—use GitHub/Bitbucket.
3. **Global vs regional behaviors.** GCP HTTP(S) Load Balancing is global by default; AWS ALB is regional. DNS propagation and IP models vary. (HTTP(S) = HyperText Transfer Protocol (Secure); ALB = Application Load Balancer; DNS = Domain Name System.)
4. **Identity terminology.** Azure splits **Entra ID (tenant)** vs **subscriptions**; AWS focuses on **accounts**; GCP uses **projects** under an **organization**. (ID = Identifier.)
5. **Networking defaults.** Default VPCs, firewall rule directions, and NAT outbound models differ—review egress paths and costs. (VPC = Virtual Private Cloud; NAT = Network Address Translation.)
6. **Eventing vs queuing.** Event buses (EventBridge/Event Grid/Eventarc) are not the same as message queues (SQS/Service Bus/Pub/Sub). Choose by semantics. (SQS = Simple Queue Service; Pub/Sub = Publish/Subscribe.)
7. **Serverless nuances.** Cold start, concurrency, and execution timeouts differ across Lambda/Functions/Cloud Functions/Run. (FaaS = Function as a Service.)
8. **Multi‑cloud data egress.** Fees, free tiers, and cross‑cloud waivers change—verify when building portable lakes and DR patterns. (DR = Disaster Recovery.)

---

## 16) Quick Reference – Abbreviations

| Abbrev  | Full form                                                      | Context / note                                                                                 |
| ------- | -------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| ACL     | Access Control List                                            | Network ACLs in VPC/VNet.                                                                      |
| ACR     | Azure Container Registry                                       | Container images/packages.                                                                     |
| ACS     | Azure Communication Services                                   | Email/voice/chat APIs.                                                                         |
| AD      | Active Directory                                               | Directory/identity (Managed AD).                                                               |
| ADF     | Azure Data Factory                                             | Visual ELT/ETL & copy pipelines.                                                               |
| ADLS    | Azure Data Lake Storage                                        | Data lake on Azure (Appendix 18).                                                              |
| ADX     | Azure Data Explorer                                            | Time-series/analytics (“Kusto”).                                                               |
| AKS     | Azure Kubernetes Service                                       | Managed Kubernetes.                                                                            |
| ALB     | Application Load Balancer                                      | L7 load balancer in AWS ELB family.                                                            |
| ANN     | Approximate Nearest Neighbor                                   | Vector similarity search family.                                                               |
| API     | Application Programming Interface                              | Service endpoints, API Mgmt.                                                                   |
| APM     | Application Performance Monitoring                             | App Insights/X-Ray concepts.                                                                   |
| ARM     | Azure Resource Manager                                         | Azure IaC templates (ARM/Bicep).                                                               |
| ASG     | Auto Scaling Group                                             | AWS VM autoscaling.                                                                            |
| ASR     | Azure Site Recovery                                            | Azure DR service (VM replication/orchestration).                                               |
| AWS     | Amazon Web Services                                            | Cloud provider.                                                                                |
| BI      | Business Intelligence                                          | Dashboards (Power BI/Looker/QuickSight).                                                       |
| BQ      | BigQuery                                                       | GCP serverless data warehouse (shorthand).                                                     |
| CAF     | Cloud Adoption Framework                                       | Azure Landing Zone guidance.                                                                   |
| CD      | Continuous Delivery/Deployment                                 | The “CD” in CI/CD.                                                                             |
| CDN     | Content Delivery Network                                       | Edge caching (CloudFront/Front Door/Cloud CDN).                                                |
| CDAP    | Cask Data Application Platform                                 | Basis for Cloud Data Fusion.                                                                   |
| CDK     | Cloud Development Kit                                          | IaC framework (AWS CDK).                                                                       |
| CI      | Continuous Integration                                         | Part of CI/CD.                                                                                 |
| CIAM    | Customer Identity and Access Management                        | B2C auth (Cognito/Entra External ID/Identity Platform).                                        |
| CLB     | Classic Load Balancer                                          | Legacy AWS ELB (L4/L7).                                                                        |
| CCAIP   | Contact Center AI Platform                                     | GCP contact center suite.                                                                      |
| CUD     | Committed Use Discounts                                        | GCP cost commitment model.                                                                     |
| CUR     | Cost and Usage Report                                          | AWS billing export.                                                                            |
| DC      | Data Center                                                    | Private connectivity targets.                                                                  |
| DDoS    | Distributed Denial of Service                                  | Network attack class (Shield/Armor).                                                           |
| DNS     | Domain Name System                                             | Authoritative DNS services.                                                                    |
| DR      | Disaster Recovery                                              | Replication/failover patterns/services.                                                        |
| DW      | Data Warehouse                                                 | Redshift/Synapse/BigQuery.                                                                     |
| EBS     | Elastic Block Store                                            | AWS block storage for EC2.                                                                     |
| EC2     | Elastic Compute Cloud                                          | AWS virtual machines service.                                                                  |
| ECR     | Elastic Container Registry                                     | AWS container registry.                                                                        |
| ECS     | Elastic Container Service                                      | AWS container orchestration.                                                                   |
| EFS     | Elastic File System                                            | AWS NFS file share.                                                                            |
| EKS     | Elastic Kubernetes Service                                     | AWS managed Kubernetes service.                                                                |
| ELB     | Elastic Load Balancing                                         | AWS LB family (ALB/NLB/CLB).                                                                   |
| ELT     | Extract-Load-Transform                                         | Analytics pipeline style.                                                                      |
| EMR     | Elastic MapReduce                                              | AWS managed Hadoop/Spark.                                                                      |
| ETL     | Extract-Transform-Load                                         | Traditional data pipeline style.                                                               |
| FaaS    | Function as a Service                                          | Lambda/Functions/Cloud Functions.                                                              |
| FSx     | Amazon FSx (brand)                                             | Managed file systems (Lustre/ONTAP/OpenZFS/Windows). Brand, not a strict acronym.              |
| FTP     | File Transfer Protocol                                         | Legacy transfer protocol.                                                                      |
| FTPS    | File Transfer Protocol Secure                                  | FTP over TLS.                                                                                  |
| GA      | General Availability                                           | Product lifecycle stage.                                                                       |
| GCP     | Google Cloud Platform                                          | Cloud provider.                                                                                |
| GCS     | Google Cloud Storage                                           | Object storage on GCP (Appendix 18).                                                           |
| GDC     | Google Distributed Cloud                                       | GCP edge/hybrid portfolio.                                                                     |
| GKE     | Google Kubernetes Engine                                       | Managed Kubernetes.                                                                            |
| HA      | High Availability                                              | Redundancy/failover characteristic.                                                            |
| HCI     | Hyper-Converged Infrastructure                                 | Azure Stack HCI.                                                                               |
| HNSW    | Hierarchical Navigable Small World                             | Vector index type (ANN).                                                                       |
| HPC     | High-Performance Computing                                     | Batch/FS/instance classes.                                                                     |
| HSM     | Hardware Security Module                                       | Dedicated crypto hardware.                                                                     |
| HTTP(S) | HyperText Transfer Protocol (Secure)                           | Layer-7 traffic/proxies/LBs.                                                                   |
| IaC     | Infrastructure as Code                                         | ARM/Bicep/CloudFormation/CDK/etc.                                                              |
| IAM     | Identity and Access Management                                 | Roles/policies on resources.                                                                   |
| IDE     | Integrated Development Environment                             | Cloud9/Cloud Workstations/Dev Box.                                                             |
| IoT     | Internet of Things                                             | Device messaging/ops.                                                                          |
| IP      | Internet Protocol                                              | Networking addressing.                                                                         |
| iPaaS   | Integration Platform as a Service                              | Logic Apps / Workflows / connectors.                                                           |
| IPSec   | Internet Protocol Security                                     | VPN tunnel encryption.                                                                         |
| IVF     | Inverted File (index)                                          | Vector DB index type (e.g., IVF-PQ).                                                           |
| K8s     | Kubernetes                                                     | Container orchestration (numeronym).                                                           |
| KMS     | Key Management Service                                         | Envelope encryption APIs.                                                                      |
| LDAP    | Lightweight Directory Access Protocol                          | Directory bind/query.                                                                          |
| LB      | Load Balancer                                                  | Traffic distribution component.                                                                |
| MIG     | Managed Instance Group                                         | GCP VM groups (mentioned conceptually).                                                        |
| MSK     | Managed Streaming for Apache Kafka                             | AWS managed Kafka.                                                                             |
| MWAA    | Managed Workflows for Apache Airflow                           | AWS Airflow service.                                                                           |
| NAT     | Network Address Translation                                    | Egress without public IPs.                                                                     |
| NLB     | Network Load Balancer                                          | L4 load balancer in AWS.                                                                       |
| NLP     | Natural Language Processing                                    | AI text analysis category.                                                                     |
| NLU     | Natural Language Understanding                                 | Conversational AI intent/slots, etc.                                                           |
| NFS     | Network File System                                            | POSIX-style file shares.                                                                       |
| OCI     | Open Container Initiative                                      | Image/spec standard; “OCI registry” compat.                                                    |
| OIDC    | OpenID Connect                                                 | Authn protocol over OAuth 2.0.                                                                 |
| ONTAP   | NetApp ONTAP (brand)                                           | NetApp filesystem/software.                                                                    |
| OS      | Operating System                                               | VM/agent context (“OS Config”).                                                                |
| OT      | Operational Technology                                         | Industrial/IoT systems context.                                                                |
| OTT     | Over-The-Top                                                   | Media streaming delivery model.                                                                |
| OU      | Organizational Unit                                            | AWS Organizations hierarchy node.                                                              |
| PaaS    | Platform as a Service                                          | App Engine/App Service/Beanstalk/etc.                                                          |
| PII     | Personally Identifiable Information                            | Data classification (NLP/Compliance).                                                          |
| Pub/Sub | Publish/Subscribe                                              | Messaging pattern; GCP service name.                                                           |
| RBAC    | Role-Based Access Control                                      | Azure RBAC; permissions model.                                                                 |
| RDS     | Relational Database Service                                    | AWS managed RDBMS (MySQL/PG/SQL Server/etc.).                                                  |
| SUD     | Sustained Use Discounts                                        | GCP usage‑based savings.                                                                       |
| S3      | Simple Storage Service                                         | AWS object storage.                                                                            |
| SAML    | Security Assertion Markup Language                             | SSO federation protocol.                                                                       |
| SAP     | Systems, Applications & Products (SAP SE)                      | Enterprise apps; FSx for Windows/SAP certs.                                                    |
| SCP     | Service Control Policy                                         | Org-level guardrails in AWS.                                                                   |
| SDK     | Software Development Kit                                       | Client libraries/tooling.                                                                      |
| SES     | Simple Email Service                                           | AWS email service.                                                                             |
| SFTP    | SSH File Transfer Protocol                                     | Secure file transfer (over SSH).                                                               |
| SLA     | Service Level Agreement                                        | Availability/latency commitments.                                                              |
| SLO     | Service Level Objective                                        | Internal/reported service target.                                                              |
| SMB     | Server Message Block                                           | Windows/SMB file sharing protocol.                                                             |
| SNS     | Simple Notification Service                                    | Pub/sub notifications & mobile push.                                                           |
| SQL DW  | SQL Data Warehouse                                             | Legacy name → Synapse Dedicated SQL pool.                                                      |
| SQS     | Simple Queue Service                                           | At-least-once message queues.                                                                  |
| SRE     | Site Reliability Engineering                                   | Ops discipline/practices.                                                                      |
| SSM     | Systems Manager                                                | AWS config/state/secrets Parameter Store.                                                      |
| SSO     | Single Sign-On                                                 | Workforce login to consoles/apps.                                                              |
| SQL     | Structured Query Language                                      | Relational data language.                                                                      |
| STT     | Speech-to-Text                                                 | Transcription (speech recognition).                                                            |
| TTS     | Text-to-Speech                                                 | Synthetic voice.                                                                               |
| VM      | Virtual Machine                                                | Compute instance; EC2/VMs/Compute Engine.                                                      |
| VNet    | Virtual Network                                                | Azure private network.                                                                         |
| VPC     | Virtual Private Cloud                                          | AWS/GCP private network.                                                                       |
| VPN     | Virtual Private Network                                        | IPSec tunnels to cloud.                                                                        |
| WAN     | Wide Area Network                                              | Connectivity between sites/clouds.                                                             |
| WAF     | Web Application Firewall                                       | L7 security policy.                                                                            |

---

## 17) Appendix – Near‑Equivalents Not 1:1

- **Lakehouse**: *AWS* (S3 + Lake Formation + Athena/EMR + Redshift Spectrum) / *Azure* (ADLS + Synapse + Delta + **Microsoft Fabric**) / *GCP* (GCS + **Dataplex/BigLake** + BigQuery + Dataflow/Dataform).
- **API Management**: *AWS API Gateway* vs *Azure API Management* vs *GCP API Gateway / Apigee*. Apigee is closest to enterprise API lifecycle with monetization.
- **Vector search**: Rapid changes—validate current limits (index size, dimensions, filter support, HNSW vs IVF, latency SLOs) before committing. (SLO = Service Level Objective.)

---

## 18) Updates (as of Nov 6, 2025)

- **Azure Spring Apps retirement**: On **March 17, 2025**, Azure began a three‑year retirement period for **Azure Spring Apps** (Basic/Standard/Enterprise), with **final retirement on March 31, 2028**. For Spring workloads on Azure, use **Azure Container Apps** or **AKS**. (This does not change the PaaS Web row—**App Service** remains current.)
- **BigQuery Vector Search** reached **GA on Sept 27, 2024**, included above under Vector Search.
- **Cloud Source Repositories**: not available to new GCP customers since **June 17, 2024**; prefer GitHub/Bitbucket with **Cloud Build** triggers.
- **Azure Media Services** retired **June 30, 2024**; use partners for live/OTT pipelines and **Azure AI Video Indexer** for analysis. (OTT = Over‑The‑Top.)

---
