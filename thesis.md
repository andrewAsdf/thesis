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
babel-lang: magyar
documentclass: report
figPrefix:
header-includes:
    - \usepackage{graphicx}
    - \usepackage[style=german]{csquotes}
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


\setcounter{tocdepth}{1}
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
esélyeire a miénkkel szemben. Ezért a Texas hold'em komplexebb stratégiák
alkalmazását engedi meg a másik nevezett pókerfajtához képest. Több könyv is
íródott, ami a Texas hold'em stratégiáit tárgyalja. Ezek könnyen alkalmazható
heurisztikákat adnak a játékosok kezébe [@sklansky1999theory]. Az
emberi játékosok számára készített stratégiákat felhasználják szakértői
rendszerek készítésénél is, erről majd a későbbi fejezetekben lesz szó.

A póker, mint kutatási terület
------------------------------
<!-- TODO: szövegezni-->
<!-- TODO: póker vs. sakk stb?-->

A játékoknak általában tisztán definiált szabályaik, és céljaik vannak. A
legtöbb játéknak, ahol a játékos képességei számítanak a szerencsével
ellentétben, egyszerű a játékmenete, viszont komplex stratégiák kidolgozását
teszik lehetővé. Eredmények tekintetében ezek miatt könnyű összemérni két
játékos teljesítményét is, így összehasonlíthatóvá válnak a különböző gépi
játékosok.

A póker, mint játék, több mozzanatában különbözik a többi gyakran kutatott
játéktól (pl. sakk).

### Nem-tökéletes információ
<!--
- imperfect: nem ismert minden esemény, ami történik a játékban (pl. _inicializáció_)
- incomplete: a játék **szerkezete** nem ismert, pl hasznosságfüggvények, célok
-->

Nem-tökéletes információjú játéknak azt nevezik, amikor a játékosok nem tudnak
mindig a játékban történt eseményekről[@Perfecti13:online]. A ha a póker
játékfájába^[A játékfa egy irányított gráf, ahol a csúcsok a játék állapotai,
és az élek a játékosok lépései] belevesszük a lapok leosztását, mint
eseményeket, akkor mondhatjuk, hogy mindegyik játékos csak a saját lapjainak
leosztásáról értesül.

A pókerben a hiányzó információk szerepe nagyon fontos, ellentétben pl a
Scrabble-vel, ahol az ellenfél betűit nem ismerjük, viszont a gyakorlatban nem
befolyásolja nagyban a játékosok stratégiáit[@billings1995computer].


### Nemdeterminisztikus kimenetel

Általánosságban mondhatjuk, hogy egy játékhoz annál nehezebb játékost készíteni
a mesterséges intelligencia segítségével, minél nagyobb a lehetséges
játékállapotok száma, és ezzel a játékfa mérete.

A nemdeterminisztikus elemek egy játékban úgy növelik a játékfát, hogy közben a
hagyományos keresőalgoritmusok keresési terét nehéz lesz csökkenteni, így az
alkalmazhatóságuk is kérdéses lesz ilyen esetekben[@billings1995computer].


A számítógépes póker
====================

A játékot játszó hatékony mesterséges intelligenciának az alábbi
tulajdonságokkal kell rendelkeznie a sikeres játék érdekében [@davidson2002opponent, ch. 3]:


* Kéz értékelése: A lapjaink jelenlegi erősségét a közös lapok, valamint az
  ellenfél lehetséges lapjai határozzák meg. A játékban a legerősebb kézre van
  szükségünk a győzelemhez, ezért ennek a becslése fontos feladat egy ágens
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
  további cselekvéseire. A kézerősség becslése nagyban függ attól, hogy hogyan
  tudjuk az ellenfél lapjainak eloszlását megbecsülni, azaz hogy milyen
  lapokkal hajlamos játszani.

Ezek a tulajdonságok iránymutatóként szolgálnak egy pókerágens tervezésénél.
Azt viszont már nekünk kell eldöntenünk, hogy ezeket az információkat pontosan
hogyan állítjuk elő, és használjuk fel. Nem is biztos, hogy egy ágens
működése során explicit módon megjelennek ezek a fogalmak, viszont mindeképp
számolni kell velük.

Kéz értékelése
--------------

A kezek értékeléséhez több heurisztika, és egyszerűsítés is rendelkezésünkre
áll [@billings1998poker]. Ezeknek még a későbbi ágensek bemutatása során is
szerepük lesz.

### Kezdőkezek csoportosítása

A kezdőkezek a Texas hold'emben 2 laposak, de ezeket 169 ekvivalens osztályra
tudjuk csoportosítani, a következő módon:

$$13 * pár + \binom{13}{2} * suited + \binom{13}{2} * offsuit$$

Itt a _suited_ azt jelenti, hogy a két lap egy színű, az _offsuit_ pedig azt,
hogy eltérő. Erősség szempontjából a konkrét színek nem számítanak, mivel az
elején még nem ismerjük a közös lapokat.

A kezdőkezek rangsorolását David Slansky is elvégzi tapasztalati alapon a
Theory of Poker című könyvében [@sklansky1999theory], viszont kísérleti úton is
előállítottak egy ilyen rangsort: a kézosztályok mindegyikével 1.000.000 játékot
játszottak véletlenszerű ellenfelek ellen [@billings1998poker]. Az eredmény egy
rangsor lett, ami korrelál a könyvben megadottal. Az ászpár nyert a
legtöbbször, és a kettes-hetes _offsuit_ lett a legrosszabb.

A kezekhez csoportonként hozzárendeltek továbbá egy _bevételi rátát_, aminek a
mértékegysége a pókerben egyébként is használatos $BB/100 játszott\_kéz$. Ennek
a továbbiakban lesz jelentősége.

### Kézerősség

Ez egy metrika, amit a flop után lehet alkalmazni, hogy megbecsüljük a kezünk
erősségét a többi játékoshoz képest[@billings1998opponent]. Ellenfélmodellezés
nélkül egyszerűen megszámoljuk a nálunk jobb, rosszabb, és ugyanolyan kezeket.
Flop esetén pl $\binom{47}{2} = 1081$ lehetséges kéz lehet egy ellenfélnél^[A
nálunk levő kettőt, és a 3 db. leosztott lapot ismerjük: 52 - 5 db. lap
ismeretlen].
Több ellenfél esetén egyszerűen hatványozzuk a kapott értéket az ellenfelek
számával.

### Kézpotenciál

Kíváncsiak lehetünk arra, hogy milyen esélyünk van, hogy javul a kezünk a flop
után, ahogy leosztanak még közös lapokat. A kézpontenciál megmondja, hogy
mekkora eséllyel javul a lapunk, úgy, hogy összeszámolja, hogy hány esetben
történhet ez meg (a kézpotenciáltmég pozitív potenciálnak is hívják).

Ehhez meg kell nézni egy ellenfél lehetséges lapjait: a flopon  pl. 990 * 1081
lehetséges kombinációt kell végignéznünk, mert 990 féle turn és river
kombináció lehetséges. Bizonyos kezeknek (ha már ismerjük a flopot) sokkal
nagyobb esélyük van a javulásra, pl a sorhúzóknak^[Olyan kéz, ami 1 lap híján
sort alkot] és a flösshúzóknak^[Olyan kéz, ami egy lap híján flösst alkot].

A negatív potenciál az előbbivel analóg módon annak az esélyét adja meg, hogy
egy kéz a leosztott lapok hatására gyengébbnek számít. Például nálunk
$\heartsuit K, \clubsuit A$ van, és a flopon az alábbi lapokat kapjuk:
$\spadesuit 5, \spadesuit 6, \spadesuit 7$. Itt az összes ellenfélnél levő, és
további közös káró lap, vagy négyes, és nyolcas rontja az esélyeinket.


### EHS

Az egyszerűség kedvéért jó, ha van egy darab érték, ami kifejezi az erősséget a
potenciállal együtt. Annak a valószínűségét akarjuk, hogy nálunk lesz a legjobb
kéz, miután leosztották az utolsó lapot is. Ezt hívjuk _effektív kézerősségnek_
(effective hand strength, EHS) [@davidson2002opponent, ch. 3.3].

$$ EHS = HS * (1 - NPot) + (1 - HS) * PPot $$

A képletben levő $HS$ a kézerősségnek felel meg, az $NPot$ és a $PPot$ pedig
rendre a pozitív, és negatív potenciált jelenti. A gyakorlatban viszont
kiveszik a negatív potenciált, és az alábbi képletet használják:

$$ EHS = HS + (1 - HS) * PPot $$

Elsőre ez nem tűnik magától értődőnek. A magyarázat erre az, hogy ha magas a
negatív potenciál, akkor valószinűleg amúgy is emelnénk, így kényszerítve az
ellenfelet, hogy be kelljen fektetnie a következő lapig^[Az ellenfél keze
javulhatna annak ellenére, hogy nem kell megfontolnia, hogy folytassa a kört.].


### Pot Odds

Egy másik, lazán kapcsolódó fogalom még a _pot odds_, ami kifejezi egy darab
érték formájában, hogy a befektetendő pénzhez mennyit nyerhetünk vissza. Ezt
sok helyzetben felhasználhatják az emberi, illetve a gépi játékosok is.

  $$pot\_odds = frac{megadandó\_zsetonok}{megadandó\_zsetonok + pot}$$

A megadandó zsetonok az adott körben, a játékos előtt történt emelések
összességét jelenti.


Opponent Modelling
------------------

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


Az ellenfélmodellezés minél tökéletesebb véghezviteléhez több különböző
próbálkozás született idáig:

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

A neurális háló a gépi tanuló rendszerek egyik fajtája. Különböző osztályozási
és regressziós feladatok megoldására lehet használni. Egyszerű felépítésének és
felhasználhatóságának köszönhetően az ellenfélmodellezés feladata is megoldható
vele.

Az alapegysége a perceptron, ami az emberi idegsejthez hasonlóan súlyozva
összegzi a bemeneteit, és ezt egy előjelfüggvény, vagy egy szigmoid függvény
bemeneteként használja fel. Az így képzett struktúra a bemenetek lineáris
szeparálására alkalmas. Nemlineáris szeparálás is megoldható több réteg
perceptron összekötésével, ahol egy réteg bemenete az előző réteg kimenete.

Egy konkrét példáról a Poki-ról szóló részben lesz szó.

<!-- esetleg a decision fákról még???-->


Különböző ágensek ellenfélmodellezése
-------------------------------------

A Loki és utódja, Poki a CRPG által fejlesztett játékos. Ezek az ágensek azok,
amelyek viszonylag összetettebbek, és valamennyire dokumentálva van a
működésük. Minden részletre nem térnek ki a tanulmányok, de a működésük
alapelve megismerhető.

A Poki megtalálható a PokerAcademy-ben is, mint ellenfél.

### Loki

Ahogy korábban elhangzott, az ellenfélmodellezésnek két feladatot kell
megoldania. Az egyik ezek közül az ellenfél laptartományának megbecslése, azaz
hogy milyen lapokkal fog játszani az asztalnál. A másik az ellenfél
cselekvéseinek megbecslése, de a Loki-nak erre nincsen explicit módon szüksége.

A laptartományok becslését a Loki egy súlyozott táblával oldja meg, ahol
mindegyik kezdőkézhez tartozik egy szorzó, hogy a többihez képest mekkora
eséllyel játssza meg az adott ellenfél az adott lapot.

A súlyok módosítása az ellenfelek cselekvései alapján történik: például ha egy
ellenfél emel a flopon, akkor az erősebb kezekhez tartozó súlyokat növeljük, és
a gyengébb kezekhez tartozóakat csökkentjük. A módosítást végző algoritmus
_generikus_, azaz minden ellenfélhez ugyanolyan szabályok érvényesek, viszont
minden egyes ellenfélnek külön súlytábla van tárolva [@billings1998opponent].

#### Súlyok kezdeti értéke

A súlyok kezdeti értékét statisztikai módszerekkel állapítják meg. Minden
ellenfélnél számon tartják, hogy mekkora arányban foldoltak, adtak meg, illetve
emeltek a flop előtt. Ebből származtatnak egy középértéket ($\mu$, a medián
kéz), és egy varianciát ($\sigma$), amiknek a mértékegysége $nagyvak / 100
játszott\_kéz$, azaz megegyezik a kezek bevételi rátájával.

Például egy ellenfél a kezeinek 30%-át adja meg. Ez egy +200-as medián kéznek
felel meg. Ha a varianciát 100-nak vesszük, akkor:

- a +300 feletti bevételi rátával rendelkező kezek súlyát 1-re állítjuk
- a +100 alatti bevételi rátával rendelkező kezek súlyát 0.01-re állítjuk
- a kettő érték között levő kezek súlyát arányosan állítjuk
- a 0-ra állítást, azaz annak feltételezését, hogy egy kezet egyáltalán nem
  játszik az ellenfél, elkerüljük

Az ellenfél releváns, flop előtt végzett cselekvéseit aszerint osztályozzuk,
hogy a 3 fajta cselekvés mennyibe került (0, 1, vagy 1-nél több), és melyik
körben hajtotta végre (pre-flop, flop, turn, river). Így 36 kategóriát
kapunk^[A Loki no-limit játékot is tud játszani ilyen módon].

#### Súlyok állítása

A súlyok állítását a flop után végezzük. Viszont mivel már 3 közös lap látható,
nem képezhetjük le közvetlenül a $\mu$-t és a $\sigma$-át kezdőkezekre.
Ehelyett a kezeket olyan módon rangsoroljuk, hogy tartalmazzák a közös lapokat
is. Ehhez a korábban megismert EHS-t használjuk, viszont a módosítás menete
analóg a flop előttivel, tehát szintén a kategóriákba rendezett cselekvések
alapján módosítunk (a kategóriákba rendezett cselekvések módosítássá történő
leképzése nem volt dokumentálva).


### Poki

A Poki a Loki továbbfejlesztett változata. A korábbi kezdőkezeknek fenttartott
súlytáblákat úgyanúgy használja, viszont ez az ágens már explicit módon
előrejelzi az ellenfél következő lépését, aminek a döntéshozásnál lesz szerepe.

Az egyik fő újítás a Pokiban a neurális háló alapú ellenfélmodellezés
használata. [@davidson2002opponent, chapter 4], A konkrét háló egy előrecsatolt
neurális háló, 1 rejtett réteggel.


\# | Típus | Leírás
-- | ----- | -----------------------------------------
0  | valós | pot odds
1  | valós | tétek aránya $tétek /(tétek + megadások)$
2  | bool  | a játékos befektetett zsetont
3  | bool  | egy tétet kell megadni
4  | bool  | kettő, vagy több tétet kell megadni
5  | bool  | turn van éppen
6  | bool  | river van éppen
7  | bool  | a játékos megadott téteket
8  | bool  | a játékos utoljára emelt
9  | valós | játékosok száma * 0.1
10 | bool  | 2 aktív játékos van
11 | bool  | játékos jön először
12 | bool  | játékos jön utoljára
13 | valós | kézerősség az adott ellenfélre
14 | valós | kézpotenciál az adott ellenfélre
15 | bool  | szakértői rendszer szerint megadna
16 | bool  | szakértői rendszer szerint emelne
17 | bool  | Poki jön éppen

Table: A Poki által használt bemenetek

A bemenetek a játék nyilvános állapotából kerülnek ki, amit mindegyik játékos
észlel. Látható, hogy a bool típusú bemenetek közül egymást kizáróak is vannak.
A szakértői rendszer alatt a Loki által is használt képlet-alapú döntéshozást
kell érteni, aminek a bemutatása később következik.

A súlyokat vizualizálták, így látható volt, hogy melyik bemenetek szignifikánsak az
előrejelzés szempontjából. Ezt mutatja a @fig:pokiNN ábra.

![A Poki által használt háló tanítás után](figures/poki_nn.png){width=50% #fig:pokiNN}


Döntéshozás
-----------

Minden mesterséges pókerjátékosnak, még a legegyszerűbbnek is, a rendelkezésre
álló információk alapján valamilyen döntések sorozatát kell meghoznia egy játék
megnyeréséhez. A döntéshozás^[A *Betting strategy* az ennek legjobban megfelelő fogalom
az angolban.] módja szerint többféle ágenst lehet készíteni.

### Szakértői rendszerek

Az ellenfélmodellezéshez hasonlóan itt is kézenfekvő, hogy szakértői tudással
vértezzük fel ágensünket. Tulajdonképpen ugyanazt a tudást fel lehet
használni mindkét célra.

<!--
### Analitikus

### Szimuláció alapű
-->
### Loki

Idáig megismertük a Loki ellenfélmodellezését. Most megnézzük, hogy hogyan
használja fel az előállított eloszlásokat az ellenfél lapjairól.

A Loki a flop előtt egy egyszerű szakértői rendszert használ, hogy eldöntse,
hogy beszáll-e a játékba. Ez lehetővé teszi a korábban megismert súlyozást is,
mivel a flop előtt még nincsenek súlyozva az ellenfelek kézskáláját számon
tartó táblák, így nem is lehetne használni a flop után levő döntési folyamatot.

#### Döntési folyamat

A Loki a flop után kiszámolja az _EHS_-ét. Az így kapott értéket összeveti egy
szakértői tudást tartalmazó táblázattal, amiből megkapja, hogy milyen értékek
esetén kell emelni. Az EHS-t a negatív kézpotenciál figyelembe vétele nélkül
használja fel, egyrészt az ellenfél elijesztése miatt, viszont amiatt is,
hogy nem tudjuk, hogy az ellenfelünk játszani fog-e (ez például nem feltétlen
van így, ha az ellenfélmodellezés ad erre választ)[@billings1998poker].

Például 0.5 fölött már jó eséllyel vagyunk az ellenfeleink előtt, és ezért a
Loki emelni fog, ha idáig senki sem emelt.

A Lokinak azon kívül, hogy eldöntse, hogy mikor kell emelnie, azt is el kell
döntenie, hogy mikor éri meg megadnia. Ezt a pot odds, és a pozitív
kézpotenciál számításával teszi meg. Ha $PPot > pot\_odds$, akkor ad meg egy
emelést.

A pozitív potenciál számításához egy kártya leosztását nézi meg előre. Ha a
turn, és a river közös lapját is figyelembe venné, akkor meg kéne vizsgálni azt
is, hogy vajon emelnek-e az első lap után.

### Poki
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

A Poki alapvetően tartalmazza a Loki összes képességét, viszont egy új módszert
is használ a döntéshozásra.

Számos játékban használnak fakeresés alapú technikákat, ahol egy játékfát
építenek, és a fa bejárásával próbálják megtalálni a legmegfelelőbb lépést.
Ezek a technikák^[Pl. minimax algoritmus] a pókernél nem működnek, mert a fa
köztes csúcsaihoz, amik nem levelek, az ismeretlen lapok miatt nem tudunk
kiértékelőfüggvényt biztosítani, és a játékfa mérete általában nagyon nagy,
hogy az egészet bejárjuk.

Ezekre a problémákra nyújtanak megoldást a szimuláció alapú megközelítések,
ahol a játékfa szélessége bejárása helyett néhány, irányított mélységi keresést
végzünk, azaz gyakorlatilag konkrét végigjátszásokat (_rollout_) csinálunk
egymás után (@fig:treesearch ábra).

![Különbség a klasszikus fakeresés, és a szimuláció
között.](figures/treesearch.png){width=50% #fig:treesearch}

Amikor a Pokinak egy döntésre van szüksége, és szimulációt használ, akkor
kiindulóállapotnak a játék jelenlegi állapotát veszi. Egy rollout
végigjátszásához előre kioszt ismert lapokat az ellenfeleknek is. A lapok
kiosztását el tudja végezni a már megismert módon, a súlytábla szerinti
eloszlással.

A végigjátszás közben, amikor az ellenfél jön, az ellenfélmodellező tudja
megválaszolni, hogy mit lépne. A publikációk nem térnek ki arra, hogy a Poki
hogyan dönti el, hogy szimuláció közben mit lépjen, viszont arra igen, hogy ez
nem egy triviális kérdés [@davidson2002opponent]. Ebbe a problémába én is
belefutottam a munkám közben.

Néhány száz rollout után a szimuláció eredménye már konvergál az egyes
cselekvések várhatóértékéhez. A legnagyobb várhatóértékű cselekvés kerül
kiválválasztásra.

<!-- még részleteket esetleg -->

Ágens megvalósítása
===================

<!-- 4 oldal -->
Az ágens megvalósításához figyelembe vettem a korábbi önálló laboron készült
terveket. A keretrendszer, és az architektúra egyes elemei onnan származnak.

A felhasznált keretrendszer
---------------------------

A pókerfajta kiválasztása után a következő lépés a keretrendszer kiválasztása
volt. Fő szempont volt, hogy a keretrendszer nyújtson már előre kész
ellenfeleket, amik ellen lehet tesztelni a kész játékost. Az önálló labor alatt
használt PokerAcademy megfelel ennek a követelménynek, mivel az University of
Alberta-i CRPG tagjai is részt vettek a fejlesztésében, és mellékeltét többek
között a Poki egyik változatát.

A PokerAcademy eredetileg egy edzőprogram emberi játékosoknak, de beállítható
úgy is, hogy csak gépi játékosok játsszanak egymással. Saját gépi játékost is
adhatunk a programhoz.

A keretrendszer Java-t használ, és Windows alatt fut. A hozzá készített
ágenseket (a játékban botnak hívják) plugin formájában kell mellékelni, és az
erre a célra készített Meerkat API-t^[Ez az API botok írását teszi lehetővé a
játékhoz. A benne levő Player interfész implementálását kell elvégezni.
(http://www.poker-academy.com/community.php) ] kell használniuk. Ez a
fejlesztést körülményessé teszi, főleg, ha az elkészített ágenst gyakran újra
szeretnénk fordítani, mivel mindig be kell csomagolni `.jar` formába az ágenst,
és újraindítani a játékot.

A fejlesztést könnyítendő, készítettem egy dummy ágenst, ami egyfajta
kliensként funkcionál egy tetszőlegesen megírt szerverhez, ami a tényleges logikát
tartalmazza. A dummy ágenst a játék által használt teljes Java runtime miatt
könnyen meg lehetett valósítani. Az elkészült kliens http-n keresztül küld, és
fogad XML üzeneteket, egy egyszerű API szerint[@GitHuban99:online]. Az ágenst
már erre alapozva fejlesztettem.

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

![Az ágens architektúrájának vázlata](figures/poker_architecture.png){width=75%}

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
