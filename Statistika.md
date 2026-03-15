# Statistika

> Základní statistické metody (bodové odhady, intervaly spolehlivosti, testování statistických hypotéz). ANOVA. Neparametrické testy hypotéz. Mnohonásobná lineární regrese, autokorelace, multikolinearita. Analýza hlavních komponent (PCA). (MA012)

## 1. Základní statistické metody

### 1.1 Bodové odhady
- Výběrový průměr  
- Výběrový rozptyl a směrodatná odchylka  
- Odhad parametrů rozdělení  
- Metoda momentů  
- Metoda maximální věrohodnosti  
  
### 1.2 Intervaly spolehlivosti
- Interval spolehlivosti pro střední hodnotu (známý / neznámý rozptyl)  
- Interval spolehlivosti pro rozptyl  
- Interval spolehlivosti pro podíl  
- Interpretace hladiny spolehlivosti  

### 1.3 Testování statistických hypotéz
- Nulová a alternativní hypotéza  
- Testová statistika  
- Kritický obor  
- p-hodnota  
- Chyba I. a II. druhu  
- Síla testu  

---

## 2. ANOVA
ANOVA (ANalysis Of VAriance – analýza rozptylu) je statistická metoda pro porovnávání průměrů tří a více skupin. Zjišťuje, zda nezávislé kategorické proměnné (faktory) statisticky významně ovlivňují spojitou závislou proměnnou, a to rozkladem celkové variability na rozptyl mezi skupinami a uvnitř skupin.

_Například mám 4 různé odrůdy brambor sesbírané po trsech s různou hmotností. Zajímá mě, jestli je některá odrůda výnosnější než jiná (průměrná hmotnost trsu se liší na základě odrůdy)_


$H_0$: Předpokládá, že všechny průměry skupin jsou stejné. $$\mu_i = \dots = \mu_a \quad \text{pro všechna } i = 1, \dots, k$$

$H_1$: Alespoň jeden průměr skupiny je odlišný. $$\exists\, i,j \in \{1,\dots,k\},\ i \neq j:\ \mu_i \neq \mu_j$$

<img width="434" height="289" alt="image" src="img/anova.png"  />

Předpoklady ANOVA
1) **Nezávislost výběrů**  Jednotlivá pozorování musí být na sobě nezávislá. Pokud nejsou, mění se rozdělení testové statistiky i p-hodnota → výsledky nejsou spolehlivé.
2) **Normalita dat**  Ověřuje se pomocí Q-Q grafu nebo testů (např. Shapiro–Wilk). ANOVA je poměrně odolná – pokud má každá skupina alespoň ~20 pozorování a data nejsou silně vychýlená, malé porušení nevadí, při silném porušení použijeme neparametrický test (např. Kruskal–Wallis).
3) **Shoda rozptylů (homogenita variancí)**  Ověřuje se např. Leveneho nebo Bartlettovým testem, mírné porušení většinou nevadí, pokud mají skupiny podobnou velikost.

Jednofaktorová ANOVA zkoumá vliv jedné nezávislé proměnné (faktoru). Vícefaktorová ANOVA (dvou- či více-) analyzuje vliv dvou a více faktorů současně, včetně jejich vzájemných interakcí na závislou proměnnou.

_Například mám 3 různé druhy hnojiva a 2 různé typy půdy, a výnos polí. Zajímá mě, jestli je některá kombinace hnojiva a typu půdy výnosnější než jiná. Mám faktory hnojivo a typ půdy._

 U dvoufaktorové ANOVA rozlišujeme model **bez interakce** a model **s interakcí** mezi faktory.
 - bez interakce předpokládáme, že vliv jednoho faktoru je stejný bez ohledu na úroveň druhého faktoru  _Například hnojivo A zvyšuje výnos vždy o +2 kg bez ohledu na typ půdy_
 - s interakcí připouštíme, že efekt jednoho faktoru závisí na úrovni druhého faktoru. _Hnojivo A je nejlepší na jílovité půdě, ale na písčité půdě je lepší hnojivo B._

<img width="1000"  alt="image" src="img/anova2faktory.png"  />

Rozšíření: sums of squares, F test mezi nimi, anova model, co jsou post hoc testy

---

## 3. Neparametrické testy hypotéz
Nevyžadují konkrétní tvar **rozdělení** (typicky normalitu) a často pracují s pořadím (ranks) místo s původními hodnotami. Používají se hlavně tehdy, když:
- data nejsou normální (silná šikmost, outliery),
- máme malé výběry a normalitu nelze rozumně předpokládat,
- závislá proměnná je ordinální (pořadová škála: „souhlasím–nesouhlasím“, škály bolesti, známky…),
- porušujeme předpoklad homogenity rozptylů (u některých situací),
- chceme robustnější postup vůči extrémům.

Neparametrické testy bývají „konzervativnější“ (méně citlivé) než parametrické testy, pokud jsou předpoklady parametrických testů splněny. Naopak při silném porušení normality nebo při outlierech mohou být neparametrické testy spolehlivější.

### Wilcoxonův párový test (= Wilcoxon signed-rank)
Neparametrická alternativa k párovému t-testu. Hodí se, když máme stejné subjekty měřené dvakrát (před/po), nebo páry, a rozdíly nejsou normální. _Typicky před a po: např. krevní tlak před a po podání léku._
### Mann-Whitneyův test  (= Wilcoxon rank-sum)
Porovnává dvě nezávislé skupiny bez předpokladu normality. Intuitivně ověřuje, zda hodnoty v jedné skupině mají tendenci být větší než ve druhé. _Například mám dva typy hnojiva A a B a měřím výnos (kg) na nezávislých polích. Data jsou silně šikmá kvůli pár extrémně výnosným polím. Chci zjistit, jestli se výnosy mezi hnojivy liší._
### Kruskal-Wallisův test
Kruskal–Wallisův test je neparametrická alternativa k jednofaktorové ANOVA pro tři a více nezávislých skupin. _Například mám 4 odrůdy brambor, ale hmotnosti trsů jsou výrazně nenormální a s outliery. Chci zjistit, zda se odrůdy liší._

<img width="600" alt="image" src="img/non-parametric-cheat-sheet1.jpg"  />

Rozšíření: Jednotlivé hypotézy a testové statistiky

---

## 4. Mnohonásobná lineární regrese

Zkoumá vztah mezi **jednou spojitou závislou proměnnou a více nezávislými proměnnými**. Cílem je popsat, jak se mění hodnota závislé proměnné při změně několika vysvětlujících proměnných současně, a odhadnout jejich samostatný vliv při kontrole ostatních proměnných.

_např. cena bytu = $\beta_0 + \beta_1*$výměra $+ \beta_2 *$počet pokojů_

Model:
$$
Y_i = \beta_0 + \beta_1 X_{i1} + \beta_2 X_{i2} + \dots + \beta_p X_{ip} + \varepsilon_i
$$

- $Y_i$ je hodnota závislé proměnné u $i$-tého pozorování,
- $\beta_0$ je absolutní člen,
- $\beta_1, \beta_2, \dots, \beta_p$ jsou regresní koeficienty,
- $X_{i1}, X_{i2}, \dots, X_{ip}$ jsou hodnoty vysvětlujících proměnných,
- $\varepsilon_i$ je náhodná chyba.

<img width="600" alt="image" src="img/mnoho-regrese.png" />

_Graf: hmotnost člověka ~ výška a pohlaví_

Pozor, neplést s jednoduchou regresí s více parametry! Záleží na tom, kolik je prediktorů ($X_j$ - u mnohonásobné musí být aspoň 2), ne parametrů ($\beta$). Každý koeficient $\beta_j$ vyjadřuje, o kolik se v průměru změní závislá proměnná $Y$, když se proměnná $X_j$ zvýší o 1 jednotku a ostatní proměnné zůstanou stejné.

Parametry $\beta$ spočítám metodou nejmenších čtverců, ale pak ještě testuji jejich celkovou významnost pomocí F-testu.

- $H_0$: žádná vysvětlující proměnná nemá vliv na $Y$, $\beta_1 = \beta_2 = \dots = \beta_p = 0$
- $H_1$: alespoň jedna proměnná vliv má, $\text{alespoň jedno } \beta_j \neq 0$

Taky se testují jednotlivé významnosti pomocí t-testu.

- $H_0$: daná proměnná nemá při zohlednění ostatních proměnných vliv na $Y$, $\beta_j = 0$

- $H_1$: vliv dané proměnné je nenulový. $\beta_j \neq 0$

### Koeficient determinace R²
Kvalitu modelu často popisujeme pomocí **koeficientu determinace**, který udává, jaká část variability závislé proměnné je vysvětlena modelem.

$$
R^2 = \frac{S_R}{S_T} = 1 - \frac{S_e}{S_T}
$$

- $S_T$ je celkový součet čtverců,
- $S_e$ je reziduální součet čtverců.
- $S_R$ je regresní součet čtverců. $S_R = S_T − S_e ≥ 0$

_Např. $R^2 = 0.8$ znamená, že model vysvětluje 80 % variability závislé proměnné._

Protože při přidávání dalších proměnných $R^2$ skoro vždy roste, používá se i **adjustované $R^2$**, které zohledňuje počet proměnných v modelu a penalizuje zbytečně složitý model (penalizuje overfitting).

$$
R^2_{adj} = 1 - \frac{\frac{S_e}{(n-p-1)}}{\frac{S_T}{(n-1)}}
$$

I když model na první pohled vychází dobře, mohou se v něm objevit problémy, které zkreslují závěry. Typicky jde o **multikolinearitu** a **autokorelaci**.

## Multikolinearita
Multikolinearita znamená, že některé vysvětlující proměnné jsou mezi sebou silně lineárně závislé. Model pak obtížně rozlišuje jejich samostatný vliv na závislou proměnnou.

_např. v modelu mzdy použiju současně věk, délku praxe a počet let od ukončení školy – tyto proměnné spolu úzce souvisejí_

Důsledkem multikolinearity je, že odhady regresních koeficientů mohou být nestabilní, mají větší směrodatné chyby a jednotlivé proměnné pak mohou vycházet jako statisticky nevýznamné, i když ve skutečnosti se závislou proměnnou souvisejí. Model tedy může dobře predikovat, ale hůře se interpretuje.

obrázek

Často se sleduje pomocí ukazatele VIF (Variance Inflation Factor)

$$
VIF_j = \frac{1}{1 - R_j^2}
$$

- $R_j^2 \approx 0 => VIF_j \approx 1$ znamená bez problému,
- $R_j^2 > 0 => VIF_j > 1$ vysoká korelace mazi vysvětlujícími proměnnými vede k vyššímu $VIF$,
- $VIF_j > 10$ ukazuje na výraznou multikolinearitu.

Nalezení vhodného modelu při velkém počtu prediktorů může být obtížné kvůli korelacím a vztahům mezi nimi. Neexistuje žádný algoritmus, který by obecně našel nejlepší model. Existuje mnoho různých kritérií, některá jsou citlivá na specifický datový typ, některá jsou relativní. Často používaným algoritmem je **metoda postupné regrese** (stepwise regression method), kritérium může být třeba Akaike information criterion (**AIC** → min).

## Autokorelace

---

## 5. Analýza hlavních komponent (PCA)
PCA (Principal Component Analysis – analýza hlavních komponent) je metoda redukce dimenze, která převádí původní (často korelované) proměnné na menší počet nových proměnných – hlavních komponent.
Nevýhodou PCA je nemožnost interpretace PC (např. kvůli nekompatibilitě použitých fyzikálních jednotek), protože každá PC je nějakou lineární kombinací původních proměnných.

PCA se používá hlavně tehdy, když máme mnoho proměnných a chceme:
- zjednodušit data,
- odstranit redundanci mezi silně korelovanými proměnnými,
- zobrazit vícerozměrná data ve 2D nebo 3D,
- použít menší počet nových proměnných v dalších analýzách.

_např. mám 10 měření tělesných proporcí člověka, která spolu silně souvisejí, a chci je nahradit 2–3 hlavními komponentami_


<img width="400" alt="image" src="img/pca-priklad.png" />

_Redukce dimenzí ze 3D na 2D_


PCA hledá nové osy v prostoru dat tak, aby:
- první osa měla co největší rozptyl dat,
- druhá osa měla co největší rozptyl ze zbytku variability a byla kolmá na první,
- další komponenty se určují obdobně.

První hlavní komponenta tedy zachycuje největší možnou část informace obsažené v datech, další komponenty zachycují zbývající variabilitu.

<img width="300" alt="image" src="img/pca.png" />

_Optimální a neoptimální báze_

### Matematický tvar hlavních komponent

Každá hlavní komponenta je lineární kombinací původních proměnných:

$$
PC_1 = a_{11}X_1 + a_{12}X_2 + \dots + a_{1p}X_p
$$

$$
PC_2 = a_{21}X_1 + a_{22}X_2 + \dots + a_{2p}X_p
$$

obecně

$$
PC_k = a_{k1}X_1 + a_{k2}X_2 + \dots + a_{kp}X_p
$$

kde:
- $X_1, \dots, X_p$ jsou původní proměnné,
- $a_{kj}$ jsou koeficienty určující směr komponenty.

Tyto koeficienty tvoří **vlastní vektory (eigenvektory)** kovarianční nebo korelační matice.

### Výpočet PCA

Před použitím PCA je často potřeba data **standardizovat**, zejména pokud jsou proměnné v různých jednotkách nebo mají velmi rozdílné rozptyly. Jinak by proměnné s větším měřítkem měly nepřiměřeně velký vliv.

Pak se spočítá:
- **kovarianční matice**, pokud mají proměnné srovnatelné jednotky,
- nebo **korelační matice**, pokud jsou proměnné v různých jednotkách.

Z této matice se určují:
- **vlastní vektory** – směry hlavních komponent,
- **vlastní čísla** – rozptyl vysvětlený jednotlivými komponentami.

Platí:

$$
\mathbf{S}\mathbf{a}_k = \lambda_k \mathbf{a}_k
$$

kde:
- $\mathbf{S}$ je kovarianční nebo korelační matice,
- $\mathbf{a}_k$ je vlastní vektor,
- $\lambda_k$ je vlastní číslo příslušné $k$-té komponentě.

### Vysvětlená variabilita

Každé vlastní číslo udává, kolik variability vysvětluje daná komponenta. Podíl vysvětlené variability $k$-tou komponentou je:

$$
\frac{\lambda_k}{\sum_{j=1}^p \lambda_j}
$$

Kumulativní vysvětlená variabilita prvních $m$ komponent je:

$$
\frac{\sum_{k=1}^m \lambda_k}{\sum_{j=1}^p \lambda_j}
$$

Podle toho se rozhodujeme, kolik komponent ponechat.

### Volba počtu komponent

Počet ponechaných komponent se volí tak, aby se zachovala co největší část informace a zároveň došlo ke zjednodušení dat. Často se používají tato kritéria:
- ponechat tolik komponent, aby kumulativně vysvětlily např. 80–90 % variability,
- **Kaiserovo pravidlo** – ponechat komponenty s vlastním číslem větším než 1,
- **scree plot** (sutinový graf) – graf vlastních čísel, hledá se „zlom“, za kterým už další komponenty přidávají málo informace.

<img width="300" alt="image" src="img/pca-scree.png" />

_Scree plot_

### Scores a loadings

U PCA se často rozlišují:
- **loadings** – koeficienty, které říkají, jak silně se původní proměnné podílejí na jednotlivých komponentách,
- **scores** – nové souřadnice pozorování v prostoru hlavních komponent.

Díky scores můžeme zobrazit objekty v prostoru prvních dvou nebo tří komponent a hledat shluky, odlehlá pozorování nebo strukturu dat.

### Interpretace PCA

PCA je užitečná pro redukci dimenze, ale interpretace komponent bývá obtížná. Každá hlavní komponenta je kombinací více proměnných, takže často není možné jí dát jednoznačný věcný význam.

Například pokud první komponenta silně souvisí s výškou, délkou končetin a hmotností, můžeme ji interpretovat jako „celkovou velikost těla“, ale tato interpretace je vždy jen přibližná.

### Vlastnosti PCA

- komponenty jsou navzájem nekorelované,
- celková variabilita se zachovává,
- první komponenty nesou nejvíce informace,
- poslední komponenty často obsahují jen malou část variability a někdy hlavně šum.

### Omezení PCA

- PCA je lineární metoda,
- je citlivá na měřítko proměnných, proto bývá nutná standardizace,
- může být citlivá na odlehlá pozorování,
- hlavní komponenty nemusí mít snadnou interpretaci,
- vysoká vysvětlená variabilita ještě nemusí znamenat dobrou praktickou interpretovatelnost.
