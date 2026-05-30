# Metody umělé inteligence

> Prohledávání stavového prostoru, lokální prohledávání a metaheuristiky s jedním řešením, 
> populační metaheuristiky (evoluční algoritmy, inteligence hejna). 
> Plánování, reprezentace problému, plánování se stavovým prostorem. 
> Práce s neurčitostí, Bayesovské sítě, exaktní a aproximační odvozování, čas a neurčitost, 
> teorie užitku, Markovský rozhodovací proces, iterace hodnot, iterace strategie. 
> Robotika, plánování pohybu robota (konfigurační prostor, kombinatorické a pravděpodobnostní přístupy).

---

## Prohledávání stavového prostoru
* **Heuristické algoritmy:** Specifické algoritmy navržené pro konkrétní problém. Neexistuje u nich žádná teoretická záruka kvality výsledku ani dosažení globálního optima. Využívají vnitřní strukturu problému (tzv. „art of discovering“).
    * *Příklad (Single machine scheduling):* Rozvrhni jako první tu úlohu, která má nejbližší termín dokončení (*Earliest Due Date*).
    * *Příklad (Vehicle routing):* Přiřaď novou zastávku vozidlu, které se aktuálně nachází nejblíže danému místu.
* **Metaheuristiky:** Termín zavedl Fred Glover v roce 1986 (*meta* = metodologie vyšší úrovně). Jedná se o obecné, na problému nezávislé šablony a řídicí strategie vyšší úrovně. Tyto šablony vedou a propojují podřízené heuristiky za účelem efektivního prohledávání stavového prostoru. Jsou široce aplikovatelné na různé domény (např. simulované žíhání lze aplikovat na rozvrhování zkoušek i na problém obchodního cestujícího).

#### Diversifikace vs. Intenzifikace
Při návrhu jakékoli metaheuristiky dochází k neustálému balancování mezi dvěma protichůdnými kritérii:
1. **Diversifikace (Průzkum / Exploration):** Schopnost algoritmu opustit lokální oblasti a prohledávat dosud neprozkoumané, vzdálené části stavového prostoru (globální pohled).
2. **Intenzifikace (Využití / Exploitation):** Schopnost algoritmu detailně prohledat slibné lokální okolí aktuálního řešení s cílem nalézt lokální optimum (lokální pohled).

<img alt="img.png" src="img/metody_umele_inteligence/divers-vs-intens.png" width="600"/>

#### Reprezentace (Kódování / Encoding)
Způsob, jakým je řešení v algoritmu zakódováno, zásadně ovlivňuje velikost prohledávacího prostoru a složitost operátorů.
* **Lineární reprezentace (řetězce symbolů):**
    * **Binární kódování:** Každá rozhodovací proměnná nabývá hodnoty $s_i \in \{0, 1\}$.
        * *Příklad (Problém batohu / Knapsack problem):* Máme $n$ předmětů. Řešení je popsáno vektorem $s = (s_1, s_2, \dots, s_n)$, kde $s_i = 1$, pokud je předmět vložen do batohu, a $s_i = 0$ v opačném případě. Využívá se také u SAT problému či celočíselného programování.
    * **Diskrétní kódování:** Zobecněné binární kódování, kde proměnné nabývají hodnot z konečných diskrétních domén. Často se na tento problém redukují přiřazovací úlohy (*Assignment problems*).
        * *Příklad (Alokace zdrojů):* Přiřazení $n$ úloh $m$ agentům. Řešení reprezentuje vektor $s$ o velikosti $n$, kde $s_i = j$ značí, že úloha $i$ je přiřazena agentovi $j$.
    * **Reprezentace permutací:** Vhodná pro sekvenční, rozvrhovací a trasovací problémy. Každý prvek se v zápisu smí vyskytnout nejvýše jednou.
        * *Příklad (Problém obchodního cestujícího / TSP):* Permutace indexů měst udává pořadí jejich návštěvy.
        * *Příklad (Rozvrhování na jednom stroji):* Permutace určuje pořadí zpracování úloh bez překryvu.
* **Nelineární reprezentace:** Založená na složitějších strukturách, nejčastěji grafech nebo stromech (využívá se např. v genetickém programování).

#### Redukce reprezentačního prostoru
Správnou volbou kódování lze eliminovat zjevně neplatná řešení a drasticky zmenšit velikost prohledávaného prostoru.
* *Příklad (Rozmístění 8 šachových dam tak, aby se navzájem neohrožovaly):*
    1. **Kartézské pozice:** Vektor topologií $s = (s_1, \dots, s_8)$, kde $s_i = (x_i, y_i)$. Počet možností: $64^8 \approx 2.81 \times 10^{14}$ stavů.
    2. **Jedna dáma na sloupec:** Vektor řádkových pozic $(y_1, \dots, y_8)$, kde index vektoru fixuje sloupec. Počet možností: $8^8 = 16\,777\,216$ stavů.
    3. **Permutace:** Pokud navíc zakážeme, aby dvě dámy sdílely stejný řádek, kódování se redukuje na čistou permutaci řádků. Počet možností: $8! = 40\,320$ stavů.

#### Práce s omezeními (Constraint Handling)
1. **Reject strategie:** Algoritmus generuje a udržuje striktně pouze přípustná (*feasible/consistent*) řešení. Jakýkoli nepřípustný krok je okamžitě zahozen.
2. **Penalizační strategie (Penalizing):** Prohledávací prostor se rozšíří i o nepřípustná řešení. Účelová funkce je penalizována za každé porušené omezení. Váhy penalizací bývají o řád vyšší než původní kritérium, aby byl algoritmus naváděn zpět do přípustné oblasti.
3. **Opravné strategie (Repairing):** Pokud algoritmus vygeneruje nepřípustné řešení, aplikuje se specifická opravná heuristika, která jej transformuje na přípustné. Prohledávání následně pokračuje s touto opravenou verzí.

#### Účelová funkce (Objective Function)
* **Samospustitelná (Self-sufficient):** Původní matematické kritérium problému lze přímo použít pro optimalizaci (např. minimalizace sumy vzdáleností u TSP).
* **Naváděcí (Guiding):** Pokud je původní vyhodnocení typu True/False (např. u splnitelnosti SAT formulí), nelze prohledávání efektivně řídit. Účelová funkce se proto transformuje na naváděcí funkci – u $k$-SAT se maximalizuje *počet aktuálně splněných klauzulí*, což poskytuje jemnější gradient pro lokální prohledávání.

---
## Lokální prohledávání

**Hill Climbing (Horolezecký algoritmus)** je nejstarší a nejjednodušší lokální metaheuristika. Pracuje nad jedním kompletním řešením, které se v každém kroku pokouší nahradit lepším sousedním řešením.

#### Základní algoritmus (pro minimalizační problém)
```pascal
s := s0; // Vygenerování počátečního řešení s0
while stopping condition not satisfied do
    generate(N(s)); // Vygenerování kandidátních sousedů z okolí N(s)
    if no better neighbor exist then 
        terminate; // Dosaženo lokální optimum
    s := s'; // Náhrada aktuálního stavu lepším sousedem s' ∈ N(s)
end while
output: finální nalezené řešení (lokální optimum)
```

### Strategie výběru souseda

Při lokálním prohledávání určuje tato strategie, jakým způsobem se z vygenerovaného okolí $N(s)$ vybere kandidát, který nahradí aktuální řešení:

* **Best improvement (Steepest descent):** Prohledá se kompletně celé okolí $N(s)$ a vybere se ten soused, který přináší největší možné zlepšení účelové funkce. Pro rozsáhlá okolí je tato strategie časově velmi náročná.
* **First improvement:** Okolí se prochází sekvenčně a vybere se hned první nalezený soused, který zlepšuje aktuální řešení. Výrazně tak urychluje výpočetní čas jednoho kroku (iterace).
* **Random selection:** Náhodně se vybere podmnožina sousedů a z nich se zvolí ten, který zlepšuje aktuální stav.


### Koncepty okolí (Neighborhood) a lokální optimum

#### Formální definice okolí
Funkce okolí $N$ je zobrazení $N: S \rightarrow 2^S$, které každému řešení $s \in S$ přiřadí množinu sousedních řešení $N(s) \subset S$. 

V diskrétní optimalizaci je okolí typicky definováno pomocí metriky vzdálenosti $d$ navázané na přechodový operátor (tzv. *move operator*):
$$N(s) = \{s' \in S \mid d(s', s) \le \epsilon\}$$

Pokud $d(s, s') \le 1$, znamená to změnu hodnoty právě jedné proměnné. Celý prohledávací prostor lze vizualizovat jako graf, kde uzly jsou kompletní řešení a sousední stavy jsou propojeny hranami.

#### Lokální optimum
Řešení $s \in S$ je **lokálním optimem** vzhledem k okolí $N$, pokud žádný z jeho sousedů nemá lepší hodnotu účelové funkce. Pro minimalizační problém platí:
$$f(s) \le f(s') \quad \forall s' \in N(s)$$

#### Okolí pro permutační problémy (rozvrhování a trasování)
* **Poziční okolí (Position-based):** Využívá operátor vkládání (**insertion** operator) – prvek se z jedné pozice vyjme a vloží se na jinou pozici v sekvenci.
* **Pořadové okolí (Order-based):**
    * Operátor výměny (**exchange / swap** operator) – prohodí pozice dvou prvků.
    * Operátor inverze (**inversion** operator) – obrátí pořadí prvků v celé vybrané podsekvenci.

#### Okolí pro Problém obchodního cestujícího (TSP)
* **$k$-distance:** Výměna pozic $k$ měst. Pro základní $2$-distance (výměna dvou měst) je velikost kompletního okolí rovna:
  $$\frac{n(n - 1)}{2} \quad \text{(kde } n \text{ je počet měst)}$$
* **$k$-opt:** Odstraní z okružní cesty $k$ hran a nahradí je $k$ jinými hranami tak, aby vznikla nová validní okružní cesta. Pro $2$-opt (odstranění dvou sousedních hran a křížové přepojení) je velikost okolí:
  $$\left[\frac{n(n - 1)}{2} - n\right]$$

#### Inkrementální vyhodnocování okolí (Incremental Evaluation)
Kompletní výpočet účelové funkce $f(s)$ od nuly pro každého kandidáta v okolí je nejnákladnější operací celého algoritmu (tzv. naivní vyhodnocení). Cílem je implementovat **inkrementální vyhodnocení** pomocí výpočtu diferenční změny $\Delta f$.

*Příklad (2-opt u TSP):* Namísto přepočítávání délky celé cesty se spočte pouze rozdíl způsobený odebráním původních hran a přidáním nových hran:
$$\Delta f = \text{distance}(A, E) + \text{distance}(C, D) - \text{distance}(A, D) - \text{distance}(C, E)$$
$$f(s') = f(s) + \Delta f$$


### Iterované lokální prohledávání (ILS)

Základní Hill Climbing uvízne v prvním lokálním optimu. Pro pokračování prohledávání se používají techniky restartu.

* **Multistart local search:** Opakované spouštění lokálního prohledávání, přičemž v každé nové iteraci je počáteční řešení $s_0$ vygenerováno zcela náhodně od nuly. Dochází ke ztrátě dosavadní informace z prohledávání.
* **Iterated Local Search (ILS):** Staví na historii prohledávání. Místo náhodného restartu aplikuje na nalezené lokální optimum strukturální narušení (**perturbaci**) a z tohoto modifikovaného stavu spustí lokální prohledávání znovu.

#### Pseudokód ILS
```pascal
s = s0; // Vygenerování počátečního řešení
s* = local_search(s); // Dosažení prvního lokálního optima
repeat
    s' = perturb(s*, search_history); // Aplikace perturbace na optimum
    s'_* = local_search(s'); // Lokální prohledávání z narušeného stavu s'
    s* = accept(s*, s'_*, search_memory); // Akceptační kritérium pro novou iteraci
until stopping_criteria;
output: nejlepší globálně nalezené řešení
```

#### Koncept perturbace
Perturbace představuje velký náhodný krok v aktuálním řešení (např. část prvků se fixuje a část se promíchá). Musí být:
* **Dostatečně velká:** Aby algoritmus dokázal vyskočit z lůžka přitažlivosti aktuálního lokálního optima (příliš malá změna způsobí, že lokální prohledávání zkolabuje zpět do stejného bodu).
* **Přiměřeně malá:** Pokud by byla změna stoprocentní, ILS degraduje na čistý náhodný *Multistart*.


## Metaheuristiky s jedním řešením

Pokročilé metaheuristiky s jedním řešením umožňují průběžný únik z lokálních optim tím, že za určitých podmínek dovolují přechod na horší sousední stav.

#### Simulované žíhání (Simulated Annealing - SA)
Klasická stochastická metaheuristika inspirovaná procesem ochlazování kovů v metalurgii. Využívá náhodný výběr souseda. Zlepšující krok je přijat vždy. Horší krok je přijat stochasticky s pravděpodobností:
$$P(\text{přijetí}) = e^{-\frac{\Delta f}{T}}$$
Kde $\Delta f = f(s') - f(s) > 0$ je míra zhoršení a $T$ je globální parametr teploty. Teplota se v čase postupně snižuje: na začátku je vysoká (vysoká diversifikace, přijímají se i špatné kroky), na konci konverguje k nule (čistý Hill Climbing / intenzifikace).

#### Record-to-record travel (RRT)
Deterministický algoritmus inspirovaný simulovaným žíháním pro minimalizační úlohy. Udržuje si hodnotu historicky nejlepšího nalezeného řešení (`RECORD`). Nové náhodně vygenerované sousední řešení $s'$ je akceptováno, pokud jeho hodnota nepřekročí `RECORD` o více než pevnou prahovou odchylku $D$ ($D > 0$).

```pascal
input: deviation D > 0
s = s0; RECORD = f(s);
repeat
    generate a random neighbor s' ∈ N(s);
    if f(s') < RECORD + D then s = s'; // Akceptace řešení pod prahem
    if f(s') < RECORD then RECORD = f(s'); // Aktualizace nejlepšího záznamu
until stopping_criteria_satisfied;
output: nejlepší nalezené řešení
```

#### Algoritmus Velké potopy (Great Deluge - GD)
Deterministický algoritmus založený na analogii s horolezcem, který se snaží při potopě udržet nohy v suchu. V minimalizační verzi algoritmu představuje `LEVEL` neustále klesající hladinu vody (strop), přičemž aktuální řešení se musí striktně nacházet pod touto hladinou ($f(s') < \text{LEVEL}$).

```pascal
s = s0; 
choose rain speed UP > 0; // Rychlost poklesu hladiny
choose initial water level LEVEL; // Počáteční strop
repeat
    generate a random neighbor s' ∈ N(s);
    if f(s') < LEVEL then s = s'; // Akceptace, pokud je řešení pod hladinou vody
    LEVEL = LEVEL - UP; // Lineární snížení hladiny vody pro další krok
until stopping_criteria_satisfied;
output: nejlepší nalezené řešení
```
*Parametr UP* je kritický: vysoká hodnota vede k rychlému výpočtu, ale nízké kvalitě výsledku; velmi malá hodnota poskytuje vysokou kvalitu za cenu dlouhého výpočetního času.

#### Zakázané prohledávání (Tabu Search - TS)
Deterministické rozšíření lokálního prohledávání, které využívá paměťové struktury k zabránění cyklení v okolí lokálních optim. V každém kroku vybere **nejlepšího přípustného souseda** z celého $N(s)$, i když tento soused přináší zhoršení účelové funkce.

* **Tabu list (krátkodobá paměť):** FIFO fronta (typicky o pevné délce 5–9 prvků), která uchovává nedávno provedené kroky. Ukládání celých řešení je paměťově náročné, proto se ukládají pouze **atributy tahů** (např. u TSP dvojice měst, které byly prohozeny). Tah s těmito atributy je po určitý počet iterací zakázaný (*tabu*).
* **Ašpirační kritérium (Aspiration criteria):** Podmínka, která umožňuje ignorovat tabu status. Nejčastěji se aplikuje tehdy, pokud by zakázaný tah vedl k řešení, které je prokazatelně lepší než doposud nejlepší nalezené řešení v celé historii běhu algoritmu ($f(s') < f(s_{best})$).

```pascal
s := s0; Initialize tabu list;
repeat
    find best admissible neighbor s' ∈ N(s); // (není tabu, nebo platí ašpirační kritérium)
    s := s';
    update tabu list & aspiration conditions;
until stopping_criteria_satisfied;
output: nejlepší nalezené řešení
```


### Prohledávání s velmi velkým okolím (Very Large-Scale Neighborhood Search - VLNS)

#### Definice velikosti okolí
* **Malé okolí:** Velikost okolí (počet sousedů) roste jako nízko-stupňová polynomální funkce vzhledem k velikosti vstupní instance (např. lineárně či kvadraticky).
* **Velmi velké okolí:** Velikost okolí roste exponenciálně nebo jako polynom vysokého řádu vzhledem k velikosti instance.
* **Hlavní výzva:** Jak najít nejlepšího nebo alespoň prokazatelně zlepšujícího souseda v obrovském prostoru efektivně, bez kompletní enumerace všech stavů. Cílem je dosáhnout kompromisu mezi výpočetní náročností jednoho kroku a kvalitou nalezeného okolního stavu.

#### Hlavní rodiny metod v rámci VLNS
1. **Metody s proměnlivou hloubkou (Variable-depth methods)**
2. **Algoritmy založené na síťových tocích (Network flows based improvement):** Vyhledání nejlepšího souseda se transformuje na exaktně a rychle řešitelný grafový problém (např. hledání minimálního řezu nebo maximálního toku).
3. **Efektivně řešitelné speciální případy (Efficiently solvable special cases):** Okolí je nadefinováno tak, aby odpovídalo matematické struktuře řešitelné v polynomálním čase (např. dynamickým programováním).
4. **Vyhazovací řetězce (Ejection chains)**
5. **Large Neighborhood Search (LNS)**
6. **Variable Neighborhood Search (VNS)**

#### Vyhazovací řetězce (Ejection Chains)
Sekvence lokálních opravných operací (přesunů a výměn). 
* **Princip:** První oprava odstraní jeden defekt (porušení omezení), ale vyvolá vznik jiného defektu. Následující krok odstraní tento sekundární defekt, ale vyvolá terciární, atd. Řetězec úspěšně končí, jakmile se v nějakém kroku podaří defekt zcela eliminovat bez generování nového.
* *Cyklický vyhazovací řetězec (Cyclic ejection chain):* Výměny prvků tvoří uzavřený cyklus.
* *Příklad (Capacitated Vehicle Routing):* Přesouvání přetížených zákazníků z jedné trasy vozidla na druhou tak dlouho, dokud není splněna kapacita všech zúčastněných vozidel.

#### Large Neighborhood Search (LNS)
Metaheuristika založená na opakované aplikaci metod destrukce (**Destroy**) a následné opravy (**Repair**). Algoritmus záměrně znehodnotí velkou část aktuálního stavu a následně ji rekonstruuje jiným (ideálně lepším) způsobem.

*Příklad kombinatorické velikosti (CVRP, 100 zákazníků, 15 % destruováno):* Počet způsobů, jak vybrat 15 zákazníků k odebrání, je:
$$\binom{100}{15} = \frac{100!}{15! \times 85!} \approx 2.5 \times 10^{17} \text{ možných sousedů v rámci jednoho kroku LNS.}$$

* **Metody destrukce (Destroy methods):** Zruší přiřazení vybrané podmnožiny rozhodovacích proměnných.
    * *Random removal:* Náhodný výběr entit k odebrání.
    * *Worst-case removal:* Odebere ty proměnné, které nejvíce negativně přispívají k aktuální hodnotě účelové funkce.
    * *Shaw removal:* Odebere entity, které jsou si strukturně velmi podobné (idea: podobné entity lze při opravě mezi sebou nejsnáze zaměnit a dosáhnout tak strukturálního zlepšení).
* **Metody opravy (Repair methods):** Hledají optimální nebo suboptimální přiřazení pro uvolněné proměnné.
    * *Greedy insertion:* Hladové postupné vkládání prvků na lokálně nejlepší pozice.
    * *Regret insertion:* Vypočte pro každou entitu ztrátu skóre (*score loss*) mezi jejím nejlepším a druhým nejlepším možným přiřazením. Algoritmus přednostně umisťuje entitu s nejvyšší hodnotou "lítosti" (*regret*), pro kterou by později nemuselo zbýt přípustné a kvalitní místo.
* **Stupeň destrukce (Degree of destruction):** Klíčový parametr. Pokud je zničena příliš malá část, algoritmus neopustí lokální optimum. Pokud je zničena příliš velká část (např. >80 %), LNS degraduje na čistý náhodný *Multistart*.
* **Pseudokód LNS:**
```pascal
s := initialSolution; s_best := initialSolution;
repeat
    s' := s;
    destroy part of s' using a destroy heuristic;
    repair s' using a repair heuristic;
    if f(s') < f(s_best) then s_best := s';
    if accept(s', s) then s := s'; // např. akceptace dle pravidel simulovaného žíhání
until stopping_criteria_satisfied;
return s_best;
```

#### Variable Neighborhood Search (VNS)
Strategie postavená na systematické změně struktur okolí během prohledávání. Využívá myšlenku, že lokální optimum vůči okolí $N_i$ nemusí být lokálním optimem vůči okolí $N_j$. Změna operátoru mění topologii a lokální extrémy tzv. *fitness landscape* (krajiny zdatnosti).

* **Variable Neighborhood Descent (VND):** Čistě deterministická verze VNS. Prohledává sekvenčně řadu komplementárních okolí $N_1, N_2, \dots, N_{l_{max}}$. Pokud v aktuálním okolí $N_l$ najde nejlepšího souseda, který zlepšuje stav, provede se skok a index okolí se resetuje na $l = 1$. Pokud v daném okolí žádné zlepšení neexistuje, postoupí se do jiného/většího okolí ($l = l + 1$).
    * *Pseudokód VND:*
    ```pascal
    s := s0; l := 1;
    while l <= l_max do
        find the best neighbor s' ∈ N_l(s);
        if f(s') < f(s) then
            s := s'; l := 1; // Zlepšení nalezeno, návrat k prvnímu okolí
        else
            l := l + 1; // Přechod na širší/jinou strukturu okolí
    end while
    output: nejlepší nalezené řešení
    ```
* **Basic VNS Algorithm:** Kombinuje stochastickou fázi „třesení“ (**shaking**) pro únik z lůžka lokálního optima s následným intenzivním lokálním prohledáváním (kterým bývá nejčastěji právě VND). Okolí pro shaking mají postupně se zvyšující intenzitu/velikost (např. přepínání k-opt operátorů).
    * *Pseudokód Basic VNS:*
    ```pascal
    s := s0;
    repeat
        k := 1;
        repeat
            pick a random solution s' ∈ N_k(s); // Shaking (stochastický skok)
            s'' := local_search(s');            // Lokální prohledávání (např. pomocí VND)
            if f(s'') < f(s) then
                s := s''; k := 1;               // Úspěch, návrat k nejmenšímu shaking okolí
            else
                k := k + 1;                     // Selhání, zvětšení intenzity shakingu
        until k == k_max;
    until stopping_criteria_satisfied;
    output: nejlepší nalezené řešení
    ```


### Softwarové nástroje pro lokální prohledávání a metaheuristiky

Pro nasazení v praxi se využívají zavedené softwarové frameworky:
* **LocalSolver:** Komerční optimalizační solver postavený primárně na technikách lokálního prohledávání a VLNS.
* **OptaPlanner:** Open-source framework (udržovaný společností Red Hat) v jazyce Java, implementující různé typy lokálních i populačních metaheuristik pro plánování a rozvrhování.
* **ParadisEO:** Modulární C++ framework určený pro návrh metaheuristik (jak single-solution, tak populačních).
* **ECJ:** Rozsáhlý výzkumný systém v Javě zaměřený na evoluční výpočty.

---
## Populační metaheuristiky

Populační metaheuristiky pracují s celou množinou (populací) kompletních nebo částečných řešení současně. Oproti metodám s jedním řešením (které intenzivně prohledávají lokální okolí) populační algoritmy excelují v **diversifikaci** (globálním průzkumu) stavového prostoru, čímž snižují riziko uvíznutí v nevýhodném lokálním optimu. 

Mezi hlavní zástupce patří genetické algoritmy, evoluční strategie, genetické programování, optimalizace mravenčí kolonou (ACO), optimalizace hejnem částic (PSO) či umělé imunitní systémy.

### Obecná šablona populačního algoritmu
```pascal
P = P0; // Inicializace a generování počáteční populace
t = 0;
repeat
    P'_t = generate(Pt);       // Vygenerování nové populace (nových kandidátů)
    Pt+1 = select_population(Pt ∪ P'_t); // Výběr přeživších jedinců do další generace
    t = t + 1;
until stopping_criteria_satisfied;
output: nejlepší nalezené řešení (nebo množina řešení)
```

### Klasifikace: Evoluce vs. Sdílená paměť
Populační metaheuristiky lze rozdělit do dvou základních kategorií podle toho, jakým způsobem jedinci v populaci interagují:
1. **Založené na evoluci (Evolution based):** Jedinci z aktuální populace jsou přímo vybráni a reprodukováni pomocí biologicky inspirovaných operátorů (křížení, mutace). Přenáší se genetická informace konkrétních jedinců.
    * *Příklady:* Genetické algoritmy, Evoluční strategie.
2. **Založené na sdílené paměti (Blackboard based):** Jedinci přímo nereprodukují své genetické řetězce, ale svými vlastnostmi přispívají do společně sdílené paměti (tzv. "tabule"). Nová populace kandidátů se v dalším kroku generuje čistě na základě stavu této sdílené paměti.
    * *Příklady:* Feromonová matice u optimalizace mravenčí kolonou (ACO), pravděpodobnostní model učení u EDA (*Estimation of Distribution Algorithms*).

### Koncepty inicializace populace
Kvalita a rozmanitost počáteční populace $P_0$ (obvykle o velikosti 20–100 jedinců) zásadně ovlivňuje konvergenci výpočtu:
* **Náhodná generování (Random):** Jedinci se generují zcela stochasticky. Pro silně omezené problémy však může být nalezení přípustného náhodného stavu výpočetně velmi náročné.
* **Sekvenční diversifikace (Sequential diversification):** Jedinci se generují postupně za účelem maximalizace vzdálenosti mezi nimi. Nové řešení je přijato pouze tehdy, pokud je jeho vzdálenost od všech doposud vygenerovaných řešení alespoň $\Delta$. Má vysokou výpočetní režii.
* **Paralelní diversifikace (Parallel diversification):** Jedinci jsou generováni nezávisle a paralelně s cílem pokrýt co největší plochu prohledávacího prostoru. Často jde o problém stejně složitý jako samotná optimalizace.
* **Heuristická inicializace (Heuristic initialization):** Populace se naplní řešeními, která byla předpřipravena rychlou hladovou heuristikou nebo lokálním prohledáváním. Hrozí zde však rychlá ztráta diverzity a předčasná konvergence do lokálního extrému.

### Kritéria ukončení (Stopping Criteria)
* **Statické procedury (známé a priori):** Pevný počet iterací (generací), limit na CPU čas, nebo maximální počet vyhodnocení účelové funkce.
* **Adaptivní procedury (dynamické):** Ukončení po dosažení fixního počtu generací bez jakéhokoli zlepšení nejlepšího jedince, dosažení uspokojivé hodnoty fitness, nebo pokles diverzity v populaci pod kritickou mez.

---

## Evoluční algoritmy

Evoluční algoritmy (EA) modelují optimalizační proces jako digitální simulaci Darwinovy evoluční teorie o přírodním výběru a přežití nejsilnějších (*survival of the fittest*).

### Základní terminologie
* **Populace (Population):** Množina aktuálně žijících kandidátních řešení.
* **Chromozom / Jedinec (Individual):** Zakódované kompletní řešení problému.
* **Gen (Gene):** Konkrétní rozhodovací proměnná (pozice v řetězci chromozomu).
* **Alela (Allele):** Konkrétní hodnota, kterou daná proměnná (gen) nabývá.
* **Zdatnost (Fitness):** Hodnota účelové funkce jedince. V kontextu EA se typicky maximalizuje (čím vyšší fitness, tím kvalitnější řešení).

### Základní šablona evolučního algoritmu
```pascal
generate(P0); // Generování počáteční populace
t = 0;
while not termination-criterion(P(t)) do
    evaluate(Pt);                     // Vyhodnocení fitness u všech jedinců
    P'_t = selection(Pt);            // Strategie výběru rodičů pro páření
    P'_t = reproduction(P'_t);        // Aplikace křížení a mutace -> vznik potomků
    evaluate(P'_t);                   // Vyhodnocení fitness nových potomků
    Pt+1 = replace(Pt, P'_t);        // Strategie nahrazování (výběr přežívajících)
    t = t + 1;
end while;
output: nejlepší nalezený jedinec
```

### Hlavní větve evolučních algoritmů
Evoluční výpočty se historicky vyvíjely na různých akademických pracovištích do specifických forem:
* **Genetické algoritmy (GA):** (Holland, USA). Původně navržené pro binární reprezentaci (dnes upravené pro jakoukoli). Klade hlavní důraz na operátor **křížení** nad dvěma rodiči. Mutace slouží pouze jako sekundární operátor pro udržení diverzity. Využívá fixní pravděpodobnosti křížení $p_c$ a mutace $p_m$.
* **Evoluční strategie (ES):** (Rechenberg & Schwefel, Německo). Vyvinuty primárně pro kontinuální optimalizaci a vektory reálných čísel. Křížení je zde vzácné (vhodné pro problémy, kde smysluplné křížení nelze navrhnout), hlavní evoluční pohon zajišťuje **mutace**. Používá se striktní elitářské nahrazování. Velikost populace rodičů se značí $\mu$, velikost populace potomků $\lambda$ ($\lambda \ge \mu$).
    * *(1 + 1)-ES:* Jeden rodič vygeneruje jednoho potomka. Potomek nahradí rodiče pouze tehdy, pokud je lepší; jinak je zahozen.
    * *$(\mu + \lambda)$-ES:* $\mu$ rodičů vygeneruje $\lambda$ potomků. Nová populace $\mu$ přeživších se vybírá spojením a setříděním *rodičů i potomků dohromady* (čistý elitismus).
    * *$(\mu, \lambda)$-ES:* Nová populace $\mu$ jedinců se vybírá *striktně pouze z $\lambda$ potomků*. Žádný rodič nepřežívá (vhodné pro sledování pohybujících se cílů v dynamickém prostředí).
* **Genetické programování (GP):** (Koza, USA). Jedinci nejsou datové vektory, ale přímo **vykonatelné programy** reprezentované nelineárně pomocí stromových grafových struktur. Slouží k automatickému generování programů pro řešení úkolů.
    * *Příklad (Symbolická regrese):* Cílem je nalézt matematický vzorec, který nejlépe odpovídá naměřeným datovým bodům. Fitness jedince (programu) je definována jako minimalizace sumy čtverců odchylek vůči testovacím bodům.

### Strategie výběru rodičů (Selection Strategies)
Určují, jakým způsobem jsou z populace vybíráni jedinci, kterým bude umožněno se rozmnožovat.

* **Ruletový výběr (Roulette wheel selection):** Pravděpodobnost výběru jedince $p_i$ je přímo úměrná jeho fitness $f_i$ vůči sumě fitness celé populace:
  $$s_i \in \text{Pop} \implies p_i = \frac{f_i}{\sum_{j=1}^{n} f_j}$$
  *Problém:* Pokud se v populaci objeví jeden dominantní jedinec s extrémně vysokou fitness oproti ostatním, ruleta ho bude vybírat téměř neustále. To vede k rychlému zaplavení populace jeho kopiemi a k **předčasné konvergenci** do lokálního optima.
* **Stochastické univerzální vzorkování (Stochastic universal sampling - SUS):** Odstraňuje nectnosti rulety. Na pomyslné ruletové kolo se umístí $\mu$ rovnoměrně rozmístěných ukazatelů (prahů). Jediným roztočením kola se naráz vybere všech $\mu$ rodičů, což dává šanci i slabším jedincům a stabilizuje selekční tlak.
* **Turnajový výběr (Tournament selection):** Náhodně se vybere $k$ jedinců z populace. Ti mezi sebou srovnají fitness a absolutní vítěz (nejlepší z nich) se stává rodičem. Pro výběr $\mu$ rodičů se tento turnaj nezávisle opakuje $\mu$-krát. Parametr $k$ řídí intenzitu selekčního tlaku.
* **Výběr podle pořadí (Rank-based selection):** Jedinci se seřadí podle výkonnosti a je jim přiřazena hodnota pořadí (*rank*). Výběrová pravděpodobnost se následně počítá nad tímto pořadím (např. lineárním škálováním) namísto absolutní hodnoty fitness. Tím se eliminuje vliv obřích rozdílů ve fitness hodnotách.

### Reprodukční strategie (Reproduction Strategies)

#### 1. Mutace (Mutation)
Představuje malé stochastické narušení jednoho vybraného jedince. Pravděpodobnost mutace jednoho genu $p_m$ se standardně nastavuje velmi nízko ($p_m \in [0.001, 0.01]$). Typickým doporučením je $p_m = 1/k$, kde $k$ je počet genů v chromozomu (v průměru se zmutuje právě jeden gen na jedince).
* *Binární reprezentace:* Invertování hodnoty bitu ($0 \rightarrow 1$, $1 \rightarrow 0$).
* *Diskrétní reprezentace:* Změna hodnoty genu na jiný náhodný znak z dané abecedy.
* *Permutační reprezentace:* Aplikace permutačních operátorů – vložení (*insertion*), prohození dvou prvků (*swapping*) nebo otočení podsekvence (*inversion*).

#### 2. Křížení (Crossover)
Binární operátor, který kombinuje genetickou informaci dvou rodičů za účelem vytvoření potomků. Aplikuje se s pravděpodobností $p_c \in [0.45, 0.95]$.

* **Pro lineární reprezentace (mimo permutace):**
    * *Jednobodové křížení (1-point crossover):* Náhodně se zvolí jeden dělicí bod $k$. Potomci vzniknou prohozením koncových segmentů rodičů za tímto bodem.
      $$\text{Rodiče: } 10011100 \mid 1001 \quad \times \quad 01110010 \mid 0111 \implies \text{Potomci: } 10011100 \mid 0111 \quad \text{a} \quad 01110010 \mid 1001$$
    * *Vícebodové křížení (n-point crossover):* Zvolí se $n$ dělicích bodů a rodičovské segmenty se mezi potomky střídají obkročmo.
    * *Uniformní křížení (Uniform crossover):* Ignoruje ucelené segmenty. O každém genu potomka se rozhoduje nezávisle a náhodně – s padesátiprocentní pravděpodobností se gen zkopíruje z prvního nebo z druhého rodiče.

* **Pro permutační reprezentace:**
    Standardní křížení by v permutacích způsobilo duplicitu prvků a ztrátu jiných. Používají se pokročilé mapovací techniky, například **Pořadové křížení (Order crossover - OX)**:
    1. V prvním rodiči se náhodně zvolí dva dělicí body a celý tento vnitřní segment hodnot se beze změny zkopíruje do prvního potomka.
    2. Druhý rodič se začne procházet cyklicky od druhého dělicího bodu doprava. Prvky, které *již jsou obsaženy* v segmentu z prvního rodiče, se přeskakují. Zbylé prvky se postupně vkládají do volných pozic potomka, rovněž od druhého dělicího bodu dále.

### Strategie nahrazování (Replacement Strategies / Survivor Selection)
Určují, jakým způsobem spolu soupeří stará generace rodičů $P_t$ a nová generace potomků $P'_t$ o přežití do dalšího kroku $P_{t+1}$.
* **Generační nahrazování (Generational replacement):** Celá populace potomků kompletně a bezpodmínečně nahradí populaci rodičů. Riskujeme tím, že můžeme ztratit historicky nejlepšího jedince, pokud jeho potomci nedosáhnou stejné kvality.
* **Ustálený stav (Steady-state replacement):** V každé iteraci se vygeneruje pouze jeden (nebo velmi málo) potomků. Tento potomek v původní populaci typicky nahradí prokazatelně nejhoršího jedince. Jde o extrémně lokálně orientovanou verzi GA.
* **Smíšené strategie:** Nahrazuje se přesně specifikovaný fixní počet $\lambda$ jedinců v populaci o velikosti $\mu$ ($1 < \lambda < \mu$).
* **Elitismus (Elitism):** Zajišťuje, že nejlepší jedinci z obou populací (rodičů i potomků) automaticky a garantovaně postupují do další generace. Elitismus dramaticky urychluje konvergenci, ale zvyšuje riziko uvíznutí v lokálním optimu.

<img alt="img.png" src="img/metody_umele_inteligence/evolalg.png" width="400"/>

---

## Inteligence hejna

Inteligence hejna (*Swarm intelligence*) je subdisciplína inspirovaná kolektivním, decentralizovaným chováním sociálně žijících biologických druhů (mravenci, včely, vosy, hejna ryb či ptáků).

### Hlavní charakteristiky
* Hejno se skládá z velkého množství **jednoduchých, nesofistikovaných agentů** (částic).
* Neexistuje žádné centrální řízení. Agenti komunikují výhradně lokálně a nepřímo prostřednictvím modifikace okolního prostředí (jev zvaný **stigmergie**).
* Celé hejno vykazuje emergentní inteligentní chování vyšší úrovně při kolektivním vyhledávání zdrojů potravy nebo optimálních tras v rozhodovacím prostoru.

### Optimalizace mravenčí kolonou (Ant Colony Optimization - ACO)
Algoritmus simuluje schopnost mravenců najít nejkratší cestu mezi mraveništěm a zdrojem potravy pomocí vylučování chemické stopy – **feromonu**. Feromon je těkavá látka, která se v čase přirozeně odpařuje. Na kratší cestě se mravenci otočí rychleji, feromonová stopa se tam vrství intenzivněji a silnější koncentrace následně přitahuje další mravence.

#### Obecný algoritmus ACO
```pascal
initialize the pheromone trails; // Inicializace feromonové matice
repeat
    for each ant do
        solution construction using the pheromone trail; // Mravenec vytvoří kompletní řešení
    
    // Aktualizace feromonových stop
    evaporation;    // Odpaření části feromonu na všech hranách
    reinforcement;  // Posílení feromonu na úspěšných hranách
until stopping_criteria;
output: nejlepší nalezené řešení
```

#### Matematické operace s feromonem ($\tau$)
Feromonová informace $\tau$ je modelována jako vektor nebo matice parametrů (např. $\tau_{ij}$ reprezentuje množství feromonu na hraně mezi uzly $i$ a $j$).

1. **Odpařování (Evaporation):** Provádí se plošně na všech hranách optimalizačního grafu, aby se zabránilo nekonečnému kumulování feromonu a umožnil se únik z lokálních optim:
   $$\tau_{ij} = (1 - \rho)\tau_{ij} \quad \forall (i, j), \quad \text{kde } \rho \in [0, 1] \text{ je koeficient odpařování.}$$
2. **Posílení (Reinforcement):** Přidání feromonu na hrany. Existují tři přístupy k načasování:
    * *Online step-by-step update:* Mravenec mění feromon ihned při vykonání dílčího kroku na dané hraně.
    * *Online delayed update:* Mravenec aplikuje feromon na celou svou trasu až poté, co úspěšně zkonstruuje celé validní řešení.
    * *Off-line update (nejpopulárnější):* Feromon se aktualizuje až na konci generace, kdy *všichni* mravenci dokončili svá řešení. Posílení se provede pouze na hranách, které tvoří historicky nejlepší trasu (nebo top-$k$ nejlepších tras):
      $$\tau_{ij} = \tau_{ij} + \Delta \quad \forall (i, j) \in \text{best\_solution}$$

#### Aplikace ACO na Problém obchodního cestujícího (TSP)
Při řešení TSP se mravenec nachází v uzlu $i$ a vybírá si následující město $j$ z množiny dosud nenavštívených měst $S$. 

* **Základní pravděpodobnost volby (čistě dle feromonu):**
  $$p_{ij} = \frac{\tau_{ij}}{\sum_{k \in S} \tau_{ik}} \quad \forall j \in S$$
* **Pokročilá pravděpodobnost (s heuristickou informací):**
  Do rozhodování se integruje lokální viditelnost uzlů (heuristická kvalita nezávislá na feromonu), definovaná jako převrácená hodnota fyzické vzdálenosti hran $\eta_{ij} = 1/d_{ij}$:
  $$p_{ij} = \frac{\tau_{ij}^\alpha \times \eta_{ij}^\beta}{\sum_{k \in S} \tau_{ik}^\alpha \times \eta_{ik}^\beta} \quad \forall j \in S$$
  Kde parametry $\alpha$ a $\beta$ definují relativní vliv feromonové stopy vůči čistě geografické blízkosti:
    * $\alpha = 0$: Algoritmus se chová jako čistě stochastické hladové prohledávání (*stochastic greedy search*).
    * $\beta = 0$: Rozhodování probíhá výhradně na základě feromonových stop z minulých generací.


---
## Plánování

Automatické plánování (*Automated Planning*) se zabývá výběrem a časovou organizací akcí s cílem transformovat aktuální stav světa do stavu požadovaného. Tento přístup poskytuje vysokou míru flexibility pro agenty založené na cílech a modelech (*goal-based* a *model-based agents*) v situacích, kdy cíl není bezprostředně dosažitelný jedinou akcí a je nutné uvažovat o budoucích důsledcích chování.

### Formální model (Systém $\Sigma$)
Plánovací prostředí je modelováno jako stavový transformační systém $\Sigma$, který je definován jako trojice:
$$\Sigma = (S, A, E)$$
* **$S$ (Množina stavů):** Rekurzivně spočetná množina všech možných konfigurací světa.
* **$A$ (Množina akcí):** Rekurzivně spočetná množina přechodů, které jsou plně pod kontrolou plánovače (včetně prázdné akce `no-op`).
* **$E$ (Množina událostí):** Rekurzivně spočetná množina vnějších událostí, které plánovač nemá pod kontrolou (včetně neutrální události $\varepsilon$).

Změna stavu je řízena přechodovou funkcí $\gamma$:
$$\gamma: S \times A \times E \rightarrow 2^S \quad \text{případně separátně} \quad \gamma: S \times (A \cup E) \rightarrow 2^S$$

### Cíle plánování
Plánovací úloha hledá posloupnost kroků vedoucí z počátečního stavu $s_0$ do cíle. Definice cíle může mít několik podob:
1. **Cílový stav / Množina stavů:** Specifikace vlastností, které musí koncový stav splňovat.
2. **Omezení nad trajektorií stavů:** Podmínky definující stavy, které musí být během plnění plánu navštíveny, nebo naopak ty, které jsou striktně zakázány.
3. **Optimalizace účelové funkce:** Hledání plánu, který minimalizuje nebo maximalizuje určité kritérium (např. finanční náklady, spotřeba energie, celkový čas trvání).

---

## Reprezentace problému

Aby bylo možné plánovací problémy řešit doménově nezávislými algoritmy, je nutné zavést formální reprezentaci stavů a akcí. Klasický přístup staví na logice prvního řádu (koncepty typu STRIPS).

### Stavy a atomy
Stav je definován jako konečná množina **plně instanciovaných atomů** (neobsahuje žádné volné proměnné). Atomy se dělí na dvě kategorie:
* **Fluenty:** Atomy, jejichž pravdivostní hodnota se může v závislosti na aplikovaných akcích měnit (např. `at(robot1, location2)`).
* **Rigidní atomy:** Atomy, jejichž pravdivostní hodnota je konstantní a nezávislá na stavu systému (např. `adjacent(location1, location2)`).

Při vyhodnocování stavů se striktně uplatňuje **Předpoklad uzavřeného světa (Closed World Assumption - CWA)**: *Jakýkoli atom, který není explicitně uveden v popisu daného stavu, je považován za nepravdivý (neplatný).*

### Plánovací operátory a akce
* **Plánovací operátor ($o$):** Parametrická šablona definovaná jako trojice:
  $$o = (\text{name}(o), \text{precond}(o), \text{effects}(o))$$
  * `name(o)`: Unikátní identifikátor ve tvaru $n(x_1, \dots, x_k)$, kde $x_i$ jsou proměnné (parametry operátoru). Název musí obsahovat všechny proměnné použité v definici.
  * `precond(o)`: Předpodmínky vyjádřené jako množina literálů (atomů nebo jejich negací), které musí ve stavu platit, aby byl operátor aplikovatelný.
  * `effects(o)`: Efekty, tedy literály, které se stanou pravdivými po úspěšném provedení. Efekty nesmí obsahovat rigidní atomy.
* **Akce ($a$):** Plně instanciovaný plánovací operátor, kde byly všechny proměnné nahrazeny konkrétními konstantami (objekty z domény).

<img alt="img.png" src="img/metody_umele_inteligence/operators-predicates.png" width="500"/>


#### Aplikace akce v praxi
Označme $S^+$ jako množinu pozitivních atomů ve stavu $s$ a $S^-$ jako množinu atomů, jejichž negace v daném stavu platí. Akce $a$ je aplikovatelná na stav $s$ tehdy a jen tehdy, pokud:
$$\text{precond}^+(a) \subseteq s \quad \land \quad \text{precond}^-(a) \cap s = \emptyset$$

Výsledný stav po aplikaci akce $a$ na stav $s$ určuje přechodová funkce $\gamma$:
$$\gamma(s, a) = (s \setminus \text{effects}^-(a)) \cup \text{effects}^+(a)$$

### Plánovací problém a plán
Plánovací problém $P$ je formálně reprezentován trojicí:
$$P = (O, s_0, g)$$
Kde $O$ reprezentuje doménový model (množinu operátorů a predikátů), $s_0$ je počáteční stav (obsahující konkrétní objekty) a $g$ je množina instanciovaných literálů zadávajících cíl. Stav $s$ splňuje cíl $g \iff g^+ \subseteq s \land g^- \cap s = \emptyset$. Množina všech vyhovujících stavů se značí $S_g$.

**Plán $\pi$** je uspořádaná sekvence akcí $\langle a_1, a_2, \dots, a_k \rangle$. Plán je platným řešením problému $P$, pokud sekvenční aplikace jeho akcí na počáteční stav vede do cílového stavu:
$$\gamma(s_0, \pi) \in S_g$$

### Jazyk PDDL (Planning Domain Definition Language)
PDDL je standardizovaný formální jazyk určený ke kódování klasických plánovacích úloh. Striktně odděluje fyzikální vlastnosti světa od konkrétního zadání rozdělením do dvou souborů:

1. **Domain File (Doménový soubor):** Definuje typy objektů, predikáty a univerzální operátory (akce).
2. **Problem File (Soubor problému):** Definuje konkrétní instance objektů, výchozí stav světa (`:init`) a cílové podmínky (`:goal`).

#### Příklad: Úloha Gripper (Robot se dvěma rameny přenáší 4 míčky)

**Výsek z doménového souboru (`gripper.pddl`):**
```lisp
(define (domain gripper)
   (:predicates (ROOM ?x) (BALL ?x) (GRIPPER ?x)
                (at-robby ?x) (at-ball ?x ?y) (free ?x) (carry ?x ?y))

   (:action move
       :parameters (?x ?y)
       :precondition (and (ROOM ?x) (ROOM ?y) (at-robby ?x))
       :effect (and (at-robby ?y) (not (at-robby ?x))))
)
```

**Výsek ze souboru problému (`gripper-four.pddl`):**
```lisp
(define (problem gripper-four-balls)
   (:domain gripper)
   (:objects rooma roomb ball1 ball2 ball3 ball4 left right)
   (:init (ROOM rooma) (ROOM roomb)
          (BALL ball1) (BALL ball2) (BALL ball3) (BALL ball4)
          (GRIPPER left) (GRIPPER right) (free left) (free right)
          (at-robby rooma)
          (at-ball ball1 rooma) (at-ball ball2 rooma)
          (at-ball ball3 rooma) (at-ball ball4 rooma))
   (:goal (and (at-ball ball1 roomb) (at-ball ball2 roomb)
               (at-ball ball3 roomb) (at-ball ball4 roomb)))
)
```
*Rozšíření jazyka:* Pokročilé verze PDDL podporují univerzální kvantifikátory (`forall`), podmíněné efekty (`when`), numerické fluenty (měření kapacity, paliva), trvání akcí v čase (`durative actions`) nebo měkké preference.

---

## Plánování se stavovým prostorem

Při plánování se stavovým prostorem (*State Space Planning*) odpovídají uzly prohledávacího stromu přímo konkrétním stavům světa a hrany představují přechody mezi nimi realizované aplikací akcí. Cílem je nalézt cestu z počátečního stavu do libovolného cílového stavu.

### 1. Dopředné plánování (Forward Planning / Progression)
Algoritmus začíná v počátečním stavu $s_0$ a prohledává prostor směrem dopředu k cíli aplikací akcí, jejichž předpodmínky jsou v aktuálním stavu splněny.


<img alt="img.png" src="img/metody_umele_inteligence/forward.png" width="400"/>

#### Algoritmus dopředného plánování
```pascal
s = s0;
π = the empty plan;
loop
    if s satisfies g then return π; // Cíl byl úspěšně splněn
    
    // Množina všech plně instanciovaných aplikovatelných akcí
    E = {a | a is a ground instance of an operator in O and precond(a) is true in s};
    if E == ∅ then return failure;   // Slepá ulička
    
    nondeterministically choose an action a ∈ E;
    s = γ(s, a);                     // Provedení přechodu do nového stavu
    π = π . a;                       // Připojení akce na konec plánu
```

#### Vlastnosti a implementace
* **Korektnost (Soundness):** Algoritmus je korektní. Pokud vrátí plán, jedná se o validní řešení, což lze snadno ověřit dopřednou simulací $s = \gamma(s_0, \pi)$.
* **Úplnost (Completeness):** Algoritmus je úplný. Pokud pro daný problém existuje alespoň jedno řešení, existuje v nedeterminismu větev, která ho nalezne.
* **Deterministické varianty:** V reálných plánovačích je nedeterminismus nahrazen standardními vyhledávacími algoritmy. Prohledávání do šířky (BFS) je úplné a optimální, ale paměťově neúnosné. Prohledávání do hloubky (DFS) vyžaduje striktní detekci cyklů na prohledávané větvi. V moderní praxi dominuje **algoritmus $A^*$** využívající pokročilé doménově nezávislé heuristiky.
* **Faktor větvení (Branching Factor):** Hlavní nevýhoda dopředného prohledávání. Počáteční stav může generovat obrovské množství aplikovatelných akcí, které vůbec nesouvisí s finálním cílem. Tento problém se řeší prořezáváním prostoru (např. ukládáním navštívených stavů do paměti; pokud dva různé dílčí plány $\pi_1$ a $\pi_2$ dosáhnou identického stavu, delší z nich se již dále nerozbaluje).

### 2. Zpětné plánování (Backward Planning / Regression)
Zpětné plánování začíná od popisu cíle $g$ (nikoli od konkrétního stavu, protože vyhovujících stavů může být konečné či nekonečné množství) a postupuje pozpátku směrem k počátečnímu stavu $s_0$ pomocí vytváření podcílů.


<img alt="img_1.png" src="img/metody_umele_inteligence/backward.png" width="400"/>

#### Relevantní akce
Aby bylo možné provést krok zpět, musíme vybrat akci, která je pro aktuální cíl **relevantní**. Akce $a$ je relevantní pro cíl $g$ tehdy a jen tehdy, pokud:
1. Akce prokazatelně přispívá k dosažení cíle: $g \cap \text{effects}(a) \neq \emptyset$.
2. Efekty akce nejsou v přímém konfliktu s cílem: $g^- \cap \text{effects}^+(a) = \emptyset \quad \land \quad g^+ \cap \text{effects}^-(a) = \emptyset$.

Při splnění těchto podmínek lze definovat **regresní množinu** $\gamma^{-1}(g, a)$, která reprezentuje nový podcíl (množinu podmínek, které musely platit bezprostředně před aplikací akce $a$, aby se dosáhlo cíle $g$):
$$\gamma^{-1}(g, a) = (g \setminus \text{effects}(a)) \cup \text{precond}(a)$$

#### Algoritmus zpětného plánování
```pascal
π = the empty plan;
loop
    if s0 satisfies g then return π; // Počáteční stav splňuje aktuální podcíl
    
    // Množina všech plně instanciovaných relevantních akcí
    A = {a | a is a ground instance of an operator in O and γ^-1(g, a) is defined};
    if A == ∅ then return failure;
    
    nondeterministically choose an action a ∈ A;
    π = a . π;                       // Pozor: Akce se řadí na ZAČÁTEK plánu
    g = γ^-1(g, a);                  // Aktualizace cíle na nový podcíl
```

#### Vlastnosti a limity
* Algoritmus je korektní a úplný. Deterministická implementace vyžaduje detekci cyklů (pokud v sekvenci cílů nastane situace $g_i \subseteq g_k$ pro $i < k$, prohledávání dané větve se ukončí).
* **Faktor větvení:** Zpětné prohledávání mívá často menší faktor větvení než dopředné, protože ignoruje akce, které nijak nepřispívají k naplnění cíle. Stále však může narazit na neefektivitu. Pokud např. robot potřebuje být na pozici `loc51` a existuje do ní přímá cesta z padesáti různých míst (`loc1` až `loc50`), vygeneruje algoritmus 50 relevantních akcí, ačkoli v této fázi plánování je konkrétní startovní pozice robota nepodstatná.

### 3. Částečně instanciované zpětné plánování (Lifted Backward Planning)
Tato technika řeší problém velkého větvení zpětného plánování tím, že akce neinstancuje na konkrétní konstanty okamžitě, ale ponechává parametry akcí jako volné proměnné, dokud to není nezbytně nutné.

* **Standardizace (Standardization):** Vytvoření čisté kopie operátoru s novými, dosud nepoužitými proměnnými.
* **MGU (Most General Unifier):** Nejobecnější unifikátor, neboli minimální nutná substituce proměnných, která sjednotí atom v cíli s atomem v efektech operátoru.

#### Algoritmus (Lifted verze)
```pascal
π = the empty plan;
loop
    if s0 satisfies g then return π;
    
    // Hledá se operátor a unifikace vůči aktuálnímu cíli
    A = {(o, θ) | o is a standardization of an operator in O,
                 θ is an MGU for an atom of g and an atom of effects(o),
                 and γ^-1(θ(g), θ(o)) is defined};
    if A == ∅ then return failure;
    
    nondeterministically choose a pair (o, θ) ∈ A;
    π = concatenation of θ(o) and θ(π); // Použití unifikovaného operátoru
    g = γ^-1(θ(g), θ(o));               // Regrese s dosazenými vazbami
```

*Příklad:* Máme cíl `at(robot, loc1)`. Použijeme operátor `move(r, l, m)` s efektem `at(r, m)`. Unifikátor $\theta$ provede vazbu $\{r \rightarrow \text{robot}, m \rightarrow \text{loc1}\}$. Parametr startovního místa $l$ zůstává jako volná proměnná $l_1$. Nový podcíl bude obsahovat podmínku `at(robot, l1)` a `adjacent(l1, loc1)`. Tím se v jednom kroku pokryje všech 50 potenciálních lokací bez větvení.

*Kompromis:* Použitím volných proměnných se výrazně zmenšuje faktor větvení prohledávacího stromu, avšak výměnou za výpočetně mnohem náročnější kontrolu cyklů a správu konzistence unifikačních omezení.


---
## Práce s neurčitostí

Uvažování za neurčitosti, neúplnosti či vágnosti informací je klíčovou vlastností pokročilých systémů umělé inteligence. Historicky se od čistě logického odvozování (které v reálném světě naráželo na rigiditu) přecházelo k pravidlovým systémům, fuzzy logice či Dempster-Shaferově teorii, až se jako dominantní standard prosadila **teorie pravděpodobnosti**.

### Klíčové pravděpodobnostní pojmy
* **Sdružená pravděpodobnostní distribuce (Joint Probability Distribution):** Tabulka pokrývající pravděpodobnosti všech možných kombinací stavů (světů) v dané doméně. Umožňuje odpovědět na jakýkoli dotaz, ale roste exponenciálně s počtem proměnných ($2^n$ pro $n$ binárních proměnných).
* **Marginalizace (Marginalization / Summing out):** Proces výpočtu pravděpodobnosti menší množiny proměnných sečtením (integrováním) pravděpodobností přes všechny možné hodnoty ostatních (skrytých) proměnných:
    $$P(Y) = \sum_{z \in Z} P(Y, z)$$
* **Podmíněná pravděpodobnost (Conditional Probability):** Pravděpodobnost jevu $x$ za předpokladu, že již nastal jev $y$ (tzv. evidence / svědectví):
    $$P(x \mid y) = \frac{P(x \land y)}{P(y)}$$
* **Bayesův teorém:** Základní kámen pravděpodobnostního uvažování, který umožňuje otočit směr podmínění. V diagnostických systémech se typicky využívá k určení pravděpodobnosti příčiny (nemoci) na základě pozorovaných efektů (symptomů):
    $$P(\text{příčina} \mid \text{efekt}) = \frac{P(\text{efekt} \mid \text{příčina}) \times P(\text{příčina})}{P(\text{efekt})}$$

---

## Bayesovské sítě

**Bayesovská síť (Bayesian Network)** je strukturovaný reprezentativní model, který explicitně zachycuje vztahy podmíněné nezávislosti mezi náhodnými proměnnými.

### Formální definice
Jedná se o **Orientovaný acyklický graf (DAG)**, kde:
1.  **Uzly** reprezentují náhodné proměnné (diskrétní či spojité).
2.  **Orientované hrany** reprezentují přímý vliv nebo závislost mezi proměnnými.
3.  Každý uzel $X$ obsahuje **Tabulku podmíněné pravděpodobnosti (CPT - Conditional Probability Table)**, která definuje distribuci $P(X \mid \text{Parents}(X))$ vůči všem kombinacím hodnot jeho přímých rodičů.

### Sémantika a kompaktnost
Bayesovská síť implicitně reprezentuje kompletní plnou sdruženou distribuci pravděpodobnosti. Hodnotu libovolného stavu lze spočítat jako součin lokálních podmíněných pravděpodobností:
$$P(x_1, \dots, x_n) = \prod_{i=1}^{n} P(x_i \mid \text{parents}(X_i))$$

* **Výhoda kompaktnosti:** Pokud je síť řídká a každý uzel má nejvýše $k$ rodičů, klesá paměťová náročnost z původních $2^n$ (pro plnou sdruženou distribuci) na pouhých $n \cdot 2^k$ parametrů.

### Konstrukce sítě a řazení uzlů
Při budování sítě algoritmus postupně přidává uzly a volí pro ně minimální množinu rodičů. Výsledná podoba sítě kriticky závisí na **pořadí zadávání uzlů**:
* **Kauzální směr (Doporučeno):** Řazení od *příčin k efektům* (např. Vloupání $\rightarrow$ Alarm $\rightarrow$ Volání souseda). Vede k přirozeným, řídkým grafům s minimem hran a CPT tabulkami, které se snadno plní na základě expertních znalostí.
* **Diagnostický směr (Nevhodné):** Řazení od *efektů k příčinám*. Vede k obřím, hustě propojeným sítím s mnoha redundantními závislostmi, které ztrácejí paměťové výhody.

---

## Exaktní a aproximační odvozování

Cílem odvozování (Inference) je spočítat výslednou podmíněnou distribuci dotazované proměnné $X$ na základě pozorované evidence $e$ (přičemž proměnné $Y$ jsou skryté / marginalizované): $P(X \mid e) = \alpha P(X, e) = \alpha \sum_{y} P(X, e, y)$.

### 1. Exaktní odvozování (Exact Inference)

Cílem je spočítat matematicky přesný výsledek.

* **Odvozování výčtem (Inference by enumeration):** Top-down procházení stromu možných stavů. Algoritmus rekurzivně prochází a sčítá větve pro všechny skryté proměnné.
    * *Nevýhoda:* Je extrémně neefektivní, protože opakovaně počítá identické podvýrazy v různých větvích stromu.
* **Eliminace proměnných (Variable elimination):** Algoritmus založený na principu dynamického programování. Pravděpodobnostní tabulky (CPTs) jsou uvažovány jako matematické **faktory** (matice). Výpočet probíhá zdola nahoru (zprava doleva):
    1.  **Bodový součin (Pointwise product):** Spojení souvisejících faktorů do nového faktoru přes sjednocení jejich proměnných.
    2.  **Vysčítání (Summing out):** Eliminace konkrétní skryté proměnné sečtením jejích dílčích submatic, čímž se proměnná z faktoru odstraní.
* **Složitost exaktního odvozování:**
    * Pokud je síť **strom (poly-tree)** (mezi dvěma uzly existuje nejvýše jedna neorientovaná cesta), je časová i prostorová složitost lineární vůči velikosti sítě: $O(n \cdot d^k)$.
    * Pro obecné, vícenásobně propojené sítě je problém **NP-těžký** (NP-hard) a v obecném případě až **#P-těžký** (převoditelné na počítání splnitelných přiřazení v 3-SAT). Složitost je dána velikostí největšího faktoru vytvořeného během eliminace (ovlivněno heuristikou řazení proměnných).

### 2. Aproximační odvozování (Approximate Inference)

Pro rozsáhlé a husté sítě je exaktní výpočet nezvládnutelný. Využívají se stochastické **Monte Carlo** algoritmy, které generují velké množství náhodných vzorků (simulací) a výsledek odhadují pomocí statistické četnosti.

* **Přímé vzorkování (Direct sampling):** Generuje vzorky od kořenů k listům v topologickém uspořádání grafu. Hodnota každého uzlu se sampluje náhodně na základě pravděpodobnosti z CPT, která je podmíněna hodnotami již vybranými pro jeho rodiče. Neumí nativně pracovat s pevnou evidencí.
* **Zamítavé vzorkování (Rejection sampling):** Používá se pro výpočet podmíněné pravděpodobnosti $P(X \mid e)$. Generují se kompletní vzorky pomocí přímého vzorkování, ale jakmile je vygenerovaný vzorek v rozporu s pozorovanou evidencí $e$, je okamžitě **zamítnut (zahozen)**. Výsledek se spočte z poměru zachovaných vzorků.
    * *Zásadní slabina:* Pokud je evidence $e$ vzácná (má nízkou apriorní pravděpodobnost), algoritmus zahodí drtivou většinu vzorků a efektivita drasticky padá.
* **Váhová věrohodnost (Likelihood weighting):** Zabraňuje plýtvání a zahazování vzorků. Proměnné, které jsou součástí evidence $e$, se **zafixují na pevno** a vůbec se nenasazují do náhodného vzorkování. Vzorkují se pouze nestanovené (volné) proměnné.
    * Aby byla distribuce matematicky korektní, každému vygenerovanému vzorku se přiřadí **váha $w$**, která odpovídá součinu podmíněných pravděpodobností všech fixovaných proměnných v momentě, kdy na ně v topologickém průchodu dojde řada. Výsledná distribuce se pak normalizuje přes sumu těchto vah.

---

## Čas a neurčitost

Při uvažování v čase pracuje agent v částečně pozorovatelném prostředí, kde si udržuje stav víry (*belief state*) na základě pozorovaných vjemů a senzorického modelu. Modelování dynamického světa probíhá pomocí rozdělení na diskrétní časové řezy (*time slices*). V každém časovém řezu $t$ rozlišujeme:
* **Skryté náhodné proměnné $X_t$:** Neovlivnitelné a přímo nepozorovatelné vlastnosti světa (stav systému).
* **Pozorovatelné náhodné proměnné $E_t$:** Evidence, vjemy nebo měření ze senzorů s konkrétními hodnotami $e_t$.

### Přechodový a senzorický model
* **Přechodový model (Transition model):** Specifikuje pravděpodobnostní rozdělení nad aktuálními stavovými proměnnými při znalosti celé historie: $P(X_t \mid X_{0:t-1})$. Jelikož historie v čase neomezeně roste, zavádí se **Markovský předpoklad**:
    * *Markovský proces 1. řádu:* Aktuální stav závisí striktně pouze na stavu bezprostředně předcházejícím: $P(X_t \mid X_{0:t-1}) = P(X_t \mid X_{t-1})$.
    * *Stacionární proces:* Zákony změny světa se v čase nemění, tzn. tabulky podmíněných pravděpodobností (CPTs) přechodů jsou identické pro všechna $t$.
* **Senzorický model (Sensor model):** Popisuje závislost pozorování na ostatních proměnných. Uplatňuje se **senzorický Markovský předpoklad** – evidence $E_t$ v čase $t$ závisí pouze na skrytém stavu $X_t$ ve stejném časovém řezu: $P(E_t \mid X_{0:t}, E_{1:t-1}) = P(E_t \mid X_t)$.
* **Sdružená distribuce v Bayesovské síti:** Celý časový model lze reprezentovat dynamickou Bayesovskou sítí spuštěnou z počáteční distribuce $P(X_0)$:
    $$P(X_{0:t}, E_{1:t}) = P(X_0) \prod_{i=1}^{t} P(X_i \mid X_{i-1}) P(E_i \mid X_i)$$

### Základní inferenční úlohy
1.  **Filtrace (Filtering):** Výpočet aktuálního stavu víry na základě všech dosavadních pozorování: $P(X_t \mid e_{1:t})$. Využívá rekurzivní odhad (*forward message passing*) bez nutnosti procházet celou historii od nuly:
    $$P(X_{t+1} \mid e_{1:t+1}) = \alpha P(e_{t+1} \mid X_{t+1}) \sum_{x_t} P(X_{t+1} \mid x_t) P(x_t \mid e_{1:t})$$
2.  **Predikce (Prediction):** Výpočet pravděpodobnostního rozdělení budoucího stavu $k$ kroků dopředu: $P(X_{t+k} \mid e_{1:t})$ pro $k > 0$. Lze na ni nahlížet jako na filtraci bez přísunu nové evidence. Distribuce v čase konverguje ke stacionárnímu rozdělení Markovského procesu.
3.  **Vyhlazování (Smoothing):** Výpočet pravděpodobnosti stavu v minulosti na základě evidence až do současnosti: $P(X_k \mid e_{1:t})$ pro $0 \le k < t$. Počítá se pomocí obousměrného posílání zpráv (zkombinuje se *forward* zpráva od $1$ do $k$ a *backward* zpráva od $t$ zpět do $k+1$).
4.  **Nejpravděpodobnější vysvětlení (Most likely explanation):** Hledání ucelené sekvence skrytých stavů, která s nejvyšší pravděpodobností vygenerovala danou sekvenci pozorování: $\text{argmax}_{x_{1:t}} P(x_{1:t} \mid e_{1:t})$ (řeší se např. Viterbiho algoritmem).

---

## Teorie užitku

Zatímco teorie pravděpodobnosti popisuje, čemu by měl agent na základě důkazů věřit, **teorie užitku** definuje jeho preference a cíle. Jejich spojením vzniká **teorie rozhodování** (*Decision theory*), která říká, jak by se měl racionální agent chovat.

### Princip maximalizace očekávaného užitku (MEU)
Každému stavu $s$ přiřadí užitková funkce $U(s)$ reálné číslo vyjadřující jeho atraktivitu pro agenta. Očekávaný užitek akce $a$ při znalosti evidence $e$ je dán váženým průměrem užitků všech možných výsledných stavů:
$$EU(a \mid e) = \sum_{s} P(\text{Result}(a) = s \mid a, e) U(s)$$
Racionální agent volí akci, která tento očekávaný užitek maximalizuje: $a = \text{argmax}_a EU(a \mid e)$.

### Racionální preference a loterie
Agent vyjadřuje preference mezi stavy ($A \succ B$ pro upřednostnění, $A \sim B$ pro lhostejnost). Vzhledem k neurčitosti se volba často stává **loterií** — strukturou $[p_1, S_1; \dots; p_n, S_n]$, kde výsledek $S_i$ nastane s pravděpodobností $p_i$.
* **Hledání užitkové funkce (Preference elicitation):** Užitek nejlepšího možného stavu se zafixuje na $U(S_{max}) = 1$ a nejhoršího na $U(S_{min}) = 0$. Užitek libovolného mezistavu $S$ se určí nalezením takové pravděpodobnosti $p$, při které je agentovi jedno, zda dostane garantovaný stav $S$, nebo podstoupí standardní loterii $[p, S_{max}; 1-p, S_{min}]$. Pak platí $U(S) = p$.
* **Užitek peněz:** Peníze nerostou lineárně s užitkem. Většina lidí vykazuje odpor k riziku (*risk aversion*), protože užitek z peněz má konkávní charakter (přírůstek bohatství přináší klesající marginální užitek). Racionální chování závisí na aktuálním celkovém majetku aktéra.

### Vícekriteriální užitek a dominance
V reálném světě jsou stavy popsány více atributy (např. cena, bezpečnost, komfort).
* **Striktní dominance (Strict dominance):** Pokud je možnost $B$ ve všech sledovaných atributech horší než možnost $A$, je z dalšího uvažování vyřazena.
* **Stochastická dominance (Stochastic dominance):** Zobecnění dominance pro neurčité výsledky (loterie) na základě porovnání kumulativních distribučních funkcí.
* **Aditivní hodnotová funkce:** Pokud jsou atributy na sobě preferenčně nezávislé, celkový užitek lze vyjádřit jako vážený součet užitků jednotlivých atributů: $U(x_1, \dots, x_n) = \sum_i U_i(x_i)$.

### Rozhodovací sítě (Decision Networks / Influence Diagrams)
Mechanismus pro vizualizaci a výpočet jednoduchých racionálních rozhodnutí rozšiřuje klasické Bayesovské sítě o nové typy uzlů:
1.  **Uzly náhodných proměnných (Chance nodes - ovály):** Reprezentují stochastické proměnné prostředí (stejně jako v Bayesovských sítích).
2.  **Rozhodovací uzly (Decision nodes - obdélníky):** Body, ve kterých má agent přímou volbu mezi dostupnými akcemi.
3.  **Užitkové uzly (Utility nodes - kosočtverce):** Reprezentují agentovu užitkovou funkci (jsou do nich staženy hrany ze stavů, které užitek přímo ovlivňují).

---

## Markovský rozhodovací proces

**Markovský rozhodovací proces (MDP)** formalizuje sekvenční rozhodovací problém v plně pozorovatelném, stochastickém prostředí s Markovským přechodovým modelem a aditivními odměnami.

### Komponenty MDP
* Množina stavů $S$ s počátečním stavem $s_0$.
* Množina akcí $A(s)$ dostupných v každém stavu.
* Přechodový model $P(s' \mid s, a)$ vyjadřující pravděpodobnost, že aplikace akce $a$ ve stavu $s$ povede do stavu $s'$.
* Funkce odměny $R(s)$ přidělovaná za návštěvu (nebo setrvání v) daném stavu.

### Strategie (Policy) a Horizont
Řešením MDP není fixní plán (posloupnost akcí), protože stochastické prostředí může agenta odklonit. Řešením je **strategie $\pi(s)$** — funkce, která doporučuje nejlepší akci pro *každý* představitelný stav, do kterého se agent může dostat. Optimální strategie $\pi^*$ maximalizuje očekávaný kumulativní užitek.
* **Konečný horizont (Finite horizon):** Existuje fixní čas $N$, po kterém na dalším vývoji nezáleží. Optimální strategie je v tomto případě *nestacionární* (závisí na zbývajícím čase — v totožném stavu se agent chová jinak v kroku $1$ a jinak těsně před vypršením limitu).
* **Nekonečný horizont (Infinite horizon):** Neexistuje pevný časový limit (deadline). Optimální strategie je *stacionární* (v identickém stavu se agent chová vždy stejně, bez ohledu na čas).

### Kumulativní užitek v čase
Při nekonečném horizontu se pro výpočet užitku sekvence stavů používá **diskontovaná odměna** s diskontním faktorem $\gamma \in (0, 1)$:
$$U([s_0, s_1, s_2, \dots]) = \sum_{t=0}^{\infty} \gamma^t R(s_t) \le \frac{R_{max}}{1 - \gamma}$$
Faktor $\gamma$ modeluje preferenci okamžitých odměn před budoucími a matematicky zajišťuje, že suma nekonečné geometrické řady užitků konverguje k finálnímu konečnému číslu (pokud jsou odměny ohraničené).

### Algoritmy pro řešení MDP

#### Bellmanova rovnice
Základní teoretický pilíř pro výpočet užitků stavů. Říká, že užitek stavu $s$ je roven okamžité odměně $R(s)$ plus očekávanému diskontovanému užitku následného stavu za předpokladu, že agent zvolí optimální akci:
$$U(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U(s')$$
Jedná se o systém $n$ nelineárních rovnic (kvůli operátoru $\max$) pro $n$ stavů.

#### 1. Iterace hodnot (Value Iteration)
Iterativní numerický algoritmus pro výpočet pravých užitků. Začne se s libovolnými hodnotami (např. samé nuly) a v každém kroku se provede tzv. **Bellmanův update** pro všechny stavy:
$$U_{i+1}(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U_i(s')$$
* **Konvergence:** Bellmanův update vykazuje vlastnost *kontrakce* (zmenšuje maximální vzdálenost mezi vektory s faktorem $\gamma$). Algoritmus prokazatelně konverguje k jedinému stabilnímu řešení. Ukončuje se, jakmile je maximální změna v iteraci $\delta < \epsilon(1-\gamma)/\gamma$, což garantuje chybu užitku nejvýše $\epsilon$.
* *Poznámka:* Doporučená strategie $\pi_i$ často konverguje k optimální verzi $\pi^*$ mnohem dříve, než plně dokongverguje samotná hodnota užitků $U_i$.

#### 2. Iterace strategie (Policy Iteration)
Alternativní přístup. Místo neustálého zpřesňování užitků střídá dva dedikované kroky:
1.  **Evaluace strategie (Policy evaluation):** Pro aktuální fixní strategii $\pi_i$ se spočítají užitky stavů $U^{\pi_i}$. Protože akce jsou pevně dané strategí, Bellmanova rovnice se zjednoduší na *lineární* systém rovnic s kubickou složitostí $O(n^3)$, což lze pro velké prostory aproximovat sérií zjednodušených hodnotových kroků:
    $$U_{j+1}(s) = R(s) + \gamma \sum_{s'} P(s' \mid s, \pi_i(s)) U_j(s')$$
2.  **Zlepšení strategie (Policy improvement):** Na základě nově spočtených užitků se hladově aktualizuje doporučená akce pro každý stav:
    $$\pi_{i+1}(s) = \text{argmax}_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U^{\pi_i}(s')$$
Algoritmus se zastaví v momentě, kdy v kroku zlepšení nedojde k žádné změně strategie. Jelikož je počet možných stacionárních strategií konečný a každá iterace prokazatelně nalezne strategii lepší (nebo ekvivalentní), algoritmus garantovaně konverguje a v praxi bývá často rychlejší než čistá iterace hodnot.

<img alt="img.png" src="img/metody_umele_inteligence/markov.png" width="400"/>

---
## Iterace hodnot

**Iterace hodnot (Value Iteration)** je iterativní numerický algoritmus určený k nalezení optimálního užitku stavů (a následně optimální strategie) v Markovských rozhodovacích procesech (MDP). Vychází přímo z **Bellmanovy rovnice**, která je kvůli operátoru maxima nelineární, a proto ji nelze řešit jako běžnou soustavu lineárních rovnic.

### Princip a výpočet
Algoritmus začíná s libovolnými (typicky nulovými) odhady užitků pro všechny stavy a v každém kroku provádí tzv. **Bellmanův update (aktualizaci)** pro každý stav $s \in S$:
$$U_{i+1}(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U_i(s')$$

### Vlastnosti a konvergence
* **Vlastnost kontrakce:** Bellmanův update vykazuje matematickou vlastnost *kontrakce* s faktorem $\gamma$ (kde $\gamma \in (0, 1)$). To znamená, že s každou aplikací aktualizace se vektor užitků přiblíží k jedinému pevnému bodu (skutečnému optimálnímu užitku $U$) a algoritmus garantovaně konverguje nezávisle na počátečních hodnotách.
* **Ukončovací podmínka:** Iterace se zastaví, jakmile je maximální změna užitku u kteréhokoli stavu v dané iteraci ($\delta$) menší než definovaná mez přesnosti:
  $$\delta < \frac{\epsilon(1 - \gamma)}{\gamma} \implies |U_{i+1} - U| < \epsilon$$
* **Ztráta strategie (Policy loss):** Vyjadřuje, kolik může agent nejvýše ztratit, pokud se bude rozhodovat podle aktuální (neúplné) užitkové funkce $U_i$ namísto optimální $U$. Platí, že pokud je chyba užitku $|U_i - U| < \epsilon$, pak ztráta strategie je $|U^{\pi_i} - U| < 2\epsilon$.
* *Klíčový poznatek:* V praxi se doporučená strategie $\pi_i$ stává optimální **mnohem dříve**, než plně zkonvergují samotné numerické hodnoty užitků $U_i$.

---

## Iterace strategie

**Iterace strategie (Policy Iteration)** je alternativní algoritmus pro řešení MDP. Staví na myšlence, že pro nalezení optimálního chování není nutné znát zcela přesné hodnoty užitků stavů — pokud je jedna konkrétní akce zjevně lepší než ostatní, přesná numerická hodnota užitku okolních stavů nehraje roli.

### Mechanismus (Dva střídavé kroky)
Algoritmus začíná s náhodnou počáteční strategií $\pi_0$ a neustále opakuje následující cyklus:

1. **Evaluace strategie (Policy Evaluation):** Pro aktuální fixní strategii $\pi_i$ se spočítají přesné užitky stavů $U^{\pi_i}$. Protože akce v každém stavu je pevně určena strategií, Bellmanova rovnice se zjednoduší na **systém lineárních rovnic**:
   $$U^{\pi_i}(s) = R(s) + \gamma \sum_{s'} P(s' \mid s, \pi_i(s)) U^{\pi_i}(s')$$
   * *Pro malé stavové prostory:* Lze řešit exaktně (např. Gaussovou eliminací) s časovou složitostí $O(n^3)$.
   * *Pro velké stavové prostory:* Provádí se série zjednodušených hodnotových iterací (bez operátoru `max`) pro získání dostatečné aproximace.
2. **Zlepšení strategie (Policy Improvement):** Na základě nově spočtených užitků se hladově aktualizuje doporučená akce pro každý stav (vybere se akce s nejvyšším očekávaným užitkem):
   $$\pi_{i+1}(s) = \arg\max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U^{\pi_i}(s')$$

### Vlastnosti a ukončení
* **Garantované ukončení:** Algoritmus končí v momentě, kdy v kroku zlepšení nedojde k žádné změně strategie (`changed == false`). 
* **Efektivita:** Jelikož je počet možných stacionárních strategií pro konečný stavový prostor konečný a každá iterace přináší striktní strukturální zlepšení, algoritmus garantovaně zkonverguje k optimálnímu řešení. V praxi velmi často vyžaduje **mnohem méně iterací** než čistá iterace hodnot.
---
## Robotika

Robotika studuje fyzikální agenty (roboty), kteří provádějí úkoly manipulací s reálným světem. K tomu jsou vybaveni **efektory** (kola, nohy, klouby, chapadla) pro působení silou na okolí a **senzory** (kamery, lasery, gyroskopy, akcelerometry) pro vnímání prostředí a vlastního pohybu.

### Kategorie robotů
* **Manipulátory (Robotická ramena):** Fyzicky ukotvené v pracovním prostoru, typické pro průmyslovou výrobu.
* **Mobilní roboti:** Pohybují se v prostředí pomocí kol či nohou. Patří sem bezpilotní pozemní vozidla (UGV), letecké drony (UAV), podvodní plavidla (AUV) nebo planetární vozítka.

### Senzory a stav robota
* **Pasivní senzory:** Pouze zachycují signály generované jinými zdroji v prostředí (např. běžné kamery).
* **Aktivní senzory:** Vysílají energii do okolí a měří její odraz zpět (např. sonar, LiDAR). Poskytují více informací, ale spotřebovávají více energie a hrozí interference s jinými aktivními senzory.
* **Stav robota:** Rozlišuje se **kinematický stav** (geometrická pozice a orientace kloubů/platformy) a **dynamický stav** (přidaná rychlost, zrychlení a síly).

### Robotické vnímání (Percepce)
Percepce je proces mapování měření ze senzorů do vnitřní reprezentace světa. Je náročná, protože senzory jsou zatíženy šumem a svět je pouze částečně pozorovatelný a nepředvídatelný. Vnímání se modeluje jako **temporální odvozování** pomocí **Dynamických Bayesovských sítí (DBN)**:
* **Filtrace za přítomnosti akcí:** Výpočet aktuální pozice zohledňuje nejen nová měření $z_{t+1}$, ale i vykonané akce $a_t$ robota:
    $$P(X_{t+1} \mid z_{1:t+1}, a_{1:t}) = \alpha P(z_{t+1} \mid X_{t+1}) \int P(X_{t+1} \mid x_t, a_t) P(x_t \mid z_{1:t}, a_{1:t-1}) \, dx_t$$
* **Lokalizace:** Zjišťování polohy robota v mapě (řeší se pomocí DBN, např. Monte Carlo lokalizací pro diskrétní stavy nebo Kalmanovými filtry pro spojité proměnné).
* **Mapování:** Konstrukce mapy neznámého prostředí.
* **SLAM (Simultaneous Localization and Mapping):** Simultánní lokalizace a mapování. Robot se nachází v neznámém prostředí bez mapy a musí současně stavět mapu i odhadovat svou polohu v ní.

---

## Plánování pohybu robota

Základním problémem plánování pohybu (*Motion Planning*) je nalézt spojitou sekvenci konfigurací, která bezpečně převede robota z počátečního stavu $q_I$ do cílového stavu $q_G$, aniž by po cestě došlo ke kolizi s jakoukoli překážkou.

### Hierarchie úloh v robotice
Při návrhu autonomního chování se problémy dělí do tří úrovní, které se následně integrují:
1.  **Plánování úloh (Task planning):** Rozhoduje o sekvencích vysokoúrovňových akcí (např. *jdi ke dveřím $\rightarrow$ otevři dveře $\rightarrow$ nastup do výtahu*).
2.  **Plánování pohybu (Motion planning):** Hledá geometrickou trajektorii (cestu) z bodu A do bodu B pro splnění každé vysokoúrovňové akce.
3.  **Řízení (Control):** Generuje konkrétní nízkoúrovňové příkazy (napětí v motorech, momenty) pro akční členy (aktuátory), aby robot naplánovaný pohyb fyzicky vykonal.

### Přístupy k řešení spojitého prostoru
Spojitý prostor konfigurací nelze prohledávat přímo. Řeší se to jeho **diskretizací** (převedením na graf, voxelovou mapu či mraky bodů) a následným nasazením standardních algoritmů prohledávání grafů ($A^*$, BFS, Jump-point search).

---

## konfigurační prostor,

* **Pracovní prostor (Workspace):** Fyzický reálný prostor (např. 2D nebo 3D), ve kterém se robot fyzicky pohybuje a kde se nacházejí geometrické překážky $O$. Kontrola kolizí pro těleso robota složené z mnoha bodů je v pracovním prostoru výpočetně extrémně drahá.
* **Konfigurační prostor (C-space):** Abstraktní vícerozměrný prostor, kde je celá (jakkoli složitá) struktura robota reprezentována jako **jeden jediný bod**. Počet dimenzí $C$-prostoru odpovídá počtu stupňů volnosti robota (DoF).
    * *Příklad (Mobilní robot):* Pozice v rovině se zadává jako bod $(x, y)$, případně s orientací $(x, y, \theta)$.
    * *Příklad (Robotické rameno):* Konfigurace dvoukloubového ramene je definována úhly kloubů $(\phi_{shou}, \phi_{elb})$.
* **Dělení C-prostoru:**
    * $C_{obs}$ (Obstacle space): Množina všech konfigurací, ve kterých robot koliduje s překážkou nebo sám se sebou. Tvary překážek v $C$-prostoru jsou geometricky velmi komplikované, i když v pracovním prostoru byly jednoduché.
    * $C_{free}$ (Free space): Bezpečný prostor konfigurací, kde ke kolizím nedochází ($C_{free} = C \setminus C_{obs}$).

<img alt="img.png" src="img/metody_umele_inteligence/config vs workspace.png" width="400"/>


### Kinematika
Plánovač se pohybuje v $C$-prostoru, ale kolize musí fyzicky vyhodnocovat v souřadnicích pracovního prostoru. K tomu slouží transformační aparát kinematiky:
* **Dopředná kinematika (Forward kinematics):** Přepočítává známou konfiguraci (např. úhly kloubů) na přesné souřadnice konkrétního bodu robota (typicky koncového efektoru / chapadla) v pracovním prostoru.
* **Inverzní kinematika (Inverse kinematics):** Hledá potřebnou konfiguraci kloubů robota na základě požadované cílové pozice a orientace efektoru v pracovním prostoru.
* **Sondování (Probing):** Protože explicitní konstrukce celého $C_{obs}$ je analyticky téměř nemožná, v praxi se prostor pouze "sonduje" — vygeneruje se konfigurace v $C$-prostoru, pomocí dopředné kinematiky se spočtou pozice ramen a ty se v pracovním prostoru otestují na kolize ("black-box" kolizní detektor).

---

## kombinatorické a pravděpodobnostní přístupy

### 1. Kombinatorické (deterministické) přístupy
Tyto přístupy diskretizují spojitý volný prostor $C_{free}$ exaktními geometrickými či algebraickými metodami.

#### Metody založené na cestovních mapách (Roadmaps)
* **Graf viditelnosti (Visibility graph):** Spojuje úsečkami start, cíl a všechny vrcholy polygonálních překážek. Hrany, které protínají překážky, jsou eliminovány.
    * *Vlastnosti:* Vždy garantuje nalezení **nejkratší možné cesty**. Cesta však vede těsně kolem vrcholů překážek (nulová bezpečnostní rezerva při chybě v řízení). Výpočetně drahé ve vyšších dimenzích (naivní konstrukce $O(n^3)$).
* **Voronoiův diagram:** Generuje cestovní mapu složenou z bodů, které mají přesně stejnou vzdálenost od dvou nebo více nejbližších překážek.
    * *Vlastnosti:* **Maximalizuje bezpečnostní odstup** (*clearance*) od překážek. Cesty jsou konzervativní (nejsou nejkratší). Malá změna v pozici překážky může radikálně změnit podobu celého diagramu.

#### Dekompozice na buňky (Cell Decomposition)
Rozděluje $C_{free}$ na konečný počet nepřekrývajících se spojitých oblastí (buněk), uvnitř kterých je plánování triviální (např. pohyb po úsečce).
* **Exaktní (vertikální) dekompozice:** Prostor se svisle rozdělí v místech kritických geometrických událostí (využívá se *plane sweep* algoritmus). Středy výsledných buněk (lichoběžníků/trojúhelníků) se propojí do exaktního grafu.
* **Aproximační dekompozice (Pravidelná mřížka / Octree / Quadtree):** Prostor se rekurzivně dělí na sub-buňky (např. na kostky v 3D). Buňky se dělí na prázdné (*empty*), obsazené (*full*) a smíšené (*mixed*). Smíšené buňky se rekurzivně dělí hlouběji, dokud se nenajde čistá cesta nebo se nenarazí na limit rozlišení. Ohodnocení buněk cenou se počítá pomocí algoritmu $A^*$ nebo iterace hodnot.

<img alt="img.png" src="img/metody_umele_inteligence/cell-decomp.png" width="400"/>


#### Potenciálová pole (Potential Field Methods)
Pohyb robota je řízen virtuálními silovými poli. Cíl generuje **atraktivní (přitažlivé) pole**, které robota táhne k sobě, zatímco překážky generují **repulzivní (odpudivá) pole**, která ho odtlačují. Robot se pohybuje ve směru gradientu výsledného pole.
* *Limity:* Metoda je lokální a hrozí uvíznutí v **lokálních minimech** (kde se přitažlivá a odpudivá síla vyruší ještě před dosažením cíle). Kompletnost je garantována pouze při zavedení analyticky náročné "navigační funkce".

---

### 2. Pravděpodobnostní přístupy (Sampling-based)
Místo exaktní konstrukce překážek náhodně vzorkují konfigurace v $C$-prostoru a testují je kolizním detektorem. Poskytují **pravděpodobnostní úplnost** (s rostoucím počtem vzorků pravděpodobnost nalezení řešení, pokud existuje, konverguje k 1). Jsou velmi úspěšné ve vysokých dimenzích (4 a více DoF). Hlavní slabinou pravděpodobnostního vzorkování je prohledávání **úzkých průchodů** (*narrow passages*).

#### Probabilistická cestovní mapa (PRM) — Multi-query strategie
Navržena pro statická prostředí, kde se plánuje opakovaně (vytvoří se jedna robustní mapa a nad ní se provádějí dotazy).
1.  **Fáze učení (Learning phase):** Vygeneruje se $n$ náhodných bodů v $C_{free}$. Lokální plánovač (zkoušející jednoduché přímočaré spojení) se pokusí propojit blízké uzly v definovaném poloměru $\rho$ do grafu.
2.  **Fáze dotazu (Query phase):** Startovní a cílová konfigurace se lokálním plánovačem připojí k hotové mapě a cesta se vyhledá pomocí klasického grafového vyhledávání (Dijkstra, $A^*$).
* **sPRM (Simpified PRM):** Verze, kde se každý nový náhodný uzel slepě zkouší propojit se všemi sousedy v poloměru $\rho$. Klasické **PRM** je efektivnější — sousedy propojuje v pořadí rostoucí vzdálenosti a vazbu vytvoří pouze tehdy, pokud kandidáti dosud neleží ve stejné komponentě souvislosti (šetří se drahé kolizní testy). sPRM je asymptoticky optimální.

#### Rychle se rozrůstající náhodné stromy (RRT) — Single-query strategie
Konstruuje nový graf (strom) pro každou úlohu zvlášť, přičemž se rozrůstá z počáteční konfigurace $q_{init}$ přímo směrem k cíli. Je ideální pro dynamická prostředí a umožňuje snadno zohlednit kinematické a dynamické limity robota.

**Algoritmus výstavby stromu:**
1.  Počáteční stav $q_{init}$ se stane kořenem stromu.
2.  Náhodně se vygeneruje konfigurace $q_{new}$ v $C_{free}$.
3.  Ve stromu se vyhledá uzel $q_{near}$, který je geometricky nejblíže k $q_{new}$ (využívají se KD-stromy a knihovny ANN/FLANN).
4.  Strom se rozšíří z bodu $q_{near}$ malým krokem směrem k $q_{new}$ (vzniká nový platný uzel $u$, ověřený kolizní knihovnou jako Black-Box).
5.  Proces se opakuje od kroku 2, dokud se větev stromu nedostane do dostatečné blízkosti cíle.

*Vlastnosti:* RRT velmi rychle a agresivně expanduje do velkých, dosud neprozkoumaných oblastí prostoru (přitahuje ho volný prostor). Základní verze RRT poskytuje přípustné řešení, ale neposkytuje žádné záruky na optimalitu (výsledná dráha bývá klikatá a dlouhá).


