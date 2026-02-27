# Statistika (MA012)

## Popis okruhu

Základní statistické metody (bodové odhady, intervaly spolehlivosti, testování statistických hypotéz). ANOVA. Neparametrické testy hypotéz. Mnohonásobná lineární regrese, autokorelace, multikolinearita. Analýza hlavních komponent (PCA).

## Témata

### Základní statistické metody

#### Bodové odhady
- Definice odhadů a vlastnosti odhadů (nestrannost, konzistence, eficience)
- Metoda maximální věrohodnosti (MLE)
- Metoda momentů
- Bayesovský odhad

#### Intervaly spolehlivosti
- Definice intervalu spolehlivosti a hladiny spolehlivosti
- Intervaly pro průměr (známý/neznámý rozptyl): z-interval, t-interval
- Intervaly pro rozptyl (chi-kvadrát rozdělení)
- Intervaly pro podíl (binomické pravděpodobnosti)
- Bootstrap intervaly

#### Testování statistických hypotéz
- Nulová a alternativní hypotéza
- Testovací statistika, p-hodnota, kritická oblast
- Chyba I. a II. druhu (α, β), síla testu
- Jednovýběrové a dvouvýběrové testy pro průměr (z-test, t-test)
- Testy pro rozptyl (F-test, chi-kvadrát test)
- Testy pro podíly

### ANOVA (Analýza rozptylu)
- Jednocestná ANOVA: předpoklady, F-statistika, dekompozice celkového rozptylu (SST, SSB, SSW)
- Mnohonásobná porovnání a post-hoc testy (Tukey, Bonferroni)
- Dvoucestná ANOVA: hlavní efekty a interakce
- MANOVA (vícerozměrná analýza rozptylu) – úvod

### Neparametrické testy hypotéz
- Motivace: robustnost vůči porušení normality
- Wilcoxonův test pro jeden výběr (znaménkový test, Wilcoxon signed-rank test)
- Mann-Whitneyův U-test (dvouvýběrový neparametrický test)
- Kruskal-Wallisův test (neparametrická ANOVA)
- Friedmanův test (neparametrická dvoucestná ANOVA)
- Testy dobré shody: chi-kvadrát test, Kolmogorov-Smirnovův test

### Mnohonásobná lineární regrese
- Model mnohonásobné lineární regrese: maticový zápis, odhad metodou nejmenších čtverců (OLS)
- Koeficient determinace R² a adjustovaný R²
- Testy významnosti koeficientů (t-test) a celkového modelu (F-test)
- Výběr proměnných: dopředná/zpětná selekce, stepwise regrese

#### Autokorelace
- Definice autokorelace reziduí
- Durbin-Watsonův test
- Důsledky autokorelace a opravné metody (Cochrane-Orcutt)

#### Multikolinearita
- Definice a příčiny multikolinearity
- Diagnostika: korelační matice, faktor inflace rozptylu (VIF)
- Řešení: redukce proměnných, hřebenová regrese (ridge regression)

### Analýza hlavních komponent (PCA)
- Motivace: redukce dimenzionality, odstranění korelací
- Kovarianční matice a její vlastní vektory/hodnoty
- Výpočet hlavních komponent (PCA přes SVD nebo vlastní rozklad)
- Výběr počtu komponent: kumulativní vysvětlený rozptyl, scree plot
- Interpretace hlavních komponent
- Aplikace PCA: vizualizace dat, předzpracování pro strojové učení
