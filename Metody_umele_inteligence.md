# Metody umělé inteligence

> Prohledávání stavového prostoru, lokální prohledávání a metaheuristiky s jedním řešením, 
> populační metaheuristiky (evoluční algoritmy, inteligence hejna). 
> Plánování, reprezentace problému, plánování se stavovým prostorem. 
> Práce s neurčitostí, Bayesovské sítě, exaktní a aproximační odvozování, čas a neurčitost, 
> teorie užitku, Markovský rozhodovací proces, iterace hodnot, iterace strategie. 
> Robotika, plánování pohybu robota (konfigurační prostor, kombinatorické a pravděpodobnostní přístupy). (IV126)

# Metody umělé inteligence

## Prohledávání stavového prostoru
Prohledávání stavového prostoru (State Space Search)

Prohledávání stavového prostoru je univerzální metodika řešení problémů v AI, kde je cílem najít sekvenci akcí vedoucí z počátečního stavu do stavu cílového.

1. Definice a Formální reprezentace

Motivace: Aby mohl počítač řešit reálný problém (např. navigaci), musíme jej převést do abstraktního matematického modelu, se kterým dokáží pracovat algoritmy.

Stav ($s$): Momentální konfigurace světa.

Počáteční stav ($s_0$): Bod, ve kterém agent začíná.

Cílový test ($GoalTest$): Podmínka, která určuje, zda jsme dosáhli řešení (může být jeden stav nebo vlastnost).

Akce a Přechodový model: Funkce, která pro daný stav vrátí možné následníky.

Cena cesty ($c$): Numerická hodnota (váha), která určuje „náročnost“ přechodu mezi stavy.

Příklad: 8-hlavolam (Sliding Puzzle)

Stav: Rozmístění čísel 1–8 na mřížce 3x3.

Akce: Posun prázdného pole nahoru, dolů, vlevo, vpravo.

Cíl: Seřazená čísla 1–8.

2. Strategie prohledávání

Motivace: Efektivita hledání závisí na tom, v jakém pořadí expandujeme uzly. Špatná strategie může vést k nekonečným cyklům nebo vyčerpání paměti.

Dopředné prohledávání (Forward Search): Postupujeme od $s_0$ k cíli. Vhodné, pokud je málo možných startů.

Zpětné prohledávání (Backward Search): Postupujeme od cíle k počátku. Vhodné, pokud je cíl velmi specifický a faktor větvení směrem od cíle je menší.

Obousměrné prohledávání: Hledáme z obou stran zároveň, dokud se cesty neprotnou. Výrazně snižuje časovou složitost ($b^{d/2} + b^{d/2}$ místo $b^d$).

Příklad: Hledání cesty v bludišti

Dopředné: Jdete od vchodu a zkoušíte chodby.

Zpětné: Podíváte se na východ a zkoumáte, které chodby k němu vedou (často efektivnější u složitých výstupů).

3. Klasické algoritmy (Slepé vs. Informované)

Motivace: Výběr algoritmu určuje, zda řešení vůbec najdeme a zda bude optimální (nejkratší/nejlevnější).

A. Neinformované (Slepé) prohledávání

Nemají informaci o tom, jak blízko je cíl.

BFS (Do šířky): Prochází stavy po vrstvách. Najde nejkratší cestu (při stejných cenách), ale je extrémně náročné na paměť.

DFS (Do hloubky): Jde co nejdále v jedné větvi. Malé nároky na paměť, ale může uvíznout v nekonečné větvi a nenajde nejkratší cestu.

Uniform-Cost Search (UCS): Rozšíření BFS pro cesty s různými cenami. Vždy expanduje uzel s nejnižší průběžnou cenou $g(n)$.

B. Informované (Heuristické) prohledávání

Využívají nápovědu ve formě heuristiky $h(n)$ – odhadu ceny do cíle.

Greedy Best-First Search: Jde vždy tam, kde to vypadá nejblíže k cíli (podle $h(n)$). Rychlé, ale nemusí najít optimální cestu.

Algoritmus A*: Kombinuje $g(n)$ (cena z počátku) a $h(n)$ (odhad do cíle). Pokud je heuristika přípustná (nikdy nepodstřelí cenu), A* zaručeně najde optimální cestu.

Příklad: Plánování trasy v mapě

Slepé (BFS): Prohledáváte všechny ulice v kruhu kolem startu, dokud nenarazíte na cíl.

Informované (A):* Prioritně zkoumáte ulice, které vedou směrem k cílovému městu (využíváte vzdušnou vzdálenost jako heuristiku).

4. Vlastnosti algoritmů a Heuristiky

Motivace: Abychom mohli algoritmu důvěřovat, musíme vědět, zda je úplný (vždy najde řešení) a optimální (najde to nejlepší).

Přípustnost (Admissibility): Heuristika $h(n)$ je přípustná, pokud $h(n) \leq h^*(n)$, kde $h^*(n)$ je skutečná minimální cena do cíle.

Konzistence (Monotonicity): Cena přechodu plus odhad z následníka není menší než odhad ze současného stavu.

## Lokální prohledávání
Lokální prohledávání se používá v situacích, kdy nás nezajímá cesta, jakou jsme se k řešení dostali, ale pouze koncový stav (např. rozvrh hodin, rozmístění komponent). Pracuje s omezenou pamětí – obvykle udržuje pouze jeden aktuální stav.

1. Motivace a Princip "Krajiny" (State-space Landscape)

Motivace: U mnoha problémů je stavový prostor tak obrovský, že nelze stavět strom hledání (BFS/A*). Lokální hledání se místo toho "rozhlíží" po okolí aktuálního stavu a snaží se najít lepší konfiguraci.

Cílová funkce (Objective Function): Hodnotíme, jak "dobrý" stav je (např. počet konfliktů v rozvrhu).

Krajina (Landscape): Představujeme si prostor jako mapu s horami (maxima) a údolími (minima).

Globální optimum: Absolutně nejlepší řešení v celém prostoru.

Lokální optimum: Bod, který je lepší než všichni jeho sousedé, ale horší než globální maximum.

2. Horolezecký algoritmus (Hill Climbing)

Princip: "Hladový" algoritmus, který se neustále pohybuje ve směru největšího nárůstu hodnoty cílové funkce. Pokud žádný soused není lepší, zastaví se.

Problémy:

Lokální maxima: Algoritmus uvízne na "vrcholku", který není nejvyšší.

Hřebeny (Ridges): Úzké vyvýšeniny, kde pohyb v základních směrech vede dolů, i když hřeben stoupá.

Plošiny (Plateaus): Oblasti se stejnou hodnotou, kde algoritmus "bloudí".

Příklad: Problém 8 dam

Stav: 8 dam na šachovnici (v každém sloupci jedna).

Sousední stav: Posun jedné dámy v rámci jejího sloupce.

Cílová funkce: Počet dvojic dam, které se vzájemně ohrožují (chceme minimalizovat).

Hill Climbing: V každém kroku posune dámu tak, aby co nejvíce klesl počet ohrožení.

3. Simulované žíhání (Simulated Annealing)

Motivace: Hill Climbing nikdy neudělá krok "dolů", takže snadno uvízne. Simulované žíhání kombinuje Hill Climbing s náhodným procházením, aby mohlo uniknout z lokálních optim.

Princip: Inspirováno metalurgií (kalení kovů).

Pravidlo: Pokud je sousední stav lepší, přijmeme ho. Pokud je horší, přijmeme ho s určitou pravděpodobností, která závisí na:

Teplotě ($T$): Na začátku je vysoká (přijímáme i velmi špatné kroky), postupně klesá.

Míře zhoršení ($\Delta E$): Čím horší krok je, tím menší je šance na jeho přijetí.

Při dostatečně pomalém ochlazování (chladicí plán) algoritmus s vysokou pravděpodobností najde globální optimum.

Příklad: Logistické plánování
Při hledání trasy pro rozvoz balíků může algoritmus dočasně přijmout delší trasu (krok dolů), aby se následně mohl "přehoupnout" přes kopec lokálního optima k výrazně efektivnějšímu rozvržení.

4. Tabu prohledávání (Tabu Search)

Motivace: Algoritmy se často vrací do stejných stavů (cyklí se). Tabu prohledávání využívá paměť k prevenci tohoto chování.

Tabu seznam: Seznam nedávno navštívených stavů nebo provedených změn, které jsou po určitou dobu "zakázané".

Tímto mechanismem je algoritmus nucen prozkoumávat nové části stavového prostoru, i kdyby to znamenalo dočasné zhoršení výsledku.

Příklad: Rozvrhování směn
Pokud jsme právě přesunuli sestru ze směny A do směny B, zakážeme její přesun zpět na smenu A na dalších 10 iterací. Tím donutíme algoritmus vyzkoušet přesuny jiných zaměstnanců.

Srovnání s ostatními metodami

Vlastnost

BFS / A*

Lokální prohledávání

Cesta k cíli

Je součástí řešení.

Nezajímá nás.

Paměť

Ukládá celé větve stromu.

Ukládá jen aktuální stav (a okolí).

Úplnost

Zaručena (pokud existuje).

Není zaručena (může uvíznout).

Vhodnost

Logické hádanky, navigace.

Optimalizace, návrh systémů.

## Metaheuristiky s jedním řešením
Tyto metody rozšiřují lokální prohledávání o mechanismy, které umožňují únik z lokálních optim. Motivací je najít rovnováhu mezi prozkoumáváním prostoru (exploration) a vytěžováním známých dobrých oblastí (exploitation).

**Simulované žíhání** (Simulated Annealing) je inspirováno chladnutím kovů. Algoritmus s určitou pravděpodobností přijímá i horší řešení, což mu dovoluje vyskočit z lokálního minima. Tato pravděpodobnost se postupně snižuje podle parametru teploty. **Tabu prohledávání** využívá krátkodobou paměť k zakázání nedávno navštívených stavů, čímž brání cyklení a nutí agenta prozkoumávat nové oblasti.

## Populační metaheuristiky
Populační algoritmy nepracují s jedním řešením, ale s celou množinou (populací) kandidátů. Motivací je paralelní prohledávání různých částí prostoru a možnost kombinovat (sdílet) informace mezi jednotlivými řešeními.

**Evoluční algoritmy** simulují přírodní výběr. Proces zahrnuje selekci nejzdatnějších jedinců na základě fitness funkce, jejich křížení (výměnu částí řešení) a mutaci (náhodnou změnu). Patří sem Genetické algoritmy (binární řetězce), Evoluční strategie (reálná čísla) a Genetické programování (vývoj programů). **Inteligence hejna** (Swarm Intelligence) je inspirována kolektivním chováním zvířat. Například **optimalizace mravenčí kolonií** (ACO) využívá umělé feromonové stopy k nalezení optimálních cest v grafu.

## Plánování
Plánování je proces hledání sekvence akcí k dosažení cíle v komplexních doménách. Zatímco klasické prohledávání stavového prostoru vnímá stavy jako "černé skříňky", plánování využívá vnitřní strukturu stavů a logické vazby mezi akcemi. Hlavní motivací je schopnost řešit rozsáhlé průmyslové a logistické úlohy efektivněji než čistým prohledáváním grafu.

## Reprezentace problému
Aby mohl plánovač efektivně pracovat, potřebuje formalizovaný popis světa. Standardem je jazyk **PDDL** (Planning Domain Definition Language). Ten odděluje **Doménu** (obsahuje typy objektů, predikáty a šablony akcí s jejich podmínkami a efekty) a **Problém** (specifikuje konkrétní objekty, výchozí situaci a cílový stav). Tato dekompozice umožňuje používat stejný plánovací algoritmus na různé typy úloh.

## Plánování se stavovým prostorem
Tento přístup aplikuje prohledávací algoritmy přímo na stavy popsané v PDDL. **Dopředné plánování** postupuje od počátku a aplikuje akce splňující podmínky. **Zpětné plánování** (regresní) začíná od cíle a hledá akce, které tento cíl naplňují. Zpětný postup je často výhodnější, protože výrazně redukuje prohledávaný prostor tím, že uvažuje pouze akce relevantní pro splnění cíle.

## Práce s neurčitostí
V reálném světě jsou akce nespolehlivé a senzory nepřesné. Motivací je vytvořit agenty, kteří se dokáží racionálně rozhodovat i za neúplné informace. Agent místo jednoho stavu udržuje **stav víry** (belief state), což je rozdělení pravděpodobnosti přes všechny možné stavy světa. K popisu těchto vztahů se využívá teorie pravděpodobnosti.

## Bayesovské sítě
Bayesovské sítě jsou grafický model, který reprezentuje pravděpodobnostní vztahy mezi proměnnými. Motivací je drastické snížení výpočetní náročnosti oproti úplným pravděpodobnostním tabulkám díky využití podmíněné nezávislosti. Síť je tvořena orientovaným grafem (DAG), kde uzly jsou proměnné a hrany značí přímý vliv. Každý uzel obsahuje tabulku podmíněné pravděpodobnosti (CPT).

## Exaktní a aproximační odvozování
Odvozování (inference) je proces výpočtu pravděpodobnosti dotazu na základě známých důkazů. **Exaktní odvozování** (např. Variable Elimination) poskytuje matematicky přesný výsledek, ale v rozsáhlých sítích je výpočetně neúnosné. Proto se používá **aproximační odvozování** pomocí vzorkování (sampling), kde generujeme tisíce scénářů a sledujeme četnost výsledků. Metody jako Likelihood Weighting zajišťují, aby vzorky odpovídaly známým faktům.

## Čas a neurčitost
Mnoho systémů vyžaduje sledování stavu, který se mění v čase. K tomu slouží modely časových řad, kde stav v čase $t$ závisí na stavech předchozích. Základem je **Markovský předpoklad**, který říká, že budoucí stav závisí pouze na stavu současném, nikoliv na celé historii. To umožňuje efektivní výpočty úloh jako je filtrování (odhad aktuálního stavu), predikce (budoucnost) a vyhlazování (analýza minulosti).

## Teorie užitku
Samotná pravděpodobnost agentovi neříká, co má dělat. Teorie užitku zavádí funkci $U(s)$, která kvantifikuje, jak moc je daný stav pro agenta žádoucí. Motivací je vytvoření racionálního agenta, který se rozhoduje podle principu **maximálního očekávaného užitku** (MEU) – vybírá akci, která v průměru vede k nejlepším výsledkům, i když výsledek akce není jistý.

## Markovský rozhodovací proces (MDP)
MDP je matematický rámec pro modelování sekvenčního rozhodování v prostředích, kde jsou výsledky akcí částečně náhodné. Skládá se ze stavů, akcí, přechodového modelu $P(s' | s, a)$ a funkce odměny $R(s)$. Cílem je najít **strategii** (policy) $\pi(s)$, která agentovi určí optimální akci v každém možném stavu tak, aby maximalizoval dlouhodobý součet odměn.

## Iterace hodnot
Iterace hodnot je algoritmus pro řešení MDP. Využívá Bellmanovu rovnici k postupnému výpočtu užitku každého stavu. Algoritmus opakovaně aktualizuje hodnotu stavu na základě odměny a očekávaného užitku budoucích stavů, dokud se hodnoty neustálí (nekonvergují). Výsledné hodnoty pak přímo určují optimální strategii.

## Iterace strategie
Na rozdíl od iterace hodnot se tento přístup zaměřuje přímo na vylepšování strategie. Skládá se ze dvou kroků: **evaluace strategie** (výpočet užitků stavů pro aktuálně zvolené akce) a **zlepšení strategie** (přehodnocení akcí v každém stavu). Tento proces často vede k nalezení optimální strategie v mnohem menším počtu kroků než iterace hodnot.

## Robotika
Robotika představuje propojení AI s fyzickým světem. Robot musí vnímat své okolí (senzory), vytvářet modely světa, plánovat své akce a fyzicky je provádět (efektory). Klíčovým problémem je zvládání chyb v pohybu a šumu v senzorech, což se řeší pomocí technik pravděpodobnostního vnímání a lokalizace (např. SLAM).

## Plánování pohybu robota
Cílem plánování pohybu je nalezení spojité cesty v prostoru tak, aby robot nenarazil do žádné překážky. Motivací je oddělení vysoké úrovně plánování (kde se má robot nacházet) od nízké úrovně řízení (jaké napětí poslat do motorů).

## Konfigurační prostor (C-space)
Konfigurační prostor je klíčová abstrakce v robotice. Zatímco v reálném světě má robot složitý geometrický tvar, v C-space je reprezentován jako **jediný bod**. Každý stupeň volnosti robota (např. posun, rotace, úhel v kloubu) představuje jednu dimenzi tohoto prostoru. Úloha hledání cesty se tak mění na nalezení trajektorie bodu ve volném konfiguračním prostoru $C_{free}$.

## Kombinatorické přístupy
Tyto metody se snaží o exaktní vyřešení geometrie volného prostoru. **Graf viditelnosti** propojuje rohy překážek a hledá nejkratší cestu (která ale vede nebezpečně blízko hran). **Dekompozice na buňky** rozděluje prostor na jednoduché geometrické tvary (např. lichoběžníky), ve kterých se lze pohybovat přímočaře. Tyto přístupy jsou přesné, ale pro vysoké dimenze výpočetně nezvládnutelné.

## Pravděpodobnostní přístupy
V moderní robotice se pro komplexní úkoly (např. ramena s mnoha klouby) využívají pravděpodobnostní metody založené na vzorkování. **PRM** (Probabilistic Roadmaps) náhodně vzorkuje konfigurační prostor a staví síť cest. **RRT** (Rapidly-exploring Random Trees) rychle rozšiřuje strom z počáteční polohy do neprobádaných oblastí. Tyto metody jsou velmi rychlé a efektivní i ve složitých prostorech, i když nezaručují vždy nejkratší možnou cestu.