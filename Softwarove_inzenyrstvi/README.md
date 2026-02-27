# Softwarové inženýrství (PA017)

## Popis okruhu

Proces vývoje SW. Metodika Rational Unified Process. Agilní vývoj SW. Fáze testování a typy testů. Softwarové metriky, refaktoring kódu. Kvalita softwaru. Odhadování nákladů a času vývoje SW. Údržba a znovupoužitelnost.

## Témata

### Proces vývoje softwaru
- Životní cyklus softwaru (SDLC): fáze a jejich obsah
- Modely procesů vývoje:
  - **Vodopádový model (Waterfall):** lineární sekvence fází; jednoduchý, ale nepružný
  - **Iterativní a inkrementální model:** opakující se cykly; postupné rozšiřování funkcionality
  - **Spirálový model (Boehm):** opakující se cykly s analýzou rizik
  - **Prototypování:** rychlé vytváření prototypů pro ověření požadavků
- Artefakty procesu: specifikace požadavků (SRS), design dokumenty, testovací plány, kód, manuály

### Metodika Rational Unified Process (RUP)

#### Základní charakteristiky
- Iterativní a inkrementální proces vývoje
- Use-case driven: požadavky vyjádřeny pomocí use casů
- Architecture-centric: zaměření na softwarovou architekturu od začátku
- Risk-driven: prioritizace rizik a jejich řešení v raných iteracích

#### Čtyři fáze RUP
1. **Zahájení (Inception):** vymezení rozsahu projektu, identifikace klíčových use casů, první odhady
2. **Elaborace (Elaboration):** analýza problémové domény, návrh architektury, plán projektu, řešení hlavních rizik
3. **Konstrukce (Construction):** iterativní vývoj funkcionality, testování, integrace
4. **Přechod (Transition):** nasazení k uživatelům, beta testování, opravy chyb, školení

#### Pracovní toky (Workflows / Disciplines)
- Business Modeling, Requirements, Analysis & Design, Implementation, Test, Deployment
- Project Management, Configuration & Change Management, Environment

### Agilní vývoj softwaru

#### Agilní manifest (Agile Manifesto)
- Hodnoty: individua a interakce > procesy a nástroje; funkční software > dokumentace; spolupráce se zákazníkem > smlouvy; reakce na změnu > dodržování plánu

#### Scrum
- Role: Product Owner, Scrum Master, Development Team
- Artefakty: Product Backlog, Sprint Backlog, Increment
- Ceremonie: Sprint Planning, Daily Scrum, Sprint Review, Sprint Retrospective
- Sprint: iterace 1–4 týdny s konkrétním cílem

#### Kanban
- Vizualizace práce na tabuli (Kanban board): To Do, In Progress, Done
- Omezení WIP (Work in Progress): prevence přetížení
- Průběžné doručování (continuous delivery)

#### Extreme Programming (XP)
- Praktiky: TDD (Test-Driven Development), pair programming, continuous integration, refactoring, jednoduchý design
- Hodnoty: komunikace, jednoduchost, zpětná vazba, odvaha, respekt

#### Scaled Agile
- SAFe (Scaled Agile Framework): škálování agility na úroveň programu a portfolia
- LeSS (Large-Scale Scrum): Scrum pro více týmů

### Fáze testování a typy testů

#### Úrovně testování
1. **Unit testy:** testování jednotlivých komponent/funkcí v izolaci; nástroje: JUnit, pytest, NUnit
2. **Integrační testy:** testování interakcí mezi komponentami; nástroje: TestNG, pytest
3. **Systémové testy:** testování celého systému vůči požadavkům
4. **Akceptační testy (UAT):** ověření shody s požadavky zákazníka

#### Typy testů
- **Funkcionální testy:** ověření správnosti funkcí
- **Nefunkcionální testy:** výkon (performance), zátěž (load), stres (stress), bezpečnost, použitelnost
- **Regresní testy:** ověření, že změny nezlomily existující funkcionalitu
- **Smoke testy:** rychlé základní ověření po nasazení
- **Exploratorní testování:** neformální, bez předem definovaných testových případů

#### Testovací techniky
- **Black-box testování:** testování bez znalosti vnitřní struktury; ekvivalenční rozdělení, hraniční hodnoty
- **White-box testování:** testování s přístupem ke zdrojovému kódu; pokrytí větví, podmínek, cest
- **Grey-box testování:** kombinace obou přístupů

#### Testovací pyramida
- Základ: unit testy (mnoho, rychlé)
- Střed: integrační testy
- Vrchol: E2E/UI testy (málo, pomalé, nákladné)

### Softwarové metriky a refaktoring kódu

#### Softwarové metriky

##### Metriky kódu
- **LOC (Lines of Code):** jednoduchost vs. nevypovídající o kvalitě
- **Halsteadovy metriky:** objem, složitost, délka programu na základě operátorů a operandů
- **Cyklomatická složitost (McCabe):** počet lineárně nezávislých cest; CC = E - N + 2P
- **Metriky OOP:** CBO (Coupling between Objects), WMC (Weighted Methods per Class), LCOM (Lack of Cohesion of Methods), DIT (Depth of Inheritance Tree)

##### Procesní metriky
- Defect density: počet defektů na jednotku kódu
- Mean Time to Failure (MTTF), Mean Time to Repair (MTTR)
- Velocity (Scrum): story points dokončené za sprint

#### Refaktoring kódu
- Definice: změna vnitřní struktury kódu bez změny vnějšího chování
- Cíle: zlepšení čitelnosti, snížení složitosti, snížení technického dluhu
- Code smells: příznaky špatného kódu (God Class, Long Method, Duplicate Code, Feature Envy, Shotgun Surgery)
- Refaktorovací techniky:
  - Extract Method/Class/Interface
  - Rename, Move, Inline
  - Replace Conditional with Polymorphism
  - Introduce Design Pattern

### Kvalita softwaru

#### Modely kvality softwaru
- **ISO/IEC 25010 (SQuaRE):** charakteristiky: funkcionální vhodnost, výkon, kompatibilita, použitelnost, spolehlivost, bezpečnost, udržovatelnost, přenositelnost
- **McCallův model:** faktory, kritéria a metriky
- **Boursmův model (FURPS+):** Functionality, Usability, Reliability, Performance, Supportability

#### Řízení kvality
- Quality Assurance (QA): procesy pro prevenci defektů
- Quality Control (QC): detekce defektů v produktech
- Code review: inspekce kódu, Fagan inspection
- Statická analýza: nástroje pro automatické odhalení problémů (SonarQube, Checkstyle)

### Odhadování nákladů a času vývoje

#### Metody odhadování
- **Expert judgment:** zkušení odborníci odhadují na základě zkušeností; Delphi technika
- **Analogie:** porovnání s podobnými dokončenými projekty
- **COCOMO (Constructive Cost Model):** parametrický model; základní, střední, pokročilý
  - Effort = a · (KLOC)^b; Tdev = c · (Effort)^d
- **Funkční body (Function Points):** měří funkcionalitu z pohledu uživatele; nezávislé na jazyce
- **Story points + planning poker (Agile):** relativní odhady komplexity

#### Faktory ovlivňující náklady
- Velikost projektu, komplexita, technologie, zkušenosti týmu, požadavky na spolehlivost

### Údržba a znovupoužitelnost

#### Údržba softwaru
- Typy údržby (ISO 14764):
  1. **Korektivní:** opravy chyb
  2. **Adaptivní:** přizpůsobení novému prostředí (OS, HW, závislosti)
  3. **Perfektivní:** zlepšení výkonu nebo udržovatelnosti
  4. **Preventivní:** předcházení budoucím problémům (refaktoring, aktualizace dokumentace)
- Technický dluh (Technical Debt): kumulace špatných rozhodnutí; nutnost splácení
- Legacy systémy: výzvy udržitelnosti starého kódu

#### Znovupoužitelnost (Reusability)
- Úrovně znovupoužití: zdrojový kód, komponenty, knihovny, frameworky, architektury, vzory
- Návrhové vzory (Design Patterns): Factory, Singleton, Observer, Strategy, Decorator, MVC
- Softwarové komponenty a jejich standardy: COM/DCOM, JavaBeans, OSGi
- Microservices: znovupoužitelné služby přes API
- Open-source software a jeho integrace
