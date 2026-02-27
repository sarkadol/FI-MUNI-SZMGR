# Cloudové počítání a distribuované databáze (PA200, PA195)

## Popis okruhu

Cloudové počítání: základní principy, infrastruktura jako služba (IaaS), virtualizace a kontejnery, migrace na cloud, bezpečnost služeb, horizontální a vertikální škálovatelnost. Současné technologie a poskytovatelé cloudových služeb. Distribuované databáze: principy a výhody NoSQL přístupu, konzistence, distribuce dat. Úložiště párů klíč-hodnota, dokumentové databáze, grafové databáze, sloupcově orientované databáze.

## Témata

### Cloudové počítání

#### Základní principy
- Definice (NIST): on-demand self-service, broad network access, resource pooling, rapid elasticity, measured service
- Modely nasazení:
  - **Veřejný cloud (Public Cloud):** sdílená infrastruktura dostupná přes internet
  - **Privátní cloud (Private Cloud):** dedikovaná infrastruktura pro jednu organizaci
  - **Hybridní cloud (Hybrid Cloud):** kombinace veřejného a privátního
  - **Multi-cloud:** využití více cloudových poskytovatelů
- Ekonomický model: pay-as-you-go, OpEx vs. CapEx

#### Modely cloudových služeb
- **IaaS (Infrastructure as a Service):** virtuální stroje, úložiště, síť; příklady: AWS EC2, Azure VMs, Google Compute Engine
- **PaaS (Platform as a Service):** platforma pro vývoj a provoz aplikací; příklady: Heroku, AWS Elastic Beanstalk, Google App Engine
- **SaaS (Software as a Service):** hotové aplikace přes internet; příklady: Gmail, Salesforce, Microsoft 365
- **FaaS/Serverless:** funkce jako služba; příklady: AWS Lambda, Azure Functions, Google Cloud Functions

#### Infrastruktura jako služba (IaaS)
- Virtuální stroje: CPU, RAM, disk, síť konfigurovatelné na vyžádání
- Síťové prvky: virtuální sítě (VPC/VNet), load balancery, firewally
- Úložiště: blokové (EBS), objektové (S3), souborové (EFS)
- Správa prostředků: instance typy, regiony, zóny dostupnosti

#### Virtualizace a kontejnery
- **Virtualizace:** hypervisor (Type 1: bare-metal, Type 2: hosted), VM izolace, snímky (snapshots)
  - KVM, VMware ESXi, Hyper-V, Xen
- **Kontejnery:** sdílené jádro OS, izolace pomocí namespaces a cgroups
  - Docker: image, container, Dockerfile, Docker Hub
  - Výhody oproti VM: rychlejší start, nižší overhead, přenositelnost
- **Kubernetes:** orchestrace kontejnerů – viz Infrastrukturní a cloudové systémy (PA234)

#### Migrace na cloud (Cloud Migration)
- Strategie „6R":
  1. **Rehost** (Lift & Shift): přenos bez úprav
  2. **Replatform** (Lift, Tinker & Shift): minimální optimalizace
  3. **Repurchase:** přechod na SaaS
  4. **Refactor/Re-architect:** redesign pro cloud-native
  5. **Retire:** vyřazení zbytečných aplikací
  6. **Retain:** ponechání on-premise
- Výzvy migrace: datová závislost, latence, bezpečnost, náklady

#### Bezpečnost cloudových služeb
- Shared responsibility model: poskytovatel vs. zákazník
- Identity and Access Management (IAM): uživatelé, role, politiky (AWS IAM, Azure AD)
- Šifrování dat: at-rest (AES-256), in-transit (TLS/SSL)
- Síťová bezpečnost: security groups, NACLs, VPN, DDoS ochrana
- Compliance a certifikace: ISO 27001, SOC 2, GDPR

#### Horizontální a vertikální škálovatelnost
- **Vertikální škálování (Scale Up):** zvyšování kapacity jednoho uzlu (více RAM, CPU); limity hardwaru
- **Horizontální škálování (Scale Out):** přidání dalších uzlů; neomezená škálovatelnost; vyžaduje distribuovaný design
- Auto-scaling: automatické přidávání/odebírání instancí dle zatížení (AWS Auto Scaling, GCP Managed Instance Groups)

#### Současné technologie a poskytovatelé
- **Amazon Web Services (AWS):** největší poskytovatel; EC2, S3, RDS, Lambda, DynamoDB
- **Microsoft Azure:** silná integrace s enterprise prostředím; VMs, Azure SQL, Cosmos DB, AKS
- **Google Cloud Platform (GCP):** Compute Engine, GKE, BigQuery, Spanner
- **Alibaba Cloud, IBM Cloud:** regionální a specializovaní hráči
- OpenStack: open-source platforma pro privátní cloudy

### Distribuované databáze

#### Principy a výhody NoSQL přístupu
- Motivace: limitace relačních databází při velkých datech a horizontálním škálování
- Vlastnosti NoSQL:
  - Schéma-less nebo flexibilní schéma
  - Horizontální škálování
  - Optimalizace pro konkrétní vzory přístupu
  - BASE místo ACID: Basically Available, Soft state, Eventually consistent
- Výhody: škálovatelnost, výkon pro specifické vzory, flexibilita schématu
- Nevýhody: ztráta ACID záruk, složitější konzistentní dotazy, omezená podpora ad-hoc dotazů

#### CAP teorém
- **Consistency (Konzistence):** všechny uzly vidí stejná data najednou
- **Availability (Dostupnost):** každý požadavek dostane odpověď
- **Partition tolerance (Odolnost vůči rozdělení sítě):** systém funguje při výpadku komunikace
- Teorém: nelze zaručit všechny tři vlastnosti současně; nutno volit kompromis
- Reálné systémy: CP (HBase, Zookeeper), AP (Cassandra, CouchDB), CA (tradiční RDBMS)

#### PACELC model (rozšíření CAP)
- Při rozdělení (P): volba konzistence vs. dostupnosti
- Bez rozdělení (E): volba latence vs. konzistence

#### Konzistence v distribuovaných databázích
- **Strong consistency:** linearizovatelnost; čtení vždy vidí poslední zápis
- **Eventual consistency:** systém se nakonec synchronizuje; přijímá dočasnou nekonzistenci
- **Causal consistency:** kauzálně závislé operace vidí správné pořadí
- Quorum čtení/zápisu: W + R > N pro silnou konzistenci

#### Distribuce dat
- **Replikace:** kopie dat na více uzlech; zvyšuje dostupnost a výkon čtení
  - Master-slave replikace
  - Multi-master replikace
- **Sharding (partitioning):** data rozdělena na fragmenty distribuované napříč uzly
  - Range sharding, hash sharding, consistent hashing
- Consistent hashing: minimalizace přesunu dat při přidání/odebrání uzlu

### Typy NoSQL databází

#### Úložiště párů klíč-hodnota (Key-Value Stores)
- Jednoduchý datový model: klíč → hodnota (libovolná binární data)
- Operace: GET, PUT, DELETE; jednoduchý dotazovací model
- Použití: session management, caching, shopping cart
- Příklady:
  - **Redis:** in-memory, datové struktury (listy, sety, hash mapy), perzistence, Pub/Sub
  - **Amazon DynamoDB:** managed, škálovatelný, SSD-based, globální tabulky
  - **Memcached:** distribuovaná cache, jednoduché klíč-hodnota

#### Dokumentové databáze
- Datový model: dokumenty (JSON, BSON, XML) s flexibilním schématem
- Každý dokument je self-contained s vnořenými strukturami
- Dotazování: dle obsahu dokumentu, indexy na atributy
- Použití: e-commerce katalogy, CMS, uživatelské profily
- Příklady:
  - **MongoDB:** BSON dokumenty, agregační pipeline, sharding, replica sets
  - **CouchDB:** HTTP/REST API, MVCC, offline-first synchronizace
  - **Elasticsearch:** full-text vyhledávání, distribuovaný, analytické dotazy

#### Grafové databáze
- Datový model: vrcholy (nodes) + hrany (relationships) + vlastnosti
- Dotazovací jazyky: Cypher (Neo4j), Gremlin, SPARQL
- Efektivní pro: traversal grafů, pathfinding, doporučovací systémy
- Použití: sociální sítě, fraud detection, znalostní grafy
- Příklady:
  - **Neo4j:** nejrozšířenější grafová databáze, Cypher dotazovací jazyk, ACID
  - **Amazon Neptune:** managed grafová databáze, Gremlin + SPARQL

#### Sloupcově orientované databáze (Column-Family / Wide-Column)
- Datový model: rodiny sloupců (column families); záznamy mohou mít různé sloupce
- Ukládání po sloupcích: efektivní komprese a analytické dotazy
- Použití: IoT, časové řady, analytické zpracování, write-heavy workloads
- Příklady:
  - **Apache Cassandra:** masterless architektura, eventuální konzistence, CQL dotazovací jazyk, lineární škálovatelnost
  - **HBase:** na HDFS, silná konzistence, integrace s Hadoop ekosystémem
  - **Google Bigtable:** základ pro HBase, spravovaná služba v GCP
