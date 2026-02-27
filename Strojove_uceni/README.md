# Strojové učení (PV056)

> **Poznámka:** Pro absolventy předmětu od jara 2025 včetně.

## Popis okruhu

Základy strojového učení (supervizované, semi-supervizované a nesupervizované učení; operace klasifikace, regrese, detekce anomálií). Učení metrik (kontrastivní učení, triplet-loss učení). Vektorová/produktová kvantizace s využitím pro aproximované hledání. Principy křížově-modálního (cross-modal) učení (CLIP).

## Témata

### Základy strojového učení

#### Typy učení
- **Supervizované učení (Supervised Learning):** trénovací data obsahují vstupní vzory i správné výstupy (štítky)
- **Semi-supervizované učení (Semi-supervised Learning):** kombinace malého množství označených a velkého množství neoznačených dat; self-training, co-training, label propagation
- **Nesupervizované učení (Unsupervised Learning):** data bez štítků; cílem je odhalení skryté struktury

#### Operace strojového učení
- **Klasifikace:** přiřazení vstupu do jedné z předem definovaných kategorií; příklady: SVM, Random Forest, neuronové sítě; metriky: accuracy, F1, AUC-ROC
- **Regrese:** predikce spojité výstupní hodnoty; příklady: lineární regrese, SVR, gradientní boosting; metriky: MSE, MAE, R²
- **Detekce anomálií:** identifikace vzorků, které se výrazně liší od ostatních; metody: Isolation Forest, One-Class SVM, autoencodery, LOF (Local Outlier Factor)

### Učení metrik (Metric Learning)
- Motivace: naučit se reprezentační prostor, kde jsou si podobné vzorky blízko a nepodobné daleko
- Vzdálenostní funkce a jejich vlastnosti (metrika)

#### Kontrastivní učení (Contrastive Learning)
- Kontrastivní ztráta (contrastive loss): minimalizace vzdálenosti podobných párů, maximalizace vzdálenosti nepodobných párů
- Siamese sítě: sdílené váhy pro zpracování dvou vstupů
- SimCLR, MoCo: moderní kontrastivní metody pro samodohlížené učení (self-supervised learning)
- Pozitivní a negativní páry: strategie vzorkování (hard negative mining)

#### Triplet-loss učení
- Triplet: anchor (kotva), positive (podobný), negative (nepodobný)
- Triplet loss: L = max(0, d(a,p) - d(a,n) + margin)
- Online triplet mining: batch hard, batch semi-hard strategie
- Aplikace: rozpoznávání obličejů (FaceNet), ověřování mluvčích, vyhledávání obrazů

### Vektorová/produktová kvantizace

#### Vektorová kvantizace (VQ)
- Princip: mapování spojitého vektorového prostoru na diskrétní kódovou knihu (codebook)
- Algoritmus k-means jako základ VQ
- Distorze a minimalizace rekonstrukční chyby

#### Produktová kvantizace (PQ)
- Princip: rozdělení vektoru na M podvektorů, každý kvantizován nezávislou kódovou knihou
- Asymetrické distanční výpočty (ADC) a symetrické distanční výpočty (SDC)
- Efektivita paměti: komprese vektorů
- Výpočetní efektivita: předpočítané distanční tabulky

#### Aplikace pro aproximované hledání (ANN)
- Approximate Nearest Neighbor (ANN) search: definice a motivace pro vysokodimenzionální data
- IVFPQ (Inverted File with Product Quantization): kombinace invertovaného souboru a PQ
- FAISS (Facebook AI Similarity Search): implementace různých indexovacích metod
- Metriky kvality ANN: recall@k, kompromis rychlost/přesnost

### Principy křížově-modálního učení (Cross-modal Learning)

#### CLIP (Contrastive Language-Image Pre-training)
- Motivace: propojení vizuální a textové modality v jednom prostoru
- Architektura: vizuální encoder (ViT nebo ResNet) + textový encoder (Transformer)
- Trénování: kontrastivní ztráta na velkém datasetu párů (obrázek, text) z internetu
- Výsledná reprezentace: sdílený embedding prostor

#### Schopnosti a aplikace CLIP
- Zero-shot klasifikace obrázků: bez nutnosti přetrénování na cílové třídě
- Obraz-text retrieval: hledání obrázků pomocí textového dotazu a vice versa
- Generativní modely s CLIP (DALL-E, Stable Diffusion – CLIP jako guidance)
- Omezení CLIP: citlivost na formulaci promptu, bias v trénovacích datech

#### Obecné principy křížově-modálního učení
- Modality: obraz, text, zvuk, video, 3D data
- Alignment v latentním prostoru: zarovnání reprezentací různých modalit
- Fusion strategie: early fusion, late fusion, cross-attention
- Aplikace: vizuální odpovídání na otázky (VQA), popis obrázků (image captioning), multimodální vyhledávání
