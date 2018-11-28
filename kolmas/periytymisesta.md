# Olioiden periytyminen


## Tehtävänanto


* Tunnetusti ("I hope"! ;-)) olio-ohjelmoinnin periytymistekniikan kaksi tärkeää tavoitetta ovat: a) ratkaistavan ohjelmointiongelman käsitteiden luonteva mallintaminen ja siten siis ongelman ratkaisijan ajattelun selkeyttäminen ja helpottaminen, sekä toisaalta b) koodin turhan kopioimisen välttäminen, koodin uudelleenkäyttö.

* Pyrkikää hahmottamaan ja löytämään, millaiset JavaScriptin ohjelmointitekniikat mahdollisimman hyvin palvelisivat näitä tavoitteita. Perusteluja tarvitaan!


## Mitä ehkä voi käsitellä 

* Periytymisen toteuttamisen tavat
  * Konstruktorin avulla luominen
  * __proto__-kentän arvon manuaalinen asettaminen
  * Object.create-funktiolla luotavan olion prototyypin osoittaminen
* Superin käyttäminen
* Mikä perinnässä voi mennä pieleen: erot liittyen arvon hakemiseen ja asettamiseen (asetettaessa x.b = c joko muutetaan x:n kenttää b tai luodaan x:llä tällainen uusi kenttä; koskaan ei edetä x:n prototyyppiin sen kenttieä tarkastelemaan. Sen sijaan kutsuessa x.b etsitään koko sen prototyyppiketjusta kenttää b liikkuen aina seuraavaan __proto__-kentän osoittamaan arvoon). 

Erikseen mainitut asiat: 
* Perintä loogisen ja auttavan ongelma-alueen mallintamisen välineenä
  * periminen: is-a, eli aina tarkentuvia olioita
  * onko muiden kuin 'oikeiden' olioiden periytyminen fiksua? Esim funktioiden/taulukoiden monimutkaisempi periytyminen? Tähän asti nähty vain funktioita joiden __proto__ on Function.prototype, ehkä syystä? 

* koodin turhan kopioimisen välttäminen aka koodin uudelleenkäyttö
  * jos olion toiminnallisuus on jonkin toisen olion toiminnallisuus + ekstraa => periminen hyvinkin aiheellista

* Jotain mikä JavaScriptin periytymisessä voi olla yllättävää muista kielistä tulijoille?

## Super
Usein pelkän prototyyppitetjun päähän lisääminen ei ole kaikki, mitä perinnältä halutaan. Esimerkiksi voidaan haluta, että alialioita luova konstruktori hyödyntää yliolioita luovan konstruktoria. Voi olla, että aliolioita luodessa halutaan esimerkiksi oletuksena antaa ylikonstruktorifunktiolle eri parametri, kuin sen oletusarvo on. Tällöin ei riitä, että luodun aliolion prototyypillä on kenttä, jossa on oletus arvo. Tutkitaan seuraavaan koodin aiheuttamaa rakenteiden linkitystilannetta: 
```javascript
function Yli(n){
  this.arvo = n || 0; 
}

function Ali(n, nimi){
  this.super = Yli; 
  this.super(n); 
  this.name = nimi || "nobody"; 
}

Ali.prototype = new Yli(); 
a = new Ali(3, "jukka"); 
```

Tilanteessa tätä super-rakennelmaa käytetään konstruktoriparametrien välitykseen a-olion prototyypin prototyypin konstruktorifunktiolle (eli Yli-funktiolle). Kuten materiaalissakin esitellään, täysin saman toiminnallisuuden saa aikaan ilman super-kentän lisäämistä jokaiselle Ali-funktiota konstruktorina käyttäville olioille. Tämä onnistuu Function-funktion prototyyppiolion funktiota call käyttämällä. Sen avulla voidaan määritellä, mihin olioon jonkin funktion this-viite kohdistuu. Ylempi olisi siis: 

```javascript
function Yli(n){
  this.arvo = n || 0; 
}

function Ali(n, nimi){
  Yli.call(this, n); 
  this.name = nimi || "nobody"; 
}
Ali.prototype = new Yli(); 
a = new Ali(3, "jukka"); 
```
Mikään JavaScriptissa ei velvoita muuttamaan Ali-funktion prototyyppiolioita Yli-funktiolla konstruoituun olioon. Olisi mahdollista siis, että Yli ja Ali eivät olisi niinkään lapsi ja vanhempi, vaan kaksi sisarusta (molempien funktioiden prototyyppioliot viittaisivat Object-funktion prototyyppiolioon __proto__-kentillään). Tällöinkin Ali voisi käyttää Yliä tällä tavalla, jolloin tietyllä tavalla Alin konstruoimat oliot sisältävät samoja kompotennteja. Jos Yli taas perisi muita olioita, eivät ketjun varrella olevat ominaisuudet kuuluisi Alilla luotuihin olioihin. Tällaisessa tapauksessa ei siis olisi kyse 'oikeasta' perimisestä. 
## Perintä ongelma-alueen loogisena jäsentelytapana
