# Cloudové počítání a distribuované databáze

> Cloudové počítání: základní principy, infrastruktura jako služba (IaaS), virtualizace a kontejnery, migrace na cloud, 
> bezpečnost služeb, horizontální a vertikální škálovatelnost. 
> Současné technologie a poskytovatelé cloudových služeb. 
> Distribuované databáze: principy a výhody NoSQL přístupu, konzistence, distribuce dat. 
> Úložiště párů klíč-hodnota, dokumentové databáze, grafové databáze, sloupcově orientované databáze. (PA200, PA195)
> 
> ## Cloudové počítání

### Základní principy
* On-demand self-service
* Broad network access
* Resource pooling
* Rapid elasticity
* Measured service

## Infrastruktura jako služba (IaaS)
* Definice a model sdílené odpovědnosti
* Virtuální výpočetní zdroje (CPU, RAM, Storage, Networking)
* Příklady: AWS EC2, Azure VMs, Google Compute Engine

## Virtualizace a kontejnery
* Hypervizory a virtuální stroje (VM)
* Kontejnerizace (Docker, podpora v jádře OS)
* Rozdíly v izolaci, výkonu a přenositelnosti
* Orchestrace kontejnerů (Kubernetes)

## Migrace na cloud
* Strategie migrace (6 R's: Rehost, Replatform, Refactor atd.)
* Analýza nákladů (TCO) a rizik
* Hybridní a multi-cloud scénáře

## Bezpečnost služeb
* Identity and Access Management (IAM)
* Šifrování dat (at-rest, in-transit)
* Governance, compliance a auditování
* Síťová bezpečnost (VPC, Security Groups)

## Horizontální a vertikální škálovatelnost
Škálovatelnost je schopnost systému efektivně zvládat rostoucí nároky (data, uživatelé, výpočetní výkon) přidáváním hardwarových prostředků bez nutnosti zásadní změny architektury.

### Vertikální škálování (Scaling Up)
Zvyšování výkonu **jednoho stávajícího uzlu** (navýšení CPU, RAM, GPU nebo kapacity disku).
* **Vlastnosti:** Architektonicky nejjednodušší přístup; aplikace běží na jednom stroji, což eliminuje síťovou latenci mezi uzly.
* **Omezení:** Má pevný **hardwarový strop** (fyzické limity serveru); představuje **SPOF** (Single Point of Failure); náklady rostou nelineárně (špičkový hardware je neúměrně drahý).
* **Kontext AI:** Ideální pro trénování modelů, které vyžadují vysokou propustnost paměti na jednom stroji a vejdou se do kapacity jedné nebo více GPU v rámci jednoho serveru.

### Horizontální škálování (Scaling Out)
Propojování **více běžných strojů** do jednoho logického celku (**clusteru**).
* **Vlastnosti:** Teoreticky **neomezený růst** přidáváním dalších uzlů; vysoká dostupnost a odolnost proti výpadku (při selhání uzlu zbytek clusteru pracuje dál).
* **Omezení:** Vyžaduje komplexní distribuční logiku (Load Balancer, Sharding); naráží na limity sítě a **CAP teorém** (kompromis mezi konzistencí a dostupností).
* **Kontext AI:** Klíčové pro Big Data a distribuované trénování (např. trénování LLM rozprostřené přes tisíce GPU pomocí frameworků jako Horovod nebo PyTorch Distributed).

### Srovnání parametrů

| Vlastnost | Vertikální (Scale-Up) | Horizontální (Scale-Out) |
| :--- | :--- | :--- |
| **Metoda** | Navýšení výkonu (silnější HW) | Navýšení počtu (více uzlů) |
| **Limit výkonu** | Pevně daný HW limity | Prakticky neomezený |
| **Dostupnost** | Nízká (výpadek stroje = výpadek všeho) | Vysoká (redundance uzlů) |
| **Cena** | Nelineární (high-end HW je drahý) | Lineární (běžný "commodity" HW) |
| **Administrace** | Snadná (správa jedné instance) | Náročná (správa clusteru a sítě) |

<img alt="img.png" src="img/hor-ver-scaling.png" width="500"/>

### Další pojmy související se škálováním

* **Load Balancer:** Rozděluje příchozí provoz mezi více serverů pro rovnoměrné vytížení.
* **Statelessness:** Architektura bez ukládání stavu na uzlu, umožňující snadné přidávání instancí.
* **Sharding:** Horizontální dělení velkých databází na menší části (shardy) napříč uzly.
* **Auto-scaling:** Automatické upravování počtu běžících instancí podle aktuální zátěže.
* **Read Replicas:** Kopie databáze vyhrazené pro čtení, odlehčující hlavnímu (zápisovému) uzlu.
* **Caching:** Ukládání dat v rychlé paměti (RAM) pro snížení počtu dotazů na úložiště.
* **Rate Limiting:** Omezení počtu požadavků na uživatele pro ochranu systému před zahlcením.
* **High Availability (HA):** Návrh systému zajišťující provoz i při selhání jednotlivých komponent.
* **Latency vs. Throughput:** Latence je doba odezvy, propustnost je množství odbavených požadavků.

## Současné technologie a poskytovatelé cloudových služeb
* Dominantní poskytovatelé (AWS, Azure, GCP)
* Serverless computing (FaaS)
* Cloud-native technologie a mikro-služby

## Distribuované databáze
Distribuované databáze (DDB) ukládají data na více uzlech propojených sítí. Cílem je zvýšení dostupnosti, odolnosti proti chybám a výkonu při zpracování velkých objemů dat.

### CAP teorém (Brewerův teorém)
Základní teoretický limit distribuovaných systémů. Říká, že nelze současně zajistit všechny tři následující vlastnosti:

* **Consistency (Konzistence):** Všichni klienti vidí ve stejný okamžik stejná data (všechny uzly mají stejnou kopii).
* **Availability (Dostupnost):** Každý požadavek obdrží odpověď (úspěch/selhání) i při výpadku některých uzlů.
* **Partition Tolerance (Odolnost proti rozdělení):** Systém funguje i v případě, že dojde k přerušení komunikace mezi uzly (rozdělení sítě).

**V praxi:** Protože v distribuovaném prostředí **musíme** počítat s chybami sítě ($P$), 
systémy se dělí na **CP** (upřednostňují konzistenci, např. HBase) a **AP** (upřednostňují dostupnost, např. Cassandra).

### BASE model
Alternativa k tradičnímu modelu **ACID** (u RDBMS), typická pro NoSQL databáze:
* **Basically Available:** Systém zaručuje dostupnost dat i při částečných výpadcích.
* **Soft state:** Stav systému se může měnit v čase i bez uživatelských vstupů (kvůli synchronizaci).
* **Eventual consistency (Eventuální konzistence):** Pokud přestanou přicházet aktualizace, všechny uzly se časem dopracují ke stejné hodnotě (data nejsou konzistentní hned, ale "nakonec").

## Principy a výhody NoSQL přístupu
* Flexibilita schématu (schema-less)
* CAP teorém (Consistency, Availability, Partition Tolerance)
* BASE vlastnosti (Basically Available, Soft state, Eventual consistency)

## Konzistence
* Silná vs. Eventuální konzistence
* Quorum-based konzistence
* Distribuované transakce a 2PC (Two-Phase Commit)

## Distribuce dat
* Replikace (Master-Slave, Multi-Master)
* Sharding (Partitioning) a distribuční klíče
* Latence a geodistribuce

## Úložiště párů klíč-hodnota
* Princip (Key-Value pairs)
* Příklady: Redis, Riak, DynamoDB

## Dokumentové databáze
* Formáty (JSON, BSON, XML)
* Indexování vnořených polí
* Příklady: MongoDB, CouchDB

## Grafové databáze
* Uzly, hrany a vlastnosti (Property Graphs)
* Algoritmy pro procházení grafů
* Příklady: Neo4j, Amazon Neptune

## Sloupcově orientované databáze
* Column families a sparse storage
* Optimalizace pro analytické dotazy
* Příklady: Apache Cassandra, HBase, Google Bigtable