# Pokročilé techniky vyhledávání (PA212)

> **Poznámka:** Povinné pro studium dle kontrolní šablony 2022/2023 nebo novější.

## Popis okruhu

Zpracování dat pomocí přístupu Map-Reduce. Vyhledávání pomocí technik Locality-Sensitive Hashing (LSH) a Min-Hashing. Zpracování proudů dat (DGIM, Bloom filtry). PageRank a jeho výpočet iterační metodou.

## Témata

### Map-Reduce

#### Princip Map-Reduce
- Motivace: zpracování velkých dat paralelně na distribuovaném clusteru
- Programovací model: dva kroky – **Map** a **Reduce**
- **Map fáze:** aplikace uživatelské funkce na každý vstupní záznam → generování párů (klíč, hodnota)
- **Shuffle & Sort fáze:** seskupení párů se stejným klíčem a seřazení
- **Reduce fáze:** agregace párů se stejným klíčem → výstupní (klíč, hodnota)

#### Implementace
- Apache Hadoop MapReduce: HDFS, YARN, MapReduce framework
- Apache Spark: in-memory alternativa, RDD, DataFrames
- Fault tolerance: spekulativní provádění, re-execution při selhání
- Příklady: počítání slov (Word Count), inverzní index, grep, PageRank

#### Vzory Map-Reduce
- Kombinátory (Combiners): lokální agregace pro redukci síťového přenosu
- Sekundární třídění: řazení hodnot v rámci Reduce fáze
- Řetězení Map-Reduce jobů (job chaining)

### Locality-Sensitive Hashing (LSH) a Min-Hashing

#### Jaccard podobnost
- Definice: |A ∩ B| / |A ∪ B| pro množiny
- Využití: porovnávání dokumentů reprezentovaných jako množiny slov (shingling)

#### Shingling
- k-shingle: podřetězec délky k v dokumentu
- Reprezentace dokumentu jako množiny k-shingles

#### Min-Hashing
- Cíl: efektivní aproximace Jaccard podobnosti
- Princip: pro náhodnou permutaci π je MinHash(S, π) = min{π(i) : i ∈ S}
- Klíčová vlastnost: P[MinHash(A, π) = MinHash(B, π)] = Jaccard(A, B)
- Signature matrix: matice MinHash hodnot pro všechny dokumenty
- Výpočet: náhrada permutací hašovacími funkcemi (universal hashing)

#### Locality-Sensitive Hashing (LSH)
- Cíl: efektivní hledání přibližně podobných párů (candidate pairs) bez porovnání všech párů
- Princip: dokumenty s vysokou podobností by měly být zhashovány do stejného bucketu
- LSH pro MinHash: band technique
  - Rozdělení signature matrix na b bandů po r řádcích
  - Dokumenty jsou kandidáti, pokud jsou v alespoň jednom bandu shodné
  - Kompromis: b a r řídí práh podobnosti, FP a FN rate
- Obecný LSH framework: rodiny hašovacích funkcí pro různé vzdálenostní míry

### Zpracování proudů dat

#### DGIM (Datar-Gionis-Indyk-Motwani)
- Problém: počítání jedniček v posledních N prvcích bitového proudu s omezenou pamětí
- Přesné počítání: O(N) paměti – není škálovatelné
- DGIM aproximace:
  - Buckety: každý bucket má timestamp posledního prvku a velikost (mocnina 2)
  - Invariant: existují nejvýše 2 buckety každé velikosti
  - Merge: při třetím bucketu stejné velikosti se dva starší sloučí
  - Výpočet: součet velikostí bucketů (první bucket napůl)
- Přesnost: chyba nejvýše 50 %; lze snížit povolením více bucketů

#### Bloom filtry
- Problém: efektivní testování příslušnosti prvku do množiny s omezenou pamětí
- Struktura: bitové pole délky m, k hašovacích funkcí
- Přidání prvku: hashovati k-krát, nastavit příslušné bity
- Dotaz: ověřit, zda všechny k bitů jsou nastaveny
- Vlastnosti:
  - Žádné false negatives
  - Možné false positives (pravděpodobnost závisí na m, k, počtu prvků n)
  - Nelze mazat (Counting Bloom Filter toto řeší čítači místo bitů)
- Aplikace: databáze (RocksDB, Cassandra), webové cache, deduplication

### PageRank

#### Princip PageRank
- Motivace: hodnocení důležitosti webových stránek podle struktury odkazů
- Základní idea: stránka je důležitá, pokud na ni odkazují jiné důležité stránky
- Random surfer model: náhodný uživatel kliká na náhodné odkazy nebo teleportuje
- Matematická definice: r(v) = α · Σ_{u→v} r(u)/outdeg(u) + (1-α)/N
  - α: damping factor (typicky 0.85)
  - N: celkový počet stránek

#### Iterační výpočet PageRank
- Inicializace: r⁰(v) = 1/N pro všechny stránky
- Iterační krok: aplikace PageRank vzorce
- Konvergence: iterace do té doby, než se hodnoty ustalí (||r^(k+1) - r^k|| < ε)
- Matice přechodu: stochastická matice M; PageRank = stacionární distribuce Markovova řetězce
- Problém dangling nodes (stránky bez odchozích odkazů): uniformní redistribuce

#### Výpočet PageRank v Map-Reduce
- Mapování: každá stránka distribuuje svůj rank rovnoměrně odchozím odkazům
- Redukce: součet příchozích rank kontribucí + teleportace
- Škálovatelnost pro velké grafy (miliardy stránek)

#### Varianty a aplikace PageRank
- Topic-Sensitive PageRank: biased teleportation pro konkrétní témata
- TrustRank: PageRank vycházející ze spolehlivých seed stránek pro detekci spamu
- Aplikace v sociálních sítích: HITS algoritmus (hubs and authorities)
