# Infrastrukturní a cloudové systémy (PA234)

> **Poznámka:** Povinné pro studium dle kontrolní šablony 2024/2025 nebo novější.

## Popis okruhu

Data-center architecture; supercomputers vs. heterogeneous clusters; orchestration architectures (PBS/grids, Kubernetes, OpenStack); GPU vs. CPU computing and workloads that benefit from massive parallelism; scalable application models; storage tiers, technologies, data temperature and movement; infrastructure resilience and reliability; automation, DevOps/GitOps and SRE; workflow managers and workload portability; identity, SSO, and AAI.

## Témata

### Architektura datových center
- Fyzická organizace datového centra: racky, řady, zóny, sály
- Síťová topologie: spine-leaf, three-tier (core-aggregation-access)
- Napájení a chlazení: PUE (Power Usage Effectiveness), redundance
- Konvergovaná a hyperkonvergovaná infrastruktura

### Superpočítače vs. heterogenní clustery
- Architektura superpočítačů: tightly coupled systémy, vysokorychlostní propojení (InfiniBand, Omni-Path)
- Heterogenní clustery: kombinace CPU, GPU, FPGA uzlů
- Srovnání výkonnosti a vhodnosti pro různé pracovní zátěže (workloads)
- Příklady: TOP500, Green500, přední HPC systémy

### Orchestrační architektury

#### PBS/Gridy
- Portable Batch System (PBS/Torque) a SLURM
- Gridové výpočty: princip, middleware (UNICORE, ARC, gLite)
- MetaCentrum a národní gridy

#### Kubernetes
- Architektura: Control Plane (API server, etcd, scheduler, controller manager), Worker Nodes
- Základní objekty: Pod, Deployment, Service, ConfigMap, Secret, PersistentVolume
- Škálování: HPA (Horizontal Pod Autoscaler), VPA, Cluster Autoscaler
- Síťování v Kubernetes: CNI pluginy, Ingress, NetworkPolicy

#### OpenStack
- Komponenty: Nova (výpočty), Neutron (síť), Cinder (blokové úložiště), Swift (objektové úložiště), Glance (obrazy), Keystone (identita), Heat (orchestrace)
- Virtualizační hypervisory: KVM, Xen, VMware
- Projektové a tenantové oddělení zdrojů

### GPU vs. CPU výpočty
- Architektura GPU: masivní paralelismus, SIMD/SIMT, tisíce jader vs. desítky jader CPU
- CUDA a OpenCL: programovací modely pro GPU
- Workloady vhodné pro GPU: hluboké učení, simulace, renderování, kryptografie
- Workloady vhodné pro CPU: sekvenční logika, nízká latence, diverzifikovaná výpočetní logika

### Škálovatelné aplikační modely
- Horizontální vs. vertikální škálování
- Mikroservisová architektura: výhody, nevýhody, vzory (API Gateway, Circuit Breaker, Service Mesh)
- Serverless / FaaS (Function as a Service): AWS Lambda, OpenFaaS
- Event-driven architektura a message brokery (Kafka, RabbitMQ)
- CAP teorém a jeho důsledky pro distribuované systémy

### Úrovně úložiště, technologie, teplota dat a jejich přesun

#### Úrovně úložiště (Storage Tiers)
- Tier 0: NVMe SSD, paměti (RAM, Persistent Memory)
- Tier 1: SSD (SATA/SAS), výkonné pole
- Tier 2: HDD, SAN/NAS
- Tier 3: páskové knihovny, archivní úložiště

#### Technologie úložišť
- SAN (Storage Area Network) vs. NAS (Network Attached Storage)
- Objektové úložiště (S3-compatible: Ceph, MinIO)
- Distribuované souborové systémy: HDFS, Lustre, GPFS/Spectrum Scale

#### Teplota dat a jejich přesun
- Hot / Warm / Cold data a příslušné strategie ukládání
- Lifecycle policies pro automatický přesun dat
- Data tiering a hierarchické správy úložišť (HSM)

### Odolnost a spolehlivost infrastruktury
- Vysoká dostupnost (HA): redundance, failover, clustering
- Recovery Time Objective (RTO) a Recovery Point Objective (RPO)
- RAID a replikace dat
- Geografická redundance: multi-site a multi-region nasazení
- Chaos engineering (Chaos Monkey)

### Automatizace, DevOps/GitOps a SRE

#### DevOps
- CI/CD pipelines: Jenkins, GitLab CI, GitHub Actions
- Infrastructure as Code (IaC): Terraform, Ansible, Pulumi
- Kontejnerizace a kontejnerové registry: Docker, Podman

#### GitOps
- Principy GitOps: Git jako single source of truth
- Nástroje: ArgoCD, Flux
- Deklarativní správa infrastruktury

#### SRE (Site Reliability Engineering)
- SLI, SLO, SLA: definice a vztahy
- Error budget a jeho využití
- Toil a jeho eliminace
- Postmortémy a kultura bez obviňování (blameless postmortems)

### Správci pracovních postupů a přenositelnost zátěže
- Workflow manažery: Apache Airflow, Nextflow, Snakemake, Luigi
- Přenositelnost zátěže: kontejnery (Docker, Singularity/Apptainer) pro HPC
- Standardy: CWL (Common Workflow Language), WDL
- Multi-cloud a hybridní cloud strategie

### Identita, SSO a AAI

#### Identity Management
- Adresářové služby: LDAP, Active Directory
- Identity Provider (IdP) a Service Provider (SP)

#### Single Sign-On (SSO)
- Protokoly: SAML 2.0, OAuth 2.0, OpenID Connect (OIDC)
- Kerberos a lístkovací systémy

#### AAI (Authentication and Authorization Infrastructure)
- Federativní identity: eduGAIN, GÉANT
- AARC Blueprint Architecture
- Role-Based Access Control (RBAC) a Attribute-Based Access Control (ABAC)
