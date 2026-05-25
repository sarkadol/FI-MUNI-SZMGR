# Statistika

> Základní statistické metody (bodové odhady, intervaly spolehlivosti, testování statistických hypotéz). ANOVA. Neparametrické testy hypotéz. Mnohonásobná lineární regrese, autokorelace, multikolinearita. Analýza hlavních komponent (PCA).

---

## 1. Základní statistické metody

Slouží k tomu, abychom z dat **výběru** (vzorku o rozsahu $n$) vyvodili zobecňující závěry o celém **základním souboru** (populaci) pomocí pravděpodobnostních modelů.

### 1.1 Bodové odhady
Bodový odhad je jedno konkrétní číslo vypočítané z výběrových dat, které slouží jako nejlepší aproximace neznámého parametru populace (např. střední hodnoty $\mu$ nebo rozptylu $\sigma^2$).

* **Výběrový průměr ($\bar{x}$):** Nejlepší odhad střední hodnoty $\mu$.
  $$\bar{x} = \frac{1}{n} \sum_{i=1}^{n} x_i$$
* **Výběrový rozptyl ($s^2$):** Odhad populačního rozptylu $\sigma^2$. Používáme dělení $(n-1)$ namísto $n$ (tzv. Besselova korekce), aby byl odhad **nestranný**.
  $$s^2 = \frac{1}{n-1} \sum_{i=1}^{n} (x_i - \bar{x})^2$$

#### Vlastnosti kvalitního bodového odhadu:
* **Nestrannost (Unbiasedness):** Střední hodnota odhadu se přesně rovná skutečné hodnotě odhadovaného parametru. Odhad systematicky nenadhodnocuje ani nepodhodnocuje.
* **Konzistence (Consistency):** S rostoucím rozsahem výběru ($n \to \infty$) se pravděpodobnost, že se odhad liší od skutečného parametru o více než libovolně malé $\varepsilon$, blíží nule.
* **Efektivita (Efficiency):** Mezi všemi nestrannými odhady má tento odhad nejmenší možný rozptyl (značí maximální přesnost).

#### Centrální limitní věta (CLV)
Zásadní pilíř matematické statistiky. Říká, že pokud je rozsah výběru $n$ dostatečně velký, má výběrový průměr $\bar{x}$ přibližně **normální rozdělení**, a to bez ohledu na to, jaké rozdělení má původní populace.
* **Matematická formulace:** $$\bar{X}_n \xrightarrow{d} N\left(\mu, \frac{\sigma^2}{n}\right)$$
* **Praktický dopad:** U velkých výběrů (obvykle $n > 30$) můžeme bezpečně používat parametrické testy a standardní intervaly spolehlivosti, i když původní data vykazují nenormální rozdělení.

#### Metoda maximální věrohodnosti (MLE - Maximum Likelihood Estimation)
Principiálně odlišný přístup k odhadu parametrů rozdělení. Hledá takové hodnoty parametrů, pro které je pravděpodobnost získání reálně pozorovaných dat maximální.
* **Věrohodnostní funkce (Likelihood function):** Pro nezávislá pozorování je definována jako součin jejich hustot pravděpodobnosti (nebo pravděpodobnostních funkcí):
  $$L(\theta) = \prod_{i=1}^{n} f(x_i; \theta)$$
* **Postup:** V praxi maximalizujeme logaritmus této funkce ($\ln L(\theta)$), což transformuje součin na součet a zjednodušuje derivování. MLE odhady jsou asymptoticky nestranné, normální a efektivní, avšak pro malé výběry mohou být vychýlené (např. MLE odhad rozptylu dělí číslem $n$, nikoliv $n-1$).

---

### 1.2 Intervaly spolehlivosti (CI - Confidence Interval)
Protože se bodový odhad téměř nikdy netrefí přesně do skutečné hodnoty parametru, konstruujeme interval spolehlivosti. Je to náhodný interval, který s předem zvolenou vysokou pravděpodobností $1-\alpha$ (spolehlivost) pokrývá neznámý pevný parametr populace.

* **Hladina významnosti ($\alpha$):** Pravděpodobnost, že parametr v intervalu nebude obsažen (standardně $\alpha = 0,05$, tj. 5 %).
* **Interpretace:** Pokud bychom nezávisle opakovali výběr dat a pokaždé sestrojili 95% interval spolehlivosti, pak přesně 95 % těchto intervalů bude skutečný populační parametr obsahovat a 5 % jej mine. Parametr je fixní, náhodný je samotný interval.

#### Faktory ovlivňující šířku intervalu:
1. **Variabilita dat (směrodatná odchylka $s$):** Vyšší rozptyl v datech znamená větší nejistotu a širší interval.
2. **Rozsah výběru ($n$):** Větší množství dat snižuje standardní chybu odhadu a interval se zužuje.
3. **Požadovaná spolehlivost ($1-\alpha$):** Pokud chceme vyšší jistotu (např. 99% místo 95%), musíme kritickou hodnotu zvětšit, čímž se interval rozšíří.

<img alt="img.png" src="img/statistika/conf-interval.png" width="400"/>

---

### 1.3 Testování statistických hypotéz
Formalizovaný proces rozhodování o vlastnostech populace na základě dat z náhodného výběru.

#### Standardní postup:
1. **Formulace hypotéz:**
   * $H_0$ (Nulová hypotéza): Předpokládá absenci efektu, shodu nebo stav "beze změny" (např. lék nefunguje, rozdíl mezi skupinami je nulový).
   * $H_1$ (Alternativní hypotéza): Konkurenční tvrzení, které chceme daty prokázat (reprezentuje efekt / rozdíl / odchylku).
2. **Volba hladiny významnosti ($\alpha$):** Maximální přípustná pravděpodobnost chyby I. typu (standardně 0,05).
3. **Výběr a výpočet testové statistiky:** Náhodná veličina s přesně známým teoretickým rozdělením za předpokladu platnosti $H_0$ (např. t-statistika, Z-statistika).
4. **Rozhodnutí pomocí p-hodnoty:**
   * **p-hodnota:** Pravděpodobnost, že za platnosti $H_0$ bychom získali testovou statistiku stejně nebo více extrémní, než je ta naše reálně pozorovaná.
   * Pokud $p \le \alpha \implies$ **Zamítáme $H_0$** (výsledek je statisticky významný).
   * Pokud $p > \alpha \implies$ **Nezamítáme $H_0$** (nemáme dostatek důkazů pro prokázání změny).

#### Chyby v rozhodovacím procesu:
Při testování se můžeme dopustit dvou typů chyb:

| Skutečnost \ Rozhodnutí | Nezamítáme $H_0$ | Zamítáme $H_0$ |
| :--- | :--- | :--- |
| **$H_0$ ve skutečnosti platí** | Správné rozhodnutí | **Chyba I. typu ($\alpha$)** (Falešný poplach) |
| **$H_1$ ve skutečnosti platí** | **Chyba II. typu ($\beta$)** (Nepoznaný efekt) | Správné rozhodnutí (Síla testu $1-\beta$) |

#### Síla testu ($1-\beta$)
Vyjadřuje pravděpodobnost, že test správně zamítne neplatnou nulovou hypotézu $H_0$ (tj. rozpozná skutečně existující efekt či rozdíl a nedopustí se chyby II. typu). 
* **Interpretace:** Čím vyšší je síla testu, tím menší je riziko, že přehlédneme důležitý objev. V praxi se požaduje síla testu alespoň 80 % ($1-\beta = 0,80$).
* **Faktory zvyšující sílu testu:**
  1. **Větší rozsah výběru ($n$):** Více dat snižuje standardní chybu a zpřesňuje test.
  2. **Větší velikost efektu (Effect Size):** Výrazné rozdíly v populaci se prokazují snáze než miniaturní odchylky.
  3. **Menší variabilita dat ($\sigma$):** Homogennější data snižují množství šumu.
  4. **Zvýšení hladiny významnosti ($\alpha$):** Snížením přísnosti (např. z 5 % na 10 %) sice zvýšíme sílu testu, ale za cenu vyššího rizika falešného poplachu (chyby I. typu).

#### Základní příklady testů:
* **t-test:** Testování průměru pro malé výběry při neznámem populačním rozptylu.
* **z-test:** Testování průměru pro velké výběry nebo při známém rozptylu.
* **chí-kvadrát test:** Testování shody rozdělení nebo nezávislosti nominálních znaků.

---

## 2. ANOVA (Analysis of Variance - Analýza rozptylu)

ANOVA je statistická metoda pro testování rovnosti středních hodnot u **tří a více nezávislých skupin**. Zjišťuje, zda nezávislé kategorické proměnné (faktory) statisticky významně ovlivňují spojitou závislou proměnnou, a to rozkladem celkové variability na rozptyl mezi skupinami a uvnitř skupin.

* *Příklad:* Máme 4 různé odrůdy brambor sesbírané po trsech s různou hmotností. Zajímá nás, jestli je některá odrůda výnosnější než jiná (průměrná hmotnost trsu se liší na základě odrůdy).

* **$H_0$:** Všechny skupinové průměry jsou stejné: $\mu_1 = \dots = \mu_k$ pro všechna $i = 1, \dots, k$.
* **$H_1$:** Alespoň jeden pár skupinových průměrů se liší: $\exists\, i,j \in \{1,\dots,k\},\ i \neq j:\ \mu_i \neq \mu_j$.

<img width="434" height="289" alt="image" src="img/statistika/anova.png" />

#### Předpoklady ANOVA:
1. **Nezávislost výběrů:** Jednotlivá pozorování musí být na sobě zcela nezávislá. Pokud nejsou, mění se rozdělení testové statistiky i p-hodnota a výsledky nejsou spolehlivé.
2. **Normalita dat:** Ověřuje se pomocí Q-Q grafu nebo testů (např. Shapiro–Wilk). ANOVA je poměrně odolná – pokud má každá skupina alespoň ~20 pozorování a data nejsou silně vychýlená, mírné porušení nevadí. Při silném porušení použijeme neparametrický test (např. Kruskal–Wallis).
3. **Shoda rozptylů (homogenita variancí):** Ověřuje se např. Leveneho nebo Bartlettovým testem. Mírné porušení většinou nevadí, pokud mají srovnávané skupiny podobný rozsah.

#### Formální modelové vyjádření ANOVA
ANOVA lze chápat jako speciální případ lineárního regresního modelu (kde prediktory jsou kategorické proměnné). Každé jednotlivé pozorování můžeme rozložit na teoretické složky pomocí lineárního modelu:

$$Y_{ij} = \mu + \alpha_i + \varepsilon_{ij}$$

Kde:
* $Y_{ij}$ je naměřená hodnota $j$-tého pozorování v $i$-té skupině (např. hmotnost konkrétního trsu brambor z konkrétní odrůdy).
* $\mu$ je **celkový průměr** (grand mean) napříč všemi skupinami bez rozdílu.
* $\alpha_i$ je **efekt $i$-té skupiny** (faktoru), který vyjadřuje odchylku průměru dané skupiny od celkového průměru ($\alpha_i = \mu_i - \mu$). Pro jednoznačnost modelu platí dodatečná podmínka $\sum \alpha_i = 0$.
* $\varepsilon_{ij}$ je **náhodná chyba** (reziduum) daného pozorování, která představuje přirozenou variabilitu (šum). Předpokládá se, že tyto chyby jsou nezávislé a mají normální rozdělení s nulovou střední hodnotou a konstantním rozptylem, tedy $\varepsilon_{ij} \sim N(0, \sigma^2)$.

#### Jednofaktorová vs. Vícefaktorová ANOVA
Jednofaktorová ANOVA zkoumá vliv jedné nezávislé proměnné (faktory). Vícefaktorová ANOVA analyzuje vliv dvou a více faktorů současně, včetně jejich vzájemných interakcí na závislou proměnnou.

* *Příklad:* Máme 3 různé druhy hnojiva, 2 různé typy půdy a výnos polí. Zajímá nás, jestli je některá kombinace hnojiva a typu půdy výnosnější než jiná. Máme faktory hnojivo a typ půdy.

U dvoufaktorové ANOVA rozlišujeme model **bez interakce** a model **s interakcí** mezi faktory:
* **Bez interakce:** Předpokládáme, že vliv jednoho faktoru je stejný bez ohledu na úroveň druhého faktoru. *Například hnojivo A zvyšuje výnos vždy o +2 kg bez ohledu na typ půdy.*
* **S interakcí:** Připouštíme, že efekt jednoho faktoru závisí na úrovni druhého faktoru. *Hnojivo A je nejlepší na jílovité půdě, ale na písčité půdě je lepší hnojivo B.*

<img width="1000" alt="image" src="img/statistika/anova2faktory.png" />

### 2.1 Rozklad variability a F-test

ANOVA rozkládá celkovou variabilitu na dvě základní složky:
* **Variabilita mezi skupinami ($SS_{between}$):** Rozdíly mezi průměry skupin a celkovým průměrem.
* **Variabilita uvnitř skupin ($SS_{within}$):** Přirozená variabilita uvnitř jednotlivých skupin (šum).

Celkový součet čtverců:
$$SS_{total} = SS_{between} + SS_{within}$$

Z těchto veličin se počítají střední čtverce (MS) podělením příslušnými stupni volnosti:
$$MS_{between} = \frac{SS_{between}}{k - 1}, \quad MS_{within} = \frac{SS_{within}}{n - k}$$

Výsledná testová statistika F:
$$F = \frac{MS_{between}}{MS_{within}}$$

* Pokud jsou průměry skupin stejné, očekáváme $F \approx 1$.
* Výrazně velká hodnota $F$ překračující teoretickou kritickou hodnotu vede k zamítnutí $H_0$.

<img alt="img.png" src="img/statistika/soucty ctvercu.png" width="300"/>

### 2.2 Post-hoc testy
Pokud ANOVA zamítne $H_0$, víme, že existuje rozdíl, ale nevíme, mezi kterými konkrétními skupinami. K tomu slouží post-hoc testy:
* **Tukey HSD:** Porovnává všechny dvojice a matematicky koriguje hladinu významnosti.
* **Bonferroniho korekce:** Velmi přísná metoda, přímo dělí hladinu významnosti $\alpha$ celkovým počtem prováděných testů.
Bez těchto korekcí by při vícenásobném testování hrozilo, že najdeme "náhodný" rozdíl, který ve skutečnosti neexistuje (kumulace chyby I. typu).

---

## 3. Neparametrické testy hypotéz

Nevyžadují konkrétní tvar **rozdělení** (typicky normalitu) a nejčastěji pracují s pořadím (ranks) namísto původních hodnot. Používají se tehdy, když:
* Data vykazují silnou nenormalitu (šikmost, outliery).
* Máme malé výběry a normalitu nelze rozumně předpokládat.
* Závislá proměnná je ordinálního typu (např. škály bolesti, známky, spokojenost).
* Je porušen předpoklad homogenity rozptylů.
* Chceme robustnější postup vůči extrémním hodnotám.

Neparametrické testy bývají „konzervativnější“ (mají nižší sílu testu) než parametrické testy, pokud jsou předpoklady parametrických testů splněny. Naopak při silném porušení normality nebo přítomnosti outlierů mohou být výrazně spolehlivější.

### 3.1 Wilcoxonův párový test (Wilcoxon Signed-Rank Test)
Neparametrická alternativa k párovému t-testu. Hodí se, když máme stejné subjekty měřené dvakrát (před/po) a rozdíly vykazují nenormální rozdělení.
* **$H_0$:** Medián rozdílů mezi párovými měřeními je roven nule.
* **$H_1$:** Medián rozdílů se statisticky významně liší od nuly.
* *Příklad:* Měření krevního tlaku u stejných pacientů bezprostředně před podáním léku a hodinu po něm.

### 3.2 Mann-Whitneyův test (Wilcoxon Rank-Sum Test)
Porovnává dvě nezávislé skupiny bez předpokladu normality dat. Ověřuje, zda hodnoty v jedné skupině mají tendenci být systematicky větší než ve druhé (testuje rozdíl rozdělení polohy).
* **$H_0$:** Distribuce obou nezávislých skupin jsou totožné.
* **$H_1$:** Distribuce polohy obou skupin se liší (hodnoty jedné skupiny mají tendenci převyšovat druhou).
* *Příklad:* Máme dva typy hnojiva A a B a měříme výnos (kg) na nezávislých polích. Data jsou silně šikmá kvůli několika extrémně výnosným polím a chceme zjistit, jestli se výnosy liší.

### 3.3 Kruskal-Wallisův test
Neparametrická alternativa k jednofaktorové ANOVA pro tři a více nezávislých skupin.
* **$H_0$:** Všechny srovnávané skupiny pocházejí ze stejného rozdělení (mediány skupin jsou shodné).
* **$H_1$:** Alespoň jedna skupina se liší distribucí polohy od ostatních.
* *Příklad:* Máme 4 odrůdy brambor, ale hmotnosti trsů jsou výrazně nenormální s outliery. Chceme zjistit, zda se odrůdy liší.

<img width="600" alt="image" src="img/statistika/non-parametric-cheat-sheet1.jpg" />

---

## 4. Mnohonásobná lineární regrese

Zkoumá lineární vztah mezi **jednou spojitou závislou proměnnou a více nezávislými proměnnými**. Cílem je popsat, jak se mění hodnota závislé proměnné při změně několika vysvětlujících proměnných současně, a odhadnout jejich samostatný vliv při kontrole ostatních proměnných.

* *Příklad:* $\text{Cena bytu} = \beta_0 + \beta_1 \cdot \text{výměra} + \beta_2 \cdot \text{počet pokojů}$

### 4.1 Základní předpoklady lineární regrese
Aby byly odhady stabilní a testy spolehlivé, musí platit:
1. **Linearita:** Vztah mezi závislou proměnnou $Y$ a prediktory $X_j$ je lineární.
2. **Nezávislost chyb:** Rezidua $\varepsilon_i$ jsou na sobě nezávislá.
3. **Homoskedasticita:** Konstantní rozptyl náhodných chyb (reziduí).
4. **Normalita chyb:** Rezidua mají normální rozdělení, což je klíčové pro platnost t-testů a intervalů spolehlivosti.
5. **Absence dokonalé multikolinearity** – Mezi vysvětlujícími proměnnými ($X_j$) nesmí existovat přesný lineární vztah (dokonalá kolinearita), ani extrémně silná korelace. Pokud by byly proměnné dokonale závislé, matematicky nelze jednoznačně odhadnout regresní koeficienty $\beta$ (matice prediktorů by byla singulární a nešlo by ji invertovat).

### 4.2 Matematický model
$$Y_i = \beta_0 + \beta_1 X_{i1} + \beta_2 X_{i2} + \dots + \beta_p X_{ip} + \varepsilon_i$$

* $Y_i$ je hodnota závislé proměnné u $i$-tého pozorování.
* $\beta_0$ je absolutní člen (intercept).
* $\beta_j$ jsou regresní koeficienty vyjadřující vliv jednotlivých prediktorů.
* $X_{ij}$ jsou hodnoty vysvětlujících proměnných.
* $\varepsilon_i$ je náhodná chyba (reziduum).

<img width="600" alt="image" src="img/statistika/mnoho-regrese.png" />

*Graf: Hmotnost člověka v závislosti na výšce a pohlaví*

**Pozor:** Nezaměňovat jednoduchou regresi s více parametry s mnohonásobnou regresí! Záleží na počtu prediktorů ($X_j$ – u mnohonásobné musí být aspoň 2), ne na počtu parametrů $\beta$. Každý koeficient $\beta_j$ vyjadřuje, o kolik se v průměru změní $Y$, když se $X_j$ zvýší o 1 jednotku a ostatní proměnné zůstanou konstantní.

Parametry $\beta$ se počítají metodou nejmenších čtverců (OLS). Celková významnost modelu se následně testuje pomocí **F-testu**:
* $H_0: \beta_1 = \beta_2 = \dots = \beta_p = 0$ (žádná vysvětlující proměnná nemá vliv na $Y$).
* $H_1: \text{alespoň jedno } \beta_j \neq 0$ (alespoň jedna proměnná má prokazatelný vliv).

Dále testujeme významnost jednotlivých koeficientů pomocí **t-testu**:
* $H_0: \beta_j = 0$ (daná proměnná nemá při zohlednění ostatních vliv na $Y$).
* $H_1: \beta_j \neq 0$ (vliv dané proměnné je nenulový).

### 4.3 Koeficient determinace $R^2$
Udává, jaká část celkové variability závislé proměnné je vysvětlena modelem.

$$R^2 = \frac{S_R}{S_T} = 1 - \frac{S_e}{S_T}$$

* $S_T$ je celkový součet čtverců odchylek.
* $S_e$ je reziduální součet čtverců (nevysvětlená variabilita).
* $S_R$ je regresní součet čtverců (vysvětlená variabilita).

Protože při přidávání jakýchkoliv dalších proměnných hodnota $R^2$ téměř vždy roste, používá se **adjustované $R^2$**, které zohledňuje počet proměnných v modelu a penalizuje zbytečně složité modely (prevence overfittingu).

$$R^2_{adj} = 1 - \frac{\frac{S_e}{n-p-1}}{\frac{S_T}{n-1}}$$

### 4.4 Výběr proměnných a postupná regrese (Stepwise Regression)
Nalezení optimálního modelu při velkém počtu prediktorů bývá náročné kvůli vztahům mezi nimi. Jednou z metod je **postupná regrese**, která automaticky vybírá proměnné na základě kritérií (např. minimalizace Akaikeho informačního kritéria AIC).
Tato metoda má svá jasná omezení – jedná se o mechanický algoritmus, který může vést k overfittingu, nestabilním modelům a zcela ignoruje věcný či teoretický kontext kauzality.

---

### 4.5 Multikolinearita
Multikolinearita znamená, že některé vysvětlující proměnné ($X$) jsou mezi sebou silně lineárně závislé (korelované). Model pak velmi obtížně rozlišuje jejich samostatný izolovaný vliv na závislou proměnnou.

* *Příklad:* V modelu mzdy použijeme současně věk, délku praxe a počet let od ukončení školy – tyto proměnné spolu extrémně úzce souvisejí.

#### Důsledky multikolinearity:
Odhady regresních koeficientů jsou nestabilní, mají obrovské směrodatné chyby a jednotlivé proměnné mohou vycházet jako statisticky nevýznamné, i když se závislou proměnnou silně souvisejí. Model sice dobře predikuje, ale nelze jej spolehlivě interpretovat.

Míra multikolinearity se detekuje pomocí ukazatele **VIF (Variance Inflation Factor)**:
$$VIF_j = \frac{1}{1 - R_j^2}$$

* $VIF_j \approx 1$: Bez problému (absence kolinearity).
* $VIF_j > 5$: Možný problém s kolinearitou.
* $VIF_j > 10$: Silná, kritická multikolinearita.

<img alt="img.png" src="img/statistika/multikol.png" width="400"/>

---

### 4.6 Autokorelace
Autokorelace nastává v situaci, kdy náhodné chyby (rezidua) $\varepsilon_i$ v modelu nejsou nezávislé, ale vykazují vzájemnou závislost. Nejčastěji se vyskytuje u **časových řad**, kde hodnota v čase $t$ přímo souvisí s hodnotou v čase $t-1$.

#### Důsledky autokorelace:
Odhady koeficientů $\beta$ zůstávají nestranné, ale nejsou efektivní (mají velký rozptyl). OLS systematicky podhodnocuje rozptyl chyb, což vede k **neplatnosti t-testů a p-hodnot** (model indikuje falešnou významnost prediktorů).

Detekuje se nejčastěji pomocí **Durbin-Watsonova testu** (hodnoty v rozsahu 0 až 4):
* $DW \approx 2$: Rezidua jsou nezávislá (v pořádku).
* $DW \to 0$: Silná pozitivní autokorelace.
* $DW \to 4$: Silná negativní autokorelace.

*Řešení:* Použití metod upravených pro časové řady (např. zobecněné nejmenší čtverce GLS).

<img alt="img.png" src="img/statistika/autokor.png" width="400"/>

---

## 5. Analýza hlavních komponent (PCA - Principal Component Analysis)

PCA je statistická metoda redukce dimenze, která převádí původní (často silně korelované) proměnné na menší počet nových, lineárně nezávislých proměnných – **hlavních komponent (PC)**. PCA lze chápat jako ortonormální projekci dat do nového podprostoru nižší dimenze tak, aby byla minimalizována ztráta informace o celkové variabilitě.

* *Příklad:* Máme 10 různých měření tělesných proporcí člověka, která spolu úzce souvisejí, a chceme je pro zjednodušení nahradit 2–3 hlavními komponentami.

<img width="400" alt="image" src="img/statistika/pca-priklad.png" />

*Redukce dimenzí ze 3D na 2D*

PCA hledá nové osy v prostoru dat tak, aby:
* První osa ($PC_1$) vykazovala co největší možný rozptyl dat.
* Druhá osa ($PC_2$) měla co největší rozptyl ze zbytku variability a byla striktně kolmá na první osu.
* Další komponenty se určují analogicky.

První hlavní komponenta tedy zachycuje největší část informace obsažené v datech, zatímco poslední komponenty často nesou jen minimální rozptyl nebo náhodný šum.

<img width="300" alt="image" src="img/statistika/pca.png" />

*Optimální a neoptimální báze*

### 5.1 Matematický tvar hlavních komponent
Každá hlavní komponenta je lineární kombinací původních proměnných:
$$PC_k = a_{k1}X_1 + a_{k2}X_2 + \dots + a_{kp}X_p$$

Kde $X_j$ jsou původní proměnné a $a_{kj}$ jsou koeficienty určující směr komponenty (*loadings*). Tyto koeficienty tvoří **vlastní vektory** kovarianční nebo korelační matice.

### 5.2 Výpočet a Spektrální rozklad
Před použitím PCA je nezbytné data **standardizovat**, zejména pokud jsou proměnné v různých fyzikálních jednotkách, jinak by proměnné s přirozeně větším měřítkem zkreslily celou analýzu.

Následně se určí charakteristická rovnice matice:
$$\mathbf{S}\mathbf{a}_k = \lambda_k \mathbf{a}_k$$

Kde $\mathbf{S}$ je kovarianční nebo korelační matice, $\mathbf{a}_k$ je vlastní vektor a $\lambda_k$ je **vlastní číslo**, které vyjadřuje rozptyl vysvětlený danou $k$-tou komponentou.

Podíl vysvětlené variability $k$-tou komponentou odpovídá zlomku:
$$\frac{\lambda_k}{\sum_{j=1}^p \lambda_j}$$

### 5.3 Volba počtu komponent
Počet ponechaných komponent se volí na základě několika kritérií:
* Ponechat tolik komponent, aby kumulativně vysvětlily cca 80–90 % celkové variability.
* **Kaiserovo pravidlo:** Ponechat pouze komponenty s vlastním číslem $\lambda_k > 1$.
* **Scree plot (Sutinový graf):** Vizuální graf vlastních čísel, kde se hledá zlom ("koleno"), za kterým už další komponenty přinášejí zanedbatelné množství informace.

<img width="300" alt="image" src="img/statistika/pca-scree.png" />

*Scree plot*

### 5.4 Scores a Loadings
* **Loadings:** Koeficienty definující váhu původních proměnných v komponentě (říkají, jak silně proměnné s PC korelují).
* **Scores:** Nově vypočítané souřadnice konkrétních pozorování v prostoru hlavních komponent, které lze vizualizovat v grafech.

#### Hlavní omezení PCA:
Jedná se o čistě lineární metodu, která je velmi citlivá na odlehlá pozorování. Hlavní nevýhodou je často obtížná nebo nemožná věcná interpretace jednotlivých PC (např. kvůli kombinaci nekompatibilních fyzikálních jednotek v jedné rovnici).