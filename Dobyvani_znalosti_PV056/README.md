# Dobývání znalostí (PV056)

> **Poznámka:** Pro absolventy předmětu do jara 2024 včetně.

## Popis okruhu

Předzpracování dat. Učení častých vzorů a asociačních pravidel. Nástroje pro strojové učení a dolování z dat (obecně + popis jednoho podrobně). Analýza temporálních dat.

## Témata

### Předzpracování dat
- Motivace: kvalita dat a její vliv na výsledky analýzy
- Typy dat: nominální, ordinální, intervalová, poměrová
- Čištění dat: detekce a oprava chybějících hodnot, detekce a zpracování odlehlých hodnot (outliers)
- Integrace dat: řešení konfliktů při slučování více zdrojů, entitní rozlišení
- Transformace dat: normalizace (min-max, z-score), diskretizace (binning, entropie), agregace
- Redukce dat: výběr příznaků (feature selection), extrakce příznaků (PCA, LDA), vzorkování
- Kódování kategorických proměnných: one-hot encoding, label encoding, target encoding
- Datové nerovnováhy (class imbalance): oversampling (SMOTE), undersampling

### Učení častých vzorů a asociačních pravidel

#### Základní pojmy
- Transakční databáze, itemset, podpora (support), důvěra (confidence)
- Výtah (lift), přesvědčení (conviction)
- Frekventovaný itemset vs. asociační pravidlo

#### Algoritmus Apriori
- Princip antimonotonicity (Apriori vlastnost)
- Generování kandidátů a prořezávání
- Výpočet podpory
- Časová a prostorová složitost

#### Algoritmus FP-Growth
- FP-strom: struktura a konstrukce
- Mining frekventovaných itemsetů z FP-stromu
- Srovnání s Apriori: efektivita, paměťová náročnost

#### Další algoritmy
- ECLAT: vertikální reprezentace dat
- Maximal a closed frekventované itemsety

### Nástroje pro strojové učení a dolování z dat

#### Obecný přehled
- WEKA: Java-based, GUI a API, algoritmy pro klasifikaci, clustering, asociační pravidla
- RapidMiner: vizuální workflow, komerční i open-source verze
- Orange: Python-based, vizuální programování
- scikit-learn: Python knihovna pro strojové učení
- Knime: enterprise data analytics

#### Podrobný popis: scikit-learn
- Architektura: estimátory, transformátory, prediktory, pipelines
- Preprocessing: StandardScaler, MinMaxScaler, OneHotEncoder, SimpleImputer
- Klasifikace: SVM, Random Forest, KNN, Decision Trees, Logistic Regression
- Regrese: LinearRegression, Ridge, Lasso, SVR
- Clustering: KMeans, DBSCAN, AgglomerativeClustering
- Výběr modelu: cross_val_score, GridSearchCV, RandomizedSearchCV
- Metriky hodnocení: accuracy, precision, recall, F1, AUC-ROC, confusion matrix
- Pipelines: řetězení transformací a modelů
- Model persistence: joblib, pickle

### Analýza temporálních dat
- Typy temporálních dat: časové řady, event log, sekvenční data
- Reprezentace časových řad: ekvidistantní vzorkování, nerovnoměrné časové razítko
- Předzpracování časových řad:
  - Normalizace a standardizace
  - Interpolace chybějících hodnot
  - Detekce a zpracování anomálií
  - Dekompozice: trend, sezónnost, zbytek (STL, additivní vs. multiplikativní model)
- Klouzavý průměr (Moving Average, MA): prostý, vážený, exponenciální (EMA)
- Autokorelace a parciální autokorelace (ACF, PACF)
- Modely pro časové řady: AR, MA, ARMA, ARIMA, SARIMA
- Dynamic Time Warping (DTW): definice, výpočet (dynamické programování), využití pro klasifikaci a shlukování
- Shapelets: charakteristické podsekvence pro klasifikaci
- Frekvenční analýza: Fourierova transformace, diskrétní Fourierova transformace (DFT)
