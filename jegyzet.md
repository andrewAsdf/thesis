---
title: Számítógépes póker intelligens ágensekkel
author: Sőre András
bibliography: bibliography.bib
csl: iso690-numeric-en.csl
date: 2016
---

# Feladatkiírás/Címoldal/Tartalomjegyzék/Nyilatkozat
...

# Kivonat/Abstract
...

# Bevezetés
...


# A számítógépes póker összefoglalása

Requirements of a computer poker player [@billings1998opponent, page 2-4]
...

## Opponent Modeling

Célok: Kezek eloszlását, illetve ellenfél cselekvéseit megbecsülni

### Fajtái

* Generikus

* Specifikus


#### Loki

Lapok eloszlása: súlyozott tábla a lapkombinációkról. [@billings1998opponent,
page 4-6] Kezdeti súlyok a megfigyelt kezek alapján.

#### Poki

Cselekvés előrejelzésére: előrecsatolt MLP ellenfélmodell
[@davidson2000improved, page 3]


# Ágens megvalósítása

## Ellenfélmodellezés kidolgozása



### Featurek a játék állapotáról

A featurek egy része a Poki által használtakból került ki
[@davidson2002opponent, page 42].

* Board featurek - ?

* Pot odds - [@davidson2002opponent, page 27]

* Commited - aktuális játékos befektetett már pénzt

* Bets to call - aktuális játékosnak hány tétet kell megadnia

* Last action - bet vagy raise volt az előző lépése a játékosnak

* Stage - aktuális kör

* ...

## Döntéshozás

# Eredmények értékelése

