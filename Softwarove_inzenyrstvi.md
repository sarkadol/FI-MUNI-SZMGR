# Softwarové inženýrství

> Proces vývoje SW. 
> Metodika Rational Unified Process. 
> Agilní vývoj SW. 
> Fáze testování a typy testů. 
> Softwarové metriky, refaktoring kódu. 
> Kvalita softwaru. 
> Odhadování nákladů a času vývoje SW. 
> Údržba a znovupoužitelnost.

## Proces vývoje SW
Proces vývoje softwaru je systematický, inženýrský a kontrolovaný soubor činností, jejichž cílem je transformovat netriviální uživatelské a byznys požadavky v plně funkční, verifikovaný, bezpečný a dlouhodobě udržovatelný softwarový produkt při dodržení časových a finančních limitů.

### Základní fáze softwarového cyklu (SDLC – Software Development Life Cycle)
* **Analýza a specifikace požadavků:** Proces zjišťování, vyjednávání, dokumentace a validace potřeb zúčastněných stran (stakeholders). Výstupem je formální dokument **SRS (Software Requirements Specification)**.
    * *Funkční požadavky:* Definují chování systému, transformaci vstupů na výstupy a konkrétní byznys logiku.
    * *Nefunkcionální požadavky:* Definují systémové kvality a provozní limity (výkon, propustnost, latence, škálovatelnost, dostupnost, bezpečnost, legislativní shoda).
* **Návrh (Architecture & Design):** Dekompozice systému do subsystémů a komponent. Zahrnuje návrh softwarové architektury (např. mikroslužby, vrstvená architektura), datových schémat, komunikačních rozhraní (REST, gRPC) a komponentních diagramů.
* **Implementace (Kódování):** Syntéza algoritmů a datových struktur do formy spustitelného zdrojového kódu v souladu s architektonickými standardy a konvencemi.
* **Verifikace a validace (V&V / Testování):** * *Verifikace:* „Stavíme produkt správně?“ – Kontrola shody se specifikací a designem.
    * *Validace:* „Stavíme správný produkt?“ – Ověření, zda produkt plní reálné potřeby uživatele.
* **Nasazení (Deployment):** Proces transferu otestovaného softwaru do cílového produkčního prostředí. Zahrnuje orchestraci infrastruktury (Infrastructure as Code – IaC), datové migrace a nastavení monitoringu.
* **Provoz a údržba:** Kontinuální správa softwaru po nasazení, odstraňování skrytých defektů, adaptace na změny okolního prostředí a optimalizace výkonu.

### Základní procesní modely
* **Vodopádový model (Waterfall):** Rigorózní lineární model, kde každá fáze začíná až po formálním schválení a zmrazení výstupů fáze předchozí. 
    * *Vlastnosti:* Vysoká míra determinismu, rigidita, nízká schopnost reagovat na změny v průběhu vývoje. Zákazník vidí funkční software až na samotném konci cyklu. Vhodný pro projekty s kritickou bezpečností a fixním zadáním.
* **Inkrementální a iterační model:** * *Iterace:* Opakování celého cyklu SDLC v menších časových oknech s cílem postupně zpřesňovat a vylepšovat systém.
    * *Inkrement:* Postupné přidávání hotových, funkčních částí (přírůstků) k systému. Snižuje riziko selhání průběžným ověřováním funkčnosti.
* **Spirálový model (Spiral - Boehm):** Iterační model postavený na **systematické analýze rizik** v každé obrátce spirály. Každý cyklus zahrnuje určení cílů, vyhodnocení rizik, vývoj/testování a plánování další iterace. Vhodný pro rozsáhlé, komplexní a technologicky unikátní systémy.

---

## Metodika Rational Unified Process
Rational Unified Process (RUP) je robustní, těžká (heavyweight), objektově orientovaná softwarová metodika založená na vizuálním modelování pomocí jazyka **UML (Unified Modeling Language)**, architektuře řízené případy užití (Use-Case Driven) a iteračním přístupu.

### 4 klíčové fáze životního cyklu RUP
Životní cyklus je rozdělen horizontálně v čase do čtyř po sobě jdoucích fází, z nichž každá může obsahovat několik iterací:
1.  **Inception (Zahájení):** Definice byznys záměru (Business Case) a rozsahu projektu. Identifikují se klíčoví herci (Actors) a případy užití (Use Cases) na makroúrovni. Určují se základní ekonomické a časové mantinely a provádí se primární analýza rizik.
2.  **Elaboration (Projektování / Elaborace):** Klíčová fáze zaměřená na **mitigaci technologických rizik** a validaci architektury. Vytváří se spustitelný architektonický prototyp (Executable Architecture Baseline) a detailně se specifikuje většina požadavků. Architektura se na konci této fáze fixuje.
3.  **Construction (Realizace / Konstrukce):** Masivní vývojový proces, kde se implementují zbývající Use Cases do zdrojového kódu na bázi stabilní architektury. Dochází k integraci komponent a intenzivnímu testování. Výstupem je kompletní systém připravený k nasazení (Beta verze).
4.  **Transition (Předání / Transice):** Transfer softwaru do produkčního prostředí ke koncovým uživatelům. Zahrnuje beta testování, opravy defektů z reálného provozu, migraci databází, optimalizaci výkonu a školení personálu.

### Základní disciplíny (Workflows)
RUP rozděluje činnosti logicky do 9 základních disciplín (procesních procesů), které probíhají vertikálně napříč všemi fázemi, ale s různou intenzitou v čase:

| Disciplína | Popis a intenzita v čase |
| :--- | :--- |
| **Business Modeling** | Pochopení procesů organizace, mapování byznys entit. Dominantní v zahájení. |
| **Requirements** | Sběr a formalizace Use Cases a softwarových požadavků. Vrcholí v elaboraci. |
| **Analysis & Design** | Transformace požadavků do architektonického návrhu (UML diagramy tříd, sekvencí). |
| **Implementation** | Kódování, unit testování a kompilace komponent do spustitelných celků. Maximální v konstrukci. |
| **Test** | Verifikace chování systému, integrační, systémové a regresní testy. Roste s každou iterací. |
| **Deployment** | Balení softwaru, konfigurace releasů a nasazení. Dominantní v transici. |
| **Configuration & Change Mgmt** | Verzování kódu (Git), správa buildů, trasovatelnost změn a správa artefaktů. Spojitá po celou dobu. |
| **Project Management** | Plánování iterací, řízení rizik, alokace zdrojů a metrické vyhodnocování. Spojitá po celou dobu. |
| **Environment** | Správa vývojových nástrojů, IDE, hardwarové infrastruktury a konfigurace CASE nástrojů. |

---

## Agilní vývoj SW
Agilní vývoj je empirický přístup k řízení projektů postavený na flexibilitě, adaptabilitě, minimalizaci procesní byrokracie, průběžné dodávce funkčního softwaru (Continuous Delivery) a těsné zpětnovazební smyčce se zákazníkem.

### Agilní manifest (4 základní hodnoty)
Formulován v roce 2001, definuje priority agilního myšlení (přičemž položky vpravo mají hodnotu, ale položky vlevo mají hodnotu vyšší):
1.  **Jednotlivci a interakce** mají přednost před procesy a nástroji.
2.  **Fungující software** má přednost před obsáhlou dokumentací.
3.  **Spolupráce se zákazníkem** má přednost před vyjednáváním o smlouvách.
4.  **Reagování na změnu** má přednost před dodržováním plánu.

### Klíčové agilní metodiky

#### Scrum
Strukturovaný projektový rámec založený na pevných časových oknech zvaných **Sprinty** (typicky 1–4 týdny), během kterých je vytvářen potenciálně nasaditelný inkrement produktu.
* **Role:**
    * *Product Owner:* Reprezentuje byznys a zákazníka. Spravuje a prioritizuje Product Backlog, definuje akceptační kritéria.
    * *Scrum Master:* Fasilitátor procesu. Odstraňuje překážky (impediments), chrání tým před vnějšími vlivy a zajišťuje dodržování Scrum pravidel.
    * *Vývojový tým (Developers):* Cross-funkční, samoorganizující se tým plně odpovědný za technickou realizaci a kvalitu inkrementu.
* **Artefakty:**
    * *Product Backlog:* Dynamický, prioritizovaný seznam všech požadovaných funkcí, chyb a technických vylepšení.
    * *Sprint Backlog:* Podmnožina Product Backlogu vybraná pro aktuální Sprint, dekomponovaná do konkrétních technických úkolů.
    * *Inkrement:* Plně funkční, otestovaný a nasaditelný kus kódu splňující firemní definici hotového (**DoD – Definition of Done**).
* **Události (Ceremonie):**
    * *Sprint Planning:* Plánování cílů a obsahu nadcházejícího Sprintu.
    * *Daily Scrum:* Denní 15minutový stand-up zaměřený na synchronizaci práce a identifikaci blokád.
    * *Sprint Review:* Demonstrace hotového inkrementu zainteresovaným stranám na konci Sprintu, sběr zpětné vazby.
    * *Sprint Retrospective:* Interní analýza procesů, nástrojů a týmové spolupráce s cílem kontinuálního zlepšování (Kaizen).

#### Kanban
Metoda řízení toku práce (flow) založená na vizuální tabuli a evolučním přístupu. Na rozdíl od Scrumu nemá pevně dané iterace ani specifické role.
* **WIP Limit (Work In Progress Limit):** Striktní omezení maximálního počtu úkolů, které se mohou simultánně nacházet v daném stavu (např. max 3 úkoly ve fázi "In Testing"). Tím se předchází vzniku úzkých hrdel (bottlenecks), maximalizuje se propustnost a vynucuje se dokončování rozdělané práce (*Stop starting, start finishing*).

#### Extrémní programování (XP)
Agilní metodika orientovaná na vysokou kvalitu inženýrských praktik a technickou excelenci vývojářů.
* **Klíčové praktiky:**
    * *TDD (Test-Driven Development):* Vývoj řízený testy v cyklu *Red-Green-Refactor* (nejprve se napíše selhávající unit test, pak minimální kód pro jeho splnění, následně se kód refaktoruje).
    * *Párové programování (Pair Programming):* Dva vývojáři pracují na jedné pracovní stanici. Jeden píše kód (Driver), druhý reviduje strategii, syntaktickou čistotu a architekturu v reálném čase (Navigator).
    * *Kontinuální integrace (CI):* Vývojáři integrují kód do hlavní větve několikrát denně, přičemž každý commit spouští automatizované buildy a testy.
    * *Společné vlastnictví kódu (Collective Code Ownership):* Kdokoliv z týmu může modifikovat jakoukoliv část codebase, čímž se eliminuje závislost na jednotlivcích (bus factor).

---

## Fáze testování a typy testů
Testování je systematický proces exekuce softwaru pod kontrolovanými podmínkami s cílem verifikovat shodu se specifikací, detekovat funkční i nefunkční defekty a evaluovat celkové riziko nasazení.

### Fáze (úrovně) testování – podle granularity
...
* **Jednotkové testy (Unit Testing):** Izolované testování nejmenších programových celků (metod, tříd, funkcí).
    * *Charakteristika:* Prováděno vývojáři. Veškeré externí závislosti (databáze, sítě, API) jsou striktně izolovány pomocí testovacích záskoků (**Mocks, Stubs, Fakes**). Cílem je stoprocentní determinismus a rychlost exekuce.
* **Integrační testy (Integration Testing):** Ověřování správnosti interakcí a datových toků mezi integrovanými moduly nebo externími subsystémy.
    * *Strategie:* Top-Down (od nejvyšších modulů dolů pomocí stubů), Bottom-Up (od nejnižších nahoru pomocí driverů), nebo Big-Bang (vše naráz – silně nedoporučováno).
* **Systémové testy (System Testing):** Validace kompletně integrovaného systému jako end-to-end celku v prostředí, které je izomorfní s produkčním. Ověřují se jak funkční scénáře, tak nefunkční kvality (bezpečnost, výkon).
* **Akceptační testy (Acceptance Testing):** Finální fáze validace systému prováděná zákazníkem nebo koncovými uživateli s cílem potvrdit splnění kontraktu a připravenost k provozu (UAT – User Acceptance Testing).
    * *Alpha testování:* Prováděno interními uživateli v simulovaném prostředí vývojáře.
    * *Beta testování:* Prováděno reálnými koncovými uživateli v reálném produkčním prostředí v rámci pilotního provozu.

### Typy testů – podle přístupu k vnitřní struktuře
* **Black-box testování:** Funkcionální nebo nefunkcionální testování bez jakékoli znalosti vnitřní implementace, zdrojového kódu či architektury. Vstupy jsou generovány striktně na základě specifikace požadavků a porovnávají se s očekávanými výstupy.
    * *Techniky:* Analýza hraničních hodnot (Boundary Value Analysis), ekvivalentní třídění (Equivalence Partitioning).
* **White-box testování (Strukturní):** Testování založené na přímé znalosti a analýze zdrojového kódu a programových struktur. Sleduje se pokrytí kódu (**Code Coverage**).
    * *Metriky pokrytí:* Pokrytí příkazů (Statement Coverage), pokrytí větví (Branch/Decision Coverage), pokrytí podmínek (Condition Coverage).
* **Grey-box testování:** Kombinovaný přístup. Tester má částečnou znalost vnitřních struktur (např. zná databázové schéma, stavový model nebo API rozhraní), ale k samotnému testování přistupuje zvenčí přes rozhraní systému.

### Typy testů – podle účelu a charakteristiky
* **Funkcionální testy:** Verifikují specifické chování systému, byznys pravidla a funkční požadavky (např. „Lze provést platbu kartou?“).
* **Nefunkcionální testy:** Měří a hodnotí systémové kvality a limity:
    * *Výkonnostní testy (Performance Testing):* Evaluace doby odezvy a stability systému při simulované standardní zátěži.
    * *Zátěžové testy (Load Testing):* Testování chování systému při dlouhodobém dosahování maximální očekávané zátěže (peak load).
    * *Stresové testy (Stress Testing):* Záměrné přetížení systému nad jeho nominální limity s cílem identifikovat bod selhání (breaking point) a ověřit schopnost korektního zotavení (Graceful Degradation).
    * *Bezpečnostní testy (Security Testing):* Identifikace zranitelností (např. OWASP Top 10), ověření šifrování, autorizačních matric a odolnosti proti průniku.
* **Regresní testy:** Opakované spouštění sady testů (ideálně plně automatizovaných) po jakékoli modifikaci codebase (oprava chyby, refaktoring, nový feature). 
    * *Účel:* Garantovat, že provedené změny nezpůsobily regresi – tj. nerozbily dosud stabilní a správně fungující části systému.

---

## Softwarové metriky, refaktoring kódu

### Softwarové metriky
Kvantitativní nástroje pro objektivní měření, hodnocení a predikci vlastností softwarového produktu nebo procesu jeho vývoje.

#### Produktové metriky (Statická analýza kódu)
* **LOC (Lines of Code):** Hrubá metrika velikosti kódu. Rozlišuje se *Physical LOC* (čisté řádky) a *Logical LOC* (počet příkazů). Má nízkou vypovídající hodnotu o kvalitě a složitosti, je silně závislá na syntaxi jazyka.
* **Cykloatická komplexita (McCabeova):** Kvantifikuje strukturální složitost kódu na základě počtu lineárně nezávislých cest v grafu řízení toku programu (Control Flow Graph).
    * *Vzorec:* $M = E - V + 2P$, kde $E$ je počet hran grafu, $V$ je počet uzlů a $P$ je počet komponent (pro jednu metodu $P=1$).
    * *Význam:* Vyšší hodnota (typicky $> 10$) indikuje špatnou srozumitelnost, vysokou náchylnost k chybám a extrémní náročnost na unit testování (je potřeba napsat minimálně $M$ testů pro pokrytí všech větví).
* **Soudržnost (Cohesion):** Míra, do jaké prvky uvnitř jednoho modulu či třídy spolupracují na realizaci jediné, jasně definované odpovědnosti (v souladu se Single Responsibility Principle). Cílem je **vysoká soudržnost (High Cohesion)**.
* **Vázanost (Coupling):** Míra vzájemné provázanosti a mezikomponentových závislostí. Vysoká vázanost znamená, že změna v jednom modulu vynucuje kaskádové změny v modulech ostatních. Cílem je **nízká vázanost (Loose Coupling)**.
* **Code Coverage (Pokrytí kódu):** Procentuální vyjádření částí kódu exekvovaných sadou unit testů. Vysoké pokrytí (např. 80 %+) zvyšuje důvěru v kód, ale samo o sobě negarantuje kvalitu samotných testovacích asercí.

### Refaktoring kódu
Systematický proces modifikace vnitřní struktury zdrojového kódu bez jakékoli změny jeho vnějšího chování, funkčnosti či API.
* **Hlavní cíle:** Redukce technického dluhu (Technical Debt), zvýšení čitelnosti (Readability), udržovatelnosti (Maintainability), zjednodušení architektury a usnadnění lokalizace budoucích defektů.
* **Základní postulát:** Refaktoring lze bezpečně provádět **pouze nad kódem, který je pokryt spolehlivými, rychlými automatizovanými unit testy**. Po každé mikro-změně jsou testy spuštěny pro okamžitou detekzi nechtěné změny chování.
* **Code Smells (Zápachy kódu):** Symptomy v codebase indikující nutnost refaktoringu:
    * *Duplicitní kód (Duplicated Code):* Stejná struktura kódu na více místech (porušení principu DRY – Don't Repeat Yourself).
    * *Dlouhá metoda (Long Method):* Metoda realizuje příliš mnoho operací, je nečitelná a má vysokou cykloatickou komplexitu.
    * *Velká třída (Large Class):* Třída má příliš mnoho polí a metod, porušuje Single Responsibility Principle (tzv. God Object).
    * *Závistivý feature (Feature Envy):* Metoda jedné třídy excesivně přistupuje k datům a metodám jiné třídy. Řešením je přesun metody tam, kde jsou data doma.
* **Typické techniky refaktoringu:**
    * *Extract Method:* Vyčlenění logického bloku z dlouhé metody do samostatné metody s výstižným názvem.
    * *Rename Variable/Method:* Změna kryptických názvů za samovysvětlující (Self-Documenting Code).
    * *Introduce Parameter Object:* Sdružení vysokého počtu parametrů metody do samostatné datové struktury / objektu.
    * *Replace Conditional with Polymorphism:* Nahrazení komplexních konstrukcí `switch-case` nebo `if-else` provádějících typovou kontrolu čistým polymorfním voláním nad hierarchií objektů.

---

## Kvalita softwaru
Kvalita softwaru je komplexní souhrn vlastností a charakteristik softwarového produktu, které určují jeho schopnost plnit specifikované i implicitně předpokládané potřeby uživatelů a byznysu.

### Standard ISO/IEC 25010 (System and Software Quality Models)
Definuje mezinárodní standard pro hodnocení kvality softwarových produktů rozdělením do 8 základních charakteristik:


1.  **Funkční vhodnost (Functional Suitability):** Míra, do jaké funkce softwaru pokrývají reálné potřeby. Dělí se na funkční úplnost, funkční správnost (korektnost výpočtů) a funkční přiměřenost.
2.  **Výkonová efektivita (Performance Efficiency):** Výkonový stav systému vzhledem k množství spotřebovaných hardwarových zdrojů za definovaných podmínek. Sleduje se doba odezvy (Response Time), propustnost (Throughput) a limity využití zdrojů (CPU, RAM, diskové I/O).
3.  **Kompatibilita (Compatibility):** Schopnost systému efektivně si vyměňovat informace s jinými systémy (Interoperabilita) a sdílet stejné hardwarové či softwarové prostředí bez negativních interferencí (Koexistence).
4.  **Použitelnost (Usability):** Snadnost pochopení, naučení, ovládání a celková atraktivita uživatelského rozhraní pro koncového uživatele (UX/UI, přístupnost – Accessibility).
5.  **Spolehlivost (Reliability):** Schopnost systému plnit své funkce v čase za definovaných podmínek. Měří se pomocí:
    * *MTBF (Mean Time Between Failures):* Střední doba mezi poruchami.
    * *MTTR (Mean Time To Repair):* Střední doba opravy systému.
    * *Dostupnost (Availability):* Procentuální vyjádření doby, kdy je systém plně funkční (např. SLA 99.99 %).
    * *Odolnost vůči chybám (Fault Tolerance):* Schopnost zachovat provoz i při částečném selhání komponent.
6.  **Bezpečnost (Security):** Ochrana informací a dat před neautorizovaným přístupem, modifikací či smazáním. Zahrnuje Důvěrnost (Confidentiality), Integritu (Integrity), Neodpopiratelnost (Non-repudiation), Autentizaci a Autorizaci.
7.  **Udržovatelnost (Maintainability):** Efektivita a snadnost, s jakou může být software modifikován vývojáři. Zahrnuje Modularitu, Znovupoužitelnost, Analyzovatelnost, Modifikovatelnost a **Testovatelnost**.
8.  **Přenositelnost (Portability):** Snadnost transformace systému z jednoho hardwarového, softwarového či cloudového prostředí do druhého. Zahrnuje Adaptabilitu, Instalovatelnost a Nahraditelnost.

### Zajištění kvality (QA vs. QC)
* **Quality Assurance (QA - Zajištění kvality):** **Proaktivní a preventivní proces** orientovaný na řízení celého vývojového cyklu a procesů. Cílem je nastavit standardy, metodiky, CI/CD pipelines a revize kódu tak, aby vzniku chyb a defektů v softwaru efektivně předcházel.
* **Quality Control (QC - Řízení kvality):** **Reaktivní proces** zaměřený na samotný produkt. Zahrnuje exekuci testů, detekci konkrétních chyb v hotovém softwaru a verifikaci, že produkt splňuje požadavky před jeho uvolněním do produkce.

---

## Odhadování nákladů a času vývoje SW
Proces predikce lidského úsilí (člověkoměsíce – Person-Months), kalendářního času a celkových finančních nákladů nezbytných pro úspěšnou realizaci softwarového projektu.

### Problémy odhadování a Kužel nejistoty (Cone of Uncertainty)
Odhadování softwarového vývoje je zatíženo extrémní mírou neurčitosti na začátku projektu. **Kužel nejistoty** demonstruje, že ve fázi iniciace (před specifikací požadavků) může být chyba odhadu až čtyřnásobná ($0.25\times$ až $4\times$). S postupující analýzou, fixací architektury a dokončováním iterací se rozptyl odhadu postupně zužuje směrem k reálné hodnotě.

### Základní přístupy k odhadování

#### Expertní odhady (Expert Judgment)
Založeny na intuici a historické zkušenosti seniorních inženýrů, kteří realizovali analogické projekty.
* **Tříbodový odhad (PERT – Program Evaluation and Review Technique):** Zmírňuje subjektivismus experta definováním tří scénářů: Optimistický ($O$), Pesimistický ($P$) a Nejvíc pravděpodobný ($M$).
    * *Výsledné očekávané úsilí ($E$):* $$E = \frac{O + 4M + P}{6}$$
    * *Směrodatná odchylka ($\text{SD}$ - vyjádření rizika):* $$\text{SD} = \frac{P - O}{6}$$

#### Algoritmické (Parametrické) modely
Využívají statistické a matematické vzorce odvozené z analýzy velkého množství historických projektů z minulosti.
* **COCOMO (Constructive Cost Model - Boehm):** Model, kde primární nezávislou proměnnou je velikost softwaru vyjádřená v tisících řádků zdrojového kódu (**KLOC - Thousands of Lines of Code**). Výsledný odhad úsilí je dán nelineární rovnicí:
    $$\text{Effort} = A \cdot (\text{KLOC})^B \cdot \text{EAF}$$
    kde $A$ a $B$ jsou konstanty závislé na typu projektu (organický, semioddělený, vnořený) a $\text{EAF}$ (Effort Adjustment Factor) je multiplikátor zohledňující 15 atributů (schopnosti týmu, složitost platformy, požadavky na spolehlivost).
* **FPA (Function Point Analysis - Analýza funkčních bodů):** Metoda měření velikosti softwaru z pohledu uživatele, která je plně **nezávislá na implementační technologii a programovacím jazyce**. Hodnotí se počet a komplexita pěti typů komponent:
    * *Externí vstupy (Inputs), Externí výstupy (Outputs), Externí dotazy (Queries), Interní logické soubory (Internal Files), Externí rozhraní (Interfaces).*
    * Výsledná hodnota neupravených funkčních bodů (UFP) se koriguje faktorem charakteristiky systému (VAF) na upravené funkční body (AFP), které lze následně pomocí historických koeficientů převést na člověkodny.

#### Agilní relativní odhadování
Ignoruje absolutní časové jednotky (hodiny/dny) a zaměřuje se na čisté porovnání komplexnosti úkolů vůči sobě.
* **Story Points:** Abstraktní jednotka vyjadřující kombinaci velikosti, technické složitosti, závislostí a inherentního rizika uživatelského příběhu (User Story). Pro odhady se striktně používá **upravená Fibonacciho posloupnost** (1, 2, 3, 5, 8, 13, 21, ...). Nelineární růst odráží fakt, že s rostoucí velikostí úkolu lineárně roste jeho neurčitost.
* **Planning Poker:** Týmová, konsenzuální technika. Členové týmu současně vyloží karty s hodnotami Story Points pro daný úkol. Vývojáři s extrémními odhady (nejnižší vs. nejvyšší) vysvětlí svůj pohled, proběhne diskuse o technických úskalích a proces se opakuje, dokud tým nedosáhne konsenzu. Předchází se tím zkreslení odhadu dominantním členem týmu (Anchoring Effect).

---

## Údržba a znovupoužitelnost

### Údržba softwaru (Software Maintenance)
Souhrn všech činností prováděných na softwarovém systému po jeho formálním předání do ostrého produkčního provozu. Náklady na údržbu dlouhodobých systémů standardně dominují celému TCO (Total Cost of Ownership) a tvoří **60 % až 80 % celkového rozpočtu životního cyklu**.

#### 4 typy údržby podle Lehmanova rozdělení
1.  **Korektivní údržba (Corrective):** Reaktivní odstraňování a opravy funkčních defektů, chyb a havárií, které byly detekovány uživateli až v produkčním provozu.
2.  **Adaptivní údržba (Adaptive):** Modifikace softwaru s cílem zajistit jeho plnou funkčnost při změnách vnějšího provozního prostředí. Zahrnuje migrace na nové verze operačních systémů, upgrady databázových platforem, změny hardwarové architektury či adaptaci na nové legislativní a daňové předpisy.
3.  **Perfektivní údržba (Perfective):** Implementace nových funkčních požadavků, vylepšení uživatelského rozhraní (UX) a optimalizace výkonu na základě zpětné vazby z reálného provozu. Je zaměřena na zvyšování užitné hodnoty pro zákazníka.
4.  **Preventivní údržba (Preventive):** Proaktivní modifikace systému zaměřená na **zvýšení udržovatelnosti a čitelnosti kódu do budoucna**. Zahrnuje refaktoring, odstraňování zápachů kódu, restrukturalizaci databázových schémat a aktualizaci technické dokumentace. Cílem je zpomalit softwarové stárnutí (Software Entropy).

### Znovupoužitelnost (Software Reuse)
Strategický inženýrský přístup, při kterém jsou softwarové artefakty (kód, design, architektura, testy) záměrně navrhovány a vyvíjeny tak, aby mohly být bezpečně a efektivně replikovány a použity v různých částech systému nebo v úplně odlišných projektech.

* **Hlavní přínosy:** Dramatická redukce času vývoje (Time-to-Market), snížení nákladů na vývoj a testování, zvýšení celkové spolehlivosti systému (používají se komponenty prověřené provozem) a vynucení architektonických standardů.
* **Úrovně znovupoužitelnosti:**
    * *Úroveň zdrojového kódu (Ad-hoc reuse):* Kopírování a vkládání fragmentů kódu (Copy-Paste). Nebezpečný anti-pattern vedoucí k masivní duplicitě, fragmentaci chyb a extrémnímu růstu nákladů na údržbu.
    * *Knihovny a komponenty:* Izolované programové balíčky s jasně definovaným rozhraním spravované pomocí balíčkovacích systémů (npm, NuGet, Maven). Řeší generické problémy (např. validace, kryptografie).
    * *Návrhové vzory (Design Patterns):* Znovupoužitelná koncepční řešení často se opakujících architektonických problémů v objektově orientovaném návrhu (např. *Strategy, Observer, Factory*).
    * *Frameworky:* Komplexní softwarové kostry aplikací, které diktují celkovou architekturu. Vývojář vkládá svůj kód do předpřipravených slotů (Inversion of Control - IoC).
    * *Mikroslužby / SOA (Service-Oriented Architecture):* Znovupoužitelnost celých funkčních byznys celků ve formě nezávisle běžících služeb zapouzdřených za stabilním síťovým API (např. jednotná autentizační služba pro 10 různých firemních aplikací).
* **Překážky a limity:**
    * *Pravidlo třetího použití (Rule of Three):* Návrh, vývoj, testování a precizní zdokumentování skutečně znovupoužitelné komponenty je typicky **$3\times$ dražší a náročnější** než vývoj jednoúčelového řešení pro jeden konkrétní projekt. Investice se vrací až při opakovaném nasazení.
    * *Syndrom "Not Invented Here" (NIH):* Psychologická bariéra vývojových týmů, které odmítají adoptovat cizí/externí kód a preferují psaní vlastních, často duplicitních řešení od nuly.
    * *Závislosti (Dependency Hell):* Riziko zablokování aktualizací celého systému kvůli kaskádovým závislostem na zastaralých nebo nekompatibilních verzích znovupoužitelných knihoven třetích stran.