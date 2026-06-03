# Metody umělé inteligence a robotiky

<details>
<summary>Prohledávání stavového prostoru</summary>

* **Prohledávání stavového prostoru:** Systematické procházení kombinatoricky velkých množin možných řešení ($s$) u komplexních inženýrských problémů.
* **Heuristiky vs. Metaheuristiky:** Heuristiky jsou rychlé postupy na míru konkrétního problému; metaheuristiky jsou obecné řídicí strategie (např. simulované žíhání).
* **Diverzifikace vs. Intenzifikace:** Diverzifikace prozkoumává nové vzdálené oblasti (globální pohled); intenzifikace detailně prohledává slibné okolí (lokální optimum).
* **Lineární reprezentace:** Binární (ano/ne), diskrétní (vektor přiřazení zdrojů) nebo permutace (pořadí prvků, např. trasa obchodního cestujícího).
* **Nelineární reprezentace & Redukce:** Kódování v grafech/stromech; chytrá reprezentace (např. permutace u 8 dam) matematicky eliminuje neplatné stavy předem.
* **Práce s omezeními:** Strategie při neplatném řešení – buď okamžité zahození (*Reject*), penalizace účelové funkce (*Penalty*), nebo transformace na platný stav (*Repair*).
* **Účelová funkce:** Hodnotí kvalitu stavu; je buď samospustitelná (přímá minimalizace/maximalizace), nebo naváděcí (vytváří gradient u True/False problémů).
* **Ukončovací kritéria:** Statická (předem daný čas/počet iterací) nebo adaptivní (reagují na vývoj, např. stop při stagnaci či poklesu diverzity).
</details>

<details>
<summary>Lokální prohledávání a metaheuristiky s jedním řešením</summary>

* **Horolezecký algoritmus (Hill Climbing):** Iterativní lokální hledání, které se posouvá ze současného stavu $s$ do sousedního stavu $s'$, pokud $f(s') > f(s)$. Trpí uváznutím v lokálních maximech, na sedlových bodech a hřebenech.
* **Simulované žíhání (Simulated Annealing):** Metaheuristika inspirovaná metalurgií. S pravděpodobností $P = \exp(-\Delta E / T)$ přijímá i zhoršující kroky, což umožňuje uniknout z lokálního optima. Teplota $T$ se v čase snižuje podle ochlazovacího plánu.
* **Zakázané prohledávání (Tabu Search):** Využívá krátkodobou paměť (*Tabu List*) k ukládání naposledy navštívených stavů nebo zakázaných transformací, čímž brání cyklení v grafech a vynucuje diverzifikaci.
* **Aspirace v Tabu Search:** Mechanismus umožňující ignorovat tabu status prvku/kroku, pokud daný krok vede k řešení, které je prokazatelně lepší než doposud nejlepší nalezené historické řešení.
* **Prohledávání s proměnným okolím (VNS - Variable Neighborhood Search):** Systematicky mění strukturu okolí ($N_1, N_2, \dots$) během hledání. Pokud stávající lokální okolí nevede ke zlepšení, přepne se na širší typ okolí, čímž vyskočí z lokálního optima.
</details>

<details>
<summary>Populační metaheuristiky (evoluční algoritmy, inteligence hejna)</summary>

* **Evoluční algoritmy (EA):** Stochastické globální vyhledávací metody inspirované přírodním výběrem. Pracují s celou populací jedinců (chromozomů), kde každý jedinec reprezentuje jedno z možných řešení.
* **Genetické operátory:** *Selekce* (výběr jedinců pro reprodukci na základě jejich zdatnosti/fitness), *Křížení* (Crossover - kombinace genetické informace dvou rodičů) a *Mutace* (náhodná drobná změna genu zajišťující diverzitu).
* **Předčasná konvergence (Slepá ulička):** Stav, kdy populace ztratí genetickou diverzitu a uvízne v lokálním optimu, protože v ní převládnou mírně nadprůměrní jedinci dříve, než stihne prohledat globální prostor.
* **Inteligence hejna (Swarm Intelligence):** Kolektivní chování decentralizovaných, samoorganizovaných systémů (např. mravenci, ptáci). Jedinci komunikují lokálně nebo nepřímo skrze změny prostředí (*Stigmargie*).
* **Optimalizace částicovým hejnem (PSO - Particle Swarm Optimization):** Částice se pohybují v prostoru na základě své vlastní nejlepší historické pozice ($p_{best}$) a nejlepší pozice celého hejna ($g_{best}$).
* **Algoritmus mravenčí kolonie (ACO - Ant Colony Optimization):** Inspirován chováním mravenců při hledání nejkratší cesty. Mravenci ukládají na hrany grafu feromonovou stopu $\tau_{ij}$, která se v čase vypařuje, což přirozeně posiluje kratší (rychleji projité) cesty.
</details>

<details>
<summary>Plánování a reprezentace problému</summary>

* **Plánování v AI:** Hledání optimální sekvence akcí, které transformují počáteční stav světa na stav cílový. Oproti klasickému prohledávání grafů pracuje se strukturovanou vnitřní reprezentací stavů.
* **Klasická reprezentace (STRIPS):** Stavy jsou popsány jako konjunkce pozitivních literálů bez proměnných. Akce mají definované *Preconditions* (podmínky splnitelnosti) a *Effects* (efekty rozdělené na ADD list a DELETE list).
* **PDDL (Planning Domain Definition Language):** Standardizovaný jazyk, který odděluje definici domény (typy, predikáty, obecné akce) od definice konkrétního problému (objekty, konkrétní počáteční stav a cílové podmínky).
* **Předpoklad uzavřeného světa (Closed-World Assumption):** Syntaktické pravidlo, kde vše, co není explicitně uvedeno v popisu daného stavu jako pravdivé, se automaticky považuje za nepravdivé.
</details>

<details>
<summary>Plánování se stavovým prostorem</summary>

* **Plánování dopředu (Progression / Forward Search):** Začíná v počátečním stavu a aplikuje na něj dostupné akce. Tím generuje následné stavy a buduje strom řešení směrem k cíli. Může trpět obrovským faktorem větvení.
* **Plánování pozpátku (Regression / Backward Search):** Začíná od cílových podmínek a postupuje obráceně. Hledá akce, jejichž efekty uspokojují cíle, a jejich podmínky stanovuje jako nové pod-cíle, dokud nedosáhne počátečního stavu.
* **Plánovací grafy (Graphplan):** Polynomiální struktura střídající úrovně stavů (literálů) a úrovně akcí. Pomocí relací vzájemného vyloučení (*Mutual Exclusion - Mutex*) efektivně odhaduje dosažitelnost cíle a slouží jako silný generátor heuristik.
* **Heuristiky pro plánování:** Často založené na relaxaci problému – např. ignorování DELETE listů v STRIPS doméně, což umožňuje snadno v polynomiálním čase spočítat délku optimálního plánu zjednodušeného problému ($h_{FF}$ heuristika).
</details>


<details>
<summary>Bayesovské sítě</summary>

* **Bayesovská síť (Bayesian Network):** Orientovaný akcyklický graf (DAG), kde uzly reprezentují náhodné veličiny a hrany definují přímé pravděpodobnostní závislosti. Umožňuje velmi kompaktní zápis sdruženého rozdělení pravděpodobnosti.
* **Tabulka podmíněných pravděpodobností (CPT):** Každý uzel $X_i$ v síti obsahuje CPT tabulku specifikující rozdělení $P(X_i \mid Parents(X_i))$, což výrazně redukuje počet parametrů oproti plné sdružené tabulce.
* **Podmíněná nezávislost:** Veličiny $X$ a $Y$ jsou podmíněně nezávislé při známém $Z$, pokud platí $P(X, Y \mid Z) = P(X \mid Z) P(Y \mid Z)$. Bayesovské sítě tuto nezávislost přímo strukturálně kódují.
* **D-separace (Directed Separation):** Grafické kritérium pro ověření podmíněné nezávislosti mezi skupinami uzlů. Rozlišuje 3 základní konfigurace: kaskádu ($X \rightarrow Y \rightarrow Z$), vidlici ($X \leftarrow Y \rightarrow Z$) a obrácenou vidlici/kolizor ($X \rightarrow Y \leftarrow Z$).
* **Markovovo přikrytí (Markov Blanket):** Nejmenší množina uzlů, která plně izoluje daný uzel od zbytku sítě. Skládá se z rodičů uzlu, jeho dětí a ostatních rodičů jeho dětí.
</details>

<details>
<summary>Exaktní a aproximační odvozování</summary>

* **Exaktní odvozování (Inference):** Přesný matematický výpočet podmíněné pravděpodobnosti dotazu typu $P(Query \mid Evidence)$. Pro obecné a husté sítě je tento problém NP-těžký.
* **Eliminace proměnných (Variable Elimination):** Algoritmus exaktního odvozování, který vyhodnocuje sumace distribuovaně přes tzv. faktory, čímž eliminuje opakované výpočty shodných mezivýsledků.
* **Aproximační odvozování (Vzorkování):** Používá se pro rozsáhlé sítě, kde je exaktní výpočet netraktovatelný. Generuje velké množství náhodných vzorků simulujících distribuci sítě.
* **Zamítací vzorkování (Rejection Sampling):** Generuje náhodné vzorky od kořenů k listům. Vzorky, které neodpovídají pozorovanému stavu (*Evidence*), se okamžitě zahodí, což je extrémně neefektivní při vzácných jevech.
* **Vzorkování podle důležitosti (Likelihood Weighting):** Fixuje hodnoty pozorovaných veličin napevno (nikdy vzorek nezamítne) a každý vygenerovaný vzorek penalizuje/váží vahou, která odpovídá pravděpodobnosti daného pozorování: $w = \prod P(evidence_i \mid parents_i)$.
* **Markov Chain Monte Carlo (MCMC):** Třída algoritmů (např. Gibbsův vzorkovač), kde aktuální vzorek závisí pouze na předchozím stavu. Generuje novou hodnotu pro proměnnou podmíněnou jejím Markovovým přikrytím.
</details>

<details>
<summary>Čas a neurčitost</summary>

* **Dynamické Bayesovské sítě (DBN):** Rozšíření Bayesovských sítí pro modelování procesů vyvíjejících se v čase. Stav v čase $t$ závisí na stavech v předchozích krocích (zpravidla splňuje Markovův předpoklad 1. řádu).
* **Skrytý Markovův model (HMM):** Speciální případ DBN, kde stavový prostor tvoří jediná diskrétní skrytá veličina $X_t$ a proces generuje pozorovatelná data (emise) $E_t$. Je charakterizován maticí přechodu a emisní maticí.
* **Filtrování (Filtering / Tracking):** Úloha průběžného počítání aktuálního stavu na základě všech dosavadních pozorování: $P(X_t \mid e_{1:t})$. Využívá rekurzivní update (predikce + korekce).
* **Vyhlazování (Smoothing):** Výpočet minulého stavu na základě všech dosavadních pozorování (včetně těch budoucích vzhledem k hledanému času): $P(X_k \mid e_{1:t})$ pro $0 \leq k < t$.
* **Viterbiho algoritmus (Predikce sekvence):** Hledání celkově nejpravděpodobnější posloupnosti skrytých stavů, která vedla k dané sekvenci pozorování: $\arg\max_{x_{1:t}} P(x_{1:t} \mid e_{1:t})$. Využívá dynamické programování.
* **Kalmanův filtr:** Analytické řešení filtrování pro spojité lineární systémy zatížené Gaussovým šumem. Reprezentuje stav pomocí střední hodnoty a kovarianční matice.
</details>


<details>
<summary>Teorie užitku</summary>

* **Teorie užitku (Utility Theory):** Kvantifikuje preference agenta. Každému stavu přiřazuje reálné číslo (užitek $U(s)$), které vyjadřuje jeho atraktivitu. Agent se rozhoduje tak, aby maximalizoval očekávaný užitek.
* **Axiomy racionality:** Formální pravidla chování (Uspořádanost, Tranzitivita, Spojitost, Zaměnitelnost, Monotonie, Nezávislost), která zaručují, že preference agenta lze popsat konzistentní funkcí užitku.
* **Maximální očekávaný užitek (MEU - Maximum Expected Utility):** Základní princip chování racionálního agenta: zvolit takovou akci $a$, která maximalizuje hodnotu $\sum_{s'} P(s' \mid s, a) U(s')$.
* **Postoj k riziku:** *Averze k riziku* (preferuje jistý výnos před loterií se stejnou střední hodnotou, funkce užitku je konkávní), *Vyhledávání rizika* (konvexní funkce užitku) a *Rizikově neutrální* (lineární funkce).
</details>

<details>
<summary>Markovský rozhodovací proces</summary>

* **Markovský rozhodovací proces (MDP):** Formální rámec pro modelování rozhodování v plně pozorovatelném, stochastickém prostředí s diskrétním časem. Definován pěticí $(S, A, T, R, \gamma)$.
* **Transition Model $T(s, a, s')$:** Pravděpodobnostní funkce $P(s' \mid s, a)$, která udává šanci, že akce $a$ ve stavu $s$ povede do stavu $s'$. Splňuje Markovovu vlastnost (budoucnost závisí jen na přítomnosti, ne na historii).
* **Odměna $R(s, a, s')$ a Diskontní faktor $\gamma$:** $R$ je okamžitá odměna za přechod mezi stavy. $\gamma \in [0, 1)$ určuje váhu budoucích odměn oproti současným a zajišťuje konvergenci u nekonečného časového horizontu.
* **Strategie (Policy $\pi$):** Předpis/funkce $\pi(s) \rightarrow a$, která agentovi říká, jakou akci má zvolit v každém možném stavu $s$. Cílem je nalézt optimální strategii $\pi^*$.
* **Bellmanova rovnice optimality:** Matematický vztah definující hodnotu stavu $V^*(s)$ při optimálním chování:
  $$V^*(s) = \max_{a \in A} \sum_{s' \in S} T(s, a, s') \left[ R(s, a, s') + \gamma V^*(s') \right]$$
</details>

<details>
<summary>Iterace hodnot a iterace strategie</summary>

* **Iterace hodnot (Value Iteration):** Algoritmus dynamického programování pro výpočet $V^*(s)$. Začíná s libovolnými hodnotami a rekurzivně aplikuje Bellmanův update jako operátor kontrakce, dokud hodnoty nekonvergují ($\max |V_{k+1} - V_k| < \epsilon$).
* **Iterace strategie (Policy Iteration):** Alternativní algoritmus, který střídá dva kroky: *Evaluace strategie* (výpočet hodnot $V^{\pi}$ pro pevnou strategii řešením soustavy lineárních rovnic) a *Zlepšení strategie* (greedy aktualizace $\pi$ na základě vypočtených hodnot). Často konverguje v méně iteracích.
* **Asynchronní MDP algoritmy:** Aktualizují hodnoty stavů selektivně (např. pouze ty státy, které jsou reálně navštěvované), místo procházení celého stavového prostoru v každém jednotlivém kroku, což výrazně šetří výpočetní čas.
</details>


<details>
<summary>Robotika a konfigurační prostor</summary>

* **Konfigurační prostor (C-space):** Množina všech možných pozic a orientací (konfigurací $q$), kterých může robot v prostředí nabýt. Počet dimenzí C-prostoru odpovídá počtu stupňů volnosti (DoF) robota.
* **C-Obstacle ($C_{obs}$):** Oblast v konfiguračním prostoru, která odpovídá kolizním stavům (kde těleso robota fyzicky koliduje s překážkami v reálném pracovním prostoru $W$).
* **Volný prostor ($C_{free}$):** Podmnožina konfiguračního prostoru, kde robot nekoliduje s žádnou překážkou: $C_{free} = C \setminus C_{obs}$. Úlohou plánování je najít spojitou křivku ležící plně uvnitř $C_{free}$.
* **Minkowského suma:** Matematická operace ($A \oplus B = \{a + b \mid a \in A, b \in B\}$) používaná k transformaci geometrie překážek při redukci robota na bezrozměrný bod v C-prostoru (překážka se geometricky "nafoukne" o rozměr robota).
</details>

<details>
<summary>Kombinatorické a pravděpodobnostní přístupy k plánování pohybu</summary>

* **Kombinatorické přístupy (Exaktní):** Budují explicitní geometrickou reprezentaci volného prostoru $C_{free}$. Jsou výpočetně velmi drahé a netraktovatelné pro vysoké dimenze (vysoké DoF), ale poskytují stoprocentní záruku úplnosti.
* **Grafy viditelnosti (Visibility Graphs):** Kombinatorická metoda pro 2D polygony. Spojuje vrcholy překážek lineárními úsečkami, které neprocházejí vnitřkem překážek. Nejkratší cesta v tomto grafu odpovídá nejkratší bezkolizní cestě.
* **Voronoiův diagram (Skeletizace):** Množina bodů, které mají stejnou vzdálenost ke dvěma nebo více nejbližším překážkám. Plánování na Voronoiově diagramu maximalizuje bezpečnostní clearance (vzdálenost robota od překážek).
* **Pravděpodobnostní přístupy:** Neplánují v explicitně vyjádřeném $C_{free}$, ale testují náhodně vzorkované konfigurace pomocí rychlého detektoru kolizí (*Collision Checker*). Ideální pro roboty s vysokým počtem stupňů volnosti (kloubová ramena).
* **PRM (Probabilistic Roadmaps):** Dvoufázový algoritmus vhodný pro vícenásobné dotazy (multi-query). V *učící fázi* náhodně vzorkuje body v $C_{free}$ a spojuje blízké body lokálním plánovačem do grafu. V *dotazovací fázi* napojí start a cíl na tento graf a hledá cestu pomocí $A^*$.
* **RRT (Rapidly-exploring Random Trees):** Algoritmus pro rychlé budování stromu z počáteční pozice směrem k cíli, ideální pro single-query problémy. V každém kroku vygeneruje náhodný bod $q_{rand}$, najde k němu nejbližší uzel stromu $q_{near}$ a vytvoří nový uzel $q_{new}$ ve směru k $q_{rand}$ o krok $\Delta q$. Má silnou tendenci expandovat do neprozkoumaných oblastí (tzv. *Voronoi bias*).
* **Pravděpodobnostní úplnost:** Klíčová vlastnost pravděpodobnostních algoritmů (PRM, RRT), která garantuje, že pokud bezkolizní cesta reálně existuje, pravděpodobnost jejího nalezení se limitně blíží 1 s rostoucím časem / počtem vygenerovaných vzorků.
</details>

# Statistika

<details>
<summary>Bodové odhady</summary>

* **Bodový odhad:** Jedno konkrétní číslo vypočítané z výběrových dat o rozsahu $n$, které slouží jako optimální aproximace neznámého parametru celého základního souboru (např. populační střední hodnoty $\mu$ nebo rozptylu $\sigma^2$).
* **Výběrový průměr ($\bar{x}$):** Nejlepší bodový odhad střední hodnoty $\mu$. Vypočítá se jako aritmetický průměr všech hodnot: $\bar{x} = \frac{1}{n} \sum_{i=1}^{n} x_i$.
* **Výběrový rozptyl ($s^2$):** Odhad populačního rozptylu $\sigma^2$. Pro dosažení **nestrannosti** se ve jmenovateli používá dělení $(n-1)$ namísto $n$ (tzv. Besselova korekce): $s^2 = \frac{1}{n-1} \sum_{i=1}^{n} (x_i - \bar{x})^2$.
* **Nestrannost (Unbiasedness):** Vlastnost odhadu, kdy se jeho střední hodnota přesně rovná skutečné hodnotě odhadovaného parametru; odhad systematicky nenadhodnocuje ani nepodhodnocuje realitu.
* **Konzistence (Consistency):** S rostoucím rozsahem výběru ($n \to \infty$) se pravděpodobnost, že se odhad liší od skutečného parametru o více než libovolně malé $\varepsilon$, blíží nule (odhad se zpřesňuje s množstvím dat).
* **Efektivita (Efficiency):** Vlastnost vyjadřující, že daný nestranný odhad má ze všech možných nestranných odhadů nejmenší možný rozptyl (dosahuje maximální přesnosti).
* **Centrální limitní věta (CLV):** Zásadní teorém říkající, že pro dostatečně velký rozsah výběru ($n > 30$) má výběrový průměr $\bar{x}$ přibližně normální rozdělení $\bar{X}_n \xrightarrow{d} N\left(\mu, \frac{\sigma^2}{n}\right)$, bez ohledu na rozdělení původní populace.
* **Metoda maximální věrohodnosti (MLE):** Přístup, který hledá takové hodnoty parametrů rozdělení ($\theta$), pro které je pravděpodobnost získání reálně pozorovaných dat maximální. Maximalizuje se věrohodnostní funkce $L(\theta) = \prod_{i=1}^{n} f(x_i; \theta)$, respektive její logaritmus.
* **Výběrová kovariance ($S_{XY}$):** Míra směru a síly lineárního vztahu mezi dvěma veličinami vyjádřená v původních jednotkách: $S_{XY} = \frac{1}{n-1} \sum_{i=1}^{n} (X_i - \bar{X})(Y_i - \bar{Y})$. Organizuje se do symetrické kovarianční matice, kde na diagonále leží rozptyly.
* **Pearsonův korelační koeficient ($r_{XY}$):** Standardizovaná kovariance vydělená součinem směrodatných odchylek, udávající bezrozměrnou sílu lineárního vztahu v intervalu $\langle -1, 1 \rangle$: $r_{XY} = \frac{S_{XY}}{S_X \cdot S_Y}$.
</details>

<details>
<summary>Intervaly spolehlivosti</summary>

* **Interval spolehlivosti (CI):** Náhodný interval konstruovaný kolem bodového odhadu, který s předem zvolenou vysokou pravděpodobností $1-\alpha$ (spolehlivost) pokrývá neznámý pevný parametr populace.
* **Hladina významnosti ($\alpha$):** Pravděpodobnost, že skutečný populační parametr v sestrojeném intervalu nebude obsažen (standardně se volí $\alpha = 0,05$, což odpovídá 95% spolehlivosti).
* **Interpretace spolehlivosti:** Pokud bychom nezávisle opakovali výběr dat a pokaždé sestrojili 95% interval spolehlivosti, pak přesně 95 % těchto různých intervalů bude skutečný parametr obsahovat a 5 % jej mine.
* **Vliv variability dat:** Vyšší rozptyl v datech (směrodatná odchylka $s$) zvyšuje nejistotu, což vede k širšímu intervalu spolehlivosti.
* **Vliv rozsahu výběru:** Větší množství dat ($n$) snižuje standardní chybu odhadu, což způsobuje, že se interval spolehlivosti zužuje (odhad je přesnější).
* **Vliv požadované spolehlivosti:** Pokud požadujeme vyšší jistotu (např. 99% namísto 95%), musíme použít větší kritickou hodnotu, čímž se interval spolehlivosti rozšíří.
</details>

<details>
<summary>Testování statistických hypotéz</summary>

* **Nulová hypotéza ($H_0$):** Výchozí předpoklad, který postuluje absenci efektu, shodu nebo stav "beze změny" (např. testovaný lék nemá žádný účinek).
* **Alternativní hypotéza ($H_1$):** Konkurenční tvrzení, které se snažíme daty prokázat na úkor nulové hypotézy (reprezentuje přítomnost efektu, rozdílu nebo odchylky).
* **Testová statistika:** Náhodná veličina vypočítaná z výběru (např. $t$-statistika, $Z$-statistika), jejíž teoretické rozdělení za platnosti $H_0$ je přesně známé.
* **p-hodnota:** Pravděpodobnost, že za platnosti nulové hypotézy $H_0$ bychom získali testovou statistiku stejně nebo více extrémní, než je hodnota reálně pozorovaná. Pokud $p \le \alpha$, $H_0$ zamítáme.
* **Chyba I. typu ($\alpha$):** Situace, kdy nesprávně zamítneme nulovou hypotézu $H_0$, přestože ve skutečnosti platí (tzv. falešný poplach). Pravděpodobnost této chyby je omezena hladinou významnosti $\alpha$.
* **Chyba II. typu ($\beta$):** Situace, kdy nezamítneme nulovou hypotézu $H_0$, přestože ve skutečnosti neplatí a platí alternativní hypotéza $H_1$ (přehlédnutí reálného efektu).
* **Síla testu ($1-\beta$):** Pravděpodobnost, že test správně zamítne neplatnou nulovou hypotézu (rozpozná skutečně existující rozdíl). V praxi se požaduje síla alespoň 80 %.
* **Faktory ovlivňující sílu testu:** Síla testu roste s větším rozsahem výběru ($n$), větší velikostí reálného efektu (Effect Size), menší variabilitou dat ($\sigma$) nebo s uvolněním přísnosti hladiny významnosti $\alpha$.
</details>

<details>
<summary>ANOVA</summary>

* **ANOVA (Analýza rozptylu):** Statistická metoda určená pro testování rovnosti středních hodnot u tří a více nezávislých skupin rozkladem celkové variability dat.
* **Hypotézy ANOVA:** $H_0: \mu_1 = \mu_2 = \dots = \mu_k$ (všechny skupinové průměry jsou stejné) vůči $H_1: \exists \, i, j : \mu_i \neq \mu_j$ (alespoň jeden pár průměrů se liší).
* **Předpoklady ANOVA:** Nezávislost jednotlivých pozorování, normalita dat uvnitř skupin a homogenita variancí (shoda rozptylů, ověřovaná např. Leveneovým testem).
* **Modelové vyjádření:** Každé pozorování lze zapsat jako lineární model $Y_{ij} = \mu + \alpha_i + \varepsilon_{ij}$, kde $\mu$ je celkový průměr, $\alpha_i$ je efekt $i$-té skupiny ($\sum \alpha_i = 0$) a $\varepsilon_{ij}$ je normální náhodná chyba.
* **Jednofaktorová vs. Vícefaktorová ANOVA:** Jednofaktorová zkoumá vliv jedné kategorické proměnné. Vícefaktorová analyzuje vliv více faktorů současně a umožňuje testovat i jejich interakce (zda efekt jednoho faktoru závisí na úrovni druhého).
* **Rozklad variability:** Celkový součet čtverců ($SS_{total}$) se rozkládá na meziskupinový součet čtverců ($SS_{between}$, variabilita způsobená faktorem) a vnitroskupinový součet čtverců ($SS_{within}$, náhodný šum/rezidua).
* **F-test v ANOVA:** Testová statistika definovaná jako podíl středních čtverců $F = MS_{between} / MS_{within}$. Výrazně vysoká hodnota $F$ vede k zamítnutí $H_0$.
* **Kumulace chyby I. typu:** Důvod, proč nepoužít vícero párových t-testů. Při $m$ nezávislých t-testech stoupá rodinná hladina chyby na $1 - (1-\alpha)^m$. ANOVA provádí jeden globální test na stabilní hladině $\alpha$.
* **Post-hoc testy:** Následné testy prováděné pouze v případě, že globální F-test zamítne $H_0$. Určují, které konkrétní dvojice skupin se od sebe liší, a používají korekce (Tukey HSD, přísná Bonferroniho korekce) pro potlačení chyby I. typu.
</details>

<details>
<summary>Neparametrické testy hypotéz</summary>

* **Neparametrické testy:** Robustní testy nevyžadující konkrétní tvar rozdělení populace (normalitu). Zpravidla transformují původní hodnoty na jejich **pořadí (ranks)**, což sice mírně snižuje sílu testu, ale odstraňuje citlivost na odlehlé hodnoty.
* **Wilcoxonův jednovýběrový test:** Neparametrická alternativa k jednovýběrovému t-testu. Testuje, zda je medián jednoho výběru roven zadané konstantě ($H_0: \tilde{x} = c$).
* **Wilcoxonův párový test:** Alternativa k párovému t-testu. Pracuje s absolutními hodnotami rozdílů dvojic, kterým přiřadí vzestupné pořadí a následně porovnává sumu pořadí kladných odchylek ($T^+$) a záporných odchylek ($T^-$).
* **Mann-Whitneyův test (Wilcoxon Rank-Sum):** Alternativa k dvouvýběrovému t-testu pro dvě nezávislé skupiny. Spojí data obou skupin, seřadí je a testuje, zda se liší sumy globálních pořadí ($R_1$ a $R_2$), tj. zda má jedna skupina tendenci nabývat větších hodnot.
* **Kruskal-Wallisův test:** Neparametrická alternativa k jednofaktorové ANOVA pro tři a více nezávislých skupin. Zobecňuje Mann-Whitneyův test; porovnává průměrná globální pořadí skupin a testová statistika $H$ má asymptoticky $\chi^2$ rozdělení.
* **Znaménkový test (Sign Test):** Nejjednodušší párový či jednovýběrový test. Zcela ignoruje velikost odchylek a sleduje pouze jejich znaménko (`+` nebo `-`). Počet znamének testuje proti binomickému rozdělení $Bi(n, p=0,5)$, přičemž nevyžaduje ani symetrii rozdělení.
</details>

<details>
<summary>Mnohonásobná lineární regrese</summary>

* **Mnohonásobná lineární regrese:** Modelování lineárního vztahu mezi jednou spojitou závislou proměnnou $Y$ a dvěma či více nezávislými prediktory $X_j$. Umožňuje izolovat vliv jednoho prediktoru při kontrole ostatních.
* **Základní předpoklady:** Linearita vztahu, nezávislost náhodných chyb, homoskedasticita (konstantní rozptyl chyb), normalita chyb (klíčová pro t-testy) a absence dokonalé multikolinearity.
* **Rezidua vs. Náhodné chyby:** Náhodné chyby ($\varepsilon_i$) jsou nepozorovatelné teoretické odchylky v populaci. Rezidua ($r_i = Y_i - \hat{Y}_i$) jsou reálně spočítané svislé odchylky dat od odhadnutého modelu, na kterých se předpoklady ověřují.
* **Matice plánu (Design Matrix — $X$):** Matice o rozměrech $n \times (k+1)$, kde první sloupec tvoří jednotkové vektory (příslušící absolutnímu členu $\beta_0$) a další sloupce obsahují hodnoty jednotlivých prediktorů.
* **Metoda nejmenších čtverců (OLS):** Algoritmus minimalizující součet čtverců reziduí: $\sum r_i^2 \to \min$. Parciální derivace součtu čtverců podle $\beta_j$ položené rovny nule vedou na soustavu normálních rovnic $X^T X \hat{\beta} = X^T Y$.
* **Analytické řešení OLS:** Pokud je matice plánu regulární (nevykazuje dokonalou kolinearitu), koeficienty se spočítají jako: $\hat{\beta} = (X^T X)^{-1} X^T Y$.
* **Globální F-test v regresi:** Testuje významnost modelu jako celku proti nulovému modelu (pouhému průměru). $H_0: \beta_1 = \beta_2 = \dots = \beta_k = 0$.
* **Individuální t-testy v regresi:** Testují statistickou významnost každého jednotlivého koeficientu $\beta_j$ samostatně. Zjišťují, zda daný prediktor přináší unikátní informaci za přítomnosti všech ostatních prediktorů ($H_0: \beta_j = 0$).
* **Koeficient determinace ($R^2$):** Udává podíl celkové variability závislé proměnné vysvětlené regresním modelem: $R^2 = S_R / S_T = 1 - (S_e / S_T)$, kde $S_T$ je celkový, $S_e$ reziduální a $S_R$ regresní součet čtverců.
* **Adjustovaný koeficient determinace ($R^2_{adj}$):** Úprava $R^2$ penalizující model za přidávání zbytečných prediktorů zohledněním stupňů volnosti: $R^2_{adj} = 1 - \frac{S_e / (n-k-1)}{S_T / (n-1)}$. Brání overfittingu.
* **Postupná regrese (Stepwise Regression):** Automatizovaný algoritmický výběr optimálního podmodelu přidáváním či odebíráním prediktorů na základě statistických kritérií (např. AIC). Hrozí u ní overfitting a ignoruje kauzální kontext.
</details>

<details>
<summary>Multikolinearita</summary>

* **Multikolinearita:** Stav, kdy jsou dvě nebo více vysvětlujících proměnných (prediktorů) v regresním modelu silně lineárně provázané (korelované), což komplikuje oddělení jejich samostatných vlivů.
* **Důsledky multikolinearity:** Regresní koeficienty $\beta$ jsou extrémně nestabilní, mají obrovské směrodatné chyby a t-testy je mohou chybně označit za nevýznamné, přestože model jako celek (F-test) a jeho predikční schopnost fungují dobře.
* **Singularita matice:** Při *dokonalé* multikolinearitě je matice $X^T X$ singulární (její determinant je nula), což znamená, že neexistuje inverzní matice $(X^T X)^{-1}$ a model nelze matematicky spočítat.
* **Variance Inflation Factor (VIF):** Diagnostický ukazatel měřící, kolikrát je rozptyl odhadnutého regresního koeficientu zvýšen kvůli kolinearitě: $VIF_j = \frac{1}{1 - R_j^2}$, kde $R_j^2$ je koeficient determinace regrese $X_j$ na všechny ostatní prediktory.
* **Kritické hodnoty VIF:** Hodnota $VIF \approx 1$ značí absenci kolinearity; $VIF > 5$ indikuje možný problém a $VIF > 10$ představuje závažnou multikolinearitu, kterou je nutné řešit (např. vyřazením proměnné či pomocí PCA).
</details>

<details>
<summary>Autokorelace</summary>

* **Autokorelace:** Situace, kdy teoretické náhodné chyby (a potažmo rezidua) nejsou nezávislé, ale vykazují vzájemnou systematickou závislost v rámci sekvence. Typická pro data ve formě časových řad.
* **Autokorelace prvního řádu — $AR(1)$:** Nejčastější případ, kdy chyba v čase $t$ přímo lineárně závisí na chybě v předchozím čase $t-1$ prostřednictvím koeficientu $\rho$: $\varepsilon_t = \rho \varepsilon_{t-1} + u_t$.
* **Důsledky autokorelace:** Odhady koeficientů $\beta$ metodou OLS zůstávají nestranné, ale ztrácejí efektivitu (nemají nejmenší možný rozptyl). OLS navíc systematicky podhodnocuje směrodatné chyby koeficientů.
* **Zkreslení testů:** V důsledku podhodnocení směrodatných chyb chybami zatížený model uměle nadhodnocuje t-statistiky, což vede k neplatnosti p-hodnot a k falešnému prohlašování prediktorů za statisticky významné.
* **Durbin-Watsonův test:** Standardní test pro detekci autokorelace 1. řádu, jehož statistika $DW$ nabývá hodnot v rozmezí $\langle 0, 4 \rangle$.
* **Interpretace DW statistiky:** Hodnota $DW \approx 2$ znamená nezávislost reziduí (absence autokorelace); $DW \to 0$ indikuje silnou pozitivní autokorelace a $DW \to 4$ značí silnou negativní autokorelace.
</details>

<details>
<summary>Analýza hlavních komponent (PCA)</summary>

* **Analýza hlavních komponent (PCA):** Nepřehlížená metoda redukce dimenze, která transformuje množinu korelovaných proměnných na menší počet lineárně nezávislých (ortogonálních) proměnných zvaných **hlavní komponenty (PC)**.
* **Geometrická podstata:** Ortonormální projekce dat do nového podprostoru. První osa ($PC_1$) se orientuje ve směru maximálního rozptylu dat; druhá osa ($PC_2$) zachycuje maximum zbylého rozptylu a je striktně kolmá k první ose.
* **Matematický tvar:** Každá komponenta je lineární kombinací původních proměnných: $PC_k = a_{k1}X_1 + a_{k2}X_2 + \dots + a_{kp}X_p$. Koeficienty $a_{kj}$ (*loadings*) definují směrové vektory nových os.
* **Nezbytnost standardizace:** Před výpočtem PCA je nutné data centrovat a standardizovat (převést na z-skóre), jinak proměnné s přirozeně většími řádovými jednotkami dominantně ovládnou rozptyl a zkreslí výsledky.
* **Charakteristická rovnice:** Výpočet PCA spočívá v řešení spektra matice vztahů (kovarianční nebo korelační $\mathbf{S}$): $\mathbf{S}\mathbf{a}_k = \lambda_k \mathbf{a}_k$. Hledání netriviálních řešení probíhá přes nulový determinant: $\det(\mathbf{S} - \lambda \mathbf{I}) = 0$.
* **Vlastní čísla ($\lambda_k$) a Vlastní vektory ($\mathbf{a}_k$):** Vlastní čísla vyjadřují velikost rozptylu, který daná komponenta absorbuje ($\lambda_1 \ge \lambda_2 \ge \dots$). Odpovídající vlastní vektory udávají směry hlavních komponent.
* **Podíl vysvětlené variability:** Poměr rozptylu $k$-té komponenty k celkovému rozptylu je dán vztahem $\lambda_k / \sum_{j=1}^p \lambda_j$. Kumulativní součet určuje celkovou zachovanou informaci.
* **Kritéria pro volbu počtu komponent:** * *Kaiserovo pravidlo:* Ponechat pouze komponenty s vlastním číslem $\lambda_k > 1$ (vysvětlí víc než původní standardizovaná proměnná).
  * *Scree plot (Sutinový graf):* Vizuální hledání "kolena" (zlomu) v klesajícím grafu vlastních čísel.
  * *Kritérium celkové variability:* Ponechat dostatek komponent pro zachování 80–90 % původního rozptylu.
* **Scores vs. Loadings:** *Loadings* jsou korelace mezi původními proměnnými a hlavními komponentami (vlastní vektory). *Scores* jsou nově transformované souřadnice původních pozorování v novém prostoru hlavních komponent.
</details>

# Infrastrukturní a cloudové systémy

<details>
<summary>Architektura datových center (Data-center architecture)</summary>

* **Datové centrum (DC):** Komplexní technologický objekt navržený pro integraci IT vybavení (servery, úložiště, síťové prvky) a podpůrné infrastruktury (napájení, chlazení, bezpečnost).
* **PUE (Power Usage Effectiveness):** Klíčová metrika efektivity DC vyjadřující poměr celkové energie spotřebované objektem k energii spotřebované čistě IT hardwarem: $\text{PUE} = \frac{\text{Celková energie}}{\text{Energie IT vnější zátěže}}$. Ideální hodnota je 1.0, moderní cloudová DC dosahují ~1.1.
* **Spine-Leaf (Clos) topologie:** Moderní dvouvrstvá síťová architektura optimalizovaná pro **East-West traffic** (komunikace mezi servery uvnitř DC). Každý přístupový switch (*Leaf*) je připojen ke každému páteřnímu switchi (*Spine*), což garantuje deterministickou latenci (max. 2 hopy).
* **North-South vs. East-West traffic:** *North-South* reprezentuje datový provoz mezi datovým centrem a vnějším světem (klientem). *East-West* označuje interní datové přenosy mezi servery uvnitř DC (např. synchronizace databází, komunikace mikroslužeb), které v cloudové éře dominují.
* **SDN (Software Defined Networking):** Architektonický přístup, který logicky odděluje řídicí rovinu sítě (*Control Plane*) od roviny pro přeposílání dat (*Data Plane / Forwarding Plane*), což umožňuje programovou a dynamickou rekonfiguraci sítě.
* **Disagregace hardware:** Trend fyzického oddělování komponent (výpočetní výkon CPU, paměť RAM, persistentní úložiště) do samostatných samostatně škálovatelných fondů, které jsou propojeny ultra-rychlou síťovou bází (fabric).
</details>

<details>
<summary>Superpočítače vs. heterogenní clustery (Supercomputers vs. heterogeneous clusters)</summary>

* **Superpočítače (HPC - High Performance Computing):** Monolitické, vysoce specializované systémy složené z homogenního hardware, navržené pro řešení extrémně **těsně vázaných (tightly-coupled)** úloh vyžadujících masivní synchronizaci v reálném čase.
* **MPI (Message Passing Interface):** Standardizovaný komunikační protokol a knihovna používaná u superpočítačů pro předávání zpráv mezi procesy běžícími na tisících samostatných výpočetních jader v distribuované paměti.
* **Nízkolatentní technologie:** Síťová rozhraní s extrémně nízkou latencí a vysokou propustností (např. InfiniBand, HPE Slingshot) využívající mechanismus **RDMA** (Remote Direct Memory Access) pro přímý zápis do RAM vzdáleného uzlu bez účasti OS.
* **Heterogenní clustery (HTC - High Throughput Computing):** Distribuované systémy složené z komoditního, často různorodého hardware (různé generace CPU, různé typy GPU doplňkových akcelerátorů) zaměřené na maximalizaci celkového objemu odbavených úloh za dlouhý časový úsek.
* **Volně vázané úlohy (Loosely-coupled):** Úlohy (též *embarrassingly parallel*), které lze rozdělit na velké množství vzájemně zcela nezávislých podúloh běžících bez nutnosti průběžné komunikace či synchronizace (např. Monte Carlo simulace, renderování po snímcích).
</details>

<details>
<summary>Orchestrační architektury (PBS/grids, Kubernetes, OpenStack)</summary>

* **Orchestrace infrastruktury:** Automatizovaný proces správy, plánování, alokace zdrojů a životního cyklu aplikací nebo hardwarových prostředků v distribuovaném prostředí.
* **PBS (Portable Batch System) / Slurm:** Plánovače zaměřené na dávkové zpracování úloh (*Batch processing*) v HPC. Uživatel zadá úlohu do fronty s požadavky na zdroje a systém ji spustí v momentě, kdy je HW volný, s cílem maximalizovat celkovou utilizaci strojů.
* **Fairshare algoritmus:** Prioritizační mechanismus v dávkových systémech, který dynamicky upravuje prioritu čekajících úloh na základě historického objemu prostředků, které daný uživatel či projekt v poslední době vyčerpal.
* **Backfilling:** Optimalizační strategie plánovače HPC, která do dočasně vzniklých časových a kapacitních mezer (kdy systém čeká na uvolnění velkého počtu uzlů pro masivní výpočet) "vsune" menší, krátké úlohy, které stihnou doběhnout před startem hlavní úlohy.
* **OpenStack (IaaS):** Komplexní modulární open-source cloudová platforma pro správu a virtualizaci hardwarových zdrojů, poskytující uživatelům on-demand přístup k virtuálním strojům (VM) s plnými administrátorskými (root) právy.
* **Komponenty OpenStack:** *Nova* (compute modul řídící životní cyklus VM), *Neutron* (správa virtuálních sítí a firewallů), *Keystone* (autentizace a autorizace), *Cinder* (perzistentní blokové úložiště pro VM).
* **Kubernetes (K8s / CaaS):** Orchestrační platforma specializovaná na automatizaci nasazování, škálování a správu aplikací zabalených do **kontejnerů**, které na rozdíl od VM sdílejí jádro hostitelského OS.
* **Deklarativní přístup a Control Loop:** Uživatel definuje požadovaný cílový stav systému (např. v YAML souboru). Kubernetes tento stav neustále porovnává s realitou skrze kontrolní smyčku (*Reconciliation Loop*) a v případě odchylky automaticky zjedná nápravu (*Self-healing*).
* **Základní abstrakce K8s:** *Pod* (nejmenší nasaditelná jednotka obsahující jeden či více sdílených kontejnerů), *Node* (fyzický nebo virtuální pracovník vykonávající pody), *Control Plane* (řídicí vrstva spravující stav clusteru).
</details>

<details>
<summary>GPU vs. CPU výpočty a masivní paralelismus (GPU vs. CPU computing & workloads)</summary>

* **CPU (Latency Oriented):** Architektura optimalizovaná pro minimalizaci času vykonání jednoho sekvenčního vlákna (nízká latence). Většinu plochy čipu zabírá složitá řídicí logika (Control Unit, Branch Prediction) a masivní vyrovnávací paměti (*Cache*).
* **GPU (Throughput Oriented):** Architektura optimalizovaná pro maximální celkový objem paralelně vykonané práce za jednotku času (vysoká propustnost). Skládá se z tisíců jednoduchých jader (ALU) seskupených do bloků sdílejících řídicí logiku.
* **SIMT (Single Instruction, Multiple Threads):** Paralelní model GPU, kde tisíce vláken vykonávají stejnou instrukci paralelně nad různými datovými elementy (datový paralelismus). Vlákna jsou hardwarově plánována v blocích (tzv. *warps* u NVIDIA).
* **Skrývání latence (Latency Hiding):** Mechanismus GPU, které neobsahuje velké cache pro ochranu před pomalým přístupem do VRAM. Místo toho hardwarový plánovač okamžitě přepne na výpočet jiného bloku vláken, pokud stávající blok čeká na doručení dat z paměti.
* **Amdahlův zákon (Strong Scaling):** Definuje limit zrychlení fixního problému při zapojení $N$ paralelních procesorů: $S(N) = \frac{1}{(1-P) + \frac{P}{N}}$, kde $P$ je paralelizovatelná část programu. Celkové zrychlení je striktně omezeno sekvenční částí kódu ($1-P$).
* **PCIe Bottleneck:** Výkonnostní úzké hrdlo způsobené relativně pomalým přenosem dat mezi systémovou RAM (CPU) a dedikovanou VRAM (GPU) přes sběrnici PCI Express. Režie přenosu může smazat benefit rychlého výpočtu na GPU.
* **GPGPU (General-Purpose GPU):** Využití grafických akcelerátorů pro obecné nematematicko-grafické výpočty (kryptografie, fyzika, Deep Learning) pomocí programovacích rozhraní jako proprietární **CUDA** (NVIDIA) nebo otevřené **OpenCL**.
* **Multi-Instance GPU (MIG):** Hardwarová technologie umožňující rozdělit jedno fyzické GPU (architektury Ampere a novější) až na 7 samostatných, na úrovni křemíku plně izolovaných instancí s vlastní garantovanou kapacitou VRAM a výpočetních jader.
</details>

<details>
<summary>Škálovatelné aplikační modely (Scalable application models)</summary>

* **Škálovatelnost:** Schopnost systému efektivně absorbovat zvýšený objem požadavků/zátěže lineárním navýšením hardwarových prostředků bez strukturální změny architektury či degradace výkonu.
* **Vertikální škálování (Scale-up):** Zvyšování výkonu stávajícího uzlu přidáním silnějšího procesoru, více RAM či rychlejšího disku. Má pevné hardwarové limity a finančně roste exponenciálně.
* **Horizontální škálování (Scale-out):** Zvyšování výkonu přidáváním dalších samostatných uzlů do distribuovaného systému. Představuje základ cloudových architektur, vyžaduje však síťový *Load Balancer* a distribuovaný software.
* **Gustafsonův zákon (Weak Scaling):** Teoretický model škálování, který předpokládá, že s rostoucím počtem procesorů roste i velikost samotného řešeného problému v čase: $S(N) = N - (N-1)(1-P)$. Ukazuje efektivitu paralelního zpracování pro variabilní velikost dat.
* **Paralelní režie (Brzdy lineárního růstu):** Faktory způsobující, že přidání $N$-násobku uzlů nepřinese $N$-násobné zrychlení. Způsobeno síťovou latencí komunikace, synchronizačními bariérami (čekání na nejpomalejší uzel) a serializací zdrojů (např. zámky nad jednou DB).
* **Bezstavovost (Statelessness):** Princip návrhu aplikace, kde aplikační vrstva neukládá žádná persistentní ani session data do své lokální paměti RAM či na lokální disk. Každý požadavek nese veškerou informaci v sobě (např. JWT token) nebo ji čte z externí distribuované databáze, což umožňuje libovolné horizontální škálování a směrování požadavků.
* **Serverless / FaaS (Function as a Service):** Výpočetní model, kde je správa serverů plně abstrahována poskytovatelem. Kód (funkce) se spouští ryze na základě událostí (*Event-driven*), škáluje se dynamicky podle zátěže a umožňuje škálování až na nulu (neplatí se za nečinnost).
* **Twelve-Factor App:** Metodika 12 principů pro budování přenositelných, robustních a horizontálně škálovatelných cloud-native aplikací (např. explicitní závislosti, konfigurace v proměnných prostředí, bezstavové procesy, logy jako proud událostí).
</details>

<details>
<summary>Úrovně úložišť, technologie, teplota dat a přesun (Storage tiers & movement)</summary>

* **Teplota dat:** Klasifikace dat na základě frekvence přístupu a modifikace. Rozlišuje se data **Horká** (Hot - aktivně využívaná, kritická latence), **Teplá** (Warm - méně častý přístup, vyvážený poměr cena/výkon) a **Studená/Archivní** (Cold - přístup výjimečně, důraz na nejnižší cenu za GB).
* **Hierarchie úložišť (Storage Tiers):** Odstupňování technologií od nejrychlejších po nejlevnější. *Tier 0/1* (HBM, RAM, NVMe SSD pro horká data), *Tier 2* (SAS/SATA HDD pro teplá data), *Tier 3* (Páskové knihovny LTO, cloudové cold archivy pro studená data).
* **Block Storage:** Úložiště poskytující surové, nenaformátované datové bloky bez metadat (přístupné např. přes iSCSI, Fibre Channel). Nabízí nejnižší latenci a nejvyšší výkon, ideální pro databáze a virtuální pevné disky (OpenStack Cinder).
* **File Storage:** Úložiště organizující data do hierarchické struktury souborů a složek se základními metadaty. Je sdílené napříč uzly přes síťové protokoly (NFS, SMB).
* **Object Storage:** Úložiště ukládající data jako nestrukturované objekty do plochého jmenného prostoru. Každý objekt obsahuje unikátní ID, samotná data a bohatá rozšiřitelná metadata. Přístup probíhá přes HTTP REST API (např. AWS S3, Ceph), což zajišťuje masivní horizontální škálovatelnost.
* **Infrastruktura propojení (DAS, NAS, SAN):** *DAS* (Direct Attached Storage - disky přímo uvnitř serveru), *NAS* (Network Attached Storage - souborové úložiště připojené do standardní LAN sítě), *SAN* (Storage Area Network - dedikovaná vysokorychlostní síť blokového typu).
* **ILM / HSM:** *ILM* (Information Lifecycle Management) je strategie správy životního cyklu dat na základě obchodních pravidel. *HSM* (Hierarchical Storage Management) je konkrétní technologický proces, který tato pravidla vykonává a automaticky přesouvá soubory mezi rychlými a levnými vrstvami úložišť.
* **Erasure Coding (EC):** Metoda ochrany dat v distribuovaných úložištích, která data rozdělí na $k$ datových fragmentů a dopočítá $m$ paritních fragmentů. Systém dokáže obnovit data při výpadku libovolných $m$ uzlů/disků, přičemž spotřebuje výrazně méně kapacity než klasické zrcadlení (Replication).
* **Caching vs. Tiering:** *Caching* vytváří dočasnou kopii často čtených dat v rychlejší vrstvě, zatímco originál zůstává na pomalém médiu. *Tiering* fyzicky stěhuje data mezi vrstvami na základě jejich teploty, data existují vždy pouze na jednom místě.
* **Lokalita dat (Data Locality):** Architektonický princip v distribuovaných systémech (např. Hadoop), který se snaží umístit výpočetní proces na stejný fyzický uzel, kde jsou uložena zpracovávaná data, za účelem eliminace síťového přenosu.
</details>

<details>
<summary>Odolnost a spolehlivost infrastruktury (Infrastructure resilience)</summary>

* **SPOF (Single Point of Failure):** Jakákoliv kritická komponenta systému (např. jeden switch, jeden napájecí zdroj, jeden disk), jejíž případné selhání způsobí okamžitý výpadek celé infrastruktury či služby. Cílem resilientního návrhu je eliminace všech SPOF.
* **Klasifikace Uptime Institute (Data Center Tiers):** Standardizované hodnocení spolehlivosti fyzických DC. *Tier I* (základní bez redundance), *Tier II* (částečná redundance komponent N+1), *Tier III* (souběžná udržitelnost bez přerušení provozu IT), *Tier IV* (úplná odolnost proti chybám - Fault Tolerance, zvládne jakýkoliv výpadek HW větví).
* **Modely redundance:** *N+1* (množství komponent nutných pro provoz navýšené o jeden záložní prvek pro případ poruchy), *2N* (úplné zdvojení celých infrastrukturních větví, např. dva nezávislé přívody elektrické energie).
* **Blast Radius (Akční rádius poruchy):** Maximální rozsah škod nebo počet zasažených systémů/uživatelů při selhání konkrétní komponenty či zavedení chybné konfigurace. Architektura se snaží o jeho minimalizaci (např. izolací do autonomních zón).
* **High Availability (HA) Cluster:** Skupina vzájemně propojených serverů nakonfigurovaných tak, že při hardwarovém selhání jednoho uzlu dojde k automatickému detekování výpadku (*Failover*) a převzetí jeho služeb ostatními funkčními uzly v řádu sekund.
* **RTO (Recovery Time Objective):** Klíčová metrika kontinuity provozu definující maximální přípustný časový úsek, po který může být služba od okamžiku výpadku nedostupná, než dojde k její kompletní obnově.
* **RPO (Recovery Point Objective):** Metrika definující maximální přípustné stáří dat, o která může systém v důsledku havárie přijít; udává časový odstup mezi posledním konzistentním stavem (zálohou/replikou) a momentem selhání.
</details>

<details>
<summary>Automatizace, DevOps/GitOps a SRE</summary>

* **DevOps:** Kulturní a organizační hnutí zaměřené na smazání bariér mezi vývojem softwaru (*Dev*) a jeho provozem (*Ops*) skrze sdílenou odpovědnost, kontinuální zpětnou vazbu a masivní automatizaci.
* **CI/CD (Continuous Integration / Continuous Deployment):** *CI* automaticky integruje, sestavuje a testuje kód při každé změně v Gitu pro včasnou detekce chyb. *CD* zajišťuje automatizované a bezpečné nasazení úspěšně otestovaného buildu přímo do produkčního prostředí.
* **Infrastructure as Code (IaC):** Metodika správy a konfigurace infrastrukturních zdrojů (sítě, VM, load balancery) pomocí deklarativních, člověkem čitelných textových souborů (např. Terraform, Ansible), což umožňuje verzování infrastruktury v Gitu a její replikovatelnost.
* **GitOps:** Evoluční stupeň IaC, kde je Git ustanoven jako **jediný zdroj pravdy** (Single Source of Truth) pro stav infrastruktury. Nasazení probíhá výhradně schválením změn v Gitu (Pull Request), přičemž synchronizační agent v clusteru (např. ArgoCD) neustále srovnává deklarovaný stav s realitou.
* **SRE (Site Reliability Engineering):** Disciplína aplikující principy softwarového inženýrství na řešení provozních problémů infrastruktury. SRE inženýři dělí svůj čas mezi automatizaci (odstraňování rutinní práce — *Toil*) a udržování stability systémů.
* **SLI, SLO, SLA:** *SLI* (Site Reliability Indicator - konkrétní kvantitativní metrika, např. latence požadavků), *SLO* (Site Reliability Objective - cílová hodnota spolehlivosti odvozená z SLI, např. latence < 200 ms pro 99.9 % požadavků), *SLA* (Site Reliability Agreement - právní závazek vůči zákazníkům s finančními penalizacemi při nesplnění SLO).
* **Error Budget (Rozpočet na chyby):** Matematická tolerance nespolehlivosti definovaná jako $100\,\% - \text{SLO}$. Tento rozpočet může vývojový tým legálně spotřebovat pro riskantní nasazování nových funkcí. Pokud je vyčerpán, nasazování se zastavuje a prioritu získává stabilizace infrastruktury.
* **Observability (Pozorovatelnost):** Schopnost dedukovat vnitřní stavy a anomálie distribuovaného systému na základě jeho externích výstupů. Stojí na třech pilířích (tzv. *MELT*): **Metriky** (agregovaná čísla v čase), **Logy** (strukturované záznamy událostí) a **Traces** (trasování průchodu jednoho požadavku napříč mikroslužbami).
</details>

<details>
<summary>Workflow manažeři a přenositelnost úloh (Workflow managers & portability)</summary>

* **Workflow Manager:** Softwarový nástroj navržený pro orchestraci, exekuci a monitorování komplexních výpočetních řetězců (pipelines), kde výstup jednoho programu tvoří vstup pro další. Kroky a jejich závislosti jsou typicky modelovány jako **DAG** (Directed Acyclic Graph).
* **Příklady workflow nástrojů:** *Nextflow* a *Snakemake* (standardy v bioinformatice a zpracování velkých vědeckých dat, podporující nativní integraci s HPC a cloudem), *Apache Airflow* (využívaný v datovém inženýrství).
* **Přenositelnost úloh (Workload Portability):** Schopnost spustit identickou výpočetní úlohu na libovolné podkladové infrastruktuře (lokální workstation, HPC cluster, veřejný cloud) s garantovaným identickým chováním bez nutnosti přepisování zdrojového kódu či konfiguračních skriptů.
* **Apptainer (dříve Singularity):** Kontejnerizační technologie vyvinutá speciálně pro potřeby HPC a superpočítačů. Na rozdíl od Dockeru nepoužívá rootovského démona, spouští procesy s právy aktuálního uživatele a umožňuje přímou nativní integraci s HPC technologiemi (InfiniBand, GPU, Slurm).
* **Cloud-native architektura:** Přístup k vývoji aplikací, který od počátku počítá s vlastnostmi cloudu. Využívá mikroslužby, bezstavovost, kontejnerizaci a orchestraci (Kubernetes), což zajišťuje maximální přenositelnost mezi různými cloudovými poskytovateli (prevence *Vendor Lock-in*).
</details>

<details>
<summary>Identita, SSO a AAI (Identity, SSO, and AAI)</summary>

* **Identita uživatele:** Digitální reprezentace entity v systému, složená z unikátního identifikátoru a sady doprovodných atributů (jméno, e-mail, role, domovská organizace).
* **Autentizace (AuthN):** Bezpečnostní proces ověření deklarované identity subjektu (odpověď na otázku „Kdo jsi?“), realizovaný pomocí jednoho či více faktorů (heslo, hardwarový token, biometrie, MFA).
* **Autorizace (AuthZ):** Následný proces ověření oprávnění autentizovaného subjektu (odpověď na otázku „Co smíš dělat?“), určující přístupová práva ke konkrétním infrastrukturním zdrojům.
* **Single Sign-On (SSO):** Autentizační mechanismus, který umožňuje uživateli přihlásit se pouze jednou v rámci jednoho centrálního poskytovatele identit (*IdP*) a získat transparentní přístup do mnoha nezávislých softwarových systémů/služeb (*SP*) bez opakovaného zadávání přihlašovacích údajů.
* **AAI (Authentication and Authorization Infrastructure):** Standardizovaný konfederační rámec umožňující bezpečné sdílení identit, autentizaci a autorizaci uživatelů napříč nezávislými distribuovanými organizacemi a výpočetními infrastrukturami.
* **Federace identit:** Sdružení nezávislých organizací (např. akademických institucí v rámci národní federace *eduid.cz* nebo mezinárodní *eduGAIN*), které si vzájemně důvěřují v oblasti ověřování identit. Uživatel z domovské univerzity se může svými interními údaji přihlásit k výpočetním zdrojům jiné partnerské organizace.
* **SAML (Security Assertion Markup Language):** Otevřený standard založený na formátu XML pro bezpečnou výměnu autentizačních a autorizačních dat (asercí) mezi poskytovatelem identity (*IdP*) a poskytovatelem služby (*SP*). Hojně rozšířený v akademických federacích.
* **OIDC (OpenID Connect):** Moderní identifikační vrstva postavená nad autorizačním protokolem OAuth 2.0. Pro přenos identity využívá odlehčený formát JSON a kryptograficky podepsané **JWT tokeny** (JSON Web Token), což z něj dělá standard pro cloudové a webové mikroslužby.
* **Proxy IdP:** Pokročilý integrační mezičlánek (např. systém *Perun* nebo *Unity* v e-INFRA CZ), který vystupuje jako jednotná brána mezi stovkami externích federativních IdP a koncovými službami. Agreguje uživatelské identity, obohacuje je o specifické atributy a spravuje členství ve virtuálních organizacích (*VO*).
</details>

# Databáze

<details>
<summary>Ukládání dat</summary>

* **I/O úzké hrdlo (I/O Bottleneck):** Primární limit výkonu DBMS způsobený řádovým rozdílem rychlostí mezi operacemi v operační paměti RAM a sekundárním úložištěm. Architektura DB je navržena tak, aby minimalizovala diskové operace.
* **Stránky (Bloky):** Atomická jednotka přenosu dat mezi RAM a diskem. Hardwarová vrstva nedokáže efektivně číst/zapisovat jednotlivé bajty, proto komunikace probíhá vždy v ucelených blocích (typicky o velikosti $4\text{ KiB}$ až $8\text{ KiB}$).
* **Náhodný vs. Sekvenční přístup u HDD:** Mechanická konstrukce pevných disků (seek hlavy + rotační latence) způsobuje, že náhodný přístup (*Random I/O*) je až $300\times$ pomalejší než sekvenční přístup (*Sequential I/O*), u kterého se čtecí hlava přesune pouze jednou.
* **Out-of-place Updates (SSD):** Architektonická vlastnost NAND Flash pamětí, kde lze číst a zapisovat po stránkách, ale mazat pouze po celých blocích. Data nelze přepsat na stejném místě; nová verze se zapíše na čistou stránku a stará se označí za neplatnou.
* **Write Amplification (Zesílení zápisu):** Nežádoucí jev u SSD disků, kdy interní správa a čištění neplatných stránek (*Garbage Collection*) vyvolá přesuny dat na pozadí, v důsledku čehož disk fyzicky zapíše mnohonásobně více dat, než databáze reálně požadovala.
* **RAID konfigurace:** Sdružování disků do logických polí pro zvýšení rychlosti I/O a odolnosti. *RAID 0* (striping, čistý výkon bez redundance), *RAID 1* (mirroring, zrcadlení 1:1 ideální pro logy), *RAID 5* (distribuovaná parita, přežije výpadek 1 disku), *RAID 6* (duální parita, přežije výpadek 2 disků), *RAID 10* (kombinace 1+0, maximální výkon i spolehlivost).
* **Diff-RAID:** Specifický mechanismus pro SSD pole, který záměrně opotřebovává disky v poli nerovnoměrně, aby se předešlo situaci, kdy všechna SSD selžou v tentýž den kvůli simultánnímu dosažení limitu zápisů (*TBW*).
* **Dedikovaný disk pro logování:** Strategie striktního oddělení transakčního logu na fyzicky samostatný disk/pole od datových souborů. Log se zapisuje sekvenčně; pokud by sdílel disk s daty, hlavy by neustále přebíhaly mezi náhodným a sekvenčním I/O, což drasticky degraduje propustnost.
* **Controller Cache:** Hardwarová cache paměť RAID řadiče (často se záložní baterií), která dokáže optimalizovat a stírat negativní dopady nevhodné diskové konfigurace tím, že I/O operace řadí a slučuje přímo v paměti řadiče.
</details>

<details>
<summary>Adresování záznamů</summary>

* **Slotted-Page (Stránka se sloty):** Standardní fyzická organizace diskové stránky. Na začátku stránky se nachází adresář slotů rostoucí shora dolů (obsahuje dvojice `[ofset, délka]`), zatímco samotné datové řádky se ukládají od konce stránky odspoda nahoru.
* **Nepřímé adresování:** Výhoda architektury Slotted-Page. Externí struktury (indexy) neodkazují na absolutní fyzickou adresu řádku, ale na **ID slotu**. Pokud se řádek v rámci stránky posune, změní se pouze ofset v adresáři, ID slotu zůstává stejné a indexy netřeba aktualizovat.
* **Record ID (RID / ROWID):** Logicko-fyzický identifikátor řádku, který jednoznačně definuje umístění záznamu na disku. Skládá se z trojice: $\text{RID} = \langle \text{File ID}, \text{Page ID}, \text{Slot ID} \rangle$.
* **Heap File (Hromada):** Organizace souboru, kde se řádky ukládají na stránky v takovém pořadí, v jakém přicházejí, na libovolné volné místo. Zápis je extrémně rychlý $O(1)$, vyhledávání vyžaduje pomalý full table scan $O(N)$.
* **Sequential File (Sekvenční soubor):** Soubor udržovaný v permanentně setříděném pořadí podle vyhledávacího klíče. Vyhledávání je rychlé $O(\log N)$ pomocí binárního půlení intervalů nad bloky, ale zápis je extrémně drahý kvůli nutnosti fyzického přesunu dat.
* **Hashed File (Hašovaný soubor):** Organizace, kde je číslo cílové stránky (bucketu) pevně určeno matematickou funkcí: $\text{Číslo stránky} = h(\text{Klíč}) \bmod M$. Vyhledávání na přesnou shodu je okamžité $O(1)$, ale rozsahové dotazy jsou zcela nepoužitelné.
</details>

<details>
<summary>Indexování a hašování více atributů</summary>

* **B+ strom:** Vyvážený strom optimalizovaný pro diskové operace, kde jsou všechny datové ukazatele výhradně v listech a listy jsou obousměrně zřetězené. Podporuje bodové i rozsahové dotazy s komplexitou $O(\log N)$.
* **Složený index (Composite Index):** Spojení hodnot více atributů do jednoho klíče (např. `Příjmení + Jméno`) v rámci jednoho B+ stromu. Je efektivní pro dotazy na všechny zaindexované sloupce nebo na jejich **levou podmnožinu**. Index nad `(Příjmení, Jméno)` nepomůže dotazu cílícímu čistě na `Jméno`.
* **Kombinace samostatných indexů (Index Intersection):** Strategie, kdy databáze při dotazu nad více podmínkami použije dva nezávislé jednorozměrné indexy, paralelně vyhledá seznamy RID a následně v paměti provede množinový průnik (`AND`).
* **Dělené hašování (Partitioned Hashing):** Přístup pro více atributů, kde výsledná adresa bucketu vznikne bitovým zřetězením (spojením) výstupů samostatných hašovacích funkcí pro jednotlivé sloupce. Pokud dotaz specifikuje jen část klíčů, musí se prohledat všechny buckety odpovídající známému bitovému vzoru.
* **Mřížkový index (Grid Index):** Rozdělení datového prostoru do vícedimenzionální mřížky (matice), kde každá osa odpovídá jednomu atributu a buňky ukazují na příslušné produktové buckety. Výborné pro prostorové a rozsahové dotazy, ale trpí plýtváním místa při nerovnoměrném rozdělení dat.
* **Vektorové indexy (Vector Indexes):** Indexy pro AI a vyhledávání v embeddings (např. struktury HNSW - Hierarchical Navigable Small World nebo IVF - Inverted File) navržené pro rychlé přibližné vyhledávání nejbližších sousedů (ANN) ve vícedimenzionálních prostorech.
* **Selektivita indexu:** Poměr počtu unikátních hodnot k celkovému počtu řádků. Indexy mají smysl pouze pro vysoce selektivní sloupce (kde podmínka vybere malé procento řádků). U nízké selektivity (např. pohlaví) optimalizátor raději zvolí sekvenční table scan.
* **Pokrývající index (Covering Index):** Stav, kdy indexová struktura obsahuje úplně všechny sloupce požadované dotazem. Databáze v takovém případě přečte data přímo z paměti indexu a zcela vynechá drahý přístup k samotným datovým záznamům na disku.
</details>

<details>
<summary>Rastrové (bitmap) indexy</summary>

* **Rastrový (Bitmap) index:** Index reprezentující přítomnost či nepřítomnost konkrétní hodnoty pomocí sekvencí bitů (jedniček a nul) namísto klasických RID ukazatelů. Každému unikátnímu řádku odpovídá jedna bitová pozice ve vektoru.
* **Rychlost bitových operací:** Hlavní výhoda bitmap. Vyhledávání kombinovaných podmínek (`AND`, `OR`, `NOT`) probíhá extrémně rychle na hardwarové úrovni procesoru pouhým protnutím či sloučením bitových vektorů.
* **Vhodnost použití:** Ideální pro sloupce s **nízkou kardinalitou** (malý počet unikátních hodnot, napž. stav objednávky, pohlaví, kraj) a pro analytické systémy typu datových skladů (OLAP), kde převládá hromadné čtení nad zápisy.
* **Zamykání souběhu (Lock Contention):** Hlavní nevýhoda v transakčních systémech (OLTP). Modifikace byť jediného řádku vyžaduje aktualizaci bitmapového vektoru, což efektivně zamyká celý příslušný datový blok řádků a paralyzuje paralelní zápisy.
* **Run-Length Encoding (RLE) komprese:** Kompresní metoda bitmapových indexů, která namísto ukládání jednotlivých bitů zaznamenává délku nepřerušených úseků shodných hodnot (runů) zakončených jedničkou, což radikálně zmenšuje prostor na disku.
* **Unární kódování délky (Prefixové):** Mechanismus v RLE umožňující parseru správně oddělit komprimovaná čísla v nepřerušeném bitovém proudu. Před samotnou hodnotu se zapíše její bitová délka v unární soustavě zakončená nulou (např. délka 4 bity se zapíše jako `11110`).
* **Word Aligned Hybrid (WAH):** Pokročilý bitmapový kodek, který zarovnává komprimovaná data na velikost procesorového slova (32/64 bitů). Umožňuje provádět bitové operace přímo nad komprimovanými vektory bez nutnosti jejich dekomprimování v RAM.
</details>

<details>
<summary>Dynamické hašování</summary>

* **Dynamické hašování:** Třída hašovacích algoritmů navržená pro tabulky, jejichž objem dat se v čase výrazně mění, čímž předchází vzniku dlouhých a neefektivních přetékajících řetězců (*overflow chains*).
* **Rozšířitelné hašování (Extendible Hashing):** Využívá paměťový **adresář (directory)**, jehož velikost je vždy mocninou 2. Adresář obsahuje ukazatele na samotné datové buckety na disku. Indexace do adresáře probíhá přes prvních $i$ bitů (globální hloubka) z výstupu hash funkce.
* **Štěpení u rozšířitelného hašování:** Při přeplnění bucketu s lokální hloubkou $j$ se bucket rozštěpí na dva a lokální hloubka vzroste. Pokud platilo $j = i$ (lokální hloubka byla rovna globální), **velikost adresáře se zdvojnásobí** inkrementací globální hloubky $i$. Vyhledání záznamu trvá max. 2 I/O operace.
* **Lineární hašování (Linear Hashing):** Dynamické hašování, které **nepoužívá adresář**. Počet bucketů roste plynule a lineárně (přidáváním jednoho po druhém). Pro adresaci se využívá $i$ nejnižších (koncových) bitů adresy.
* **Asynchronní štěpení u lineárního hašování:** Rozhodnutí o štěpení se řídí globálním faktorem zaplnění celého prostoru (např. překročení 80 %). Při triggeru se rozštěpí bucket určený interním ukazatelem pointeru $P$ – ten se však často liší od bucketu, do kterého se právě zapisovalo, proto mohou dočasně vznikat krátké overflow bloky.
</details>

<details>
<summary>Vyhodnocování dotazu, algoritmy a odhady nákladů</summary>

* **Logický vs. Fyzický plán:** Logický plán je strom operátorů relační algebry definující *co* se má provést (např. Natural Join). Fyzický plán specifikuje konkrétní algoritmickou implementaci, tedy *jak* se to provede (např. Hash Join).
* **Cost-Based Optimizer (CBO):** Moderní optimalizátor, který generuje ekvivalentní prováděcí plány a na základě statistických metadat odhaduje jejich cenu v arbitrárních jednotkách (I/O operace + CPU cykly). Vybírá plán s nejnižší cenou.
* **Pipelining (Proudové zpracování):** Ideální režim exekuce, kdy si operátory ve stromu předávají mezivýsledky průběžně po jednotlivých řádcích přímo v paměti RAM bez nutnosti zápisu na disk.
* **Materializace a Pipeline Breakers:** Stav, kdy operátor musí kompletně zpracovat všechna data ze vstupu a zapsat celkový mezivýsledek do dočasné tabulky, než jej může předat dál. Operátory jako `ORDER BY`, `GROUP BY` nebo Build fáze `Hash Join` fungují jako bariéry proudového zpracování.
* **Externí třídění (External Merge Sort):** Třídicí algoritmus pro data, která se nevejdou do RAM ($M$ bloků). V první fázi se data po částech načtou, setřídí a zapíšou jako setříděné běhy (*runs*). V druhé fázi se runs paralelně slévají za využití $M-1$ vstupních bufferů.
* **Block Nested-Loop Join:** Spojení, kde pro každý diskový blok vnější tabulky načtený do RAM sekvenčně prohledáme celou vnitřní tabulku, což snižuje I/O náročnost oproti čistému řádkovému nested-loopu.
* **Indexed Nested-Loop Join:** Pokud má vnitřní tabulka index nad spojovacím sloupcem, neprochází se celá. Pro každý řádek z vnější tabulky se shoda vyhledá přímo přes index. Výhodné, pokud je vnější tabulka malá.
* **Sort-Merge Join:** Algoritmus, který obě relace nejprve samostatně setřídí podle spojovacího klíče a následně je paralelně prochází jedním lineárním průchodem, při kterém rovnou páruje shody.
* **Hash Join:** Nejejefektivnější join pro velké tabulky bez indexů. V *Build fázi* se nad menší tabulkou vybuduje v paměti RAM hashovací tabulka. V *Probe fázi* se sekvenčně čte větší tabulka a její klíče se okamžitě ověřují proti hashovací tabulce v RAM.
* **Metadata katalogu:** $T(R)$ je celkový počet řádků (kardinalita), $B(R)$ je počet diskových bloků, $V(R, A)$ je počet unikátních hodnot atributu $A$. Tyto statistiky doplňují frekvenční histogramy zachycující datové zešikmení (*skew*).
* **Odhad selektivity ($sf$):** Podíl řádků splňujících podmínku. Pro rovnost platí $sf = 1 / V(R, A)$. Pro konjunkci (`AND`) nezávislých podmínek se selektivity násobí: $sf(C_1 \land C_2) = sf(C_1) \cdot sf(C_2)$. Pro disjunkci (`OR`) platí princip inkluze a exkluze: $sf(C_1) + sf(C_2) - (sf(C_1) \cdot sf(C_2))$.
* **Odhad velikosti Natural Join:** Výpočet předpokládající rovnoměrné rozdělení, kde se velikost výsledku odhaduje na základě maximálního počtu unikátních hodnot společného atributu $A$: $T(R_1 \bowtie R_2) = \frac{T(R_1) \cdot T(R_2)}{\max\{V(R_1, A), V(R_2, A)\}}$.
</details>

<details>
<summary>Optimalizace dotazů, schémat a pravidla transformace</summary>

* **Normalizace vs. Denormalizace:** Normalizace (až do BCNF) odstraňuje redundanci a anomálie rozdělením dat do více tabulek, což ale vynucuje drahé `JOIN` operace. Denormalizace záměrně porušuje normální formy (duplikuje data) pro zrychlení kritických dotazů za cenu vyšších nároků na zápis.
* **Clusterované ukládání (Clustered Storage):** Fyzické ukládání záznamů ze dvou různých tabulek k sobě na disku na základě společného klíče (např. řádky položek objednávky přímo za konkrétní hlavičku objednávky), což umožňuje bleskové čtení bez logického joinování.
* **Materializovaný pohled (Materialized View):** Pohled, jehož výsledná data jsou fyzicky vypočtena a uložena jako tabulka na disku. Urychluje čtení; optimalizátor umí přepsat původní složitý dotaz přímo na materializovaný pohled (*Query Rewrite*).
* **Komutativita a Asociativita operátorů:** Logická pravidla umožňující optimalizátoru libovolně měnit pořadí provádění operací ve stromu dotazu (platí pro Natural Join, Kartézský součin i Sjednocení), aniž by se změnil výsledek: $R \bowtie S = S \bowtie R$ a $(R \bowtie S) \bowtie T = R \bowtie (S \bowtie T)$.
* **Včasná selekce (Pushing Selections Down):** Zásadní heuristika optimalizace. Přesunutí filtračního operátoru $\sigma$ hlouběji do stromu (přímo ke zdrojovým tabulkám ještě před provedením drahého spojení $\bowtie$). Tím se radikálně zmenší objem dat putujících do joinu: $\sigma_{p}(R \bowtie S) = [\sigma_{p}(R)] \bowtie S$.
* **Včasná projekce (Pushing Projections Down):** Heuristika odřezávání nepotřebných sloupců $\pi$ co nejdříve v exekučním stromu, což zúží šířku řádků a šetří paměťové buffery. Při posunu pod join se však v projekci musí povinně zachovat spojovací atributy $Z$: $\pi_{XY}(R \bowtie S) = \pi_{XY}([\pi_{X \cup Z}(R)] \bowtie [\pi_{Y \cup Z}(S)])$.
</details>

<details>
<summary>Rozdělování dat</summary>

* **Horizontální dělení (Horizontal Partitioning):** Distribuce řádků jedné logické tabulky do více samostatných fyzických particií na základě klíče (rozsah, seznam nebo hash hodnot).
* **Prořezávání particií (Partition Pruning):** Klíčový výkonnostní benefit horizontálního dělení. Pokud dotaz specifikuje podmínku na klíč particie, optimalizátor zcela ignoruje nerelevantní particie a fyzicky čte pouze ty bloky, kde se data mohou nacházet.
* **Vertikální dělení (Vertical Partitioning):** Rozdělení jedné logické tabulky na více fyzických tabulek (vztah 1:1) podle sloupců. Používá se pro separaci často dotazovaných úzkých sloupců od zřídka čtených širokých dat (např. odeslání těžkých textových popisů či `BLOB`ů na samostatnou strukturu).
* **Vertikální replikace (Antipartitioning):** Strategie záměrného zavedení redundance duplikováním několika málo klíčových atributů z jedné tabulky do druhé (např. zkopírování aktuální ceny přímo k detailu akcie), čímž se eliminují drahé `JOIN` operace u nejčastějších dotazů.
</details>

<details>
<summary>Ladění dotazů a schématu</summary>

* **EXPLAIN:** Diagnostický SQL příkaz, který zobrazí interní exekuční plán vygenerovaný optimalizátorem (zvolené algoritmy, index scan vs. table scan, odhadované ceny a kardinality), sloužící jako výchozí bod ladění.
* **Věta o duplicitách a DISTINCT:** `DISTINCT` vynucuje drahé řazení či hašování v paměti. Je nadbytečný, pokud je každá tabulka v dotazu privilegovaná (výběr obsahuje její primární klíč) nebo pokud neprivilegovaná tabulka lineárně "dosáhne" na primární klíč tabulky privilegované.
* **Zneplatnění indexu funkcí:** Častá chyba vývojářů. Pokud je indexovaný sloupec v klauzuli `WHERE` obalen funkcí nebo matematickou operací (např. `WHERE salary/12 >= 4000` nebo `WHERE SUBSTR(name, 1, 1) = 'G'`), optimalizátor nedokáže index použít. Výraz je nutné izolovat (`WHERE salary >= 48000`).
* **Sargable podmínky (Search Argumentable):** Podmínky v dotazu zapsané tak, aby mohl prováděcí engine přímo provést efektivní index scan (vyhledávání ve stromu). Např. `LIKE 'G%'` je sargable, ale `LIKE '%G'` není, protože vynucuje scan celého indexu.
* **Nevhodné umístění HAVING:** Klauzule `HAVING` filtruje data až *po* agregaci. Pokud podmínka neobsahuje agregační funkci, musí být striktně přesunuta do `WHERE`, aby se snížil počet řádků vstupujících do drahého operátoru `GROUP BY`.
</details>

<details>
<summary>Zpracování transakcí</summary>

* **ACID:** Čtyři základní pilíře garantující spolehlivost transakcí. *Atomicita* (všechno nebo nic), *Konzistence* (přechod z jednoho platného stavu do druhého), *Izolovanost* (paralelní běh nemá vliv na korektnost), *Trvanlivost* (potvrzené změny přežijí i pád systému).
* **Two-Phase Locking (2PL):** Protokol řízení souběhu zaručující serializovatelnost. Má *růstovou fázi* (transakce zámky pouze získává, nesmí žádný uvolnit) a *smršťovací fázi* (zámky se pouze uvolňují, nelze získat nový).
* **Dirty Read anomálie:** Jev, kdy transakce $T_1$ čte data, která byla modifikována transakcí $T_2$, ale ještě nebyla potvrzena (`COMMIT`). Pokud $T_2$ provede `ROLLBACK`, $T_1$ pracovala s neexistujícími daty.
* **Non-repeatable Read anomálie:** Situace, kdy transakce $T_1$ načte řádek, transakce $T_2$ tento řádek změní a potvrdí, a následné opakované čtení téhož řádku v rámci $T_1$ vrátí jinou hodnotu.
* **Phantom Read anomálie:** Stav, kdy transakce $T_1$ provede rozsahový čtenářský dotaz (např. počet zaměstnanců), transakce $T_2$ vloží do daného rozsahu nový řádek a potvrdí jej. Opakovaný dotaz v $T_1$ pak vrátí "přízračný" nový záznam.
* **Úrovně izolace (SQL Standard):** *Read Uncommitted* (povoluje všechny anomálie), *Read Committed* (brání Dirty Read, standard u mnoha DB), *Repeatable Read* (brání Non-repeatable Read, drží zámky do konce transakce), *Serializable* (nejvyšší, stoprocentní izolace bránící všem anomáliím ekvivalentem sériového řazení).
* **Sekání transakcí (Chopping):** Technika ladění transakcí, kdy se dlouhá komplexní transakce rozdělí na několik kratších izolovaných transakcí za předpokladu, že paralelní operace přistupují k jednotlivým prvkům nezávisle, což minimalizuje dobu držení zámků.
</details>

<details>
<summary>Výpadky a zotavení</summary>

* **Write-Ahead Logging (WAL):** Kardinální pravidlo trvanlivosti: Žádný modifikovaný (špinavý) datový blok z RAM bufferu nesmí být zapsán do primárních souborů na disku dříve, než je příslušný záznam o této změně bezpečně synchronizován a zapsán v transakčním logu.
* **Politika Steal vs. No-Steal:** *Steal* dovoluje správci bufferu zapsat na disk stránku změněnou dosud nepotvrzenou transakcí (vyžaduje fázi **UNDO** při pádu). *No-Steal* zakazuje zápis necommitovaných dat na disk (při pádu netřeba UNDO, ale limituje kapacitu RAM).
* **Politika Force vs. No-Force:** *Force* vyžaduje, aby při commitu byla všechna změněná data okamžitě zapsána do datových souborů (velmi pomalé, netřeba REDO). *No-Force* dovoluje potvrdit transakci pouze zápisem do logu, data se zapíší asynchronně později (rychlé, vyžaduje fázi **REDO** při pádu).
* **Kombinace Steal / No-Force:** Nejrychlejší možný produkční režim správy bufferu. Poskytuje paměti absolutní svobodu pro asynchronní dávkové zápisy na pozadí, avšak proces zotavení po havárii musí povinně implementovat fázi **REDO i UNDO**.
* **UNDO vs. REDO Logování:** *UNDO log* obsahuje staré hodnoty a při pádu prochází log od konce dozadu a maže změny nedokončených transakcí. *REDO log* obsahuje nové hodnoty, prochází log od začátku dopředu a znovu vynucuje změny potvrzených transakcí.
* **Algoritmus ARIES:** Standard pro moderní UNDO/REDO zotavení. Každá operace v logu má číslo LSN (Log Sequence Number) a datové stránky mají políčko `pageLSN` zajišťující idempotenci (ochranu před duplicitním zápisem téže změny).
* **Tři fáze zotavení ARIES:** 1. *Analytická fáze* (prochází log dopředu, identifikuje aktivní transakce - *losers* a špinavé stránky v momentě pádu), 2. *Fáze REDO* (prochází log dopředu a zopakuje úplně všechny změny z historie do stavu před pádem), 3. *Fáze UNDO* (prochází log pozpátku a rollbuje změny všech loser transakcí).
* **Compensation Log Record (CLR):** Speciální dopředný záznam zapisovaný do logu během fáze UNDO za každou úspěšně vrácenou operaci. Pokud systém zkolabuje uprostřed samotného zotavování, díky CLR ví, co už jednou vrátil a netřeba to rollbovat podruhé.
* **Fuzzy Checkpoint:** Periodický kontrolní bod, který neblokuje běžící transakce. Na pozadí asynchronně splachuje špinavé stránky na disk a do logu zapíše tabulku aktuálně aktivních transakcí, čímž radikálně zkracuje délku logu, kterou je nutné při pádu analyzovat.
</details>

<details>
<summary>Bezpečnost a přístupová oprávnění</summary>

* **Diskreční řízení přístupu (DAC):** Bezpečnostní model, kde vlastník/tvůrce datového objektu má plnou diskréční pravomoc rozhodovat o tom, kterým dalším uživatelům udělí nebo odebere přístupová práva.
* **GRANT a WITH GRANT OPTION:** SQL příkaz pro přidělení práv (`SELECT`, `INSERT` atd.) nad objektem. Doložka `WITH GRANT OPTION` dává příjemci legální právo toto oprávnění distribuovat a delegovat dále na další subjekty.
* **REVOKE a kaskádový efekt:** Příkaz pro odnětí dříve udělených oprávnění. Pokud je právo odebráno uživateli, který jej distribuoval dál přes grant option, systém provede kaskádové (řetězové) stažení práv u všech takto ovlivněných sub-uživatelů.
* **Řízení přístupu na základě rolí (RBAC):** Pokročilý model, kde se oprávnění neváží na konkrétní uživatele, ale na logické **role** reprezentující pracovní funkce v organizaci (např. role `účetní`). Uživatelům jsou následně tyto role pouze přiřazovány, což usnadňuje audit management.
* **Row-Level Security (RLS):** Dynamické zabezpečení na úrovni jednotlivých řádků. Databázový engine na pozadí automaticky modifikuje uživatelský SQL dotaz o bezpečnostní predikát (např. obchodník uvidí pouze řádky, kde figuruje jeho ID), čímž filtruje citlivá data bez nutnosti úpravy schématu.
</details>


# Neuronové sítě


<details>
<summary>Vícevrstvé sítě a jejich výrazové schopnosti</summary>

* **Vícevrstvý perceptron (Multilayer Perceptron — MLP):** Dopředná (*feed-forward*) neuronová síť obsahující vstupní vrstvu, jednu nebo více skrytých vrstev a výstupní vrstvu. Tok informací probíhá striktně jedním směrem bez cyklů.
* **Nelineární aktivační funkce:** Nezbytná komponenta každého neuronu, která transformuje vážený součet vstupů $z = \sum w_i x_i + b$. Bez nelinearity by se celá síť (bez ohledu na počet vrstev) chovala pouze jako jediná složená lineární transformace a nedokázala by řešit komplexní problémy.
* **Klíčové aktivační funkce:**
  * **ReLU (Rectified Linear Unit):** Definována jako $\max(0, x)$. Nejpoužívanější funkce ve skrytých vrstvách; díky konstantní derivaci pro kladné hodnoty radikálně omezuje problém mizejícího gradientu.
  * **Sigmoida a Tanh:** Saturační funkce mapující výstup do intervalu $(0, 1)$, respektive $(-1, 1)$. Jsou náchylné k saturaci – pro velmi vysoké či nízké vstupy je jejich derivace téměř nulová, což zastavuje učení.
  * **Softmax:** Aplikuje se výhradně ve výstupní vrstvě pro klasifikaci do více tříd. Převádí výstupní hodnoty (logity) na pravděpodobnostní rozdělení, kde součet všech výstupů je přesně roven 1.
* **Problém lineární separability (XOR):** Jednoduchý jednovrstvý perceptron nedokáže vyřešit logickou funkci XOR, protože její stavy nelze v prostoru oddělit jedinou přímkou (nadrovinou). MLP se skrytou vrstvou tento limit překonává tím, že vstupní prostor nelineárně deformuje a transformuje body tak, aby se staly lineárně separabilními.
* **Univerzální aproximační věta:** Zásadní teoretický teorém matematicky dokazující, že dopředná neuronová síť s **jedinou skrytou vrstvou** (obsahující dostatečný, potenciálně obrovský počet neuronů) a libovolnou nelineární aktivační funkcí dokáže s libovolnou přesností aproximovat jakoukoli spojitou funkci na kompaktní množině.
* **Hloubka vs. šířka architektury:** Ačkoliv teoreticky stačí jedna široká vrstva, v reálné praxi jsou hluboké sítě (mnoho vrstev za sebou) řádově efektivnější. Umožňují hierarchickou extrakci rysů – první vrstvy detekují triviální koncepty (hrany, textury), střední vrstvy tvary a koncové vrstvy ucelené sémantické objekty.
</details>

<details>
<summary>Učení neuronových sítí a gradientní sestup</summary>

* **Učení neuronové sítě:** Matematický proces optimalizace, jehož cílem je najít takovou konfiguraci matic vah $w$ a biasů $b$, která minimalizuje hodnotu chybové funkce (Loss function $E$) na trénovací množině.
* **Gradientní sestup (Gradient Descent):** Iterativní optimalizační algoritmus pro vyhledávání lokálního minima chybové funkce. Využívá faktu, že gradient $\nabla E(w)$ je vektor ukazující směr nejstrmějšího růstu funkce; pohyb proti směru gradientu proto vede k jejímu poklesu.
* **Rychlost učení (Learning Rate — $\eta$):** Klíčový hyperparametr určující délku kroku proti směru gradientu. Příliš vysoký $\eta$ způsobuje oscilace a divergenci algoritmu (přestřelování minima); příliš nízký $\eta$ vede k extrémně pomalé konvergenci nebo uvíznutí v mělkých lokálních minimech.
* **Varianty gradientního sestupu:**
  * **Batch Gradient Descent:** Spočítá přesný gradient chybové funkce z kompletní trénovací množiny najednou. Je stabilní, ale výpočetně netraktovatelný pro velká data.
  * **Stochastic Gradient Descent (SGD):** Aktualizuje váhy na základě gradientu vypočítaného z každého **jednotlivého** náhodného trénovacího vzorku. Je extrémně rychlý, ale trajektorie k minimu je silně zašuměná.
  * **Mini-batch Gradient Descent:** Zlatý standard. Gradient se počítá ze small-batch skupin vzorků (typicky velikosti 32 až 256). Spojuje výpočetní stabilitu batch verze s rychlostí a stochastickou regularizací SGD.
* **Nekonvexní chybová funkce:** U vícevrstvých sítí je chybový povrch vysoce komplexní a obsahuje obrovské množství lokálních minim a sedlových bodů (oblasti s nulovým gradientem, kde se učení klasického SGD zastaví).
</details>

<details>
<summary>Algoritmus zpětné propagace (Backpropagation)</summary>

* **Zpětná propagace (Backpropagation):** Vysoce efektivní algoritmus určený pro exaktní výpočet parciálních derivací chybové funkce vzhledem ke všem jednotlivým váhám v síti ($\frac{\partial E}{\partial w_{ij}}$). 
* **Řetězové pravidlo (Chain Rule):** Matematický základ backpropagation. Vzhledem k tomu, že neuronová síť je de facto kompozicí mnoha složených funkcí, parciální derivace složeného řetězce se vypočítá jako součin parciálních derivací jednotlivých mezikroků.
* **Forward Pass (Dopředný průchod):** První fáze algoritmu, kdy trénovací vzorek projde sítí od vstupu k výstupu. Vypočítají se a v paměti RAM uchovají aktivace ($y$) všech neuronů a výsledná celková chyba $E$.
* **Backward Pass (Zpětný průchod):** Druhá fáze, kdy se chybový signál ($\delta$) propaguje zpětně od výstupní vrstvy směrem ke vstupu. Chyba na výstupu je definována jako $\delta_j^{(L)} = \frac{\partial E}{\partial y_j} \cdot \sigma'(z_j^{(L)})$.
* **Propagace chyby do skryté vrstvy:** Chybový signál skrytého neuronu se vypočítá jako vážený součet chybových signálů neuronů z následující vrstvy vynásobený derivací vlastní aktivační funkce: $\delta_i^{(l)} = \sigma'(z_i^{(l)}) \sum_j w_{ij}^{(l+1)} \delta_j^{(l+1)}$.
* **Výpočet výsledného gradientu váhy:** Gradient konkrétní váhy se určí jednoduše jako součin aktivace vystupující z předchozího neuronu a chybového signálu cílového neuronu: $\frac{\partial E}{\partial w_{ij}^{(l)}} = y_i^{(l-1)} \delta_j^{(l)}$.
* **Výpočetní efektivita:** Díky uložení mezivýsledků z forward passu stačí pro výpočet gradientů celého modelu s miliony parametrů pouhý **jediný zpětný průchod**, což radikálně snižuje komplexnost výpočtu.
</details>

<details>
<summary>Praktické otázky učení (Příprava dat, inicializace vah, volba a adaptace hyperparametrů)</summary>

* **Z-score normalizace dat:** Proces transformace vstupních dat odečtením průměru a vydělením směrodatnou odchylkou pro každý příznak zvlášť ($\mu=0, \sigma^2=1$). Pokud mají vstupy různé rozsahy, chybový povrch je extrémně protáhlý, což nutí gradientní sestup neefektivně oscilovat. Normalizace zajišťuje sférický tvar chybové funkce a zrychluje konvergenci.
* **Rozdělení dat:** Striktní separace na množinu *Trénovací* (výpočet gradientů a úprava vah), *Validační* (ladění hyperparametrů a monitorování přeučení) a *Testovací* (finální objektivní validace kvality generalizace modelu).
* **Narušení symetrie (Breaking the Symmetry):** Váhy sítě nesmí být nikdy inicializovány na shodné hodnoty (např. na samé nuly). V takovém případě by všechny neurony v dané vrstvě generovaly identické gradienty, učily by se naprosto stejně a celá vrstva by degenerovala na chování jediného neuronu.
* **Xavier (Glorot) inicializace:** Metoda inicializace vah pro symetrické saturační funkce (sigmoida, tanh). Váhy se náhodně losují z distribuce s nulovým průměrem a rozptylem inverzně proporčním k počtu vstupních a výstupních neuronů dané vrstvy: $\text{Var}(W) = \frac{2}{\text{in} + \text{out}}$. Udržuje konstantní rozptyl signálu napříč vrstvami.
* **He inicializace:** Optimalizovaná inicializace vah pro sítě využívající funkci **ReLU**. Zohledňuje fakt, že ReLU polovina záporného signálu zcela nuluje, a proto kompenzačně zvyšuje rozptyl počátečních vah: $\text{Var}(W) = \frac{2}{\text{in}}$. Zabraňuje mizejícímu gradientu v hlubokých architekturách.
* **Pokročilé adaptivní optimizátory:**
  * **Momentum (Setrvačnost):** Akumuluje předchozí směry posunů vah. Pomáhá potlačovat oscilace a dodává algoritmu kinetickou energii pro rychlé překonání plochých sedlových bodů.
  * **RMSProp / AdaGrad:** Adaptivně škálují rychlost učení pro každý parametr nezávisle. Váhy s historicky obrovskými gradienty jsou penalizovány zpomalením kroku, zatímco váhy se vzácnými drobnými gradienty dostávají prostor pro větší krok.
  * **Adam (Adaptive Moment Estimation):** Kombinuje výhody momenta (analýza prvního momentu gradientu) a RMSProp (analýza druhého momentu gradientu). Aktuálně nejstabilnější a nejuniverzálnější optimizátor pro hluboké učení.
</details>

<details>
<summary>Regularizace (Omezení přeučení)</summary>

* **Přeučení (Overfitting):** Stav, kdy model perfektně memoruje trénovací data včetně jejich specifického náhodného šumu, ale ztrácí schopnost správně generalizovat a predikovat nad novými, doposud neviděnými vzorky (vysoký rozptyl / high variance).
* **Penalizace norem (Weight Decay):** Regularizační technika modifikující chybovou funkci přidáním penalizačního členu, který znevýhodňuje příliš velké hodnoty vah v duchu Occamovy břitvy.
  * **L2 regularizace:** K chybové funkci přičítá člen $\frac{\lambda}{2} \sum w^2$. Nutí váhy konvergovat k malým hodnotám, což zajišťuje, že se výstup sítě mění hladce a lineárně bez strmých výkyvů při drobných změnách na vstupu.
  * **L1 regularizace:** Přičítá člen $\lambda \sum |w|$. Vede k tzv. **řídkým maticím vah (sparsity)**, kde je mnoho parametrů vytlačeno na absolutní nulu. Funguje jako vestavěný automatický selektor relevantních příznaků.
* **Dropout:** Vysoce efektivní regularizační metoda, která v každém kroku trénovací iterace náhodně a dočasně deaktivuje (vynuluje) stanovené procento neuronů (např. 30 %) v dané vrstvě. Zabraňuje ko-adaptaci neuronů (situace, kdy se jeden neuron slepě spoléhá na specifický výstup jiného) a vynucuje robustní distribuované učení rysů. Při inferenci jsou aktivní všechny neurony, ale jejich výstup je škálován pravděpodobností zapnutí.
* **Včasné ukončení (Early Stopping):** Algoritmická regularizace monitorující trénovací a validační chybu paralelně. V momentě, kdy trénovací chyba dál klesá, ale validační chyba začne permanentně stoupat, se proces učení okamžitě zastaví a síť se navrátí ke stavu s nejnižší validační chybou.
* **Rozšiřování dat (Dataset Augmentation):** Umělé navyšování rozsahu trénovací množiny generováním modifikovaných variant stávajících dat pomocí geometrických či senzorických transformací (rotace, ořezy, změny jasu, vkládání šumu). Učí model invariantnosti vůči těmto transformacím.
</details>

<details>
<summary>Konvoluční neuronové sítě (CNN)</summary>

* **Konvoluční sítě (CNN):** Specializované dopředné sítě navržené pro efektivní zpracování dat s fixní mřížkovou topologií (např. 2D digitální obrazy, audio spektrogramy).
* **Lokální receptivní pole (Lokální konektivita):** Na rozdíl od plně propojených vrstvách (fully connected) jsou neurony v konvoluční vrstvě napojeny pouze na malou prostorovou sub-oblast předchozí vrstvy, což radikálně redukuje celkové množství parametrů sítě a reflektuje lokální závislosti v obraze.
* **Sdílení vah (Shared Weights):** Princip, kdy se jeden matematický filtr (jádro / kernel o rozměrech např. $3 \times 3$ nebo $5 \times 5$) posouvá a aplikuje na úplně všechna místa vstupního obrazu. Detektor určitého rysu (např. hrany) naučený v jedné části obrazu je tak automaticky schopen stejný rys detekovat kdekoliv jinde.
* **Feature Map (Mapa rysů):** Dvourozměrný výstup vzniklý aplikací (konvolucí) jednoho konkrétního filtru napříč celým obrazem. Počet filtrů v konvoluční vrstvě přímo definuje hloubku (počet kanálů) výsledné feature mapy.
* **Matematická operace konvoluce:** Hodnota buňky na pozici $(i, j)$ se počítá jako diskrétní konvoluce filtru se vstupem: $y_{i,j} = \sigma \left( \sum_{m} \sum_{n} w_{m,n} \cdot x_{i+m, j+n} + b \right)$.
* **Hyperparametry konvoluce:**
  * **Stride (Krok):** Velikost posunu filtru při skenování obrazu. Stride 1 znamená posun o 1 pixel; vyšší stride (např. 2) prostorově zmenšuje výstupní mapu.
  * **Padding (Zarovnání):** Doplnění vnějších okrajů vstupu (typicky nulami — *zero padding*). Umožňuje aplikovat filtr i na okrajové pixely a brání nechtěnému smršťování rozměrů mapy v hlubokých sítích.
* **Vrstva sdružování (Pooling Layer):** Vkládá se mezi konvoluční bloky za účelem downsamplingu – snižování prostorových rozměrů (šířky a výšky) feature map. Redukuje výpočetní náročnost a vnáší do systému translační invarianci.
  * **Max Pooling:** Skenuje mapu a z daného okna (např. $2 \times 2$) vybere striktně nejvyšší hodnotu (nejsilnější vizuální aktivaci).
  * **Average Pooling:** Vypočítá aritmetický průměr hodnot v okně.
* **Hierarchická architektura CNN:** Spodní konvoluční vrstvy extrahují elementární nízkoúrovňové rysy (hrany, rohy, barevné přechody). Hlubší vrstvy tyto mapy matematicky kombinují do komplexnějších geometrických textur a tvarů. Na samém konci sítě se feature mapy zploští (*flatten*) do jednoho vektoru a projdou klasickou plně propojenou vrstvou pro finální klasifikaci.
</details>

<details>
<summary>Rekurentní neuronové sítě (RNN)</summary>

* **Rekurentní sítě (RNN):** Třída neuronových sítí specializovaná na zpracování sekvenčních dat s časovou či poziční závislostí (textové věty, audio nahrávky, časové řady, senzory), kde délka vstupu či výstupu může být variabilní.
* **Skrytý stav (Hidden State — $h_t$):** Interní vektorová paměť sítě. V každém časovém kroku $t$ se aktuální skrytý stav vypočítá jako kombinace aktuálního vstupu $x_t$ a skrytého stavu z předchozího kroku $h_{t-1}$: $h_t = \sigma(W_h h_{t-1} + W_x x_t + b)$.
* **Sdílení parametrů v čase:** Matice vah $W_h$ a $W_x$ jsou striktně **shodné a sdílené** napříč všemi časovými kroky sekvence. To umožňuje modelu nacházet a zobecňovat identické sekvenční vzory bez ohledu na to, v jaké fázi (čase) se v datech objeví.
* **Rozvinutí v čase (Unrolling):** Teoretická transformace rekurentního grafu na ekvivalentní strukturu dopředné sítě, kde každý časový krok představuje jednu vrstvu a všechny tyto vrstvy sdílejí identické váhy. Umožňuje aplikaci standardních optimalizačních technik.
* **Backpropagation Through Time (BPTT):** Algoritmus učení RNN nad rozvinutým grafem. V dopředném směru projde celá sekvence a spočítá se akumulovaná chyba. Ve zpětném směru se chybový signál propaguje od konce sekvence chronologicky zpět k počátku. Výsledný gradient pro každou sdílenou váhu je sumou gradientů ze všech časových kroků.
* **Problém mizejícího a explodujícího gradientu:** Při zpracování dlouhých sekvencí dochází v algoritmu BPTT k opakovanému sekvenčnímu násobení stejnou maticí vah $W_h$. Pokud jsou vlastní čísla matice menší než 1, gradient exponenciálně degraduje k nule (mizející gradient), což síť paralyzuje a znemožňuje jí učit se dlouhodobé závislosti. Pokud jsou větší než 1, gradient nekontrolovaně naroste (explodující gradient, řeší se osekáním — *gradient clipping*).
* **Hradlové mechanismy (Gates):** Pokročilé architektury navržené pro potlačení mizejícího gradientu zavedením vnitřních hradel, která selektivně řídí tok a perzistenci informací v paměti.
  * **LSTM (Long Short-Term Memory):** Zavádí dedikovanou stavovou buňku (*Cell State*), která funguje jako lineární datová dálnice, po které může gradient propadat bez exponenciálních ztrát. Tok řídí tři hradla: *Forget Gate* (kolik staré paměti zapomenout), *Input Gate* (co nového do paměti uložit) a *Output Gate* (co ze stavu propustit na aktuální výstup).
  * **GRU (Gated Recurrent Unit):** Výpočetně odlehčená varianta LSTM. Slučuje stavovou buňku a skrytý stav do jediného vektoru a kombinuje vstupní a zapomínací hradlo do jednoho integrovaného aktualizačního hradla (*Update Gate*), což snižuje počet parametrů a urychluje trénink.
</details>

# Strojové učení


<details>
<summary>Základy strojového učení (Supervizované, semi-supervizované a nesupervizované učení)</summary>

* **Strojové učení:** Schopnost počítačových systémů induktivně optimalizovat svůj výkon při řešení zadané úlohy na základě zkušenosti (dat), aniž by byly pro tuto úlohu explicitně naprogramovány.
* **Supervizované učení (Učení s učitelem):** Proces učení nad trénovací množinou kompletních párů $(x, y)$, kde $x$ je vstupní vektor příznaků a $y$ je známý cílový štítek (label). Algoritmus iterativně minimalizuje ztrátovou funkci měřící odchylku predikce $\hat{y}$ od skutečnosti $y$.
* **Generalizace vs. Overfitting:** Generalizace je schopnost modelu podávat stabilní a přesné predikce nad novými, doposud neviděnými testovacími daty. Overfitting (přeučení) nastává, pokud model slepě memoruje trénovací data včetně jejich specifického náhodného šumu (nízká chyba na trénovacích datech, obrovská chyba na validačních).
* **Semi-supervizované učení:** Kompromisní přístup kombinující malé množství lidsky anotovaných (označených) dat s obrovským objemem dat neoznačených. Staví na shlukovací hypotéze (body v těsném sémantickém shluku sdílejí stejný label) a technikách jako *Self-training*, což drasticky snižuje náklady na ruční anotaci.
* **Nesupervizované učení (Učení bez učitele):** Analýza dat, která neobsahují žádné labely. Model se snaží autonomně odhalit vnitřní geometrickou strukturu, hierarchické vztahy, hustotu pravděpodobnosti nebo skryté vazby v datech.
* **Základní operace nesupervizovaného učení:**
  * **Clustering (Shlukování):** Matematické seskupování objektů do separátních shluků na základě jejich vzájemné podobnosti/vzdálenosti v prostoru (např. algoritmus $K$-means).
  * **Redukce dimenzionality:** Projektování vysokorozměrných dat do podprostoru s nižším počtem proměnných při zachování maxima původní variability a informace (např. PCA), čímž se potlačuje "prokletí dimenzionality".
  * **Odhad hustoty pravděpodobnosti:** Statistické modelování rozdělení, ze kterého data pocházejí (např. *Gaussian Mixture Models — GMM*), pro stanovení pravděpodobnosti výskytu konkrétních jevů.
  * **Asociační pravidla:** Objevování skrytých vazeb a podmíněných pravděpodobností výskytu prvků ve velkých transakčních databázích (např. algoritmus *Apriori*).
</details>

<details>
<summary>Operace klasifikace, regrese a detekce anomálií</summary>

* **Klasifikace:** Proces predikce diskrétní (kategorické) hodnoty. Cílem je nalézt optimální rozhodovací hranici (*Decision Boundary*), která v prostoru příznaků odděluje jednotlivé třídy. Rozlišuje se binární (2 třídy), vícetřídní (*multiclass* — $K$ tříd) a víceštítková (*multilabel* — jeden objekt patří do více nezávislých kategorií současně). Hodnotí se pomocí metrik jako *Accuracy*, *Precision*, *Recall* a *F1-score*.
* **Regrese:** Předpovídání spojitých číselných hodnot proložením optimální křivky či nadroviny, která minimalizuje sumu čtverců odchylek (např. *Mean Squared Error — MSE*). Výstupem může být nekonečně mnoho hodnot v daném intervalu.
* **Klíčové typy regresních modelů:**
  * **Lineární regrese:** Předpokládá striktně lineární vztah mezi prediktory a cílem, parametry hledá metodou nejmenších čtverců.
  * **Polynomiální regrese:** Zahrnuje vyšší mocniny proměnných pro zachycení nelineárních závislostí za cenu zvýšeného rizika overfittingu.
  * **Logistická regrese:** Matematicky regresní model, který se však v praxi používá výhradně pro **binární klasifikaci**. Obaluje lineární kombinaci sigmoidální funkcí a predikuje hodnotu v intervalu $(0, 1)$, interpretovanou jako pravděpodobnost příslušnosti ke třídě.
  * **Komplexní regrese:** Nelineární metody využívající robustní algoritmy jako *Support Vector Regression (SVR)*, regresní stromy, náhodné lesy nebo hluboké neuronové sítě.
* **Detekce anomálií (Outlier Detection):** Identifikace specifických vzorků, které se zásadně odlišují od běžného vzorce chování zbytku datové distribuce.
* **Typy anomálií:**
  * **Kontextuální anomálie:** Bod vybočuje z normálu pouze vázán na specifický kontext (např. vysoká teplota prostředí v zimním období).
  * **Kolektivní anomálie:** Sekvence bodů vykazuje anomální charakter pouze tehdy, pokud se vyskytnou hromadně a v daném pořadí, přestože izolovaně jsou v normálu (např. kybernetický útok složený z legitimních systémových volání).
</details>

<details>
<summary>Učení metrik (Metric Learning, kontrastivní a triplet-loss učení)</summary>

* **Učení metrik:** Proces transformace dat neuronovou sítí, kde cílem není zařadit objekt do fixní třídy, ale naučit se obecné relaci podobnosti. Výsledný model mapuje data na numerické vektory (**embeddingy**), kde geometrická vzdálenost v prostoru přímo odpovídá sémantické podobnosti objektů. Exceluje v úlohách s chudými daty a obřím počtem tříd (*Few-shot learning*), jako je rozpoznávání tváří.
* **Kontrastivní učení (Contrastive Learning):** Široké trénovací paradigma, které namísto klasického softmaxu učí model rozpoznávat sémantické rozdíly porovnáváním vzorků mezi sebou (řeší otázku: "Patří tyto dva vzorky k sobě, nebo jsou odlišné?").
* **Směry nesupervizovaného kontrastivního učení:**
  * **SimCLR:** Aplikuje na jeden obrázek dvě náhodné augmentace pro vytvoření pozitivního páru, zatímco ostatní obrázky v batchi fungují jako negativní příklady.
  * **MoCo (Momentum Contrast):** Zefektivňuje výpočet nahrazením obřích batchů dynamickou frontou negativních vzorků, což redukuje hardwarové nároky.
  * **BYOL (Bootstrap Your Own Latent):** Kontrastivní učení zcela bez negativních vzorků; online síť se učí predikovat reprezentaci cílové sítě aktualizované klouzavým průměrem vah.
  * **DINO:** Staví na self-distilaci Student-Teacher a maskování patchů; učí se vizuální reprezentace čistě ze surových pixelů bez textu, přičemž přirozeně chápe geometrii a segmentaci scény.
  * **MAE (Masked Autoencoders):** Maskuje až 75 % patchů obrazu a Vision Transformer se je snaží rekonstruovat, čímž se nuceně učí sémantické struktuře.
* **Pairwise kontrastivní loss:** Ztrátová funkce operující nad dvojicemi vzorků. Pro pozitivní pár ($y=0$) minimalizuje kvadratickou euklidovskou vzdálenost. Pro negativní pár ($y=1$) aktivuje penalizaci pouze tehdy, pokud je jejich vzdálenost menší než stanovená absolutní marže $m$: $L = (1 - y) [dist]^2 + y [\max \{0, m - dist\}]^2$. Trpí geometrickou nestabilitou fixního vynucování absolutní marže.
* **Triplet-loss učení:** Pokročilá ztrátová funkce, která rigidní absolutní marže nahrazuje flexibilnějším relativním uspořádáním trojic vzorků: **Kotva** (Anchor — $x^a$), **Pozitivní** (Positive — $x^p$, stejná třída jako kotva) a **Negativní** (Negative — $x^n$, odlišná třída).
* **Matematická formulace Triplet Loss:**
  $$L = \sum_{i=1}^{N} \max [ 0, \text{dist}^2(f(x_i^a), f(x_i^p)) - \text{dist}^2(f(x_i^a), f(x_i^n)) + \delta ]$$
  Model generuje nulovou ztrátu a je stabilní pouze tehdy, když je kvadratická vzdálenost od kotvy k negativnímu vzorku větší než vzdálenost k pozitivnímu vzorku alespoň o relativní marži $\delta$. Kvadratická euklidovská vzdálenost se volí z výpočetních důvodů, protože je na rozdíl od prosté euklidovské vzdálenosti hladce diferencovatelná i v nulovém bodě, což stabilizuje výpočet gradientů.
* **Strategie výběru trojic (Mining):**
  * **Hard Negatives:** Negativní vzorky, které jsou ke kotvě blíže než pozitivní vzor. Trénování čistě na nich může způsobit kolaps embeddingového prostoru do nulových vektorů.
  * **Easy Negatives:** Negativní vzorky daleko za marží $\delta$. Generují nulový loss, síť se z nich nic neučí.
  * **Semi-Hard Negatives:** Negativní vzorky ležící dál než pozitivní vzorek, ale spadající do zakázané zóny definované marží $\delta$ ($dist(a,p) < dist(a,n) < dist(a,p) + \delta$). **Nejvhodnější pro stabilní učení** a vysokou diskriminační schopnost.
</details>

<details>
<summary>Vektorová a produktová kvantizace pro aproximované hledání</summary>

* **Aproximované hledání nejbližších sousedů (ANN — Approximate Nearest Neighbor):** Přístup, který obětuje stoprocentní přesnost vyhledávání výměnou za řádové zrychlení odezvy a drastickou úsporu paměti RAM při práci s miliardovými databázemi vysokorozměrných vektorů.
* **Vektorová kvantizace (VQ):** Metoda ztrátové komprese prostoru. Pomocí algoritmu $K$-means se trénovací prostor rozdělí do $K$ clusterů, jejichž středy definují centroidy uspořádané do tzv. číselníku (**codebook**). Libovolný spojitý vstupní vektor je "zaokrouhlen" a nahrazen pouze celočíselným indexem nejbližšího centroidu. Geometricky tak prostor definuje Voroného diagram.
* **Produktová kvantizace (PQ):** Pokročilá dekompozice odstraňující limity VQ (která pro jemné rozlišení vyžaduje obří neúnosný číselník). Vstupní vektor o dimenzi $d$ se rozseká na $m$ menších nezávislých podvektorů (sub-vectors). Každý podvektor je kvantován samostatně pomocí vlastního miniaturního číselníku (např. o 256 controidech). Výsledný vektor je reprezentován jako $m$-tice bajtových indexů. Díky kartézskému součinu pod-číselníků dokáže PQ reprezentovat astronomické množství kombinací (např. $256^8$) při minimální paměťové stopě (8 bajtů).
* **Asymmetric Distance Computation (ADC):** Algoritmus pro bleskové vyhledávání nad produktově kvantovanou databází. Dotazový vektor se záměrně **nekvantuje** (zůstává v čisté plné podobě — asymetrie). Vzdálenost mezi čistým dotazem a všemi kvantovanými vektory v databázi se počítá bleskovým sčítáním hodnot vyhledaných v předpočítaných tabulkách vzdáleností k centroidům (*look-up tables*), což odstraňuje drahé výpočty euklidovských vzdáleností v plné dimenzi.
* **Inverted File System (IVF) indexace:** Technika hrubého před-rozdělení prostoru na velké makro-clustery. Při dotazu se vyhledají pouze nejbližší makro-centroidy a následné jemné PQ/ADC vyhledávání probíhá striktně uvnitř těchto vymezených invertovaných seznamů, což eliminuje nutnost skenovat celou databázi (např. knihovna *FAISS*).
</details>

<details>
<summary>Principy křížově-modálního učení a CLIP</summary>

* **Modalita dat:** Specifický formální způsob, jakým je informace kódována, vyjádřena nebo vnímána (např. text, statický obraz, audio signál, video sekvence).
* **Křížově-modální (Cross-modal) učení:** Architektonický koncept navržený k překonání "propasti mezi modalitami" hledáním skrytých korelací. Cílem je namapovat různorodé datové typy (např. obraz a text) do jednoho společného vnořeného prostoru (*Common Embedding Space*) se zachováním sémantického zarovnání (alignment) — koncept "pes" v textu musí mít velmi blízký vektor jako fotografie psa.
* **CLIP (Contrastive Language-Image Pre-training):** Přelomová multimodální architektura (OpenAI, 2021) stavící na masivním kontrastivním tréninku nad stovkami milionů internetových párů obrázek-text.
* **Architektura CLIP:** Sestává ze dvou nezávislých paralelních enkodérů: **Image Encoder** (typicky Vision Transformer — ViT) a **Text Encoder** (klasický Transformer).
* **Kontrastivní tréninková matice CLIP:** Model se netrénuje na klasifikaci labelů. V rámci trénovacího batche o velikosti $N$ dvojic model paralelně vyhodnocuje matici $N \times N$ možných kombinací obrázek-text. Cílem je maximalizovat kosinovou podobnost u $N$ reálných (diagonálních) pozitivních párů a minimalizovat ji (odpuzovat v prostoru) u zbylých $N^2 - N$ negativních kombinací.
* **Zero-shot klasifikace:** Schopnost CLIPu okamžitě generalizovat na zcela neznámé úkoly a datasety bez jakéhokoliv dotrénovávání vah (*fine-tuningu*).
* **Princip Zero-shot přes Prompt Engineering:** Fyzické názvy cílových klasifikačních tříd se obalí do textových šablon (promptů), např. *"a photo of a [dog]"*. Tyto texty projdou textovým enkodérem a vygenerují cílové textové embeddingy. Vstupní neznámý obrázek projde obrazovým enkodérem a systém prostým výpočtem nejvyšší kosinové podobnosti určí, ke kterému textovému embeddingu má obrázek sémanticky nejblíže.
* **Evaluace křížově-modálního vyhledávání:**
  * **Recall@K (R@1, R@5, R@10):** Výkonnostní metrika udávající procentuální pravděpodobnost, že se správný hledaný multimodální protějšek nachází mezi prvními $K$ nejbližšími navrženými výsledky v embeddingovém prostoru.
  * **Median Rank (MedR):** Střední hodnota (medián) pořadí, na kterém se v seřazeném seznamu výsledků reálně nachází správná odpověď (optimální hodnota je 1).
</details>

# Dobývání znalostí

<details>
<summary>Asociační pravidla a hledání frekventovaných vzorů</summary>

* **Model tržního koše:** Abstraktní datový model pro identifikaci skrytých kauzalit a korelací mezi položkami. Sestává z univerza položek (*items*) a souboru transakcí (*baskets*), kde každý koš je malou podmnožinou všech položek. Cílem je nalézt pravidla typu $I \rightarrow j$, kde $I$ je množina položek (antecedent) a $j$ je položka (konsekvent).
* **Support (Podpora):** Pravděpodobnost, s jakou se daná množina položek $I$ vyskytuje napříč všemi transakcemi v datasetu: $\text{Support}(I) = \frac{\text{počet košů obsahujících } I}{\text{celkový počet košů}}$. Definuje se minimální práh podpory ($s$), pod kterým se množiny ignorují.
* **Confidence (Spolehlivost):** Ukazatel podmíněné pravděpodobnosti $P(j \mid I)$ vyjadřující, jak často se v koši objeví konsekvent $j$ za předpokladu, že už obsahuje kompletní antecedent $I$: $\text{Confidence}(I \rightarrow j) = \frac{\text{Support}(I \cup \{j\})}{\text{Support}(I)}$.
* **Interest (Zajímavost):** Rozdíl mezi spolehlivostí konkrétního pravidla a osamocenou celkovou pravděpodobností výskytu konsekventu $j$ v celém datasetu: $\text{Interest}(I \rightarrow j) = \text{Confidence}(I \rightarrow j) - \text{Support}(\{j\})$. Vysoká kladná hodnota indikuje silný sémantický vztah; hodnota blízká 0 značí statistickou nezávislost.
* **Princip monotonicity (Apriori vlastnost):** Základní pilíř pro boj s kombinatorickou explozí. Říká, že *pokud je množina položek frekventovaná, musí být garantovaně frekventované i všechny její podmnožiny*. Obráceně platí: pokud je libovolný prvek/dvojice nefrekventovaný, jakákoli nadmnožina obsahující tento prvek nemůže dosáhnout prahu podpory.
* **Algoritmus A-Priori:** Víceprůchodový algoritmus využívající princip monotonicity. V prvním průchodu (*Pass 1*) spočítá výskyty jednotlivých prvků a sestaví seznam frekventovaných jednoprvkových množin ($L_1$). V druhém průchodu (*Pass 2*) generuje kandidáty na dvojice ($C_2$) **striktně a pouze** kombinací prvků obsažených v $L_1$, čímž radikálně omezuje velikost prohledávaného paměťového prostoru. Proces pokračuje pro trojice ($C_3$) atd.
</details>

<details>
<summary>Algoritmus PCY a jeho rozšíření (Multistage, Multihash)</summary>

* **Algoritmus PCY (Park-Chen-Yu):** Optimalizace algoritmu A-Priori, která efektivně vytěžuje nevyužitou operační paměť RAM během prvního průchodu daty (*Pass 1*). Vedle počítání frekvencí jednotlivých položek si v RAM alokuje **hashovací tabulku bucketů**.
* **Princip redukce kandidátů v PCY:** Během *Pass 1* se pro každý koš vezmou všechny dostupné dvojice a proženou se hashovací funkcí do příslušných kbelíků (bucketů), kde se inkrementují jejich čítače. Na konci průchodu se buckety porovnají s prahem $s$ a transformují se do **bitové mapy** (1 pro frekventovaný bucket, 0 pro nefrekventovaný). Pokud je celková suma výskytů v bucketu menší než $s$, žádná dvojice v něm obsažená nemůže být frekventovaná.
* **PCY Pass 2 Filter:** V druhém průchodu je dvojice zařazena mezi kandidáty ($C_2$) tehdy a jen tehdy, pokud: 1. Obě položky jsou samostatně frekventované (pravidlo A-Priori) **A ZÁROVEŇ** 2. Její hash spadá do bucketu, který má v bitmapě hodnotu 1. Výrazně se tím redukuje počet prvků, pro které je nutné v paměti alokovat přesné celočíselné čítače.
* **Multistage PCY:** Rozšíření, které mezi *Pass 1* a *Pass 2* vkládá dodatečné průchody daty za účelem filtrace falešně pozitivních kandidátů. Využívá pokaždé novou, nezávislou hashovací funkci, přičemž do další tabulky hashují pouze ty dvojice, které úspěšně prošly bitovými mapami všech předchozích fází. Výsledkem je kaskáda bitmap drasticky čistící prostor kandidátů.
* **Multihash PCY:** Paralelní přístup, který namísto sekvenčních fází implementuje **více nezávislých hashovacích tabulek přímo v prvním průchodu** (*Pass 1*). Paměť RAM pro hashování se rozdělí mezi tyto tabulky. Dvojice se stane legitimním kandidátem v *Pass 2* pouze tehdy, pokud její hash dosáhne prahu podpory ve všech tabulkách simultánně.
</details>

<details>
<summary>Principy shlukovacích algoritmů (k-means, hierarchické shlukování, DBSCAN, Chameleon)</summary>

* **Shluková analýza (Clustering):** Disciplína nesupervizovaného učení s cílem rozsegmentovat data do homogenních skupin (shluků) tak, aby vnitřní podobnost objektů uvnitř shluku (*intra-cluster similarity*) byla maximalizována a vnější podobnost mezi shluky (*inter-cluster similarity*) minimalizována.
* **k-means:** Partitní (rozdělovací) algoritmus minimalizující součet čtverců eukleidovských vzdáleností bodů od příslušných těžišť shluků — **centroidů**. Začíná náhodnou inicializací $k$ centroidů, načež cyklicky opakuje dvě fáze: *přiřazení* (každý bod je alokován k nejbližšímu centroidu) a *aktualizace* (poloha centroidu se přepočítá jako aritmetický průměr souřadnic všech jemu přiřazených bodů).
* **Limity k-means:** Vyžaduje striktní manuální zadání parametru $k$ předem, je extrémně citlivý na odlehlá pozorování (outliery, které deformují průměr polohy centroidu) a dokáže korektně identifikovat výhradně sférické (kulovité) shluky podobných velikostí a hustot.
* **Hierarchické shlukování (Aglomerativní):** Přístup "zdola nahoru", který buduje stromovou strukturu vzájemných vazeb — **dendrogram**. Na počátku tvoří každý datový bod samostatný mikroshluk. V každém dalším kroku algoritmus vyhledá a sloučí dva geometricky nejbližší shluky, což opakuje, dokud se vše nesjednotí do jednoho finálního shluku. Výsledný počet shluků se definuje dodatečným horizontálním "uříznutím" dendrogramu v požadované výšce.
* **Metodiky měření vzdálenosti (Linkage) v hierarchickém shlukování:**
  * **Single Link (MIN):** Vzdálenost dvou nejbližších bodů z různých shluků. Dokáže flexibilně obepnout protáhlé nelineární tvary, ale trpí na anomálii **řetězení** (*chaining*), kdy se dva odlišné shluky chybně slijí v jeden kvůli úzkému náhodnému mostu bodů.
  * **Complete Link (MAX):** Vzdálenost dvou nejvzdálenějších bodů z různých shluků. Generuje striktně kompaktní, kulovité, husté struktury, ale je vysoce náchylný na outliery.
  * **Average Link / Group Average:** Průměrná eukleidovská vzdálenost vypočítaná mezi všemi páry bodů obou shluků. Stabilní kompromis odolný vůči šumu.
  * **Centroid Linkage:** Vzdálenost mezi těžišti obou shluků. Může vykazovat neintuitivní chování (ne-monotonní zvraty v dendrogramu), pokud mají shluky diametrálně odlišný rozsah.
* **DBSCAN:** Shlukování striktně založené na **hustotě**. Vyžaduje dva konfigurační parametry: poloměr okolí ($Eps$) a minimální počet bodů v tomto poloměru ($MinPts$). Dokáže detekovat shluky libovolných amorfních tvarů a automaticky izoluje šum.
* **Klasifikace bodů v DBSCAN:**
  * **Core Point (Jádrový bod):** Bod, který má ve svém kruhovém okolí $Eps$ alespoň $MinPts$ bodů (včetně sebe samého).
  * **Border Point (Hraniční bod):** Bod, který sám nesplňuje limit hustoty pro jádro, ale fyzicky leží v sousedství jiného jádrového bodu.
  * **Noise Point (Šum / Outlier):** Bod, který není ani jádrový, ani hraniční; je algoritmem ignorován a nezařazen do žádného shluku.
* **Chameleon:** Pokročilý dvoufázový hierarchický algoritmus, který odstraňuje nedostatky statických modelů zavedením dynamického modelování pro slučování shluků.
* **Dvě fáze algoritmu Chameleon:**
  1. **Fáze rozdělení:** Data se transformují na graf nejbližších sousedů (*$k$-nearest neighbor graph*), který se následně pomocí pokročilého topologického grafového řezání (např. systém METIS) rozseká na velké množství miniaturních, vysoce homogenních podshluků (*sub-clusters*).
  2. **Fáze spojování:** Podshluky se postupně aglomerativně propojují na základě kritérií **Relativní propojenosti (Relative Interconnectivity)** a **Relativní blízkosti (Relative Closeness)**. Dva shluky se sloučí pouze tehdy, pokud je jejich vzájemný mezilehlý kontakt silný a blízký v relativním poměru k tomu, jak hustě a pevně jsou body propojeny uvnitř těchto shluků samotných. Zvládá extrémně složité, zakřivené a do sebe vnořené geometrie.
</details>

<details>
<summary>Analýza temporálních dat (Vlastnosti a předzpracování časových řad)</summary>

* **Časová řada:** Chronologicky uspořádaná posloupnost skalárních pozorování $X = x_1, x_2, \dots, x_n$ snímaných v čase. Pořadí bodů je striktně invariantní a body vykazují časovou závislost (**autokorelaci**).
* **Základní strukturální komponenty časové řady:**
  * **Trend:** Dlouhodobý, permanentní deterministický nárůst nebo pokles globální hladiny hodnot v čase (lineární či nelineární povahy).
  * **Sezónnost (Seasonality):** Pravidelně a systematicky se opakující datové oscilace a vzorce vázané na **pevně definovanou a známou časovou periodu** (např. roční pokles teplot v zimě, týdenní cykly logistiky).
  * **Cykličnost (Cyclicity):** Fluktuace hodnot v čase vykazující vlnový charakter, které však **nemají pevnou ani předvídatelnou periodu** (typicky makroekonomické hospodářské cykly, solární cykly).
  * **Stacionarita:** Klíčový statistický předpoklad pro prediktivní modelování. Časová řada je striktně stacionární, pokud jsou její průměr, rozptyl a autokorelační struktura v čase konstantní (řada nevykazuje trend ani změny variability). Nestacionární řady se transformují na stacionární nejčastěji pomocí **diferencování** ($\Delta x_t = x_t - x_{t-1}$).
  * **Šum (Noise):** Stochastická náhodná složka s nulovou střední hodnotou, která nenese sémantickou informaci; cílem analýzy je její filtrace.
* **Matematické techniky předzpracování řad:**
  * **Zarovnání posunu (Offset Translation):** Eliminace rozdílných absolutních hladin hodnot vycentrováním řady kolem nulové osy odečtením celkového průměru $\mu$: $x'_i = x_i - \mu$.
  * **Změna amplitudy (Amplitude Scaling):** Sjednocení vertikálního měřítka rozptylu podělením každého bodu směrodatnou odchylkou $\sigma$: $x'_i = x_i / \sigma$.
  * **Z-score normalizace:** Kombinace obou kroků ($x'_i = \frac{x_i - \mu}{\sigma}$), zajišťující průměr 0 a rozptyl 1. Je to kritický předpoklad pro to, aby eukleidovská vzdálenost porovnávala čistý geometrický tvar křivky bez zkreslení absolutními hodnotami.
  * **Odstranění trendu (Detrending):** Odfiltrování dlouhodobého sklonu řady. Trend se matematicky namodeluje (např. lineární regresí jako $y = at + b$) a výsledná přímka se od původní řady odečte: $x'_t = x_t - (at + b)$.
</details>

<details>
<summary>Dynamic Time Warping (DTW)</summary>

* **Dynamic Time Warping (DTW):** Algoritmus navržený pro exaktní měření sémantické podobnosti mezi dvěma časovými řadami, které mohou vykazovat odlišnou délku, nebo jsou vůči sobě v čase fázově posunuté, lokálně roztažené či smrštěné.
* **Selhání Eukleidovské vzdálenosti:** Klasická eukleidovská vzdálenost striktně porovnává body ve stejném čase $t$ ($x_t$ proti $y_t$). Pokud jsou dvě řady tvarově naprosto identické, ale jedna je pouze o jeden časový krok posunutá, eukleidovská vzdálenost vrátí obrovskou hodnotu (indikuje falešnou nepodobnost).
* **Konstrukce optimální distanční cesty:** DTW sestaví matici vzdáleností o rozměrech $n \times m$ pro všechny vzájemné dvojice bodů obou řad. Následně pomocí dynamického programování vyhledá **optimální vyrovnávací cestu (warping path)**, která prochází maticí a minimalizuje celkovou kumulativní vzdálenost mezi srovnávanými úseky křivek.
* **Striktivní matematické podmínky na warping path:**
  * **Okrajové podmínky (Boundary conditions):** Cesta musí povinně startovat v levém dolním rohu matice $(1,1)$ a končit v pravém horním rohu $(n,m)$, což garantuje protažení a zohlednění celých řad od začátku do konce.
  * **Monotonicita:** Indexy cesty se v čase smí pohybovat pouze kupředu, nikdy se nesmí vracet v čase zpět ($i_{k+1} \ge i_k$ a $j_{k+1} \ge j_k$).
  * **Spojitost:** Cesta se smí v matici posouvat v každém kroku maximálně o jedno pole (diagonálně, vodorovně nebo svisle), což brání přeskakování důležitých datových bodů.
</details>

<details>
<summary>Klouzavý průměr (Moving Average — MA)</summary>

* **Jednoduchý klouzavý průměr (Simple Moving Average — SMA):** Základní deterministická technika digitálního předzpracování a vyhlazování dat. Každý bod časové řady v čase $t$ je nahrazen aritmetickým průměrem hodnot spadajících do lokálního časového okna o fixní délce $k$:
  $$MA_t = \frac{1}{k} \sum_{i=0}^{k-1} x_{t-i}$$
* **Filtrace signálu pomocí SMA:** Funguje de facto jako matematický **nízkofrekvenční filtr** (low-pass filter). Efektivně vyhlazuje a odstraňuje vysokofrekvenční šum a drobné náhodné fluktuace, čímž odkrývá čistou podkladovou trajektorii dlouhodobého trendu.
* **Vliv délky okna ($k$):** Nastavení délky okna představuje inženýrský kompromis. Čím je okno $k$ širší, tím je výsledná křivka hladší a imunnější vůči šumu. Daň za vysoké vyhlazení je však **fázové zpoždění (lag)** – vyhlazená křivka reaguje na prudké reálné zvraty a změny v datech se zpožděním a dochází k vyhlazení (ztrátě) významných datových špiček a extrémů.
</details>

# Vizualizace

<details>
<summary>Metriky hodnocení kvality a pre-attentive vnímání</summary>

* **Vizualizační transformace:** Matematicko-grafický proces mapování surových dat přes vizuální reprezentaci do lidského vědomí a kognitivní paměti: $\text{Data} \rightarrow \text{Presentation} \rightarrow \text{Mind}$.
* **Expresivita (Expressiveness):** Základní kvalitatívní metrika (Bertin, Mackinlay). Definuje schopnost vizualizace prezentovat **právě a pouze všechny relevantní informace** obsažené v datech a vůbec nic navíc:
  $$M_{exp} = \frac{\text{zobrazená informace}}{\text{informace k vyjádření}} \quad \text{kde} \quad 0 \le M_{exp} \le 1$$
  *Porušení expresivity (Sémantický šum):* Pokud vizualizace indukuje klamnou informaci, která v datech neexistuje (např. použití stupňovaného jasu jedné barvy pro nominální kategorie nutí lidský zrak chybně hledat v datech neexistující uspořádání či hierarchii).
* **Efektivita (Effectiveness):** Metrika hodnotící kognitivní rychlost a přesnost přenosu informace k lidskému mozku a technickou náročnost vykreslení. Určuje, zda byly zvoleny optimální vizuální proměnné pro daný datový typ:
  $$M_{eff} = \frac{1}{1 + \text{interpretace} + \text{vykreslování}} \quad \text{kde} \quad 0 \le M_{eff} \le 1$$
* **Pre-attentive (předmnožinové) vnímání:** Vizuální stimuly a podněty, které lidské oko a vizuální kortex zpracovávají podvědomě, okamžitě (v časovém okně do 200–250 ms) a bez nutnosti vědomého soustředění či skenování scény položku po položce.
* **Třídy pre-attentive prvků:**
  * *Geometrické:* Pozice v prostoru, prostorová orientace/sklon, délka čáry, velikost, zakřivení objektu, tloušťka čáry.
  * *Optické:* Odstín barvy (*hue*), intenzita/jas barvy (*value*), shlukování prvků.
  * *Dynamické:* Pohyb v prostoru, rychlost pohybu, blikání či oscilace (nejsilnější pre-attentive stimul vůbec).
</details>

<details>
<summary>Osm základních vizuálních proměnných</summary>

* **Vizuální proměnné:** Grafické mechanismy sloužící kódování datových atributů do vizuální podoby na obrazovce. Jejich efektivita je striktně závislá na datovém typu (Kvantitativní, Ordinální, Nominální):
1. **Pozice (Position):** Nejdůležitější a kognitivně nejpřesnější proměnná pro **všechny** typy dat. Umístění prvku na osách $(X, Y, Z)$ v kartézském prostoru.
2. **Tvar (Shape):** Geometrická povaha bodových prvků či symbolů. Představuje **čistě nominální proměnnou**; lidským okem jí nelze vyjádřit velikost ani pořadí, slouží výhradně pro kategorizaci.
3. **Velikost (Size):** Délka úsečky, plocha obrazce nebo objem tělesa. Výborná pro kvantitativní data, avšak vykazuje limity při přesném odhadu poměru ploch u regionů (Stevensův psychofyzikální zákon výkonu).
4. **Jas / Hodnota (Value / Brightness):** Úroveň světlosti barvy na monochromatické škále (od bílé po černou). Má vrozené, přirozené vnitřní uspořádání, což ji činí **ideální proměnnou pro ordinální (seřazená) data**.
5. **Barva / Odstín (Hue):** Konkrétní barevný tón (červená, zelená). **Čistě nominální proměnná** vhodná pro klasifikaci. Nemá přirozené uspořádání (duhová paleta je kognitivně nevhodná, protože vytváří umělé optické hranice). Bezpečná kardinalita pro člověka je max. 5–8 odstínů současně.
6. **Orientace (Orientation):** Úhel natočení vizuálního prvku v rovině. Klíčový pre-attentive prvek pro vizualizaci směrových vektorových polí.
7. **Textura (Texture):** Grafický vzor definovaný hustotou a velikostí vnitřních prvků. Změna hustoty prvků dodává textuře ordinální charakter, na který lidský zrak reaguje s vysokou citlivostí.
8. **Pohyb (Motion):** Zahrnuje směr, rychlost, blikání nebo oscilaci grafického prvku. Dominantní pre-attentive stimul upoutávající pozornost i na periferii zorného pole.
</details>

<details>
<summary>Vizualizační techniky pro 1D, 2D a 3D prostor</summary>

* **1D data (Sekvence jedné proměnné):** Čárové grafy, sloupcové grafy, barevné pruhy (*color bar*). Pro zobrazení vícerozměrných 1D dat se využívá buď **juxtapositioning** (skládání samostatných grafů izolovaně vedle sebe/pod sebe), nebo **superimpositioning** (překrývání více křivek v jednom souřadném systému s rozlišením barvou/stylem).
* **2D data (Data mapovaná na plochu):** *Scatterplot* (bodový graf bez interpolace pro detekci korelací), *choroplethy* (geografické mapy s vybarvenými polygony), *rastrový obraz* (spojitá interpolovaná pixelová data), *cityscape* (2.5D zobrazení výšky bloků nad rovinou) a *izolinie/kontury* (spojnice míst se stejnou hodnotou skalárního pole, např. vrstevnice, izobary).
* **3D data (Objemová data):** Data reprezentovaná jako spojitý prostor diskrétně vzorkovaný do pravidelné mřížky (**Voxely**) nebo nepravidelné mřížky (**Tetrahedry**). Rozlišujeme dva základní přístupy k zobrazení povrchů v 3D vizualizaci:
* **Explicitní reprezentace povrchu (Boundary Representation):** Povrch je přesně a pevně definován konkrétním výčtem strukturních prvků. Sestává z explicitního seznamu 3D vrcholů (*vertices*), hran (*edges*) a rovinných polygonů (*faces*) tvořících polygonální síť, která se bleskově renderuje na GPU pomocí rasterizace.
* **Implicitní reprezentace povrchu:** Povrch není v datech přímo definován, ale je popsán geometricky jako izohladina matematické funkce nebo hustotního pole ve tvaru $f(x, y, z) = c$, kde $c$ je prahová hodnota (*isovalue*).
  * **Marching Cubes algoritmus:** Algoritmus transformující implicitní objemová data na explicitní polygonální síť trojúhelníků. Prochází 3D mřížku po buňkách (krychlích). Na základě toho, které z 8 vrcholů krychle leží nad/pod stanoveným prahem $c$, vyhledá v indexové tabulce typologií (256 kombinací redukovaných symetrií na 15 základních případů) odpovídající topologii trojúhelníků a vygeneruje explicitní polygony.
  * **Ray Casting (Direct Volume Rendering):** Alternativní technika, která nepřevádí data na polygony. Skrz objemový voxelový prostor se z kamery vrhají imaginární paprsky. V pravidelných intervalech se podél paprsku vzorkují hustotní hodnoty, které se přes **přenosové funkce (transfer functions)** mapují na barvu a neprůhlednost (*opacity*). Tyto hodnoty se podél paprsku matematicky integrují do výsledného pixelu na obrazovce.
</details>

<details>
<summary>Vizualizace multidimenzionálních dat a hierarchických struktur</summary>

* **Multidimenzionální vizualizace:** Metody pro transformaci a projekci vztahů vysokorozměrných dat ($N$-dimenzí) do srozumitelného 2D prostoru obrazovky.
* **Paralelní souřadnice (Parallel Coordinates):** Geometrická technika, kde jsou osy $X_1, X_2, \dots, X_n$ uspořádány rovnoběžně a vertikálně vedle sebe na obrazovce. Každá vícerozměrná datová položka (bod) je reprezentována jako spojitá **lomená čára (polyline)**, která protíná jednotlivé osy v místech svých hodnot. Trpí vizuálním zahlcením (*cluttering*) při velkém objemu dat.
  * *Detekce korelací:* Pokud se lomené čáry mezi dvěma sousedními osami masivně kříží, indikuje to negativní korelaci; pokud běží rovnoběžně a horizontálně, indikuje to pozitivní korelaci.
* **RadViz (Radial Coordinate Visualization):** Nelineární projekce inspirovaná Hookeovým elastickým zákonem. Kotvy jednotlivých $N$ dimenzí jsou rovnoměrně rozmístěny po obvodu kružnice. Datový bod je zobrazen jako bod uvnitř kružnice v místě, kde se přitažlivé síly fiktivních "pružin" všech dimenzí ustálí v rovnováze (síla pružiny je úměrná normalizované hodnotě dané dimenze). Nevýhodou je riziko překryvu odlišných bodů uprostřed kružnice.
* **Matice bodových grafů (Scatterplot Matrices — SPLOM):** Symetrická čtvercová matice o velikosti $N \times N$, kde každá buňka obsahuje standardní 2D scatterplot pro danou dvojici dimenzí. Na hlavní diagonále jsou obvykle zobrazeny jednorozměrné histogramy. Špatně škáluje pro vysoké desítky dimenzí z důvodu miniaturizace jednotlivých oken.
* **Dimensional Stacking:** Mapování diskrétního prostoru do 2D pomocí rekurzivního vnořování souřadných systémů. Hlavní dvojice dimenzí rozdělí obrazovku na makro-mřížku a každá její buňka se vnitřně dělí na sub-mřížky podle dvojic méně významných dimenzí, což vytváří hierarchický vizuální vzor.
* **Treemaps (pro hierarchické struktury):** *Space-filling* metoda zobrazující stromová data jako do sebe rekurzivně vnořené obdélníky, čímž stoprocentně využívá plochu obrazovky. Celá plocha se dělí střídavě horizontálně a vertikálně podle hierarchických úrovní. Plocha obdélníku kóduje kvantitativní metriku (např. velikost souboru), barva kóduje kategorii či stav.
  * **Squarified Treemaps:** Pokročilý algoritmus pro tvorbu treemaps. Na rozdíl od základního *Slice-and-Dice* (který generuje příliš úzké, nečitelné nudle) optimalizuje řezání plochy tak, aby se poměr stran obdélníků co nejvíce blížil čtverci ($1:1$), což usnadňuje lidské porovnávání velikosti ploch.
</details>

<details>
<summary>Interakční techniky a koncept Focus + Context</summary>

* **Koncept Focus + Context:** Základní interakční a kognitivní paradigma řešící problém, jak detailně analyzovat specifickou podmnožinu dat (*Focus*) bez ztráty globálního přehledu o struktuře celku (*Context*), a to bez nutnosti přepínání oken či neustálého zoomování.
* **Fisheye (Rybí oko):** Deformační operátor aplikovaný v **prostoru grafického zobrazení (v rovině obrazovky)**. Uživatel definuje střed ohniska (fokus), poloměr lupy a matematickou míru deformace. Centrální oblast se geometricky zvětší pro detailní analýzu prvků, zatímco okolní kontext zůstane plynule komprimován a stlačen směrem k okrajům obrazovky, aniž by zmizel z dohledu.
* **Perspektivní stěny (Perspective Walls):** Deformační technika řešící navigaci v **prostoru samotných dat/objektů** (typicky lineárně dlouhé struktury, např. časové osy). Data se horizontálně rozprostřou na virtuální trojdílnou stěnu. Centrální panel (rovnoběžný s rovinou obrazovky) zobrazuje nezkreslená data v plném detailu (*Focus*), zatímco dva boční panely ubíhají do 3D pozadí pomocí perspektivní deformace (*Context*), čímž dramaticky šetří šířku obrazovky.
</details>

<details>
<summary>Aplikace interakcí v prostoru dat vs. v prostoru atributů</summary>

* **1. Interakce v prostoru samotných dat (Space of data values):**
  Interakční techniky, které operují přímo nad **abstraktními hodnotami proměnných, logickou strukturou, metadaty nebo topologií dat**. Tyto operace mění výběr, uspořádání nebo věcný rozsah zobrazovaných datových řádků/položek.
  * *Příklady technik:*
    * **Filtrování rozsahů (Filtering):** Omezení datové sady zadáním striktních logických a matematických podmínek na hodnoty atributů (např. posuvníkem omezit vizualizaci pouze na záznamy, kde $150 < \text{systolický\_tlak} < 180$).
    * **Brushing a Linking:** Pokročilá technika, kde interaktivní výběr (označení — *brushing*) podmnožiny dat v jednom konkrétním grafu (např. v histogramu) okamžitě vyvolá automatické zvýraznění a vyhledání identických datových položek (*linking*) ve všech ostatních otevřených vizualizačních oknech (např. v geografické mapě či paralelních souřadnicích).
    * **Topologické a strukturální změny:** Interaktivní změna pořadí os v paralelních souřadnicích (kritická pro odhalení skrytých sousedních korelací) nebo dynamické přidávání/odstraňování uzlů a hran v grafových strukturách síťových analýz.

* **2. Interakce v prostoru atributů / grafického zobrazení (Space of attributes / Presentation):**
  Interakční techniky, které operují výhradně nad **vizuálními vlastnostmi grafických entit na obrazovce** (pixely, barevné palety, parametry shaderů, geometrické transformace zobrazení). Tyto techniky **nijak nemění podkladová data ani jejich strukturu**, upravují pouze způsob, jakým jsou data na monitoru vykreslena a vnímána lidským okem.
  * *Příklady technik:*
    * **Modifikace přenosových funkcí (Transfer Functions):** U objemového vykreslování (Ray Casting) uživatel interaktivně křivkami mění vztah, jaká 3D hustota bude mít jakou barvu a neprůhlednost. Lze tak interaktivně "zneviditelnit" měkké tkáně (nastavením jejich opacity na 0) a nechat zobrazenou pouze kostru, přičemž zdrojová voxelová data pacienta zůstávají netknutá.
    * **Optické a渲染 úpravy:** Změna jasu, kontrastu, změna barevné palety (např. přepnutí z barevného kódování do stupňů šedi z důvodu barvosleposti) nebo interaktivní úprava parametrů stínování (*shading*, např. Phongův osvětlovací model) a pozice světel pro lepší vnímání 3D tvarů.
    * **Geometrická navigace prezentace:** Standardní operace typu pan (posun kamery), zoom (přiblížení scény) nebo aplikace deformačních filtrů v rovině obrazovky (Fisheye).
</details>

---

# Analýza dat

<details>
<summary>Datové sklady, životní cyklus a moderní architektury</summary>

* **Datový sklad (Data Warehouse — DWH):** Centralizované integrované perzistentní úložiště určené pro ukládání, transformaci a analýzu historických firemních dat. Slouží výhradně jako podpora pro strategické manažerské rozhodování a Business Intelligence.
* **Čtyři základní vlastnosti DWH (podle Inmona):**
  * **Subject-oriented (Subjektově orientovaný):** Data jsou logicky strukturována a organizována kolem klíčových témat podniku (např. *zákazník, produkt, prodej*), nikoliv podle technických potřeb provozních aplikací.
  * **Integrated (Integrovaný):** Data z různých heterogenních zdrojů jsou kompletně sjednocena, vyčištěna a standardizována (jednotné kódování, formáty a měrné jednotky).
  * **Time-variant (Časově proměnný):** Data zachycují dlouhodobou historii a jsou vždy striktně svázána s časovým údajem (každý klíč v DWH v sobě přímo nebo nepřímo obsahuje časový prvek), na rozdíl od OLTP, které odráží pouze aktuální stav.
  * **Non-volatile (Stálý/Neměnný):** Nahrání dat do skladu je trvalé. Koncoví uživatelé data pouze čtou, standardní modifikace (`UPDATE`) nebo mazání (`DELETE`) jsou zakázány.
* **OLTP vs. OLAP:** *OLTP (Online Transaction Processing)* podporuje každodenní byznys operace, provádí bleskové, detailní a izolované transakce nad aktuálními daty při vysokém souběhu uživatelů. *OLAP (Online Analytical Processing)* provádí komplexní, výpočetně extrémně drahé dotazy nad historickými agregovanými daty pro účely plánování.
* **Data Lake (Datové jezero):** Úložiště pro ukládání dat v jejich surovém, nativním formátu (strukturovaná, polostrukturovaná jako JSON/XML i nestrukturovaná jako obrázky či texty). Využívá přístup **Schema-on-Read** (schéma se definuje a interpretuje až při samotném čtení a analýze), na rozdíl od DWH s přístupem *Schema-on-Write*.
* **Data Lakehouse:** Moderní hybridní architektura kombinující škálovatelnost a nízkou cenu Data Laku s transakční spolehlivostí (ACID), vynucováním datové integrity a vysokým výkonem SQL typickým pro datové sklady. Staví na otevřených formátech úložišť (např. *Delta Lake* nebo *Apache Iceberg*).
* **Data Virtualization (Datová virtualizace):** Abstraktní logická vrstva nad stávajícími zdrojovými systémy. Umožňuje dotazovat a propojovat data v reálném čase přímo tam, kde fyzicky leží, bez nutnosti jejich fyzické extrakce a centralizovaného stěhování (vynechává se fyzický staging).
* **Čtyři vrstvy toku dat v DWH:**
  1. *Source Systems* (provozní transakční OLTP systémy, externí CRM, ERP, soubory).
  2. *Data Staging Area* (dočasné tranzitní úložiště pro čištění a integraci; uživatelské dotazy jsou sem zakázány a operace běží sekvenčně bez transakčních logů a zamykání).
  3. *Data Warehouse Storage* (centrální perzistentní vrstva ukládající atomická historická data).
  4. *Information Delivery / Presentation* (vrstva zpřístupňující data koncovému uživateli — Data Marty, OLAP kostky, Power BI).
</details>

<details>
<summary>Zúžené datové sklady (Data Marts) a metodiky návrhu</summary>

* **Zúžený datový sklad (Data Mart):** Specializovaná, tematicky vymezená podmnožina dat vyčleněná z celopodnikového datového skladu. Je striktně přizpůsobena potřebám a analytickým dotazům konkrétní skupiny uživatelů nebo specifického oddělení podniku (např. *marketing, finance*). Data jsou zde často předagregována a denormalizována.
* **Top-down přístup (Bill Inmon):** Metodologie budování DWH jako jednoho centralizovaného, plně normalizovaného celku (Enterprise Data Warehouse — EDW, obvykle ve 3NF). Jednotlivé zúžené Data Marty jsou v tomto modelu chápány jako **závislé** — nevznikají samostatně, ale jsou fyzicky vyřezávány a denormalizovány až z již hotového a čistého EDW.
  * *Výhody:* Garantuje jedinou verzi pravdy (*Single Version of Truth*) napříč celou firmou, vysoká konzistence a robustnost architektury.
  * *Nevýhody:* Extrémní časová a finanční náročnost na počátku projektu; uživatelé čekají měsíce až roky na první reporty.
* **Bottom-up přístup (Ralph Kimball):** Pragmatický přístup, kde se celopodnikový datový sklad skládá z unie jednotlivých samostatných Data Martů navrhovaných rovnou v dimenzionálním formátu (hvězdné schéma). Data Marty se budují inkrementálně podle okamžitých byznysových potřeb.
  * *Výhody:* Velmi rychlá implementace a dodání hodnoty (*Business Value*) v řádu týdnů; flexibilita a organický růst.
  * *Nevýhody:* Riziko vzniku izolovaných a nepropojitelných datových sil (*Data Silos*), pokud se na začátku striktně neudrží standardy pro sdílené dimenze.
* **Shodné dimenze (Conformed Dimensions):** Klíčový stavební prvek Kimballova přístupu. Jedná se o dimenze (např. *čas, produkt, zákazník*), které mají napříč všemi samostatnými Data Marty naprosto identickou strukturu, atributy i surogátní klíče, což umožňuje různé Data Marty mezi sebou analyticky korektně propojovat.
</details>

<details>
<summary>Dimenzionální model (Hvězdné a vločkové schéma)</summary>

* **Dimenzionální modelování:** Změna paradigmatu v ukládání dat. Záměrně opouští vysoce normalizované struktury relačních databází a organizuje data tak, aby přímo kopírovala byznysové uvažování analytiků. Data rozděluje do dvou typů tabulek:
* **Tabulka faktů (Fact Table):** Centrální tabulka obsahující numerická měření, metriky a fakta popisující kvantifikovatelné události (např. *tržba, prodané množství*). Obsahuje cizí klíče směřující do tabulek dimenzí.
  * **Granularita (Grain):** Definuje úroveň detailu zachycenou v jediném řádku tabulky faktů (např. *jednotlivá položka na účtence* vs. *denní suma za prodejnu*). Čím jemnější granularita, tím větší flexibilita pro budoucí nečekané analýzy.
  * **Aditivita faktů:** Metriky mohou být *plně aditivní* (lze je sčítat přes všechny dimenze), *semi-aditivní* (lze je sčítat jen přes některé dimenze — např. stav skladu na konci dne nelze sčítat podél dimenze času) nebo *neaditivní* (poměrové ukazatele, marže).
* **Tabulka dimenzí (Dimension Table):** Tabulka obklopující tabulku faktů, která obsahuje široké textové a popisné atributy definující sémantický kontext faktů (odpovídá na otázky *kdo, co, kde, kdy, proč*).
* **Surogátní klíč (Surrogate Key):** Uměle vygenerovaný unikátní celočíselný identifikátor (integer) použitý jako primární klíč dimenze namísto přirozeného byznysového klíče z produkčních systémů. Zajišťuje absolutní nezávislost DWH na změnách ve zdrojových systémech, zvyšuje výkon operací `JOIN` a je nezbytný pro sledování historie změn.
* **Typy tabulek faktů:**
  * **Transaction Fact Table:** Každý řádek odpovídá jedné konkrétní transakční události v čase. Tabulka neustále roste do délky, staré řádky se nikdy nemodifikují.
  * **Periodic Snapshot Fact Table:** Jeden řádek představuje agregovaný stav za jasně definované fixní časové období (např. *měsíční uzávěrka skladu*).
  * **Accumulating Snapshot Fact Table:** Jeden řádek reprezentuje celý životní cyklus určité entity vyvíjející se v čase (např. *vyřízení objednávky od přijetí po doručení*). Obsahuje více datových milníků a existující řádky se v čase **aktualizují**.
* **Degenerovaná dimenze (Degenerate Dimension):** Dimenzionální atribut (např. *číslo faktury, ID tiketu*), který nemá žádné další popisné vlastnosti, a proto nemá smysl pro něj budovat samostatnou tabulku. Ukládá se **přímo do tabulky faktů**, kde slouží ke seskupování souvisejících položek.
* **Junk dimenze (Odpadková / Kombinovaná dimenze):** Jedna společná tabulka dimenze vytvořená pro uložení **všech existujících kombinací** drobných stavových indikátorů, flagů a kódů (např. *Platba_Kartou Y/N, Doruceno Y/N*). Brání přehlcení tabulky faktů velkým množstvím samostatných cizích klíčů.
* **Hvězdné schéma (Star Schema):** Implementace dimenzionálního modelu, kde je centrální tabulka faktů přímo propojena s tabulkami dimenzí pomocí relací `1:N`. Tabulky dimenzí jsou striktně **denormalizované** (obsahují redundanci), což zjednodušuje strukturu, minimalizuje počet potřebných `JOIN` operací a maximalizuje rychlost čtení. V BI praxi jednoznačně převažuje.
* **Vločkové schéma (Snowflake Schema):** Varianta hvězdného schématu, ve které jsou tabulky dimenzí plně nebo částečně **normalizovány** do více hierarchických úrovní (např. *Zboží* $\rightarrow$ *Subkategorie* $\rightarrow$ *Kategorie*). Šetří diskový prostor a usnadňuje přímou údržbu dimenzí, ale drasticky zpomaluje analytické dotazy kvůli kaskádovým operacím `JOIN`.
</details>

<details>
<summary>Správa změn v dimenzích (SCD — Slowly Changing Dimensions)</summary>

* **SCD (Slowly Changing Dimensions):** Soubor návrhových vzorů pro správu a uchovávání historie změn v tabulkách dimenzí (např. změna adresy či příjmení zákazníka). Je kritická pro pravdivost historického reportingu (aby se staré tržby zákazníka z Brna po jeho přestěhování do Prahy zpětně chybně nepřipsaly Praze).
* **Klíčové typy SCD:**
  * **SCD Typ 0:** Pevná hodnota (*Fixed*) — Hodnota se nastaví při prvním zápisu a nikdy se nemění (např. *datum narození*). Jakékoliv změny ze zdrojových systémů se ignorují.
  * **SCD Typ 1:** Přepis (*Overwrite*) — Stará hodnota je v dimenzi jednoduše přepsána novou. Historie se kompletně ztrácí a všechny minulé transakce se okamžitě začnou reportovat pod novým atributem.
  * **SCD Typ 2:** Přidání řádku (*Add new row*) — **Zlatý standard pro zachování historie**. Při změně se v dimenzi vytvoří zcela nový řádek s novým surogátním klíčem, časovým vymezením platnosti (`Valid From, Valid To`) a příznakem aktivity (`Is Active`). Staré transakce zůstávají navázané na původní surogátní klíč, nové dostanou klíč nový.
  * **SCD Typ 3:** Přidání sloupce (*Add new column*) — Stará hodnota se odsunie do dedikovaného sloupce (např. `Soucasne_Mesto` a `Predchozi_Mesto`), čímž se zachovává pouze omezená historie (aktuální a bezprostředně předcházející stav).
  * **SCD Typ 4:** Historická tabulka (*History Table*) — Aktuální data se udržují v hlavní tabulce dimenze (která se chová jako SCD1), ale každá změna se paralelně zapisuje jako nový řádek do samostatné čistě historické tabulky, což udržuje hlavní dimenzi malou a rychlou.
  * **SCD Typ 6 (Hybridní 2+3+1):** Kombinuje vlastnosti typů 2, 3 a 1. Vytvoří se nový řádek pro zachování historie (jako Typ 2), tabulka obsahuje sloupec pro předchozí hodnotu (jako Typ 3) a při změně se v historických řádcích přepíše příznak aktuální hodnoty (jako Typ 1), což umožňuje flexibilní reporting *As-Is* (z dnešního pohledu) i *As-Was* (jak to bylo tehdy).
</details>

<details>
<summary>Datová kostka (Data Cube) a OLAP operace</summary>

* **Datová kostka (Data Cube / OLAP Cube):** Vícerozměrná nadstavbová logická reprezentace agregovaných dat odvozená z dimenzionálního modelu. Data jsou v ní ze všech kombinací úhlů a hierarchií **předpočítána**, což umožňuje bleskovou interaktivní analýzu v reálném čase. Kostka se skládá z buněk (*cells*) definovaných kombinací dimenzí, přičemž buňky obsahují předpočítané hodnoty faktů.
* **Základní analytické operace nad kostkou:**
  * **Roll-up (Agregace):** Přechod od detailních dat k obecnějším, sumarizace hodnot nahoru podél hierarchie dimenze (např. posun z úrovně *Den* na úroveň *Měsíc* nebo z *Město* na *Stát*).
  * **Drill-down (Rozpad):** Opak operace roll-up. Přechod od sumárních globálních dat k podrobnějším detailům, odhalení jemnějších struktur (např. rozpad z úrovně *Rok* na jednotlivé *Kvartály*).
  * **Slice (Řez):** Výběr jedné fixní hodnoty jedné konkrétní dimenze, čímž dochází k redukci dimenzionality o 1 (vznikne plochý "řez" kostkou, např. *„pouze data pro rok 2026“*).
  * **Dice (Výřez/Kostka):** Výběr menší sub-kostky definováním omezujících podmínek (predikátů) na více dimenzích současně (např. *„pouze prodejny v Brně a pouze elektronika“*).
  * **Pivot (Otočení):** Změna vizuální orientace zobrazení dat v reportu, typicky záměna řádků za sloupce (otočení os).
* **Technologické implementace OLAP:**
  * **MOLAP (Multidimensional OLAP):** Data jsou fyzicky uložena ve speciálních proprietárních vícerozměrných polích. Všechny agregace jsou kompletně předpočítány při sestavení kostky. Poskytuje extrémní rychlost čtení, ale vyžaduje dlouhý čas procesování a vykazuje špatnou škálovatelnost, pokud je kostka příliš velká a řídká (*sparse*).
  * **ROLAP (Relational OLAP):** Datová kostka fyzicky neexistuje. Data zůstávají v relační databázi (Star Schema) a BI nástroj za běhu posílá komplexní SQL dotazy s funkcemi jako `SUM` a `GROUP BY`. Skvěle škáluje na obří terabajtové objemy, ale rychlost odezvy je plně závislá na výkonu DB.
  * **HOLAP (Hybrid OLAP):** Komplexní kompromis. Detailní atomická data zůstávají v relační databázi (ROLAP), ale vysoké manažerské agregace jsou předpočítány v rychlé vícerozměrné struktuře (MOLAP). Nabízí rychlý globální reporting s možností proklikat se (*drill-down*) do nejmenšího transakčního detailu.
</details>

<details>
<summary>Proces ETL vs. ELT a integrace dat</summary>

* **ETL (Extract, Transform, Load):** Páteřní proces integrace dat v DWH. Data jsou extrahována ze zdrojů, přenesena na dedikovaný integrační ETL server, kde proběhne čištění, byznys logika, výpočet metrik a tvorba surogátních klíčů, a až jako vyčištěný celek jsou zapsána do cílového DWH. Nevýhodou je, že výkon ETL serveru bývá úzkým hrdlem.
* **ELT (Extract, Load, Transform):** Moderní cloudový přístup. Data jsou v surové, nativní podobě extrahována ze zdrojů a okamžitě nahrána do cílového cloudového úložiště (do staging/transient vrstvy). Veškeré transformace, čištění a modelování se spouštějí až **uvnitř samotného DWH** pomocí drahých, ale vysoce distribuovaných SQL dotazů. Plně využívá masivní paralelní výpočetní výkon cloudu.
* **Change Data Capture (CDC):** Technika pro realizaci **přírůstkového nahrávání (Incremental Load)**. Z důvodu úspory času a minimalizace zátěže se nečte celá zdrojová databáze, ale identifikují a stahují se pouze změněná či nová data (delty) na základě časových razítek (`timestamp`), triggerů nebo přímým asynchronním čtením transakčních logů databáze.
* **Hromadné nahrávání (Bulk Load):** Optimalizační technika pro fázi *Loading*. Namísto posílání milionů SQL příkazů `INSERT` po jednotlivých řádcích se data zapíší hromadně na diskové bloky. Pro maximální výkon se před importem dočasně vypínají nebo dropují indexy a integritní omezení, které se po dokončení nahrávání hromadně přebudují (*rebuild*).
</details>

<details>
<summary>Profilování dat, datová integrita a kvalita dat (Data Quality)</summary>

* **Profilování dat:** Analytické zkoumání a auditování datových sad ze zdrojových systémů před jejich samotnou integrací do DWH. Cílem je získat hluboké porozumění skutečné struktuře, obsahu, závislostem a skrytým anomáliím v datech, aby se předešlo selhání ETL procesů.
* **Techniky profilování dat:**
  * **Strukturální analýza:** Ověřování formálního formátu a schématu (např. délka řetězců, nečekaná přítomnost `NULL` hodnot v povinných polích).
  * **Obsahová analýza:** Výpočet frekvenčních distribucí (odhalení nekonzistentních zápisů jako *CZ/CZE/Czechia*) a statistických shrnutí (min, max, průměry pro detekci outlierů typu záporný věk).
  * **Vztahová analýza:** Zkoumání vazeb napříč tabulkami a ověřování, zda klíče mezi nimi reálně lícují (odkrývání referenční integrity, která často v OLTP chybí a je udržována jen na úrovni aplikačního kódu).
* **Vynucování referenční integrity v DWH:** Zajišťuje správnost vztahů, kdy cizí klíč v tabulce faktů musí vždy odkazovat na existující primární klíč v dimenzi. Pokud při importu faktů klíč v dimenzi chybí, ETL nesmí záznam zahodit (způsobilo by to ztrátu finančních reportů); namísto toho se záznam v tabulce faktů naváže na speciální předem připravený řádek v dimenzi s identifikátorem např. `-1` (reprezentující hodnotu *"Neznámý/Neuvedený produkt"*).
* **Kvalita dat (Data Quality):** Míra vhodnosti a spolehlivosti dat pro byznys rozhodování v duchu pravidla **GIGO (Garbage In, Garbage Out)** — i ten nejlepší report poskytne chybné výsledky, pokud pracuje s nekvalitními daty.
* **Šest základních dimenzí kvality dat:**
  * **Přesnost (Accuracy):** Míra, do jaké uložená data věrně a bez překlepů popisují reálné objekty nebo události.
  * **Úplnost (Completeness):** Indikace, zda jsou v databázi přítomny všechny byznysově vyžadované datové prvky a pole (např. zda nechybí PSČ u adresy).
  * **Konzistence (Consistency):** Zajištění, že identická informace nemá v různých částech systému (nebo v různých systémech napříč firmou) protichůdné hodnoty (např. celkový obrat musí vykazovat stejné číslo v reportu pro logistiku i pro finance).
  * **Unikátnost (Uniqueness):** Stav, kdy je každá entita nebo událost v databázi zachycena právě jednou; detekce a eliminace duplicitních zákaznických profilů.
  * **Aktuálnost (Timeliness):** Dostupnost dat v čase, kdy jsou potřeba pro rozhodování. Zpoždění dat nesmí znehodnotit jejich informační hodnotu.
  * **Validita (Validity):** Míra, do jaké data striktně odpovídají definovaným syntaktickým standardům, vzorům a regulím (např. e-mailová adresa musí obsahovat znak `@`, rodné číslo musí splňovat dělitelnost jedenácti).
</details>

# Pokročilé techniky vyhledávání


<details>
<summary>Zpracování dat pomocí přístupu Map-Reduce</summary>

* **I/O limit Big Data:** Hlavním omezením masivních datových analýz není čistý výpočetní výkon procesoru (CPU), ale rychlost sekvenčního čtení z disku. Distribuovaná architektura (např. *HDFS*) dělí soubory na fixní bloky, replikuje je napříč clusterem uzlů (*nodes*) organizovaných v racích a uplatňuje princip **Data Locality** (výpočet se přesouvá fyzicky k uzlu s daty, čímž se eliminuje zahlcení síťové propustnosti).
* **Model Map-Reduce:** Deklarativní paralelní programovací model pro zpracování obrovských objemů dat. Veškeré datové toky jsou striktně reprezentovány jako strukturované dvojice **(klíč, hodnota)** neboli `(k, v)`. Framework automaticky zajišťuje rozdělení práce, failover a síťovou komunikaci.
* **Fáze Map (Mapper):** Uživatelem definovaná funkce, která nezávisle a paralelně zpracovává přidělený datový blok na konkrétním uzlu. Přijímá vstupní dvojici a generuje množinu mezilehlých (*intermediate*) dvojic:
  $$\text{Map}(k_1, v_1) \rightarrow \text{list}(k_2, v_2)$$
* **Fáze Shuffle and Grouping:** Automatický distribuovaný mezikrok mezi fázemi Map a Reduce, který představuje největší zátěž pro síťovou infrastrukturu clusteru. Systém seskupí všechny mezilehlé hodnoty se shodným klíčem z různých mapperů a vytvoří pro ně jednotný seznam: `(k2, list(v2))`. Současně provádí rovnoměrný rozklad dat (*Partitioning*) na cílové uzly.
* **Fáze Reduce (Reducer):** Uživatelem definovaná funkce, která přebírá unifikovaný klíč a kompletní seznam hodnot k němu přiřazených. Provádí finální agregaci, sumarizaci či transformaci dat a výsledky zapisuje přímo do distribuovaného souborového systému:
  $$\text{Reduce}(k_2, \text{list}(v2)) \rightarrow \text{list}(k_3, v_3)$$
* **Combiner (Lokální Reducer):** Volitelná optimalizační funkce, která provádí redukční operaci lokálně přímo na uzlu mapplera ještě před odesláním dat do síťové fáze Shuffle. Lze jej použít výhradně pro striktně asociativní a komutativní operace (např. suma, maximum), přičemž radikálně snižuje objem přenášených dat po síti.
* **Odolnost proti chybám (Fault Tolerance):** Master uzel průběžně monitoruje dostupnost worker uzlů pomocí pravidelných signálů (*heartbeat*). Pokud worker uzel selže:
  * *Pád Map workeru:* Úloha se musí spustit znovu na jiném uzlu od počátku, protože mezivýsledky Map fáze jsou ukládány na lokálních (ne-replikovaných) discích daného stroje.
  * *Pád Reduce workeru:* Úloha se pouze přeplánuje na jiný uzel, protože reducery čtou data ze sítě a výstup zapisují do redundantního HDFS.
</details>

<details>
<summary>Převod dokumentů na množiny (Shingling a Min-Hashing)</summary>

* **Near-Neighbor Search (Hledání podobných položek):** Problém vyhledávání sémanticky blízkých textů ve vysokorozměrných prostorech. Porovnání všech párů dokumentů má neudržitelnou kvadratickou složitost $O(n^2)$. Proces se efektivně optimalizuje třemi fázemi: *Shingling* $\rightarrow$ *Min-Hashing* $\rightarrow$ *LSH*.
* **Shingling (k-shingles):** Proces transformace souvislého textového dokumentu na množinu krátkých řetězců (tokenů) o pevně stanovené délce $k$. Zachycuje lokální lingvistickou strukturu textu.
* **Jaccardova podobnost:** Základní metrika pro exaktní porovnání podobnosti dvou množin $S_1$ a $S_2$ (v našem případě množin shingles). Vypočítá se jako podíl mohutnosti jejich průniku k mohutnosti jejich sjednocení:
  $$J(S_1, S_2) = \frac{|S_1 \cap S_2|}{|S_1 \cup S_2|}$$
* **Min-Hashing:** Technika drastického zkrácení obřích řídkých množin shingles do podoby krátkých, fixně dlouhých vektorů — **signatur**, se zachováním informace o původní Jaccardově podobnosti.
* **Matematická podstata Min-Hashe:** Představme si charakteristickou booleovskou matici (řádky jsou shingles, sloupce dokumenty). Pokud náhodně permutujeme řádky matice, pak hodnota $h(C)$ určuje index vůbec prvního řádku (v novém permutovaném pořadí), ve kterém daný sloupec/dokument obsahuje jedničku.
* **Vlastnost zachování podobnosti:** Pravděpodobnost, že se po náhodné permutaci budou min-hash hodnoty dvou různých dokumentů přesně shodovat, je matematicky exaktně rovna jejich původní Jaccardově podobnosti:
  $$P(h(C_1) = h(C_2)) = sim(C_1, C_2)$$
  Opakováním postupu s $n$ nezávislými náhodnými permutacemi (či hash funkcemi) vygenerujeme pro každý dokument signaturní vektor. Jaccardovu podobnost dokumentů pak odhadujeme prostým podílem shodných prvků v jejich signaturách.
</details>

<details>
<summary>Locality-Sensitive Hashing (LSH) a S-křivka</summary>

* **Locality-Sensitive Hashing (LSH):** Algoritmus, který řeší problém $O(n^2)$ porovnávání signatur. Mapuje signaturní vektory dokumentů do paměťových kbelíků (*buckets*) tak, že sémanticky podobné dokumenty skončí ve stejném kbelíku s extrémně vysokou pravděpodobností, zatímco nepodobné dokumenty se v něm potkají jen zcela výjimečně.
* **Princip pásem (Bands) a řádků (Rows):** Matice signatur o délce $n$ se striktně rozdělí horizontálně na $b$ pásem, kde každé pásmo obsahuje přesně $r$ řádků ($n = b \times r$).
* **Logika generování kandidátů:** Dva nezávislé dokumenty se stanou legitimními **kandidáty** pro detailní (drahé) porovnání pouze tehdy, pokud se jejich signatury **shodují úplně ve všech $r$ řádcích alespoň v jednom z $b$ pásem**. Pokud se dokumenty neshodnou v žádném pásmu, systém je okamžitě vyřadí a ušetří výpočetní čas.
* **Matematická formulace S-křivky:** Pravděpodobnost $P$, že se dva dokumenty o Jaccardově podobnosti $s$ stanou kandidáty, je popsána nelineární funkcí:
  $$P = 1 - (1 - s^r)^b$$
  * $s^r$: Pravděpodobnost, že se dokumenty dokonale shodují ve všech řádcích jednoho konkrétního pásma (logické vyhodnocení `AND` napříč řádky).
  * $1 - s^r$: Pravděpodobnost, že se v daném pásmu liší alespoň v jednom řádku.
  * $(1 - s^r)^b$: Pravděpodobnost, že se dokumenty neshodnou ani v jednom z dostupných $b$ pásem.
  * $1 - (1 - s^r)^b$: Finální pravděpodobnost shody v alespoň jednom pásmu (logické vyhodnocení `OR` napříč pásmy).
* **Interpretace geometrie S-křivky:** Tato funkce vykresluje strmou S-křivku s prahem $t \approx (1/b)^{1/r}$. Dokumenty s podobností $s > t$ jsou bleskově propuštěny jako kandidáti s pravděpodobností blížící se 100 %, zatímco dokumenty pod prahem jsou odfiltrovány s pravděpodobností blížící se 0 %.
* **Typy chyb v LSH:**
  * *Falešně negativní (False Negatives):* Vysoce podobné dokumenty, které se nešťastnou náhodou neshodly ani v jednom pásmu a systém je přehlédl (minimalizuje se navýšením počtu pásem $b$).
  * *Falešně pozitivní (False Positives):* Nepodobné dokumenty, které se náhodně shodly v jednom pásmu a staly se kandidáty (systém je bezpečně odhalí a zahodí v následném exaktním kroku výpočtu podobnosti).
</details>

<details>
<summary>Zpracování proudů dat a Bloomovy filtry</summary>

* **Datové proudy (Data Streams):** Specifický typ dat, která přicházejí kontinuálně, extrémní rychlostí, jsou potenciálně nekonečná a z kapacitních důvodů je nelze celá trvalé uložit na disková úložiště. Analýza probíhá nad fixními časovými výřezy — **klouzavými okny** (*Sliding Windows*).
* **Bloomův filtr:** Vysoce prostorově efektivní pravděpodobnostní datová struktura navržená pro okamžité a paměťově úsporné testování příslušnosti prvku do množiny bez nutnosti fyzického ukládání samotných prvků.
* **Architektura a exekuce Bloomova filtru:** Sestává z bitového pole o délce $m$ (na začátku vynulovaného) a sady $k$ nezávislých uniformních hashovacích funkcí $h_1, h_2, \dots, h_k$.
  * *Vkládání (Insertion):* Pro vstupní prvek $x$ se vypočítá $k$ hashů a bity na pozicích $h_1(x), h_2(x), \dots, h_k(x)$ se v poli striktně nastaví na hodnotu 1.
  * *Dotazování (Query):* Pro testovaný prvek $y$ se ověří stavy bitů na pozicích všech $k$ příslušných hashů. Pokud je **alespoň jeden** z těchto bitů roven 0, prvek v množině **garantovaně a stoprocentně není** (absolutní absence chyb typu *False Negatives*). Pokud jsou všechny bity rovny 1, prvek v množině **pravděpodobně je**, avšak existuje riziko chyby typu *False Positive*.
* **Matematická optimalizace filtru:** Pravděpodobnost výskytu falešně pozitivní chyby je popsána vztahem $(1 - e^{-kn/m})^k$. Pro minimalizaci této chyby při dané velikosti pole $m$ a počtu reálně vložených prvků $n$ se počet hashovacích funkcí $k$ optimalizuje podle vzorce:
  $$k = \frac{m}{n} \ln 2 \approx 0.7 \times \frac{m}{n}$$
  Při této ideální konfiguraci je v bitovém poli jedničkami obsazena přesně polovina kapacity, což poskytuje maximální entropii a informační hodnotu.
</details>

<details>
<summary>Algoritmus DGIM</summary>

* **Problém "Counting Ones":** Úloha odhadnout přesný počet jedniček v posledních $N$ bitech nekonečného proudového okna. Přímé uložení celého okna vyžaduje neúnosných $N$ bitů paměti.
* **Algoritmus DGIM:** Metoda logaritmické komprese toku dat pro úsporu operační paměti, která snižuje prostorové nároky na $O(\log^2 N)$ bitů za cenu zavedení drobné, shora striktně omezené nepřesnosti (maximální chyba odhadu je 50 %).
* **Struktura kbelíků (Buckets) v DGIM:** Proud je reprezentován jako posloupnost časově označených a nepřekrývajících se kbelíků. Každý kbelík si ukládá časovou značku svého konce a svou **velikost** (počet jedniček, který v něm leží). Velikost kbelíku musí být povinně **striktní mocninou 2** (1, 2, 4, 8, 16...).
* **Strukturální pravidla udržování DGIM:**
  1. Kbelíky jsou chronologicky seřazeny podle času a nesmí se překrývat.
  2. Pro každou povolenou velikost kbelíku smí v paměti existovat **maximálně dva (nebo jeden)** kbelíky současně.
  3. *Kaskádové slučování při zápisu:* Pokud do systému dorazí nová bitová 1, vytvoří se pro ni nový kbelík o velikosti 1. Pokud v ten moment v paměti vzniknou tři kbelíky o velikosti 1, dva chronologicky nejstarší z nich se automaticky sloučí a transformují se do jednoho integrovaného kbelíku o velikosti 2. Pokud by tímto spojením vznikly tři kbelíky o velikosti 2, proces se kaskádově řetězí dál do vyšších mocnin.
* **Výpočet odhadu součtu:** Když systém dostane dotaz na počet jedniček za posledních $N$ bitů, sečte velikosti všech kbelíků, které kompletně spadají do tohoto časového okna. Z úplně posledního (nejstaršího, jen částečně překrývajícího se) kbelíku pak do součtu započítá **přesně polovinu jeho velikosti**, což matematicky garantuje minimalizaci chyby.
</details>

<details>
<summary>PageRank</summary>

* **Webový graf:** Modelování internetu jako obřího orientovaného grafu, kde uzly představují webové stránky a orientované hrany reprezentují hypertextové odkazy mezi nimi.
* **PageRank:** Algoritmus stanovující objektivní míru autority a důležitosti stránek čistě na základě topologie odkazů. Odkaz ze stránky $A$ na stránku $B$ funguje jako sémantický "hlas". Síla a váha tohoto hlasu je přímo úměrná vlastní autoritě odkazující stránky $A$ a nepřímo úměrná počtu jejích odchozích odkazů.
* **Stochastická matice přechodu ($M$):** Pokud webová stránka $j$ obsahuje celkově $d_j$ odchozích odkazů a jeden z nich směřuje na stránku $i$, pak je prvek matice definován jako $M_{ij} = 1/d_j$. Pokud odkaz neexistuje, prvek je 0. Sloupce matice jsou stochastické vectors (jejich součet je roven 1).
* **Vlastní vektor rovnice:** PageRank je definován jako stacionární rozdělení pravděpodobnosti náhodného surfaře. Hledá se jako hlavní vlastní vektor matice $M$ odpovídající vlastnímu číslu 1:
  $$r = M \cdot r$$
* **Strukturální anomálie webu:**
  * **Dead Ends (Slepé uličky):** Uzly, které nemají žádné odchozí odkazy. Způsobují porušení stochastičnosti matice (sloupec obsahuje samé nuly), v důsledku čehož se PageRank z grafu při iteracích "vylévá" a celková autorita systému degraduje k nule.
  * **Spider Traps (Pavoučí pasti):** Uzly nebo izolované skupiny uzlů, které odkazují výhradně samy na sebe. Fungují jako datové černé díry — při prostém násobení do sebe nasají veškerou autoritu z ostatních částí internetu a zbytek grafu zůstane na nule.
* **Koncept náhodného surfaře (Random Surfer Model):** Matematická korekce anomálií zavedením **teleportace**. Předpokládá se, že surfař s pravděpodobností $\beta$ kliká na odkazy na stránce, avšak s pravděpodobností $1-\beta$ (tzv. *zdanění*, v praxi typicky 15 %) adresní řádek přepíše a skočí na naprosto náhodnou stránku celého webu.
* **Finální PageRank rovnice se zdaněním:**
  $$r = \beta M r + (1-\beta) \frac{1}{n} \mathbf{1}$$
  Kde $\beta$ je tlumicí faktor (*damping factor*, standardně 0.85), $n$ je celkový počet uzlů v grafu a $\mathbf{1}$ je jednotkový vektor. Teleportace garantuje, že matice přechodu se stane primitivní a ireducibilní, což zajišťuje jednoznačnou konvergenci výpočtu.
</details>

<details>
<summary>Výpočet Pageranku iterační metodou</summary>

* **Proč nelze použít přímé metody (Gaussovu eliminaci):** Přímé řešení soustavy lineárních rovnic o miliardách neznámých (miliardy stránek na webu) má kubickou časovou složitost $O(n^3)$, což je výpočetně naprosto nerealizovatelné.
* **Mocninná iterace (Power Iteration):** Iterační numerická metoda, která využívá faktu, že matice $M$ je extrémně řídká (*sparse* — drtivá většina buněk obsahuje nuly, protože stránka odkazuje jen na malé jednotky jiných webů). Násobení řídké matice vektorem je vysoce optimalizovatelné a dosahuje lineární složitosti vzhledem k počtu nenulových prvků.
* **Algoritmický postup exekuce:**
  1. *Inicializace:* Každému uzlu se na počátku přiřadí stejná startovní autorita: $r^{(0)} = [1/n, 1/n, \dots, 1/n]^T$.
  2. *Iterační smyčka:* V každém kroku se provede maticové přelití autority obohacené o teleportaci:
     $$r^{(t+1)} = \beta M r^{(t)} + \frac{1-\beta}{n} \mathbf{1}$$
  3. *Ukončovací kritérium (Konvergence):* Výpočet se cyklicky opakuje, dokud eukleidovská (či manhattanská) vzdálenost mezi vektory dvou po sobě jdoucích iterací neklesne pod stanovenou mez přesnosti: $\lvert r^{(t+1)} - r^{(t)} \rvert < \epsilon$.
* **Vysoká rychlost konvergence:** V praxi webového grafu rovnice konverguje překvapivě rychle. Pro dosažení stabilního a vysoce přesného uspořádání miliard stránek internetu stačí obvykle provést pouhých 50 až 100 iterací. Celý proces lze navíc nativně distribuovat a paralelizovat pomocí frameworku Map-Reduce.
</details>

# Podobnostní hledání

<details>
<summary>Principy podobnostního vyhledávání a formální model</summary>

* **Změna paradigmatu vyhledávání:** Tradiční databázové systémy staví na exaktním vyhledávání (*Exact Match*) nad strukturovanými daty, která lze lineárně seřadit. S nástupem nestrukturovaných multimediálních dat (obrázky, audio, video, textové embeddingy) se vyhledávání transformuje na vyhodnocování sémantické **podobnosti** na základě blízkosti v **metrickém prostoru**.
* **Formální komponenty modelu:** Proces vyhledávání podle podobnosti je exaktně popsán systémem čtyř prvků:
  * $M$: Univerzum (doména) obsahující úplně všechny teoreticky možné objekty.
  * $X$: Databáze reálných, persistentně uložených objektů, přičemž platí $X \subseteq M$.
  * $d$: Vzdálenostní funkce (metrika) měřící míru odlišnosti prvků ($d: M \times M \rightarrow \mathbb{R}$).
  * $q$: Dotazový objekt (*query object*), přičemž $q \in M$.
* **Cíl vyhledávání:** Efektivně identifikovat a vyjmout takovou podmnožinu prvků z databáze $X$, která vykazuje minimální vypočtenou vzdálenost (a tedy maximální podobnost) k zadanému dotazovému objektu $q$.
* **Exaktní vs. Aproximované vyhledávání (ANN):**
  * *Exaktní vyhledávání (Exact/Precise Search):* Garantuje stoprocentní přesnost a úplnost výsledků ($\text{Recall} = 1$). Vrátí přesně všechny objekty splňující matematické zadání dotazu. Index zde slouží výhradně jako akcelerátor výpočtu, výsledek je totožný se sekvenčním skenováním celé databáze.
  * *Aproximované vyhledávání (Approximate Search — ANN):* Záměrně obětuje marginální část přesnosti a úplnosti výměnou za řádové zrychlení vyhledávání a úsporu I/O operací. Je nezbytné pro zvládnutí obřích datových sad zatížených *prokletím dimenzionality*, kdy uživateli postačují výsledky „dostatečně blízké“.
</details>

<details>
<summary>Metrický prostor</summary>

* **Metrický prostor:** Uspořádaná matematická dvojice $(M, d)$, kde $M$ je univerzum objektů a $d$ je vzdálenostní funkce (metrika).
* **Čtyři základní axiomy metriky:** Aby byla funkce $d$ uznána za regulérní metriku, musí pro libovolné tři prvky $x, y, z \in M$ striktně a bez výjimek splňovat tato pravidla:
  1. **Nezápornost:** $d(x, y) \geq 0$ (vzdálenost nemůže být záporná).
  2. **Identita:** $d(x, y) = 0 \iff x = y$ (nulová vzdálenost existuje tehdy a jen tehdy, pokud jde o identické objekty).
  3. **Symetrie:** $d(x, y) = d(y, x)$ (vzdálenost z bodu $x$ do $y$ je stejná jako z $y$ do $x$).
  4. **Trojúhelníková nerovnost:** $d(x, z) \leq d(x, y) + d(y, z)$ (cesta přes mezilehlý bod $y$ nemůže být nikdy kratší než přímá spojnice z $x$ do $z$).
* **Fundamentální význam trojúhelníkové nerovnosti:** Absolutní základ veškerého metrického indexování. Umožňuje matematicky odvozovat spodní a horní odhady vzdáleností mezi objekty v prostoru, aniž by je systém musel reálně měřit, což slouží jako klíčový mechanismus pro odřezávání (*pruning*) nerelevantních větví indexu.
* **Varianty při oslabení axiomů:**
  * *Pseudometrika:* Nesplňuje směr $\Leftarrow$ u identity. Různé objekty mohou mít nulovou vzdálenost ($d(x, y) = 0$ pro $x \neq y$).
  * *Kvazimetrika:* Nesplňuje symetrii ($d(x, y) \neq d(y, x)$, např. jednosměrné ulice v logistické síti).
  * *Semimetrika:* Nesplňuje trojúhelníkovou nerovnost. Nad semimetrickým prostorem nelze vybudovat standardní prořezávací indexové struktury.
* **Koncepční rozdíl: Vektorový vs. Metrický prostor:**
  * *Vektorový prostor (Lineární):* Striktně definován souřadnicovými osami a absolutními pozicemi (bázemi). Prvky lze sčítat, násobit skalárem a existuje zde absolutní počátek souřadnic $[0,0,\dots,0]$. Každý normovaný vektorový prostor lze vyjádřit jako metrický.
  * *Metrický prostor:* Mnohem obecnější, abstraktnější koncept. **Neexistují zde žádné osy, souřadnice ani počátek a objekty nelze sčítat.** Jedinou dostupnou informací je vzdálenostní funkce $d(x,y)$ vracející reálné číslo pro dvojici prvků.
* **Čistě metrické (ne-vektorové) prostory:** Prostory, které nelze popsat souřadnicemi, ale splňují axiomy metriky (lze nad nimi stavět např. *M-Tree*):
  * *String Spaces (Řetězce):* Objekty jsou texty a metrikou je *Levenshteinova (Edit) vzdálenost* počítající minimální počet editačních operací (vložení, smazání, záměna) pro transformaci slov.
  * *Set Spaces (Množiny):* Objekty jsou nákupní košíky nebo sady příznaků a metrikou je *Jaccardova vzdálenost* ($d_J = 1 - \frac{|A \cap B|}{|A \cup B|}$).
  * *Graph Spaces (Grafy):* Objekty jsou uzly v síti a metrikou je délka nejkratší propojovací cesty.
* **Standardní Minkowského metriky ($L_p$ normy):** Rodina geometrických metrik v lineárních prostorech $\mathbb{R}^n$:
  * $p=1$: *Manhattan (City-block) vzdálenost* ($L_1$, součet absolutních rozdílů složek).
  * $p=2$: *Eukleidovská vzdálenost* ($L_2$, klasická přímočará geometrická vzdálenost).
  * $p \to \infty$: *Čebyševova vzdálenost* ($L_\infty$, definována jako čisté maximum z absolutních rozdílů jednotlivých složek: $\max |x_i - y_i|$).
</details>

<details>
<summary>Extrakce popisovačů a jejich vztah s člověkem vnímanou podobností a typy dotazů a jejich definice</summary>

* **Extrakce popisovačů (Feature Extraction):** Proces transformace komplexních, nestrukturovaných objektů reálného světa do podoby kompaktní matematické reprezentace — **příznakového vektoru (popisovače / descriptorů)**. Může běžet na exaktních algoritmech (*hand-crafted features*, např. barevné histogramy) nebo na hlubokých neuronových sítích (*deep learning*), které objekty projektují jako high-dimensional **embeddingy**.
* **Sémantická propast (Semantic Gap):** Klíčová výzva podobnostního vyhledávání. Představuje propastný rozdíl mezi tím, jak data technicky reprezentuje počítač (nízkoúrovňová data, např. matice RGB pixelů), a tím, jak je přirozeně interpretuje člověk (vysokoúrovňové sémantické koncepty a emoce). Cílem extrakce je dosáhnout stavu, kdy matematická blízkost v metrickém prostoru věrně odráží člověkem vnímanou sémantickou podobnost.
* **Čtyři základní typy podobnostních dotazů:**
1. **Range Query (Rozsahový dotaz):** Vyhledá všechny objekty z databáze $X$, jejichž metrická vzdálenost od dotazu $q$ nepřekračuje fixně stanovený uživatelský poloměr tolerance $r$:
   $$\text{Range}(q, r) = \{ x \in X \mid d(q, x) \leq r \}$$
   *Limit:* V málo hustých prostorech hrozí vrácení prázdné množiny, v přehuštěných prostorech naopak zahlcení výsledky.
2. **k-Nearest Neighbor Query (k-NN / Dotaz na k nejbližších sousedů):** Vrátí exaktní množinu $A \subseteq X$ obsahující přesně $k$ prvků ($|A| = k$), pro které platí, že žádný mimostojící objekt z databáze není k dotazu $q$ blíže než objekty uvnitř této výsledné množiny:
   $$\forall y \in A, \forall z \in X \setminus A: d(q, y) \leq d(q, z)$$
   *Implementace:* V indexech se vyhodnocuje jako rozsahový dotaz s dynamicky se zmenšujícím vyhledávacím poloměrem regulovaným prioritní frontou dosavadních top-$k$ kandidátů.
3. **Reverse k-Nearest Neighbor Query (Rk-NN / Reverzní dotaz):** Vyhledá všechny objekty v databázi $X$, pro které je zadaný dotazový prvek $q$ jedním z jejich $k$ nejbližších sousedů. Zkoumá situaci z perspektivy samotných uložených dat (odpovídá na otázku: *„Pro které existující objekty je tento nový prvek atraktivní/blízký?“*):
   $$\text{Rk-NN}(q) = \{ x \in X \mid q \in \text{k-NN}(x) \}$$
   Hojně se využívá v cíleném marketingu, analýze vlivu uzlů nebo při profilování anomálií.
4. **Similarity Join Query (Podobnostní propojení):** Operátor pracující nad dvěma sadami dat $X$ a $Y$. Vyhledá všechny vzájemné dvojice prvků $(x, y)$, jejichž vzdálenost je menší nebo rovna zadané byznys prahové hodnotě $\mu$:
   $$X \bowtie_{\mu} Y = \{ (x, y) \in X \times Y \mid d(x, y) \leq \mu \}$$
   Představuje analogii relačního `JOIN`, avšak namísto exaktní rovnosti atributů vyhodnocuje metrickou blízkost. Má extrémní naivní výpočetní složitost $\mathcal{O}(|X| \cdot |Y|)$, klíčová je pro odstraňování duplicit a čištění dat.
</details>

<details>
<summary>Principy indexování a dělení dat</summary>

* **I/O a CPU limity Brute-Force vyhledávání:** Sekvenční skenování databáze (*Sequential Scan*) vyžaduje vypočítat vzdálenost $d(q, x)$ pro každý uložený prvek, což má lineární složitost $\mathcal{O}(N)$. Protože výpočet komplexních metrik je procesorově extrémně drahý, cílem indexování je prostor strukturovat a rozdělit, aby bylo možné velké bloky dat při vyhledávání bezpečně ignorovat (prořezat).
* **Pivoty (Reference Points):** Vybrané reálné objekty z databáze, které metrické indexy používají jako opěrné kotevní body pro relativní strukturování a štěpení prostoru (jelikož v čistě metrickém prostoru neexistují fixní souřadnicové osy).
* **Základní principy dělení dat:**
  * **Ball Partitioning (Dělení do metrických koulí):** Zvolí se jeden pivot $p \in X$ a dělící poloměr $r_m$ (zpravidla matematický medián vzdáleností okolních prvků k tomuto pivotu). Data se striktně rozštěpí na vnitřní region uvnitř koule ($d(p, x) \leq r_m$) a vnější region vně této koule ($d(p, x) > r_m$). Tvoří základ hierarchií jako **VP-Tree (Vantage Point Tree)**.
  * **Generalized Hyperplane Partitioning (Dělení zobecněnou nadrovinou):** Zvolí se dva samostatné pivoty $p_1, p_2 \in X$. Prostor se imaginárně prořízne nadrovinou nacházející se přesně uprostřed mezi nimi. Každý objekt z databáze je následně alokován k tomu pivotu, ke kterému má geometricky blíže. Využívá se v **GHT (Generalized Hyperplane Tree)** nebo v dynamickém, diskově orientovaném **M-Tree**.
* **Space Partitioning vs. Data Partitioning:**
  * *Space Partitioning (Dělení prostoru):* Rozsekává topologický prostor na fixní oblasti nezávisle na poloze či přítomnosti reálných datových objektů. Výsledné regiony jsou striktně **disjunktní** (nikdy se nepřekrývají), avšak mohou zůstat zcela prázdné (napž. *K-D Tree, Quadtree*).
  * *Data Partitioning (Dělení dat):* Rozděluje množinu konkrétních existujících datových objektů do hierarchických shluků na základě jejich vzájemných relací a vzdáleností. Obalové regiony (metrické koule) se v prostoru **velmi často geometricky překrývají (overlap)**, regiony se plně přizpůsobují distribuci dat a nikdy nejsou prázdné (např. *M-Tree, R-Tree*).
</details>

<details>
<summary>Pivoting</summary>

* **Pivoting:** Výkonná technika eliminace nerelevantních kandidátů bez nutnosti počítat jejich reálnou vzdálenost k dotazu $q$. Je stoprocentně závislá na platnosti **trojúhelníkové nerovnosti**. Vzdálenosti mezi datovými objekty $x$ a fixním pivotem $p$ (hodnoty $d(p, x)$) jsou exaktně předpočítány během fáze budování indexu. Při provádění dotazu systém spočítá pouze jedinou vzdálenost: $d(q, p)$.
* **Matematická prořezávací podmínka (Pruning Condition):** Z trojúhelníkové nerovnosti vyplývá, že spodní odhad vzdálenosti je dán jako $|d(p, x) - d(q, p)| \leq d(q, x)$. Pro rozsahový dotaz s poloměrem $r$ platí:
  $$\text{Pokud } |d(p, x) - d(q, p)| > r, \text{ pak zaručeně platí } d(q, x) > r$$
  Pokud je tato podmínka splněna, objekt $x$ (nebo celý podstrom) je okamžitě **vyřazen (prořezán)** a výpočet drahé vzdálenosti $d(q, x)$ se zcela přeskočí.
* **Klíčová prořezávací pravidla (Constraints):**
  * **A) Object-Pivot Distance Constraint:** Využívá přímo výše uvedený vztah. Pokud absolutní rozdíl vzdáleností k fixnímu globálnímu pivotu překročí poloměr $r$, objekt $x$ vypadává. Základ lineárních tabulkových indexů (*Pivot Tables*).
  * **B) Range-Pivot Distance Constraint (Obalové koule):** Aplikuje se v hierarchických stromech (např. *M-Tree*), kde uzly reprezentují celé podprostory — metrické koule definované pivotem $p$ a poloměrem pokrytí $r_p$ (což je maximální vzdálenost od $p$ k jakémukoliv prvku uvnitř daného podstromu). Celý podstrom lze prořezat naráz, pokud platí: $d(q, p) - r_p > r$. Šetří tisíce výpočtů.
  * **C) Pivot-Pivot Distance Constraint:** Využívá předpočítané vzdálenosti mezi samotnými pivoty navzájem ($d(p_1, p_2)$) k eliminaci celého regionu kolem sub-pivota $p_1$, aniž by se musela kalkulovat vzdálenost $d(q, p_1)$.
  * **D) Double-Pivot Distance Constraint:** Kombinuje informace od dvou různých pivotů ($p_1, p_2$) vůči jednomu objektu $x$, čímž zpřesňuje a zužuje spodní odhad polohy objektu v prostoru pro maximalizaci prořezávacího efektu za cenu vyšších nároků na uložení metadat.
* **Dve fáze algoritmu Pivot Filtering:**
  1. *Fáze filtrování (Filtering State):* Postupně se počítají vzdálenosti $d(q, p)$ k jednotlivým pivotům a aplikují se prořezávací pravidla nad tabulkou předpočítaných hodnot. Pracuje se výhradně s levnými skalárními operacemi (rozdíly reálných čísel), množina kandidátů se radikálně zužuje.
  2. *Fáze ověřování (Refinement State):* Pro objekty, které nebyly filtrem prořezány (kandidáti), se spočítá reálná, drahá metrická vzdálenost $d(q, x)$ a ty, které projdou, tvoří finální výsledek.
</details>

<details>
<summary>Srovnání s tradičními indexy (B+ trees)</summary>

* **Totální uspořádání (Total Ordering) u B+ stromů:** B+ stromy z relačních databází staví svou excelentní logaritmickou složitost $\mathcal{O}(\log N)$ na předpokladu striktního jednorozměrného (1D) uspořádání domény. Pro každé dva klíče lze exaktně říct, zda platí $a < b$, $a = b$ nebo $a > b$. Vyhledávací algoritmus při sestupu stromem vždy přesně ví, do kterého jediného disjunktního sub-intervalu má pokračovat.
* **Proč B+ stromy strukturálně selhávají u podobnostního vyhledávání:**
  * *Absence lineárního řazení:* Vícerozměrný prostor (např. 512-dimenzionální embedding) nelze seřadit na 1D přímku bez naprosté destrukce sémantického sousedství prvků. Pokusy o mapování prostoru SFC křivkami (Z-order, Hilbert) selhávají pro dimenze vyšší než 3, protože prostorově blízké objekty skončí na přímce extrémně daleko od sebe.
  * *Geometrický překryv regionů:* V metrických indexech (např. *M-Tree*) jsou podprostory ohraničeny obalovými koulemi, které se z logiky distribuce dat v prostoru velmi často **geometricky překrývají (overlap)**. Pokud dotaz $q$ zasáhne zónu překryvu, vyhledávací algoritmus musí expandovat do **všech těchto paralelních větví současně**, čímž se stromové vyhledávání mění na drahé prohledávání grafu.
* **Prokletí dimenzionality (Curse of Dimensionality):** Geometrický fenomén kolapsu vícerozměrných prostorů. Se stoupající dimenzí ($n \to \infty$) dochází k tzv. **koncentraci mír vzdálenosti** (*Distance Concentration*) — rozdíl mezi vzdáleností k nejbližšímu objektu a nejvzdálenějšímu objektu se limitně blíží nule:
  $$\lim_{n \to \infty} \frac{D_{\max} - D_{\min}}{D_{\min}} = 0$$
* **Důsledek pro kolaps indexů:** Ve vysoké dimenzi jsou všechny objekty od sebe přebytečně stejně daleko a leží na hyperpovrchu prostoru. Obalové koule uzlů v indexu musí drasticky zvětšit svůj poloměr pokrytí $r_p$, aby vůbec dokázaly pojmout datové prvky. Tím se index zaplní obřími, masivně se překrývajícími regiony. Jakýkoliv dotaz prohne téměř 100 % všech regionů ve stromu, struktura kompletně kolabuje a vyhodnocení degraduje na **sekvenční sken (Sequential Scan)**, který je kvůli režii správy stromu pomalejší než naivní brute-force porovnání.
* **Opatření proti prokletí dimenzionality:**
  * *1. Redukce dimenzionality:* Transformace vysokorozměrných dat do podprostoru s nižším počtem proměnných (*intrinsic dimensionality*) pomocí lineárních metod (*PCA*), nelineárních deep learning modelů (*Autoenkodéry*) nebo náhodných projekcí splňujících *Johnson-Lindenstrauss lemma* (garantuje zachování relativních vzdáleností s malou chybou).
  * *2. Aproximované vyhledávání (ANN):* Obcházení geometrických kolapsů stromů nasazením pokročilých struktur, jako jsou **grafové indexy (HNSW — Hierarchical Navigable Small World)**, které propojují data do navigačních sítí malého světa, techniky **vektorového kvantování (Product Quantization — PQ)**, nebo lokalizovaného hashování (**LSH**).
</details>

# Cloudové počítání a distribuované databáze

<details>
<summary>Základní principy cloud computingu</summary>

* **Cloud computing:** Model umožňující všudypřítomný, pohodlný síťový přístup na vyžádání ke sdílenému fondu konfigurovatelných výpočetních zdrojů (sítě, servery, úložiště, aplikace), které lze rychle alokovat a uvolnit s minimálním úsilím při správě.
* **On-demand self service (Samoobsluha na vyžádání):** Uživatel si může automaticky sjednat výpočetní kapacity (např. čas serveru nebo diskový prostor) zcela samostatně bez nutnosti lidské interakce s poskytovatelem služeb.
* **Broad network access (Široký síťový přístup):** Služby cloudu jsou dostupné přes standardní sítě prostřednictvím mechanismů podporujících heterogenní klientské platformy (mobilní telefony, tablety, notebooky).
* **Resource pooling (Sdílení zdrojů):** Výpočetní zdroje poskytovatele jsou dynamicky sdíleny a alokovány více spotřebitelům pomocí modelu *multi-tenancy*. Zákazník zpravidla nezná přesné fyzické umístění hardware, ale může specifikovat polohu na vyšší úrovni abstrakce (např. stát či datacentrum).
* **Rapid elasticity (Rychlá elasticita):** Výpočetní kapacity mohou být elasticky, flexibilně a často automaticky navyšovány či uvolňovány, aby systém okamžitě reagoval na aktuální zátěž směrem nahoru i dolů.
* **Measured service (Měřená služba):** Cloudové systémy automaticky monitorují, řídí a optimalizují zdroje na základě exaktního měření spotřeby (např. velikost úložiště, procesorový čas, šířka pásma), což umožňuje transparentní vyúčtování typu *Pay-as-you-go*.
* **Modely nasazení (Deployment Models):**
  * *Public cloud (Veřejný cloud):* Infrastruktura je přístupná široké veřejnosti a vlastněna velkou organizací prodávající cloudové služby (např. AWS, Azure).
  * *Private cloud (Soukromý cloud):* Infrastruktura je dedikována a provozována výhradně pro potřeby jedné konkrétní organizace (např. interní datové centrum).
  * *Community cloud (Komunitní cloud):* Infrastruktura je sdílená několika organizacemi, které spojuje společný zájem, cíle, požadavky na bezpečnost nebo shoda s předpisy (compliance).
  * *Hybrid cloud (Hybridní cloud):* Propojení dvou či více odlišných cloudových infrastruktur (veřejné, soukromé, komunitní) pomocí standardizované technologie umožňující bezpečný přenos dat a aplikací.
* **Modely služeb (Service Models):**
  * *IaaS (Infrastructure as a Service):* Poskytování surových, virtualizovaných hardwarových zdrojů (virtuální stroje, sítě, úložiště), kde si zákazník sám spravuje OS a kompletní software.
  * *PaaS (Platform as a Service):* Poskytování běhového prostředí (runtime) a nástrojů pro vývoj aplikací. Uživatel nespravuje podkladový HW ani OS, dodává pouze samotný kód aplikace.
  * *SaaS (Software as a Service):* Poskytování hotových koncových aplikací běžících v cloudu, ke kterým uživatel přistupuje vzdáleně (nejčeštěji přes webový prohlížeč), aniž by řešil kód či platformu.
* **Serverless / FaaS (Function as a Service):** Výpočetní model, kde uživatel nespravuje žádné servery a pouze nahrává krátké, jednoúčelové funkce, které se spouštějí striktně na základě událostí (*Event-driven*). Platí se pouze za čistý čas běhu funkce; umožňuje škálování až na nulu.
* **Cold Start (Studený start) u FaaS:** Výkonnostní prodleva (od stovek milisekund po sekundy) při prvním zavolání serverless funkce po delší nečinnosti. Platforma musí nejprve alokovat zdroje, nastartovat izolovaný kontejner a inicializovat runtime prostředí, než funkci vykoná. Následná volání (*Warm Start*) jsou okamžitá, protože kontejner zůstává dočasně v paměti.
</details>

<details>
<summary>Infrastruktura jako služba (IaaS)</summary>

* **Infrastruktura jako služba (IaaS):** Nejnižší a nejvíce flexibilní úroveň cloudových služeb poskytující virtualizované hardwarové prostředky na vyžádání.
* **Komponenty IaaS:**
  * *Compute (Výpočet):* Alokace virtuálních strojů (VM) s definovaným počtem virtuálních CPU jader a fixní kapacitou operační paměti RAM.
  * *Storage (Úložiště):* Poskytování virtuálních pevných disků (blokového úložiště) nebo škálovatelných objektových úložišť.
  * *Networking (Sítě):* Virtuální privátní sítě (VPC), firewally, směrovače, load balancery a flexibilní přidělování IP adres.
* **Nákladová efektivita IaaS:** Zákazník eliminuje investiční náklady (*CapEx*) na nákup fyzického hardware a transformuje je na provozní náklady (*OpEx*) na základě reálné spotřeby zdrojů.
* **Odpovědnost uživatele v IaaS:** Uživatel má plnou kontrolu a konfiguraci nad operačním systémem, bezpečností, síťovými pravidly, aktualizacemi a kompletním softwarovým zásobníkem.
* **Služby top poskytovatelů:** Amazon EC2 (AWS), Azure Virtual Machines (Microsoft), Compute Engine (GCP).
</details>

<details>
<summary>Virtualizace a kontejnery</summary>

* **Virtualizace:** Technologie vytvářející abstraktní softwarovou vrstvu nad fyzickým hardwarem za účelem spouštění více nezávislých operačních systémů současně na jednom stroji.
* **Hypervisor:** Řídicí software (emulátor), který spravuje, izoluje a spravedlivě rozděluje fyzické systémové zdroje (CPU, RAM, disky) mezi jednotlivé virtuální stroje.
* **Typy hypervisorů:** *Typ 1 (Bare-metal)* běží přímo na surovém fyzickém hardware bez podkladového OS (např. VMware ESXi, KVM), což je standard pro datová centra. *Typ 2 (Hosted)* běží jako aplikace nad běžným hostitelským operačním systémem (např. VirtualBox).
* **Virtuální stroj (Virtual Machine — VM):** Izolovaný logický celek obsahující kompletní kopii operačního systému (*Guest OS*), virtuální ovladače, aplikaci a všechny knihovny. Vyžaduje vysokou paměťovou a výpočetní režii (*overhead*) a startuje v řádu minut.
* **Kontejnerizace:** Odlehčená forma virtualizace na úrovni procesů, která neobsahuje vlastní operační systém, ale sdílí jádro (*kernel*) hostitelského operačního systému.
* **Kontejner:** Izolovaný, přenositelný softwarový balíček obsahující výhradně kód aplikace a všechny specifické závislosti (knihovny, konfigurace). Startuje v řádu milisekund/sekund a zabírá minimum místa (MB).
* **Container Engine:** Softwarové prostředí (např. *Docker, Podman*), které zajišťuje izolaci a spouštění kontejnerů nad hostitelským systémem.
* **Orchestrace kontejnerů:** Automatizovaná správa, plánování, síťové propojení a horizontální škálování velkého množství kontejnerů v produkčním clusteru. Průmyslovým standardem je platforma **Kubernetes (K8s)**.
* **Mechanismy nasazení aplikací (Srovnání):**
  * *Bare Metal:* Aplikace běží přímo na OS fyzického serveru. Žádná virtualizace, riziko konfliktů knihoven, špatné využití výkonu.
  * *Virtualized:* Více VM běžících nad hypervisorem. Silná izolace různých OS, vysoká bezpečnost, ale velká režie na duplicitní operační systémy.
  * *Containerized:* Kontejnery sdílející jedno jádro OS. Minimální režie, maximální rychlost a hustota aplikací na server.
  * *Containerized on Virtualized:* Standard v cloudu, kde kontejnery běží uvnitř pronajatých virtuálních strojů (spojení silné IaaS izolace s aplikační flexibilitou kontejnerů).
</details>

<details>
<summary>Migrace na cloud</summary>

* **Migrace na cloud:** Proces strategického a technologického přesunu digitálních aktiv, databází, aplikací a IT infrastruktury z lokálního on-premise prostředí do cloudu.
* **Model 6 R (Strategie migrace podle Gartnera):**
  1. *Retiring (Vyřazení):* Identifikace a trvalé vypnutí nadbytečných, zastaralých aplikací, které již podniku nepřinášejí hodnotu.
  2. *Retaining (Ponechání):* Rozhodnutí nemigrovat danou aplikaci a ponechat ji v on-premise prostředí (např. z důvodu přísné legislativy nebo potřeby extrémně nízké latence k lokálnímu HW).
  3. *Repurchasing (Odkoupení):* Kompletní opuštění stávající interní aplikace a přechod na hotové cloudové komerční řešení ve formě SaaS (např. přechod z lokálního e-mail serveru na Microsoft 365).
  4. *Rehosting (Lift-and-Shift):* Přímý přesun aplikace do cloudu v poměru 1:1 bez jakýchkoliv úprav v jejím zdrojovém kódu či architektuře (nasazení na IaaS virtuální stroje). Fast-track migrace.
  5. *Replatforming (Lift-and-Reshape):* Přesun aplikace s drobnými optimalizacemi pro lepší využití cloudu bez změny jádra aplikace (např. přesun lokální DB do plně spravované cloudové PaaS databáze).
  6. *Refactoring / Re-architecting (Přepsání):* Kompletní přepsání architektury aplikace tak, aby byla plně *cloud-native*. Typicky zahrnuje rozbití robustního monolitu na kontejnerizované mikroslužby a serverless funkce pro dosažení elasticity.
* **Výzvy a rizika migrace:**
  * *Vendor Lock-in:* Stav nebezpečné závislosti na proprietárních technologiích jednoho cloudového poskytovatele, což dramaticky komplikuje případný budoucí odchod.
  * *Egress Fees:* Poplatky, které si cloudoví poskytovatelé účtují za stahování dat z cloudového prostředí směrem ven do internetu.
  * *Data Sovereignty:* Povinnost splňovat legislativní požadavky na fyzické umístění dat (např. GDPR vyžaduje přísnou kontrolu nad ukládáním osobních dat evropských občanů mimo EU).
</details>

<details>
<summary>Bezpečnost služeb v cloudu</summary>

* **Model sdílené odpovědnosti (Shared Responsibility Model):** Základní bezpečnostní rámec dělící úkoly mezi poskytovatele a zákazníka. Poskytovatel plně ručí za **bezpečnost cloudu** (fyzické zabezpečení datacenter, hardware, hypervisory). Zákazník nese stoprocentní odpovědnost za **bezpečnost v cloudu** (záplatování OS instancí, zabezpečení dat, správa přístupů, konfigurace firewallů).
* **Identity and Access Management (IAM):** Robustní systém pro správu identit a řízení přístupových práv, určující které entity mohou provádět specifické operace nad cloudovými zdroji.
* **Princip nejnižších privilegií (Least Privilege):** Striktní bezpečnostní pravidlo zajišťující, že uživatel nebo proces má přidělena pouze ta minimální práva, která jsou nezbytně nutná pro výkon jeho práce, což minimalizuje blast radius při kompromitaci účtu.
* **Zero Trust Architecture (Architektura nulové důvěry):** Bezpečnostní koncept založený na principu „nikdy nedůvěřuj, vždy prověřuj“. Každý požadavek na přístup ke zdrojům musí být striktně autentizován a autorizován, i když přichází zevnitř zdánlivě bezpečné sítě.
* **Šifrování dat ve všech stavech:**
  * *Data at Rest (Uložená data):* Šifrování persistentních disků, databází a objektových úložišť (ochrana před fyzickým zneužitím hardwaru).
  * *Data in Transit (Data v pohybu):* Šifrování datových toků přenášených po síti pomocí kryptografických protokolů (HTTPS, TLS, IPsec VPN).
  * *Data in Use (Data při zpracování):* Ochrana dat přímo v procesoru a RAM během samotného výpočtu pomocí hardwarově izolovaných enkláv (*Confidential Computing*).
* **VPC / VNet (Virtual Private Cloud):** Logicky izolovaná virtuální síť v rámci veřejného cloudu, která plně separuje síťové zdroje zákazníka od ostatních nájemců infrastruktury.
* **Security Groups:** Stavové virtuální firewally aplikované přímo na úrovni síťové karty jednotlivých virtuálních serverů, které striktně omezují příchozí a odchozí provoz na vybraných portech a IP adresách.
</details>

<details>
<summary>Horizontální a vertikální škálovatelnost</summary>

* **Škálovatelnost:** Schopnost infrastruktury a aplikací plynule absorbovat zvýšený nápor požadavků a dat adekvátním navýšením hardwarových prostředků bez degradace výkonu.
* **Vertikální škálování (Scaling Up / Výkonové):** Navyšování výpočetního výkonu **jednoho jediného stávajícího uzlu** (přidání silnějšího CPU, větší kapacity RAM či rychlejšího NVMe disku).
  * *Limity:* Naráží na pevný hardwarový strop jedné základní desky, vyžaduje dočasnou odstávku při upgradu, představuje nebezpečný *Single Point of Failure (SPOF)* a finanční náklady rostou silně nelineárně.
* **Horizontální škálování (Scaling Out / Kapacitní):** Zvyšování celkového výkonu **přidáváním dalších samostatných uzlů** do distribuovaného clusteru, které pracují paralelně jako jeden logický celek.
  * *Výhody:* Nabízí prakticky neomezený růst, vysokou dostupnost (*High Availability*) a redundanci (při výpadku jednoho stroje zbytek clusteru dál funguje). Vyžaduje však distribuovanou architekturu, bezstavovost (*statelessness*) a load balancing.
* **Load Balancer:** Síťové zařízení nebo softwarová služba, která distribuuje příchozí uživatelský provoz a požadavky mezi více backendových serverů v clusteru za účelem jejich rovnoměrného vytížení a eliminace přetížení.
* **Auto-scaling:** Automatizační mechanismus cloudu, který na základě sledování metrik v reálném čase (např. vytížení CPU nad 80 %) plynule a autonomně přidává nebo odebírá instance virtuálních strojů či kontejnerů bez zásahu správce.
* **Sharding:** Horizontální dělení obřích databázových tabulek na menší, nezávislé a samostatně dotazovatelné části (*shardy*) distribuované napříč disky různých uzlů v clusteru na základě distribučního klíče (*shard key*).
* **Read Replicas:** Dedikované kopie hlavní databáze synchronizované asynchronním způsobem, které jsou vyhrazeny výhradně pro odbavování čtecích operací, čímž radikálně odlehčují hlavnímu (zápisovému) uzlu.
</details>

<details>
<summary>Distribuované databáze a CAP teorém</summary>

* **Distribuovaná databáze:** Systém, ve kterém jsou data fyzicky uložena a spravována na více autonomních výpočetních uzlech propojených počítačovou sítí, přičemž uživateli se navenek prezentuje jako jednotný logický celek.
* **CAP teorém (Brewerův teorém):** Základní teorém distribuovaných systémů dokazující, že v distribuované databázi nelze v jeden okamžik stoprocentně zajistit všechny tři následující vlastnosti současně:
  * *Consistency (Konzistence — C):* Všichni klienti napříč všemi uzly vidí v identický časový okamžik naprosto stejná a nejčerstvější data.
  * *Availability (Dostupnost — A):* Každý požadavkem zasažený uzel, který je funkční, musí vrátit odpověď (nesmí ohlásit chybu), i když tato data nemusí být v důsledku zpoždění sítě zcela nejnovější.
  * *Partition Tolerance (Odolnost proti rozdělení — P):* Celý systém dokáže korektně fungovat a odbavovat požadavky i v situaci, kdy dojde k výpadku nebo přerušení síťové komunikace (*network partition*) mezi uzly.
* **Vztah v praxi:** Síťové poruchy a výpadky komunikace jsou v distribuovaném prostředí nevyhnutelným jevem ($P$ je fixní podmínka). Systémy se proto musí striktně rozhodnout a rozdělit na:
  * *CP systémy (Konzistence + Odolnost):* Upřednostňují absolutní přesnost. Při rozpadu sítě uzly raději odmítnou odpovědět (obětují dostupnost $A$), než aby riskovaly vrácení nekonzistentních dat.
  * *AP systémy (Dostupnost + Odolnost):* Upřednostňují rychlost a kontinuitu. Při rozpadu sítě uzly odpoví tím, co lokálně vědí, i za cenu, že se data v různých částech světa liší (obětují okamžitou konzistenci $C$).
* **PACELC teorém:** Rozšíření CAP teorému. Říká, že pokud nastane rozdělení sítě (**P**artition), volíme mezi Dostupností (**A**) a Konzistencí (**C**). V opačném případě, kdy systém běží normálně (**E**lse), musíme volit kompromis mezi Latencí sítě (**L**) a přísnou Konzistencí (**C**).
</details>

<details>
<summary>NoSQL přístup a BASE model</summary>

* **NoSQL (Not Only SQL):** Třída databázových systémů navržená pro ukládání nestrukturovaných či polostrukturovaných dat, která upouští od rigidních schémat relačního modelu s cílem dosáhnout masivní horizontální škálovatelnosti a vysoké propustnosti (*throughput*).
* **Základní principy NoSQL:**
  * *Absence pevných relací:* Data nejsou svázána cizími klíči, což eliminuje výpočetně drahé operace spojování tabulek (`JOIN`).
  * *Schema-less (Flexibilní schéma):* Datové záznamy (dokumenty/řádky) v rámci jedné kolekce mohou mít dynamicky odlišnou vnitřní strukturu a datové typy.
  * *Horizontální orientace:* Architektura je nativně navržena pro rozprostření zátěže (sharding a replikaci) na stovky levných komoditních serverů v clusteru.
* **BASE model:** Alternativní databázový model k tradičnímu transakčnímu modelu **ACID**, typický pro NoSQL systémy, které preferují vysokou elasticitu a dostupnost před okamžitou atomickou konzistencí:
  * *Basically Available (Základní dostupnost):* Systém garantuje, že databáze bude pro uživatele dostupná a funkční i při výpadku části uzlů (selhání se projevuje lokálně, ne celkovým kolapsem).
  * *Soft state (Měkký stav):* Stav dat a uzlů se může v čase plynule měnit i bez přímých uživatelských vstupů, což je způsobeno asynchronním "probubláváním" a replikací dat na pozadí.
  * *Eventual consistency (Eventuální konzistence):* Data nejsou synchronizována na všech uzlech okamžitě po zápisu. Systém však garantuje, že pokud ustanou nové zápisy, všechny repliky se časem zesynchronizují a dosáhnou identického stavu.
</details>

<details>
<summary>Modely konzistence a distribuované transakce</summary>

* **Silná konzistence (Strong Consistency):** Přísný model zaručující, že bezprostředně po úspěšném zápisu jakýkoliv následný čtenářský dotaz získá nejnovější zapsanou hodnotu, bez ohledu na to, ke kterému replikačnímu uzlu v síti se připojí.
* **Konzistence čtení vlastních zápisů (Read-your-writes):** Specifický model garantující, že uživatel $A$ po provedení aktualizace dat uvidí při svém dalším dotazu tuto změnu vždy okamžitě, přestože pro ostatní globální uživatele může být hodnota v důsledku asynchronní replikace ještě chvíli neviditelná.
* **Kauzální konzistence (Causal Consistency):** Zaručuje zachování chronologického a sémantického pořadí operací, které jsou v kauzálním vztahu. Pokud operace $B$ přímo reaguje na operace $A$, každý uzel v síti musí uživatelům zobrazit nejprve krok $A$ a až poté krok $B$ (např. vlákna komentářů).
* **Laditelná konzistence (Tunable Consistency):** Architektonická vlastnost některých NoSQL databází (např. Apache Cassandra), umožňující vývojáři explicitně konfigurovat úroveň přísnosti konzistence pro každou jednotlivou I/O operaci zvlášť (volba optimálního trade-off mezi latencí a přesností).
* **Kvorum (Quorum):** Mechanismus distribuovaného hlasování zajišťující silnou konzistenci splněním matematické podmínky:
  $$W + R > N$$
  Kde $N$ je celkový počet replikačních uzlů, $W$ je počet uzlů, které musí potvrdit úspěšný zápis, a $R$ je počet uzlů vyžadovaných pro úspěšné čtení. Pokud tato nerovnost platí, zápisová a čtecí množina uzlů se garantovaně protnou v alespoň jednom uzlu s nejčerstvější verzí dat.
* **Distribuované transakce:** Transakční operace, jejichž atomické provedení (vše nebo nic) musí být synchronně garantováno napříč více fyzicky separovanými síťovými uzly současně.
* **Two-Phase Commit (2PC):** Klasický dvoufázový koordinační protokol pro řízení distribuovaných transakcí:
  1. *Fáze hlasování (Prepare Phase):* Centrální koordinátor rozešle všem zúčastněným uzlům dotaz, zda jsou připraveny transakci potvrdit. Uzly lokálně uzamknou potřebné zdroje a odpoví Ano/Ne.
  2. *Fáze potvrzení (Commit Phase):* Pokud **všechny** uzly odpověděly kladně, koordinátor rozešle finální příkaz k provedení transakce (*Commit*). Pokud byť jediný uzel odpověděl záporně (nebo neodpověděl), koordinátor nařídí všem uzlům okamžité zrušení změn (*Rollback*). Nevýhodou protokolu je vysoká latence a riziko totálního zablokování zdrojů clusteru při výpadku koordinátora.
</details>

<details>
<summary>Distribuce dat (Replikace a sharding)</summary>

* **Replikace dat:** Strategie ukládání a udržování identických kopií datových sad na více fyzicky nezávislých výpočetních uzlech s cílem maximalizovat dostupnost dat a odolnost vůči hardwarovým haváriím.
* **Modely replikace:**
  * *Master-Slave (Primary-Secondary):* Veškeré zápisové operace jsou striktně směrovány na jediný hlavní uzel (*Master*), který tyto změny asynchronně či synchronně distribuuje na podřízené uzly (*Slaves*). Podřízené uzly odbavují pouze operace čtení, což umožňuje snadné škálování čtenářského výkonu.
  * *Multi-Master (Leaderless):* Zápisové operace mohou být prováděny na jakémkoliv uzlu v clusteru současně. Vyžaduje implementaci komplexních algoritmů pro detekci a automatické řešení datových konfliktů (např. *Vector Clocks* nebo pravidlo *Last-Write-Wins*).
* **Sharding (Horizontální dělení):** Architektonické rozsekání jedné obří logické tabulky/kolekce na menší, samostatné fyzické datové segmenty (*shardy*), kde každý uzel nese pouze dedikovanou část celkového objemu dat.
* **Distribuční klíč (Shard Key):** Atribut (sloupec) vybraný pro výpočet distribuce dat. Databáze nad hodnotou klíče aplikuje hashovací funkci nebo rozsahový filtr, čímž exaktně určí, na který konkrétní fyzický uzel daný řádek patří. Kritický pro rovnoměrné rozložení zátěže bez vzniku přetížených uzlů (*hotspots*).
* **Geodistribuce:** Fyzické rozmístění uzlů distribuované databáze napříč různými geografickými lokalitami, kontinenty a datacentry světa. Zajišťuje minimální síťovou latenci (přiblížením dat ke koncovému uživateli) a stoprocentní kontinuitu provozu při kompletním kolapsu celého regionu (*Disaster Recovery*).
</details>

<details>
<summary>NoSQL modely: Úložiště párů klíč-hodnota (Key-Value)</summary>

* **Úložiště párů klíč-hodnota (Key-Value Store):** Nejjednodušší a nejrychlejší NoSQL databázový model, kde jsou data organizována jako plochá kolekce unikátních vyhledávacích **klíčů** a k nim přiřazených libovolných **hodnot**.
* **Neprůhlednost hodnoty (Blob):** Databázový engine přistupuje k uložené hodnotě jako k surovému, sémanticky neprůhlednému celku (binárnímu blobu či řetězci). Databáze nedokáže nahlížet dovnitř hodnoty, filtrovat podle jejích vnitřních polí ani nad nimi provádět SQL operace. Přístup probíhá výhradně skrze klíč pomocí primitivních operací `PUT`, `GET` a `DELETE`.
* **Výkonnostní charakteristika:** Funguje jako obří distribuovaná hashovací tabulka v RAM či na rychlém disku. Dosahuje konstantní časové složitosti $\mathcal{O}(1)$ pro čtení i zápis, vykazuje extrémně nízkou latenci a je dokonale horizontálně škálovatelná.
* **Typické oblasti nasazení:** Efektivní správa uživatelských relací (*Session Management*), ukládání nákupních košíků v e-shopech, distribuce konfiguračních předvoleb aplikací nebo blesková mezipaměť (*Caching*) předřazená před pomalou relační databázi.
* **Technologie a cloudové služby:** Azure Cache for Redis, Azure Table Storage, Amazon ElastiCache, AWS DynamoDB (v KV režimu), Redis, Memcached.
</details>

<details>
<summary>NoSQL modely: Dokumentové databáze</summary>

* **Dokumentová databáze:** NoSQL systém, který ukládá a spravuje data ve formě polostrukturovaných, člověkem čitelných **dokumentů** (nejčastěji standardizované formáty JSON, BSON nebo XML).
* **Sémantická čitelnost dokumentu:** Na rozdíl od Key-Value úložišť je hodnota pro databázový engine plně transparentní. Databáze zná vnitřní strukturu dokumentu, dokáže nahlížet na jednotlivá pole, indexovat je a provádět nad nimi komplexní filtrační a vyhledávací dotazy.
* **Hierarchické vnořování (Denormalizace):** Dokumenty mohou nativně obsahovat komplexní vnořené pod-objekty, struktury a pole (vztahy `1:N` jsou uloženy přímo uvnitř jednoho dokumentu jako sémantický celek), což odstraňuje nutnost normalizace a drahých spojování tabulek.
* **Flexibilní bezschémový vývoj (Schema-less):** Databáze nevynucuje žádné globální schéma. Každý dokument v rámci jedné kolekce může mít naprosto odlišnou sadu polí, což umožňuje plynulý agilní vývoj a ukládání vysoce variabilních objektů.
* **Typické oblasti nasazení:** Komplexní katalogy produktů s proměnlivými parametry, správa uživatelských profilů na sociálních sítích s nepovinnými poli, systémy pro správu obsahu (CMS) nebo sběr různorodých systémových logů.
* **Technologie a cloudové služby:** MongoDB, Azure Cosmos DB (s podorou MongoDB API), Amazon DocumentDB, Google Cloud Firestore, CouchDB.
</details>

<details>
<summary>NoSQL modely: Grafové databáze</summary>

* **Grafová databáze:** NoSQL systém navržený pro ukládání a dotazování dat, kde jsou vzájemné topologické vztahy mezi entitami zrovna tak důležité jako entity samotné. 
* **Model Labeled Property Graph:** Data jsou reprezentována jako matematický graf složený ze dvou základních entit:
  * *Uzly (Vertices/Nodes):* Reprezentují samostatné objekty (odpovídají řádkům v relační DB).
  * *Hrany (Edges/Relationships):* Explicitně definují orientované vztahy a propojení mezi uzly.
* **Vlastnosti (Properties):** Jak uzly, tak samotné hrany mohou nést libovolnou sadu dvojic klíč-hodnota představující doplňková metadata (např. hrana typu `Koupil` nese vlastnost `Datum_Nakupu`).
* **Bleskové procházení sítě (Index-free Adjacency):** Klíčová vlastnost grafových DB. Každý uzel obsahuje přímé fyzické ukazatele (ukazatele v paměti) na své sousední uzly. Procházení grafu (*traversing*) napříč miliony vazeb probíhá v konstantním čase nezávisle na celkové velikosti databáze, bez nutnosti provádět výpočetně likvidační operace `JOIN`.
* **Typické oblasti nasazení:** Mapování sociálních sítí (hledání přátelství, analýza komunit), pokročilé znalostní grafy (*Knowledge Graphs*), real-time doporučovací systémy nebo robustní detekce podvodů v bankovnictví (detekce kruhových transakcí a podvodných schémat).
* **Technologie a cloudové služby:** Neo4j, Azure Cosmos DB (Gremlin API), Amazon Neptune, ArangoDB.
</details>

<details>
<summary>NoSQL modely: Sloupcově orientované databáze (Wide-column)</summary>

* **Sloupcově orientovaná databáze (Wide-column / Column-family):** NoSQL systém ukládající a organizující data na disku striktně po sloupcích (respektive rodinách sloupců), nikoliv po celých horizontálních řádcích, jak je běžné u RDBMS.
* **Architektura Column Families:** Související sloupce jsou logicky a fyzicky seskupeny do tzv. rodin sloupců, které se na disku ukládají v kuse za sebou. Každý řádek je identifikován klíčem a může obsahovat dynamicky odlišný počet sloupců v rámci dané rodiny (*sparse data* — prázdné buňky nezabírají žádné místo).
* **Vysoká komprese a I/O efektivita:** Vzhledem k tomu, že data v rámci jednoho sloupce jsou identického datového typu, lze aplikovat vysoce účinné kompresní algoritmy. Při exekuci analytického dotazu navíc systém z disku fyzicky načítá **pouze ty konkrétní sloupce, které figurují v dotazu**, což eliminuje zbytečné čtení zbytku širokých řádků.
* **Wide-column (NoSQL) vs. Pure Columnar (Analytické):**
  * *Wide-column:* Optimalizováno pro bleskový zápis a čtení velkých objemů dat podle řádkového klíče v distribuovaném clusteru. Sloupce jsou dynamické (např. *Apache Cassandra, Google Cloud Bigtable, Apache HBase*).
  * *Pure Columnar:* Čistě sloupcová úložiště optimalizovaná pro masivní agregace (`SUM, AVG`) nad celým sloupcem napříč miliardami záznamů v datových skladech. Jsou nevhodná pro zápis jednotlivých řádků (např. *Amazon Redshift, Google BigQuery*, soubory *Apache Parquet*).
* **Typické oblasti nasazení:** Analýza časových řad a masivní sběr telemetrie z IoT senzorů, finanční transakční logy, sledování kompletní uživatelské historie kliknutí (*clickstream analytics*) nebo robustní distribuované Big Data sklady.
</details>

# Softwarové inženýrství

<details>
<summary>Proces vývoje softwaru a procesní modely</summary>

* **Proces vývoje softwaru (Software Process):** Systematický, inženýrský a kontrolovaný soubor činností, jejichž cílem je transformovat uživatelské a byznys požadavky v funkční, spolehlivý a udržitelný softwarový produkt při dodržení časových a finančních limitů.
* **Měřítko vývoje:** Rozlišuje se *Programming-in-the-small* (vývoj v malém měřítku, např. jeden programátor píše izolovaný zálohovací skript) a *Programming-in-the-large* (komplexní vývoj, kdy robustní týmy inženýrů budují rozsáhlý bankovní systém vyžadující striktní plánování a řízení rizik).
* **Klíčové charakteristiky softwarového procesu:**
  * *Srozumitelnost (Comprehensiveness):* Míra, do jaké jsou procesní aktivity jasně a jednoznačně definovány.
  * *Viditelnost (Visibility):* Možnost externího manažera objektivně sledovat reálný progres vývoje na základě schválených milníků a běžících prototypů.
  * *Spolehlivost (Reliability):* Schopnost procesu včas odhalovat skryté chyby a předcházet jim.
  * *Akceptovatelnost (Acceptability):* Míra, do jaké vývojáři definovaný proces přijímají a skutečně v praxi využívají.
  * *Robustnost (Robustness):* Schopnost procesu vyrovnat se s nečekanými komplikacemi (např. zastupitelnost a dokumentace při nemoci klíčového člena týmu).
  * *Udržovatelnost (Maintainability):* Snadnost, s jakou lze samotný proces modifikovat podle nových potřeb organizace.
* **Fáze životního cyklu softwaru (SDLC):**
  1. *Analýza a specifikace požadavků:* Zjišťování a validace potřeb stakeholders, završená dokumentem **SRS** (*Software Requirements Specification*). Požadavky se dělí na **funkční** (co systém dělá, např. storno objednávky) a **nefunkční** (provozní vlastnosti, např. odezva do 200 ms).
  2. *Návrh (Architecture & Design):* Dekompozice systému do logických subsystémů, návrh datových modelů a rozhraní.
  3. *Implementace (Kódování):* Přepis architektury do spustitelného zdrojového kódu.
  4. *Verifikace a validace (V&V):* Verifikace odpovídá na otázku *„Stavíme produkt správně?“* (kontrola kódu vůči specifikaci SRS). Validace odpovídá na otázku *„Stavíme správný produkt?“* (ověření, zda produkt reálně plní skutečné potřeby zákazníka).
  5. *Nasazení (Deployment):* Transfer otestovaného softwaru do produkčního prostředí.
  6. *Provoz a údržba:* Kontinuální správa, odstraňování chyb z provozu a adaptace systému.
* **Vodopádový model (Waterfall):** Rigorózní sekvenční model, kde každá fáze začíná až po formálním dokončení a schválení fáze předchozí. Má pevné milníky. Výhodou je snadné plánování, nevýhodou rigidita a vysoké riziko selhání při změnách požadavků, protože funkční SW vidí zákazník až na úplném konci.
* **Iterace vs. Inkrement:** *Iterace* je cyklické opakování SDLC v kratších časových oknech s cílem postupně zpřesňovat a vylepšovat kvalitu celého systému (vertikální růst). *Inkrement* je postupné přidávání a nasazování ucelených, plně funkčních částí/přírůstků k softwaru (horizontální růst funkcionality).
* **Spirálový model (Spiral — Boehm):** Iterační model postavený na **systematické analýze a řízení rizik**. Vývoj probíhá v cyklech (obrátkách spirály), z nichž každá je rozdělena do čtyř kvadrantů: 1. Určení cílů a omezení, 2. Vyhodnocení alternativ a mitigace rizik (např. tvorba maketových prototypů), 3. Implementace a testování verze, 4. Plánování další iterace. Vhodný pro kritické velkorozsahové systémy.
</details>

<details>
<summary>Metodika Rational Unified Process (RUP)</summary>

* **Rational Unified Process (RUP):** Robustní, těžká (*heavyweight*), objektově orientovaná softwarová metodika. Je charakterizována třemi základními pilíři: je **řízena případy užití** (*Use-Case Driven*), **soustředěna na architekturu** (*Architecture-Centric*) a vyvíjena **iteračně a inkrementálně**. Jako formální modelovací standard využívá jazyk **UML**.
* **Čtyři chronologické fáze RUP a jejich milníky:**
  1. **Inception (Zahájení):** Definice byznys záměru, hrubého rozsahu a ekonomické smysluplnosti projektu. Milníkem je **Vision** (shoda na záměru).
  2. **Elaboration (Projektování / Elaborace):** Mitigace hlavních technologických rizik a stabilizace softwarové architektury. Vytváří se spustitelný architektonický prototyp. Milníkem je **Baseline Architecture** (zafixování architektury).
  3. **Construction (Realizace / Konstrukce):** Masivní kódování a implementace zbývajících use cases na bázi zafixované architektury, integrace komponent a testování. Milníkem je **Initial Capability** (funkční produkt připravený na beta provoz).
  4. **Transition (Předání / Transice):** Nasazení softwaru k reálným koncovým uživatelům, odstraňování provozních chyb a školení. Milníkem je **Product Release** (finální předání produktu).
* **Šest paralelních inženýrských disciplín (Workflows):** Činnosti, které probíhají s různou intenzitou vertikálně napříč všemi čtyřmi fázemi v čase:
  1. *Business Modelling* (pochopení vnitropodnikových procesů organizace).
  2. *Requirements* (analýza a specifikace use cases).
  3. *Analysis and Design* (transformace požadavků do architektury a UML diagramů tříd či sekvencí).
  4. *Implementation* (samotný vývoj komponent a jednotkové testy).
  5. *Test* (verifikace, validace a hledání chyb).
  6. *Deployment* (balení, distribuce a instalace softwaru u uživatele).
</details>

<details>
<summary>Agilní vývoj softwaru (Scrum, FDD, XP)</summary>

* **Agilní vývoj:** Empirický přístup k řízení vývoje postavený na flexibilitě, adaptabilitě, minimalizaci byrokracie, průběžné dodávce funkčního softwaru a těsné kolaboraci se zákazníkem.
* **Čtyři hodnoty Agilního manifestu (2001):** Priority jsou nastaveny tak, že levé položky mají vyšší hodnotu než pravé:
  1. *Jednotlivci a interakce* před procesy a nástroji.
  2. *Fungující software* před obsáhlou dokumentací.
  3. *Spolupráce se zákazníkem* před vyjednáváním o smlouvách.
  4. *Reagování na změnu* před dodržováním pevného plánu.
* **Agilní smluvní vztahy:** Opuštění prediktivních kontraktů typu *Fixed Time, Fixed Price* (které neflexibilně reagují na změny). Využívají se agilní smlouvy typu **Time & Material**, kde zákazník platí za reálně spotřebovaný čas a úsilí, což umožňuje plynule měnit priority prioritního backlogu bez nutnosti sepisování dodatků.
* **Rámec Scrum:** Agilní framework založený na pevných časových oknech zvaných **Sprinty** (1–4 týdny), na jejichž konci se odevzdává potenciálně nasaditelný produktový inkrement splňující kritéria **DoD** (*Definition of Done*).
  * *Role ve Scrumu:* **Product Owner** (reprezentuje byznys a spravuje prioritizovaný Product Backlog), **Scrum Master** (fasilitátor odstraňující překážky a chránící tým), **Vývojový tým** (samoorganizující se cross-funkční celek).
  * *Ceremonie:* Sprint Planning (plánování), Daily Scrum (denní 15min synchronizace), Sprint Review (předvedení inkrementu zákazníkovi), Sprint Retrospective (analýza a zlepšování procesů týmu).
* **Feature-Driven Development (FDD):** Agilní metodika orientovaná na klientem oceňované drobné vlastnosti (funkcionality) — *features* (např. „vypočítat DPH položky“). Vývoj probíhá v krátkých dvoutýdenních cyklech napříč fázemi: 1. Vývoj celkového modelu, 2. Budování seznamu vlastností, 3. Plánování podle vlastností, 4. Návrh podle vlastností, 5. Realizace podle vlastností.
* **Extrémní programování (XP):** Metodika striktně zaměřená na technickou excelenci inženýrských praktik:
  * *TDD (Test-Driven Development):* Vývoj řízený testy — programátor píše nejdříve automatizovaný test, který selže, a až poté implementuje minimální kód aplikace, aby test prošel.
  * *Párové programování (Pair Programming):* Dva vývojáři pracují simultánně u jednoho počítače na shodném kódu (jeden píše kód — *driver*, druhý strategicky reviduje — *navigator*).
  * *Refaktorizace (Refactoring):* Průběžné permanentní čištění kódu bez změny jeho externího chování.
</details>

<details>
<summary>Fáze testování, typy testů a klasifikace chyb</summary>

* **Testování softwaru:** Proces exekuce systému s cílem detekovat defekty a ověřit shodu s požadavky. Úspěšný test je takový, který odhalí dosud neodhalenou chybu (Myers). Testování může potvrdit přítomnost chyb, ale nikdy nemůže dokázat jejich absolutní absenci.
* **Čtyři úrovně testování (podle granularity):**
  1. **Jednotkové testy (Unit Testing):** Izolované testování nejmenších programových celků (metody, funkce, třídy) autorem kódu.
  2. **Integrační testy (Integration Testing):** Ověřování interakcí a datových toků mezi integrovanými moduly. Přístupy: *Top-Down* (využití napodobenin — *stubs*), *Bottom-Up* (využití řídicích modulů — *drivers*), *Big-Bang* (sloučení všeho naráz).
  3. **Systémové testy (System Testing):** Validace kompletního integrovaného systému jako celku v prostředí blízkém produkci, ověřování funkčních i nefunkčních vlastností.
  4. **Akceptační testy (Acceptance Testing):** Finální ověření zákazníkem, zda systém splňuje zadání (UAT). Dělí se na **Alpha testování** (prováděno interními lidmi v prostředí vývojáře) a **Beta testování** (prováděno reálnými koncovými uživateli v reálném provozu před oficiálním vydáním).
* **Typy testů podle přístupu ke struktuře:**
  * *Black-box:* Tester nemá žádnou znalost vnitřního kódu. Testuje funkčnost na základě specifikace. Techniky: ekvivalentní třídění, analýza hraničních hodnot.
  * *White-box:* Tester má úplnou znalost struktury kódu. Testuje vnitřní logické cesty a smyčky. Metriky: pokrytí kódu (*Code Coverage* — pokrytí příkazů, větví, podmínek).
  * *Grey-box:* Částečná znalost (např. tester zná databázové schéma nebo API rozhraní, ale testuje zvenčí).
* **Nefunkcionální testy:** Ověřují systémové kvality. *Performance/Load testing* zkoumá odezvu při standardní zátěži. *Stress testing* testuje chování při extrémním přetížení s cílem ověřit, zda systém degraduje bezpečně (*Graceful Degradation*) bez poškození dat.
* **Regresní testování:** Opakované spouštění sady testů po jakékoli modifikaci kódu (např. po opravě chyby) s cílem garantovat, že nový zásah omylem nerozbil stávající, doposud funkční vlastnosti softwaru.
* **Formální inspekce (Statické testování):** Týmové přezkoumání kódu/dokumentů bez spuštění programu. Role v týmu: *Moderátor* (řídí proces a schůzku), *Zapisovatel* (Scribe — fixuje nalezené vady), *Autor* (tvůrce kódu, pouze vysvětluje nejasnosti), *Inspektor* (kritický recenzent hledající vady).
* **Čtyři stupně závažnosti chyb:**
  1. *Kritická (Critical):* Pád systému, totální ztráta či poškození dat, nelze pokračovat v práci, neexistuje obchvat (*workaround*).
  2. *Vážná (Major):* Výrazná ztráta funkcionality v klíčové oblasti, ale systém běží nebo existuje náhradní náhradní postup.
  3. *Středně závažná (Medium):* Nesprávné chování v méně podstatných byznysových funkcích.
  4. *Málo závažná (Minor):* Kosmetické a vizuální vady, překlepy v UI či dokumentaci.
* **Ortogonální klasifikace defektů (ODC):** Metodika (IBM) jednoznačného zatřídění každé vady do nezávislých, nekombinovatelných kategorií (např. *Rozhraní, Algoritmus, Časování*), což umožňuje statisticky analyzovat, v jaké etapě vývojového procesu dochází k největší chybovosti.
</details>

<details>
<summary>Softwarové metriky a refaktoring kódu</summary>

* **Softwarové metriky:** Kvantitativní nástroje pro objektivní měření a hodnocení vlastností softwarového produktu a vývojového procesu.
* **Strukturální produktové metriky:**
  * **LOC (Lines of Code):** Fyzický či logický počet řádků kódu. Má nízkou sémantickou vypovídající hodnotu o reálné složitosti a kvalitě systému.
  * **Cykloatická komplexita (McCabeova):** Vyjadřuje vnitřní strukturální složitost kódu na základě počtu lineárně nezávislých cest v grafu řízení toku dat (řídicí struktury `if`, `while`, `switch`). Vzorec pro jednu metodu ($P=1$) je:
    $$M = E - V + 2$$
    Kde $E$ je počet hran a $V$ je počet uzlů v grafu. Doporučená hodnota pro udržovatelnost by neměla překročit 10. Udává minimální počet testů pro pokrytí všech větví.
  * **Halsteadova Software Science:** Metrika složitosti odvozená algoritmicky z počtu unikátních a celkových operátorů ($n_1, N_1$) a operandů ($n_2, N_2$) v kódu. Definuje slovník programu ($n = n_1 + n_2$), délku ($N = N_1 + N_2$) a celkový informační objem programu ($V = N \cdot \log_2 n$).
  * **McClureova komplexita:** Metrika složitosti zaměřená na řídicí struktury, vyhodnocovaná na základě počtu rozhodovacích porovnání $C$ a počtu aktivních řídicích proměnných $V$ v modulu.
  * **Metriky architektury (Card & Glass):** Hodnotí složitost na úrovni modulů kombinací strukturální složitosti ($S(i) = \text{fan-out}^2(i)$, kde fan-out je počet volaných sub-modulů) a datové složitosti ($D(i)$, závislé na počtu předávaných proměnných). Celková složitost je $C(i) = S(i) + D(i)$.
* **Soudržnost (Cohesion) vs. Vázanost (Coupling):** Základní architektonické cíle. Žádoucí je **vysoká soudržnost uvnitř modulu** (všechny vnitřní prvky a metody úzce spolupracují na plnění jednoho logického úkolu) a **nízká vázanost mezi moduly** (moduly jsou na sobě nezávislé, mají minimální propojení, takže změna v jednom modulu neovlivní negativně funkčnost druhého).
* **Refaktoring kódu:** Systematický proces interní restrukturalizace a pročištění zdrojového kódu **bez jakékoliv změny jeho vnějšího chování, funkcionality či rozhraní (API)**. Slouží k redukci technického dluhu a zvýšení udržovatelnosti. Podmínkou pro bezpečný refaktoring je existence robustní sady automatizovaných regresních testů.
</details>

<details>
<summary>Kvalita softwaru a QA vs. QC</summary>

* **Kvalita softwaru:** Míra, do jaké softwarový produkt splňuje explicitně specifikované požadavky (SRS) a implicitní očekávání či potřeby koncových uživatelů.
* **McCallův model kvality (1977):** Hierarchický model člení softwarové vlastnosti do tří hlavních perspektiv, které se dále rozpadají na produktové faktory kvality:
  1. **Product Operation (Provoz produktu):** Hodnotí chování za běhu. *Korektnost* (splnění specifikace), *Spolehlivost* (funkce bez selhání, měřená střední dobou mezi poruchami MTBF a střední dobou opravy MTTR), *Efektivnost* (využití CPU a paměti), *Integrita* (zabezpečení proti neautorizovanému přístupu), *Použitelnost* (snadnost naučení a obsluhy).
  2. **Product Revision (Revize produktu):** Hodnotí schopnost modifikace softwaru. *Udržovatelnost* (úsilí na lokalizaci a opravu chyb), *Flexibilita* (snadnost úpravy pro nové funkce), *Testovatelnost* (úsilí nutné pro validaci a testy).
  3. **Product Transition (Přechod produktu):** Hodnotí adaptabilitu na prostředí. *Přenositelnost* (Portability — snadnost migrace mezi různými HW/SW platformami), *Znovupoužitelnost* (opakované nasazení částí kódu v jiných systémech), *Schopnost spolupráce* (Interoperability — snadnost propojení s cizími systémy).
* **FURPS model:** Alternativní klasifikace nefunkčních vlastností softwaru: *Functionality* (funkčnost), *Usability* (použitelnost), *Reliability* (spolehlivost), *Performance* (výkon), *Supportability* (podporovatelnost/udržovatelnost).
* **Zajištění kvality (Quality Assurance — QA):** **Proaktivní a preventivní proces** striktně orientovaný na celkový **vývojový proces**. Cílem je optimalizovat metodiky, standardy a inženýrské postupy tak, aby se vzniku chyb v kódu efektivně předcházelo (např. zavedení code review pravidel).
* **Řízení kvality (Quality Control — QC):** **Reaktivní proces** orientovaný na **výsledný produkt**. Cílem je testováním, kontrolou a exekucí hotového kódu reálně detekovat, zdokumentovat a nechat opravit chyby, které v produktu už vznikly (napž. spouštění integračních testů).
</details>

<details>
<summary>Odhadování nákladů, času a softwarová fyzika</summary>

* **Odhadování softwaru (Estimation):** Matematicko-manažerská predikce potřebného lidského úsilí (vyjadřovaná v jednotkách člověkoměsíců — *Person-Months*), kalendářního času a finančních rozpočtů nutných pro úspěšné dokončení projektu.
* **Kužel nejistoty (Cone of Uncertainty):** Koncept prokazující, že na samém počátku projektu (před specifikací požadavků) je odhad zatížen masivní chybou z důvodu inherentní neurčitosti. Počáteční odhad se může od finální reality lišit **až čtyřnásobně (4:1) oběma směry** ($0.25\times$ až $4\times$). S postupným upřesňováním specifikace a fázemi SDLC se nejistota plynule zužuje k hodnotě 1.
* **Brooksův zákon softwarového managementu:** Klíčové pravidlo pro řízení zpožděných projektů: *„Přidání technických pracovníků do zpožděného softwarového projektu může způsobit jeho ještě větší zpoždění.“* Důvodem je vysoká komunikační režie (*overhead*) v rozšířeném týmu a nutnost stávajících vývojářů alokovat čas na zaškolení nováčků namísto produktivní práce.
* **Metodiky odhadování:**
  * **Tříbodový odhad (PERT):** Expertní odhad potlačující subjektivitu výpočtem váženého průměru tří scénářů: Optimistického ($O$), Pesimistického ($P$) a Nejvíc pravděpodobného ($M$):
    $$E = \frac{O + 4M + P}{6}$$
  * **Model COCOMO 1 (Boehm):** Algoritmetický model, kde hlavním indikátorem pracnosti je velikost vyjádřená v tisících řádků kódu (**KSLOC**). Rozlišuje tři vývojové módy: *Organický* (malý stabilní tým, známá doména), *Bezprostřední/Střední* (střední tým, kombinace zkušeností), *Vázaný/Komplexní* (přísné hardwarové a spolehlivostní limity, vestavěné systémy). Pracnost $E = A \cdot (\text{KSLOC})^B \cdot \text{EAF}$, kde EAF je faktor úpravy na základě 15 atributů.
  * **COCOMO II:** Modernizovaný standard dělící odhad do 3 modelů podle pokročilosti projektu: *Application Composition Model* (využívá Object Points v rané fázi), *Early Design Model* (funkční body v hrubém návrhu), *Post Architecture Model* (nejdetailnější odhad po stabilizaci architektury využívající 5 měřítkových faktorů pro exponent složitosti).
  * **Analýza funkčních bodů (FPA):** Měří velikost softwaru z pohledu funkčních požadavků uživatele **zcela nezávisle na použitém programovacím jazyce a technologii**. Hodnotí složitost na základě výskytu datových funkcí (Interní logické soubory **ILF**, Externí rozhraní **EIF**) a transakčních funkcí (Externí vstupy **EI**, Externí výstupy **EO**, Externí dotazy **EQ**).
* **Softwarová fyzika a Putnamův model:** Matematicky popisuje vztah mezi pracností ($N$), časem ($T$) a velikostí softwaru. Prokazuje extrémní nelinearitu: lidské úsilí a kalendářní čas nejsou lineárně zaměnitelné. Stlačení času pod kritickou mez Rayleighovy vlny posouvá projekt do **nedosažitelné oblasti**, kde projekt garantovaně zkolabuje bez ohledu na výši finančního rozpočtu či počet programátorů (programy psané ve spěchu vykazují deformaci a jsou delší).
* **Agilní relativní odhadování:**
  * *Story Points:* Abstraktní bezrozměrná jednotka složitosti, velikosti a neurčitosti úkolu porovnávaná relativně vůči sobě. Využívá upravenou Fibonacciho posloupnost (1, 2, 3, 5, 8, 13, 21...), což reflektuje rostoucí nejistotu u velkých úkolů.
  * *Planning Poker:* Kolektivní technika odhadování Story Points, kde všichni členové týmu vyloží své karty s odhady současně, což eliminuje psychologický efekt ukotvení (*Anchoring Effect*). O extrémních odchylkách se diskutuje do dosažení konsenzu.
</details>

<details>
<summary>Údržba softwaru a znovupoužitelnost (Reuse)</summary>

* **Údržba softwaru (Software Maintenance):** Soubor inženýrských činností realizovaných po formálním odevzdání softwaru do produkčního provozu. Náklady na údržbu dlouhodobých systémů dominantně zatěžují celkový životní cyklus a standardně tvoří **60 % až 80 % celkových nákladů na vlastnictví (TCO)**.
* **Čtyři typy údržby (podle Lehmana):**
  1. **Korektivní údržba (Corrective):** Reaktivní identifikace a oprava chyb, selhání a poruch nahlášených z produkčního provozu.
  2. **Adaptivní údržba (Adaptive):** Modifikace softwaru vynucené evolučními změnami v jeho externím okolním prostředí (např. legislativní změna sazeb DPH, aktualizace OS na serverech, změna API třetí strany).
  3. **Perfektivní údržba (Perfective):** Implementace nových funkčních požadavků a vylepšení na základě přímých požadavků uživatelů systému v provozu.
  4. **Preventivní údržba (Preventive):** Proaktivní inženýrské zásahy (např. refaktoring, čištění kódu) s cílem zvýšit budoucí srozumitelnost a udržovatelnost softwaru, opravit latentní skryté vady a zpomalit přirozené stárnutí softwaru.
* **Lehmanovy zákony softwarové evoluce:**
  * *Zákon neustálé změny:* Systémy provozované v reálném světě se musí neustále adaptovat a měnit, jinak se stávají progresivně méně užitečnými.
  * *Zákon rostoucí složitosti:* Jak se softwarový systém vyvíjí a mění, jeho vnitřní složitost přirozeně roste, pokud se aktivně neinvestuje úsilí do jejího snižování (preventivní údržba).
* **Znovupoužitelnost softwaru (Software Reuse):** Záměrný inženýrský přístup, kdy se softwarové artefakty (kód, návrhy, architektury) navrhují tak, aby mohly být opakovaně nasazeny v jiných projektech, což zkracuje čas uvedení na trh (*Time-to-Market*) a snižuje chybovost.
* **Úrovně znovupoužitelnosti v praxi:**
  * *Zdrojový kód (Ad-hoc kopírování):* Anti-pattern mechanického kopírování kódu (Copy-Paste). Vede k masivním duplicitám a kriticky ztěžuje budoucí údržbu (chyba se opraví na jednom místě, v duplikátech zůstává).
  * *Knihovny a komponenty:* Zapouzdřené znovupoužitelné softwarové balíčky s jasně deklarovaným a stabilním veřejným rozhraním (API).
  * *Návrhové vzory (Design Patterns):* Osvědčená šablonovitá koncepční řešení typických opakujících se architektonických problémů v objektovém návrhu (např. *Singleton, Observer, Factory*).
  * *Frameworky:* Komplexní skelety a softwarové kostry aplikací určující celkovou architekturu. Využívají princip **Inversion of Control (IoC)** — framework plně řídí tok programu a v případě potřeby volá specifický kód vývojáře, nikoliv vývojář framework.
* **Překážky znovupoužitelnosti:**
  * **Pravidlo třetího použití (Rule of Three):** Vývoj, otestování, zobecnění rozhraní a sepsání dokumentace pro skutečně univerzální znovupoužitelnou komponentu je **3× dražší a náročnější** než napsání jednoúčelového kódu pro jeden projekt. Investice se finančně a časově začíná vracet až při jejím minimálně třetím úspěšném nasazení v jiných nezávislých systémech.
  * **Syndrom Not Invented Here (NIH):** Psychologický a organizační blok inženýrů, kteří a priori odmítají nasazení cizích hotových knihoven/řešení a preferují psaní vlastního kódu od nuly, což zvyšuje náklady a prodlužuje čas vývoje.
</details>


