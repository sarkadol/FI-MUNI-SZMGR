# Metody umělé inteligence (IV126)

## Popis okruhu

Prohledávání stavového prostoru, lokální prohledávání a metaheuristiky s jedním řešením, populační metaheuristiky (evoluční algoritmy, inteligence hejna). Plánování, reprezentace problému, plánování se stavovým prostorem. Práce s neurčitostí, Bayesovské sítě, exaktní a aproximační odvozování, čas a neurčitost, teorie užitku, Markovský rozhodovací proces, iterace hodnot, iterace strategie. Robotika, plánování pohybu robota (konfigurační prostor, kombinatorické a pravděpodobnostní přístupy).

## Témata

### Prohledávání stavového prostoru
- Definice stavového prostoru (stavy, přechody, počáteční a cílový stav)
- Neinformované prohledávání: BFS, DFS, iterativní prohlubování (IDDFS), obousměrné prohledávání
- Informované prohledávání: hladové prohledávání, A*, IDA*
- Vlastnosti algoritmů: úplnost, optimálnost, časová a prostorová složitost

### Lokální prohledávání a metaheuristiky s jedním řešením
- Horolezecký algoritmus (hill climbing) a jeho varianty
- Simulované žíhání (simulated annealing)
- Tabu prohledávání (tabu search)
- Prohledávání s proměnlivým okolím (VNS)

### Populační metaheuristiky

#### Evoluční algoritmy
- Genetické algoritmy: reprezentace, selekce, křížení, mutace
- Genetické programování
- Evoluční strategie a evoluční programování

#### Inteligence hejna
- Mravenčí optimalizace (ACO)
- Optimalizace rojem částic (PSO)
- Včelí algoritmy

### Plánování

#### Reprezentace problému
- STRIPS a PDDL jazyk pro popis plánovacích problémů
- Stavy, akce, předpoklady a efekty

#### Plánování se stavovým prostorem
- Dopředné plánování (forward planning)
- Zpětné plánování (backward planning / regression planning)
- Heuristiky pro plánování
- Hierarchické plánování (HTN)

### Práce s neurčitostí

#### Bayesovské sítě
- Definice a struktura bayesovské sítě (DAG, podmíněné pravděpodobnosti)
- D-separation a podmíněná nezávislost
- Exaktní odvozování: eliminace proměnných, inference ve stromových sítích
- Aproximační odvozování: Monte Carlo metody, loopy belief propagation

#### Čas a neurčitost
- Skryté Markovovy modely (HMM)
- Kalmanův filtr
- Dynamické bayesovské sítě

#### Teorie užitku
- Preferenční relace a axiomy racionality
- Funkce užitku a rozhodování za neurčitosti
- Maximalizace očekávaného užitku (MEU)

#### Markovský rozhodovací proces (MDP)
- Definice MDP: stavy, akce, přechodové pravděpodobnosti, odměnová funkce
- Iterace hodnot (value iteration)
- Iterace strategie (policy iteration)
- Parciálně pozorovatelné MDP (POMDP) – úvod

### Robotika

#### Plánování pohybu robota
- Konfigurační prostor (C-space)
- Překážky v konfiguračním prostoru

#### Kombinatorické přístupy
- Rozklad buněk (cell decomposition)
- Silniční mapy (roadmaps): viditelnostní grafy, Voroného diagramy

#### Pravděpodobnostní přístupy
- Pravděpodobnostní silniční mapy (PRM)
- Rychle se rozrůstající náhodné stromy (RRT, RRT*)
