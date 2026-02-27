# Podobnostní hledání (PA128)

> **Poznámka:** Povinné pro studium dle kontrolní šablony 2022/2023 nebo novější.

## Popis okruhu

Principy podobnostního hledávání: metrický prostor, extrakce popisovačů a jejich vztah s člověkem vnímanou podobností, typy dotazů a jejich definice. Principy indexování: dělení dat, filtrování dat (pivoting). Srovnání s tradičními indexy (B+ trees).

## Témata

### Principy podobnostního hledávání

#### Metrický prostor
- Definice metrického prostoru: (U, d), kde U je univerzum objektů a d: U × U → ℝ je metrika
- Axiomy metriky:
  1. **Nezápornost:** d(x, y) ≥ 0
  2. **Identita:** d(x, y) = 0 ⟺ x = y
  3. **Symetrie:** d(x, y) = d(y, x)
  4. **Trojúhelníková nerovnost:** d(x, z) ≤ d(x, y) + d(y, z)
- Příklady metrik: Euklidovská vzdálenost, Manhattanská vzdálenost, Hammingova vzdálenost, editační vzdálenost (edit distance), Cosinová vzdálenost

#### Extrakce popisovačů (Feature Extraction)
- Cíl: reprezentovat komplexní objekty (obrázky, texty, zvuky) jako vektory příznaků
- Popisovač (descriptor/feature vector): kompaktní numerická reprezentace objektu
- Vztah s člověkem vnímanou podobností:
  - Sémantická propast (semantic gap): rozdíl mezi nízkoúrovňovými příznaky a vysokoúrovňovým vnímáním
  - Cíl: navrhnout popisovače, jejichž vzdálenost odpovídá lidskému vnímání podobnosti
- Příklady popisovačů:
  - **Obrazy:** SIFT, SURF, HOG, color histogramy, hluboké embeddingy (CNN features)
  - **Text:** TF-IDF vektor, word2vec, BERT embeddingy
  - **Zvuk:** MFCC (Mel-Frequency Cepstral Coefficients)
  - **Video:** temporální příznaky, 3D konvoluční sítě

#### Typy dotazů v podobnostním hledání

1. **Range query (sférický dotaz):** nalezení všech objektů v databázi v okruhu poloměru r od dotazového objektu q
   - Formálně: {o ∈ D : d(q, o) ≤ r}

2. **k-NN dotaz (k nejbližších sousedů):** nalezení k objektů nejbližších dotazovacímu objektu
   - Formálně: k objektů S ⊆ D takových, že ∀o ∈ S, ∀o' ∈ D\S: d(q, o) ≤ d(q, o')

3. **Reverse k-NN dotaz:** objekty, pro které je q jedním z jejich k nejbližších sousedů

4. **All k-NN dotaz:** k-NN dotazy pro každý objekt v databázi (využití ve shlukování)

### Principy indexování

#### Motivace pro indexování
- Sekvenční procházení databáze: O(n) výpočtů vzdálenosti – neškálovatelné
- Cíl indexování: redukce počtu výpočtů vzdálenosti pomocí předpočítaných struktur
- Klíčová technika: dělení dat a filtrování

#### Dělení dat (Data Partitioning)
- **Ball trees (M-stromy):** rekurzivní dělení prostoru do koulí
  - Vnitřní uzel: centrum a poloměr pokrývající všechny objekty podstromu
  - Princip: pokud d(q, pivot) - r(node) > threshold, přeskoč celý podstrom
- **kd-stromy:** rekurzivní dělení prostoru hyperrovinami rovnoběžnými s osami
  - Efektivní pro nízké dimenze, degraduje pro vysoké dimenze (curse of dimensionality)
- **Voroného dělení:** přiřazení každého bodu k nejbližšímu pivotu; indexace podle partice

#### Filtrování dat – Pivoting (Technika pivotů)
- Myšlenka: vybrat k referenčních bodů (pivotů) p₁, ..., pₖ ∈ U
- Předpočítání: pro každý objekt databáze o uložit vzdálenosti d(o, pᵢ) pro i = 1..k
- Filtrování při dotazu: pomocí trojúhelníkové nerovnosti
  - |d(q, pᵢ) - d(o, pᵢ)| > r ⟹ d(q, o) > r (objekt lze vyloučit bez výpočtu d(q, o))
- Výpočet lower bound: lb(q, o) = max_i |d(q, pᵢ) - d(o, pᵢ)| ≤ d(q, o)
- Výběr pivotů: náhodný, maximalizace rozptylu vzdáleností

#### M-strom (Metric Tree)
- Dynamická indexovací struktura pro metrické prostory
- Vnitřní uzly: pivot (routovací objekt) + poloměr pokrývajícího prostoru
- Listové uzly: objekty databáze s předpočítanými vzdálenostmi od rodičovského pivotu
- Operace: vyhledávání (range, k-NN), vkládání, mazání
- Rozdělování uzlů: různé strategie (random, sampling, cluster-based)

### Srovnání s tradičními indexy (B+ stromy)

| Vlastnost | B+ strom | Metrický index (pivoting, M-strom) |
|---|---|---|
| **Typ dat** | 1D nebo nD s totálním uspořádáním | Libovolný metrický prostor |
| **Operace** | Přesné dotazy, range queries na uspořádané klíče | Similarity queries (range, k-NN) |
| **Složitost dotazu** | O(log n) | Závisí na dimenzi a datasetu |
| **Škálovatelnost s dimenzí** | Funguje pro nD, ale vyžaduje uspořádání | Trpí prokletím dimenzionality pro velké d |
| **Předpoklady** | Atributy musí být porovnatelné (≤, ≥) | Pouze existence metrické funkce d |
| **Výpočty** | Porovnání klíčů | Výpočty vzdálenosti (mohou být nákladné) |

#### Prokletí dimenzionality (Curse of Dimensionality)
- Při vysoké dimenzi se vzdálenosti koncentrují: max/min vzdálenost konverguje k 1
- Indexy ztrácejí efektivitu – nutno prohledat většinu databáze
- Řešení: redukce dimenzionality (PCA, random projections), aproximované hledání (ANN)
