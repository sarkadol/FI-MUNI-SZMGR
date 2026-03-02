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

Jednofaktorová ANOVA zkoumá vliv jedné nezávislé proměnné (faktoru). Vícefaktorová ANOVA (dvou- či více-) analyzuje vliv dvou a více faktorů současně, včetně jejich vzájemných interakcí na závislou proměnnou.

_Například mám 3 různé druhy hnojiva a 2 různé typy půdy, a výnos polí. Zajímá mě, jestli je některá kombinace hnojiva a typu půdy výnosnější než jiná. Mám faktory hnojivo a typ půdy._

 U dvoufaktorové ANOVA rozlišujeme model **bez interakce** a model **s interakcí** mezi faktory.
 - bez interakce předpokládáme, že vliv jednoho faktoru je stejný bez ohledu na úroveň druhého faktoru  _Například hnojivo A zvyšuje výnos vždy o +2 kg bez ohledu na typ půdy_
 - s interakcí připouštíme, že efekt jednoho faktoru závisí na úrovni druhého faktoru. _Hnojivo A je nejlepší na jílovité půdě, ale na písčité půdě je lepší hnojivo B._

<img width="1000"  alt="image" src="img/anova2faktory.png"  />

Předpoklady ANOVA
1) **Nezávislost výběrů**  Jednotlivá pozorování musí být na sobě nezávislá. Pokud nejsou, mění se rozdělení testové statistiky i p-hodnota → výsledky nejsou spolehlivé.
2) **Normalita dat**  Ověřuje se pomocí Q-Q grafu nebo testů (např. Shapiro–Wilk). ANOVA je poměrně odolná – pokud má každá skupina alespoň ~20 pozorování a data nejsou silně vychýlená, malé porušení nevadí, při silném porušení použijeme neparametrický test (např. Kruskal–Wallis).
3) **Shoda rozptylů (homogenita variancí)**  Ověřuje se např. Leveneho nebo Bartlettovým testem, mírné porušení většinou nevadí, pokud mají skupiny podobnou velikost.

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

### 4.1 Model
- Tvar modelu  
- Odhad parametrů (metoda nejmenších čtverců)  
- Interpretace regresních koeficientů  

### 4.2 Diagnostika modelu
- Koeficient determinace (R², Adjusted R²)  
- Test významnosti modelu  
- Test významnosti jednotlivých koeficientů  

### 4.3 Problémy regrese
- Autokorelace  
- Multikolinearita  
- Heteroskedasticita  

---

## 5. Analýza hlavních komponent (PCA)
- Motivace redukce dimenze  
- Kovarianční a korelační matice  
- Vlastní čísla a vlastní vektory  
- Hlavní komponenty  
- Vysvětlená variabilita  
- Interpretace komponent  
