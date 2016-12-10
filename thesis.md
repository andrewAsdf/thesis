---
bibliography: bibliography.bib
csl: ieee.csl
fontsize: 11pt
margin-left:  3.5cm
margin-right: 3.5cm
margin-top: 3cm
margin-bottom: 3cm
linestretch:  1.25
lang: hu
documentclass: report
header-includes:
    - \usepackage{graphicx}
---


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


<!-- titlepage as inline LaTeX-->

\begin{titlepage}
\begin{center}
\includegraphics[width=60mm,keepaspectratio]{figures/BMElogo.png}\\
\vspace{0.3cm}
\textbf{Budapesti Műszaki és Gazdaságtudományi Egyetem}\\
\textmd{Villamosmérnöki és Informatikai Kar}\\
\textmd{Méréstechnikai és Információs Rendszerek tanszék}\\[5cm]

\vspace{0.4cm}
{\huge \bfseries Számítógépes póker intelligens ágensekkel}\\[0.8cm]
\vspace{0.5cm}
\textsc{\Large Szakdolgozat}\\[4cm]

\begin{tabular}{cc}
 \makebox[7cm]{\emph{Készítette}} & \makebox[7cm]{\emph{Konzulens}} \\
 \makebox[7cm]{Sőre András} & \makebox[7cm]{Dr. Dobrowiecki Tadeusz doc.}
\end{tabular}

\vfill
{\large \today}
\end{center}
\end{titlepage}

<!-- end titlepage-->


<!-- acknowledgement as inline LaTeX-->

\thispagestyle{empty}

\begin{center}
\large
\textsc{Hallgatói Nyilatkozat}\\
\end{center}

Alulírott \emph{Sőre András}, szigorló hallgató kijelentem, hogy ezt a
szakdolgozatot meg nem engedett segítség nélkül, saját magam készítettem, csak
a megadott forrásokat (szakirodalom, eszközök stb.) használtam fel. Minden
olyan részt, melyet szó szerint, vagy azonos értelemben, de átfogalmazva más
forrásból átvettem, egyértelmûen, a forrás megadásával megjelöltem.

Hozzájárulok, hogy a jelen munkám alapadatait (szerzõ(k), cím, angol és magyar
nyelvû tartalmi kivonat, készítés éve, konzulens(ek) neve) a BME VIK
nyilvánosan hozzáférhetõ elektronikus formában, a munka teljes szövegét pedig
az egyetem belsõ hálózatán keresztül (vagy autentikált felhasználók számára)
közzétegye. Kijelentem, hogy a benyújtott munka és annak elektronikus verziója
megegyezik. Dékáni engedéllyel titkosított diplomatervek esetén a dolgozat
szövege csak 3 év eltelte után válik hozzáférhetõvé.

\begin{flushleft}
\vspace*{1cm}
Budapest, \today
\end{flushleft}

\begin{flushright}
 \vspace*{1cm}
 \makebox[7cm]{\rule{6cm}{.4pt}}\\
 \makebox[7cm]{\emph{Sőre András}}\\
 \makebox[7cm]{hallgató}
\end{flushright}

\vfill
\setcounter{page}{0}
\newpage

<!-- end of acknowledgement-->


\tableofcontents


Kivonat/Abstract {-}
================




Bevezetés
=========

<!-- 1 oldal -->
A számítógépes póker a mesterséges intelligencia egy régóta kutatott területe.
A játék tulajdonságai alkalmassá teszik arra, hogy számos algoritmust és
módszert lehessen vizsgálni ellenőrzött, reprodukálható körülmények között.

Azért választottam ezt a témát, mert régebben hobbiszinten én is pókereztem
internetes pókertermekben, így felkeltette az érdeklődésemet, hogy ezt a
játékot számítógép is végezheti, ráadásul sokszor hatékonyabban, mint profi
emberi játékosok.
Annak ellenére, hogy a témakörben már rengeteg kutatást végeztek, egy
pókerágens tervezése és fejlesztése még mindig egy összetett feladat, rengeteg
megoldandó problémával.

A terület egyik legnagyobb művelője a kanadai University of Albertán működő
Computer Poker Research Group (CRPG), akik már a kilencvenes évek közepétől
foglalkoznak a témával, és rengeteg eredményt publikáltak. A kutatáson
kívül versenyeket is szerveznek, ahol számítógépes pókerágensek mérhetik össze
tudásukat: a leghíresebb az Annual Computer Poker Competition, amelyet 2006-ban
tartottak először, és évente szervezik, különböző kategóriákban.

Az évek során számos ágenst fejlesztettek ki. A kezdeti próbálkozásokhoz képest
mára ott tartunk, hogy a CRPG "megoldotta" az általam is választott
pókerfajtát, azaz a legújabb játékosuk már gyakorlatilag tökéletesen játszik
[@CepheusP8:online]. Az általuk készített ágensek forrása nem nyílt, viszont
vázlatos képet kaphatunk a működésükről.

A saját munkám során a vizsgált pókerfajta, és a tesztkörnyezet kiválasztása
után olyan megoldásra törekedtem, ami felhasználja a gépi tanulást valamilyen
formában, valamint elég egyszerű ahhoz, hogy a félév során működőképes ágens
legyen az eredmény. Ennek megfelelően a korábban önálló labormunka keretében
tervezett ágens architektúráját használtam fel, ahol az ellenfél viselkedésének
modellezését egy neurális háló végzi, valamint az ágens döntéseit a viszonylag
egyszerű MCTS algoritmus segítségével állítom elő.

A dolgozat elején bemutatom a pókerjátékot, valamint az általam választott
pókerfajtát, a Limit Texas hold'emet, kitérve annak a számítógépes póker
szempontjából lényeges aspektusaira. Utána magát a számítógépes pókert, mint
területet mutatom be, a fontosabb tulajdonságaira, megoldadó kérdésekre kitérve.
Külön szót ejtek még az ellenfél modellezésének kérdéseiről, valamint a
meglévő megoldásokról aszerint, hogy hogyan tudnak az egyes részproblémákra
választ adni.

Ezek után a saját ágensem architektúráját fogom ismertetni, a rendszerterven
kívül a megvalósítás részleteivel együtt, kitérve az ellenfélmodellezésre és a
döntéshozásra is. <!-- todo: tesztek hol lesznek-->

Végül értékelem az eredményeket, és megvizsgálom hogy milyen lehetőségek vannak
a további fejlesztésekre.

A pókerjáték
============

A pókernek számos fajtája létezik[@PokerWik95:online]. Ezek elég különbözőek
lehetnek, de a közös vonás mindegyikben, hogy a játékosok egy kör során
valahány saját lapot kapnak, és a licitkör(ök) után a legjobb lapkombinációval
(kézzel) rendelkező játékos viszi el a tétet. A kezek a játékosok saját
lapjaiból, illetve a leosztott közös lapokból állhatnak, ha van olyan.

A játékfajták több mindenben különbözhetnek, a teljesség igénye nélkül:

  - Licitkörök száma
  - Licitek határa
  - Játékosok saját lapjainak száma, illetve a mindenki által látható közös
    lapok száma
  - Lapok cseréjének lehetősége, ezek száma
  - Legjobb vagy legrosszabb kéz nyer-e, vagy mindkettő


A Texas hold'em
---------------

A számítógépes pókerben a leggyakrabban vizsgált pókerfajta a Texas hold'em. A
játék e változata nagy népszerűségre tett szert a 2000-es évek elején, a
televíziónak és az internetnek köszönhetően. Ekkoriban terjedtek el az
internetes pókertermek is, ahol főleg ezt a változatot játszották.

A hagyományos 5 lapos pókerhez képest a játék elején minden játékos 2 saját
lapot kap. Az osztó *(button)* utáni játékos *(kisvak/small blind)* az alaptét
felét teszi meg, és az utána következő játékos *(nagyvak/big blind)* az alaptétet
teszi meg kötelezően, így ösztönözve a további liciteket.

A Texas hold'em alfajai különbözhetnek a licitekre vonatkozó megkötésekben. A
legnépszerűbb fajta, amit emberi játékosok játszanak, az a *no-limit*, azaz
limit nélküli hold'em, ahol a tét minimuma van csak megszabva, ami az adott
nagyvak, maximuma pedig a játékos összes zsetonja. További fajták még a *pot
limit* és a *fixed limit* hold'em. Az előbbinél a tét maximuma a jelenlegi
tétek összessége, a *pot* lehet, míg az utóbbinál a tét egy fix összeg,
pontosan egyenlő a nagyvakkal (ami a nevezéktant illeti; a játékosokat és a
téteket is vakoknak hívják).

Az első licitkör után leosztásra kerülnek a közös lapok. Ezt *flop* nak hívják,
és 3 közös lapot jelent. Az újabb licitelés után rendre leosztásra kerülnek a
*turn* és *river* lapok, ezek 1-1 lapot jelentenek körönként.
Az utolsó licitkör után a játékosok kezei a leosztott lapokból és a saját,
privát lapjaikból állnak. Az így rendelkezésre álló 7 lapból alkotott legjobb 5
lapos kombináció fog számítani. A lapok mutatását hívják _showdownnak_.

A játékot játszhatják *cash game* vagy verseny formában. Az előbbinél a játékos
célja az asztalhoz leülve minél több zsetont megszerezni valahány kör során. A
játékos szabadon hozhat további zsetonokat az asztalhoz. A versenyek során viszont
a versenyző játékosok fix mennyiségű zsetont kapnak, és kieséssel távoznak az
asztaltól.

Én cash game-t használtam az ágens tesztkörnyezetének, és a munka során
feltettem továbbá, hogy minden játékosnak végtelen sok zsetonja van. Így egy
lejátszott játék eredménye közvetlenül mérhető volt.

Limites Texas hold'em
---------------------

Az általam választott játék a limites Texas hold'em lett. Mivel no-limites és a
pot-limites változattal ellentétben az emelés összege mindig ugyanaz, ha
sorrakerül egy játékos, 4 féle cselekvést végezhet alapvetően:

- Bedobja a lapjait *(fold)*
- Jelzi, hogy nem szeretne emelni, de nem kell tétet megadnia: *check*el
- Megadja az előtte levő emelést, ha volt *(call)*
- Emel az előre meghatározott fix összeggel *(bet/raise)*

A többi fajtában az emelés összegét variálhatja a játékos. Ez megnehezíti az
ágensek készítését a játékhoz, mivel a lehetséges lépések száma sokkal nagyobb.


Hold'em vs. 5 lapos póker
-------------------------

A fentiek alapján látható, hogy ennél a pókerfajtánál a játékosnak sokkal több
információ áll rendelkezésére a döntéshez, mint a klasszikus 5 lapos pókernél,
ahol nincsenek közös lapok, és így nem tudunk következtetni az ellenfél
esélyeire a miénkkel szemben. Így a Texas hold'em komplexebb stratégiák
alkalmazását engedi meg a másik nevezett pókerfajtához képest. Több mű is
született, ami a Texas hold'em stratégiáit tárgyalja. Ezek könnyen alkalmazható
heurisztikákat adnak a játékosok kezébe [@sklansky1999theory]. Az
emberi játékosok számára készített stratégiákat felhasználják szakértői
rendszerek készítésénél is, erről majd a későbbi fejezetekben lesz szó.

A póker, mint kutatási terület
------------------------------
<!-- TODO: szövegezni-->
<!-- TODO: póker vs. sakk stb?-->
<!--
### Játékfa naiv megoldása:
  - Ki kell teríteni, és a csúcsokat színezni
  - A végén ki nyert: egy játékos, vagy döntetlen
  - Egy szinten levő csúcsokat aszerint, hogy ki játszik: ekkor ha van
    ellentétes, vagy döntetlen szín alatta, akkor olyanra, egyébként a játékos
    színére
  - A gyökér színe dönti el a játék kimenetelét

### Minmax algoritmus szekvenciális játékokhoz:
  - Kiértékelőfüggvény a játék állapotához (heurisztikus is lehet, ha nagy a fa)
  - Az ellenfél az érték minimalizálására törekszik, a játékos a maximalizálására
  - Az kiválasztott érték felfele propagálódik
-->

A játékoknak általában tisztán definiált szabályaik, és céljaik vannak. A
legtöbb játéknak, ahol a játékos képességei számítanak a szerencsével
ellentétben, egyszerű a játékmenete, viszont komplex stratégiák kidolgozását
teszik lehetővé. Eredmények tekintetében ezek miatt könnyű összemérni két
játékos teljesítményét is, így összehasonlíthatóvá válnak a különböző gépi
játékosok. 

A póker, mint játék, több mozzanatában különbözik a többi gyakran kutatott
játéktól (pl. sakk)[@billings1995poker].

### Nem-tökéletes információ
<!-- 
- imperfect: nem ismert minden esemény, ami történik a játékban (pl. _inicializáció_)
- incomplete: a játék **szerkezete** nem ismert, pl hasznosságfüggvények, célok
-->

Nem-tökéletes információjú játéknak azt nevezik, amikor a játékosok nem tudnak
mindig a játékban történt eseményekről[@Perfecti13Online]. A ha a póker
játékfájába belevesszük a lapok leosztását, mint eseményeket, akkor mondhatjuk,
hogy mindegyik játékos csak a saját lapjainak leosztásáról értesül.

### Nemdeterminisztikus kimenetel

Általánosságban mondhatjuk, hogy egy játékhoz annál nehezebb játékost készíteni
a mesterséges intelligencia segítségével, minél nagyobb a lehetséges
játékállapotok száma, és ezzel a játékfa mérete.

A nemdeterminisztikus elemek egy játékban úgy növelik a játékfát, hogy közben a
hagyományos keresőalgoritmusok keresési terét nehéz lesz csökkenteni, így az
alkalmazhatóságuk is kérdéses lesz ilyen esetekben[@billings1995poker].




A számítógépes póker
====================

A játékot játszó hatékony mesterséges intelligenciának az alábbi
tulajdonságokkal kell rendelkeznie a sikeres játék érdekében [@davidson2002opponent, ch. 3]:


* Kéz kiértékelése: A lapjaink jelenlegi erősségét a közös lapok, valamint az
  ellenfél lehetséges lapjai határozzák meg. Adott szituációban a legerősebb
  kézre van szükségünk, ezért ennek a becslése fontos feladat egy ágens
  számára.

* Kiszámíthatatlanság: A cselekvéseinknek nem szabad elárulnia a kezünk
  erősségét. Játék közben *blöffölnünk*, *slow-playt*^[A blöff egy formája,
  ahol kevesebbet emelünk, mint ami a lapunk alapján várható lenne], vagy
  *check-raise-t*^[Egy emelési stratégia, amikor is egy check után az ellenfél
  emel, majd mi visszaemelünk] kell használnunk. Fontos, hogy ne ugyanazt a
  stratégiát használjuk mindig egy adott szituációban, hogy elrejtsük a
  kézerősségünket.

* Ellenfélmodellezés: Meg kell értenünk, hogy hogyan játszik egy ellenfél, hogy
  kihasználjuk a gyengeségeit, és védekezni is tudjunk ellene. Ehhez tudnunk
  kell értelmezni a cselekvéseit, hogy következtethessünk a kézerősségére, és a
  további cselekvéseire.

Ezek a tulajdonságok iránymutatóként szolgálnak egy pókerágens tervezésénél.
Azt viszont már nekünk kell eldöntenünk, hogy ezeket az információkat pontosan
hogyan állítjuk elő, és használjuk fel. Nem is biztos, hogy egy ágens
működése során explicit módon megjelennek ezek a fogalmak, viszont mindeképp
számolni kell velük.


Opponent Modelling
==================

<!-- 10 oldal, todo kifejteni -->

Ahogy korábban elhangzott, az ellenfélmodellezés két fő célja az ellenfél
cselekvéseinek, és kézerősségének megbecslése.

Az ellenfélmodellezést megnehezítő tényezők [@davidson2002opponent, ch. 4]:

- Bizonytalanság: A nagy számú ismeretlen kártya megnehezíti a felhasználható
  információk kinyerését. Minden játék teljsen más lehet, mint az előző, ezért
  sok megfigyelésre van szükségünk, mielőtt azonosítunk egy tendenciát.

- Hiányzó információ: Csak akkor látjuk az ellenfél lapjait, ha eljut a
  showdownig, ráadásul ezek a lapok csak a megjátszott kezeinek csak egy
  részhalmaza.

- Ismeretlen változók: Egy játékos döntéseit ismeretlen számú változó
  befolyásolja, és a játékosok különböző tényezőket vehetnek figyelembe játék
  közben. Például amíg az asztalnál elfoglalt pozíció fontos lehet egy
  játékosnak, addig egy másiknak kevésbé. A játékot meghatározó tényezők
  megválasztása nem feltétlenül racionális módon történik, például amikor egy
  játékos csak a pikk színű lapokat preferálja a játékához.
  Az ellenfél modellezése közben viszont bármilyen korreláció kiszűrése előnyös
  lehet.

- Lassú alkalmazkodás: Az emberi játékosok megérzés alapján, és tapasztalat alapján
  gyorsan ki tudják találni az ellenfeleik játékát. Még a saját játékukat is
  megváltoztathatják, hogy verifikáljanak egy-egy ilyen elméletet. Ezzel
  szemben a gépi tanuló módszereknek sok megfigyelésre van szülségük, és
  lassabban alkalmazkodnak.

- A modellezés több szintje: Az ellenfél modellezése nem csak a közvetlen döntéseire
  vonatkozhat, hanem arra is, hogy ő milyen modellt alkot rólunk, vagy hogy
  milyen modellt alkot arról, hogy mi milyen modellt alkotunk róla, stb.

- Változó ellenfél: Egy jó ellenfél változtatni fogja a játékát idővel, így a
  róla alkotott modellünk elavulhat addigra, mire már elég megfigyeléssel
  rendelkezünk a játékáról.


A predikció módszerei
---------------------

### Szakértői rendszerek

Kiindulásnak jó alapot nyújthat, ha az ellenfél modellezését a saját
stratégiánkkal, vagy  valamilyen szabályok halmazával végezzük (akár emberi
játékosoknak készült heurisztikákból). Ekkor feltesszük, hogy az ellenfél
valamennyire racionálisan játszik. Ez egyfajta _generikus ellenfélmodell_, ami
azt takarja, hogy nem vesszük figyelembe az adot ellenfél sajátosságait, hanem
minden ellenfélre ugyanazt a modellt alkalmazzuk.

### Statisztika

Egy másik nyilvánvaló módszer, ha figyeljük, hogy egyes cselekvéseket milyen
időközönként hajt végre az ellenfél. Például ha 40%-ban emel rögtön a flop
után, akkor feltételezhetjük, hogy azt a kapott lapjainak legjobb 40%-ával
teszi.

Ha az adott ellenfél korábbi játékát akár ilyen módon figyelembe vesszük, azt
már _specifikus ellenfélmodellezésnek_ hívjuk. Nyilvánvaló, hogy a specifikus
ellenfélmodellek jobban teljesítenek a generikusaknál.

### Neurális hálók

...


Néhány ágens ellenfélmodellezése
--------------------------------

### Loki

Lapok eloszlása: súlyozott tábla a lapkombinációkról [@billings1998opponent,
page 4-6]. Kezdeti súlyok a megfigyelt kezek alapján.

<!--
* Hand strength
    - annak a valószínűsége, hogy a mi
      kezünk nyer.
    - RHS/HR - véletlenszerű kezek ellen
    - súlyozni lehet a számolást (ez lesz a súlytáblás módszer)

* Hand potential
    - A valószínűsége, hogy a kezünk javulni fog (nem mond mértéket!)
-->

Hand strength, hand potential, EHS alapján formula, [@davidson2002opponent, p.
30] hogy mi legyen a következő lépés

### Poki

architektúra [@billings2002challenge, p. 209]

action table - actionokok százalékos aránya

Cselekvés előrejelzésére: előrecsatolt MLP ellenfélmodell
[@davidson2000improved, page 3]

Selective sampling: szimuláció alapú megközelítés, viszont az MCTS-el
ellentétben nem súlyozza a játékfa csúcsait



Ágens megvalósítása
===================

<!-- 4 oldal -->

A felhasznált keretrendszer
---------------------------

A pókerfajta kiválasztása után a következő lépés a keretrendszer kiválasztása
volt. Fő szempont volt, hogy a keretrendszer nyújtson már előre kész
ellenfeleket, amik ellen lehet tesztelni a kész játékost. Az önálló labor alatt
használt PokerAcademy megfelel ennek a követelménynek, mivel az University of
Alberta-i CRPG tagjai is részt vettek a fejlesztésében, és mellékeltét többek
között a Poki egyik változatát.

A keretrendszer Java-t használ, és Windows alatt fut. A hozzá készített
ágenseket (botok) plugin formájában kell mellékelni, és az erre a célra
készített Meerkat API-t^[Ez az API botok írását teszi lehetővé a játékhoz. A
benne levő Player interfész implementálását kell elvégezni.
(http://www.poker-academy.com/community.php) ] kell implementálniuk. Ez a
fejlesztést körülményessé teszi, főleg, ha az elkészített ágenst gyakran újra
szeretnénk fordítani.

A fejlesztést könnyítendő, készítettem egy dummy ágenst, ami egyfajta
kliensként funkcionál egy külsőleg megírt szerverhez, ami a tényleges logikát
tartalmazza a játékoshoz. Ezt a játék által használt 1.5-ös JRE miatt könnyen
meg lehetett valósítani. Az elkészült kliens http-n keresztül küld, és fogad
XML üzeneteket, egy egyszerű API szerint. Az ágenst már erre alapozva
fejlesztettem, Pythont használva, webszervernek pedig Flask-et. Így a játékot
újra se kellett indítani minden alkalommal, amikor valami módosítást végeztem
az ágensen, mivel a dummy kliens ugyanúgy futott rajta.

<!--
- PokerAcademy előnyök:
  - Meglévő, bizonyított ágensek
  - GUI
  - Statisztikák
  - Ismeretség
  - Nehéz debugolás
  - Régi java

egyéb lehetőségek:

- OpenTestbed
  - Opensource
  - Debugolás könnyebb
  - Java újabb
  - Ágensek limitáltak

- Meglévő ágensek használata, portolás
- Kliens/szerver architektúra használata
 -->

Ágens architektúrája
--------------------

<!-- 4 oldal -->
- Architekturális vázlat
- Komponensek szerepe röviden


Ellenfélmodellezés megvalósítása
--------------------------------

<!-- 4 oldal -->
Poki féle neurális háló, saját választott featurekkel. Kézrange becslése:
játszott játékok


### Gépi tanulás használata

<!--
- neurális hálók intro
- on-line tanulás biztosítása
-->

A featurek egy neurális háló által betanításra kerülnek, ahol a kimenet a
végzett cselekvés a játék adott állapotában. A modelleket játékosszinten
lehet tárolni, viszont   hatékony megoldás lehet az is, hogy asztal/játékos
bontásban készülne egy-egy modell. Így a rendszer automatikusan alkalmazkodna
a különböző játékoshalmazokhoz. Hátránya persze, hogy minden halmazhoz újabb
betanítást igényelne.

- Előrecsatolt neurális háló

- scikit-learn


### Játékállapot jellegfüggvényei
<!-- jellemzők?? -->

Az előbbieknek megfelelően az ellenfél cselekvéseinek előrejelzéséhez keresni
kellett megfelelő jellemzőket, amiket fel lehet használni.  A jellegfüggvények
(feature-ök) egy része a Poki által használtakból került ki
[@davidson2002opponent, page 42].

Megoldás az ellenfelek cselekvéseinek kiszámítására:

A játékok adatait tároljuk, mégpedig úgy, hogy:
- játékosok nevét, zsetonok számát, szék számát külön (a zsetonok száma nem
  fontos, ha kellően sok kezdő zsetont feltételezünk)

- a játékban történt cselekvéseket, ami a fix tétes póker szabályai szerint:
  bedobás, megadás/check, emelés lehet

- a leosztott közös lapokat, illetve ha megismerjük, a játékosok privát lapjait

Adott játékonként a játékokat az ellenfélmodellező feldolgozza a következő módon:

- A játékok adataiból jellegfüggvényeket generál. Minden egyes cselekvésre
  kiszámolja őket. A bemenet a jelenlegi játék adatai, és a cselekvő játékosra
  lesznek érvényesek.  A felhasznált jellegfüggvények:

  - emelések száma (adott körre nézve)

  - pot odds: $megadandó\_zsetonok / (megadandó\_zsetonok + pot)$

  - pozíció: a játékos asztalnál elfoglalt helyének "jósága". 0-tól 1-ig
    terjed, ahol 1 a buttonhoz legközelebbi ülőhely, a 0 pedig a legtávolabbi

  - megadandó tétek száma: a tanítás miatt ez is 0-tól 1-ig terjed, ahol 1 a
    lehető legtöbb tét (4 darab)

  - befektetett-e a játékos pénzt (0 vagy 1)

  - aktív játékosok száma (akik még az adott körben játékban vannak)

  - jelenlegi kör száma: a kör sorszáma, szintén 0 és 1 közé normalizálva, azaz
    $n / 4$, ahol $n$ a jelenlegi kör száma 0-tól 3-ig
<!--
  - leosztott lapok "szárazsága": ez azt mutatja meg, hogy sor, vagy flöss (5
    egymást követő érték, vagy 5 egyforma szín) milyen valószínűséggel
    alakulhat ki. A közös lapok között szereplő egymást követő értékek, vagy
    egyforma színű lapok jelentősen emelik a valószínűségeket.

  A legutolsó jellegfüggvény a leosztott lapokkal dolgozik. Ezeket máshogy is figyelembe
  lehet venni, például az adott ellenfél kézerősségének becslésénél, ami
  szintén lehetne feature.
-->

### Ellenfél kézerőssége:

<!-- todo: bővebben -->
A játékosok lapeloszlásána:

Erre a szimuláció során lenne szükség, amikor kiértékelnénk egy-egy szimulált
játékot. A legegyszerűbb megoldás azon játékok számolása, ahol az ellenfél
zsetont fektetett be (beszállt a játékba szándékosan), és az érték leosztása az
összes játék számával. Ebből, optimálisan játszó ellenfelet feltételezve,
következtethetünk a kezdő lapok erősségére.

Az ágens nem vizsgál explicit módon kézerősséget, ezt a szimuláció során a
neurális háló, és a végén a kiértékelés adja.


### Kísérletek

Kezdetben a tábla állapotát felhasználó feature nem készült. 1000 mintapontot
felhasználva, a Jagbotok játékából történt a teszt.

A pontosság: ~60%

TODO: megvizsgálni pontok számának variálásával.

Layer sizes   |     Score
------------- |  --------
[5, 5]        |  0.62649
[7, 5]        |  0.63168
[15, 12]      |  0.649942
[20, 17]      |  0.678008
[15, 12, 10]  |  0.654171
[30, 25]      |  0.678585


Egy pontig növekedést érhetünk el a neuronok számának növelésével.

- Konfúziós mátrix [@davidson2000improved]: ez a metrika megmutatja, hogy a
  prediktorunk milyen típusú hibákat produkál. Az osztályozások eloszlása
  látható rajta az elvárt, és a tényleges válaszok kombinációira vetítve.

Az alábbi mátrixot kaptam teszteléskor (1800 tanítópont, 200 tesztpont, 1 adott
ellenfélre):

Act\\Pred  |   Fold |   Call |   Raise
---------- | ------ | ------ | -------
Fold       |   0.24 |   0.02 |    0.00
Call       |   0.08 |   0.33 |    0.05
Raise      |   0.06 |   0.11 |    0.10

Látható, hogy az ellenfélmodell a bedobást és a megadást tudja jobb
valószínűséggel megbecsülni. Az emeléseket arányaiban többször osztályozta
félre a háló.

Több mátrixot is vizsgálva, egyértelmű volt, hogy a jelenlegi ellenfélmodell az
emeléseket nem azonosítja elég hatékonyan.


Döntéshozás
===========

Minden mesterséges pókerjátékosnak, még a legegyszerűbbnek is, a rendelkezésre
álló információk alapján valamilyen döntések sorozatát kell meghoznia egy játék
megnyeréséhez. A döntéshozás módja szerint több féle ágenst lehet készíteni:

- Szakértői rendszer

- Analitikus

- Szimuláció alapű

<!--
Stratégiák bemutatása?

Poki, Loki döntéshozása pl.

-->

MCTS algoritmus
---------------

- A Monte Carlo algoritmusokről
- A Monte Carlo fakeresésről

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


Tesztkonfigurációk
------------------

<!-- Todo -->

A teszteléshez használt konfigurációkat a Poker Academy-vel könnyen össze
lehetett állítani, majd menteni.

### Jagbotok

A Jagbot egy szakértői rendszer alapú ágens. Teljesen statikus módon játszik,
ezért alkalmas arra, hogy a könnyű ellenfél szerepét betöltse.

A tesztkonfiguráció 4 darab Jagbotból, és az ágensünkből állt. Ezzel 2000 darab
játékot játszattam, amit fel tudtam használni a tanítás teszteléséhez.
