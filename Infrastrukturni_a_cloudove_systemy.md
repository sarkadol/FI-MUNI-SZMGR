# Infrastrukturní a cloudové systémy

> Data-center architecture; supercomputers vs. heterogeneous clusters; orchestration architectures (PBS/grids, Kubernetes, OpenStack); 
> GPU vs. CPU computing and workloads that benefit from massive parallelism; scalable application models; 
> storage tiers, technologies, data temperature and movement; infrastructure resilience and reliability; 
> automation, DevOps/GitOps and SRE; workflow managers and workload portability; identity, SSO, and AAI. (PA234) (povinné pro studium dle kontrolní šablony 2024/2025 nebo novější)


## Architektura datových center (Data-center architecture)
* **Fyzická vrstva:** Napájení, chlazení (PUE - Power Usage Effectiveness), racky.
* **Network Fabric:** Tradiční třívrstvá architektura vs. moderní **Spine-Leaf** (pro nízkou latenci a east-west provoz).
* **SDN (Software Defined Networking):** Oddělení control plane a data plane.
* **Disagregace:** Trend oddělování compute, storage a network zdrojů pro lepší efektivitu.

---

## Superpočítače vs. heterogenní clustery (Supercomputers vs. heterogeneous clusters)
* **Superpočítače (HPC):** Navrženy pro těsně vázané úlohy (MPI), nízká latence sítě (InfiniBand), sdílené paralelní souborové systémy (Lustre).
* **Heterogenní clustery:** Obsahují nody s různým HW (různé generace CPU, různé typy GPU, FPGA).
* **HTC (High Throughput Computing):** Zaměření na zpracování obrovského množství nezávislých úloh (např. projekt SETI@home nebo analýzy z CERNu).

---

## Orchestrační architektury (PBS/grids, Kubernetes, OpenStack)
* **Batch systémy (PBS, Slurm):** Plánování úloh do front, zaměření na maximální využití CPU/GPU po omezený čas.
* **IaaS platformy (OpenStack):** Správa virtuálních strojů, sítí a storage (tenants, quotas).
* **Kontejnerová orchestrace (Kubernetes):** Deklarativní správa kontejnerizovaných aplikací, self-healing, service discovery.
* **Grid Computing:** Propojování geograficky distribuovaných zdrojů (např. EGI).

---

## GPU vs. CPU výpočty a massive parallelism workloads
* **CPU:** Optimalizováno pro sériové zpracování, komplexní instrukční sady, velká cache, málo jader (Latency-oriented).
* **GPU:** Tisíce jednoduchých jader, masivní propustnost paměti (Throughput-oriented).
* **Workload:** Úlohy vhodné pro GPU (matice, deep learning, krypto), role SIMT (Single Instruction, Multiple Threads).
* **Heterogenní výpočty:** CUDA, OpenCL, přenos dat mezi host (CPU) a device (GPU).

---

## Škálovatelné aplikační modely (Scalable application models)
* **Stateless vs. Stateful:** Proč je bezstavovost klíčem ke škálování.
* **Microservices:** Dekompozice systému pro nezávislé škálování částí.
* **Cloud-native principy:** Elasticita, resilience, observability.
* **12-factor App:** Standardy pro moderní cloudové aplikace.

---

## Úrovně úložišť, technologie, teplota dat a přesun (Storage tiers, technologies, data temperature and movement)
* **Technologie:** NVMe, SSD, HDD, Tape (pásky).
* **Data Temperature:**
    * **Hot:** Často přístupná, nízká latence (NVMe).
    * **Warm:** Méně častý přístup (SATA SSD/HDD).
    * **Cold (Archive):** Zřídkavý přístup, nízká cena (Tape, Glacier).
* **ILM (Information Lifecycle Management):** Automatizovaný přesun dat mezi úrovněmi na základě politiky.

---

## Odolnost a spolehlivost infrastruktury (Infrastructure resilience and reliability)
* **Fault Tolerance vs. High Availability:** Jaký je mezi nimi rozdíl v ceně a implementaci.
* **Redundance:** N+1, 2N, geografická redundance.
* **Metriky:** **MTBF** (Mean Time Between Failures), **MTTR** (Mean Time To Repair).
* **Disaster Recovery:** RTO (Recovery Time Objective) a RPO (Recovery Point Objective).

---

## Automatizace, DevOps/GitOps a SRE
* **DevOps:** Kultura propojující vývoj a provoz (CI/CD).
* **GitOps:** Single source of truth v Gitu, deklarativní stav infrastruktury (např. ArgoCD).
* **SRE (Site Reliability Engineering):** Aplikace softwarového inženýrství na provoz.
    * **SLI** (Indicator), **SLO** (Objective), **SLA** (Agreement).
    * **Error Budgets:** Mechanismus pro vyvážení inovací a stability.

---

## Workflow manažeři a přenositelnost úloh (Workflow managers and workload portability)
* **Workflow manažeři:** Snakemake, Nextflow, Airflow, Cromwell.
* **DAG (Directed Acyclic Graph):** Reprezentace závislostí úloh.
* **Portability:** Role kontejnerů (Singularity/Apptainer v HPC vs. Docker v cloudu) pro reprodukovatelnost vědeckých výpočtů.

---

## Identita, SSO a AAI (Identity, SSO, and AAI)
* **AAI (Authentication and Authorisation Infrastructure):** Federovaná identita (např. přihlášení přes univerzitní účet do evropského gridu).
* **Protokoly:** SAML, OAuth2, OpenID Connect (OIDC).
* **SSO (Single Sign-On):** Uživatelská přívětivost vs. bezpečnostní rizika.
* **Role-Based Access Control (RBAC):** Správa oprávnění v rozsáhlých systémech.
