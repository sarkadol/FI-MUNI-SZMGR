# Vizualizace

> Základní metriky pro hodnocení kvality vizualizace (efektivita a expresivita), 
> osm základních vizuálních proměnných. 
> Základní vizualizační techniky pro 1D, 2D, 3D (explicitní a implicitní reprezentace povrchu). 
> Techniky pro vizualizaci multidimenzionálních dat 
> (paralelní souřadnice, RadViz, scatterplot matrices, dimensional stacking) a 
> hierarchických struktur (treemaps). Základní třídy interakčních technik (fisheye, perspektivní stěny), 
> specifika aplikace interakčních technik v prostoru samotných dat a v prostoru jejich atributů. (PV251)


## Základní metriky pro hodnocení kvality vizualizace

Vizualizace je transformační proces, který mapuje surová data přes grafické rozhraní do lidského vědomí ($Data \rightarrow Presentation \rightarrow Mind$). Kvalita tohoto procesu se hodnotí dvěma základními, navzájem se doplňujícími kritérii (dle Bertina a Mackinlayho):

* **Expresivita (Expressiveness):** Vyjadřuje schopnost vizualizace prezentovat **právě všechny relevantní informace** obsažené v datech a vůbec nic navíc. 
    * **Matematický vztah:** $$M_{exp} = \frac{\text{zobrazená informace}}{\text{informace k vyjádření}}$$ kde platí $0 \le M_{exp} \le 1$.
    * *Porušení (Sémantický šum):* Pokud vizualizace vizuálně indukuje informaci, která v datech neexistuje (např. použití stupňovaného jasu jedné barvy pro nominální kategorie nutí lidský zrak hledat v datech neexistující uspořádání či důležitost).
* **Efektivita (Effectiveness):** Hodnotí kognitivní a technickou rychlost a přesnost přenosu informace k uživateli. Určuje, zda k vyjádření dat byly použity ty nejvhodnější vizuální proměnné.
    * **Matematický vztah:** $$M_{eff} = \frac{1}{1 + \text{interpretace} + \text{vykreslování}}$$ kde se hodnota pohybuje v rozmezí $0 \le M_{eff} \le 1$.
    * *Ideální stav:* Čas potřebný pro kognitivní interpretaci člověkem i pro technické renderování na GPU je minimální. Efektivní vizualizace maximálně využívá pre-attentive vlastnosti lidského zraku.

### Pre-attentive (předmnožinové) vnímání
Vizuální podněty, které lidské oko a mozek zpracovávají podvědomě, okamžitě (do 200–250 ms) a bez nutnosti vědomého soustředění nebo skenování scény položku po položce. Pokud má být vizualizace anomálií efektivní, musí klíčové prvky vyčnívat pomocí pre-attentive prvků:
* *Geometrické:* Pozice, orientace/sklon, délka, velikost, zakřivení, tloušťka čáry.
* *Optické:* Odstín barvy, intenzita/jas, shlukování prvků, pohyb.

---

## Osm základních vizuálních proměnných

Představují základní grafické mechanismy pro kódování datových atributů do vizuální podoby. Jejich efektivita se striktně liší podle datového typu (Kvantitativní, Ordinální, Nominální):

1. **Pozice (Position):** Nejdůležitější a nejpřesnější proměnná pro **všechny** typy dat. Umístění prvku na osách $(X, Y, Z)$. Ideálně má každý prvek unikátní místo, aby se předešlo nežádoucím překryvům.
2. **Tvar (Shape):** Geometrická povaha prvků (body, linie, symboly). Je to **čistě nominální proměnná**; nelze jí vyjádřit velikost ani pořadí, pouze kategorizaci.
3. **Velikost (Size):** Délka, plocha nebo objem. Skvělá pro kvantitativní data na bodových prvcích. Lidský zrak má však problém s přesným odhadem poměru ploch u regionů (Stevensův zákon). Kardinalita by měla být nízká.
4. **Jas / Hodnota (Value / Brightness):** Úroveň světlosti barvy (od bílé po černou). Má přirozené vnitřní uspořádání, což ji činí **ideální pro ordinální (seřazená) data**.
5. **Barva / Odstín (Hue):** Konkrétní barevný tón (červená, modrá). **Čistě nominální proměnná** vhodná pro klasifikaci. Nemá přirozené uspořádání (duhová paleta je kognitivně nevhodná, protože vytváří umělé hranice v datech). Bezpečná kardinalita pro lidské oko je maximálně 5–8 různých odstínů současně.
6. **Orientace (Orientation):** Úhel natočení vizuálního prvku. Výborná pre-attentive vlastnost pro směrová vektorová pole.
7. **Textura (Texture):** Vzor definovaný hustotou, velikostí prvků a mírou náhodnosti. Lidský zrak velmi citlivě reaguje zejména na změnu hustoty prvků (ordinální charakter).
8. **Pohyb (Motion):** Zahrnuje směr pohybu, rychlost, blikání nebo oscilaci. Nejsilnější pre-attentive stimul vůbec (okamžitě upoutá pozornost i na periferii zorného pole).

<img alt="8vars.png" src="img/vis/8vars.png" width="500"/>

---

## Základní vizualizační techniky pro 1D, 2D, 3D

Techniky se striktně dělí podle prostorové dimenzionality domény a povahy reprezentovaných dat.

### 1D data (sekvence s jednou proměnnou)
* *Techniky:* Klasické čárové grafy, sloupcové grafy, barevný pruh (*color bar*).
* *Vícerozměrná 1D data:* Používá se buď **juxtapositioning** (skládání samostatných grafů vedle sebe/pod sebe), nebo **superimpositioning** (překrývání více křivek v jednom souřadném systému s odlišením barvou či stylem čáry).

### 2D data (data mapovaná na plochu)
* *Techniky:* **Scatterplot** (bodový graf bez interpolace pro analýzu korelací), **geografické mapy** (vektorové polygony a choroplethy), **rastrový obraz** (spojitá interpolovaná pixelová data), **cityscape** (2.5D zobrazení výšky bloků v rovině) a **kontury/izolinie** (izobary, vrstevnice – spojnice míst se stejnou hodnotou skalárního pole).

### 3D data (objemová data a 3D prostor)
Data jsou typicky reprezentována jako spojitý fenomén vzorkovaný diskrétně do pravidelné mřížky (**Voxely**) nebo nepravidelné mřížky (**Tetrahedry**). Rozlišujeme dva přístupy k zobrazení povrchů:
* **Explicitní reprezentace povrchu (Boundary Representation):** Povrch je přesně definován konkrétním výčtem prvků. Obsahuje explicitní seznam 3D vrcholů (*vertices*), hran (*edges*) a rovinných polygonů (*faces*) tvořících polygonální síť. Ideální pro rychlé renderování na GPU pomocí rasterizace.
* **Implicitní reprezentace povrchu:** Povrch je definován geometricky jako izohladina matematické funkce nebo hustotního pole ve tvaru $f(x, y, z) = c$ (kde $c$ je prahová hodnota/isovalue).
    * *Vizualizační algoritmus:* **Marching Cubes**. Převádí implicitní objemová data na explicitní polygonální síť. Prochází 3D mřížku po jednotlivých krychlích (buňkách). Na základě toho, které z 8 vrcholů buňky leží nad/pod prahem $c$, vyhledá v tabulce typologií (256 kombinací redukovaných na 15 základních případů) odpovídající konfiguraci trojúhelníků a vygeneruje explicitní polygony.
    * *Alternativa:* **Ray Casting (Direct Volume Rendering)**. Nepřevádí data na polygony. Skrz objemová data se z kamery vrhají paprsky. V pravidelných intervalech se vzorkují hodnoty, které se přes **přenosové funkce (transfer functions)** mapují na barvu a neprůhlednost (*opacity*), a ty se podél paprsku integrují do výsledného pixelu.

---

## Techniky pro multidimenzionální data a hierarchické struktury

Metody pro převod komplexních vícerozměrných vztahů ($N$-dimenzí) do srozumitelného 2D prostoru obrazovky.

* **Paralelní souřadnice (Parallel coordinates):** Osy $X_1, \dots, X_n$ jsou uspořádány rovnoběžně vedle sebe. Každá datová položka (bod) je reprezentována jako **lomená čára (polyline)** protínající tyto osy v místech svých hodnot. 
    * *Korelace:* Pokud se čáry mezi dvěma osami kříží, indikuje to negativní korelaci; pokud běží rovnoběžně, indikuje to pozitivní korelaci. Trpí vizuálním zahlcením (*cluttering*) při velkém $N$.
* **RadViz:** Nelineární projekce inspirovaná Hookeovým elastickým zákonem. Kotvy jednotlivých $N$ dimenzí jsou rozmístěny po obvodu kružnice. Datový bod je zobrazen uvnitř kružnice v místě, kde jsou přitažlivé síly "pružin" všech dimenzí v rovnováze (síla pružiny je úměrná hodnotě dimenze). Může dojít k překryvu odlišných bodů uprostřed kružnice.
* **Matice bodových grafů (Scatterplot matrices - SPLOM):** Symetrická čtvercová matice o velikosti $N \times N$, která obsahuje standardní 2D scatterploty pro všechny kombinace dvojic dimenzí. Na hlavní diagonále jsou obvykle histogramy jedné dimenze. Špatně škáluje pro vysoké desítky dimenzí kvůli zmenšování oken.
* **Dimensional stacking:** Mapuje data z diskrétního prostoru do 2D pomocí rekurzivního vnořování souřadných systémů. Dvojice hlavních dimenzí rozdělí obrazovku na makro-sekce (mřížku) a každá buňka se vnitřně dělí na sub-mřížky podle dvojic méně významných dimenzí.
* **Treemaps (pro hierarchické struktury):** *Space-filling* metoda zobrazující stromová data jako do sebe rekurzivně vnořené obdélníky. Celá plocha obrazovky se dělí střídavě horizontálně a vertikálně podle hierarchických úrovní.
    * *Kódování:* Plocha obdélníku vyjadřuje kvantitativní vlastnost (např. velikost souboru), barva vyjadřuje stav či kategorii. 
    * *Algoritmy:* Základní *Slice-and-Dice* generuje příliš úzké obdélníky. Algoritmus **Squarified Treemaps** optimalizuje poměr stran tak, aby se blížil čtverci ($1:1$), což usnadňuje lidské porovnávání ploch.

<img alt="img.png" src="img/vis/multidim.png" width="800"/>

---

## Základní třídy interakčních technik

Interakce řeší základní kognitivní problém: jak analyzovat detail dat bez ztráty celkového přehledu (koncept **Focus + Context**).

### Deformační operátory (Focus + Context)
* **Fisheye (Rybí oko):** Aplikuje se v prostoru grafického zobrazení (obrazovky). Uživatel definuje ohnisko (fokus), poloměr lupy a míru deformace. Centrální oblast se geometricky zvětší pro detailní analýzu, zatímco okolní kontext zůstane plynule komprimován a stlačen směrem k okrajům obrazovky.
* **Perspektivní stěny (Perspective walls):** Řeší navigaci v prostoru objektů (např. dlouhé časové osy). Data se horizontálně rozprostřou na trojdílnou stěnu. Centrální panel (rovnoběžný s obrazovkou) zobrazuje detail (Focus), zatímco boční panely ubíhají do pozadí pomocí 3D perspektivní deformace (Context), čímž šetří místo na obrazovce.

<img alt="img.png" src="img/vis/fisheye.png" width="700"/>

---

## Specifika aplikace interakčních technik v prostoru samotných dat a v prostoru jejich atributů

Vizualizační systémy striktně rozlišují dva odlišné prostory pro aplikaci interakcí:

### 1. V prostoru samotných dat (Space of data values)
Interakční techniky operují nad **abstraktními hodnotami proměnných, logickou strukturou nebo topologií dat**. Tyto operace přímo mění výběr, uspořádání nebo rozsah analyzovaných datových položek.
* *Specifické techniky:*
    * **Filtrování rozsahů (Filtering):** Omezení datové sady pomocí logických podmínek (např. posuvníkem zafixovat zobrazení pouze pro $20 < \text{Věk} < 35$).
    * **Brushing a Linking:** Interaktivní výběr a zvýraznění podmnožiny dat v jednom grafu (např. v histogramu) okamžitě vyvolá geometrické zvýraznění stejných datových položek ve všech ostatních otevřených vizualizacích (např. v mapě či scatterplotu).
    * **Strukturální změny:** Změna pořadí os v paralelních souřadnicích (pro odhalení nových korelací) nebo interaktivní odstraňování/přidávání hran a uzlů v grafových strukturách.

### 2. V prostoru atributů / grafického zobrazení (Space of attributes / Presentation)
Interakční techniky operují výhradně nad **vizuálními atributy grafických entit na obrazovce** (pixely, barvy, geometrické transformace zobrazení). Nemění podkladová data ani jejich strukturu, pouze upravují způsob jejich vnímání a vykreslení.
* *Specifické techniky:*
    * **Modifikace přenosových funkcí (Transfer Functions):** U objemového vykreslování (Ray Casting) uživatel interaktivně mění křivky, které definují, jaká hodnota hustoty bude mít jakou barvu a jakou míru průhlednosti (*opacity*). Tímto způsobem lze interaktivně "zneviditelnit" kůži a svaly a nechat vykreslenou pouze kostru, aniž by se změnila zdrojová 3D voxelová data.
    * **Optické úpravy:** Změna jasu, kontrastu, ladění barevných palet (např. přepnutí z barevného kódování do stupňů šedi) nebo interaktivní změna parametrů stínování (*shading*) a nasvícení 3D scény.
    * **Geometrická navigace zobrazení:** Operace typu pan (posun kamery), zoom (přiblížení scény) nebo aplikace deformačních filtrů (Fisheye) v rovině obrazovky.