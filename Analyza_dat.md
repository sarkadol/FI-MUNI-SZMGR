# Analýza dat

> Datové sklady a jejich životní cyklus, zúžené datové sklady (data marts), 
> dimezionální model a jeho implementace (star schema, data cube). 
> Proces extrakce, transformace a nahrávání dat (ETL), 
> profilování dat, datová integrita, kvalita dat.

## Datové sklady

### Definice a vlastnosti datového skladu (Inmon)
* **Subject-oriented (Subjektově orientovaný):** Data jsou organizována kolem klíčových témat/subjektů podniku (např. zákazník, produkt, prodej), nikoliv kolem provozních aplikací.
* **Integrated (Integrovaný):** Data z různých zdrojů jsou sjednocena, vyčištěna a standardizována (jednotné kódování, formáty, měrné jednotky).
* **Time-varying / Time-variant (Časově proměnný):** Data jsou vždy svázána s časovým údajem a zachycují historii (na rozdíl od OLTP, které reflektuje aktuální stav). Každý klíč v DWH musí přímo nebo nepřímo obsahovat časový prvek.
* **Non-volatile (Stálý/Neměnný):** Jakmile jsou data do skladu nahrána, jsou pouze čtena. Nedochází k jejich běžným modifikacím (`UPDATE`) nebo mazání (`DELETE`) ze strany uživatelů.

### Separace OLTP a OLAP
Hlavním důvodem vzniku DWH je oddělení provozních systémů od analytických:
* **OLTP (Operational):** Podpora každodenních byznys operací. Rychlé, detailní a izolované transakce nad aktuálními daty. Vysoký souběh uživatelů.
* **OLAP (Informational):** Podpora rozhodování a plánování. Komplexní analytické dotazy nad historickými, agregovanými daty. Nízký souběh, ale extrémní náročnost na systémové prostředky.

---

## Životní cyklus datového skladu

Proces zpracování a toku dat sestává z následujících architektonických komponent:
1.  **Source Systems (Zdrojové systémy):** Transakční OLTP databáze, ERP, CRM, ploché soubory atd.
2.  **Data Staging Area (Dočasné úložiště):** Transitní prostor mimo produkční sklad. Surová data jsou zde čištěna, transformována a integrována. Uživatelské dotazy jsou do staging area zakázány, operace probíhají striktně sekvenčně. Není vyžadováno zamykání řádků ani detailní transakční logování.
3.  **Data Warehouse Storage:** Centrální perzistentní vrstva ukládající vyčištěná atomická historická data.
4.  **Information Delivery / Presentation:** Vrstva zpřístupňující data koncovým uživatelům (OLAP kostky, Data Marts, vizualizace v Power BI).

---

## Zúžené datové sklady (Data Marts)
* Zúžený datový sklad (Data Mart) je specializovaná podmnožina dat z celopodnikového datového skladu.
* Je přizpůsoben požadavkům konkrétní skupiny uživatelů nebo specifickému byznys oddělení (např. marketing, finance, lidské zdroje).
* Obsahuje data optimalizovaná (často předagregovaná a denormalizovaná) pro konkrétní analytické úlohy daného oddělení, což zvyšuje rychlost odezvy dotazů.

### Architektonické přístupy k návrhu
* **Top-down (Inmon):** Nejdříve se vybuduje centralizovaný, plně integrovaný podnikový datový sklad (Enterprise Data Warehouse) v normalizované formě (3NF). Z tohoto centrálního zdroje se následně generují jednotlivé závislé Data Marts pro koncové uživatele.
* **Bottom-up (Kimball):** Datový sklad je budován inkrementálně. Nejdříve se navrhnou a vytvoří jednotlivé Data Marts reprezentující konkrétní byznys procesy v dimenzionálním formátu. Tyto Data Marts jsou následně integrovány do jednoho celku pomocí sdílených, standardizovaných dimenzí (**Conformed Dimensions**).

---

## Dimenzionální model a jeho implementace (star schema, data cube)

Dimenzionální modelování rozděluje data do dvou typů tabulek:
* **Tabulka faktů (Fact Table):** Centrální tabulka obsahující numerická měření, metriky nebo fakta popisující konkrétní byznys události (např. prodané množství, tržba). Obsahuje cizí klíče směřující do tabulek dimenzí.
    * *Granularita (Grain):* Určuje úroveň detailu zachycenou v jednom řádku tabulky faktů (např. jednotlivá položka transakce vs. denní sumář).
    * *Aditivita:* Fakta mohou být plně aditivní (lze je sčítat přes všechny dimenze), semi-aditivní (lze sčítat jen přes některé dimenze, např. stav skladu nelze sčítat přes časovou dimenzi) nebo neaditivní (poměrové ukazatele).
* **Tabulka dimenzí (Dimension Table):** Obklopuje tabulku faktů. Obsahuje textové a popisné atributy, které definují kontext faktů (kdo, co, kde, kdy, proč).
    * *Surogátní klíče (Surrogate Keys):** Uměle generované unikátní celočíselné identifikátory (integery) používané jako primární klíče dimenzí namísto přirozených klíčů z produkčních systémů. Důvody použití: nezávislost na změnách ve zdrojových systémech, vyšší výkon při operacích `JOIN`, podpora sledování historie.
    * *Pomalu se měnící dimenze (Slowly Changing Dimensions - SCD):* Technika správy změn atributů v čase:
        * *SCD Typ 1:* Přepis (Overwrite) – stará hodnota je přepsána novou. Historie se nezachovává.
        * *SCD Typ 2:* Přidání řádku (Add new row) – vytvoří se nový záznam s novým surogátním klíčem a časovým označením platnosti (`Valid From`, `Valid To`). Plně zachovává historii.
        * *SCD Typ 3:* Přidání sloupce (Add new column) – stará hodnota se přesune do sloupce určeného pro předchozí hodnotu. Zachovává omezenou historii.

### Implementace: Hvězdné schéma (Star Schema)
* Centrální tabulka faktů je přímo propojena s tabulkami dimenzí pomocí relací `1:N`.
* Tabulky dimenzí jsou záměrně **denormalizované** (obsahují redundanci), což minimalizuje počet potřebných propojení (`JOIN`) při dotazování a maximalizuje rychlost čtení.
* Alternativou je **Vločkové schéma (Snowflake Schema)**, kde jsou dimenze normalizovány do více úrovní, což sice šetří místo na disku, ale komplikuje a zpomaluje analytické dotazy.

### Implementace: Datová kostka (Data Cube / OLAP Cube)
* Symbolická vícedimenzionální reprezentace agregovaných dat odvozená z dimenzionálního modelu.
* Kostka se skládá z buněk (cells), které jsou definovány kombinací hodnot jednotlivých dimenzí. Buňka obsahuje předpočítané hodnoty faktů.
* *Hustota kostky:* Kostka může být hustá (dense - většina kombinací dimenzí má data) nebo řídká (sparse - mnoho buněk je prázdných, např. ne všechny produkty se prodávají v každém obchodě každý den). S rostoucím počtem a velikostí dimenzí se kostky stávají řidšími.

#### Základní operace nad datovou kostkou
* **Roll-up (Agregace):** Přechod od detailnějších dat k obecnějším, sumarizace podél hierarchie dimenze (např. z úrovně *Den* na úroveň *Měsíc* nebo z *Město* na *Stát*).
* **Drill-down (Rozpad):** Opak operace roll-up. Přechod od sumárních dat k detailnějším, odhalení podrobnějších struktur (např. z *Rok* na *Kvartál*).
* **Slice (Řez):** Výběr jedné konkrétní hodnoty jedné dimenze, čímž dojde k redukci dimenzionality (vznikne podmožina dat, např. "pouze data pro rok 2026").
* **Dice (Výřez/Kostka):** Výběr sub-kostky definováním omezujících podmínek (predikátů) na více dimenzích současně.
* **Pivot (Otočení):** Změna vizuální orientace zobrazení dat (např. otočení os v tabulce, záměna řádků za sloupce).

---

## Proces extrakce, transformace a nahrávání dat (ETL)

ETL představuje páteřní proces integrace dat z různých zdrojů do DWH a tvoří nejnákladnější a nejkomplexnější část implementace (často až 80 % času vývoje).

### 1. Extraction (Extrakce)
* Fáze získávání dat z heterogenních zdrojových systémů.
* Data jsou zkopírována 1:1 do dočasného úložiště (Staging Area).
* **Incremental Approach (Přírůstkové nahrávání):** Z důvodu úspory času a minimalizace zátěže produkčních systémů se stahují pouze změněná data (delty). Technika se označuje jako **Change Data Capture (CDC)** a může být realizována pomocí časových razítek (`timestamp`), triggerů nebo přímým čtením transakčních logů databáze.

### 2. Transformation (Transformace)
* Fáze úpravy dat do jednotného formátu kompatibilního se schématem DWH.
* *Čištění dat (Cleansing):* Odstranění duplicit, opravy nevalidních hodnot, standardizace formátů a kódování (např. sjednocení zápisu pohlaví na jednotné 'M'/'F').
* *Generování klíčů:* Nahrazení přirozených klíčů klíči surogátními a správa verzí u pomalu se měnících dimenzí (SCD).
* *Komplexní transformace a byznys logika:* Výpočet odvozených atributů, kalkulace metrik a agregací.

### 3. Loading (Nahrávání)
* Fáze zápisu zpracovaných dat do finálních struktur (tabulek faktů a dimenzí) v DWH.
* Při prvotním plnění se provádí **Initial Load** (masivní zápis historických dat). Při běžném provozu probíhá **Incremental Load** (pravidelný zápis nových přírůstků).
* *Optimalizace výkonu při nahrávání:* * Využití hromadného nahrávání (**Bulk Load**) namísto standardních `INSERT` příkazů po jednotlivých řádcích.
    * Dočasné vypnutí nebo dropování indexů a integritních omezení (constraints) před nahráváním a jejich následné znovuvytvoření (rebuild) po dokončení importu.
    * Výpočet pre-agregací a aktualizace materializovaných pohledů.

---

## Profilování dat
* Profilování dat je analytické zkoumání datových sad ze zdrojových systémů před jejich integrací do datového skladu.
* Účelem je získat hluboké porozumění skutečné struktuře, obsahu, závislostem a anomáliím v datech. Pomáhá odhalit skryté problémy dříve, než selže ETL proces.

### Klíčové techniky profilování dat
* **Strukturální analýza (Structure Analysis):** Ověřování formálního formátu dat (např. zda délka řetězců odpovídá definici schématu, detekce nečekaných `NULL` hodnot v polích, která by měla být povinná).
* **Obsahová analýza (Content Analysis):** Detailní pohled na jednotlivé hodnoty atributů.
    * *Frekvenční distribuce:* Analýza četnosti výskytu jednotlivých hodnot, která odhaluje nekonzistence (např. zápis jedné země jako "CZ", "CZE", "Czechia").
    * *Statistické shrnutí:* Výpočet minimálních, maximálních hodnot, průměrů a směrodatných odchylek pro numerická pole, což umožňuje detekovat odlehlé hodnoty (outliers, např. záporný věk nebo nereálně vysoká prodejní cena).
* **Vztahová analýza (Relationship Analysis):** Zkoumání vazeb napříč tabulkami. Identifikace implicitních vztahů a ověřování, zda klíče mezi tabulkami skutečně lícují, čímž se zjišťuje stav referenční integrity v reálných datech (bez ohledu na to, zda je deklarována na úrovni databázového schématu).

---

## Datová integrita

Datová integrita zajišťuje, že data uložená v databázi jsou přesná, platná, vnitřně konzistentní a že odpovídají sadě definovaných strukturálních pravidel. V prostředí DWH je její striktní vynucení kritické, protože analýzy pracují s daty z mnoha různých systémů.

* **Entitní integrita (Entity Integrity):** Každá tabulka musí mít definovaný primární klíč (v DWH typicky surogátní), který jednoznačně identifikuje každý řádek a nesmí obsahovat hodnotu `NULL`.
* **Referenční integrita (Referential Integrity):** Zajišťuje správnost a existenci vztahů mezi tabulkami. Cizí klíč v tabulce faktů musí vždy odkazovat na existující primární klíč v tabulce dimenze. 
    * *Řešení porušení v ETL:* Pokud v tabulce faktů existuje záznam s klíčem, který v dimenzi chybí, ETL nesmí záznam zahodit (způsobilo by to ztrátu finančních reportů). Namísto toho se záznam naváže na speciální předem připravený řádek v dimenzi s identifikátorem např. `-1` (reprezentující hodnotu "Neznámý/Neuvedený produkt").
* **Doménová integrita (Domain Integrity):** Všechny hodnoty v daném sloupci musí splňovat definovaný typ, formát a povolený rozsah hodnot (např. věk musí být kladné celé číslo v rozmezí 0–120, datum musí odpovídat reálnému kalendáři).

---

## Kvalita dat

* Kvalita dat určuje míru vhodnosti dat pro byznys rozhodování. 
* V prostředí Business Intelligence platí pravidlo **GIGO (Garbage In, Garbage Out)** – i ten nejlepší analytický model nebo report poskytne chybné a zavádějící výsledky, pokud jsou vstupní data nekvalitní.

### Dimenze kvality dat (Data Quality Dimensions)
Kvalita se měří a vyhodnocuje pomocí několika standardních dimenzí:
* **Přesnost (Accuracy):** Míra, do jaké uložená data věrně popisují reálné objekty nebo události (např. správně zapsané jméno a příjmení bez překlepů).
* **Úplnost (Completeness):** Indikuje, zda jsou přítomny všechny byznysově vyžadované datové prvky (např. zda u zákaznického profilu nechybí PSČ nebo kontaktní údaj).
* **Konzistence (Consistency):** Zajišťuje, že identická informace nemá v různých částech systému (nebo v různých systémech) protichůdné hodnoty (např. celkový reporting obratu musí vykazovat stejné číslo v reportu pro logistiku i v reportu pro finance).
* **Unikátnost (Uniqueness):** Každá entita nebo událost je v databázi zachycena právě jednou. Detekce a eliminace duplicit (např. situace, kdy je jeden zákazník v systému zaveden dvakrát pod mírně odlišnými ID).
* **Aktuálnost (Timeliness):** Určuje, zda jsou data k dispozici v čase, kdy jsou potřeba pro rozhodování. Zpoždění dat nesmí znehodnotit jejich informační hodnotu (např. report pro denní plánování skladových zásob nemůže pracovat s týden starými daty).
* **Validita / Shoda s pravidly (Validity):** Míra, do jaké data odpovídají definovaným syntaktickým standardům, vzorům a regulím (např. e-mailová adresa musí obsahovat znak `@`, rodné číslo musí splňovat dělitelnost jedenácti).