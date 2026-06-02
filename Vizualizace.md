# Vizualizace

> Základní metriky pro hodnocení kvality vizualizace (efektivita a expresivita), 
> osm základních vizuálních proměnných. 
> Základní vizualizační techniky pro 1D, 2D, 3D (explicitní a implicitní reprezentace povrchu). 
> Techniky pro vizualizaci multidimenzionálních dat 
> (paralelní souřadnice, RadViz, scatterplot matrices, dimensional stacking) a 
> hierarchických struktur (treemaps). Základní třídy interakčních technik (fisheye, perspektivní stěny), 
> specifika aplikace interakčních technik v prostoru samotných dat a v prostoru jejich atributů.


## Základní metriky pro hodnocení kvality vizualizace (efektivita a expresivita)

* **Expresivita (Expressiveness):** Vyjadřuje poměr mezi zobrazenou informací a informací, kterou
  chceme vyjádřit.
* Matematicky je definována jako $M_{exp} = \text{zobrazená informace} / \text{informace
  k vyjádření}$, kde $0 \le M_{exp} \le 1$.
* Ideální expresivita nastává při hodnotě $M_{exp} = 1$. Pokud je hodnota menší než 1, vizualizace
  prezentuje méně informací, než je požadováno.
* **Efektivita (Effectiveness):** Hodnotí úspěšnost přenosu informace na základě správnosti a
  rychlosti interpretace uživatelem a rychlosti vykreslování.
* Matematicky je zapsána vztahem $M_{eff} = 1 / (1 + \text{interpretace} + \text{vykreslování})$,
  kde $0 \le M_{eff} \le 1$.
* Blíží-li se hodnota efektivnosti k 1, znamená to, že čas potřebný pro interpretaci i renderování
  je velmi krátký.

---

## Osm základních vizuálních proměnných

Vizuální proměnné maximalizují efektivitu navržené vizualizace pro přenos informací:
* **Pozice (Position):** Nejdůležitější proměnná. Ideálně má každý symbol unikátní pozici a nedochází
  k překryvům. Využívá lineární či logaritmické měřítko.
* **Tvar (Shape):** Zahrnuje body, linie, regiony, objemy, specifické symboly či písmena.
* **Velikost (Size):** Vhodná pro sady s malou kardinalitou. Je dobře použitelná pro body a křivky,
  avšak nevhodná pro plošné regiony.
* **Jas (Brightness):** Využívá jasovou stupnici (lineární či nelineární) pro mapování hodnot
  datových proměnných.
* **Barva (Color):** Charakterizována odstínem (hue) a saturací. Výběr barevné palety výrazně
  ovlivňuje vnímání umělých hranic v datech (např. problém duhové palety).
* **Orientace (Orientation):** Určuje natočení nebo úhel vizuálního prvku na obrazovce.
* **Textura (Texture):** Percepční prvky textury mohou měnit výšku, hustotu a míru náhodnosti
  uspořádání. Hustota a velikost jsou lidským okem velmi dobře vnímány.
* **Pohyb (Motion):** Může být asociován s jakoukoliv jinou proměnnou (směr pohybu, blikání,
  oscilace). Změny v obraze přitahují pozornost a zlepšují kognici.

---

## Základní vizualizační techniky pro 1D, 2D, 3D (explicitní a implicitní reprezentace povrchu)

* **1D data:** Jde o sekvence 1D dat s jednou proměnnou. Mezi základní techniky patří klasický graf
  (linkový, sloupcový) nebo barevný pruh (color bar). Pro vícerozměrná 1D data se využívá
  juxtapositioning (vedle sebe) nebo superimpositioning (překrývání).
* **2D data:** Data se dvěma prostorovými dimenzemi mapovaná na obrazovku. Typickými technikami jsou
  scatterplot (bodový graf bez interpolace), mapy (linie, polygony), rastrový obraz s interpolací
  mezi pixely, cityscape (3D bloky v rovině) a kontury/izobary.
* **3D data:** Reprezentují diskrétní vzorky spojitého fenoménu nebo sady vrcholů, hran a polygonů.
* **Explicitní reprezentace povrchu:** Povrch je přesně definován výčtem prvků. Obsahuje seznam 3D
  vrcholů (vertices), hran (edges) a rovinných polygonů (faces), případně sadu parametrických
  rovnic definujících souřadnice propojených bodů (např. zápis polygonální sítě).
* **Implicitní reprezentace povrchu:** Povrch je definován pomocí nuly (izohladiny) matematické
  funkce o dvou či třech proměnných, například $f(x, y, z) = 0$. Definuje plochy, které přesně
  odpovídají zadaným prahovým hodnotám, bez nutnosti explicitně ukládat polygony.

---

## Techniky pro vizualizaci multidimenzionálních dat (paralelní souřadnice, RadViz, scatterplot matrices, dimensional stacking) a hierarchických struktur (treemaps)

* **Paralelní souřadnice (Parallel coordinates):** Místo ortogonálních os umisťuje osy paralelně
  vedle sebe (vertikálně či horizontálně). Každá datová položka je reprezentována jako lomená čára
  (polyline) protínající jednotlivé osy v místech odpovídajících hodnot parametrů.
* **RadViz:** Technika založená na Hookeově zákoně o elasticitě pro hledání rovnovážné polohy bodu.
  Kotvy jednotlivých dimenzí ($N$) jsou rovnoměrně rozmístěny po obvodu kružnice a simulují pružiny
  přitahující datový bod silou úměrnou hodnotě dané dimenze.
* **Scatterplot matrices (SPLOM):** Matice (mřížka) o velikosti $N \times N$ obsahující dvourozměrné
  bodové grafy pro všechny možné dvojice dimenzí. Je symetrická podle hlavní diagonály, která
  typicky obsahuje popisy dimenzí nebo histogramy rozdělení.
* **Dimensional stacking:** Mapování dat z diskrétního $N$-dimenzionálního prostoru do 2D obrazu
  rekurzivním vnořováním souřadných systémů. Dvojice vybraných dimenzí rozdělí obrazovku na sekce a
  každá sekce je vnitřně dále dělena podle dalších dvojic dimenzí.
* **Treemaps (pro hierarchické struktury):** Metoda zaplňující prostor (space-filling) zobrazující
  stromová data jako do sebe vnořené obdélníky. Celý prostor obrazovky se rekurzivně dělí střídavě
  horizontálními a vertikálními liniemi, přičemž plocha obdélníku odpovídá počtu listů.

---

## Základní třídy interakčních technik (fisheye, perspektivní stěny), specifika aplikace interakčních technik v prostoru samotných dat a v prostoru jejich atributů

* **Fisheye (Rybí oko):** Patří mezi deformační (distortion) operátory aplikované v prostoru
  obrazovky. Uživatel definuje střed transformace (ohnisko), poloměr lupy a míru deformace.
  Centrální oblast je zvětšena pro detailní náhled, zatímco okolní kontext je plynule komprimován.
* **Perspektivní stěny (Perspective walls):** Metoda pro navigaci v rozsáhlých sadách dat v prostoru
  objektů. Skládá se z centrálního panelu umístěného kolmo k pohledu uživatele a z bočních panelů,
  které se plynule svažují do pozadí na základě perspektivní deformace.
* **Specifika aplikace v prostoru samotných dat (Space of data values):** Interakce ovlivňují přímo
  hodnoty nebo strukturu. Zahrnuje filtrování rozsahů pomocí posuvníků, datově řízené zvýrazňování
  (brushing) nebo změnu topologie (změna uspořádání os, filtrování hran).
* **Specifika aplikace v prostoru jejich atributů (Space of attributes):** Interakce operují nad
  komponentami grafických entit. Dominantní je změna barevného mapování, úprava jasu, kontrastu a
  interaktivní ladění přenosových funkcí (transfer functions) pro průhlednost.