---
title: Számítógépes póker intelligens ágensekkel
author: Sőre András
bibliography: bibliography.bib
csl: iso690-numeric-en.csl
date: 2016
---

# Feladatkiírás/Címoldal/Tartalomjegyzék/Nyilatkozat

# Kivonat/Abstract

# Bevezetés
<!-- 1 oldal -->

## A póker szabályai
<!-- 3 oldal -->

# A számítógépes póker összefoglalása
<!-- 3 oldal -->

Requirements of a computer poker player [@billings1998opponent, page 2-4]

* Hand strength

* Hand potential

* Bluffing

* Unpredictability

## Opponent Modeling
<!-- 10 oldal -->

Célok: Kezek eloszlását, illetve ellenfél cselekvéseit megbecsülni.
[@billings2002challenge, page 2-4] Egy ilyen modellt prediktornak is hívnak.


### Fajtái

* Generikus/specifikus

* Multiprediktoros - szavazás alapján

* Játékelmélet-alapú

* Bayesi

#### Loki

Lapok eloszlása: súlyozott tábla a lapkombinációkról. [@billings1998opponent,
page 4-6] Kezdeti súlyok a megfigyelt kezek alapján.

Hand strength, hand potential, EHS alapján formula, [@davidson2002opponent, p.
30] hogy mi legyen a következő lépés

#### Poki

Cselekvés előrejelzésére: előrecsatolt MLP ellenfélmodell
[@davidson2000improved, page 3]

Selective sampling: szimuláció alapú megközelítés, viszont az MCTS-el
ellentétben nem súlyozza a játékfa csúcsait


# Ágens megvalósítása
<!-- 4 oldal -->

## A felhasznált keretrendszer
<!-- 4 oldal -->

## Ágens architektúrája
<!-- 4 oldal -->

## Ellenfélmodellezés megvalósítása
<!-- 4 oldal -->

### Gépi tanulás használata

### Featurek a játék állapotáról

A featurek egy része a Poki által használtakból került ki
[@davidson2002opponent, page 42].

Double:

* Pot odds - [@davidson2002opponent, page 27]

* Pozíció - (játékosok száma - (sorszám_nullától)) / játékosok száma

* becsült kézerősség

* becsült potenciál


Bool:

* Commited - aktuális játékos befektetett már pénzt

* Last action - bet vagy raise volt az előző lépése a játékosnak

* Stage - turn

* Stage - river

Int:

* Bets to call - aktuális játékosnak hány tétet kell megadnia

???:

* Board texture


## Döntéshozás
<!-- 4 oldal -->

* Szimuláció alapú

* Formula alapú

Poki, Loki döntéshozása pl.

### MCTS algoritmus

# Eredmények értékelése
<!-- 5 oldal -->
