Szakdolgozat ütemterv
================================================================================

Október
--------------------------------------------------------------------------------


### 29 - 31

Az ütemterv készítése, közben az ágens fejlesztése. Kitűzött célok:

* Az ellenfélmodellezéshez szükséges fejlesztések: a szerver támogassa a játék
  felől érkező események feldolgozását, pl emelések, játékosok lapjai

* Néhány feature kiszámítása a játékról az előbbiek segítségével


November
--------------------------------------------------------------------------------


### 1 - 6

Ellenfélmodellezés kidolgozása. Cél: a tanulás működjön valamilyen egyszerű
formában

* Irodalomkutatás a témában, jegyzet készítése

* Ellenfélmodellező kidolgozása, libraryk keresése, beépítése

* Játékadatok gyűjtése, ellenfelek kiválasztása a teszteléshez


### 7 - 13

Ellenfélmodellezés finomítása. Cél: kísérletezés különböző beállításokkal,
eredmények összegzése

* Irodalomkutatás, jegyzet folytatása

* Különböző ellenfelek összeválogatása

* Ellenfélmodell különböző változatainak kipróbálása

* Eredmények összegzése

* Az ágens képes legyen létrehozni, és tárolni modelleket adott ellenfélhez


### 14 - 20

MCTS döntéshozó implementálása, hangolása

* Játékmodell befejezése: egy játék lejátszása működik, a játék kimenetelének
  kiértékelésével együtt.

* A modell illesztése az MCTS algoritmushoz: Dummy ellenfélmodell segítségével
  a rollout működjön


### 21 - 27

A döntéshozó és az ellenfélmodell integrálása

* A döntéshozó a játék megfelelő állapotaiban már az ellenfélmodellezőhöz
  forduljon

* Az MCTS algoritmus hangolása, tesztek elvégzése a kész ágenssel

* Eredmények rögzítése


### 28 - 30

Esetleges lemaradás behozása, hibák javítása.

* Teljesítmény optimizálása, ha van rá idő (ez az MCTS algoritmus futásidejét
  fogja érinteni főleg)


December
--------------------------------------------------------------------------------

### 1 - 9

Szakdolgozat szerkesztése, véglegesítése

* Eredmények összegzése, szükség esetén apróbb kísérletek elvégzése
