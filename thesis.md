---
title: Számítógépes póker intelligens ágensekkel
author: Sőre András
bibliography: bibliography.bib
csl: iso690-numeric-en.csl
date: 2016
margin-left: 2.5cm
margin-right: 2.5cm
fontsize: 12pt
...


<!--
Feladatkiírás/Címoldal/Tartalomjegyzék/Nyilatkozat
Bevezetés                               1
A póker szabályai                       3
A számítógépes póker összefoglalása     3
Opponent Modeling                       10
Ágens megvalósítása
A felhasznált keretrendszer             4
Ágens architektúrája                    4
Ellenfélmodellezés megvalósítása        4
Gépi tanulás használata                 (2-3)
Featurek a játék állapotáról            (2-3)
Döntéshozás                             4
MCTS algoritmus                         (4)
Eredmények értékelése                   5
Irokdalom                                1
Függelék
-->

Kivonat/Abstract
================

<!-- 1 oldal -->

Bevezetés
=========
<!--
- kiírás elemzése
- előzmények
- motiváció
- megoldások
- saját megoldás
-->

<!-- 1 oldal -->
A számítógépes póker a mesterséges intelligencia egy régóta kutatott területe.
A játék tulajdonságai alkalmassá teszik arra, hogy számos algoritmust és
módszert lehessen vizsgálni ellenőrzött, reprodukálható körülmények között.

Azért választottam ezt a témát, mert régebben hobbiszinten én is pókereztem
internetes pókertermekben, így felkeltette az érdeklődésemet, hogy ezt a
játékot számítógép is végezheti, ráadásul sokszor hatékonyabban, mint profi
emberi játékosok.
Annak ellenére, hogy a témakörben már rengeteg kutatást csináltak, egy
pókerágens tervezése és fejlesztése még mindig egy összetett feladat, rengeteg
megoldandó problémával.

A terület egyik legnagyobb művelője a kanadai University of Albertán működő
Computer Poker Research Group (CRPG), akik már a kilencvenes évek közepétől
foglalkoznak a témával, és rengeteg publikációt adtak ki róla. A kutatáson
kívül versenyek is léteznek, ahol számítógépes pókerágensek mérhetik össze
tudásukat: a leghíresebb az Annual Computer Poker Competition, amelyet 2006-ban
tartottak először, és évente szervezik, különböző kategóriákban.

Az évek során számos ágenst fejlesztettek ki. A kezdeti próbálkozásokhoz képest
mára ott tartunk, hogy a CRPG "megoldotta" az általam is választott
pókerfajtát, azaz a legújabb játékosuk már gyakorlatilag tökéletesen játszik.
[@CepheusP8:online] Az általuk készített ágensek forrása nem nyílt, viszont a
tanulmányokból vázlatos képet kaphatunk a működésükről.

A saját munkám során a vizsgált pókerfajta, és a tesztkörnyezet kiválasztása
után olyan megoldásra törekedtem, ami felhasználja a gépi tanulást valamilyen
formában, valamint elég egyszerű ahhoz, hogy a félév során működőképes ágens
legyen az eredmény. Ennek megfelelően a korábban önálló labormunka keretében
tervezett ágens architektúráját használtam fel, ahol az ellenfél viselkedésének
modellezését egy neurális háló végzi, valamint az ágens döntéseit a viszonylag
egyszerű MCTS algoritmus segítségével állítom elő.

A dolgozat elején bemutatom a pókerjátékot, valamint általam választott
pókerfajtát, a Limit Texas Hold'emet, kitérve annak a számítógépes póker
szempontjából lényeges aspektusaira, utána magát a számítógépes pókert, mint
területet mutatom be, a fontosabb tulajdonságaira, megoldadó kérdésekre kitérve.

Külön szót ejtek még az ellenfél modellezésének kérdéseiről, valamint a
meglévő megoldásokról aszerint, hogy hogyan tudnak az egyes részproblémákra
választ adni.

Ezek után a saját ágensem architektúráját fogom ismertetni, a rendszerterven
kívül a megvalósítás részleteivel együtt, kitérve az ellenfélmodellezésre, és a
döntéshozásra is. <!-- todo: tesztek hol lesznek-->

Végül értékelem az eredményeket, és megvizsgálom hogy milyen lehetőségek vannak
a további fejlesztésekre.

## A póker szabályai
<!-- 3 oldal -->

# A számítógépes póker összefoglalása
<!-- 3 oldal -->

Requirements of a computer poker player [@billings1998opponent]

* Hand strength [@davidson2002opponent, page 21]
    - annak a valószínűsége, hogy a mi
      kezünk nyer.
    - RHS/HR - véletlenszerű kezek ellen
    - súlyozni lehet a számolást (ez lesz a súlytáblás módszer)

* Hand potential [@davidson2002opponent, page 22]
    - A valószínűsége, hogy a kezünk javulni fog (nem mond mértéket!)

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

architektúra [@billings2002challenge, p. 209]

action table - actionokok százalékos aránya

Cselekvés előrejelzésére: előrecsatolt MLP ellenfélmodell
[@davidson2000improved, page 3]

Selective sampling: szimuláció alapú megközelítés, viszont az MCTS-el
ellentétben nem súlyozza a játékfa csúcsait


# Ágens megvalósítása
<!-- 4 oldal -->

## A felhasznált keretrendszer
<!-- 4 oldal -->
- PokerAcademy előnyök:
  - Meglévő, bizonyított ágensek
  - GUI
  - Statisztikák
  - Ismeretség
  - Nehéz debugolás
  - Régi java

- OpenTestbed
  - Opensource
  - Debugolás könnyebb
  - Java újabb
  - Ágensek limitáltak

## Ágens architektúrája
<!-- 4 oldal -->
- Meglévő ágensek használata, portolás
- Kliens/szerver architektúra használata

## Ellenfélmodellezés megvalósítása
<!-- 4 oldal -->
Poki féle neurális háló, saját választott featurekkel. Kézrange becslése:
játszott játékok

### Featurek a játék állapotáról

A featurek egy része a Poki által használtakból került ki
[@davidson2002opponent, page 42].

Megoldás az ellenfelek cselekvéseinek kiszámítására:

A játékok adatait tároljuk, mégpedig úgy, hogy:
- játékosok nevét, zsetonok számát, szék számát külön (a zsetonok száma nem
  fontos, ha kellően sok kezdő zsetont feltételezünk)

- a játékban történt cselekvéseket, ami a fix tétes póker szabályai szerint:
  bedobás, megadás/check, emelés lehet

- a leosztott közös lapokat, illetve ha megismerjük, a játékosok privát lapjait

Adott játékonként a játékokat az ellenfélmodellező feldolgozza a következő módon:

- A játékok adataiból featurekat generál. Minden egyes cselekvésre kiszámolja őket.
  A bemenet a jelenlegi játék adatai, és a cselekvő játékosra lesznek érvényesek.
  A featurek jelenleg:

  - emelések száma (adott körre nézve)

  - pot odds: ez = megadandó_zsetonok / (megadandó_zsetonok + 1)

  - pozíció: a játékos asztalnál elfoglalt helyének "jósága". A legjobb értéket
    1-nek veszem, a legrosszabbat 0-nak, így könnyen felhasználható tanításhoz.

  - megadandó tétek száma: a tanítás miatt ez is 0-tól 1-ig terjed, ahol 1 a
    lehető legtöbb tét (4 darab)

  - befektetett-e a játékos pénzt (0 vagy 1)

  - aktív játékosok száma (akik még az adott körben játékban vannak)

  - jelenlegi kör száma: a 4 körnek megfelelően 0-tól 3-ig terjed, szintén 0 és
    1 közé normalizálva

  - leosztott lapok "szárazsága": ez azt mutatja meg, hogy sor, vagy flöss (5
    egymást követő érték, vagy 5 egyforma szín) milyen valószínűséggel
    alakulhat ki. A közös lapok között szereplő egymást követő értékek, vagy
    egyforma színű lapok jelentősen emelik a valószínűségeket.

  A legutolsó feature a leosztott lapokkal dolgozik. Ezeket máshogy is figyelembe
  lehet venni, például az adott ellenfél kézerősségének becslésénél, ami
  szintén lehetne feature.

#### Ellenfélspecifikus:

* becsült kézerősség

* becsült hand potential


A játékosok lapeloszlásának megbecsülése:

Erre a szimuláció során lenne szükség, amikor kiértékelnénk egy-egy szimulált
játékot. A legegyszerűbb megoldás azon játékok számolása, ahol az ellenfél
zsetont fektetett be (beszállt a játékba szándékosan), és az érték leosztása az
összes játék számával. Ebből, optimálisan játszó ellenfelet feltételezve,
következtethetünk a kezdő lapok erősségére.

### Gépi tanulás használata

A featurek egy neurális háló által betanításra kerülnek, ahol a kimenet a
végzett cselekvés a játék adott állapotában. A modelleket játékosszinten
lehet tárolni, viszont   hatékony megoldás lehet az is, hogy asztal/játékos
bontásban készülne egy-egy modell. Így a rendszer automatikusan alkalmazkodna
a különböző játékoshalmazokhoz. Hátránya persze, hogy minden halmazhoz újabb
betanítást igényelne.

- Előrecsatolt neurális háló

- scikit-learn


### Kísérletek

Kezdetben a tábla állapotát felhasználó feature nem készült. 1000 mintapontot
felhasználva, a Jagbotok játékából történt a teszt.

A pontosság: ~60%

TODO: megvizsgálni pontok számának variálásával.

Layer sizes       Score
-------------  --------
[5, 5]         0.62649
[7, 5]         0.63168
[15, 12]       0.649942
[20, 17]       0.678008
[15, 12, 10]   0.654171
[30, 25]       0.678585


Egy pontig növekedést érhetünk el a neuronok számának növelésével.

- Konfúziós mátrix [@davidson2000improved]: ez a metrika megmutatja, hogy a
  prediktorunk milyen típusú hibákat produkál. Az osztályozások eloszlása
  látható rajta az elvárt, és a tényleges válaszok kombinációira vetítve.

Az alábbi mátrixot kaptam teszteléskor (1800 tanítópont, 200 tesztpont, 1 adott
ellenfélre):

Act\\Pred      Fold    Call    Raise
----------  ------  ------  -------
Fold          0.24    0.02     0.00
Call          0.08    0.33     0.05
Raise         0.06    0.11     0.10

Látható, hogy az ellenfélmodell a bedobást és a megadást tudja jobb
valószínűséggel megbecsülni. Az emeléseket arányaiban többször osztályozta
félre a háló.

Több mátrixot is vizsgálva, egyértelmű volt, hogy a jelenlegi ellenfélmodell az
emeléseket nem azonosítja elég hatékonyan.

## Döntéshozás
<!-- 4 oldal -->

* Szimuláció alapú

* Formula alapú

Poki, Loki döntéshozása pl.

### MCTS algoritmus

### Optimizáció

- pszeudorandom műveletek a konstans futásidőhöz
- vmprof
- költséges műveletek
  -deepcopy
  -UTC algoritmus számítása
  -children dictionary
- gains: 30 sec -> 8 sec -> 3.5 sec

# Eredmények értékelése
<!-- 5 oldal -->
## Tesztkonfigurációk

 - Jagbot - statikus, kezdőknek való főleg
   4 db Jagbot, plusz az ágens, 150 db játék
