# Dobývání znalostí (PA212, PV056)

> **Poznámka:** Pro absolventy předmětu PV056 od jara 2025 včetně.

## Popis okruhu

Asociační pravidla a algoritmy pro hledání frekventovaných vzorů (A-Priori, PCY). Principy shlukovacích algoritmů (k-means, hierarchické shlukování, DBSCAN, Chameleon). Analýza temporálních dat: vlastnosti a předzpracování časových řad, DTW, klouzavý průměr (MA).

## Témata

### Asociační pravidla a algoritmy pro hledání frekventovaných vzorů

#### Základní pojmy
- Transakční databáze a itemset
- Podpora (support): relativní četnost výskytu itemsetu
- Důvěra (confidence): podmíněná pravděpodobnost pravidla A → B
- Výtah (lift): míra závislosti levé a pravé strany pravidla
- Frekventovaný itemset: itemset s podporou ≥ min_support

#### Algoritmus A-Priori
- Princip antimonotonicity: každá podmnožina frekventovaného itemsetu je frekventovaná
- Fáze generování: kandidáti k-itemsetů z (k-1)-itemsetů (join step)
- Fáze prořezávání: eliminace kandidátů s nefrekventovanými podmnožinami (prune step)
- Fáze počítání: průchod databází a počítání podpory kandidátů
- Časová složitost: O(n · 2^k) v nejhorším případě, n = počet transakcí
- Hlavní bottleneck: opakované průchody databází

#### Algoritmus PCY (Park-Chen-Yu)
- Motivace: omezení počtu kandidátů dvojic (2-itemsetů) pomocí hašování
- Hašovací tabulka (hash table) v 1. průchodu: hashování párů do bucketů
- Bitmapa (bitmap): zakódování frekventovaných bucketů
- 2. průchod: kandidáti pouze z frekventovaných bucketů
- Vícenásobný hašovací trik: použití více hašovacích tabulek (Multistage, Multihash)

#### Algoritmus FP-Growth
- FP-strom (Frequent Pattern tree): kompaktní reprezentace databáze
- Podmíněná FP-báze a podmíněný FP-strom
- Rekurzivní těžba vzorů bez generování kandidátů

### Principy shlukovacích algoritmů

#### k-means
- Algoritmus: inicializace centroidů, přiřazení bodů, přepočet centroidů, opakování
- Funkce minimalizace: součet kvadratických vzdáleností bodů od centroidu (SSE/inertia)
- Citlivost na inicializaci: k-means++ pro lepší inicializaci
- Problémy: sférické shlukování, citlivost na odlehlé hodnoty, nutnost znát k předem
- Výběr k: Elbow method, Silhouette score, Gap statistic

#### Hierarchické shlukování
- Aglomerativní (bottom-up): každý bod je vlastní shlukem, postupné slučování
- Divisivní (top-down): začíná jedním shlukem, postupné dělení
- Dendrogram: stromový diagram zobrazující hierarchii shluků
- Metody propojení (linkage): single, complete, average, Ward
- Výhody: dendrogramová vizualizace, není nutno specifikovat k předem

#### DBSCAN (Density-Based Spatial Clustering of Applications with Noise)
- Parametry: ε (epsilon, poloměr okolí), MinPts (minimální počet bodů)
- Typy bodů: core point, border point, noise point
- Algoritmus: rozšiřování hustotně dosažitelných shluků
- Výhody: odhalení shluků libovolného tvaru, robustnost vůči noise
- Nevýhody: citlivost na parametry, problémy s variabilní hustotou

#### Chameleon
- Dvoufázový algoritmus: 1. grafy sousednosti a hrubé shlukování, 2. aglomerativní slučování
- k-NN graf: reprezentace dat jako řídký graf sousednosti
- Metriky pro slučování: relative interconnectivity (RI) a relative closeness (RC)
- Výhoda: respektuje tvar a hustotu shluků; lépe než DBSCAN i hierarchické metody pro komplexní tvary

### Analýza temporálních dat

#### Vlastnosti časových řad
- Stacionarita: konstantní střední hodnota a rozptyl v čase
- Trend: dlouhodobé zvyšování/snižování hodnot
- Sezónnost: periodické vzory v čase
- Šum (noise): náhodná složka
- Autokorelace: korelace řady se svým zpožděním (lag)

#### Předzpracování časových řad
- Normalizace a standardizace (z-score, min-max)
- Resample a interpolace (práce s chybějícími hodnotami a nerovnoměrným vzorkováním)
- Dekompozice: STL (Seasonal and Trend decomposition using Loess), additivní a multiplikativní model
- Odstraňování šumu: vyhlazování (smoothing)

#### Klouzavý průměr (Moving Average, MA)
- Prostý klouzavý průměr (Simple MA, SMA): průměr posledních n hodnot
- Vážený klouzavý průměr (Weighted MA, WMA): váhy lineárně klesají
- Exponenciální klouzavý průměr (Exponential MA, EMA): váhy exponenciálně klesají; parametr α
- Použití: vyhlazování časových řad, identifikace trendu, obchodní signály

#### Dynamic Time Warping (DTW)
- Motivace: srovnání časových řad s různou rychlostí nebo posunutím v čase
- Algoritmus: dynamické programování na distanční matici
- DTW vzdálenost: minimální cena zarovnání dvou sekvencí
- Sakoe-Chiba band a Itakura parallelogram: omezení zarovnání
- Využití DTW: klasifikace a shlukování časových řad, hledání podobných podsekvencí
- 1-NN DTW: jednoduché a efektivní klasifikační baseline
