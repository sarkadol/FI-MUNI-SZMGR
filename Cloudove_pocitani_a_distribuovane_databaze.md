# Cloudové počítání a distribuované databáze

> Cloudové počítání: základní principy, infrastruktura jako služba (IaaS), virtualizace a kontejnery, migrace na cloud, 
> bezpečnost služeb, horizontální a vertikální škálovatelnost. 
> Současné technologie a poskytovatelé cloudových služeb. 
> Distribuované databáze: principy a výhody NoSQL přístupu, konzistence, distribuce dat. 
> Úložiště párů klíč-hodnota, dokumentové databáze, grafové databáze, sloupcově orientované databáze. (PA200, PA195)


## Základní principy cloud computingu

Cloud computing je model umožňující všudypřítomný a pohodlný síťový přístup na vyžádání ke sdílenému fondu konfigurovatelných výpočetních zdrojů. Tyto zdroje (sítě, servery, úložiště, aplikace) lze rychle poskytnout a uvolnit s minimálním úsilím při správě nebo interakci s poskytovatelem.

### Hlavní charakteristiky (dle NIST)
* **On-demand self service:** Uživatel si může automaticky sjednat výpočetní kapacity (např. čas serveru nebo síťové úložiště) bez nutnosti lidské interakce s poskytovatelem služeb.
* **Broad network access:** Služby jsou dostupné přes síť prostřednictvím standardních mechanismů, které podporují různé platformy (mobilní telefony, tablety, notebooky).
* **Resource pooling:** Výpočetní zdroje poskytovatele jsou sdíleny více spotřebiteli pomocí modelu multi-tenancy, přičemž různé fyzické a virtuální zdroje jsou dynamicky přiřazovány podle poptávky.
* **Rapid elasticity:** Kapacity mohou být elasticky uvolňovány nebo poskytovány (často automaticky), aby bylo možné rychle škálovat směrem nahoru i dolů podle aktuálních potřeb.
* **Measured service:** Cloudové systémy automaticky řídí a optimalizují zdroje pomocí měření spotřeby (např. úložiště, procesor, šířka pásma), což umožňuje transparentní vyúčtování pro poskytovatele i uživatele.

### Modely nasazení (Deployment Models)
* **Public cloud:** Infrastruktura je přístupná široké veřejnosti a vlastněna organizací prodávající cloudové služby. _např. AWS, Azure._
* **Private cloud:** Infrastruktura je provozována výhradně pro jednu organizaci. _např. interní datové centrum firmy._
* **Hybrid cloud:** Kombinace dvou nebo více cloudů (soukromých, komunitních nebo veřejných), které zůstávají unikátními entitami, ale jsou propojeny technologií umožňující přenositelnost dat a aplikací.

### Modely služeb (Service Models)
* **IaaS (Infrastructure as a Service):** Poskytování základní infrastruktury (virtuální stroje, sítě, úložiště).
* **PaaS (Platform as a Service):** Poskytování platformy pro vývoj a běh aplikací bez nutnosti správy podkladového HW a OS.
* **SaaS (Software as a Service):** Poskytování hotových aplikací běžících v cloudové infrastruktuře.

<img alt="img.png" src="img/iaas-paas.png" width="600"/>
---
## Infrastruktura jako služba (IaaS)
* Definice a model sdílené odpovědnosti
* Virtuální výpočetní zdroje (CPU, RAM, Storage, Networking)
* Příklady: AWS EC2, Azure VMs, Google Compute Engine

---
## Virtualizace a kontejnery
* Hypervizory a virtuální stroje (VM)
* Kontejnerizace (Docker, podpora v jádře OS)
* Rozdíly v izolaci, výkonu a přenositelnosti
* Orchestrace kontejnerů (Kubernetes)

---
## Migrace na cloud
* Strategie migrace (6 R's: Rehost, Replatform, Refactor atd.)
* Analýza nákladů (TCO) a rizik
* Hybridní a multi-cloud scénáře

---
## Bezpečnost služeb
* Identity and Access Management (IAM)
* Šifrování dat (at-rest, in-transit)
* Governance, compliance a auditování
* Síťová bezpečnost (VPC, Security Groups)

---
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

---
## Současné technologie a poskytovatelé cloudových služeb
* Dominantní poskytovatelé (AWS, Azure, GCP)
* Serverless computing (FaaS)
* Cloud-native technologie a mikro-služby

---
## Distribuované databáze

Distribuované databáze (DDB) ukládají data na více uzlech propojených sítí. Cílem je zvýšení dostupnosti, odolnosti proti chybám a výkonu při zpracování velkých objemů dat.

### CAP teorém (Brewerův teorém)
Základní teoretický limit distribuovaných systémů. Říká, že nelze současně zajistit všechny tři následující vlastnosti:

* **Consistency (Konzistence):** Všichni klienti vidí ve stejný okamžik stejná data. 
  _např.: Pokud v bankovní aplikaci vyberete peníze, zůstatek se okamžitě projeví na všech pobočkách světa stejně._
* **Availability (Dostupnost):** Každý požadavek obdrží odpověď (i za cenu, že data nejsou nejčerstvější). 
  _např.: I když vypadne polovina serverů Facebooku, stále byste měli vidět svůj feed, i kdyby chyběl nejnovější příspěvek._
* **Partition Tolerance (Odolnost proti rozdělení):** Systém funguje i při přerušení komunikace mezi uzly. 
  _např.: Servery v Evropě a USA spolu přestanou komunikovat, ale oba regiony stále lokálně obsluhují své uživatele._

**V praxi:** Protože v distribuovaném prostředí **musíme** počítat s chybami sítě ($P$), systémy se dělí na:
* **CP (Consistency + Partition):** Upřednostňují přesnost. Při rozpadu sítě raději přestanou odpovídat, než aby vracely nekonzistentní data. _(např. HBase, MongoDB v určitém nastavení)_
* **AP (Availability + Partition):** Upřednostňují rychlost a dostupnost. Při rozpadu sítě odpoví, co vědí, i když se data na různých uzlech mohou lišit. _(např. Cassandra, DynamoDB)_

### BASE model
Alternativa k tradičnímu modelu **ACID** (u RDBMS), typická pro NoSQL databáze, které preferují škálovatelnost před okamžitou přesností:
* **Basically Available:** Systém zaručuje dostupnost dat i při částečných výpadcích uzlů.
* **Soft state:** Stav systému se může měnit i bez uživatelských vstupů. 
  _např.: Data se na pozadí „probublávají“ z jednoho serveru na druhý, aby se srovnaly rozdíly._
* **Eventual consistency (Eventuální konzistence):** Data nejsou konzistentní hned, ale „nakonec“ se všechny uzly synchronizují. 
  _např.: Na YouTube změníte název videa – vy ho vidíte hned, ale divákovi na druhém konci světa se starý název může zobrazovat ještě dalších 10 minut._

---
## Principy a výhody NoSQL přístupu
NoSQL (Not Only SQL) systémy vznikly jako odpověď na omezení tradičních relačních databází (RDBMS) při zpracování obrovských objemů dat (**Big Data**) a potřebě extrémního horizontálního škálování.

### Základní principy NoSQL
* **Non-relational:** Data nejsou uložena v tabulkách s pevnými vztahy (cizí klíče), což eliminuje drahé operace typu `JOIN`.
* **Schema-less (Flexibilní schéma):** Záznamy v jedné kolekci mohou mít různé struktury. 
  _např.: V databázi uživatelů může mít jeden profil pole "Twitter", zatímco druhý ne, aniž by se musela měnit celá struktura databáze._
* **Horizontální škálovatelnost:** Od počátku navrženo pro běh na stovkách levných serverů (clusteru), nikoliv na jednom silném stroji.
* **BASE místo ACID:** Upřednostňuje dostupnost a rychlost před okamžitou konzistencí.
* **Agregace dat:** Související data se často ukládají k sobě do jednoho objektu, místo aby byla rozprostřena v mnoha tabulkách.
  _např.: Objednávka i se všemi položkami je uložena jako jeden JSON dokument._

### Hlavní výhody
* **Vysoký výkon (Throughput):** Optimalizováno pro specifické typy dotazů (např. bleskové vyhledání podle klíče).
* **Škálovatelnost:** Snadné přidávání dalších uzlů do clusteru za provozu bez výpadku.
* **Flexibilita vývoje:** Ideální pro agilní vývoj, kde se datový model často mění. Není nutné provádět náročné migrace schématu.
* **Vysoká dostupnost:** Nativní podpora replikace a odolnost proti výpadkům jednotlivých uzlů.

### Kdy NoSQL nepoužít
* Pokud aplikace vyžaduje komplexní transakce nad mnoha různými entitami (např. účetnictví).
* Pokud jsou data silně relační a vyžadují časté a složité spojování (JOIN) napříč celou databází.
---
## Konzistence
V distribuovaných systémech určuje model konzistence pravidla pro to, jak a kdy uvidí různí uživatelé změny v datech provedené někým jiným.

* **Silná konzistence (Strong Consistency):** Po zápisu uvidí každý další čtenář okamžitě novou hodnotu bez ohledu na to, ke kterému uzlu se připojí. 
  _např.: Systém pro rezervaci letenek – jakmile je sedadlo koupeno, nikdo jiný ho nesmí vidět jako volné._
* **Eventuální konzistence (Eventual Consistency):** Systém nezaručuje okamžitou shodu, ale slibuje, že pokud přestanou zápisy, všechny uzly budou časem mít stejná data. 
  _např.: Počet „to se mi líbí“ u virálního videa – každý uživatel může vidět mírně jiné číslo, ale po čase se hodnota ustálí._
* **Konzistence čtení vlastních zápisů (Read-your-writes):** Zaručuje, že uživatel po odeslání dat uvidí své změny okamžitě, i když ostatní uživatelé je ještě nevidí. 
  _např.: Napíšete komentář pod fotku – vy ho vidíte hned, ale vašemu kamarádovi se objeví až za pár sekund._
* **Quorum:** Mechanismus pro dosažení shody. Pro úspěšnou operaci je potřeba potvrzení od určitého počtu uzlů ($N/2 + 1$). 
  _např.: Aby byl zápis v Cassandře považován za platný, musí ho potvrdit alespoň 2 ze 3 replik._
* **Kauzální konzistence (Causal Consistency):** Zaručuje, že pokud operace B závisí na operaci A, každý uživatel uvidí nejdříve A a až potom B. 
  _např.: V diskusním fóru nemůžete vidět odpověď na komentář, aniž byste viděli ten původní komentář, na který reaguje._
* **Podmíněná / Laditelná konzistence (Tunable Consistency):** Možnost pro vývojáře nastavit úroveň přísnosti pro každou operaci (čtení/zápis) zvlášť podle potřeby (rychlost vs. jistota). 
  _např.: V Cassandře nastavíte `QUORUM` pro platbu (chcete shodu většiny), ale pro zobrazení jména uživatele stačí `ONE` (chcete okamžitou odezvu)._

### Distribuované transakce
Operace, které musí proběhnout atomicky (vše nebo nic) na více síťových uzlech současně.
* **Problém:** Riziko částečného selhání (jeden uzel zapíše, druhý spadne).
* **Řešení:** Koordinační protokoly jako **2PC**.

### Two-Phase Commit (2PC)
Klasický protokol pro zajištění atomicity v distribuovaném prostředí.
1. **Fáze hlasování (Prepare):** Koordinátor se zeptá všech uzlů, zda mohou provést zápis. Uzly data uzamknou a odpoví Ano/Ne.
2. **Fáze potvrzení (Commit):** Pokud všichni řekli Ano, koordinátor pošle příkaz k zápisu. Pokud někdo řekl Ne, všichni provedou Rollback.
* **Nevýhoda:** Vysoká latence a riziko zablokování celého systému, pokud koordinátor uprostřed procesu selže.

---
## Distribuce dat
Způsob, jakým jsou data fyzicky rozmístěna napříč infrastrukturou, aby se dosáhlo škálovatelnosti a odolnosti.
* Latence a geodistribuce

### Replikace (Replication)
Ukládání stejných dat na více fyzických uzlů.
* **Master-Slave (Primary-Secondary):** Zápisy jdou pouze do jednoho uzlu (Master), který je rozesílá ostatním (Slaves). Slouží k posílení výkonu čtení. 
  _např.: Čtení článků na zpravodajském webu – čtete z kopie, zatímco redaktor píše do hlavní databáze._
* **Multi-Master:** Zápis je možný na jakýkoliv uzel. Vyžaduje složité mechanismy pro řešení konfliktů. 
  _např.: Kolaborativní editory typu Google Docs – změny přicházejí od mnoha uživatelů naráz._

### Sharding (Partitioning)
Rozdělení datové sady na menší části (shardy), kde každý uzel nese jen část celkových dat.
* **Distribuční klíč (Shard Key):** Atribut, podle kterého se určuje, na který uzel data patří. 
  _např.: Rozdělení uživatelů podle počátečního písmene příjmení (A–M na serveru 1, N–Z na serveru 2)._
* **Výhoda:** Umožňuje databázi růst za hranice kapacity jednoho disku (horizontální škálování).

### Geodistribuce
Rozmístění uzlů databáze do různých geografických lokalit (regionů).
* **Účel:** Snížení latence (přiblížení dat uživateli) a odolnost proti výpadku celého datacentra.
* **Data Sovereignty:** Nutnost uchovávat data v určitém regionu kvůli legislativě. _např. GDPR vyžaduje uložení dat občanů EU v rámci Unie._

---
## Úložiště párů klíč-hodnota
* Princip (Key-Value pairs)
* Příklady: Redis, Riak, DynamoDB

---
## Dokumentové databáze
* Formáty (JSON, BSON, XML)
* Indexování vnořených polí
* Příklady: MongoDB, CouchDB

---
## Grafové databáze
* Uzly, hrany a vlastnosti (Property Graphs)
* Algoritmy pro procházení grafů
* Příklady: Neo4j, Amazon Neptune

---
## Sloupcově orientované databáze
* Column families a sparse storage
* Optimalizace pro analytické dotazy
* Příklady: Apache Cassandra, HBase, Google Bigtable