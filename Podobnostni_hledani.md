# Podobnostní hledání

povinné pro studium dle kontrolní šablony 2022/2023 nebo novější
> Principy podobnostního hledávání: metrický prostor, extrakce popisovačů a jejich vztah s člověkem vnímanou podobností, 
> typy dotazů a jejich definice. 
> Principy indexování: dělení dat, filtrování dat (pivoting). 
> Srovnání s tradičními indexy (B+ trees).


## Principy podobnostního hledání

Tradiční databázové systémy pracují s exaktním vyhledáváním (Exact Match) nad strukturovanými daty, která lze lineárně uspořádat. S nástupem nestrukturovaných multimediálních dat (obrázky, audio, video, textové embeddingy) se však paradigma mění na vyhledávání podle podobnosti. Základním matematickým konceptem pro exaktní formalizaci tohoto problému je **metrický prostor**.

### Metrický prostor
Metrický prostor je uspořádaná dvojice $(M, d)$, kde $M$ reprezentuje univerzum objektů (doménu) a $d$ je metrika neboli vzdálenostní funkce $d: M \times M \rightarrow \mathbb{R}$. Tato funkce každé dvojici objektů přiřazuje reálné číslo vyjadřující jejich míru odlišnosti (čím menší hodnota, tím větší podobnost).

Aby byla funkce $d$ regulérní metrikou, musí pro libovolné objekty $x, y, z \in M$ striktně splňovat následující **čtyři axiomy**:
1. **Nezápornost:** $d(x, y) \geq 0$
2. **Identita:** $d(x, y) = 0 \iff x = y$
3. **Symetrie:** $d(x, y) = d(y, x)$
4. **Trojúhelníková nerovnost:** $d(x, z) \leq d(x, y) + d(y, z)$

**Klíčový detail:** Trojúhelníková nerovnost je zcela fundamentální vlastnost, na které stojí veškeré metrické indexování. Umožňuje nám odvozovat spodní a horní odhady vzdáleností mezi objekty, aniž bychom tyto vzdálenosti museli reálně měřit, což slouží jako základ pro prořezávání (pruning) vyhledávacího prostoru.

Pokud funkce nesplňuje axiom identity (může nastat $d(x, y) = 0$ pro $x \neq y$), nazývá se **pseudometrika**. Pokud nesplňuje trojúhelníkovou nerovnost (např. často používaná Kosinová vzdálenost), nejedná se o metriku a standardní metrické indexy nad ní nelze korektně sestavit.

#### Často používané vzdálenostní funkce:
* **Minkowského vzdálenost ($L_p$ metriky):** Definuje rodinu metrik v lineárních prostorech $\mathbb{R}^n$:
    $$d(x, y) = \left( \sum_{i=1}^{n} |x_i - y_i|^p \right)^{1/p}$$
    * $p=1$: Manhattan (City-block) vzdálenost ($L_1$)
    * $p=2$: Eukleidovská vzdálenost ($L_2$) – nejčastější geometrická metrika.
    * $p \to \infty$: Čebyševova vzdálenost ($L_\infty$) – definována jako maximum z absolutních rozdílů složek: $\max_{i=1..n} |x_i - y_i|$.
* **Edit vzdálenost (Levenshtein):** Nemyslitelná bez metrického pojetí; udává minimální počet editačních operací (vložení, smazání, záměna znaku) pro transformaci jednoho řetězce na druhý.
* **Jaccardova vzdálenost:** Využívána pro porovnávání množinových dat (např. tokeny v textu):
    $$d_J(A, B) = 1 - \frac{|A \cap B|}{|A \cup B|}$$

<img alt="img.png" src="img/podobnostni_hledani/distances.png" width="300"/>

---

### Extrakce popisovačů (Feature Extraction)
Objekty reálného světa jsou pro přímé matematické porovnání příliš komplexní a nestrukturované. Proto nastupuje fáze **extrakce příznaků**, která transformuje surový objekt na kompaktní matematickou reprezentaci – **popisovač (feature descriptor / feature vector)**.

Tento proces může být:
1. **Založený na exaktních algoritmech (Hand-crafted features):** Např. barevné histogramy obrázků, textury, nebo frekvenční spektra u audia.
2. **Založený na hlubokém učení (Deep Learning):** Využití neuronových sítí (např. konvolučních sítí či transformátorů jako CLIP), kde je objekt promítnut do vícerozměrného vektorového prostoru, tzv. **embedding prostoru**.

#### Vztah s člověkem vnímanou podobností a Sémantická propast
Hlavní výzvou podobnostního vyhledávání je **sémantická propast (Semantic Gap)**. Jde o rozdíl mezi tím, jak data reprezentuje počítač (nízkoúrovňové informace, např. matice RGB pixelů obrázku), a tím, jak je vnímá lidský uživatel (vysokoúrovňové koncepty, emoce, kontext – např. "šťastný pes na louce").

* **Cíl extrakce:** Dosáhnout stavu, kdy matematická blízkost popisovačů v metrickém prostoru věrně koreluje s kognitivní (sémantickou) podobností vnímání člověka.
* Pokud extrakční funkce selže a nedokáže sémantiku zachytit, index sice bude matematicky fungovat bezchybně, ale výsledky vyhledávání budou pro člověka irelevantní.

---

### Typy dotazů a jejich definice
Při zadání databáze (množiny objektů) $X \subseteq M$ a dotazového objektu $q \in M$ (query object) rozlišujeme dva základní typy podobnostních dotazů:

<img alt="img.png" src="img/podobnostni_hledani/typy-dotazu.png" width="600"/>

#### 1. Range Query (Rozsahový dotaz)
Vrátí všechny objekty z databáze $X$, jejichž vzdálenost od dotazu $q$ je menší nebo rovna specifikovanému poloměru vyhledávání $r$ ($r \geq 0$).
$$\text{Range}(q, r) = \{ x \in X \mid d(q, x) \leq r \}$$
* **Charakteristika:** Poloměr $r$ určuje pevnou uživatelskou toleranci k odlišnosti objektů. Nevýhodou je rigidita – u málo hustých datových prostorů může dotaz vrátit prázdnou množinu, u příliš hustých naopak obrovské množství objektů.

#### 2. k-Nearest Neighbor Query (k-NN / Dotaz na k nejbližších sousedů)
Vrátí množinu $A \subseteq X$ obsahující přesně $k$ objektů ($|A| = k$), pro které platí, že žádný objekt mimo tuto množinu není k dotazu $q$ blíže než objekty uvnitř ní.
$$\forall y \in A, \forall z \in X \setminus A: d(q, y) \leq d(q, z)$$
* **Charakteristika:** Garantuje uživateli stabilní počet výsledků ($k$). 
* **Implementační detail:** Vyhodnocení $k$-NN se v indexech typicky transformuje na rozsahový dotaz s dynamicky se zmenšujícím vyhledávacím poloměrem $r$, kde $r$ je v každém kroku algoritmu definováno jako aktuální vzdálenost k $k$-tému nejbližšímu doposud nalezenému objektu (ukládanému v prioritní frontě).

#### 3. Reverse k-Nearest Neighbor Query (Rk-NN / Reverzní dotaz na k nejbližších sousedů)
Vyhledá všechny objekty v databázi $X$, pro které je zadaný dotazový objekt $q$ jedním z jejich $k$ nejbližších sousedů.
$$\text{Rk-NN}(q) = \{ x \in X \mid q \in \text{k-NN}(x) \}$$
* **Charakteristika:** Zatímco standardní $k$-NN hledá objekty blízké z pohledu dotazu $q$, reverzní dotaz zkoumá situaci z pohledu samotných datových objektů. Odpovídá na otázku: *"Pro koho je tento nový objekt/produkt zajímavý či blízký?"* Výsledná množina může být klidně i prázdná.
* **Typické využití:** Cílený marketing (identifikace zákazníků, pro které je nový produkt nejvhodnější), detekce anomálií nebo analýza vlivu uzlu v síti.

#### 4. Similarity Join Query (Dotaz na podobnostní propojení)
Pracuje nad dvěma datovými množinami $X \subseteq M$ a $Y \subseteq M$ (případně nad jednou a touto samou množinou v případě *Similarity Self-Join*). Cílem je nalézt všechny dvojice objektů $(x, y)$, jejichž vzdálenost nepřekračuje zadanou prahovou hodnotu $\mu$.
$$X \bowtie_{\mu} Y = \{ (x, y) \in X \times Y \mid d(x, y) \leq \mu \}$$
* **Charakteristika:** Jde o analogii klasického operátoru `JOIN` z relačních databází, avšak namísto exaktní rovnosti atributů vyhodnocuje metrickou blízkost prvků napříč celými sadami. Výpočetně se jedná o extrémně náročnou operaci s naivní složitostí $\mathcal{O}(|X| \cdot |Y|)$.
* **Typické využití:** Čištění dat (identifikace duplicit a překlepů), párování entit ze dvou různých zdrojů nebo shluková analýza.
---

## 2. Principy indexování

Sekvenční procházení databáze (Sequential Scan / Brute Force) vyžaduje spočítat vzdálenost $d(q, x)$ pro každý objekt $x \in X$. Jelikož je výpočet metriky $d$ výpočetně nesmírně drahý (zejména u komplexních nebo high-dimensional popisovačů), je sekvenční skenování pro velké datové sady nepoužitelné ($\mathcal{O}(N)$). Cílem indexování je organizovat data tak, aby bylo možné velkou část prostoru při vyhledávání bezpečně ignorovat.

### Dělení dat (Data Partitioning)
Metrické indexy rozdělují datový prostor na dílčí regiony (podprostory) reprezentované uzly stromové struktury. K tomuto dělení se využívají vybrané objekty zvané **pivoti (reference points)**. Na rozdíl od vektorových prostorů (kde lze dělit prostor fixními souřadnicovými osami) v čistě metrickém prostoru můžeme měřit pouze vzdálenosti mezi objekty. Existují dva základní principy dělení dat:

#### A) Ball Partitioning (Dělení do metrických koulí)
Zvolí se jeden pivot $p \in X$ a určí se dělící poloměr $r_m$ (zpravidla medián vzdáleností všech objektů k tomuto pivotu). Data se rozdělí do dvou disjunktních podmnožin:
* **Vnitřní region (uvnitř koule):** $X_{left} = \{ x \in X \mid d(p, x) \leq r_m \}$
* **Vnější region (vně koule):** $X_{right} = \{ x \in X \mid d(p, x) > r_m \}$

Tento princip tvoří základ hierarchických struktur jako je **VP-Tree (Vantage Point Tree)** nebo **Multi Vantage Point Tree (MVPT)**. Uzly si ukládají pivot $p$ a poloměr $r_m$.

#### B) Generalized Hyperplane Partitioning (Dělení zobecněnou nadrovinou)
Zvolí se dva pivoti $p_1, p_2 \in X$. Prostor se rozdělí pomyslnou nadrovinou, která se nachází přesně uprostřed mezi těmito dvěma body. Každý objekt je přiřazen k tomu pivotu, ke kterému má blíž:
* **Region pivotu $p_1$:** $X_{left} = \{ x \in X \mid d(p_1, x) \leq d(p_2, x) \}$
* **Region pivotu $p_2$:** $X_{right} = \{ x \in X \mid d(p_1, x) > d(p_2, x) \}$

Tento přístup využívá například **GHT (Generalized Hyperplane Tree)** nebo dynamický, diskově orientovaný **M-Tree**.

---

### Filtrování dat (Pivoting) a prořezávání vyhledávacího prostoru
**Pivoting** je technika eliminace kandidátů bez nutnosti počítat jejich reálnou vzdálenost k dotazu $q$. Je plně závislá na platnosti **trojúhelníkové nerovnosti**.

Mějme dotaz $q$, vyhledávací poloměr $r$ a předpočítaného pivota $p$. Vzdálenosti mezi pivotem $p$ a všemi datovými objekty $x$ (tedy hodnoty $d(p, x)$) jsou exaktně spočteny během fáze budování indexu a uloženy v paměti/na disku. Při provádění dotazu spočítáme pouze jedinou vzdálenost: $d(q, p)$.

Z trojúhelníkové nerovnosti přímo vyplývá vztah:
$$|d(p, x) - d(q, p)| \leq d(q, x)$$

Chceme-li ověřit, zda objekt $x$ může být součástí výsledku rozsahového dotazu (tedy zda potenciálně platí $d(q, x) \leq r$), aplikujeme **prořezávací podmínku (Pruning Condition)**:

$$\text{Pokud } |d(p, x) - d(q, p)| > r, \text{ pak zaručeně platí } d(q, x) > r.$$

* **Důsledek:** Pokud je tato podmínka splněna, objekt $x$ (případně celý podstrom objektů, který je ohraničen příslušnou metrickou koulí kolem pivota) můžeme okamžitě **vyřadit (prořezat)** z dalšího zpracování. Výpočet vzdálenosti $d(q, x)$ se zcela přeskočí.