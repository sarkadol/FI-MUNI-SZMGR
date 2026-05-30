# Metody umělé inteligence

> Prohledávání stavového prostoru, lokální prohledávání a metaheuristiky s jedním řešením, 
> populační metaheuristiky (evoluční algoritmy, inteligence hejna). 
> Plánování, reprezentace problému, planning se stavovým prostorem. 
> Práce s neurčitostí, Bayesovské sítě, exaktní a aproximační odvozování, čas a neurčitost, 
> teorie užitku, Markovský rozhodovací proces, iterace hodnot, iterace strategie. 
> Robotika, planning pohybu robota (konfigurační prostor, kombinatorické a pravděpodobnostní přístupy).

---

## Prohledávání stavového prostoru

Reálné inženýrské problémy (rozvrhování výroby, logistika, vyhledávání tras) často obsahují tak velké množství kombinací, že je 
nelze řešit exaktně hrubou silou. Prohledávání stavového prostoru představuje systematický způsob, jak formalizovat strukturu 
problému a efektivně procházet množinu možných řešení.

$s$ představuje stav (řešení), $s_i$ je konkrétní rozhodovací proměnná, $m$ značí počet agentů 
a $n$ počet úloh či prvků.

* **Heuristické algoritmy (Heuristic algorithms):** Specifické postupy navržené pro konkrétní problém. Využívají jeho vnitřní 
strukturu a specifické vlastnosti (tzv. „art of discovering“). Neposkytují teoretickou záruku kvality výsledku ani dosažení 
globálního optima, ale vynikají výpočetní rychlostí. *Příklad:* Pravidlo *Earliest Due Date* v rozvrhování úloh (jako první se zpracuje úloha s nejbližším termínem dokončení) 
nebo přiřazování prostorově nejbližší zastávky u plánování tras vozidel.
* **Metaheuristiky (Metaheuristics):** Obecné řídicí strategie a šablony vyšší úrovně, které jsou nezávislé na konkrétní 
problémové doméně. Neřeší problém přímo, ale vedou a propojují podřízené heuristiky. Díky této abstrakci jsou široce 
aplikovatelné (např. stejný algoritmus simulovaného žíhání lze aplikovat na rozvrhování zkoušek i na problém obchodního cestujícího).

**Diversifikace vs. intenzifikace:** Při návrhu a nastavení metaheuristik je klíčové dosáhnout rovnováhy mezi dvěma 
protichůdnými kritérii, aby algoritmus předčasně neuvízl v suboptimálním stavu, nebo naopak neprohledával prostor neefektivně. 
*Diversifikace (Průzkum / Exploration)* je schopnost algoritmu opustit aktuální lokální oblasti a prohledávat dosud neprozkoumané, 
vzdálené části stavového prostoru. Poskytuje globální pohled a zabraňuje uvíznutí v lokálním optimu. *Intenzifikace (Využití / Exploitation)* je zaměření algoritmu na detailní prohledání slibného lokálního okolí aktuálního řešení s cílem nalézt lokální optimum v dané oblasti.

<img alt="img.png" src="img/metody_umele_inteligence/divers-vs-intens.png" width="600"/>

**Reprezentace (Kódování / Encoding):** Způsob, jakým je řešení reálného problému v algoritmu zakódováno, zásadně určuje 
velikost prohledávacího prostoru a výpočetní složitost matematických operátorů.
* **Lineární reprezentace:** Využívá řetězce symbolů.
    * *Binární kódování (Binary encoding):* Každá rozhodovací proměnná nabývá hodnoty $s_i \in \{0, 1\}$. Vhodné pro Problém batohu 
    (Knapsack problem), kde hodnoty indikují zařazení či nezařazení předmětu.
    * *Diskrétní kódování (Discrete encoding):* Zobecnění binárního kódování do konečných diskrétních domén. Vhodné pro alokaci zdrojů (vektor udává, kterému z $m$ agentů je přiřazena $i$-tá úloha). *Vektor `[2, 1, 3, 2, 1]` přiřazuje pěti nezávislým úlohám konkrétní ID agentů (1, 2 nebo 3), kteří je budou zpracovávat.*
    * *Reprezentace permutací (Permutation representation):* Každý prvek se v zápisu vyskytuje právě jednou. Modeluje sekvenční úlohy, 
    jako je Problém obchodního cestujícího (Traveling Salesperson Problem - TSP).*Vektor `[4, 1, 3, 2]` určuje přesné pořadí návštěvy čtyř měst na uzavřené okružní trase bez možnosti jakékoliv duplicity.*

* **Nelineární reprezentace (Non-linear representation):** Využívá složitější struktury, nejčastěji grafy nebo stromy (např. v 
genetickém programování).

**Redukce reprezentačního prostoru:** Vhodně zvolené kódování dokáže matematicky eliminovat zjevně neplatná řešení a výrazně 
zmenšit velikost prohledávaného prostoru ještě před spuštěním samotného výpočtu. Ukažme si to na příkladu rozmístění 8 šachových dam:
1. Kartézské pozice: Vektor souřadnic $s_i = (x_i, y_i) \rightarrow 64^8 \approx 2.81 \times 10^{14}$ možných stavů.
2. Jedna dáma na sloupec: Vektor řádkových pozic $(y_1, \dots, y_8)$, kde index fixuje sloupec $\rightarrow 8^8 = 16\,777\,216$ stavů.
3. Permutace: Zákaz sdílení stejného řádku omezuje prostor na čistou permutaci indexů $\rightarrow 8! = 40\,320$ stavů.

**Práce s omezeními (Constraint Handling):** Reálné optimalizační úlohy obsahují omezující podmínky. Pokud algoritmus vygeneruje 
nepřípustné řešení, uplatňují se následující strategie. 
* *Reject strategie (Reject strategy)* generuje a udržuje striktně pouze přípustná 
řešení a jakýkoli nepřípustný krok okamžitě zahazuje. *Přidání dalšího zákazníka na trasu kamionu by překročilo jeho maximální kapacitu 10 tun, takže je tento rozvrh okamžitě smazán a algoritmus musí zkusit jinou možnost.*
* *Penalizační strategie (Penalty strategy)* rozšiřuje prohledávací prostor 
i o nepřípustná řešení, ale hodnota účelové funkce je penalizována za každé porušené omezení. Váhy penalizací bývají vysoké, aby byl 
algoritmus směrován zpět do přípustné oblasti. *Rozvrh zkoušek, ve kterém profesor zkouší 6 hodin namísto povolených 4, se nezahodí, ale jeho výsledná kvalita se drasticky sníží o trestné body.*
* *Opravné strategie (Repair strategy)* aplikují specifickou opravnou heuristiku, která 
nepřípustné řešení transformuje na platný stav. *Pokud celková váha věcí v batohu překročí limit, specifické pravidlo začne ty nejméně hodnotné předměty z batohu vyhazovat, dokud se hmotnost nedostane do povolené normy.*

**Účelová funkce (Objective Function):** Účelová funkce přiřazuje každému stavu číselné hodnocení, které definuje kvalitu řešení 
a určuje směr prohledávání. Může být *samospustitelná (self-sufficient)*, kdy původní matematické kritérium problému lze přímo použít 
pro optimalizaci (např. minimalizace celkové ujeté vzdálenosti v kilometrech u TSP). Pokud je původní vyhodnocení pouze typu True/False 
(např. u splnitelnosti logických formulí SAT), využívá se funkce *naváděcí (guiding)*. Ta se transformuje tak, aby poskytovala jemnější 
gradient pro lokální prohledávání (např. maximalizace počtu aktuálně splněných klauzulí u $k$-SAT).

---

## Lokální prohledávání

Pokud není známá globální struktura stavového prostoru, využívá se lokální prohledávání. Algoritmus začíná v jednom kompletním 
počátečním bodě a samostatně se posouvá do bezprostředního okolí, dokud nenarazí na lokální optimum, ze kterého už žádná cesta 
ke zlepšení nevede. Základním typem je **Hill Climbing (Horolezecký algoritmus)**, který pracuje nad jedním kompletním řešením:

```pascal
s := s0; // Vygenerování počátečního řešení s0
while stopping condition not satisfied do
    generate(N(s)); // Vygenerování kandidátních sousedů z okolí N(s)
    if no better neighbor exist then 
        terminate; // Dosaženo lokální optimum
    s := s'; // Náhrada aktuálního stavu lepším sousedem s' ∈ N(s)
end while
```

<img alt="landscape.png" src="img/metody_umele_inteligence/landscape.png" width="300"/>

$S$ je stavový prostor (množina všech řešení), $s, s'$ jsou konkrétní stavy, $N(s)$ představuje 
okolí stavu $s$, $d(s, s')$ je vzdálenost mezi stavy, $\epsilon$ je poloměr okolí, $f(s)$ značí hodnotu účelové funkce a $\Delta f$ 
je její diferenční změna.

**Strategie výběru souseda:** Při generování okolí aktuálního stavu určuje tato strategie způsob výběru kandidáta pro následující krok:
* *Best improvement (Steepest descent):* Prohledá kompletně celé dostupné okolí a vybere souseda, který přináší největší možné zlepšení. 
Pro rozsáhlá okolí je tato strategie časově velmi náročná.
* *First improvement:* Prochází okolí sekvenčně a okamžitě vybere prvního nalezeného souseda, který zlepšuje aktuální stav. Výrazně 
zrychluje výpočetní čas jedné iterace.
* *Random selection:* Náhodně vybere menší vzorek sousedů a z nich zvolí stav, který aktuální situaci nejvíce vylepší.

**Koncepty okolí (Neighborhood) a lokální optimum:** Funkce okolí $N$ přiřazuje každému řešení $s \in S$ množinu sousedních řešení 
na základě definovaného přechodového operátoru (*move operator*): $N(s) = \{s' \in S \mid d(s', s) \le \epsilon\}$. Řešení je 
*lokálním optimem* vzhledem k okolí $N$, pokud žádný z jeho sousedů nemá lepší hodnotu účelové funkce. Pro minimalizační problém platí: 
$$f(s) \le f(s') \quad \forall s' \in N(s)$$
U permutačních problémů využíváme *poziční okolí* založené na operátoru vkládání (**insertion** – prvek se vyjme a vloží na jinou pozici) 
nebo *pořadové okolí* využívající operátor výměny (**swap/exchange** – prohození dvou prvků) či otočení podsekvence (**inversion**). 
U Problému obchodního cestujícího (TSP) definujeme okolí jako $k$-distance (výměna pozic $k$ měst, kde pro $2$-distance je velikost 
okolí $\frac{n(n - 1)}{2}$) nebo $k$-opt (odstranění $k$ hran a jejich nahrazení jinými tak, aby vznikla nová platná okružní cesta, 
kde pro $2$-opt je velikost okolí rovna $\left[\frac{n(n - 1)}{2} - n\right]$).

**Inkrementální vyhodnocování okolí (Incremental Evaluation):** Výpočet hodnoty účelové funkce od nuly pro každého kandidáta v okolí 
je výpočetně velmi náročný. Efektivní algoritmy implementují inkrementální vyhodnocení pomocí výpočtu pouhé diferenční změny $\Delta f$. 
Například u 2-opt u TSP se namísto sčítání délky celé trasy spočítá pouze rozdíl způsobený odebráním starých hran a přidáním nových: 
$$\Delta f = \text{dist}(A, E) + \text{dist}(C, D) - \text{dist}(A, D) - \text{dist}(C, E) \implies f(s') = f(s) + \Delta f$$

**Iterované lokální prohledávání (ILS):** Základní algoritmus Hill Climbing uvízne v prvním dosaženém lokálním optimu. Pro pokračování 
v prohledávání se používají techniky restartu. *Multistart local search* provádí opakované spouštění z nových, zcela náhodně vygenerovaných 
počátečních řešení (dochází ke ztrátě informací). *Iterated Local Search (ILS)* staví na historii; na nalezené lokální optimum aplikuje 
cílené strukturální narušení (**perturbaci**) a z tohoto modifikovaného stavu spustí nové lokální prohledávání. Perturbace musí být 
dostatečně velká pro únik z lokálního optima, ale zároveň přiměřeně malá, aby se neztratila cenná informace z dosavadního průběhu.

```pascal
s = s0; s* = local_search(s); // Dosažení prvního lokálního optima
repeat
    s' = perturb(s*, search_history); // Aplikace perturbace
    s'_* = local_search(s'); // Lokální prohledávání z narušeného stavu
    s* = accept(s*, s'_*, search_memory); // Akceptační kritérium
until stopping_criteria;
```

---

## Metaheuristiky s jedním řešením

Pokročilé metody s jedním řešením umožňují průběžný únik z lokálních optim tím, že za určitých podmínek dovolují přechod na horší 
sousední stav, čímž rozšiřují pole působnosti algoritmu.

$T$ představuje globální parametr teploty, $\Delta f$ značí míru zhoršení účelové funkce ($f(s') - f(s)$), 
$D$ je pevná prahová odchylka a `LEVEL` představuje aktuální výšku hladiny (strop).

**Simulované žíhání (Simulated Annealing - SA):** Stochastická metaheuristika inspirovaná procesem ochlazování kovů v metalurgii. 
Zlepšující krok je přijat vždy. Horší krok je přijat stochasticky s pravděpodobností: $P(\text{přijetí}) = e^{-\frac{\Delta f}{T}}$. 
Teplota $T$ se v čase postupně snižuje; na začátku je vysoká (vysoká diversifikace), na konci konverguje k nule (čistá intenzifikace).

**Record-to-record travel (RRT):** Deterministická metaheuristika odvozená od simulovaného žíhání. Algoritmus si udržuje hodnotu 
historicky nejlepšího nalezeného řešení (`RECORD`). Nové náhodně vygenerované sousední řešení $s'$ je akceptováno, pokud jeho hodnota 
nepřekročí stávající rekord o více než pevně stanovenou prahovou odchylku $D > 0$: $f(s') < \text{RECORD} + D$.

**Algoritmus Velké potopy (Great Deluge - GD):** Deterministický algoritmus založený na analogii s hledačem cesty, který se snaží při 
stoupající vodě zůstat na suchu. Parametr `LEVEL` představuje neustále klesající strop (v minimalizační verzi). Každé nové řešení je 
přijato pouze tehdy, nachází-li se pod touto hladinou ($f(s') < \text{LEVEL}$). Rychlost poklesu hladiny určuje kompromis mezi časem 
výpočtu a kvalitou výsledku.

**Zakázané prohledávání (Tabu Search - TS):** Deterministická metoda využívající paměťové struktury. V každé iteraci vybere nejlepšího 
přípustného souseda ze všech dostupných, i když přináší zhoršení účelové funkce. *Tabu list* je FIFO fronta uchovávající atributy posledních 
provedených tahů (např. prohozené dvojice prvků u TSP). Tyto atributy jsou po určitý počet iterací zakázané, což brání zacyklení výpočtu. 
*Aspirační kritérium (Aspiration criterion)* představuje podmínku umožňující ignorovat tabu status, nejčastěji pokud daný tah vede na 
řešení lepší než dosavadní historické maximum ($f(s') < f(s_{best})$).

**Prohledávání s velmi velkým okolím (Very Large-Scale Neighborhood Search - VLNS):** Využívá se, pokud velikost okolí roste exponenciálně 
nebo jako vysoký polynom. Cílem je najít zlepšujícího souseda efektivně bez kompletní enumerace.
* *Vyhazovací řetězce (Ejection chains):* Sekvence lokálních opravných operací (přesunů). První krok odstraní aktuální defekt (porušení omezení), 
ale vyvolá vznik sekundárního defektu. Následující krok opraví ten sekundární, přičemž vyvolá terciární. Řetězec úspěšně končí, když se 
defekt v nějakém kroku zcela eliminuje bez generování nového.
* *Large Neighborhood Search (LNS):* Metaheuristika založená na opakované destrukci (**Destroy**) a následné opravy (**Repair**) aktuálního 
řešení. Algoritmus záměrně znehodnotí část aktuálního stavu a následně ji rekonstruuje výhodnějším způsobem. Destrukce se provádí jako 
*Random removal* (náhodné odebrání), *Worst-case removal* (odebrání prvků s nejhorším přínosem) nebo *Shaw removal* (odebrání podobných entit). 
Oprava využívá *Greedy insertion* (vkládání na lokálně nejlepší pozice) nebo *Regret insertion* (upřednostňuje entity s nejvyšší hodnotou 
lítosti – rozdílem skóre mezi prvním a druhým nejlepším přiřazením).

```pascal
s := initialSolution; s_best := initialSolution;
repeat
    s' := s;
    destroy part of s' using a destroy heuristic;
    repair s' using a repair heuristic;
    if f(s') < f(s_best) then s_best := s';
    if accept(s', s) then s := s';
until stopping_criteria_satisfied;
return s_best;
```

* *Variable Neighborhood Search (VNS):* Strategie postavená na systematické změně struktur okolí během prohledávání. *Variable Neighborhood Descent (VND)* je čistě deterministická verze, která prohledává řadu okolí $N_1, N_2, \dots$ sekvenčně. Při nalezení zlepšení se vrací k $N_1$, při selhání 
přechází na širší strukturu okolí ($N_{l+1}$). *Basic VNS* kombinuje stochatickou fázi „třesení“ (**shaking**) pro únik ze stávajícího lokálního 
minima s následným intenzivním lokálním prohledáváním (nejčastěji pomocí VND).

---

## Populační metaheuristiky

Na rozdíl od metod s jedním řešením pracují populační metaheuristiky (Population-based metaheuristics) s celou množinou (populací) 
kandidátních řešení současně. To jim umožňuje efektivně diversifikovat prohledávání stavového prostoru a snižovat riziko uvíznutí 
v lokálním optimu.

Podle charakteru vnitřní interakce se populační algoritmy dělí do dvou hlavních kategorií:
* **Založené na evoluci (Evolution-based):** Jedinci z aktuální populace jsou přímo vybráni a reprodukováni pomocí biologicky 
inspirovaných operátorů (křížení, mutace). Dochází k přenosu a míchání genetické informace konkrétních jedinců (např. Genetické 
algoritmy, Evoluční strategie).
* **Založené na sdílené paměti (Blackboard-based):** Jedinci přímo nereprodukují své řetězce, ale svými vlastnostmi přispívají do 
společně sdílené paměti. Nová populace kandidátů se v dalším kroku generuje čistě na základě aktuálního stavu této sdílené paměti 
(např. feromonová matice u optimalizace mravenčí kolonou).

Kvalita výpočtu silně závisí na inicializaci populace. *Náhodné generování* je jednoduché, ale pro silně omezené problémy je náročné 
nalézt platné počáteční stavy. *Sekvenční či paralelní diversifikace* umisťuje jedince cíleně tak, aby maximalizovala jejich vzájemnou 
vzdálenost a pokryla co největší plochu prostoru. *Heuristická inicializace* předvyplní populaci výsledky rychlých lokálních algoritmů, 
což urychlí start, ale zvyšuje riziko předčasné ztráty diverzity.

---

## Evoluční algoritmy

Evoluční algoritmy (Evolutionary Algorithms - EA) modelují optimalizační proces jako digitální simulaci biologické evoluce, 
postavené na přírodním výběru a přežití nejsilnějších (*survival of the fittest*). 

$\mu$ značí počet rodičů v populaci, $\lambda$ je počet potomků, $p_c$ představuje pravděpodobnost 
křížení, $p_m$ je pravděpodobnost mutace, $f_i$ vyjadřuje zdatnost (fitness) $i$-tého jedince a $p_i$ je pravděpodobnost jeho výběru.

**Hlavní větve evolučních výpočtů:**
* *Genetické algoritmy (Genetic Algorithms - GA):* Původně navržené pro binární řetězce. Kladou hlavní důraz na operátor křížení nad 
dvěma rodiči, zatímco mutace slouží pouze jako sekundární operátor pro udržení diverzity. Pracují s fixními pravděpodobnostmi $p_c$ a $p_m$.
* *Evoluční strategie (Evolution Strategies - ES):* Vyvinuty primárně pro kontinuální optimalizaci a vektory reálných čísel. Křížení je 
méně časté, hlavní evoluční pohon zajišťuje mutace. Používají striktní elitářské nahrazování. U strategií $(\mu + \lambda)$-ES se nová 
generace vybírá ze spojené a setříděné množiny rodičů i potomků dohromady. U verzí $(\mu, \lambda)$-ES se nová generace vybírá striktně 
pouze z řad potomků, rodiče bezpodmínečně vymírají (vhodné pro dynamická prostředí).
* *Genetické programování (Genetic Programming - GP):* Jedinci jsou přímo vykonatelné programy reprezentované nelineárně pomocí stromových 
grafových struktur (např. symbolická regrese, kde je cílem nalézt matematický vzorec nejlépe odpovídající naměřeným datům).

<img alt="img.png" src="img/metody_umele_inteligence/evolalg.png" width="400"/>

**Strategie výběru rodičů:** Selekční tlak určuje, jakým způsobem jsou upřednostňováni zdatnější jedinci před slabšími při výběru rodičů.
* *Ruletový výběr (Roulette wheel selection):* Pravděpodobnost výběru jedince je přímo úměrná jeho fitness vůči sumě fitness celé populace: 
$p_i = f_i / \sum f_j$. Pokud se v populaci objeví jeden dominantní jedinec, strategie ho volí opakovaně, což vede k rychlému zaplavení 
populace jeho kopiemi a k předčasné konvergenci (premature convergence).
* *Stochastické univerzální vzorkování (Stochastic Universal Sampling - SUS):* Používá pomyslné ruletové kolo s více rovnoměrně rozmístěnými 
ukazateli. Jedním roztočením vybere celou sadu rodičů naráz, což dává stabilnější selekční tlak a šanci i slabším jedincům.
* *Turnajový výběr (Tournament selection):* Náhodně vybere $k$ jedinců z populace, kteří mezi sebou porovnají fitness. Vítěz turnaje se stává 
rodičem. Proces se nezávisle opakuje $\mu$-krát, přičemž velikost turnaje $k$ přímo řídí sílu selekčního tlaku.
* *Výběr podle pořadí (Rank-based selection):* Jedinci se seřadí podle výkonnosti a pravděpodobnost se počítá z jejich pořadového indexu (ranku), 
nikoli z absolutní hodnoty fitness. Tím se eliminuje vliv extrémních rozdílů ve fitness hodnotách.

<img alt="roulette-vs-stochast.png" src="img/metody_umele_inteligence/roulette-vs-stochast.png" width="300"/>

**Reprodukční strategie:** Genetické operátory (křížení a mutace) zajišťují vznik nového potomstva prostřednictvím kombinace a drobných úprav genetické informace rodičů.
* *Křížení lineárních řetězců (Linear crossover):* Kombinuje genetickou informaci dvou rodičů s pravděpodobností $p_c \in [0.45, 0.95]$. 
Používá se jednobodové, vícebodové nebo uniformní křížení, kde se o každém genu potomka rozhoduje nezávisle s 50% pravděpodobností.
* *Pořadové křížení (Order Crossover - OX):* Specifická metoda pro permutační křížení. Standardní metody by u permutací způsobily duplicitu prvků, OX proto zachovává vybraný souvislý úsek od prvního rodiče a zbytek pozic doplní z druhého rodiče cyklicky bez vzniku duplicit.
* *Mutace (Mutation):* Představuje malé stochastické narušení jednoho jedince. Pravděpodobnost se záměrně nastavuje nízko, typicky $p_m \approx 1/k$, kde $k$ vyjadřuje celkový počet genů.

**Nahrazovací strategie:** Určují mechanismus a pravidla, podle kterých nově vytvoření potomci nahrazují stávající jedince v populaci pro další generaci.
* *Generační nahrazování (Generational replacement):* Strategie, při které nově vzniklí potomci kompletně nahradí celou původní generaci rodičů.
* *Ustálený stav (Steady-state):* Konzervativní přístup, kde noví potomci v populaci nahradí pouze vybrané nejhorší jedince, zatímco zbytek populace zůstává.
* *Elitářství (Elitism):* Zajišťuje, že nejlepší jedinci z předchozí generace automaticky postupují dál bez rizika, že o ně křížením přijdeme. Urychluje konvergenci, ale zvyšuje riziko uvíznutí v lokálním optimu.

<img alt="img.png" src="img/metody_umele_inteligence/crossover.png" width="700"/>

---

## Inteligence hejna

Subdisciplína inspirovaná kolektivním, decentralizovaným chováním sociálně žijících biologických druhů. Systémy se skládají z velkého 
množství jednoduchých agentů bez centrálního řízení. Komunikace probíhá lokálně a nepřímo prostřednictvím modifikace okolního 
prostředí – tento jev se označuje jako **stigmergie (stigmergy)**.

$\tau_{ij}$ představuje množství feromonu na hraně mezi uzly $i$ a $j$, $\rho$ je koeficient odpařování feromonu, 
$\Delta$ značí přírůstek feromonu, $p_{ij}$ je pravděpodobnost přechodu z uzlu $i$ do $j$, $\eta_{ij}$ vyjadřuje lokální viditelnost uzlu $j$ 
($1/d_{ij}$), $d_{ij}$ je fyzická vzdálenost hran a $\alpha, \beta$ jsou parametry určující relativní vliv feromonu a viditelnosti.

**Optimalizace mravenčí kolonou (Ant Colony Optimization - ACO)** simuluje vyhledávání nejkratší cesty mezi mraveništěm a zdrojem potravy 
pomocí vylučování chemické stopy – feromonu. Feromon se v čase přirozeně odpařuje. Na kratší trati se mravenci otočí rychleji, feromonová 
stopa se tam vrství intenzivněji a vyšší koncentrace následně přitahuje další mravence.
* Odpařování: Provádí se plošně na všech hranách optimalizačního grafu, což umožňuje únik z lokálních optim: 
$$\tau_{ij} = (1 - \rho)\tau_{ij}$$
* Posílení: Přidání feromonu na hrany. Nejčastější je *off-line aktualizace* na konci generace, kdy se posílení provede pouze na hranách, které tvoří historicky nejlepší nalezenou trasu:

  $$
  \tau_{ij} = \tau_{ij} + \Delta \quad \forall (i, j) \in \text{best\_solution}
  $$

Při aplikaci na Problém obchodního cestujícího (TSP) si mravenec v uzlu $i$ vybírá následující město $j$ z množiny dosud nenavštívených měst 
$S$ na základě kombinace feromonové stopy a lokální viditelnosti $\eta_{ij} = 1/d_{ij}$:
$$p_{ij} = \frac{\tau_{ij}^\alpha \times \eta_{ij}^\beta}{\sum_{k \in S} \tau_{ik}^\alpha \times \eta_{ik}^\beta}$$
Explicitní parametry $\alpha$ a $\beta$ definují relativní váhu obou složek. Pokud je $\alpha = 0$, algoritmus se chová jako čistě stochastické 
greedy prohledávání podle geografické blízkosti.

---

## Plánování

Klasické prohledávání stavového prostoru nebere v úvahu sémantiku akcí. Automatické plánování (Automated Planning) dává agentovi 
schopnost uvažovat o budoucích důsledcích svého chování na základě explicitního modelu světa, což umožňuje doménově nezávislým 
algoritmům nalézt optimální posloupnost kroků pro dosažení cíle.

$\Sigma$ představuje stavový transformační systém, $S$ je množina stavů, $A$ značí množinu akcí pod kontrolou 
plánovače, $E$ je množina vnějších událostí a $\gamma$ vyjadřuje přechodovou funkci.

Formálně je plánovací prostředí modelováno jako stavový transformační systém: 
$$\Sigma = (S, A, E)$$
Změnu stavu řídí přechodová funkce: 
$$\gamma: S \times (A \cup E) \rightarrow 2^S$$
Plánovací úloha hledá posloupnost kroků vedoucí z počátečního stavu do cíle, přičemž cíl může být definován jako koncový stav, omezení nad 
trajektorií stavů nebo optimalizace účelové funkce (celkový čas trvání, finanční náklady).

---

## Reprezentace problému

Aby bylo možné plánovací problémy řešit univerzálními, doménově nezávislými algoritmy, je nutné zavést formální reprezentaci stavů 
a akcí. Klasický přístup staví na prvořádové logice (např. koncept STRIPS).

$o$ představuje plánovací operátor, $a$ je plně instanciovaná akce, $s$ značí stav světa, $P$ je plánovací problém, 
$O$ vyjadřuje doménový model, $s_0$ je počáteční stav, $g$ představuje cílové podmínky, $S_g$ je množina vyhovujících stavů a $\pi$ vyjadřuje výsledný plán.

Aby mohl plánovač logicky odvozovat, definuje stav jako konečnou množinu plně instanciovaných atomů (neobsahuje volné proměnné). 
*Fluenty (Fluents)* jsou atomy, jejichž pravdivostní hodnota se v závislosti na akcích mění (např. `at(robot, location)`). *Rigidní atomy (Rigid atoms)* jsou konstantní a nezávislé na stavu systému (např. `adjacent(loc1, loc2)`). Při vyhodnocování platí **Předpoklad uzavřeného světa (Closed World Assumption - CWA)** – jakýkoli atom, který není explicitně uveden v popisu daného stavu, je považován za nepravdivý.

**Plánovací operátor ($o$):** Parametrická šablona definovaná jako trojice $(\text{name}(o), \text{precond}(o), \text{effects}(o))$. Název obsahuje proměnné, 
předpodmínky určují literály nutné pro spuštění a efekty definují změny po provedení. Jakmile proměnné v operátoru nahradíme konkrétními objekty 
z domény, vzniká plně instanciovaná **akce ($a$)**.

<img alt="img.png" src="img/metody_umele_inteligence/operators-predicates.png" width="500"/>

Akce je aplikovatelná na stav $s$, pokud platí: $\text{precond}^+(a) \subseteq s \ \land \ \text{precond}^-(a) \cap s = \emptyset$. Výsledný stav po aplikaci 
akce definuje přechodová funkce jako odebrání negativních a přidání pozitivních efektů: 
$$\gamma(s, a) = (s \setminus \text{effects}^-(a)) \cup \text{effects}^+(a)$$
Plánovací problém $P = (O, s_0, g)$ definuje doménový model $O$, počáteční konfiguraci $s_0$ a cílové literály $g$. Výsledný *plán* $\pi$ je uspořádaná 
sekvence akcí, jejíž sekvenční aplikace na počáteční stav vede do cílového stavu: 
$$\gamma(s_0, \pi) \in S_g$$

Pro kódování klasických plánovacích úloh se využívá standardizovaný formální jazyk **PDDL (Planning Domain Definition Language)**. Ten striktně 
odděluje fyzikální vlastnosti světa (*Domain File* – typy objektů, predikáty a akce) od konkrétního zadání (*Problem File* – objekty, výchozí stav 
`:init` a cílové podmínky `:goal`).

```lisp
;; Ukázka z Domain souboru (Gripper)
(:action move
   :parameters (?x ?y)
   :precondition (and (ROOM ?x) (ROOM ?y) (at-robby ?x))
   :effect (and (at-robby ?y) (not (at-robby ?x))))

;; Ukázka z Problem souboru (Gripper)
(:init (ROOM rooma) (ROOM roomb) (at-robby rooma) (at-ball ball1 rooma))
(:goal (at-ball ball1 roomb))
```

---

## Plánování se stavovým prostorem

Při plánování se stavovým prostorem (State Space Planning) představují uzly prohledávacího stromu konkrétní stavy světa a hrany 
reprezentují přechody realizované aplikací akcí. Cílem je vyhledat propojení mezi startem a cílem.

$s_0$ vyjadřuje počáteční stav, $g$ značí cíl plánování, $a$ je akce a $\gamma^{-1}(g, a)$ představuje regresní množinu (nový podcíl).

**1. Dopředné plánování (Forward Planning / Progression):** Algoritmus začíná v počátečním stavu $s_0$ a rozbaluje strom směrem dopředu k cíli 
aplikací všech akcí, které jsou v daném uzlu proveditelné. Je korektní a úplné. Hlavním úskalím je velký faktor větvení (branching factor) 
– počáteční stav může generovat množství aplikovatelných akcí nesouvisejících s cílem. V reálných plánovačích se proto dopředné prohledávání 
kombinuje s algoritmem $A^*$, pokročilými doménově nezávislými heuristikami a detekcí duplicitních stavů.

**2. Zpětné plánování (Backward Planning / Regression):** Startuje od popisu cíle $g$ a postupuje pozpátku k počátečním stavům vytvářením 
podcílů. V každém kroku vybere pouze takovou akci, která je pro aktuální cíl relevantní ($g \cap \text{effects}(a) \neq \emptyset$ a efekty 
nejsou v přímém konfliktu s cílem). Výsledkem je regresní množina, která reprezentuje nový podcíl: 
$$\gamma^{-1}(g, a) = (g \setminus \text{effects}(a)) \cup \text{precond}(a)$$
Zpětný přístup vykazuje menší faktor větvení než dopředné plánování, protože vůbec neuvažuje irelevantní akce, ale vyžaduje striktní 
implementaci detekce cyklů na prohledávané větvi.

**3. Částečně instanciované zpětné plánování (Lifted Backward Planning):** Tato technika odstraňuje zbytečné větvení zpětného plánování 
tím, že nedosazuje konstanty okamžitě, ale ponechává parametry akcí jako volné proměnné, dokud to není nutné. Využívá 
**MGU (Most General Unifier - nejobecnější unifikátor)**, což je minimální nutná substituce proměnných, která sjednotí atom v cíli s atomem v efektech operátoru. 
Tímto způsobem dokáže algoritmus v jednom kroku pokrýt celou sadu potenciálních objektů bez větvení stromu, výměnou za složitější 
správu unifikačních omezení.

<img alt="img.png" src="img/metody_umele_inteligence/back-for-plan.png" width="800"/>

---

## Práce s neurčitostí

Čistě logické odvozování v reálném světě naráží na neúplnost informací a šum senzorů. Teorie pravděpodobnosti poskytuje exaktní matematický 
aparát pro reprezentaci neurčitosti, kvantifikaci rizika a optimální rozhodování.

$Y$ představuje dotazovanou proměnnou, $Z$ značí množinu skrytých proměnných, $z$ je konkrétní stav skrytých 
proměnných, $x, y$ vyjadřují konkrétní jevy a $P(x \mid y)$ je podmíněná pravděpodobnost jevu $x$ za předpokladu $y$.

* **Sdružená pravděpodobnostní distribuce (Joint probability distribution):** Tabulka pokrývající pravděpodobnosti všech možných kombinací 
stavů v doméně. Umožňuje odpovědět na jakýkoli dotaz, ale její velikost roste exponenciálně ($2^n$).
* **Marginalizace (Marginalization / Summing out):** Výpočet pravděpodobnosti menší množiny proměnných sečtením (integrováním) pravděpodobností 
přes všechny možné hodnoty ostatních (skrytých) proměnných $Z$: 
$$P(Y) = \sum_{z \in Z} P(Y, z)$$
* **Bayesův teorém (Bayes' theorem):** Základní vztah umožňující otočit směr podmínění, což je klíčové pro diagnostické systémy určující 
pravděpodobnost skryté příčiny na základě pozorovaných efektů: 
$$P(\text{příčina} \mid \text{efekt}) = \frac{P(\text{efekt} \mid \text{příčina}) \times P(\text{příčina})}{P(\text{efekt})}$$

---

## Bayesovské sítě

Plná sdružená distribuce je kvůli své velikosti v praxi nepoužitelná. Bayesovská síť (Bayesian Network) řeší tento problém tím, že 
explicitně reprezentuje vztahy podmíněné nezávislosti mezi náhodnými proměnnými pomocí grafu.

$X_i$ představuje náhodnou proměnnou (uzel v grafu), $\text{parents}(X_i)$ značí přímé rodiče uzlu $X_i$ 
a $x_i$ vyjadřuje konkrétní hodnotu proměnné.

Jedná se o **Orientovaný acyklický graf (Directed Acyclic Graph - DAG)**. Uzly představují náhodné proměnné a hrany vyjadřují přímou závislost. 
Každý uzel obsahuje vlastní **Tabulku podmíněné pravděpodobnosti (Conditional Probability Table - CPT)**, která definuje distribuci 
$P(X_i \mid \text{Parents}(X_i))$ vůči všem kombinacím hodnot jeho přímých rodičů. Hodnotu libovolného globálního stavu lze spočítat jako součin 
lokálních tabulek: 
$$P(x_1, \dots, x_n) = \prod_{i=1}^{n} P(x_i \mid \text{parents}(X_i))$$
Díky této dekompozici klesá paměťová náročnost z původních $2^n$ na pouhých $n \cdot 2^k$ parametrů (kde $k$ je maximální počet rodičů uzlu). 
Při stavbě sítě se doporučuje volit *kauzální směr* (od příčin k efektům) $\rightarrow$ vede k řídkým grafům s minimem hran. Diagnostické 
řazení (od efektů k příčinám) generuje hustě propojené sítě s mnoha redundantními závislostmi.

<img alt="cpt.png" src="img/metody_umele_inteligence/cpt.png" width="300"/>

---

## Exaktní a aproximační odvozování

Odvozování (Inference) v Bayesovských sítích spočítá výslednou podmíněnou distribuci dotazované proměnné $X$ na základě pozorované evidence 
$e$, přičemž volné proměnné $Y$ jsou eliminovány marginalizací: 
$$P(X \mid e) = \alpha \sum_{y} P(X, e, y)$$

$X$ představuje dotazovanou proměnnou, $e$ značí pozorovanou pevnou evidenci, $Y$ vyjadřuje skryté proměnné 
určené k eliminaci, $y$ je konkrétní hodnota skrytých proměnných, $\alpha$ je normalizační konstanta a $w$ vyjadřuje přiřazenou váhu vzorku.

**1. Exaktní odvozování (Exact Inference):** Cílem exaktních metod je určit matematicky přesný výsledek distribuce.
* *Odvozování výčtem (Inference by enumeration):* Top-down rekurzivní procházení stromu možných stavů. Je neefektivní, protože opakovaně 
počítá identické podvýrazy v různých větvích.
* *Eliminace proměnných (Variable elimination):* Algoritmus založený na principu dynamického programování. Pravděpodobnostní tabulky (CPTs) 
jsou uvažovány jako matematické faktory (matice). Výpočet probíhá zdola nahoru pomocí operace bodového součinu souvisejících faktorů a následného 
vysčítání (eliminace) skrytých proměnných. Pro sítě s topologií stromu (poly-trees) je složitost lineární vůči velikosti sítě ($O(n \cdot d^k)$). 
Pro obecné, vícenásobně propojené sítě je tento problém NP-těžký, přičemž náročnost diktuje velikost největšího faktoru vytvořeného během eliminace.

<img alt="img.png" src="img/metody_umele_inteligence/inf-by-enum.png" width="400"/>

**2. Aproximační odvozování (Approximate Inference):** Pro rozsáhlé sítě je exaktní výpočet nezvládnutelný, využívají se proto stochastické 
Monte Carlo algoritmy, které generují velké množství náhodných vzorků a výsledek odhadují z jejich statistické četnosti.
* *Přímé vzorkování (Direct sampling):* Generuje vzorky od kořenů k listům v topologickém uspořádání grafu podle pravděpodobností v CPT. 
Neumí nativně zakomponovat pevnou evidenci.
* *Zamítavé vzorkování (Rejection sampling):* Generuje kompletní vzorky pomocí přímého vzorkování, ale jakmile je vygenerovaný vzorek v rozporu 
s pozorovanou evidencí $e$, okamžitě ho zamítne (zahodí). Pokud je evidence vzácná, algoritmus zlikviduje většinu vzorků a efektivita prudce klesá.
* *Váhová věrohodnost (Likelihood weighting):* Algoritmus, který předchází plýtvání vzorky. Proměnné obsažené v evidenci $e$ zafixuje napevno 
a náhodně vzorkuje pouze volné proměnné. Každému vzorku přiřadí váhu $w$, jež odpovídá součinu podmíněných pravděpodobností všech fixovaných 
proměnných v momentě průchodu. Výsledná distribuce se následně normalizuje přes sumu těchto vah.

<img alt="img.png" src="img/metody_umele_inteligence/directsampl-likelweigh.png" width="800"/>

---

## Čas a neurčitost

Při modelování dynamického světa v čase se využívá rozdělení na diskrétní časové řezy (*time slices*). Rozlišujeme skryté náhodné 
proměnné $X_t$ (stav systému) a pozorovatelné proměnné $E_t$ (evidence/měření ze senzorů).

$X_t$ představuje skrytou stavovou proměnnou v čase $t$, $E_t$ značí pozorovatelnou proměnnou v čase $t$, 
$e_t$ vyjadřuje konkrétní hodnotu pozorování, $X_{0:t}$ je sekvence skrytých stavů od času 0 do $t$ a $E_{1:t}$ vyjadřuje sekvenci pozorování 
od času 1 do $t$.

Aby byl model výpočetně realizovatelný, zavádí se dva základní předpoklady:
* *Markovský předpoklad (Markov property / assumption):* Aktuální skrytý stav systému závisí striktně pouze na stavu bezprostředně předcházejícím: 
$P(X_t \mid X_{0:t-1}) = P(X_t \mid X_{t-1})$. Starší historie nemá na budoucnost vliv. Pokud se pravidla přechodů v čase nemění, jde o stacionární proces.
* *Senzorický Markovský předpoklad (Sensor Markov assumption):* Evidence $E_t$ v čase $t$ závisí výhradně na současném skrytém stavu světa $X_t$: 
$P(E_t \mid X_{0:t}, E_{1:t-1}) = P(E_t \mid X_t)$.

Celý časový vývoj lze popsat dynamickou Bayesovskou sítí jako součin lokálních přechodových a senzorických modelů: 
$$P(X_{0:t}, E_{1:t}) = P(X_0) \prod_{i=1}^{t} P(X_i \mid X_{i-1}) P(E_i \mid X_i)$$

Nad tímto časovým modelem rekurzivně řešíme čtyři základní úlohy:
1. Filtrace (Filtering): Výpočet aktuálního stavu víry na základě všech dosavadních pozorování: $P(X_t \mid e_{1:t})$. Využívá rekurzivní odhad 
(*forward message passing*) bez nutnosti procházet celou historii od nuly.
2. Predikce (Prediction): Výpočet pravděpodobnostního rozdělení budoucího stavu $k$ kroků dopředu: $P(X_{t+k} \mid e_{1:t})$. Bez přísunu nové 
evidence distribuce v čase konverguje ke stacionárnímu rozdělení.
3. Vyhlazování (Smoothing): Výpočet pravděpodobnosti stavu v minulosti na základě evidence až do současnosti: $P(X_k \mid e_{1:t})$ pro $0 \le k < t$. 
Počítá se pomocí obousměrného posílání zpráv (*forward-backward* průchod).
4. Nejpravděpodobnější vysvětlení (Most likely explanation): Hledání ucelené sekvence skrytých stavů, která s nejvyšší pravděpodobností 
vygenerovala danou sekvenci pozorování (řeší se Viterbiho algoritmem).

---

## Teorie užitku

Zatímco teorie pravděpodobnosti popisuje, čemu by měl agent věřit, teorie užitku definuje jeho vnitřní cíle a preference. Jejich spojením 
vzniká **teorie rozhodování (Decision theory)**, která poskytuje rámec pro racionální chování agenta v neurčitém prostředí.

$s$ představuje konkrétní stav světa, $U(s)$ vyjadřuje hodnotu užitku stavu $s$, $a$ značí akci vybranou 
agentem, $e$ je pozorovaná evidence a $EU(a \mid e)$ představuje očekávaný užitek akce $a$ za předpokladu evidence $e$.

* **Princip maximalizace očekávaného užitku (Maximum Expected Utility - MEU):** Každému stavu přiřadí užitková funkce $U(s)$ reálné číslo 
vyjadřující jeho atraktivitu. Očekávaný užitek akce $a$ při znalosti evidence $e$ je dán váženým průměrem užitků všech jejích možných výsledných stavů: 
$$EU(a \mid e) = \sum_{s} P(\text{Result}(a) = s \mid a, e) U(s)$$
Racionální agent bezpodmínečně volí akci, která tento očekávaný užitek maximalizuje: $a = \arg\max_a EU(a \mid e)$.
* **Odpor k riziku (Risk aversion):** Vyjadřuje fakt, že finanční hodnoty nerostou lineárně s užitkem. Užitek z peněz má konkávní charakter, 
což znamená, že přírůstek bohatství přináší klesající marginální užitek.
* **Rozhodovací sítě (Decision Networks / Influence Diagrams):** Mechanismus pro výpočet optimálních rozhodnutí, který rozšiřuje klasické 
Bayesovské sítě o rozhodovací uzly (obdélníky – body volby akce agenta) a užitkové uzly (kosočtverce – vyjadřují užitkovou funkci).


<img alt="img.png" src="img/metody_umele_inteligence/decision net.png" width="400"/>

---

## Markovský rozhodovací proces

Markovský rozhodovací proces (Markov Decision Process - MDP) formalizuje sekvenční rozhodovací problém v plně pozorovatelném, 
stochastickém prostředí s Markovským přechodovým modelem a aditivními odměnami $R(s)$. 

$S$ představuje množinu všech stavů, $A(s)$ značí množinu akcí dostupných ve stavu $s$, $P(s' \mid s, a)$ 
vyjadřuje pravděpodobnost přechodu ze stavu $s$ do $s'$ pomocí akce $a$, $R(s)$ představuje funkci odměny, $\pi(s)$ značí strategii, $\pi^*$ 
vyjadřuje optimální strategii a $\gamma$ představuje diskontní faktor.

<img alt="gridworld-example.png" src="img/metody_umele_inteligence/gridworld-example.png" width="400"/>

Řešením MDP není fixní posloupnost akcí, ale strategie $\pi(s)$ – funkce, která doporučuje nejlepší akci pro každý představitelný stav. 
Optimální strategie $\pi^*$ maximalizuje očekávaný kumulativní užitek. Při nekonečném časovém horizontu modeluje preferenci okamžitých 
odměn před budoucími pomocí diskontního faktoru $\gamma \in (0, 1)$, což matematicky zaručuje konvergenci nekonečné řady: 
$$U([s_0, s_1, s_2, \dots]) = \sum_{t=0}^{\infty} \gamma^t R(s_t) \le \frac{R_{max}}{1 - \gamma}$$

Základním vztahem pro výpočet užitků stavů je **Bellmanova rovnice (Bellman equation)**. Určuje, že užitek stavu je roven okamžité odměně 
plus očekávanému diskontovanému užitku následného stavu za předpokladu, že agent zvolí optimální akci: 
$$U(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U(s')$$

---

## Iterace hodnot

Iterace hodnot (Value Iteration) je iterativní numerický algoritmus pro řešení MDP a výpočet optimálních hodnot užitků jednotlivých stavů.

$U_i(s)$ vyjadřuje odhad užitku stavu $s$ v $i$-té iteraci, $R(s)$ značí okamžitou odměnu, $\gamma$ představuje 
diskontní faktor, $A(s)$ vyjadřuje množinu dostupných akcí, $P(s' \mid s, a)$ značí pravděpodobnost přechodu, $\delta$ je maximální změna 
užitku a $\epsilon$ představuje mez přesnosti.

Algoritmus začíná s libovolnými počátečními hodnotami užitků a v každém kroku provádí tzv. **Bellmanovu aktualizaci (Bellman update)** pro všechny stavy: 
$$U_{i+1}(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U_i(s')$$
Bellmanova aktualizace vykazuje matematickou vlastnost kontrakce s faktorem $\gamma$, což garantuje konvergenci k jedinému optimálnímu řešení 
nezávisle na počátečních hodnotách. Iterace končí, jakmile je maximální změna užitku menší než stanovená mez: $\delta < \epsilon(1-\gamma)/\gamma$. 
Odvozená strategie $\pi_i$ v praxi konverguje k optimální verzi mnohem dříve, než plně zkonvergují samotné numerické hodnoty užitků $U_i$.

---

## Iterace strategie

Iterace strategie (Policy Iteration) je alternativní přístup k řešení MDP založený na myšlence, že pro nalezení optimálního chování 
není nutné znát zcela přesné hodnoty užitků. 

$\pi_i(s)$ vyjadřuje strategii v $i$-té iteraci, $U^{\pi_i}(s)$ značí přesný užitek stavu $s$ při dodržování 
strategie $\pi_i$, $R(s)$ představuje okamžitou odměnu, $\gamma$ vyjadřuje diskontní faktor a $P(s' \mid s, a)$ značí pravděpodobnost přechodu.

Algoritmus začíná s náhodnou výchozí strategií a cyklicky střídá dva dedikované kroky:
1. **Evaluace strategie (Policy evaluation):** Pro aktuální fixní strategii $\pi_i$ se spočítají přesné užitky stavů $U^{\pi_i}$. Protože akce 
jsou pevně dané strategií, Bellmanova rovnice ztrácí operátor maxima a zjednoduší se na systém lineárních rovnic s kubickou složitostí $O(n^3)$: 
$$U^{\pi_i}(s) = R(s) + \gamma \sum_{s'} P(s' \mid s, \pi_i(s)) U^{\pi_i}(s')$$
2. **Greedy aktualizace (Greedy update / Policy improvement):** Na základě nově spočtených užitků se aktualizuje doporučená akce pro každý stav: 
$$\pi_{i+1}(s) = \arg\max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U^{\pi_i}(s')$$

Algoritmus končí v momentě, kdy v kroku greedy aktualizace nedojde k žádné změně strategie. Jelikož je počet možných stacionárních strategií 
konečný a každá iterace přináší strukturální zlepšení, algoritmus garantovaně konverguje a v praxi vyžaduje podstatně méně iterací než čistá 
iterace hodnot.

<img alt="img.png" src="img/metody_umele_inteligence/markov.png" width="400"/>

---

## Robotika

Robotika studuje fyzikální agenty provádějící úkoly manipulací s reálným světem. K tomu jsou vybaveni efektory (kola, klouby, chapadla) pro 
působení silou na okolí a senzory pro vnímání prostředí. Senzory rozlišujeme na pasivní, které pouze zachycují externí signály (kamery), 
a aktivní, které vysílají energii do okolí a měří její odraz (LiDAR, sonar).

Stav robota se dělí na *kinematický stav* (geometrická pozice a orientace kloubů) a *dynamický stav* (přidaná rychlost, zrychlení a působící síly). 
Percepce (vnímání) je proces mapování měření ze senzorů (zatížených šumem) do vnitřní reprezentace světa. Tento proces se typicky modeluje jako 
temporální odvozování pomocí Dynamických Bayesovských sítí, které pokrývá lokalizaci (zjišťování polohy robota v mapě), mapování (konstrukce 
mapy neznámého prostředí) a **SLAM (Simultaneous Localization and Mapping)**, kdy robot současně staví mapu i odhaduje svou polohu v ní.

---

## Plánování pohybu robota

Základním problémem plánování pohybu (Motion Planning) je nalézt spojitou sekvenci konfigurací, která bezpečně převede robota 
z počátečního stavu $q_I$ do cíle $q_G$ bez kolize s překážkami. Autonomní chování se dělí do tří integrovaných úrovní:
1. Plánování úloh (Task planning): Rozhoduje o sekvencích vysokoúrovňových akcí (např. jdi k objektu $\rightarrow$ uchop objekt).
2. Plánování pohybu (Motion planning): Hledá konkrétní bezkolizní geometrickou dráhu pro splnění akce.
3. Řízení (Control): Generuje nízkoúrovňové příkazy (napětí v motorech) pro akční členy, aby robot pohyb fyzicky vykonal.

$q_I$ představuje počáteční stav (konfiguraci) robota a $q_G$ vyjadřuje cílový stav (konfiguraci) robota.

Spojitý prostor nelze prohledávat přímo, proto je nutné provést jeho diskretizaci (převod na graf či mřížku) a následně aplikovat vyhledávací 
algoritmy typu $A^*$ nebo prohledávání do šířky (BFS).

<img alt="img.png" src="img/metody_umele_inteligence/planning.png" width="400"/>

---

## Konfigurační prostor

Počítat kolize pro složité reálné těleso v trojrozměrném Pracovním prostoru (Workspace) s geometrickými překážkami je výpočetně neúnosné. 
Konfigurační prostor (Configuration space - C-space) přináší transformaci, kde je celá struktura robota reprezentována jako **jeden jediný bod**. 

$O$ představuje geometrické překážky v pracovním prostoru, $C$ vyjadřuje celkový konfigurační prostor, 
$C_{obs}$ značí prostor překážek a $C_{free}$ vyjadřuje bezpečný volný prostor konfigurací.

Počet dimenzí $C$-prostoru odpovídá přesně počtu stupňů volnosti (Degrees of Freedom - DoF) robota (např. pozice a orientace mobilního robota 
v rovině $(x, y, \theta)$ tvoří 3D prostor).
* **$C_{obs}$ (Obstacle space):** Množina všech konfigurací, ve kterých robot koliduje s překážkou $O$ nebo sám se sebou.
* **$C_{free}$ (Free space):** Bezpečný prostor konfigurací, kde ke kolizím nedochází: $C_{free} = C \setminus C_{obs}$.

Přepočet mezi souřadnicemi zajišťuje *Dopředná kinematika (Forward kinematics)*, která přepočítává známou konfiguraci kloubů na přesné souřadnice 
efektoru v pracovním prostoru. *Inverzní kinematika (Inverse kinematics)* naopak hledá potřebnou konfiguraci kloubů na základě požadované pozice 
efektoru v pracovním prostoru. Protože explicitní konstrukce celého $C_{obs}$ je analyticky téměř nemožná, v praxi se prostor pouze sonduje 
(probing) – vygenerovaný bod se pomocí dopředné kinematiky otestuje v pracovním prostoru přes "black-box" kolizní detektor.

<img alt="img.png" src="img/metody_umele_inteligence/config vs workspace.png" width="400"/>

---

## Kombinatorické a pravděpodobnostní přístupy

Při diskretizaci konfiguračního prostoru pro následné vyhledávání bezkolizní trajektorie se uplatňují dva základní metodické přístupy.

$\rho$ představuje definovaný poloměr pro spojování sousedních uzlů, $n$ značí počet náhodných bodů, 
$q_{init}$ vyjadřuje počáteční konfiguraci, $q_{new}$ je náhodně vygenerovaná konfigurace a $q_{near}$ představuje uzel stromu geometricky nejbližší k $q_{new}$.

**1. Kombinatorické (deterministické) přístupy:** Diskretizují volný prostor $C_{free}$ exaktními geometrickými či algebraickými metodami.
* *Graf viditelnosti (Visibility graph):* Spojuje úsečkami start, cíl a všechny vrcholy polygonálních překážek. Hrany protínající překážky jsou 
eliminovány. Vždy garantuje nalezení nejkratší možné cesty, ale dráha vede těsně kolem hran překážek (nulová bezpečnostní rezerva). Výpočetně náročné 
ve vyšších dimenzích.
* *Voronoiův diagram (Voronoi diagram):* Generuje cestovní mapu složenou z bodů, které mají přesně stejnou vzdálenost od dvou nebo více 
nejbližších překážek. Maximalizuje bezpečnostní odstup (*clearance*), ale cesty nejsou nejkratší a jsou vysoce citlivé na změny v pozici překážek.

<img alt="vis vs voron.png" src="img/metody_umele_inteligence/vis%20vs%20voron.png" width="700"/>

* *Dekompozice na buňky (Cell Decomposition):* Rozdeluje prostor na konečný počet nepřekrývajících se legálních oblastí (buněk). Exaktní dekompozice 
řeší svislé řezy v místech geometrických vrcholů a středy buněk propojí do grafu. Aproximační dekompozice (např. Quadtree/Octree) rekurzivně 
dělí prostor na pravidelné sub-buňky; smíšené buňky se dělí hlouběji, dokud se nenajde čistá cesta nebo se nenarazí na limit rozlišení.
* *Potenciálová pole (Potential Field Methods):* Pohyb je řízen virtuálními silovými poli. Cíl generuje atraktivní pole (přitahuje robota), 
překážky generují repulzivní pole (odtlačují ho). Robot se pohybuje ve směru gradientu výsledného pole. Metoda je rychlá, ale hrozí uvíznutí 
v lokálních minimech, kde se síly navzájem vyruší před dosažením cíle.

<img alt="img.png" src="img/metody_umele_inteligence/cell-decomp.png" width="800"/>

**2. Pravděpodobnostní přístupy (Sampling-based):** Namísto exaktní konstrukce překážek náhodně vzorkují konfigurace v $C$-prostoru a testují je 
kolizním detektorem. Poskytují pravděpodobnostní úplnost (s rostoucím počtem vzorků pravděpodobnost nalezení existujícího řešení konverguje k 1). 
Jsou efektivní ve vysokých dimenzích (4 a více DoF), ale jejich slabinou je prohledávání úzkých průchodů (*narrow passages*).
* *Probabilistická cestovní mapa (Probabilistic Roadmap - PRM) — Multi-query strategie:* Navržena pro statická prostředí, kde se plánuje opakovaně 
(vytvoří se jedna robustní mapa a nad ní se provádějí rychlé dotazy). Ve fázi učení se vygeneruje $n$ náhodných bodů v $C_{free}$ a lokální plánovač 
se pokusí propojit blízké uzly v definovaném poloměru $\rho$ do grafu. Klasické PRM vytvoří vazbu pouze tehdy, pokud kandidáti dosud neleží ve stejné 
komponentě souvislosti (šetří se drahé kolizní testy). Ve fázi dotazu se startovní a cílová konfigurace připojí k hotové mapě a cesta se vyhledá 
pomocí grafového vyhledávání ($A^*$, Dijkstra).
* *Rychle se rozrůstající náhodné stromy (Rapidly-exploring Random Trees - RRT) — Single-query strategie:* Konstruuje nový graf (strom) pro každou 
úlohu zvlášť, přičemž se rozrůstá z počáteční konfigurace $q_{init}$ přímo směrem do volného prostoru (velmi rychle expanduje do neprozkoumaných oblastí). 
Je vhodný pro dynamická prostředí a zohlednění kinematických limitů. V cyklu vygeneruje náhodnou konfiguraci $q_{new}$ v $C_{free}$, ve stromu vyhledá 
uzel $q_{near}$, který je geometricky nejblíže k $q_{new}$ a strom rozšíří z bodu $q_{near}$ malým krokem směrem k $q_{new}$ (vzniká nový platný uzel, 
ověřený kolizním detektorem). Proces se opakuje, dokud se větev stromu nedostane do dostatečné blízkosti cíle. Poskytuje přípustné, ale často 
neoptimální (klikaté) řešení.

<img alt="rrtconstuct.png" src="img/metody_umele_inteligence/rrtconstuct.png" width="400"/>
