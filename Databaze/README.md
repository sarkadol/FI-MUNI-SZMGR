# Databáze (PA152)

## Popis okruhu

Ukládání dat, adresování záznamů. Indexování a hašování více atributů, rastrové (bitmap) indexy, dynamické hašování. Vyhodnocování dotazu a algoritmy, statistiky a odhady nákladů. Optimalizace dotazů a schémat, pravidla pro transformaci dotazů, rozdělování dat. Ladění dotazů a schématu. Zpracování transakcí, výpadky a zotavení. Bezpečnost, přístupová oprávnění.

## Témata

### Ukládání dat a adresování záznamů
- Fyzická organizace dat na disku: bloky, stránky, extenty
- Záznamy pevné a proměnné délky
- Haldové soubory (heap files), seřazené soubory
- Adresování záznamů: RID (Record ID), TID (Tuple ID), stránkové offsety
- Správa vyrovnávací paměti (buffer pool): politiky náhrady (LRU, MRU, Clock)

### Indexování a hašování více atributů

#### Jednoatributové indexy
- B+ stromy: struktura, vkládání, mazání, vyhledávání
- Hašovací tabulky: statické a dynamické hašování

#### Indexování více atributů
- Složené (kompozitní) indexy v B+ stromech
- Vícerozměrné indexy: R-stromy, kd-stromy, grid files, quad-stromy
- Použití pro prostorové dotazy

#### Rastrové (bitmap) indexy
- Princip bitmapového indexu: bitový vektor pro každou hodnotu atributu
- Operace AND, OR, NOT nad bitmapami
- Komprimace bitmapových indexů (WAH, BBC)
- Vhodnost pro atributy s nízkou kardinalitou

#### Dynamické hašování
- Rozšiřitelné hašování (extensible hashing): adresář a buckety
- Lineární hašování (linear hashing): postupné rozštěpení bucketů
- Srovnání statického a dynamického hašování

### Vyhodnocování dotazů a algoritmy

#### Algoritmy pro relační operace
- Výběr (selection): lineární scan, index scan
- Projekce: třídění a eliminace duplikátů
- Spojení (join): nested-loop join, block nested-loop join, index nested-loop join, sort-merge join, hash join
- Seskupení (grouping) a agregace

#### Statistiky a odhady nákladů
- Statistiky katalogů: počet záznamů, počet stránek, počet distinct hodnot, histogramy
- Selektivita predikátů a odhad velikosti výsledku
- Modely nákladů I/O a CPU operací

### Optimalizace dotazů a schémat

#### Pravidla pro transformaci dotazů
- Pushdown projekcí a selekcí
- Komutativita a asociativita spojení
- Přepis poddotazů (subquery rewriting)
- Materialized views a jejich využití při optimalizaci

#### Plánování dotazů
- Prohledávání prostoru plánů: heuristiky, dynamické programování, genetické algoritmy
- Operátorové stromy (query trees), pipelined vs. materialized vyhodnocování

#### Rozdělování dat (partitioning)
- Horizontální dělení (sharding): range, hash, list partitioning
- Vertikální dělení
- Partition pruning

### Ladění dotazů a schématu
- Identifikace problematických dotazů: EXPLAIN / EXPLAIN ANALYZE
- Výběr vhodných indexů
- Denormalizace a její kompromisy
- Materializované pohledy a jejich obnova
- Statistické aktualizace (ANALYZE, VACUUM)

### Zpracování transakcí, výpadky a zotavení

#### ACID vlastnosti
- Atomičnost, konzistence, izolace, trvanlivost (Durability)

#### Řízení souběžnosti (Concurrency Control)
- Dvoufázové zamykání (2PL): striktní 2PL, konzervativní 2PL
- Uváznutí (deadlock): detekce, prevence, timeout
- Optimistické řízení souběžnosti (OCC)
- Víceversní řízení souběžnosti (MVCC)
- Úrovně izolace SQL: READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE

#### Zotavení po výpadcích
- Write-Ahead Logging (WAL): princip, redo a undo záznamy
- Checkpointing: fuzzy checkpoints
- ARIES algoritmus: analýza, redo fáze, undo fáze
- Stínovité stránkování (shadow paging)

### Bezpečnost a přístupová oprávnění
- Modely řízení přístupu: DAC, MAC, RBAC
- SQL příkazy GRANT a REVOKE
- Pohledy (views) jako nástroj bezpečnosti
- Auditování přístupu k datům
- SQL injection a ochrana proti němu
- Šifrování dat: at rest, in transit, transparentní šifrování databáze (TDE)
