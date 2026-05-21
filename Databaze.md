# Databáze

> Ukládání dat, adresování záznamů. 
> Indexování a hašování více atributů, rastrové (bitmap) indexy, dynamické hašování. 
> Vyhodnocování dotazu a algoritmy, statistiky a odhady nákladů. 
> Optimalizace dotazů a schémat, pravidla pro transformaci dotazů, rozdělování dat. 
> Ladění dotazů a schématu. Zpracování transakcí, výpadky a zotavení. 
> Bezpečnost, přístupová oprávnění. 

---

## Ukládání dat

Výkon každého databázového systému (DBMS) je primárně limitován rychlostí komunikace se sekundárním úložištěm. Tento jev se označuje jako **I/O úzké hrdlo** (Input/Output bottleneck), protože operace v operační paměti RAM jsou řádově rychlejší než čtení a zápis na disk. Databáze proto musí být navržena tak, aby minimalizovala počet diskových operací.

### 1. Paměťová hierarchie a specifika hardwaru
Aby mohl systém efektivně fungovat, využívá paměťovou hierarchii, kde platí: čím je paměť blíže procesoru, tím je rychlejší, dražší a má menší kapacitu.

* **Primární paměť (RAM):** Rychlá, drahá, ale volatilní (při výpadku napájení ztrácí data). Pracuje na úrovni bajtů.
* **Sekundární paměť (HDD/SSD):** Pomalá, levná, ale perzistentní (trvalá). Hardwarová vrstva nedokáže efektivně pracovat s jednotlivými bajty, proto komunikace mezi RAM a diskem probíhá vždy v **atomických blocích (stránkách)**, typicky o velikosti $4\text{ KiB}$ až $8\text{ KiB}$.

#### Mechanické disky (HDD)
Přístupová doba k datům na HDD je diktována mechanikou a skládá se ze:
1. **Seeku:** Fyzický přesun čtecí hlavy nad správnou stopu ($4-10\text{ ms}$).
2. **Rotační latence:** Čekání, než se plotna otočí pod hlavu (polovina otáčky).
3. **Samotného přenosu dat.**

Z této konstrukce plyne zásadní pravidlo: **Náhodný přístup (Random I/O) je u HDD až $300\times$ pomalejší než sekvenční přístup (Sequential I/O).** Při sekvenčním čtení se hlava přesune pouze jednou a pak kontinuálně čte celou stopu.

#### Polovodičové disky (SSD)
NAND Flash paměti sice eliminovaly mechanický přesun hlav, ale přinesly novou asymetrii mezi čtením a zápisem:
* **Čtení a zápis** probíhají po **stránkách** (např. $4\text{ KiB}$).
* **Mazání** lze provést pouze po celých **blocích** (např. $128$ stránek).

Kvůli tomu nelze data jednoduše přepsat na stejném místě. Používá se strategie **Out-of-place Updates**: při změně dat se nová verze zapíše na čistou stránku a stará stránka se označí za neplatnou. Mazání na pozadí (**Garbage Collection**) pak musí neplatné bloky vyčistit. To vede k přesunům dat, opotřebení buněk a k nežádoucímu **zesílení zápisu (Write Amplification)**, kdy se fyzicky zapíše mnohem více dat, než databáze reálně požadovala.

<img alt="img.png" src="img/db/mem-hir.png" width="700"/>

### 2. Spolehlivost a optimalizace na úrovni OS (RAID)
Pro zvýšení rychlosti I/O operací a zajištění odolnosti proti selhání hardwaru se disky sdružují do logických polí **RAID** (Redundant Array of Independent Disks):

*   **RAID 0 (Striping):** Data se střídavě dělí mezi disky. Zvyšuje výkon (čte se paralelně), ale nemá žádnou redundanci (selhání 1 disku = ztráta všech dat).
*   **RAID 1 (Mirroring):** Zrcadlení dat 1:1. Výborné pro zápis transakčních logů, poskytuje vysokou bezpečnost.
*   **RAID 5 (Distributed Parity):** Data i paritní informace jsou distribuovány napříč všemi disky (minimálně 3 disky). Přežije selhání 1 disku. Nabízí dobrý poměr ceny a výkonu pro čtení, zápis je pomalejší kvůli výpočtu parity.
*   **RAID 6 (Dual Parity):** Podobně jako RAID 5, ale se dvěma paritami. Přežije simultánní výpadek až 2 disků.
*   **RAID 10 (1+0):** Kombinace zrcadlení a proužkování. Nejvyšší výkon i spolehlivost za cenu vysokých nákladů (vyžaduje dvojnásobný počet disků).

*V praxi se u SSD polí nasazuje navíc mechanismus **Diff-RAID**. Ten záměrně opotřebovává disky v poli nerovnoměrně, aby se předešlo situaci, kdy všechna SSD selžou v tentýž den kvůli dosažení limitu zápisů.*

<img alt="img.png" src="img/db/raid.png" width="600"/>

---

## Adresování záznamů

Adresování určuje, jakým způsobem jsou logické datové řádky (záznamy) transformovány do binární podoby a jak je databáze dokáže fyzicky lokalizovat na disku a následně namapovat do operační paměti.

### 1. Reprezentace záznamů na stránce
Záznamy mohou mít pevnou délku (sloupce jako `INT`, `CHAR`) nebo proměnlivou délku (`VARCHAR`, `BLOB`). U proměnlivé délky obsahuje hlavička každého řádku **vektor ofsetů** (ukazatelů na startovní pozice jednotlivých sloupců) a **Null Bitmapu** (indikaci, které sloupce jsou prázdné, aby se pro ně netratilo místo).

Fyzická organizace uvnitř jedné diskové stránky se standardně řeší architekturou **Slotted-Page (stránka se sloty)**:
* Na samotném začátku stránky se nachází **adresář slotů**, který obsahuje dvojice `(fyzický ofset, délka záznamu)`. Tento adresář roste odshora dolů.
* Samotné datové řádky se ukládají od konce stránky a rostou **odspoda nahoru**.
* Jakmile se adresář slotů potká s datovými řádky, stránka je plná.

Tento design přináší zásadní výhodu: **nepřímé adresování**. Externí struktury (např. indexy) neodkazují na absolutní fyzickou bajtovou adresu řádku na disku, ale výhradně na **ID slotu** na dané stránce. Pokud se řádek uvnitř stránky změní (např. se nafoukne a databáze musí stránku setřást a řádky fyzicky posunout), změní se pouze ofset v adresáři slotů. ID slotu zůstává stejné, takže indexy není potřeba aktualizovat.

<img alt="img.png" src="img/db/slotted-page.png" width="700"/>

### 2. Logické identifikátory a transformace do RAM
Každý řádek v databázi má svůj jednoznačný fyzický identifikátor **Record ID (RID / ROWID)**. Tento ukazatel má strukturu:

$\text{RID} = \langle \text{File ID}, \text{Page ID}, \text{Slot ID} \rangle$

1.  `File ID`: Určuje, ve kterém souboru na disku je tabulka uložena.
2.  `Page ID`: Určuje konkrétní blok (stránku) uvnitř tohoto souboru.
3.  `Slot ID`: Určuje pořadové číslo v adresáři slotů na dané stránce.

### 3. Fyzická organizace souborů (File Organization)
Podle toho, jak jsou stránky se sloty řazeny v souboru za sebou, rozlišujeme tři základní typy organizace:

1.  **Heap File (hromada):** Řádky se ukládají na diskové stránky v takovém pořadí, v jakém přicházejí, na libovolné volné místo.
    *   *Zápis:* Extrémně rychlý, $O(1)$.
    *   *Vyhledávání:* Pomalé, $O(N)$ – vyžaduje lineární průchod všech stránek (Full Table Scan).
    *   *Použití:* Logovací tabulky a staging oblasti.

2.  **Sequential File (sekvenční soubor):** Stránky a řádky jsou fyzicky udržovány v setříděném pořadí podle určitého vyhledávacího klíče.
    *   *Vyhledávání:* Rychlé, $O(\log N)$ pomocí půlení intervalu (binární vyhledávání) nad bloky.
    *   *Zápis:* Extrémně drahý. Vložení řádku doprostřed vyžaduje fyzický posun následujících dat nebo tvorbu přetékajících logických řetězců (overflow blocks), což degraduje strukturu.

3.  **Hashed File (hašovaný soubor):** Fyzické umístění stránky na disku je striktně určeno matematickou funkcí: $\text{Číslo stránky} = h(\text{Klíč}) \bmod M$, kde $M$ je pevný počet dostupných stránek (bucketů).
    *   *Vyhledávání na přesnou shodu:* Okamžité, $O(1)$ v ideálním případě bez kolizí.
    *   *Rozsahové dotazy:* Zcela nepoužitelné. *Dotaz na hodnoty v rozmezí 10 až 20 by znamenal prohledat náhodně rozházené stránky po celém disku, protože hašovací funkce záměrně likviduje sousednost dat.*

---

## Indexování a hašování více atributů

Index je pomocná datová struktura (kolekce dvojic `[klíč, ukazatel]`), která slouží k výraznému zrychlení přístupu k datům bez nutnosti sekvenčního procházení celé tabulky (Table Scan).

### Základní jednorozměrné indexy
* **B+ strom:** Standard pro relační DB. Vyvážený strom, kde jsou všechny datové ukazatele výhradně v listech a listy jsou obousměrně zřetězené. Výborný pro bodové i rozsahové dotazy (komplexita vyhledávání, vkládání i mazání je $O(\log N)$ ).
* **Hash index:** Mapuje klíč na adresu pomocí hašovací funkce. Rychlost $O(1)$, ale nepodporuje rozsahy.

<img alt="img.png" src="img/db/hash.png" width="400"/>
<img alt="img.png" src="img/db/btree.png" width="400"/>

### Klasické přístupy (Složené indexy)
* **Index na jeden atribut + filtrace:** Vyhledá se podle jednoho atributu a nalezené záznamy se následně dofiltrují podle druhé podmínky.
* **Kombinace samostatných indexů:** Každý index vrátí vlastní seznam ukazatelů (bucketů), které se následně protnou (průnik seznamů).
* **Index v indexu (Vnořený):** Strom první úrovně obsahuje v listech ukazatele na samostatné vnořené indexy druhé úrovně. Je efektivní pro dotazy na oba atributy nebo pouze na první atribut, ale nelze jej použít pro dotaz čistě na druhý atribut.
* **Zřetězení hodnot (Concatenation / Složený B+ strom):** Hodnoty klíčů se spojí do jednoho složeného klíče (např. `Příjmení + Jméno`) a indexují se společně. *Příklad: Index nad `(Příjmení, Jméno)` zafunguje pro dotaz na `WHERE Příjmení='Novák'`, ale nepomůže pro dotaz na `WHERE Jméno='Jan'` kvůli uspořádání zleva doprava.*

<img alt="img.png" src="img/db/index-in-index.png" width="300"/>

### Dělené hašování (Partitioned Hashing)
* **Princip:** Pro vyhledávání nad více klíči se použije jedna společná výsledná adresa bloku. Ta vznikne tak, že se spojí bitové výstupy samostatných hašovacích funkcí pro jednotlivé atributy.
* **Příklad:** Atribut `Dept` má funkci $h_1$ a `Salary` funkci $h_2$. Výsledná adresa vznikne spojením jejich bitů (např. bity z $h_1$ tvoří začátek adresy, bity z $h_2$ konec).
* **Vlastnosti dotazování:** Pokud dotaz specifikuje všechny atributy, určí se jedna přesná adresa bucketu. Pokud specifikuje pouze jeden, zbývající bity adresy jsou neznámé a databáze musí prohledat všechny adresy odpovídající známému bitovému vzoru.

<img alt="img.png" src="img/db/part-hash.png" width="200"/>

### Mřížkový index (Grid Index)
* **Princip:** Prostor dat je rozdělen do vícedimenzionální mřížky (matice), kde každá osa odpovídá jednomu atributu. Hodnoty na osách mohou být definovány i jako intervaly (tzv. binning). Každá buňka mřížky ukazuje na příslušný datový bucket (případně s využitím indirection/ukazatelů).
* **Vlastnosti dotazování:** Velmi efektivní pro dotazy na přesnou shodu i pro rozsahové dotazy (`range queries`), které v mřížce vyříznou celou obdélníkovou oblast buněk.
* **Nevýhody:** Rozměry mřížky musí být fixní. Při nerovnoměrném rozdělení dat hrozí plýtvání místem (prázdné buňky) nebo přeplnění omezené kapacity buněk.

<img alt="img.png" src="img/db/grid.png" width="200"/>

### Pokročilé / AI indexy
* **Vektorové indexy (Vector Indexes):** Klíčové pro AI (vyhledávání v embeddings, RAG systémy). Používají se struktury jako **HNSW** (Hierarchical Navigable Small World) grafy nebo **IVF** (Inverted File) indexy pro rychlé přibližné hledání nejbližších sousedů (ANN – Approximate Nearest Neighbor) ve vícedimenzionálních prostorech.

### Obecná pravidla pro návrh indexů (Guidelines)
* **Dobrá selektivita:** Indexovat by se měly sloupce, kde podmínku splňuje jen malý zlomek řádků z tabulky.
* **Krátké atributy:** Kratší hodnoty atributů zvyšují větvení stromu (fan-out).
* **Cizí klíče:** Indexy jsou preferovány na join atributy (propojování tabulek).
* **Více vs. jeden:** Často je lepší mít více indexů na jeden atribut než jeden velký multi-atributový index.
* **Aktualizace a velikost:** Vysoce aktualizované tabulky by měly mít málo indexů; pro miniaturní tabulky nemá smysl tvořit indexy vůbec.
* **Pokrývající index (Covering Index):** Pokud index obsahuje všechny sloupce požadované dotazem, odpověď se přečte přímo z indexu a k samotným záznamům v tabulce se vůbec nepřistupuje.

---

## Rastrové (bitmap) indexy

Speciální databázový index, který přítomnost či nepřítomnost hodnoty reprezentuje pomocí bitových polí (sekvencí jedniček a nul) namísto klasických ukazatelů na řádky.

<img alt="img.png" src="img/db/bitmap.png" width="200"/>

### Hlavní výhody
* **Bitové operace:** Vyhledávání kombinovaných podmínek (`AND`, `OR`, `NOT`) je extrémně rychlé, protože procesor tyto operace provádí přímo na hardwarové úrovni (např. spojením vektorů pro pohlaví a město).
* **Malá velikost:** Indexy zabírají minimum místa na disku a sekvence bitů se dají výborně komprimovat.
* **Nízká kardinalita:** Ideální pro sloupce s malým počtem unikátních hodnot, jako je pohlaví, stav objednávky nebo logické hodnoty (Ano/Ne).
* **Datové sklady (OLAP):** Jsou perfektní pro analytické systémy a složité reporty, kde se data hromadně čtou, ale téměř nemění.

### Nevýhody
* **Vysoká kardinalita:** Naprosto nevhodné pro unikátní data jako rodná čísla, e-maily nebo ID, kde by index neúměrně narostl.
* **Časté zápisy (OLTP):** Zcela nevhodné pro transakční systémy s častým vkládáním a úpravou dat (`INSERT`, `UPDATE`), protože modifikace jednoho bitu často zamyká celý datový blok (vektor) a blokuje ostatní operace souběhu (lock contention).

### Komprese bitmapových indexů (Run-Length Encoding – RLE)
Bitmapové vektory jsou často velmi řídké (obsahují dlouhé sekvence samých nul) nebo naopak husté (sekvence samých jedniček). Pro minimalizaci diskového prostoru a zrychlení přenosu do RAM se komprimují pomocí metody RLE.

* **Princip:** Namísto ukládání každého bitu samostatně se zaznamená pouze hodnota bitu a délka jeho nepřerušeného opakování (run). *Příklad: Sekvence 200 nul následovaná 50 jedničkami se neuloží jako 250 jednotlivých bitů, ale jako dvojice (0, 200) a (1, 50).*
* **Výhoda pro exekuci:** Pokročilé bitmapové kodeky (např. *WAH – Word Aligned Hybrid* nebo *EWAH*) umožňují provádět procesorové bitové operace (`AND`, `OR`) přímo nad těmito komprimovanými daty bez nutnosti jejich předchozího dekomprimování v paměti.</textarea>

---

## Dynamické hašování (Dynamic Hashing)

Používá se pro data, jejichž objem se v čase mění. Na rozdíl od statického hašování předchází vzniku dlouhých přetékajících řetězců (overflow chains) průběžnou reorganizací adresního prostoru.

### 1. Rozšířitelné hašování (Extendible Hashing)
* **Princip:** Využívá mezistupeň – **adresář (directory)**, jehož velikost je vždy mocninou 2. Adresář obsahuje ukazatele na samotné datové bloky (buckety). Do adresáře se mapuje prvních $i$ bitů (globální hloubka) z výstupu hašovací funkce. Každý bucket si drží svou lokální hloubku $j$ ($j \le i$).
* **Vkládání a štěpení:** Pokud se bucket přeplní:
    * Je-li $j < i$, bucket se rozštěpí na dva, data se redistribuují podle nového bitu a adresář se pouze přenasměruje (lokální reorganizace).
    * Je-li $j = i$, **velikost adresáře se zdvojnásobí** (přidá se další bit pro indexaci, globální hloubka $i$ se inkrementuje) a až pak se bucket rozštěpí.
* **Výhody/Nevýhody:** Vysoce efektivní využití místa, vyhledání záznamu vyžaduje maximálně 2 diskové operace (adresář + bucket). Nevýhodou je, že adresář může narůst natolik, že se nevejde do paměti RAM.

<img alt="img.png" src="img/db/ext-hash.png" width="700"/>

### 2. Lineární hašování (Linear Hashing)
* **Princip:** **Nepoužívá adresář.** Počet bucketů roste lineárně (přidáváním jednoho po druhém). Pro adresaci se využívá $i$ nejnižších (koncových) bitů adresy. Rozhodování o štěpení se řídí celkovým zaplněním prostoru (faktorem zaplnění, např. překročení 80 %).
* **Štěpení:** Když nastane trigger pro štěpení, rozštěpí se konkrétní bucket určený interním ukazatelem pointeru $P$, **který se ale může lišit od bucketu, kam se právě zapisovalo** (proto mohou vzniknout dočasné overflow bloky). Pointer $P$ se posune o jedna dále. Jakmile se postupně rozštěpí všechny buckety v dané fázi, zvýší se počet bitů $i$ pro adresaci, pointer $P$ skočí na začátek (na 0) a proces běžní nanovo.
* **Výhody/Nevýhody:** Žádná režie na adresář, paměť roste plynule. Kvůli asynchronnímu štěpení se ale občas nelze vyhnout krátkým overflow řetězcům.

<img alt="img.png" src="img/db/lin-hash.png" width="700"/>

---

## Vyhodnocování dotazu, algoritmy, statistiky a odhady nákladů

Zpracování SQL dotazu probíhá ve 3 hlavních krocích:
1. **Analýza a překlad (Parsing):** Kontrola syntaxe a sémantiky proti katalogu. Vzniká logický plán (výraz relační algebry).
2. **Optimalizace (Query Optimization):** Generování ekvivalentních plánů. Cost-Based Optimizer (CBO) vybere plán s nejnižší odhadovanou cenou na základě statistik. *Starší Rule-Based Optimizer (RBO) se slepě řídil sadou pevných pravidel (např. "vždy použij index, je-li k dispozici"), což mohlo vést k neoptimálním plánům.*
3. **Kódování a spuštění (Execution):** Prováděcí engine spustí fyzický plán nad databází.

*Příklad z praxe:*
*Pro dotaz na nalezení filmů s herci narozenými v roce 1960:*
```sql
SELECT title FROM StarsIn WHERE starName IN (SELECT name FROM MovieStar WHERE birthdate = 1960);
```
*Vytvoří optimalizátor počáteční logický plán:*
$\Pi_{\text{title}} [ \sigma_{\text{starName}=\text{name} \land \text{birthdate}=1960} (\text{StarsIn} \times \text{MovieStar}) ]$
*Který následně vylepší transformací kartézského součinu na efektivnější Natural Join:*
$\Pi_{\text{title}} [ \text{StarsIn} \bowtie \sigma_{\text{birthdate}=1960}(\text{MovieStar}) ]$

<img alt="img.png" src="img/db/query-process-schema.png" width="300"/>

### Předávání dat mezi operátory
* **Materializace (Materialization):** Operátor zapíše celý mezivýsledek do dočasné tabulky na disk/do paměti a až pak ho předá dál. Vysoké I/O náklady.
* **Pipelining (Proudové zpracování):** Operátory předávají data průběžně po jednotlivých řádcích bez zápisu na disk (Iterator Model / Volcano architecture s metodami `open()`, `next()`, `close()`).

<img alt="img.png" src="img/db/pipelining.png" width="300"/>

### Algoritmy

#### A) Externí třídění (External Sort-Merge)
Používá se, pokud se tříděná data nevejdou do paměti RAM ($M$ bloků).
1. **Fáze generování běhů (Runs):** Data se načtou po částech o velikosti $M$ bloků do RAM, seřadí se a zapíšou na disk jako setříděné běhy.
2. **Fáze slévání (Merge):** V paměti se alokuje $M-1$ bloků pro vstupní proudy a $1$ pro výstup. Data se paralelně slévají do jednoho setříděného souboru.

<img alt="img.png" src="img/db/mergesort.png" width="400"/>

#### B) Algoritmy pro Join (Join Operators)
Mějme vnější relaci $R_1$ a vnitřní relaci $R_2$.
1. **Nested-Loop Join (Vnořené cykly):**
   * *Block Nested-Loop:* Pro každý blok $R_1$ v paměti se sekvenčně projde celá relace $R_2$ blok po bloku.
   * *Indexed Nested-Loop:* Pokud má $R_2$ index nad spojovacím atributem, prohledává se přímo index pro každý řádek z $R_1$. Efektivní pro malou vnější relaci.
2. **Sort-Merge Join:** Obě relace se nejprve setřídí podle spojovacího atributu a následně se procházejí paralelně jedním společným průchodem.
3. **Hash Join:**
   * *Build fáze:* Nad menší relací se v RAM vybuduje hašovací tabulka podle spojovacího klíče.
   * *Probe fáze:* Větší relace se sekvenčně čte a pro každý řádek se hašováním klíče okamžitě ověřuje shoda.

### Statistiky a odhady nákladů

CBO vyjadřuje cenu prováděcího plánu v arbitrárních jednotkách (odhad počtu diskových I/O operací a CPU cyklů). Statistiky se udržují v systémovém katalogu a aktualizují se periodicky na základě vzorkování dat přes příkaz `ANALYZE`.

**Základní metadata v katalogu:**
* $T(R)$ – celkový počet řádků (kardinalita) relace $R$.
* $B(R)$ – celkový počet datových bloků relace $R$ na disku.
* $V(R, A)$ – počet unikátních hodnot atributu $A$ v relaci $R$.
* **Histogramy:** Rozdělení četnosti hodnot pro zachycení datového zešikmení (skew).

### Matematické odhady velikosti výsledku

Při předpokladu uniformního rozdělení dat se velikost mezivýsledků odhaduje pomocí faktoru selektivity ($sf$):

* **Kartézský součin ($W = R_1 \times R_2$):**
  $$T(W) = T(R_1) \cdot T(R_2)$$

* **Selekce – Rovnost** ($\sigma_{A = \text{val}}(R)$):
  $$sf = \frac{1}{V(R, A)} \quad \implies \quad T(W) = \frac{T(R)}{V(R, A)}$$

* **Selekce – Rozsah** ($\sigma_{A \ge \text{val}}(R)$):
  $$sf = \frac{\text{Max} - \text{val} + 1}{\text{Max} - \text{Min} + 1}$$

* **Konjunkce (AND) nezávislých podmínek:**
  $$sf(C_1 \land C_2) = sf(C_1) \cdot sf(C_2)$$

* **Disjunkce (OR) nezávislých podmínek:**
  $$sf(C_1 \lor C_2) = sf(C_1) + sf(C_2) - (sf(C_1) \cdot sf(C_2))$$

* **Natural Join ($W = R_1 \bowtie R_2$) přes společný atribut $A$:**
  $$T(W) = \frac{T(R_1) \cdot T(R_2)}{\max\{V(R_1, A), V(R_2, A)\}}$$

* **Odhad počtu unikátních hodnot v mezivýsledku $U$:**
  $$V(U, A) = \min\{V(R, A), T(U)\}$$

---

## Optimalizace dotazů a schémat

Tato fáze se zaměřuje na strukturální úpravy databázového schématu (globální úroveň) a na přepisování samotných dotazů (lokální úroveň) s cílem odstranit úzká hrdla a dosáhnout maximálního výkonu.

### 1. Optimalizace schématu (Schema Optimization)
Volba správného schématu představuje trade-off mezi úsporou místa (ochranou před anomáliemi) a rychlostí čtení.

* **Normalizace (1NF, 2NF, 3NF, BCNF):** Každá funkční závislost $X \rightarrow A$ vyžaduje, aby $X$ byl superklíč. Zabraňuje redundanci a šetří místo na disku, ale vynucuje si drahé spojování tabulek (`JOIN`).
* **Denormalizace:** Záměrné porušení normálních forem pro zrychlení kritických dotazů (eliminace `JOIN`ů za cenu redundance a složitějších zápisů).

*Příklad z praxe:*
*Chceme najít položky od evropských dodavatelů. Normalizovaný dotaz spojuje 4 tabulky:*
```sql
SELECT i_orderkey, r_name FROM item, supplier, nation, region
WHERE i_suppkey = s_suppkey AND s_nationkey = n_nationkey 
AND n_regionkey = r_regionkey AND r_name = 'Europe';
```
*Vytvořením denormalizované tabulky `itemdenormalized` se sloupcem `i_regionname` získáme dotaz nad jedinou tabulkou:*
```sql
SELECT i_orderkey, i_regionname FROM itemdenormalized WHERE i_regionname = 'Europe';
```
*Tato úprava přináší až 54% nárůst propustnosti systému za cenu duplikace textového řetězce u všech 600 000 řádků.*

* **Alternative k denormalizaci - Clusterované ukládání (Clustered Storage):** Dovoluje ukládat fyzicky záznamy ze dvou různých tabulek k sobě na základě společného klíče (např. v Oracle nebo u dokumentových NoSQL databází). Například záznamy objednávek z `Order` jsou fyzicky zapsány hned za odpovídajícím dodavatelem ze `Supplier`. Rychlé čtení 1:N bez nutnosti provádět logické spojení.

### 2. Materializované pohledy a triggery
* **Materializované pohledy (Materialized Views):** Pohled, jehož data jsou fyzicky uložena v tabulce na disku. Na rozdíl od obyčejných pohledů, které se vyhodnocují za běhu, se materializovaný pohled předpočítá. Optimalizátor automaticky nahrazuje původní složité dotazy materializovaným pohledem (Query Rewrite).
* **Triggery (Databázové spouštěče):** Uložené procedury, které se automaticky spouštějí při DML (`INSERT`, `UPDATE`, `DELETE`) nebo DDL událostech. Používají se k postupné údržbě agregovaných tabulek a materializovaných pohledů (increment maintenance), což šetří výkon, ale vnáší režii na zápisové operace.

---

## Pravidla pro transformaci dotazů

Logické transformace přepisují počáteční relačně-algebraický strom dotazu do ekvivalentní, ale výpočetně mnohem efektivnější podoby. Cílem je zmenšit velikost mezivýsledků co nejdříve v průběhu exekuce.

### 1. Komutativita a asociativita (Změna pořadí)
Pořadí vyhodnocování relací není pro konečný výsledek podstatné, protože všechny atributy jsou zachovány. Optimalizátor proto může měnit strukturu stromu tak, aby drahé operace (např. spojení velkých tabulek) proběhly co nejpozději.

* **Natural Join:**
    * **Komutativita:** Na pořadí spojovaných relací nezáleží; spojení relace $R$ s relací $S$ vrátí stejný výsledek jako spojení $S$ s $R$. Optimalizátor toho využívá k tomu, aby jako vnější relaci (vlevo) zvolil tu menší z nich.
    * $$R \bowtie S = S \bowtie R$$
    * **Asociativita:** Při spojování tří a více relací nezáleží na tom, které dvě spojíme jako první. To umožňuje optimalizátoru prioritně spojit ty relace, jejichž mezivýsledek bude nejmenší, a ušetřit tak paměť.
    * $$(R \bowtie S) \bowtie T = R \bowtie (S \bowtie T)$$

* **Kartézský součin (Cross Product):**
  *   **Komutativita:** Generování všech možných kombinací řádků mezi $R$ a $S$ dává shodný výsledek bez ohledu na to, která tabulka je uvedena jako první.
  * $$R \times S = S \times R$$
  * **Asociativita:** Nezáleží na tom, zda nejprve vytvoříme kombinace pro $R \times S$ a k nim přidáme $T$, nebo nejdříve zkombinujeme $S \times T$. Výsledná multimnožina řádků bude identická.
  * $$(R \times S) \times T = R \times (S \times T)$$

* **Sjednocení (Union):**
  * **Komutativita:** Sloučení řádků z relace $R$ a $S$ do jedné tabulky vrátí stejný soubor dat, ať už začneme vkládáním řádků z $R$ nebo z $S$. 
  * $$R \cup S = S \cup R$$
  * **Asociativita:** Pokud sjednocujeme tři relace, můžeme k výsledku sjednocení $R \cup S$ přidat relaci $T$, nebo k relaci $R$ přidat hotové sjednocení $S \cup T$. Výsledek bude vždy obsahovat všechny záznamy ze všech tří zdrojů.
  * $$(R \cup S) \cup T = R \cup (S \cup T)$$


### 2. Pravidla pro Selekci ($\sigma$)
Selekce filtruje řádky. Lze ji rozkládat na kaskády nezávislých podmínek, což umožňuje přesouvat konkrétní jednodušší filtry hlouběji do stromu.

* **Kaskádové větvení (splitting AND):**
    * **Princip:** Složenou podmínku spojenou operátorem `AND` lze rozdělit na sérii samostatných selekcí prováděných za sebou. Výhodou je, že jednodušší dílčí filtry může optimalizátor posunout hlouběji do stromu blíž k samotným datům.
    * $$\sigma_{p_1 \land p_2}(R) = \sigma_{p_1}[\sigma_{p_2}(R)]$$

* **Rozklad disjunkce (splitting OR):**
    * **Princip:** Podmínku spojenou operátorem `OR` lze transformovat na sjednocení dvou samostatných dotazů. Použití varianty $\cup_{max}$ (sjednocení multimnožin) zaručuje, že pokud řádek splňuje obě podmínky zároveň, objeví se ve výsledku s takovou četností, jakou měl v původní tabulce (nedojde k duplikaci).
    * $$\sigma_{p_1 \lor p_2}(R) = \sigma_{p_1}(R) \cup_{max} \sigma_{p_2}(R)$$

### 3. Pravidla pro Projekci ($\pi$)
Projekce redukuje sloupce. Kaskádové projekce umožňují ignorovat vnější (nadbytečné) projekce, pokud jsou vnitřní projekce jejich nadmnožinou.

* **Kaskádové projekce (předpoklad $X \subseteq Y$):**
    * **Princip:** Pokud provádíme více projekcí za sebou, rozhodující je pouze ta úplně vnější (poslední vykonaná). Všechny mezilehlé projekce lze ignorovat za předpokladu, že vnitřní projekce předává dál alespoň takovou množinu sloupců $Y$, jakou požaduje vnější projekce $X$.
    * $$\pi_{X}(\pi_{Y}(R)) = \pi_{X}(R)$$

### 4. Kombinace operátorů (Heuristiky pro optimalizaci)

#### A) Včasná selekce (Pushing Selections Down)
Přesunutí selekce pod operátor spojení ($\bowtie$) drasticky snižuje počet řádků vstupujících do drahé join operace.

* **Základní posun selekce pod spojení:**
    * **Princip:** Pokud se filtrační podmínka $p$ týká výhradně atributů z tabulky $R$, je výhodnější tabulku $R$ vyfiltrovat ještě předtím, než vstoupí do spojení s tabulkou $S$. Do paměťově náročného joinu tak putuje výrazně méně řádků.
    * $$\sigma_{p}(R \bowtie S) = [\sigma_{p}(R)] \bowtie S$$

* **Komplexní posun selekce pod spojení:**
    * **Princip:** Složitý filtr obsahující podmínky pro obě tabulky i samotné spojení se nejprve rozloží (pomocí kaskádového větvení). Filtry patřící čistě k tabulce $R$ ($p$) a čistě k tabulce $S$ ($q$) se propadnou přímo ke zdrojům, zatímco propojovací podmínka $m$ se vyhodnotí až nad výsledným spojením zmenšených tabulek.
    * $$\sigma_{p \land q \land m}(R \bowtie S) = \sigma_{m}[(\sigma_{p}(R)) \bowtie (\sigma_{q}(S))]$$

* **Posun selekce pod sjednocení a rozdíl:**
    * **Princip:** U sjednocení a rozdílu se filtrace aplikuje na obě vstupní relace samostatně ještě před provedením operace. Tím se sníží objem dat, která se musí na disku či v paměti slévat nebo porovnávat. U rozdílu stačí zúžit levou stranu, ale filtrace obou stran je bezpečný a efektivní standard.
    * $$\sigma_{p}(R \cup_{sum} S) = \sigma_{p}(R) \cup_{sum} \sigma_{p}(S)$$
    * $$\sigma_{p}(R - S) = \sigma_{p}(R) - S = \sigma_{p}(R) - \sigma_{p}(S)$$

#### B) Včasná projekce (Pushing Projections Down)
Odstraněním nepotřebných sloupců co nejdříve (ještě před selekcí nebo spojením) se zúží šířka řádků v paměťových bufferech.

* **Posun projekce pod selekci:**
    * **Princip:** Sloupce, které ve výsledku nechceme ($X$), můžeme odříznout už před selekcí. Do projekce hluboko ve stromu ale musíme přibalit i sloupce $Z$, které sice nechce finální výsledek, ale jsou nezbytné pro správné vyhodnocení filtrační podmínky $P$.
    * $$\pi_{X}[\sigma_{P}(R)] = \pi_{X}(\sigma_{P}[\pi_{X \cup Z}(R)])$$

* **Posun projekce pod spojení:**
    * **Princip:** Namísto propojování širokých tabulek a následného osekávání výsledku odřízneme nepotřebné sloupce přímo na zdrojích $R$ a $S$. Do vnořených projekcí musíme kromě finálně požadovaných sloupců ($X, Y$) zahrnout také množinu společných spojovacích atributů $Z$, bez kterých by operátor joinu nedokázal řádky spárovat.
    * $$\pi_{XY}(R \bowtie S) = \pi_{XY}([\pi_{X \cup Z}(R)] \bowtie [\pi_{Y \cup Z}(S)])$$

---

## Rozdělování dat (Partitioning)

Rozdělováním se velká logická tabulka rozčlení do menších, nezávisle spravovatelných fyzických pod-tabulek (particií).

### 1. Horizontální dělení (Horizontal Partitioning)
Řádky se distribuují do particií podle klíče (na základě rozsahu hodnot, definovaného seznamu nebo hašování). Hlavní výhodou je **prořezávání particií (Partition Pruning)**, kdy optimalizátor zcela ignoruje particie, které neodpovídají podmínkám v dotazu.

*Příklad:*
*Pokud tabulku objednávek rozdělíme horizontálně podle let a dotaz směřuje pouze na prosinec 2025, databáze fyzicky čte pouze partici pro rok 2025.*

### 2. Vertikální dělení (Vertical Partitioning)
Rozdělení jedné tabulky na více menších tabulek (vztah 1:1) podle sloupců. Je výhodné, pokud jsou některé sloupce dotazovány výrazně častěji nebo jsou řádově menší než zbytek tabulky (např. LOB sloupce, které se ukládají odděleně).

*Příklad z praxe:*
*Mobilní operátor eviduje u zákazníka ID, adresu a kredit: `Customer(id, address, credit)`. Kredit se mění a kontroluje mnohokrát denně, zatímco adresa se čte pouze jednou měsíčně při generování faktur. Výhodné je rozdělení na:*
* `CustAddr(id, address)`
* `CustCredit(id, credit)`
*Tabulka `CustCredit` je extrémně malá, zabírá minimum diskových bloků a může být kompletně nahrána v RAM, což dramaticky urychlí její neustálé skenování.*

### 3. Vertikální replikace (Antipartitioning)
Záměrná replikace několika málo atributů z jedné tabulky do druhé s cílem eliminovat spojení.

*Příklad z praxe:*
*Burzovní portál má tabulky `StockDetail(stock_id, company)` a `StockPrice(stock_id, date, price)`. Dotazy na aktuální cenu vyžadují drahé spojení. Pokud do `StockDetail` zavedeme redundantní sloupce `price_today` a `price_yesterday`, nejčastější dotazy vyřešíme jediným index scanem bez nutnosti joinu.*

<img alt="img.png" src="img/db/hor-ver.png" width="400"/>

---

## Ladění dotazů a schématu

Ladění (Query/Schema Tuning) reaguje na situace, kdy automatická optimalizace nestačí a exekuce je příliš pomalá. Cílem je upravit struktury nebo samotný zápis kódu tak, aby optimalizátor dokázal najít lepší cestu. K analýze se standardně používá příkaz `EXPLAIN`.

### 1. Vliv indexů na modifikace dat
Indexy výrazně urychlují `SELECT`, ale dramaticky zpomalují zápisy (`INSERT`, `DELETE`, `UPDATE`), protože DBMS musí synchronně aktualizovat i datové struktury indexu (např. B+ strom).

*Příklad z praxe:*
*U tabulky `StarsIn` provádíme dotazy podle herce* ($p_1$) *, podle filmu* ($p_2$) *a zápisy (Insert). Náklady v počtu I/O operací se mění podle konfigurace indexů:*
* *Bez indexů: Čtení = 10 I/O (Table Scan), Zápis = 2 I/O.*
* *Index na herce: Čtení podle herce = 4 I/O, Zápis = 4 I/O.*
* *Oba indexy: Čtení podle herce = 4 I/O, Čtení podle filmu = 4 I/O, Zápis = 6 I/O.*
*Zavedení obou indexů se vyplatí pouze tehdy, pokud je frekvence vyhledávání výrazně vyšší než frekvence zápisů* ($p_1, p_2 \ge 0.4$).

### 2. Eliminace zbytečných DISTINCTů
Použití `DISTINCT` nutí databázi provést drahé řazení nebo hašování (*Unique* / *HashAggregate*) pro odstranění duplicit. Často je však `DISTINCT` v dotazu nadbytečný.

* **Pravidlo privilegovanosti:** Tabulka je privilegovaná, pokud výběr (projection) obsahuje její primární klíč.
* **Pravidlo dosažitelnosti:** Neprivilegovaná tabulka $R$ "dosáhne" na $S$, pokud je s ní spojena přes rovnost svého primárního klíče.
* **Věta o duplicitách:** Výsledek dotazu garantovaně neobsahuje duplicity (a `DISTINCT` je tedy zbytečný), pokud je každá tabulka v dotazu privilegovaná, nebo pokud každá neprivilegovaná tabulka dosáhne na nějakou privilegovanou.

*Příklad z praxe:*
```sql
SELECT DISTINCT ssnum FROM employee, tech WHERE employee.dept = tech.dept;
```
*Protože `ssnum` je primární klíč v `employee` (je privilegovaná) a `dept` je klíč v `tech`, každý zaměstnanec se spojí s nejvýše jedním oddělením. Výsledek duplicity přirozeně neobsahuje a klauzule DISTINCT je nadbytečná.*

### 3. Správné využití indexů v klauzulích WHERE
Optimalizátory často nedokážou použít index, pokud je indexovaný sloupec obalen funkcí nebo matematickým výrazem:
* **Pomalé (nevyužije index):** `WHERE salary/12 >= 4000;`
* **Rychlé (využije index):** `WHERE salary >= 48000;`
* **Pomalé (nevyužije index):** `WHERE SUBSTR(name, 1, 1) = 'G';`
* **Rychlé (využije index):** `WHERE name LIKE 'G%';`

### 5. Nevhodné použití HAVING
Klauzule `HAVING` slouží výhradně pro filtrování agregačních výsledků. Pro běžnou filtraci řádků před agregací se musí zásadně používat `WHERE`, což sníží počet řádků vstupujících do drahé operace `GROUP BY`.
* **Špatně:** `SELECT avg(salary), dept FROM employee GROUP BY dept HAVING dept = 'IT';`
* **Správně:** `SELECT avg(salary), dept FROM employee WHERE dept = 'IT' GROUP BY dept;`

---

## Zpracování transakcí

Transakce tvoří logickou jednotku práce s databází a její správné zpracování garantuje vlastnosti **ACID** (Atomicita, Konzistence, Izolovanost, Trvanlivost). Zatímco pohled aplikace na transakci je takový, že běží zcela izolovaně, databáze se musí vypořádat s masivní souběžnou exekucí (concurrency).

### 1. Řízení souběhu a zamykání (Concurrency Control)
K zajištění korektnosti při paralelním běhu transakcí se využívají semafory na úrovni celé databáze (vhodné pro in-memory databáze) nebo zamykací mechanismy na úrovni stránek a řádků (vhodné pro diskové DB).
* **Zámky:** Sdílené pro čtení (Shared - S) a exkluzivní pro zápis (Exclusive - X).
* **Protokol 2PL (Two-Phase Locking):** Garantuje serializovatelnost prováděcího plánu. Má dvě fáze:
  1. *Růstová fáze:* Transakce zámky pouze získává, nesmí žádný uvolnit.
  2. *Smršťovací fáze:* Transakce zámky pouze uvolňuje, nesmí žádný nový získat.

### 2. Pravidla pro návrh a ladění transakcí (Tuning Guidelines)
Dlouho běžící transakce drží zámky po dlouhou dobu, čímž blokují ostatní transakce, zvyšují riziko uváznutí (deadlock) a snižují propustnost systému.
* **Zákaz interakce s uživatelem:** Transakce nikdy nesmí čekat na vstup od uživatele (např. kliknutí na tlačítko), během čehož by držela zámky.
* **Zamykat pouze nezbytné:** Filtrování řádků se musí provádět na straně DB serveru (přes `WHERE`), nikoliv načtením celé tabulky do aplikace a jejím postupným zamykáním.
* **Sekání transakcí (Chopping):** Pokud transakce $T$ přistupuje k prvkům $x$ a $y$ a ostatní paralelní transakce přistupují nejvýše k jednomu z nich, je výhodné rozdělit $T$ na dvě kratší transakce $T_1$ (zpracuje $x$) a $T_2$ (zpracuje $y$).
* **Uvolňování zámků dříve:** Většina DBMS uvolňuje čtecí (S) zámky ihned po dokončení konkrétního I/O čtení, nečeká se na commit (oslabení izolace).

### 3. Úrovně izolace vs. Anomálie
SQL standard definuje 4 úrovně izolace transakcí na základě toho, jakým anomáliím (nežádoucím jevům při souběhu) předchází:

* **Read Uncommitted:** Nejnižší úroveň, nepoužívá téměř žádné zamykání pro čtení. Povoluje anomálii **Dirty Read** (čtení nepotvrzených dat jiné aktivní transakce).
* **Read Committed:** Čtecí zámek se uvolňuje ihned po přečtení dat. Zabraňuje *Dirty Read*, ale povoluje **Non-repeatable Read** (opakované čtení stejného řádku v téže transakci vrátí jiné hodnoty, protože je jiná transakce mezitím změnila a potvrdila).
* **Repeatable Read:** Čtecí zámky se drží až do konce transakce. Zabraňuje předchozím anomáliím, ale povoluje **Phantom Read** (opakovaný rozsahový dotaz v téže transakci vrátí nové, mezitím vložené řádky od jiné transakce).
* **Serializable:** Nejvyšší úroveň, exekuce je ekvivalentní sériovému spuštění transakcí za sebou. Zabraňuje všem anomáliím.

<img alt="img.png" src="img/db/isaloation-level.png" width="500"/>

---

## Výpadky a zotavení

Systém zotavení (Recovery Manager) zajišťuje atomicitu (vrácení nedokončených změn) a trvanlivost (zápis potvrzených změn) transakcí po nečekaném výpadku proudu nebo pádu operačního systému.

### 1. Write-Ahead Logging (WAL)
Základní pravidlo pro zachování konzistence dat na disku: **Žádný datový blok nesmí být zapsán do primární databáze na disk dříve, než je příslušný záznam o jeho změně bezpečně uložen a synchronizován (flushed) v transakčním logu.**

### 2. Strategie správy bufferu (Buffer Management)
Správce paměti (Buffer Manager) rozhoduje o tom, kdy se špinavé (změněné) stránky z RAM bufferu přepisují do primárního souboru na disku. Kombinace těchto strategií určuje náročnost a nutnost zotavení:

* **Steal vs. No-Steal (Rozhoduje o potřebě fáze UNDO):**
    * **Steal:** Systém smí zapsat na disk změny provedené dosud nepotvrzenou transakcí (např. když dojde RAM a je potřeba uvolnit místo). Pokud systém po tomto zápisu spadne, data na disku jsou nekonzistentní. Vyžaduje fázi **UNDO** při pádu (vrácení změn nedokončených transakcí).
    * **No-Steal:** Systém nesmí zapsat necommitované změny na disk. Vše drží v RAM, dokud transakce neskončí. Fáze UNDO pak při pádu není nutná, ale systém je limitován velikostí RAM.
* **Force vs. No-Force (Rozhoduje o potřebě fáze REDO):**
    * **Force:** Všechny změny musí být zapsány na disk okamžitě při potvrzení (commit) transakce. Databáze nepustí uživatele dál, dokud neproběhne zápis do datových souborů. Zaručuje trvanlivost, ale má katastrofální výkon kvůli neustálému náhodnému zápisu (Random I/O). Fáze REDO není nutná.
    * **No-Force:** Změny commitované transakce mohou zůstat v RAM bufferu a zapíšou se na disk asynchronně někdy později. Šetří diskové operace, ale pokud systém spadne, potvrzená data v RAM se ztratí. Vyžaduje fázi **REDO** při pádu (znovupřehrání změn z logu).

Moderní vysoce výkonné databáze používají kombinaci **Steal / No-Force** (maximální volnost a rychlost pro správu paměti), což znamená, že proces zotavení musí bezpodmínečně provádět jak fázi REDO, tak fázi UNDO.

<img alt="img.png" src="img/db/force-steal.png" width="400"/>

Vztah mezi politikami správy bufferu, výkonem systému a nároky na zotavení dokonale popisuje následující přehled:

* **Kvadrant [No-Steal / Force] – Nejpomalejší (Slowest) / No UNDO, No REDO:**
    * Paměťový buffer má svázané ruce. Nesmí zapsat nepotvrzená data na disk (No-Steal) a zároveň musí při každém commitu okamžitě a synchronně zapsat všechna změněná data do datových souborů (Force).
    * Pokud systém spadne, na disku garantovaně nejsou žádná data z nedokončených transakcí (netřeba UNDO) a zároveň jsou na něm bezpečně všechna data z potvrzených transakcí (netřeba REDO). Databáze nepotřebuje žádný složitý log.
    * Katastrofální výkon. Neustálé nucené zápisy na disk při každém commitu způsobují masivní I/O úzké hrdlo.

* **Kvadrant [Steal / Force] – Pouze UNDO / No REDO:**
    * Buffer smí přemazat necommitovanou stránku na disku (Steal), ale při commitu musí vše hned zapsat (Force).
    * Na disku se mohou ocitnout data transakcí, které nakonec spadly. Je nutné čistit a provádět fázi **UNDO**. Fáze REDO netřeba.

* **Kvadrant [No-Steal / No-Force] – No UNDO / Pouze REDO:**
    * Buffer drží nepotvrzená data striktně v RAM (No-Steal), ale po commitu dovolí transakci běžet dál bez okamžitého zápisu dat na disk (No-Force).
    * Na disku nikdy nejsou nečistá data (netřeba UNDO). Potvrzená data ale mohla zůstat jen v RAM a při pádu zmizet. Je nutné je znovu přehrát ze záznamů v logu – fáze **REDO**.

* **Kvadrant [Steal / No-Force] – Nejrychlejší (Fastest) / Vyžaduje UNDO i REDO:**
    * Buffer má absolutní svobodu. Může na disk zapsat kdykoliv cokoliv (Steal) a odkládat zápisy potvrzených dat na neurčito (No-Force).
    * Při pádu je na disku chaos – jsou tam změny, které tam být nemají, a chybí tam změny, které tam být mají. Systém obnovy musí bezpodmínečně umět obojí: **UNDO i REDO** (např. algoritmus ARIES).
    * Maximální možná rychlost. Databáze zapisuje na disk na pozadí, asynchronně a v optimálních dávkách. Právě proto tento přístup používají všechny moderní produkční databáze.

### 3. Typy logování a algoritmus ARIES
V závislosti na tom, jaké informace o změnách do logu ukládáme a jak s nimi pracujeme při obnově, rozlišujeme tři základní přístupy:

* **A) UNDO Logování:**
    * **Obsah záznamu:** `[ID_transakce, ID_objektu, stará_hodnota]` (předchozí stav dat).
    * **Zápis:** Vyžaduje strategii *No-Force*. Stará data musí být zapsána na disk ještě před commitem transakce.
    * **Zotavení:** Při pádu se log prochází **od konce dozadu** a u transakcí, které nestihly commit, se vrátí původní `stará_hodnota`.
* **B) REDO Logování:**
    * **Obsah záznamu:** `[ID_transakce, ID_objektu, nová_hodnota]` (nový stav dat).
    * **Zápis:** Vyžaduje strategii *No-Steal*. Změny se zapisují na disk až po commitu (resp. log se synchronizuje na disk při commitu).
    * **Zotavení:** Při pádu se log prochází **od začátku dopředu** a u všech potvrzených transakcí se znovu vynutí `nová_hodnota`.
* **C) Zpětně-dopředné logování (UNDO/REDO) – Algoritmus ARIES:**
    * Standard moderních vysoce výkonných DBMS splňující strategii *Steal / No-Force*. Sdružuje výhody obou přístupů a odstraňuje jejich paměťová omezení.
    * **Obsah záznamu:** `[ID_transakce, ID_objektu, stará_hodnota, nová_hodnota]`
    * Každý záznam v logu obsahuje monotónně rostoucí číslo **LSN (Log Sequence Number)** a ukazatel na předchozí operaci stejné transakce (`prevLSN`). Každá datová stránka na disku si v hlavičce nese políčko `pageLSN` (ID posledního zapsaného logu na této stránce) pro zajištění idempotence (ochrana před duplicitním zápisem).

Po restartu systému po havárii provádí **ARIES** zotavení ve třech po sobě jdoucích fázích:

1. **Analytická fáze (Analysis):**
    * Prochází log směrem **dopředu** od posledního kontrolního bodu (Checkpointu).
    * Identifikuje aktivní transakce v momentě pádu (tzv. *Losers*, které nestihly udělat commit a bude nutné je vrátit) a špinavé stránky v bufferu.
2. **Fáze REDO (Zopakování / History Repeating):**
    * Prochází log směrem **dopředu** a znovu aplikuje veškeré změny **všech** transakcí (potvrzených i aktivních *Losers*). Tím uvede databázi přesně do stavu, v jakém byla sekundu před pádem.
    * Pokud je LSN v logu menší nebo rovno `pageLSN` na disku, operace se přeskočí (změna už na disku bezpečně byla).
3. **Fáze UNDO (Vrácení zpět):**
    * Prochází log směrem **dozadu** od konce.
    * Odstraňuje změny všech transakcí označených jako *Losers* (aktivní při pádu) zápisem jejich původních `starých_hodnot`.
    * Za každou vrácenou operaci zapíše do logu speciální dopředný záznam **CLR (Compensation Log Record)**. Pokud by systém spadl znovu uprostřed zotavování, díky CLR ví, které změny už jednou úspěšně vrátil a nemusí je rollbovat podruhé.
---

### Komplexní příklad zotavení (Analýza, REDO, UNDO)

Mějme na disku tabulku s hodnotami $A=10, B=20$. Spustí se dvě transakce:
* $T_1$ upraví $A$ na $100$.
* $T_2$ upraví $B$ na $200$.
* $T_1$ úspěšně provede `COMMIT`.
* V tom okamžiku dojde k **pádu systému** (výpadek proudu). Transakce $T_2$ zůstala nedokončená.

#### Stav logu v momentě pádu:
```text
LSN 101: [T1, A, stará=10, nová=100]
LSN 102: [T2, B, stará=20, nová=200]
LSN 103: [T1, COMMIT]
<<< PÁD SYSTÉMU >>>
```
LSN (= Log Sequence Number)

#### Průběh zotavení:

1. **Analýza (Dopředu):** Optimalizátor přečte log od začátku (resp. od checkpointu). Narazí na start transakcí $T_1$ a $T_2$. Na konci logu zjistí, že $T_1$ má zaznamenaný `COMMIT`, ale $T_2$ ne. 
   * **Výsledek fáze:** $T_1$ je potvrzená, $T_2$ je označen jako **Loser** určený k vrácení.

2. **REDO fáze (Dopředu):**
   Systém vezme log od nejstaršího záznamu dopředu a slepě replikuje historii (opakuje všechny změny):
   * Podle záznamu LSN 101 zapíše do $A$ hodnotu $100$.
   * Podle záznamu LSN 102 zapíše do $B$ hodnotu $200$.
   * *Na konci této fáze je databáze v přesném stavu, v jakém se nacházela v momentě pádu (tedy $A=100, B=200$).*

3. **UNDO fáze (Dozadu):**
   Systém čistí změny po aktivní transakci $T_2$. Prochází log směrem pozpátku:
   * Narazí na záznam LSN 102 (patřící $T_2$). Vezme jeho hodnotu `stará=20` a zapíše ji do $B$.
   * Do logu okamžitě zapíše kompenzační záznam: `LSN 104: [CLR: T2, B, stará=20]`, který odkazuje na `prevLSN` této transakce (zde žádný předchozí není, takže $T_2$ je kompletně vrácena).
   * Narazí na záznam LSN 101 (patřící $T_1$). Protože transakce $T_1$ není loser, tento záznam zcela ignoruje.

**Finální konzistentní stav na disku po zotavení:** $A=100$ (změna z potvrzené $T_1$ byla zachována), $B=20$ (nedokončená změna z $T_2$ byla bezpečně odvolána).

---

### 5. Kontrolní body (Checkpoints)
Periodická operace, která zkracuje čas zotavení po havárii. Bez checkpointů by databáze musela při restartu analyzovat transakční log od úplného prvního řádku (který mohl vzniknout před měsíci).

* **Princip:** Během checkpointu se aktuální špinavé stránky z RAM bufferu zapíšou na disk a do logu se zapíše záznam o stavu aktivních transakcí. Při následném pádu víme, že všechny změny zapsané před checkpointem jsou bezpečně na disku, a analýzu logu proto nemusíme provádět od úplného začátku souboru logu.

#### Typy Checkpointů:
* **Strict (Ostrý) Checkpoint:** Zastaví se zpracování všech transakcí (přeruší se zápisy), všechny špinavé stránky z RAM se synchronně spláchnou (flush) na disk a až pak se pokračuje. Způsobuje nepříjemné výkonnostní skoky a propady propustnosti (throughput).
* **Fuzzy Checkpoint:** Nebrání běhu transakcí. Do logu se zapíše `[BEGIN_CHECKPOINT]`, na pozadí se začnou asynchronně zapisovat špinavé stránky na disk. Jakmile jsou zapsány, zapíše se `[END_CHECKPOINT]` s tabulkou transakcí (Transaction Table) a tabulkou špinavých stránek (Dirty Page Table), které byly v momentě začátku checkpointu aktivní. Analytická fáze zotavení pak začíná na záznamu `BEGIN_CHECKPOINT`.

---

## Bezpečnost, přístupová oprávnění

Bezpečnostní subsystém se zaměřuje na ochranu před neautorizovaným přístupem a zneužitím informací zvenčí. Bezpečnost v databázových systémech (DBMS) se opírá o kontrolu přístupu, která určuje, kteří uživatelé mají oprávnění k jakým datům a operacím.

V moderních databázových systémech se k řízení přístupu využívají dva hlavní koncepční modely:

* **Diskreční řízení přístupu (DAC – Discretionary Access Control):** V tomto modelu má vlastník objektu (zpravidla jeho tvůrce) plnou kontrolu nad právy k tomuto objektu. Pomocí standardních SQL příkazů může udělovat nebo odebírat oprávnění ostatním subjektům v systému.
    * **GRANT:** Příkaz slouží k přidělení specifických práv (např. `SELECT`, `INSERT`, `UPDATE`, `DELETE`) nad konkrétním objektem vybranému uživateli nebo roli. Pomocí klauzule `WITH GRANT OPTION` může příjemce toto právo legálně delegovat na další uživatele.
    * **REVOKE:** Příkaz slouží k odnětí dříve udělených práv. Pokud je právo odebráno uživateli, který jej distribuoval dále pomocí `WITH GRANT OPTION`, dochází v závislosti na nastavení systému k řetězovému odebrání práv (kaskádovému efektu) u všech takto ovlivněných uživatelů.
* **Řízení přístupu na základě rolí (RBAC – Role-Based Access Control):** Tento model výrazně zjednodušuje správu oprávnění ve větší organizaci. Namísto přidělování práv jednotlivým uživatelům se oprávnění naváží na logické entity zvané **role** (např. *role `manažer`, `analytik`, `účetní`*), které reprezentují pracovní funkce. Uživatelům jsou následně tyto role pouze přiřazovány, což minimalizuje riziko lidské chyby a usnadňuje auditování přístupu.

Z hlediska granularity se bezpečnost vynucuje na různých úrovních. Kromě klasického zabezpečení celých tabulek či schémat lze využít pohledy (`VIEWS`), které uživateli zpřístupní pouze vybranou podmnožinu sloupců a řádků, čímž efektivně skrývají citlivá data bez nutnosti měnit fyzickou strukturu podkladových tabulek. Pokročilé systémy pak integrují přímé řízení na úrovni řádků (**Row-Level Security – RLS**), kde jsou dotazy uživatelů na pozadí automaticky modifikovány o bezpečnostní predikáty. *Příklad: V tabulce `faktury` uvidí obchodní zástupce přes RLS automaticky pouze ty řádky, kde sloupec `id_obchodnika` odpovídá jeho přihlašovacímu ID.*

<img alt="img.png" src="img/db/overview.png" width="600"/>
