# Metody umělé inteligence

> Prohledávání stavového prostoru, lokální prohledávání a metaheuristiky s jedním řešením, 
> populační metaheuristiky (evoluční algoritmy, inteligence hejna). 
> Plánování, reprezentace problému, plánování se stavovým prostorem. 
> Práce s neurčitostí, Bayesovské sítě, exaktní a aproximační odvozování, čas a neurčitost, 
> teorie užitku, Markovský rozhodovací proces, iterace hodnot, iterace strategie. 
> Robotika, plánování pohybu robota (konfigurační prostor, kombinatorické a pravděpodobnostní přístupy).

---

## Prohledávání stavového prostoru

Reálné inženýrské problémy (rozvrhování výroby, logistika, hledání tras) mají často tak obrovské množství kombinací, že je nelze vyřešit hrubou silou. Prohledávání stavového prostoru představuje systematický způsob, jak se v tomto moři možností zorientovat a chytře dokráčet k cíli.

**Heuristické algoritmy** jsou specifické postupy navržené na mírů jednomu konkrétnímu problému. Využívají jeho vnitřní strukturu a specifické vlastnosti (tzv. „art of discovering“). Neposkytují sice žádnou teoretickou záruku kvality výsledku ani dosažení globálního optima, ale jsou rychlé. Typickým příkladem je pravidlo *Earliest Due Date* v rozvrhování úloh (jako první zpracuj to, co nejhořlavěji spěchá) nebo hladové přiřazování nejbližší zastávky u plánování tras vozidel.

**Metaheuristiky** představují metodologii vyšší úrovně. Jsou to obecné, na konkrétním problému nezávislé řídicí strategie a šablony. Neřeší problém napřímo, ale vedou a propojují podřízené heuristiky. Díky své abstrakci jsou široce aplikovatelné napříč doménami – stejný algoritmus simulovaného žíhání tak dokáže elegantně vyřešit rozvrh zkoušek na univerzitě i trasu obchodního cestujícího.

### Diversifikace vs. Intenzifikace

Při návrhu jakékoli metaheuristiky dochází k neustálému balancování mezi dvěma protichůdnými silami. Bez jejich správné rovnováhy algoritmus buď uvízne v prvním průměrném řešení, nebo bude nekonečně bloudit prostorem.

**Diversifikace (Průzkum / Exploration)** je schopnost algoritmu opustit prozkoumané oblasti a skočit do dosud nenavštívených, vzdálených částí stavového prostoru. Poskytuje globální pohled a chrání algoritmus před uvíznutím v lokálním optimu. 

**Intenzifikace (Využití / Exploitation)** se naopak zaměřuje na detailní, lokální prohledání slibného okolí aktuálního stavu. Jde o hloubkový pohled s cílem vytěžit z dané oblasti absolutní maximum (najít lokální optimum).

<img alt="img.png" src="img/metody_umele_inteligence/divers-vs-intens.png" width="600"/>

### Reprezentace (Kódování / Encoding)

Počítač nedokáže optimalizovat reálný svět bez jeho digitálního otisku. Způsob, jakým řešení v algoritmu zakódujeme, zásadně určuje velikost prohledávacího prostoru a složitost matematických operátorů.

V rámci **lineární reprezentace** používáme řetězce symbolů. U *binárního kódování* každá proměnná nabývá hodnoty $s_i \in \{0, 1\}$, což je ideální pro Problém batohu (předmět buď vezmu, nebo ne). *Diskrétní kódování* zobecňuje binární svět do konečných celočíselných domén, což se hodí pro alokaci zdrojů (vektor udává, kterému z $m$ agentů je přiřazena $i$-tá úloha). *Reprezentace permutací* striktně vyžaduje, aby se každý prvek vyskytl v zápisu právě jednou, což perfektně modeluje sekvenční úlohy jako Problém obchodního cestujícího (TSP). Pokud lineární řetězce nestačí, nastupuje **nelineární reprezentace** postavená na grafech a stromech, využívaná zejména v genetickém programování.

### Redukce reprezentačního prostoru

Špatně zvolené kódování nutí algoritmus prověřovat miliardy zjevně nesmyslných stavů. Správnou volbou reprezentace dokážeme matematicky eliminovat neplatná řešení a drasticky zmenšit prohledávaný prostor ještě před spuštěním výpočtu.

Ukažme si to na problému rozmístění 8 šachových dam. Pokud pozice popíšeme pomocí *Kartézských souřadnic* jako libovolné dvojice $s_i = (x_i, y_i)$, prostor obsahuje $64^8 \approx 2.81 \times 10^{14}$ stavů. Když zavedeme pravidlo *jedna dáma na sloupec*, kódování se smrskne na vektor řádkových pozic $(y_1, \dots, y_8)$ a prostor klesne na $8^8 = 16\,777\,216$ možností. Pokud navíc zakážeme, aby dámy sdílely stejný řádek, reprezentace se redukuje na čistou *permutaci*, kde prohledáváme pouhých $8! = 40\,320$ stavů.

### Práce s omezeními (Constraint Handling)

Reálné problémy jsou plné limitů – nosnost mostu, kapacita skladu, rozpočet. Pokud algoritmus během hledání vygeneruje neplatné řešení, musíme mít strategii, jak na to reagovat.

Při **reject strategii** algoritmus hraje na jistotu: generuje a udržuje striktně pouze přípustná řešení a jakýkoli neplatný krok okamžitě zahazuje. **Penalizační strategie** naopak hranice otevírá; dovoluje algoritmu procházet i nepřípustným prostorem, ale účelovou funkci zatíží penalizací. Váhy penalizací bývají vysoké, aby byl algoritmus přirozeně tlačen zpět do legální zóny. **Opravné strategie** volí aktivní přístup: pokud vznikne nepřípustné řešení, aplikuje se specifická opravná heuristika, která ho transformuje zpět na platný stav.

### Účelová funkce (Objective Function)

Aby mohl algoritmus úspěšně navigovat prostorem, potřebuje zpětnou vazbu. Účelová funkce přiřazuje každému stavu číselné hodnocení, které říká, jak blízko jsme k ideálu.

Pokud je matematické kritérium **samospustitelné (self-sufficient)**, lze ho přímo použít k optimalizaci (např. minimalizace celkové ujeté vzdálenosti v kilometrech). Někdy je ale původní zadání typu True/False (např. u splnitelnosti logických formulí SAT). V takovém stavu by algoritmus bloudil potmě, protože neví, zda je k řešení blízko, nebo daleko. Funkce se proto transformuje na **naváděcí (guiding)** – u $k$-SAT formule se maximalizuje *počet aktuálně splněných klauzulí*, což dává lokálnímu prohledávání jemný a čitelný gradient.

---

## Lokální prohledávání

Pokud nemáme mapu celého stavového prostoru, nezbývá než se chovat jako horolezec v husté mlze. Lokální prohledávání začíná v jednom kompletním počátečním bodě a krok za krokem se posouvá do bezprostředního okolí, dokud nenarazí na vrchol, ze kterého už žádná cesta nevede výš.

Základním kamenem je **Hill Climbing (Horolezecký algoritmus)**. Pracuje vždy nad jedním kompletním řešením, které se v rekurzivním cyklu pokouší nahradit lepším sousedním stavem:

```pascal
s := s0; // Vygenerování počátečního řešení s0
while stopping condition not satisfied do
    generate(N(s)); // Vygenerování kandidátních sousedů z okolí N(s)
    if no better neighbor exist then 
        terminate; // Dosaženo lokální optimum
    s := s'; // Náhrada aktuálního stavu lepším sousedem s' ∈ N(s)
end while
```

### Strategie výběru souseda

Když algoritmus vygeneruje okolí aktuálního stavu, musí se rozhodnout, jakého konkrétního souseda zvolí pro následující krok. 

* **Best improvement (Steepest descent):** Kompletně prohledá celé dostupné okolí a vybere souseda, který přináší největší možné zlepšení. Pro rozsáhlá okolí je tato strategie časově extrémně drahá.
* **First improvement:** Prochází okolí sekvenčně a okamžitě vybere prvního nalezeného souseda, který je lepší než aktuální stav. Výrazně tím urychluje výpočetní čas jedné iterace.
* **Random selection:** Náhodně vybere menší vzorek sousedů a z nich zvolí ten stav, který aktuální situaci nejvíce vylepší.

### Koncepty okolí (Neighborhood) a lokální optimum

Abychom mohli definovat sousedství, musíme stanovit, co v daném prostoru znamená „být blízko“. Funkce okolí $N$ každému řešení přiřazuje množinu sousedních řešení na základě přechodového operátoru (*move operator*):

$$N(s) = \{s' \in S \mid d(s', s) \le \epsilon\}$$

Pokud je vzdálenost rovna jedné, znamená to změnu hodnoty právě jedné proměnné. Celý prostor si lze představit jako graf, kde uzly jsou kompletní řešení a hrany představují povolené mikrozměny. Řešení je **lokálním optimem** tehdy, pokud v jeho bezprostředním okolí neexistuje žádný lepší soused. Pro minimalizační problém platí:

$$f(s) \le f(s') \quad \forall s' \in N(s)$$

U *permutačních problémů* (rozvrhování, trasování) definujeme okolí pomocí specifických operátorů. *Poziční okolí* využívá operátor vkládání (**insertion** – prvek vyjmu a vložím jinam). *Pořadové okolí* sází na prohození dvou prvků (**swap/exchange**) nebo obrácení pořadí celé podsekvence (**inversion**).

U *Problému obchodního cestujícího (TSP)* často měříme velikost okolí exaktně. Pro operátor $2$-distance (výměna dvou měst) je velikost kompletního okolí $\frac{n(n - 1)}{2}$. Pro populární operátor $2$-opt (odstranění dvou hran a křížové přepojení) je přesná velikost okolí rovna $\left[\frac{n(n - 1)}{2} - n\right]$.

#### Inkrementální vyhodnocování okolí

Kompletní přepočet účelové funkce od nuly pro každého kandidáta v okolí je výpočetní zabiják. Cílem moderních algoritmů je implementovat **inkrementální vyhodnocení** pomocí výpočtu pouhé diferenční změny $\Delta f$. U algoritmu 2-opt pro TSP tak namísto sčítání délky celé trasy spočítáme pouze rozdíl vzniklý odebráním dvou starých hran a přidáním dvou nových:

$$\Delta f = \text{dist}(A, E) + \text{dist}(C, D) - \text{dist}(A, D) - \text{dist}(C, E) \implies f(s') = f(s) + \Delta f$$

### Iterované lokální prohledávání (ILS)

Čistý algoritmus Hill Climbing má zásadní slabinu: uvízne na prvním malém kopečku (lokálním optimu) a dál se nedostane. Abychom našli skutečné globální velehorstvo, musíme umět z lokálních pastí vyskočit.

Můžeme zvolit strategii *Multistart*, kdy po dosažení optima zahodíme veškerou historii, vygenerujeme zcela nové náhodné řešení od nuly a začneme stoupat znovu. Mnohem chytřejší je však **Iterated Local Search (ILS)**. Tento algoritmus staví na historii: na nalezené lokální optimum aplikuje cílené strukturální narušení (**perturbaci**) a z tohoto modifikovaného stavu spustí nové lokální prohledávání. Perturbace musí být dostatečně velká, aby algoritmus vyskočil z lůžka přitažlivosti stávajícího optima, ale zároveň přiměřeně malá, aby se neztratila cenná informace z dosavadního hledání.

```pascal
s = s0; s* = local_search(s); // První lokální optimum
repeat
    s' = perturb(s*, search_history); // Strukturální narušení
    s'_* = local_search(s'); // Nové hledání z narušeného stavu
    s* = accept(s*, s'_*, search_memory); // Akceptační kritérium
until stopping_criteria;
```

---

## Metaheuristiky s jedním řešením

Zatímco základní lokální prohledávání striktně vyžaduje, aby byl každý krok zlepšující, pokročilé metaheuristiky s jedním řešením zavádějí kontrolovanou benevolenci. Za určitých podmínek dovolují udělat krok do horšího stavu, což jim otevírá cestu k úniku z lokálních pastí.

#### Simulované žíhání (Simulated Annealing - SA)
Stochastická metoda inspirovaná metalurgickým procesem ochlazování kovů. Zlepšující krok přijímá vždy. Horší krok akceptuje stochasticky s pravděpodobností:

$$P(\text{přijetí}) = e^{-\frac{\Delta f}{T}}$$

Míra zhoršení $\Delta f$ soupeří s globálním parametrem teploty $T$. Na začátku výpočtu je teplota vysoká, takže algoritmus divoce diversifikuje a snadno přijímá i špatná řešení. Postupným ochlazováním teplota konverguje k nule, pravděpodobnost akceptace chyb mizí a algoritmus přechází do fáze čisté intenzifikace (Hill Climbing).

#### Record-to-record travel (RRT)
Deterministický bratranec simulovaného žíhání. Algoritmus si udržuje hodnotu historicky nejlepšího nalezeného řešení (`RECORD`). Nově vygenerovaný soused $s'$ je bez milosti akceptován tehdy a jen tehdy, pokud jeho hodnota nezhorší stávající rekord o více než pevně stanovenou prahovou odchylku $D > 0$:

$$f(s') < \text{RECORD} + D$$

#### Algoritmus Velké potopy (Great Deluge - GD)
Metaheuristika postavená na analogii s horolezcem, kterému pod nohama stoupá hladina vody. Parametr `LEVEL` představuje neustále se zpřísňující strop. Každé nové řešení je přijato pouze tehdy, nachází-li se bezpečně pod touto hladinou ($f(s') < \text{LEVEL}$). Rychlost poklesu hladiny určuje kritický kompromis: rychlý pokles znamená bleskový výpočet, pomalý pokles přináší vysokou kvalitu za cenu dlouhého času.

#### Zakázané prohledávání (Tabu Search - TS)
Deterministická strategie, která k úniku z optim využívá krátkodobou paměť. V každé iteraci vybere nejlepšího možného souseda ze všich dostupných, i kdyby to znamenalo absolutní zhoršení stavu. Aby se předešlo okamžitému návratu zpět a zacyklení, zavedl se **Tabu list**. Jde o FIFO frontu, která ukládá atributy posledních provedených tahů (např. dvojice měst prohozené u TSP) a tyto kroky na několik iterací striktně zakáže. Pokud však zakázaný tah vede na řešení, které překonává dosavadní historické maximum, **ašpirační kritérium** tabu status ignoruje a krok povolí.

---

### Prohledávání s velmi velkým okolím (VLNS)

Pokud je definované okolí stavu příliš malé, algoritmus v něm snadno uvízne. Prohledávání s velmi velkým okolím (Very Large-Scale Neighborhood Search) záměrně pracuje s okolími, jejichž velikost roste exponenciálně, ale využívá chytré algoritmy, jak v tomto obrovském prostoru najít nejlepší krok bez prohledávání každého stavu.

V rámci **vyhazovacích řetězců (Ejection chains)** provádí algoritmus sekvenci lokálních oprav. První přesun odstraní aktuální defekt (porušení omezení), ale vyvolá vznik jiného defektu. Následující krok opraví ten sekundární, přičemž vyvolá terciární. Řetězec úspěšně končí v momentě, kdy se defekt zcela eliminuje bez vyvolání nového, což typicky tvoří uzavřený cyklus výměn.

**Large Neighborhood Search (LNS)** je postavena na principu destrukce (**Destroy**) a následné opravy (**Repair**). Algoritmus v každém kroku záměrně znehodnotí velkou část aktuálního stavu (např. 15 % trasy) a následně ji zrekonstruuje lepším způsobem. 

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

Při *destrukci* můžeme mazat náhodně (*Random removal*), cílit na uzly s nejhorším přínosem (*Worst-case removal*), nebo odebírat strukturálně podobné prvky (*Shaw removal*), které lze mezi sebou nejsnáze zaměnit. Při *opravě* se uplatňuje hladové vkládání (*Greedy insertion*) nebo pokročilé *Regret insertion*, které přednostně umisťuje prvky s nejvyšší hodnotou "lítosti" (rozdílem skóre mezi prvním a druhým nejlepším místem), pro které by později nemuselo zbýt legální místo.

**Variable Neighborhood Search (VNS)** staví na faktu, že lokální optimum vůči jedné struktuře okolí ($N_1$) nemusí být optimem pro strukturu jinou ($N_2$). Čistě deterministická verze **VND (Variable Neighborhood Descent)** prohledává řadu okolí sekvenčně; při úspěchu resetuje index na $N_1$, při selhání postupuje do širší struktury ($N_{l+1}$). **Basic VNS** tuto myšlenku doplňuje o stochastickou fázi „třesení“ (**shaking**), která provádí náhodné skoky za účelem úniku z lůžka přitažlivosti stávajícího lokálního minima.

---

## Populační metaheuristiky

Zatímco metody s jedním řešením vysílají do prostoru osamoceného průzkumníka, populační metaheuristiky pracují s celou množinou (populací) kandidátů současně. Díky tomu masivně excelují v **diversifikaci** stavového prostoru a drasticky snižují riziko předčasného uvíznutí.

Podle charakteru vnitřní interakce dělíme populační algoritmy do dvou hlavních kategorií:

* **Založené na evoluci (Evolution based):** Jedinci jsou přímo vybráni a reprodukováni pomocí biologicky inspirovaných operátorů (křížení, mutace). Dochází k přímému přenosu a míchání genetické informace konkrétních jedinců (např. Genetické algoritmy, Evoluční strategie).
* **Založené na sdílené paměti (Blackboard based):** Jedinci spolu napřímo nerodí potomky. Místo toho svými vlastnostmi přispívají do společně sdíleného prostoru (tzv. tabule). Nová generace kandidátů se v dalším kroku generuje čistě na základě aktuálního stavu této sdílené paměti (např. feromonová matice u ACO).

Kvalita výpočtu silně závisí na inicializaci populace. *Náhodné generování* je jednoduché, ale pro silně omezené problémy složité na nalezení platných stavů. *Sekvenční či paralelní diversifikace* umisťuje jedince cíleně tak, aby maximalizovala jejich vzájemnou vzdálenost a pokryla co největší plochu. *Heuristická inicializace* předvyplní populaci výsledky rychlých lokálních algoritmů, což urychlí start, ale dramaticky zvyšuje riziko bleskové ztráty diverzity. Výpočet se ukončuje buď staticky (časový limit, fixní počet generací), nebo adaptivně (pokud se nejlepší jedinec po $X$ generací nezlepšil).

---

## Evoluční algoritmy (EA)

Evoluční algoritmy modelují optimalizační proces jako digitální simulaci Darwinovy teorie o přírodním výběru. Základní terminologie plně kopíruje biologii: kompletní zakódované řešení je *jedinec (chromozom)*, konkrétní proměnná představuje *gen*, její hodnota je *alela* a kvalita řešení se měří funkcí *zdatnosti (fitness)*, která se v kontextu EA typicky maximalizuje.

### Hlavní větve evolučních výpočtů

**Genetické algoritmy (GA)** byly původně navrženy pro binární řetězce. Klidou absolutní důraz na operátor **křížení** nad dvěma rodiči, zatímco mutace slouží pouze jako nouzová pojistka pro udržení diverzity. Pracují s fixními pravděpodobnostmi křížení $p_c$ a mutace $p_m$.

**Evoluční strategie (ES)** vznikly pro kontinuální optimalizaci a vektory reálných čísel. Křížení je zde vzácné, hlavním evolučním motorem je **mutace**. Používají striktní elitářské nahrazování a specifické značení populací, kde $\mu$ je počet rodičů a $\lambda$ počet potomků. U strategií $(\mu + \lambda)$-ES se nová generace vybírá ze spojené a setříděné množiny rodičů i potomků dohromady. U verzí $(\mu, \lambda)$-ES rodiče bezpodmínečně vymírají a výběr probíhá striktně pouze z řad potomků, což je ideální pro sledování měnících se cílů v dynamickém prostředí.

**Genetické programování (GP)** posouvá koncept o úroveň výš. Jedinci zde nejsou datové vektory, ale přímo **vykonatelné počítačové programy** reprezentované stromovými strukturami. Typickým příkladem je *symbolická regrese*, kde je cílem nalézt matematický vzorec, který nejlépe odpovídá naměřeným datům, přičemž fitness jedince je definována jako minimalizace sumy čtverců odchylek.

### Strategie výběru rodičů

Selekční tlak určuje, jakým způsobem dáváme přednost silnějším jedincům před slabšími při výběru partnerů pro páření.

Při **ruletovém výběru** je pravděpodobnost zvolení jedince přímo úměrná jeho fitness vůči sumě fitness celé populace ($p_i = f_i / \sum f_j$). Trpí však zásadní vadou: objeví-li se na začátku jeden dominantní jedinec, ruleta ho bude volit neustále, což vede k rychlému zaplavení populace jeho klony a k předčasné konvergenci. **Stochastické univerzální vzorkování (SUS)** tento neduh eliminuje – na pomyslné kolo umístí všechny ukazatele rovnoměrně a jediným otočením vybere celou sadu rodičů naráz, čímž dává spravedlivou šanci i slabším.

**Turnajový výběr** náhodně vytáhne $k$ jedinců, kteří mezi sebou porovnají fitness, a absolutní vítěz se stává rodičem. Celý turnaj se nezávisle opakuje $\mu$-krát, přičemž velikost turnaje $k$ přímo řídí sílu selekčního tlaku. **Výběr podle pořadí (Rank-based)** nejprve jedince seřadí podle výkonnosti a pravděpodobnost počítá čistě z jejich pořadového indexu, čímž dokonale vyhlazuje extrémní rozdíly v absolutních hodnotách fitness.

### Reprodukční a nahrazovací strategie

Operátor **mutace** představuje malé stochastické narušení jednoho jedince. Pravděpodobnost se nastavuje nízko ($p_m \approx 1/k$, kde $k$ je počet genů), což v průměru znamená změnu jednoho genu na chromozom (invertování bitu, změna znaku, či permutační swap). Operátor **křížení** kombinuje genetický kód dvou rodičů s vysokou pravděpodobností ($p_c \in [0.45, 0.95]$). U lineárních řetězců používáme *jednobodové* (prohození segmentů za dělicím bodem), *vícebodové* či *uniformní křížení* (o každém genu se rozhoduje náhodně padesátiprocentní šancí). U permutací běžné křížení selhává, proto nastupují pokročilé techniky jako **Pořadové křížení (OX)**, které fixuje vybraný úsek od prvního rodiče a zbytek pozic bez duplicit dosype z rodiče druhého.

Při výběru přeživších do další generace volíme mezi **generačním nahrazováním** (potomci kompletně a bezpodmínečně vymažou starou populaci rodičů) a **ustáleným stavem** (v každém kroku vznikne jen minimum potomků, kteří nahradí nejhorší jedince). Pro zajištění stability se masivně využívá **elitismus**, který garantuje, že historicky nejlepší jedinci automaticky postupují do další generace bez rizika, že o ně křížením přijdeme.

<img alt="img.png" src="img/metody_umele_inteligence/evolalg.png" width="400"/>

---

## Inteligence hejna

Samostatný mravenec nebo včela disponují minimální inteligencí a omezeným vnímáním. Jako společenství však vykazují emergentní chování vyšší úrovně. Inteligence hejna (Swarm Intelligence) využívá armádu jednoduchých, decentralizovaných agentů, kteří komunikují výhradně lokálně a nepřímo přes úpravy okolního prostředí – tento jev se nazývá **stigmergie**.

**Optimalizace mravenčí kolonou (Ant Colony Optimization - ACO)** uměle kopíruje chování mravenců hledajících nejkratší trasu mezi mraveništěm a potravou pomocí stopování chemického feromonu ($\tau$). Jelikož se feromon v čase přirozeně odpařuje, na kratší cestě se mravenci otočí rychleji. Stopa se tam vrství intenzivněji, což zpětně přitahuje stále více mravenců.

V algoritmu provádíme s feromonem dvě klíčové operace:
* **Odpařování (Evaporation):** Provádí se plošně na všech hranách optimalizačního grafu ($\tau_{ij} = (1 - \rho)\tau_{ij}$, kde $\rho \in [0, 1]$), což uvolňuje paměť a umožňuje únik z lokálních optim.
* **Posílení (Reinforcement):** Přidání feromonu na hrany. Nejefektivnější je *off-line update* na konci generace, kdy feromon přisypeme výhradně na hrany, které tvoří historicky nejlepší nalezenou trasu ($\tau_{ij} = \tau_{ij} + \Delta$).

Při aplikaci na Problém obchodního cestujícího (TSP) si mravenec stojící v uzlu $i$ vybírá následující město $j$ na základě kombinace feromonové stopy $\tau$ a lokální viditelnosti $\eta_{ij} = 1/d_{ij}$ (převrácená hodnota fyzické vzdálenosti):

$$p_{ij} = \frac{\tau_{ij}^\alpha \times \eta_{ij}^\beta}{\sum_{k \in S} \tau_{ik}^\alpha \times \eta_{ik}^\beta}$$

Explicitní parametry $\alpha$ a $\beta$ definují relativní váhu obou složek. Nastavíme-li $\alpha = 0$, algoritmus degraduje na čistě stochastické hladové prohledávání podle geografické blízkosti.

---

## Plánování a reprezentace problému

Klasické prohledávání stavového prostoru je slepé – agent zná jen aktuální uzel a možnosti přechodu. Automatické plánování dává agentovi schopnost uvažovat o budoucnosti na základě explicitního doménového modelu. Umožňuje doménově nezávislým algoritmům najít optimální organizaci akcí pro dosažení vytyčeného cíle.

Formálně je prostředí modelováno jako transformační systém $\Sigma = (S, A, E)$, kde $S$ je množina stavů, $A$ reprezentuje akce plně pod kontrolou plánovače a $E$ zastupuje vnější neovlivnitelné události. Změnu stavu řídí přechodová funkce $\gamma: S \times (A \cup E) \rightarrow 2^S$.

### Reprezentace stavů a akcí

Aby mohl plánovač logicky odvozovat, definuje stav jako konečnou množinu **plně instanciovaných atomů**. Ty se dělí na *fluenty*, jejichž pravdivost se akcemi mění (např. `at(robot, location)`), a *rigidní atomy*, které jsou neměnné (např. `adjacent(loc1, loc2)`). Při vyhodnocování platí **Předpoklad uzavřeného světa (CWA)** – cokoli není v popisu stavu explicitně uvedeno, je považováno za nepravdivé.

**Plánovací operátor ($o$)** je univerzální parametrická šablona složená z unikátního názvu, množiny předpodmínek (`precond`) a množiny efektů (`effects`). Jakmile proměnné v operátoru nahradíme konkrétními objekty z domény, vzniká plně instanciovaná **akce ($a$)**.

<img alt="img.png" src="img/metody_umele_inteligence/operators-predicates.png" width="500"/>

Akce je aplikovatelná na stav $s$ tehdy, jsou-li splněny její pozitivní předpodmínky a zároveň neplatí žádná z negativních ($\text{precond}^+(a) \subseteq s \ \land \ \text{precond}^-(a) \cap s = \emptyset$). Výsledný stav po aplikaci akce definujeme jako odebrání negativních a přidání pozitivních efektů:

$$\gamma(s, a) = (s \setminus \text{effects}^-(a)) \cup \text{effects}^+(a)$$

Plánovací problém $P = (O, s_0, g)$ zadává doménový model $O$, počáteční konfiguraci $s_0$ a cílové literály $g$. Výsledný **plán $\pi$** je uspořádaná sekvence akcí, jejíž sekvenční aplikace na počáteční stav zaručeně dovede agenta do cíle ($\gamma(s_0, \pi) \in S_g$).

### Jazyk PDDL (Planning Domain Definition Language)

PDDL je standardizovaný formální jazyk, který striktně odděluje fyzikální zákony světa od konkrétního zadání úlohy do dvou samostatných souborů. **Domain File** obsahuje definice typů, predikátů a univerzálních akcí. **Problem File** specifikuje reálné objekty, výchozí stav světa a cílové podmínky.

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

Při plánování se stavovým prostorem představují uzly prohledávacího stromu konkrétní stavy světa a hrany reprezentují přechody mezi nimi realizované aplikací akcí. Cílem je nalézt propojující dráhu.

### 1. Dopředné plánování (Forward Planning / Progression)

Algoritmus začíná v reálném počátečním stavu $s_0$ a rozbaluje strom směrem dopředu k cíli aplikací všech akcí, které jsou v daném uzlu legálně proveditelné. Je ze své podstaty korektní a úplný. Hlavním úskalím je enormní **faktor větvení** – počáteční stav může generovat tisíce akcí, které s finálním cílem vůbec nesouvisí. V moderní praxi se proto dopředné prohledávání kombinuje s algoritmem $A^*$ a pokročilými doménově nezávislými heuristikami, které hlídají a prořezávají slepé větve.

<img alt="img.png" src="img/metody_umele_inteligence/forward.png" width="400"/>

### 2. Zpětné plánování (Backward Planning / Regression)

Zpětné plánování volí opačnou strategii: startuje od popisu cíle $g$ a postupuje pozpátku k počátečnímu stavu $s_0$ vytvářením podcílů. V každém kroku vybere pouze **relevantní akci**, která prokazatelně přispívá k naplnění cíle ($g \cap \text{effects}(a) \neq \emptyset$) a není s ním v přímém konfliktu. Výsledkem je **regresní množina**, která definuje nové podmínky, jež musely platit bezprostředně před aplikací dané akce:

$$\gamma^{-1}(g, a) = (g \setminus \text{effects}(a)) \cup \text{precond}(a)$$

Zpětný přístup mívá podstatně menší faktor větvení, protože ignoruje irelevantní akce, ale vyžaduje striktní implementaci detekce cyklů pro zamezení nekonečného bloudění.

<img alt="img_1.png" src="img/metody_umele_inteligence/backward.png" width="400"/>

### 3. Částečně instanciované zpětné plánování (Lifted Backward Planning)

Tato pokročilá technika odstraňuje zbytečné větvení zpětného plánování tím, že odmítá předčasné dosazování konkrétních objektů. Parametry akcí ponechává jako volné proměnné tak dlouho, dokud to není bezpodmínečně nutné. Využívá **MGU (Most General Unifier)** – nejobecnější unifikátor, což je minimální nutná substituce proměnných, která elegantně sjednotí požadavek v cíli s efektem operátoru. Tímto způsobem dokáže algoritmus v jediném kroku pokrýt desítky potenciálních lokací bez nutnosti větvení stromu, výměnou za výpočetně náročnější správu unifikačních omezení.

---

## Práce s neurčitostí a Bayesovské sítě

Čistá logika v reálném světě naráží na rigiditu – senzory šumí, akce mají nepředvídatelné výsledky a informace jsou neúplné. Teorie pravděpodobnosti dává umělé inteligenci pevný matematický rámec pro exaktní uvažování, kvantifikaci rizika a optimální rozhodování za neurčitosti.

Základní pilíře pravděpodobnostního světa tvoří tři koncepty:
* **Sdružená distribuce:** Obří tabulka pokrývající pravděpodobnosti všech představitelných kombinací stavů. Dokáže odpovědět na jakýkoli dotaz, ale její velikost roste exponenciálně ($2^n$).
* **Marginalizace (Summing out):** Proces, kterým odfiltrujeme nezajímavé (skryté) proměnné sečtením jejich pravděpodobností: $P(Y) = \sum_{z \in Z} P(Y, z)$.
* **Podmíněná pravděpodobnost a Bayesův teorém:** Umožňuje matematicky otočit směr podmínění, což je klíčové pro diagnostické systémy odhadující skrytou příčinu z pozorovaného symptomu:

$$P(\text{příčina} \mid \text{efekt}) = \frac{P(\text{efekt} \mid \text{příčina}) \times P(\text{příčina})}{P(\text{efekt})}$$

### Bayesovské sítě

Plná sdružená distribuce je kvůli své exponenciální velikosti v praxi nepoužitelná. Bayesovská síť řeší tento problém tím, že do struktury grafu explicitně integruje vztahy podmíněné nezávislosti mezi proměnnými.

Jedná se o **Orientovaný acyklický graf (DAG)**. Uzly představují náhodné proměnné a hrany vyjadřují přímou závislost. Každý uzel obsahuje vlastní **Tabulku podmíněné pravděpodobnosti (CPT)**, která definuje distribuci vůči kombinacím hodnot jeho přímých rodičů. Hodnotu jakéhokoli globálního stavu lze pak spočítat jako pouhý součin těchto lokálních tabulek:

$$P(x_1, \dots, x_n) = \prod_{i=1}^{n} P(x_i \mid \text{parents}(X_i))$$

Díky této dekompozici klesá paměťová náročnost z původních $2^n$ na pouhých $n \cdot 2^k$ parametrů (kde $k$ je maximální počet rodičů uzlu). Při stavbě sítě se striktně doporučuje volit **kauzální směr** (od příčin k efektům), který přirozeně vytváří řídké grafy. Diagnostické řazení (od efektů k příčinám) generuje přebujelé, husté sítě, které ztrácejí veškeré paměťové výhody.

---

## Exaktní a aproximační odvozování

Odvozování (Inference) v Bayesovských sítích spočítá výslednou podmíněnou distribuci dotazované proměnné na základě pozorované pevné evidence, přičemž všechny ostatní proměnné odmaže pomocí marginalizace: $P(X \mid e) = \alpha \sum_{y} P(X, e, y)$.

### 1. Exaktní odvozování

Cílem exaktních metod je spočítat matematicky dokonale přesný výsledek. *Odvozování výčtem* prochází strom možných stavů shora dolů, což je extrémně neefektivní, protože opakovaně počítá totožné podvýrazy. Mnohem chytřejší **Eliminace proměnných** funguje na principu dynamického programování. Chápe pravděpodobnostní tabulky jako faktory (matice) a výpočet provádí zdola nahoru pomocí dvou operací: *bodového součinu* souvisejících faktorů a následného *vysčítání* (eliminace) skrytých proměnných. Pro sítě s topologií stromu (poly-trees) je složitost lineární $O(n \cdot d^k)$, avšak pro obecné vícenásobně propojené sítě je tento problém **NP-těžký** (až #P-těžký), přičemž náročnost diktuje velikost největší matice vytvořené během eliminace.

### 2. Aproximační odvozování

Pro obří, hustě propojené reálné sítě je exaktní výpočet matematicky nezvládnutelný. Nastupují proto stochastické **Monte Carlo** algoritmy, které generují tisíce náhodných simulací (vzorků) a výsledek odhadují na základě statistické četnosti.

* **Přímé vzorkování:** Generuje vzorky od kořenů k listům slepě podle pravděpodobností v CPT. Neumí však nativně zohlednit pevnou evidenci.
* **Zamítavé vzorkování (Rejection sampling):** Generuje vzorky přímým vzorkováním, ale jakmile je jakýkoli vzorek v rozporu s pozorovanou evidencí $e$, okamžitě ho **zahodí**. Pokud je evidence vzácná (má nízkou pravděpodobnost), algoritmus zlikviduje 99 % vzorků a jeho efektivita drasticky kolabuje.
* **Váhová věrohodnost (Likelihood weighting):** Elegantní řešení, které zabraňuje plýtvání. Proměnné obsažené v evidenci $e$ **zafixuje napevno** a náhodně vzorkuje pouze volné proměnné. Aby byla distribuce matematicky korektní, každému vzorku přiřadí **váhu $w$**, jež odpovídá součinu podmíněných pravděpodobností všech fixovaných proměnných v momentě průchodu. Výsledná distribuce se následně normalizuje přes sumu těchto vah.

---

## Čas a neurčitost

Svět se neustále vyvíjí a statické modely nestačí. Při uvažování v čase pracuje agent v částečně pozorovatelném prostředí, kde si musí udržovat aktuální stav víry (*belief state*) na základě neustále přicházejícího proudu senzorických měření v diskrétních časových řezech.

Aby byl model výpočetně zvládnutelný, zavádí se dva klíčové předpoklady:
* **Markovský předpoklad (1. řádu):** Aktuální skrytý stav systému závisí striktně pouze na stavu bezprostředně předcházejícím ($P(X_t \mid X_{0:t-1}) = P(X_t \mid X_{t-1})$). Historie dál do minulosti nemá na budoucnost vliv. Pokud se pravidla přechodů v čase nemění, mluvíme o *stacionárním procesu*.
* **Senzorický Markovský předpoklad:** Aktuální pozorování ze senzorů $E_t$ závisí výhradně na současném skrytém stavu světa $X_t$ ($P(E_t \mid X_{0:t}) = P(E_t \mid X_t)$).

Celý časový vývoj lze popsat dynamickou Bayesovskou sítí jako součin:

$$P(X_{0:t}, E_{1:t}) = P(X_0) \prod_{i=1}^{t} P(X_i \mid X_{i-1}) P(E_i \mid X_i)$$

Nad tímto modelem rekurzivně řešíme čtyři základní inferenční úlohy. **Filtrace** průběžně počítá aktuální stav víry ze všech dosavadních pozorování ($P(X_t \mid e_{1:t})$) pomocí posílání zpráv směrem dopředu. **Predikce** odhaduje budoucí stav o $k$ kroků dopředu bez přísunu nových dat. **Vyhlazování** se dívá do minulosti a s využitím obousměrného průchodu (*forward-backward*) zpřesňuje odhad historického stavu na základě novějších dat ($P(X_k \mid e_{1:t})$ pro $k < t$). Poslední úlohou je nalezení **nejpravděpodobnějšího vysvětlení**, tedy ucelené sekvence skrytých stavů, která s nejvyšší pravděpodobností vygenerovala daná pozorování (řeší se Viterbiho algoritmem).

---

## Teorie užitku a rozhodování

Zatímco teorie pravděpodobnosti dává agentovi nástroj, jak popsat stav světa a předvídat budoucnost, teorie užitku definuje jeho vnitřní touhy, preference a cíle. Jejich spojením vzniká **teorie rozhodování**, která dává návod k racionálnímu chování.

Ústředním pilířem je **Princip maximalizace očekávaného užitku (MEU)**. Každému stavu přiřadí užitková funkce $U(s)$ reálné číslo vyjadřující jeho atraktivitu. Očekávaný užitek akce $a$ je dán váženým průměrem užitků všech jejích možných výsledných stavů:

$$EU(a \mid e) = \sum_{s} P(\text{Result}(a) = s \mid a, e) U(s)$$

Racionální agent pak bezpodmínečně volí takovou akci, která tento očekávaný užitek maximalizuje ($a = \arg\max_a EU(a \mid e)$).

V reálném světě uvažujeme *užitek peněz*, který neroste lineárně, ale má konkávní charakter – přírůstek bohatství přináší klesající marginální užitek, což vysvětluje přirozený lidský odpor k riziku (*risk aversion*). Pokud jsou stavy popsány více kritérii (cena, bezpečnost, rychlost), využíváme koncepty *striktní dominance* k vyřazení prokazatelně nejhorších variant. Pro grafickou vizualizaci a výpočet těchto komplexních problémů slouží **Rozhodovací sítě (Influence Diagrams)**, které klasické Bayesovské sítě rozšiřují o *rozhodovací uzly* (obdélníky znamenající přímou volbu akce) a *užitkové uzly* (kosočtverce reprezentující užitkovou funkci).

---

## Markovský rozhodovací proces (MDP)

MDP formalizuje sekvenční rozhodovací problém v plně pozorovatelném, stochastickém prostředí, kde výsledky akcí závisí na náhodě a agent je motivován průběžnými odměnami či tresty $R(s)$. Cílem není najít jeden fixní plán akcí (který by kvůli náhodě mohl selhat), ale optimální **strategii $\pi(s)$**, která doporučí nejlepší akci pro každý představitelný stav.

Při nekonečném časovém horizontu se pro výpočet dlouhodobého přínosu používá **diskontovaná odměna** s faktorem $\gamma \in (0, 1)$, která modeluje preferenci okamžitých odměn před budoucími a matematicky zaručuje konvergenci řady:

$$U([s_0, s_1, s_2, \dots]) = \sum_{t=0}^{\infty} \gamma^t R(s_t) \le \frac{R_{max}}{1 - \gamma}$$

Základním teoretickým pilířem pro výpočet hodnot užitků je **Bellmanova rovnice**. Ta říká, že užitek stavu je roven okamžité odměně plus očekávanému diskontovanému užitku následného stavu při zvolení optimální akce:

$$U(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U(s')$$

### Algoritmy pro řešení MDP

Jedná se o systém nelineárních rovnic (kvůli operátoru max), které nelze řešit běžnou soustavou rovnic. Využíváme dva dedikované algoritmy.

#### 1. Iterace hodnot (Value Iteration)
Iterativní numerický algoritmus. Začne se s libovolnými (nulovými) odhady užitků pro všechny stavy a v každém kroku se provede tzv. **Bellmanův update**:

$$U_{i+1}(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U_i(s')$$

Tento update vykazuje matematickou vlastnost *kontrakce* (s každým krokem zmenšuje vzdálenost k cíli s faktorem $\gamma$), což garantuje konvergenci k jedinému optimálnímu řešení. Iterace končí, jakmile je maximální změna $\delta < \epsilon(1-\gamma)/\gamma$. V praxi doporučená strategie $\pi_i$ zkonverguje k optimální verzi mnohem dříve, než plně dokongvergují samotné numerické hodnoty užitků $U_i$.

#### 2. Iterace strategie (Policy Iteration)
Tento algoritmus staví na myšlence, že pro nalezení optimálního chování není nutné znát zcela přesné hodnoty užitků. Začíná s náhodnou strategií a střídá dva kroky:
1.  **Evaluace strategie (Policy evaluation):** Pro aktuální fixní strategii $\pi_i$ se spočítají přesné užitky stavů $U^{\pi_i}$. Protože akce jsou pevně dané strategií, Bellmanova rovnice ztrácí operátor max a zjednoduší se na *systém lineárních rovnic* s kubickou složitostí $O(n^3)$, což lze pro velké prostory aproximovat sérií zjednodušených hodnotových kroků:
$$U^{\pi_i}(s) = R(s) + \gamma \sum_{s'} P(s' \mid s, \pi_i(s)) U^{\pi_i}(s')$$
2.  **Zlepšení strategie (Policy improvement):** Na základě nově spočtených užitků se hladově aktualizuje doporučená akce pro každý stav:
$$\pi_{i+1}(s) = \arg\max_{a \in A(s)} \sum_{s'} P(s' \mid s, a) U^{\pi_i}(s')$$

Algoritmus končí, jakmile v kroku zlepšení nedojde k žádné změně. Jelikož je počet strategií konečný a každá iterace přináší striktní strukturální zlepšení, algoritmus garantovaně konverguje a v praxi bývá často podstatně rychlejší než čistá iterace hodnot.

<img alt="img.png" src="img/metody_umele_inteligence/markov.png" width="400"/>

---

## Robotika a plánování pohybu

Robotika studuje fyzikální agenty provádějící úkoly manipulací s reálným světem. Základním problémem plánování pohybu (Motion Planning) je nalézt spojitou sekvenci konfigurací, která bezpečně a bezkolizně převede robota z počátečního stavu $q_I$ do cíle $q_G$. Autonomní chování se dělí do tří integrovaných úrovní: *Plánování úloh* rozhoduje o sekvencích vysokoúrovňových akcí, *Plánování pohybu* hledá konkrétní geometrickou dráhu a *Řízení* generuje nízkoúrovňové příkazy (napětí) pro motory.

### Konfigurační prostor (C-space)

Počítat kolize pro složité reálné těleso v trojrozměrném *Pracovním prostoru (Workspace)* plném překážek je výpočetně neúnosné. Konfigurační prostor přináší geniální geometrickou fintu: transformuje celou, jakkoli složitou strukturu robota na **jeden jediný bod**. Počet dimenzí $C$-prostoru odpovídá přesně počtu stupňů volnosti (DoF) robota (např. pozice a orientace mobilního robota $(x, y, \theta)$ tvoří 3D prostor).

Prostor se dělí na $C_{obs}$ (množina konfigurací, kde robot koliduje s překážkou nebo sám se sebou) a bezpečný $C_{free}$ ($C_{free} = C \setminus C_{obs}$). Přepočet mezi světy zajišťuje *Dopředná kinematika* (klouby $\rightarrow$ pozice v prostoru) a *Inverzní kinematika* (pozice v prostoru $\rightarrow$ úhly kloubů). V praxi se explicitní tvary překážek v $C_{obs}$ nepočítají; prostor se pouze **sonduje** – vygenerovaný bod se pomocí dopředné kinematiky otestuje v pracovním prostoru přes rychlý "black-box" kolizní detektor.

<img alt="img.png" src="img/metody_umele_inteligence/config vs workspace.png" width="400"/>

---

### Přístupy k plánování pohybu

Spojitý prostor nelze prohledávat napřímo, proto musíme provést jeho diskretizaci a převést ho na graf, který následně prohledáme standardními algoritmy typu $A^*$.

### 1. Kombinatorické (deterministické) přístupy
Diskretizují volný prostor $C_{free}$ exaktními geometrickými či algebraickými metodami.
* **Graf viditelnosti (Visibility graph):** Spojuje úsečkami start, cíl a všechny vrcholy polygonálních překážek. Vždy garantuje nalezení **nejkratší možné cesty**, ale cesta vede nebezpečně těsně kolem hran překážek (nulová bezpečnostní rezerva). Výpočetně kolabuje ve vyšších dimenzích.
* **Voronoiův diagram:** Generuje cestovní mapu složenou z bodů, které mají přesně stejnou vzdálenost od nejbližších překážek. **Maximalizuje bezpečnostní odstup** (*clearance*), ale cesty nejsou nejkratší a jsou citlivé na sebemenší pohyb překážek.
* **Dekompozice na buňky (Cell Decomposition):** Rozděluje prostor na konečný počet nepřekrývajících se legálních oblastí. *Exaktní dekompozice* řeší svislé řezy v místech geometrických vrcholů. *Aproximační dekompozice* (např. Quadtree/Octree) rekurzivně dělí prostor na pravidelné kostky; smíšené kostky seká hlouběji, dokud nenajde čistou cestu nebo nenarazí na limit rozlišení.
* **Potenciálová pole (Potential Field Methods):** Pohyb je řízen virtuálními silami – cíl generuje atraktivní pole (přitahuje), překážky repulzivní pole (odtlačují). Robot se pohybuje ve směru výsledného gradientu. Metoda je rychlá, ale hrozí uvíznutí v **lokálních minimech**, kde se síly navzájem vyruší před dosažením cíle.

<img alt="img.png" src="img/metody_umele_inteligence/cell-decomp.png" width="400"/>

### 2. Pravděpodobnostní přístupy (Sampling-based)
Místo drahé exaktní konstrukce překážek náhodně vzorkují konfigurace v $C$-prostoru a testují je kolizním detektorem. Poskytují **pravděpodobnostní úplnost** (s rostoucím počtem vzorků pravděpodobnost nalezení existujícího řešení konverguje k 1). Jsou vysoce úspěšné ve vysokých dimenzích (4 a více DoF), ale jejich hlavní slabinou je prohledávání **úzkých průchodů** (*narrow passages*).

* **Probabilistická cestovní mapa (PRM) — Multi-query strategie:** Navržena pro statická prostředí, kde se plánuje opakovaně (vytvoří se jedna robustní mapa a nad ní se provádějí dotazy).
    1.  *Fáze učení:* Vygeneruje se $n$ náhodných bodů v $C_{free}$ a lokální plánovač se pokusí propojit blízké uzly v definovaném poloměru $\rho$ do grafu. Klasické PRM vytvoří vazbu pouze tehdy, pokud kandidáti dosud neleží ve stejné komponentě souvislosti (šetří se drahé kolizní testy).
    2.  *Fáze dotazu:* Startovní a cílová konfigurace se lokálním plánovačem připojí k hotové mapě a cesta se vyhledá pomocí klasického grafového vyhledávání ($A^*$, Dijkstra).
* **Rychle se rozrůstající náhodné stromy (RRT) — Single-query strategie:** Konstruuje nový graf (strom) pro každou úlohu zvlášť, přičemž se rozrůstá z počáteční konfigurace $q_{init}$ přímo směrem do volného prostoru (velmi rychle expanduje do neprozkoumaných oblastí). Je ideální pro dynamická prostředí.
    1.  Počáteční stav $q_{init}$ se stane kořenem stromu.
    2.  Náhodně se vygeneruje konfigurace $q_{new}$ v $C_{free}$.
    3.  Ve stromu se vyhledá uzel $q_{near}$, který je geometricky nejblíže k $q_{new}$.
    4.  Strom se rozšíří z bodu $q_{near}$ malým krokem směrem k $q_{new}$ (vzniká nový platný uzel, ověřený kolizním detektorem).
    5.  Proces se opakuje, dokud se větev stromu nedostane do dostatečné blízkosti cíle. Poskytuje přípustné řešení, ale neoptimální (dráha bývá dlouhá a klikatá).
