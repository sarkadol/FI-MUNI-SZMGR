# Neuronové sítě (PV021)

## Popis okruhu

Vícevrstvé sítě a jejich výrazové schopnosti. Učení neuronových sítí: Gradientní sestup, zpětná propagace, praktické otázky učení (příprava dat, inicializace vah, volba a adaptace hyperparametrů). Regularizace. Konvoluční sítě. Rekurentní sítě.

## Témata

### Vícevrstvé sítě a jejich výrazové schopnosti
- Biologická inspirace: biologický neuron vs. umělý neuron
- Perceptron: lineární separabilita, Rosenblattův algoritmus
- Aktivační funkce: sigmoid, tanh, ReLU, Leaky ReLU, ELU, softmax
- Vícevrstvé perceptrony (MLP): vstupní, skryté a výstupní vrstvy
- Universální aproximační teorém: schopnost MLP aproximovat libovolnou spojitou funkci
- Hloubka vs. šířka sítě: expresivita a efektivita

### Učení neuronových sítí

#### Gradientní sestup
- Funkce ztráty (loss function): MSE, cross-entropy, hinge loss
- Gradientní sestup (Gradient Descent): dávkový (batch), stochastický (SGD), mini-batch
- Problémy gradientního sestupu: sedlové body, lokální minima, špatné podmíněnosti

#### Zpětná propagace (Backpropagation)
- Řetízkové pravidlo (chain rule) a výpočetní grafy
- Algoritmus zpětné propagace: dopředný průchod, zpětný průchod
- Gradientní vanishing/exploding: příčiny a důsledky

#### Praktické otázky učení

##### Příprava dat
- Normalizace a standardizace vstupních dat
- Zpracování chybějících hodnot
- Augmentace dat (data augmentation)
- Rozdělení dat: trénovací, validační, testovací množina
- Cross-validace

##### Inicializace vah
- Náhodná inicializace: uniformní, normální rozdělení
- Xavier/Glorot inicializace
- He inicializace (pro ReLU)
- Důsledky špatné inicializace

##### Volba a adaptace hyperparametrů
- Rychlost učení (learning rate): konstantní, plánovaná, cyklická
- Adaptivní optimizátory: AdaGrad, RMSProp, Adam, AdamW
- Gradient clipping
- Momentum a Nesterovovo momentum
- Hyperparametrické ladění: grid search, random search, Bayesovská optimalizace

### Regularizace
- Přeučení (overfitting) a zobecnění (generalization)
- L1 regularizace (Lasso): řídké váhy
- L2 regularizace (Ridge/weight decay): penalizace velkých vah
- Dropout: principy, inverze dropout
- Batch normalizace (Batch Norm): princip, výhody
- Layer normalizace, Instance normalizace
- Early stopping
- Data augmentation jako regularizace

### Konvoluční sítě (CNN)

#### Základní stavební bloky
- Konvoluční vrstva: filtr (kernel), stride, padding, feature maps
- Pooling vrstva: max pooling, average pooling, global pooling
- Plně propojená vrstva (FC layer)

#### Architekturní vzory
- LeNet, AlexNet, VGG, GoogLeNet/Inception, ResNet, DenseNet
- Skip connections a residuální učení
- Depthwise separable convolutions (MobileNet)
- Transfer learning a fine-tuning předtrénovaných modelů

#### Aplikace CNN
- Klasifikace obrázků, detekce objektů (YOLO, Faster R-CNN)
- Sémantická segmentace (U-Net, FCN)

### Rekurentní sítě (RNN)

#### Základní rekurentní sítě
- Architektura RNN: zpracování sekvencí, skrytý stav
- Problém mizejícího gradientu v čase (BPTT – Backpropagation Through Time)

#### LSTM a GRU
- Long Short-Term Memory (LSTM): vstupní brána, zapomínací brána, výstupní brána, buněčný stav
- Gated Recurrent Unit (GRU): zjednodušená varianta LSTM
- Bidirectional RNN a stacked RNN

#### Aplikace rekurentních sítí
- Jazykové modely, strojový překlad, generování textu
- Klasifikace sekvencí, značkování sekvencí
- Seq2Seq modely a mechanismus pozornosti (attention)
- Transformer architektura: úvod a vztah k rekurentním sítím
