# Infrastrukturní a cloudové systémy

> Data-center architecture; supercomputers vs. heterogeneous clusters; orchestration architectures (PBS/grids, Kubernetes, OpenStack); 
> GPU vs. CPU computing and workloads that benefit from massive parallelism; scalable application models; 
> storage tiers, technologies, data temperature and movement; infrastructure resilience and reliability; 
> automation, DevOps/GitOps and SRE; workflow managers and workload portability; identity, SSO, and AAI. (PA234) (povinné pro studium dle kontrolní šablony 2024/2025 nebo novější)

## Architektura datových center (Data-center architecture)

Moderní datové centrum (DC) je navrženo pro maximální hustotu výkonu, efektivitu chlazení a propustnost sítě.

### IT Vybavení (IT Equipment)
*  Superpočítače / Výpočetní clustery / Servery 
* Úložiště dat (Storage Devices)
* Síťové prvky (Networking Equipment)
* Virtualizační infrastruktura
* Napájecí jednotky (PDU)
* Záložní zdroje (UPS)
* Chladicí systémy
* Zabezpečení (Security Systems)
* Nástroje pro správu a monitoring


### Zázemí budovy (Facilities Infrastructure)
* Energetická infrastruktura (Power Infrastructure)
* HVAC (Vytápění, ventilace a klimatizace)
* Design a rozložení datacentra
* Protipožární ochrana (Fire Suppression)
* Fyzická bezpečnost
* Redundance a záložní systémy
* Správa kabeláže (Cable Management)
* Environmentální monitoring


* **Efektivita (PUE):** Klíčovou metrikou je **PUE (Power Usage Effectiveness)**. Vyjadřuje poměr celkové energie spotřebované DC k energii spotřebované IT vybavením. Ideál je 1.0. Moderní DC (Google, Azure) dosahují ~1.1; běžná DC kolem 1.5–2.0.
* **Síťová topologie (Spine-Leaf):**
    * Na rozdíl od tradiční hierarchické struktury (Core-Agg-Access) využívá cloudová éra **Spine-Leaf (Clos)** architekturu. 
    * Každý **Leaf** switch (v racku) je připojen ke každému **Spine** switchi. To zajišťuje konstantní latenci (vždy max. 2 hopy) a vysokou propustnost pro **East-West traffic** (komunikace mezi servery v DC), který dnes dominuje nad komunikací ven (North-South).
* **Disagregace:** Trend oddělování CPU, RAM a úložiště do samostatných fondů propojených ultra-rychlou sítí, což umožňuje efektivnější využití zdrojů.
  
<img alt="img.png" src="img/spine-leaf-arch.png" width="400"/>

---

## Superpočítače vs. heterogenní clustery (Supercomputers vs. heterogeneous clusters)
* **Superpočítače (HPC - High Performance Computing):**
    * Jednotlivé, vysoce specializované systémy určené pro maximální výkon
    * Navrženy pro **těsně vázané (tightly-coupled)** úlohy *např. simulace počasí, dynamika kapalin, virtuální crash testy, vývoj léků,...*
    * Využívají **MPI (Message Passing Interface)** pro neustálou komunikaci mezi tisíci jádry.
    * Klíčová je **nízká latence sítě** (technologie jako InfiniBand nebo HPE Slingshot).
    * *např.: El Capitan (USA), Karolina v Ostravě*

* **Heterogenní clustery (HTC - High Throughput Computing):**
    * Soubory standardních počítačů spolupracujících za účelem dosažení vysokého výkonu prostřednictvím paralelního zpracování
    * Skládají se z různorodého HW (různé generace CPU, různé GPU).
    * Vhodné pro **volně vázané (loosely-coupled)** nebo „embarrassingly parallel“ úlohy, které lze rozdělit na nezávislé části *např.: zpracování obrazu, Monte Carlo simulace, trénování LLM, renderování animovaných filmů, analýza velkých dat*
    * *např.: Infrastruktura e-INFRA CZ (MetaCentrum, CERIT-SC).*

* Pozor, ani superpočítače, ani výpočetní clustery nemohou výrazně zrychlit vaši práci, pokud nejsou implementovány pro paralelní/distribuované zpracování.
---

## Orchestrační architektury

Orchestrace řeší "plánování" (scheduling) – kdy a kde se co spustí. V moderních datových centrech a superpočítačových střediscích se nepoužívá jen jeden přístup. 
Výběr závisí na tom, zda potřebujete spočítat složitou simulaci (HPC), provozovat webovou aplikaci (Cloud) nebo spravovat celou infrastrukturu (IaaS).

* **PBS (Portable Batch System) / Slurm:** * **Batch processing:** Uživatel pošle úlohu do fronty. Plánovač ji spustí, až jsou zdroje volné.
* **OpenStack (IaaS):** * Správa virtuálních strojů (VM). Uživatel má "on-demand" přístup a root práva k OS.
* **Kubernetes (K8s / CaaS):** Orchestrace kontejnerů. **Deklarativní přístup**: definujete cílový stav (např. "chci 5 instancí webu"), K8s zajistí jeho udržení (*Self-healing*).



<img alt="img.png" src="img/job-lifecycle.png" width="500"/>

---

## PBS (Portable Batch System) / Slurm
Standard pro HPC (High Performance Computing) a superpočítače.

* **Batch processing (Dávkové zpracování):** Uživatel neinteraguje se strojem přímo v reálném čase. Pošle úlohu (skript) do fronty a specifikuje požadavky (např. 16 uzlů, 4 GPU, 24 hodin). Plánovač (Scheduler) ji spustí, až jsou zdroje volné.
* **Klíčové mechanismy:**
    * **Fairshare:** Prioritizační algoritmus. Pokud uživatel v poslední době vyčerpal hodně prostředků, jeho priorita klesá, aby systém zůstal spravedlivý pro ostatní.
    * **Backfilling:** Strategie pro maximalizaci využití. Pokud velká úloha čeká na uvolnění zdrojů, plánovač do vzniklé časové mezery "vsune" menší, krátké úlohy, které stihnou doběhnout dříve, než začne hlavní výpočet.
* **Využití:** Simulace počasí, CFD (dynamika kapalin), vývoj léků, trénování obřích AI modelů.
* **Příklady:** Slurm (na Karolině v Ostravě), PBS Professional, Torque.
  <img alt="img.png" src="img/job-schedule.png" width="1000"/>

---

## OpenStack 
Platforma pro budování privátních i veřejných cloudů a správa virtualizace. IaaS - Infrastructure as a Service.

* **Správa virtuálních strojů (VM):** Umožňuje rozdělit fyzický hardware na mnoho nezávislých virtuálních serverů. Uživatel má k OS "on-demand" přístup a plná **root práva**.
* **Modulární komponenty:**
    * **Nova:** Hlavní "compute" modul, který vytváří a spravuje životní cyklus virtuálních strojů.
    * **Neutron:** Zajišťuje síťovou konektivitu (L2/L3 vrstvy, IP adresy, firewally).
    * **Keystone:** Modul pro identitu, autentizaci uživatelů a správu oprávnění.
    * **Cinder:** Poskytuje blokové úložiště (virtuální pevné disky), které lze za běhu připojovat k VM.
* **Využití:** Podnikové IT systémy, vývojová prostředí, hosting webových služeb.
  <img alt="img.png" src="img/openstack-architecture.png" width="600"/>

---

## Kubernetes 
Nástroj pro orchestraci kontejnerů, zaměřený na automatizaci a škálování. CaaS - Container as a Service.

* **Orchestrace kontejnerů:** Spravuje aplikace zabalené v lehkých kontejnerech (např. Docker). Na rozdíl od VM kontejnery sdílejí jádro OS, což je činí extrémně rychlými na spuštění.
* **Deklarativní přístup:** Uživatel definuje v konfiguračním souboru (YAML) cílový stav (např. "vždy chci mít 10 aktivních instancí této služby"). Kubernetes se postará o jeho dosažení a udržení.
* **Klíčové funkce:**
    * **Self-healing (Samohojení):** Pokud kontejner spadne nebo uzel (node) selže, K8s automaticky restartuje pody na jiném funkčním hardwaru.
    * **Horizontální škálování:** Automaticky přidává nebo ubírá instance aplikace podle aktuálního vytížení CPU/RAM.
    * **Service Discovery:** Automaticky propojuje pody a vyrovnává zátěž (Load Balancing) mezi nimi.
* **Využití:** Microservices, moderní webové aplikace (Netflix, Spotify), CI/CD potrubí.
* Pojmy: 
  * Pod (nejmenší jednotka v K8s, obsahuje jeden nebo více kontejnerů), 
  * Node (fyzický nebo virtuální stroj, na kterém běží pody), 
  * Control Plane (řídící věž celého clusteru).


---

## GPU vs. CPU výpočty a massive parallelism workloads

* **CPU:** Optimalizováno na **latenci**. Má komplexní jádra, velkou cache a sofistikované predikce skoků. Vhodné pro sériovou logiku.
* **GPU:** Optimalizováno na **propustnost (throughput)**. Má tisíce jednoduchých jader.
    * **SIMT (Single Instruction, Multiple Threads):** Skupina vláken (warp) provádí stejnou instrukci nad různými daty.
* **MIG (Multi-Instance GPU):** Umožňuje rozdělit jednu výkonnou GPU (např. Nvidia A100) na několik izolovaných hardwarových instancí, což řeší problém s nevyužitým výkonem u menších úloh.
* **Využití:** Trénování AI, kryptografie, simulace částic.

![img.png](img.png)

---

## Škálovatelné aplikační modely (Scalable application models)

* **Microservices:** Rozbití monolitu na malé služby komunikující přes API (gRPC, REST). Umožňuje nezávislé škálování částí systému.
* **Bezstavovost (Statelessness):** Aplikace neukládá stav (session) lokálně. Stav je v externí DB nebo v tokenu (JWT). Klíčové pro horizontální škálování (uzly lze kdykoliv přidat/odebrat).
* **12-factor App:** Metodika pro cloud-native aplikace (např. explicitní deklarace závislostí, konfigurace v proměnných prostředí, brát procesy jako bezstavové).

---

## Úrovně úložišť, technologie, teplota dat a přesun (Storage tiers, technologies, data temperature and movement)

* **Data Temperature:**
    * **Hot:** Často přístupná (NVMe SSD). Extrémní rychlost, vysoká cena.
    * **Warm:** Méně častý přístup (SATA SSD/HDD). Balanc mezi cenou a výkonem.
    * **Cold (Archive):** Téměř se nečtou (Páskové knihovny LTO). Velmi levné, latence přístupu v minutách/hodinách.
* **ILM (Information Lifecycle Management):** Automatizovaný přesun dat mezi úrovněmi na základě pravidel (např. "pokud na soubor nikdo nesáhl 6 měsíců, přesuň ho na pásku").
* **Erasure Coding (EC):** Moderní alternativa k RAID. Data jsou rozdělena na $k$ bloků a doplněna o $m$ paritních bloků. Efektivnější využití kapacity při zachování vysoké odolnosti.

---

## Odolnost a spolehlivost infrastruktury (Infrastructure resilience and reliability)

* **Redundance:** N+1 (jeden náhradní prvek), 2N (plná kopie).
* **High Availability (HA):** Schopnost systému zůstat dostupný i při výpadku části HW.
* **Disaster Recovery (DR):** Obnova po totálním selhání (např. vyhoření DC).
    * **RPO (Recovery Point Objective):** Maximální přípustná ztráta dat (čas od poslední zálohy).
    * **RTO (Recovery Time Objective):** Maximální čas, za který musí být systém opět online.

---

## Automatizace, DevOps/GitOps a SRE

* **DevOps:** Kultura spojující vývoj a provoz. Cílem je **CI/CD** (Continuous Integration/Deployment).
* **GitOps:** Stav infrastruktury je popsán kódem v Gitu (**Single Source of Truth**). Nástroje jako *Flux* nebo *ArgoCD* automaticky synchronizují realitu v cloudu s kódem v Gitu.
* **SRE (Site Reliability Engineering):** Inženýrský přístup k provozu.
    * **SLI** (Indicator), **SLO** (Objective - např. 99.9% uptime).
    * **Error Budget:** Prostor pro chyby (0.1%). Pokud je vyčerpán, zastavuje se vývoj nových funkcí a řeší se pouze stabilita.

---

## Workflow manažeři a přenositelnost úloh (Workflow managers and workload portability)

* **Workflow manažeři (Snakemake, Nextflow):** Automatizují komplexní výpočetní řetězce (DAG - Directed Acyclic Graph). Zajišťují, že pokud krok 5 selže, nemusí se po opravě spouštět kroky 1–4 znovu.
* **Kontejnery pro HPC:** Zatímco v cloudu vládne *Docker*, v HPC se používá **Apptainer (Singularity)**.
    * *Důvod:* Singularity nevyžaduje root práva, neřeší síťovou izolaci (latence!) a snadno mapuje souborový systém hostitele.
* **Portability:** Možnost spustit stejný vědecký výpočet kdekoli (notebook -> cloud -> superpočítač).

---

## Identita, SSO a AAI (Identity, SSO, and AAI)

* **AAI (Authentication and Authorisation Infrastructure):**
    * *AuthN (Kdo jsi?):* Ověření identity (heslo, MFA).
    * *AuthZ (Co smíš?):* Oprávnění k přístupu ke zdrojům.
* **SSO (Single Sign-On):** Uživatel se přihlásí jednou a získá přístup k mnoha nezávislým systémům.
* **Federace:** Spojení identit různých institucí (např. **eduGAIN**). Umožňuje studentovi MUNI přihlásit se ke službám univerzity v Oxfordu pomocí jeho MUNI hesel.
* **Protokoly:**
    * **SAML:** Starší, XML, robustní, používaný ve federacích.
    * **OpenID Connect (OIDC):** Moderní, postavený nad **OAuth2**, využívá JSON a JWT tokeny. Populární v cloudu.
