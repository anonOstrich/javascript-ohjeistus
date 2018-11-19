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

Luokat, getterit ja setterit
----------------------------

* Hyvää: luokan määrittelyssä voidaan määritellä oliometodit, toisin kuin konstruktorifunktiossa
* Huonoa: luokat eivät tarjoa yhtä paljon kuin 'oikeat' luokat, ja saattavat hankaloittaa oliorakenteen ymmärtämistä. Toisaalta konstruktorifunktiot voivat vaatia hyvinkin konkreettista ymmärtämistä kielen toiminnasta; kostetaanhan suoraan konstruktorifunktion prototyyppiolion kenttään, jos konstuktorilla luotaville olioille halutaan määrittää fiksusti yhteisiä oliometodeja. Onko hyvä vaatia konkreettista ymmärrystä? Ehkä JavaScriptin tapauksessa on... Mutta ei toinenkaan huono ole. Pitäydy yhdessä.  
* getterit ja setterit: hyi. Peruste: hyvin hämäävää, kun voi olla tilanne jossa luulee viittaavansa olion kenttään, mutta onkin **kutsunut** sen metodia ja saanut aikaan vaikutuksia. 

Dynaamisuus ja vaarallisuus
---------------------------
*
* Dynaamisesti valittavat kentät - vaarallista jos käyttäjä voi vaikuttaa merkkijonoon, joka määrää kentän.
* Jos kirjoittaa kentän nimen väärin, saattaa luoda uuden muuttujan eikä muuttaa jo olemassaolevaa. Kutsumalla setterinomaisia funktioita (ei oletussettereitä, sillä niiden kutsumisen syntaksi on erilainen. ) aiheutuu virhe, mikä on hyvin toivottavaa!
* 