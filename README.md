# FI-MUNI-SZMGR – Státní závěrečná magisterská zkouška

Materiály k přípravě na státní závěrečnou magisterskou zkoušku na Fakultě informatiky Masarykovy univerzity (FI MUNI).

Otázky viz https://www.fi.muni.cz/studies/fe-mgr/uizd2018.html

## Okruhy ke státní zkoušce
### Společný základ programu

| Okruh | Předmět | Poznámka |
|---|---|---|
| [Metody umělé inteligence](./Metody_umele_inteligence.md) | IV126 | |
| [Statistika](./Statistika.md) | MA012 | |
| [Infrastrukturní a cloudové systémy](./Infrastrukturni_a_cloudove_systemy.md) | PA234 | Povinné pro šablonu 2024/2025 nebo novější |
| [Databáze](./Databaze.md) | PA152 | |
| [Neuronové sítě](./Neuronove_site.md) | PV021 | |
| [Dobývání znalostí](./Dobyvani_znalosti_PV056.md) | PV056 | Pro absolventy předmětu do jara 2024 včetně |
| [Strojové učení](./Strojove_uceni.md) | PV056 | Pro absolventy předmětu od jara 2025 včetně |
| [Dobývání znalostí](./Dobyvani_znalosti_PA212_PV056.md) | PA212, PV056 | Pro absolventy předmětu PV056 od jara 2025 včetně |
| [Vizualizace](./Vizualizace.md) | PV251 | |

### Zpracování a analýza rozsáhlých dat

| Okruh | Předmět | Poznámka |
|---|---|---|
| [Analýza dat](./Analyza_dat.md) | PA220 | |
| [Pokročilé techniky vyhledávání](./Pokrocile_techniky_vyhledavani.md) | PA212 | Povinné pro šablonu 2022/2023 nebo novější |
| [Podobnostní hledání](./Podobnostni_hledani.md) | PA128 | Povinné pro šablonu 2022/2023 nebo novější |
| [Cloudové počítání a distribuované databáze](./Cloudove_pocitani_a_distribuovane_databaze.md) | PA200, PA195 | |
| [Softwarové inženýrství](./Softwarove_inzenyrstvi.md) | PA017 | |

# Otázky N-UIZD Umělá inteligence a zpracování dat

## Společný základ programu 
_Pro studium od kontrolní šablony 2024/2025 a pro absolventy PV056 od jara 2025 včetně. Zkopírování ze stránek FI MUNI 30.3.2026_

1) **[Metody umělé inteligence](./Metody_umele_inteligence.md):** Prohledávání stavového prostoru, lokální prohledávání a metaheuristiky s jedním řešením, populační metaheuristiky (evoluční algoritmy, inteligence hejna). Plánování, reprezentace problému, plánování se stavovým prostorem. Práce s neurčitostí, Bayesovské sítě, exaktní a aproximační odvozování, čas a neurčitost, teorie užitku, Markovský rozhodovací proces, iterace hodnot, iterace strategie. Robotika, plánování pohybu robota (konfigurační prostor, kombinatorické a pravděpodobnostní přístupy). (IV126)
2) **[Statistika](./Statistika.md):** Základní statististické metody (bodové odhady, intervaly spolehlivosti, testování statistických hypotéz). ANOVA. Neparametrické testy hypotéz. Mnohonásobná lineární regrese, autokorelace, multikolinearita. Analýza hlavních komponent (PCA). (MA012)
> 3) **Výkonné počítače a intenzivní výpočty:** Superskalární, multijádrové a mnohojádrové (GPU, MIC) procesory, MIMD a SIMD paralelismus. Organizace paměti, sdílená a distribuovaná, cache koherence. Optimalizace kódu, optimalizující překladače. Distribuované systémy, topologie síťového propojení. Programování paralelních a distribuovaných systémů. (PA039) (povinné pro studium dle kontrolní šablony 2023/2024 nebo starší)
4) **[Infrastuctural and Cloud Systems](./Infrastrukturni_a_cloudove_systemy.md):** Data-center architecture; supercomputers vs. heterogeneous clusters; orchestration architectures (PBS/grids, Kubernetes, OpenStack); GPU vs. CPU computing and workloads that benefit from massive parallelism; scalable application models; storage tiers, technologies, data temperature and movement; infrastructure resilience and reliability; automation, DevOps/GitOps and SRE; workflow managers and workload portability; identity, SSO, and AAI. (PA234) (povinné pro studium dle kontrolní šablony 2024/2025 nebo novější)
5) **[Databáze](./Databaze.md):** Ukládání dat, adresování záznamů. Indexování a hašování více atributů, rastrové (bitmap) indexy, dynamické hašování. Vyhodnocování dotazu a algoritmy, statistiky a odhady nákladů. Optimalizace dotazů a schémat, pravidla pro transformaci dotazů, rozdělování dat. Ladění dotazů a schématu. Zpracování transakcí, výpadky a zotavení. Bezpečnost, přístupová oprávnění. (PA152)
6) **[Neuronové sítě](./Neuronove_site.md):** Vícevrstvé sítě a jejich výrazové schopnosti. Učení neuronových sítí: Gradientní sestup, zpětná propagace, praktické otázky učení (příprava dat, inicializace vah, volba a adaptace hyperparametrů). Regularizace. Konvoluční sítě. Rekurentní sítě. (PV021)
> 7) **[Strojové učení](./Strojove_uceni.md):** Semi-supervised learning a aktivní učení. Ansámblové učení. Základy analýzy anomálií. Pokročilé metody vyhodnocování experimentů (křížová validace, ROC křivky, AUC, M učících algoritmů na N datových sadách, bootstrapping). Teoretické základy strojového učení (relace generalizace ve výrokové a predikátové logice, prostor hypotéz a verzí, bias-variance trade off) (PV056) pro absolventy předmětu do jara 2024 včetně
> 8) **[Dobývání znalostí](./Dobyvani_znalosti_PV056.md):** Předzpracování dat. Učení častých vzorů a asociačních pravidel. Nástroje pro strojové učení a dolování z dat (obecně + popis jednoho podrobně). Analýza temporálních dat. (PV056) pro absolventy předmětu do jara 2024 včetně
9) **[Strojové učení](./Strojove_uceni.md):** Základy strojového učení (supervizované, semi-supervizované a nesupervizované učení; operace klasifikace, regrese, detekce anomálií). Učení metrik (kontrastivní učení, triplet-loss učení). Vektorová/produktová kvantizace s využitím pro aproximované hledání. Principy křížově-modálního (cross-modal) učení (CLIP). (PV056) pro absolventy předmětu od jara 2025 včetně
10) **[Dobývání znalostí](./Dobyvani_znalosti_PA212_PV056.md):** Asociační pravidla a algoritmy pro hledání frekventovaných vzorů (A-Priori, PCY). Principy shlukovacích algoritmů (k-means, hierarchické shlukování, DBSCAN, Chameleon). Analýza temporálních dat: vlastnosti a předzpracování časových řad, DTW, klouzavý průměr (MA). (PA212, PV056) pro absolventy předmětu PV056 od jara 2025 včetně
11) **[Vizualizace](./Vizualizace.md):** Základní metriky pro hodnocení kvality vizualizace (efektivita a expresivita), osm základních vizuálních proměnných. Základní vizualizační techniky pro 1D, 2D, 3D (explicitní a implicitní reprezentace povrchu). Techniky pro vizualizaci multidimenzionálních dat (paralelní souřadnice, RadViz, scatterplot matrices, dimensional stacking) a hierarchických struktur (treemaps). Základní třídy interakčních technik (fisheye, perspektivní stěny), specifika aplikace interakčních technik v prostoru samotných dat a v prostoru jejich atributů. (PV251)

## Specializace - Zpracování a analýza rozsáhlých dat
1) **[Analýza dat](./Analyza_dat.md).** Datové sklady a jejich životní cyklus, zúžené datové sklady (data marts), dimezionální model a jeho implementace (star schema, data cube). Proces extrakce, transformace a nahrávání dat (ETL), profilování dat, datová integrita, kvalita dat. (PA220)
2) **[Pokročilé techniky vyhledávání](./Pokrocile_techniky_vyhledavani.md)**. Zpracování dat pomocí přístupu Map-Reduce. Vyhledávání pomocí technik Locality-Sensitive Hashing (LSH) a Min-Hashing. Zpracování proudů dat (DGIM, Bloom filtry). PageRank a jeho výpočet iterační metodou. (PA212) (povinné pro studium dle kontrolní šablony 2022/2023 nebo novější)
3) **[Podobnostní hledání](./Podobnostni_hledani.md).** Principy podobnostního hledání: metrický prostor, extrakce popisovačů a jejich vztah s člověkem vnímanou podobností, typy dotazů a jejich definice. Principy indexování: dělení dat, filtrování dat (pivoting). Srovnání s tradičními indexy (B+ trees). (PA128) (povinné pro studium dle kontrolní šablony 2022/2023 nebo novější)
4) **[Cloudové počítání a distribuované databáze](./Cloudove_pocitani_a_distribuovane_databaze.md).** Cloudové počítání: základní principy, infrastruktura jako služba (IaaS), virtualizace a kontejnery, migrace na cloud, bezpečnost služeb, horizontální a vertikální škálovatelnost. Současné technologie a poskytovatelé cloudových služeb. Distribuované databáze: principy a výhody NoSQL přístupu, konzistence, distribuce dat. Úložiště párů klíč-hodnota, dokumentové databáze, grafové databáze, sloupcově orientované databáze. (PA200, PA195)
5) **[Softwarové inženýrství](./Softwarove_inzenyrstvi.md).** Proces vývoje SW. Metodika Rational Unified Process. Agilní vývoj SW. Fáze testování a typy testů. Softwarové metriky, refaktoring kódu. Kvalita softwaru. Odhadování nákladů a času vývoje SW. Údržba a znovupoužitelnost. (PA017)
> 6) **Aplikovaná kryptografie.** Symetrická a asymetrická kryptografie, rozdíly a použití. Hašovací funkce a jejich aplikace. Digitální podpis: konstrukce, nepopiratelnost, správa veřejných klíčů, certifikační autority a infrastruktury veřejných klíčů. Autentizace, autorizace a kontrola přístupu. (PV079) (povinné pro studium dle kontrolní šablony 2021/2022 nebo starší)
> 7) **Programování, organizace a administrace souborů.** UNIX systém: architektura jádra, paměťový model jádra. Program: start a ukončení, argumenty, proměnné prostředí. Proces: atributy procesu, stavy procesu, komunikace mezi procesy (roura, signály, spolehlivé signály). Indexování a hašování: B+ stromy, lineární a rozšiřitelné hašování. Souborový systém: principy, organizace dat, rysy vnějších pamětí, I/O operace, pokročilé I/O operace (multiplexing pomocí select() a poll(), zamykání souborů, scatter-gather I/O, paměťově mapované I/O operace), speciální soubory, distribuované souborové systémy. (PV065, PA152) (povinné pro studium dle kontrolní šablony 2021/2022 nebo starší)
