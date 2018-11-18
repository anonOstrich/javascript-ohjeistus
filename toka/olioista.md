Olioiden käyttämisestä
======================

Tehtävänanto
------------
* Oliot ja niiden käyttäytyminen ovat JavaScriptissä hyvin omaperäisiä:
  * Olioiden luonti ja käyttö on erittäin dynaamista: Olioita voidaan luoda monin tavoin, kenttiä voidaan lisätä ja poistaa, kenttiin voidaan viitata eri tavoin, myös dynaamisesti, kentän nimeksi kelpaa miltei mitä tahansa. Kenttien käyttötapaa ja käyttömahdollisuuksia voidaan säätää: numeroituvuus, kirjoitettavuus, muutettavuus, ...
  * Konstruktorifunktion prototyyppiolioon voidaan liittää ominaisuuksia, jotka kaikki kyseisellä funktiolla konstruoidut oliot jakavat keskenään.
  * Aksessoreita ("gettereitä" ja "settereitä") voidaan ohjelmoida omin käsin tai käyttää kielen tarjoamaa erityistekniikkaa.
  * Käytössä on nykyään myös "syntaktisena sokerina" class-määrittely.
  * Ym, ym, ...

* Kaavailkaa olioiden käytölle hyviä (ja jos mahdollista turvallisia) ohjelmointityylejä ja -malleja. Perustelkaa selkein selityksin ja antakaa lyhyitä mutta valaisevia esimerkkejä mielestänne hyvistä ohjelmointikäytännöistä. Tässä vaiheessa ei kuitenkaan vielä käsitellä periytymisen tekniikkaa!

Hahmottelua
-----------

Mahdollisia käsiteltäviä:
* Eri tavat olion ominaisuuksien accessaamiseen
  * pistenotaatio vs merkkijono hakasuluissa: jälkimmäinen mahdollistaa dynaamisemman kenttien haun, mutta tässä voi olla myös riskejä. EI kannata käyttää, jos on pieninkään mahdollisuus, että käyttäjän syöte vaikuttaa haettavan kentän nimeen!
* Olion luontimahdollisuus ja mitä pitäisi käyttäjän
  * olioliteraali, new Object vai itse tehty konstruktori. Suositeltavaa varmaan literaali tai oma konstruktori, ainakaan nopeasti en keksi milloin keskimmäinen antaisi etua...
* JavaScriptin vapaus on huono siinä mielessä, että jos tulee kirjoitusvirhe olion kentän arvoa muuttaessa. Tällöin voi luoda uuden kentän sen sijaan, että olisi muokannut olemassaolevaa kenttää. Onkohan mahdollista rajoittaa oliota niin, että sille ei esimerkiksi voi luoda uusia kenttiä? Muutoin tällaiseen väärinkirjoitukseen on vaikea varautua.
* Hyvä malli lienee luoda olio konstruktorilla, heti tämän jälkeen lisätä konstruktorifunktion prototyyppioliolla toivottavat kaikkien olioiden jakamat funktiot ja muut attribuutit, joita ei konstruktorissa ole määritelty. Tällöin yhdessä paikassa hoidettu 'luokan' pystytys
* Konstruktorifunktion prototyyppiolion muokkaaminen antaa mahdollisuuksia, joita vaikea toteuttaa ajon aikana esim Javassa. Mahdollista siis lisätä kaikille tämän prototyypin perijöille uusi metodi, mitä Javassa vastaisi luokkaan uuden attribuutin lisääminen.
* Getterit /setterit - ei vahvaaa mielipidettä suuntaan tai toiseen.
* Ehkä classien käytölle on perusteensa; pikasilmäyksellä vaikuttaisivat mahdollisesti tiivistävän asioita, joita toki voisi tehdä myös ilman niitä. Esim privaattimuuttujien määrittäminen, ja onhan niitä nyt selkeämpi lukea tietyllä tavalla. 
