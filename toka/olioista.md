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



Olioiden luonti
---------------

JavaScriptissä olioita voi luoda kolmella tavalla: olioliteraalina, konstruktorifunktion avulla, tai Object-funktion (joka on myös olio, jolla on kenttiä) create-funktiota kutsumalla. Olioliteraalit tarkoittavat tekstiin sellaisenaan kirjoitettuja olioita: 
```javascript
   var olio = {a: 0, b: 1}; 
```

Yksinkertaisin konstruktorifunktio olioiden luomiseen on Object. Tällöin luotavalla oliolla ei ole omia kenttiä, joita kaikilla muilla olioilla ei olisi: se vain perii Object-funktion prototyyppiolion kentät. Muut kentät on lisättävä luonnin jälkeen, seuraavaan tapaan: 

```javascript
 var olio = new Object(); 
 olio.a = 0; 
 olio.b = 1; 
```

On mahdollista käyttää myös muita valmiiksi määriteltyjä tai itse tehtyjä konstruktorifunktioita. Vaikka mitä tahansa funktiota voi käyttää konstruktorina new-komennon kanssa, ei tästä ole ainakaan hyötyä. Kannattaa pitäytyä kielen käytännössä, jossa konstruktorifunktioiden nimi alkaa isolla alkukirjaimella, ja muiden funktioiden pienellä. Itsetehdyllä konstruktorilla aiempien koodien olio voitaisiin muodostaa näin: 

```javascript
function Konstruktori(a, b){
    this.a = a; 
    this.b = b; 
}

var olio = new Konstruktori(0, 1);
```


Näin voidaan funktiolle annetuille parameterillä asettaa luotavan olion kenttien arvoja. Jos samantyyppisiä olioita luodaan useita, on oman konstruktorifunktion määritteleminen erittäin hyödyllistä. Saman efektin saisi aikaan myös luomalla Object.prototype:n välittömästi perivän objektin, jonka voisi antaa sitten parametriksi funktiolle, joka lisää sille tietyt kentät alkuarvoilla. Lienee kuitenkin selkeämpää käyttää konstruktoreja, ellei ole poikkeuksellista tilannetta. Toisaalta jos luodaan kertakäyttöinen olio, paras lähestymistapa saattaa olla lisätä ja poistaa sen kenttiä sangen vapaasti, ilman että sen luonnissa vielä määrittelee ainoatakaan kenttää. 

Kolmas hieman eroava tapa on käyttää valmiin Object-funktio(-olio)n create-funktiota. Tämä mahdollistaa enemmän kuin vain kenttien nimien ja arvojen määrittämisen. Lisäksi voidaan kenttien käyttömahdollisuuksia: samoja asioita, joita Object-konstruktorin defineProperty-funktiolla voidaan muokatan. Luodaan  muuten samanlainen olio kuin aiemmissa esimerkeissä, mutta säädetään toisen kentän arvo muuttumattomaksi. create-funktion ensimmäinen parametri kertoo, mikä on luotavan olon yliolio: se on siis halutun konstruktorifunktion prototyyppiolio. 

```javascript
var olio = Object.create(Object.prototype, {
  a: {
      value: 0, 
      writable: false
  }, 
  b: {value: 1}
})
```

Class
-----

Aiemmin esittelemässämmä olion luomisessa konstuktorilla on tietty ongelma (tai vähintääkin tiedostamisen arvoinen ominaisuus): jos konstuktorifunktion sisällä määrittelee luotavalle oliolle funktioarvoisen kentän, saa jokainen luotava olio oman kopionsa tuosta funktiosta. Tämä ei yleensä ole hyödyllistä ja kuluttaa turhaa muistia, joten järkevämpää on lisätä funktiokenttä luotavien olioiden ylioliolle, eli konstruktorifunktion prototyyppioliolle. Järkevä tapa funktioita kenttinään sisältävien olioiden luomiseen on siis seuraava:

```javascript
function Lapsi(name){
  this.name = name; 
}
Lapsi.prototype.f = function() {return this.name + ":)"};

olio = Lapsi("Ykä"); 
write(olio.f()); // Ykä :)
```

Tapahan on hieman kömpelö, sillä ensin luotavien olioiden toiminnallisuutta säädellään konstruktorin määrittelyllä, ja sen jälkeen mahdollisesti useita funktioita on liitettävä konstruktorifunktion prototyyppiolioon. Tätä, ja kenties muitakin, asioita yksinkertaistaa tekstin tasolla JavaScriptin luokkatoiminnallisuus. Mitään uutta toiminnallisuutta luokat eivät kieleen tuo, mutta ne saattavat tehdä kielestä selkeämpää kirjoittaa luokkapohjaiseen kieleen tottuneelle. Luokan määrittelyssä oliofunktiot voi määritellä, kuten myös konstruktorin muun toiminnan. Ylhäällä oleva esimerkki olisi mahdollista tehdä myös näin: 

```javascript
class Lapsi{
    constructor(name){
        this.name = name; 
    }

    f() {
        return this.name + ":)";
    }
}
```
Luokat antavat mahdollisuuden määritellä suurimmat osan tietynlaisten olioiden ominaisuuksista samassa paikassa. Oliometodien lisäksi luokkamäärittelyssä voi luoda staattisia metodeja eli luokkametodeja: nämä ovat konstuktorifunktion kenttiä, eli niitä ei voi käyttää konstruktorilla tuotetun olion kautta. Mielestämme luokkamäärittelyn käyttäminen auttaa koodin jäsentelemisessä, joten sen käyttäminen on suotavaa. Toisaalta vastakkainenkin mielipide on ymmärrettävä: tämä määrittely kenties tekee koodista selkeämmän luokkakieleen tottuneelle, mutta samalla se piilottaa JavaScriptin oliojärjestelmän toimintaa. Oliojärjestelmän toiminnan tunteminen on kuitenkin hyväksi, joten funktioiden prototyyppiolioiden eksplisiittinen käyttäminen auttaa pitämään mielessä mitä kielen rakenteissa oikeasti tapahtuu. Kenties sopivin muotoilu on seuraava: class-syntaksia voi käyttää hyvällä omallatunnolla, jos saman koodin osaisi kirjoittaa myös ilman tätä tapaa. Tällöin ei herätä hämmennystä sekään, että JavaScriptin luokkamäärittely ei tarjoa kaikkea toiminnallisuutta mitä esimerkiksi Javan luokat tarjoavat. 

Getterit ja setterit
--------------------
**tässä voisi olla enemmän esimerkkikoodia**

JavaScript tarjoaa monia mahdollisuuksia myös olion kenttien arvojen käyttämiseen ja muuttamiseen. Suorin tapa on viitata suoraan kenttiin: `olio.x = 3;` muuttaisi olion kentän x arvoksi 3. Kieli mahdollistaa myös olion kenttien arvoja käsittelevät getterit ja setterit. Erikoista niissä on funktioiden kutsutapa: vaikka kutsutaan funktiota, ei käytetä sulkuja. Näiden kahden funktion kutsuminen näyttää identtiseltä olion kenttään suoraan viittaamiseen, vaikka oikeasti kutsutaan funktiota jolla voi olla muitakin vaikutuksia. Tätä voi pitää eleganttina, kun ei tarvitse välittää viittaako getterifunktioon vai jonkun kentän arvoon. Mielestämme kuitenkin on vaarallista, että olion käyttäjä ei tiedä kutsuuko funktiota, ellei hän tiedä tämän olion kutsumisenhetkistä rakennetta. Jos funktiolla on sivuvaikutuksia, ei funktion kutsuja välttämättä tajua mistä ne johtuvat. Vaikka tällaiset sivuvaikutukset ehkä sopivat olio-ohjelmoinnin asenteeseen, jossa oliot vastaavat omasta datastaan ja toiminnastaan, kehottaisimme mahdollisuuksien mukaan pitämään funktiot niin puhtaina kuin mahdollista (kts. [ohjelmointityyleistä] (eka/ohjelmointityyleista.md)).

JavaScriptin vapaus mahdollistaa kenttien lisäämisen oliolle sen luomisen jälkeenkin. Tämä voi aiheuttaa tarkoittamattomia tilanteita kirjoitusvirheiden tapahtuessa: 

```javascript
var hauva = {
    nimi: 'Fido';
    tervehdi: function() { write('Vuh!') }
}

hauva.trevehdi = function(){ write('Hau!') }; 
``` 
Tarkoitus oli muuttaa tervehtimisfunktiota, mutta kirjoitusvirheen vuoksi loimme uuden funktion ja alkuperäinen ei muuttunut! Mikään ei teknisesti ole väärin, joten ohjelmoija ei kenties huomaa virheettään kovin nopeasti. Valmiit getterit ja setterit eivät auta asiaa, koska niiden syntaksi on identtinen arvon asettamisen kanssa: jos sopivaa setteriä ei löydy, lisätään siinäkin tapauksessa oliolle uusi kenttä. Ratkaisuehdotus joissain tilanteissa on oman setterin toteuttaminen, esimerkiksi seuraavaan tapaan: 

```javascript
var hauva = {
    nimi: 'Fido', 
    tervehdi: function() { write('Vuh!') },
    setNimi: function(uusi) { this.nimi = uusi }, 
    setTervehdi: function(f) { this.tervehdi = f}
}

hauva.setTervehdi(function(){ write('Mau!') }); // toimii, asettaa uuden arvon
hauva.setTrevehdi(function(){ write('Hau!') }); // aiheuttaa virheen
```

Seuraavan kerran ohjelmaa suorittaessa ohjelmoija kohtaa virheen ja saa selkeän viestin: 'hauva.setTrevehdi is not a function'. Vaikka väärinkirjoituksista johtuvista ylimääräisten kenttien luomisesta pääsisi pitkälti eroon tekemällä tällaiset setterit, tuplaisi edellä esitetty lähestymistapa kenttien lukumäärän. Jos haluaa ehdottomasti välttää väärinkirjoitusväärinymmärryksiä, voisi kirjoittaa seuraavanlaisen yleiskäyttöisen funktion, joka muuttaa ilmaistun kentän, ja heittää poikkeuksen jos sennimistä kenttää ei ole olemassa. 

```javascript

function setAttribute(olio, kentanNimi, uusiArvo){
    if (olio[kentanNimi] === undefined)
        throw ``Oliolla ${olio} ei ole kenttä nimeltä ${kentanNimi} ``
    olio[kentanNimi] = uusiArvo; 
}

```

Poikkeuksen heittämisen sijaan funktio voisi myös palauttaa false tai undefined, tai ilmaista jollain muulla tapaa että kaikki ei sujunut kuten piti. Tässä hyödynnettiin JavaScriptin vaihtoehtoista tapaa viitata olion kenttiin hakasulkujen ja kentän nimen merkkijonolla. Asiasta lisää seuraavassa kappaleessa. 





Dynaamisuus ja vaarallisuus
---------------------------

* Dynaamisesti valittavat kentät - vaarallista jos käyttäjä voi vaikuttaa merkkijonoon, joka määrää kentän. Kuten edellisessä kohdassa huomattiin, tietyt hyödylliset asiat mahdollista tehdä vain tällä tavalla...
* Milloin pitäisi asettaa arvo esim vain luettavaksi? 