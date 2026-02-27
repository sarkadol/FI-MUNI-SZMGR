# Vizualizace (PV251)

## Popis okruhu

Základní metriky pro hodnocení kvality vizualizace (efektivita a expresivita), osm základních vizuálních proměnných. Základní vizualizační techniky pro 1D, 2D, 3D (explicitní a implicitní reprezentace povrchu). Techniky pro vizualizaci multidimenzionálních dat (paralelní souřadnice, RadViz, scatterplot matrices, dimensional stacking) a hierarchických struktur (treemaps). Základní třídy interakčních technik (fisheye, perspektivní stěny), specifika aplikace interakčních technik v prostoru samotných dat a v prostoru jejich atributů.

## Témata

### Metriky pro hodnocení kvality vizualizace

#### Expresivita (Expressiveness)
- Vizualizace by měla vyjadřovat **přesně ta data**, která chceme zobrazit – ne více, ne méně
- Správná volba vizuálního kódování odpovídající typu dat (nominální, ordinální, kvantitativní)

#### Efektivita (Effectiveness)
- Vizualizace by měla efektivně využívat lidské percepční schopnosti
- Přesnost percepce pro různé vizuální proměnné (Clevelandova a McGillova studie)
- Kompromis mezi přesností, rychlostí čtení a pamětí

### Osm základních vizuálních proměnných (Bertinovy vizuální proměnné)
1. **Poloha (Position):** nejpřesnější percepce kvantitativních dat
2. **Velikost (Size):** vhodná pro kvantitativní data; snadno odhadnutelná
3. **Tvar (Shape):** vhodná pro nominální data; omezená škálovatelnost
4. **Hodnota (Value):** tmavost/světlost; vhodná pro ordinální data
5. **Barva (Color hue):** vhodná pro nominální data; omezený počet kategorií
6. **Orientace (Orientation):** vhodná pro omezený rozsah diskrétních hodnot
7. **Textura (Texture):** vzory; vhodná pro nominální data, méně efektivní
8. **Pohyb (Motion):** přitahuje pozornost; vhodná pro dynamická data

### Vizualizační techniky pro 1D, 2D, 3D data

#### 1D data
- Histogram: distribuce hodnot jednoho atributu
- Box plot (krabičkový diagram): medián, kvartily, odlehlé hodnoty
- Violin plot: hustota pravděpodobnosti + box plot

#### 2D data
- Scatter plot (bodový graf): dva atributy na osách x a y
- Heatmapa (2D hustotní mapa): hustota dat v 2D prostoru
- Konturový plot: izolinky v 2D prostoru

#### 3D data – explicitní reprezentace povrchu
- Povrchová síť (mesh): trojúhelníková nebo polygonální síť
- Bodová mračna (point clouds): kolekce 3D bodů
- Parametrické plochy: NURBS, Bezierovy plochy

#### 3D data – implicitní reprezentace povrchu
- Isosurface (izoploch): plocha definovaná hodnotou skalárního pole (Marching Cubes algoritmus)
- Signed Distance Function (SDF): vzdálenost od povrchu s příznakem
- Volumetrické renderování: ray casting, přímé zobrazení objemových dat

### Techniky pro vizualizaci multidimenzionálních dat

#### Paralelní souřadnice (Parallel Coordinates)
- Každá dimenze je reprezentována svislou osou; záznamy jsou linie protínající osy
- Vhodné pro: odhalení korelací, clusterů, vzorů mezi dimenzemi
- Nevýhody: cluttering při velkých datech, závislost na pořadí os

#### RadViz
- Dimenze jsou umístěny rovnoměrně na kružnici jako kotvy (anchors)
- Každý záznam je reprezentován bodem přitahovaným ke kotvám proporcionálně k hodnotám atributů
- Vhodné pro: klasifikační problémy, odhalení shluků

#### Scatterplot Matrices (SPLOM)
- Matice scatter plotů pro každou dvojici atributů
- Umožňuje vizuálně zkoumat všechny párové vztahy najednou
- Nevýhody: kvadratická složitost vůči počtu dimenzí

#### Dimensional Stacking
- Rekurzivní vnořování 2D mřížek pro zobrazení vícerozměrných dat
- Vnější dimenze řídí rozmístění vnitřních mřížek

### Vizualizace hierarchických struktur

#### Treemaps
- Rekurzivní dělení plochy obdélníků podle hierarchie a hodnot
- Algoritmy dělení: slice-and-dice, squarified treemaps (minimalizace poměru stran)
- Kódování: plocha obdélníku = kvantitativní atribut; barva = jiný atribut
- Vhodné pro: zobrazení hierarchického rozdělení (využití disku, portfolio, organizace)

### Interakční techniky

#### Fisheye (Rybí oko)
- Distorze zobrazení: oblast zájmu zvětšena, okolí zmenšeno
- Zachování kontextu při detailním pohledu
- Grafické fisheye: geometrická distorze; sémantické fisheye: přizpůsobení obsahu

#### Perspektivní stěny (Perspective Wall)
- 3D analogie fisheye: centrální panel (detail) + boční panely (kontext) v perspektivě
- Vhodné pro časové osy a lineárně uspořádaná data

#### Interakce v prostoru dat vs. prostoru atributů

##### Interakce v prostoru dat
- Výběr (selection): identifikace konkrétních datových bodů nebo oblastí
- Zoom & Pan: přiblížení a posun v datovém prostoru
- Brushing & Linking: výběr v jednom pohledu se projeví ve všech propojených pohledech

##### Interakce v prostoru atributů
- Filtrování (filtering): omezení zobrazovaných dat dle hodnot atributů
- Přeuspořádání dimenzí: mění pořadí os (paralelní souřadnice, SPLOM)
- Agregace: slučování záznamů do skupin dle atributů
