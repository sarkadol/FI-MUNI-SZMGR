# Analýza dat (PA220)

## Popis okruhu

Datové sklady a jejich životní cyklus, zúžené datové sklady (data marts), dimenzionální model a jeho implementace (star schema, data cube). Proces extrakce, transformace a nahrávání dat (ETL), profilování dat, datová integrita, kvalita dat.

## Témata

### Datové sklady a jejich životní cyklus

#### Definice a architektura datového skladu
- Datový sklad (Data Warehouse, DW): centrální úložiště integrovaných historických dat pro analytické účely
- Charakteristiky (W.H. Inmon): subject-oriented, integrated, non-volatile, time-variant
- Architektura: zdrojové systémy → ETL → staging area → datový sklad → datová tržiště → BI nástroje

#### Životní cyklus datového skladu
- Plánování a analýza požadavků: identifikace business otázek a klíčových metrik
- Datové modelování: dimenzionální vs. normalizovaný model
- ETL design a implementace
- Testování a validace dat
- Nasazení a provoz
- Evoluce: přidávání nových zdrojů, dimenzí, metrik

### Zúžené datové sklady (Data Marts)
- Definice: část datového skladu zaměřená na konkrétní oblast nebo oddělení
- Typy data martů:
  - **Závislý (dependent):** odvozen z centrálního datového skladu
  - **Nezávislý (independent):** přímé načítání ze zdrojových systémů
  - **Hybridní:** kombinace obou přístupů
- Výhody: rychlejší implementace, nižší složitost, zaměření na konkrétní uživatele
- Nevýhody: riziko nekonzistence mezi data marty, duplicita dat

### Dimenzionální model

#### Základní koncepty
- Fakta (facts): měřitelné číselné hodnoty (tržby, počty, náklady)
- Dimenze (dimensions): kontextové atributy k faktům (čas, produkt, zákazník, místo)
- Granularita (grain): nejjemnější úroveň detailu v tabulce faktů

#### Star Schema (Hvězdicové schéma)
- Centrální tabulka faktů obklopená tabulkami dimenzí
- Denormalizované dimenze: rychlé dotazy díky menšímu počtu JOIN operací
- Výhody: jednoduché dotazy, srozumitelné pro uživatele
- Nevýhody: redundance dat v dimenzích

#### Snowflake Schema
- Normalizované dimenze: dimenze rozděleny do více tabulek
- Výhody: menší redundance, snadnější udržitelnost
- Nevýhody: složitější dotazy s více JOINy

#### Data Cube (Datová kostka)
- Vícerozměrná reprezentace dat pro OLAP analýzu
- Osy kostky = dimenze; buňky = fakta
- OLAP operace:
  - **Roll-up:** agregace na vyšší úrovni granularity (den → měsíc → rok)
  - **Drill-down:** deagregace na nižší úroveň granularity
  - **Slice:** výběr jedné hodnoty dimenze (fixování jedné osy)
  - **Dice:** výběr podkostky (filtrování více dimenzí)
  - **Pivot:** přeuspořádání dimenzí

### Proces ETL (Extrakce, Transformace, Načítání)

#### Extrakce (Extraction)
- Zdroje dat: relační databáze, flat files (CSV, XML, JSON), API, legacy systémy
- Typy extrakce:
  - Plná extrakce (full extraction)
  - Inkrementální extrakce (delta extraction): change data capture (CDC), timestamps, triggers

#### Transformace (Transformation)
- Čištění dat: oprava chyb, normalizace hodnot, řešení null hodnot
- Integrace: standardizace formátů, rozlišení entit (entity resolution)
- Agregace a výpočet odvozených atributů
- Surrogate key generování: náhradní klíče dimenzí
- Slowly Changing Dimensions (SCD): Type 1 (přepis), Type 2 (nový řádek), Type 3 (nový sloupec)

#### Načítání (Loading)
- Full load vs. incremental load
- Bulk loading pro výkon
- Staging area a dočasné tabulky

### Profilování dat
- Definice: analýza dat za účelem pochopení jejich obsahu, struktury a kvality
- Analýza sloupců: distribuce hodnot, min/max, null hodnoty, unikátní hodnoty, kardinalita
- Analýza závislostí: funkční závislosti mezi atributy
- Analýza vzorů: formáty (e-mail, telefon, datum)
- Nástroje: OpenRefine, Apache Griffin, Talend Data Quality, dbt tests

### Datová integrita a kvalita dat

#### Dimenze kvality dat
- **Přesnost (Accuracy):** data odpovídají skutečnosti
- **Úplnost (Completeness):** absence null hodnot, chybějících záznamů
- **Konzistence (Consistency):** data jsou konzistentní napříč systémy a v čase
- **Aktuálnost (Timeliness):** data jsou k dispozici v požadovaném čase
- **Unikátnost (Uniqueness):** absence duplicit
- **Platnost (Validity):** data odpovídají definovaným formátům a pravidlům

#### Řízení kvality dat
- Data quality rules: SQL asserty, validační schémata
- Data lineage: sledování původu a transformací dat
- Master Data Management (MDM): správa klíčových entit (zákazník, produkt)
- Data governance: politiky, vlastnictví dat, odpovědnosti
