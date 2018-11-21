Olioiden käyttämisestä
======================

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

JavaScript tarjoaa monia mahdollisuuksia myös olion kenttien arvojen käyttämiseen ja muuttamiseen. Suorin tapa on viitata suoraan kenttiin: `olio.x = 3;` muuttaisi olion kentän x arvoksi 3. Kieli mahdollistaa myös olion kenttien arvoja käsittelevät getterit ja setterit. Erikoista niissä on funktioiden kutsutapa: vaikka kutsutaan funktiota, ei käytetä sulkuja. Näiden kahden funktion kutsuminen näyttää identtiseltä olion kenttään suoraan viittaamiseen, vaikka oikeasti kutsutaan funktiota jolla voi olla muitakin vaikutuksia. Tätä voi pitää eleganttina, kun ei tarvitse välittää viittaako getterifunktioon vai jonkun kentän arvoon. Mielestämme kuitenkin on vaarallista, että olion käyttäjä ei tiedä kutsuuko funktiota, ellei hän tiedä tämän olion kutsumisenhetkistä rakennetta. Jos funktiolla on sivuvaikutuksia, ei funktion kutsuja välttämättä tajua mistä ne johtuvat. Vaikka tällaiset sivuvaikutukset ehkä sopivat olio-ohjelmoinnin asenteeseen, jossa oliot vastaavat omasta datastaan ja toiminnastaan, kehottaisimme mahdollisuuksien mukaan pitämään funktiot niin puhtaina kuin mahdollista (kts. [ohjelmointityyleistä](../eka/ohjelmointityyleista.md)).

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
        throw `Oliolla ${olio} ei ole kenttä nimeltä ${kentanNimi} `
    olio[kentanNimi] = uusiArvo; 
}

```

Poikkeuksen heittämisen sijaan funktio voisi myös palauttaa false tai undefined, tai ilmaista jollain muulla tapaa että kaikki ei sujunut kuten piti. Tässä hyödynnettiin JavaScriptin vaihtoehtoista tapaa viitata olion kenttiin hakasulkujen ja kentän nimen merkkijonolla. Asiasta lisää seuraavassa kappaleessa. 





Dynaamisuus ja vaarallisuus
---------------------------

Olion kenttiin voi viitata kahdella tavalla: pistenotaatiolla (`olio.kentanNimi`) tai merkkijonolla, joka on hakasuluissa (`olio["kentanNimi"]`). Jälkimmäinen tapa mahdollistaa hyvin dynaamisen tavan olion kenttien käsittelylle, sillä merkkijono voidaan muodostaa muuttujista, joiden arvot selviävät vasta ajon aikana. Samoin yleiskäyttöisten työkalufunktioiden luominen on mahdollista: aikaisemmin esittelemämme setAttribute toimii, koska on mahdollista tutkia olion kenttää, jonka nimi selviää vasta funktiota kutsuttaessa. On siis selkeästi tilanteita, joissa tämän dynaamisemman merkkijonoviitteen käyttäminen on hyvin perusteltua. 

Toisaalta vapautta ei ole syytä käyttää tarpeettomasti. Pistenotaatiolla on ainakin selvää, mihin kenttään viitataan; hakasulkutekniikalla tätä ei välttämättä näe kenttään viittaamiskohdassa, sillä merkkijonon muodostaminen on saattanut tapahtua muualla. Erityisesti kannattaa harkita tarkoin, antaako ohjelman käyttäjän vaikuttaa viitattavaan kenttään - tämä antaa käyttäjille voimakkaan työkalun viitata mihin tahansa olion tai sen yliolioiden kenttään. 
