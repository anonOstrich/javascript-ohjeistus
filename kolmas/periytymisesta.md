# Olioiden periytyminen

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
Koska Alilla luotaville olioille ei päädy ylimääräistä super-kenttää, säästää ratkaisu hieman tilaa. Muista ohjelmointikielistä tulleille superin käyttäminen voi olla helpompaa ymmärtää, sillä call ei pelkästään koodia katselemalla välttämättä aukea lukijalle. 

Mikään JavaScriptissa ei velvoita muuttamaan Ali-funktion prototyyppiolioita Yli-funktiolla konstruoituun olioon. Olisi mahdollista siis, että Yli ja Ali eivät olisi niinkään lapsi ja vanhempi, vaan kaksi sisarusta (molempien funktioiden prototyyppioliot viittaisivat Object-funktion prototyyppiolioon __proto__-kentillään). Tällöinkin Ali voisi käyttää Yliä tällä tavalla, jolloin tietyllä tavalla Alin konstruoimat oliot sisältävät samoja kompotennteja. Jos Yli taas perisi muita olioita, eivät ketjun varrella olevat ominaisuudet kuuluisi Alilla luotuihin olioihin. Tällaisessa tapauksessa ei siis olisi kyse 'oikeasta' perimisestä.

Vaikka yleisesti kannustamme JavaScriptin käyttämiseen melko samalla tavalla kuin muita ohjelmointikieliä, haluamme nostaa esiin erään perintätavan joka sekoittaa perintään piirteitä useammasta oliosta: 

```javascript
function Eri(kentta){
   this[`${kentta}`] = 42;
}

function Yli(n){
  this.arvo = n || 0; 
}

function Ali(n, nimi){
  Yli.call(this, n); 
  Eri.call(this, "vastaus"); 
  this.name = nimi || "nobody"; 
}

Ali.prototype = new Yli(); 
a = new Ali(3, "jukka"); 

// a:n kentät:
// arvo: 3, name: "jukka", vastaus: 42

```
Alin konstruktori lisää tuottamiinsa olioihin Erin määrittelemiä ominaisuuksia sen lisäksi, että luodut olion kuuluvat __proto__-ketjuun, jonka Yli-konstruktorifunktion prototyyppiolio on. Mikään ei estä tällä tavalla poimimasta ominaisuuksia monesta paikasta. Mahdollistahan olisi myös konstruktorifunktioiden "oma perintäketju" ja niillä luotujen olioiden "oma perintäketju". Hahmotellaan asiaa: 

```javascript
function Eki(laatu){
  Kaukainen.call(this); 
  this.softness = laatu; 
}

function Eri(laatu, kentta){
   Eki.call(this, laatu); 
   this[`${kentta}`] = 42;
}

function Yli(n){
  this.arvo = n || 0; 
}

function Ali(n, nimi){
  Eri.call(this, "karhea", "vastaus"); 
  this.name = nimi || "nobody"; 
}

Ali.prototype = new Yli(); 
a = new Ali(3, "jukka"); 

// a:n kentät:
// arvo: 3, name:"jukka", vastaus: 42, softness: "karhea", ...

```
Ilmeistä on uhka sille, että perintätilannetta ei enää jossain vaiheessa. Tällaisen toiminnallisuuden hyödyntämistä ei kannata sulkea automaattisesti iäksi mahdottomaksi; syytä on tosin dokumentoida ajatuksensa ja suunniteltava toteutusjärjestely tarkkaan. Onneksi ainakin Chrome-selaimen JavaScript-suoritusympäristö heittää poikkeuksen, jos __proto__-viittaukset aiheuttavat silmukan.


## Perintä ongelma-alueen loogisena jäsentelytapana

Perintä on kaiken kaikkiaan JavaScriptissä hyvin helppoa: riittää muuttaa yksittäistä viitettä osoittamaan eri olioon. Koodin uudelleenkirjoittamisen vähentämiseksi saattaa ollakin houkutus periä ainakin joksikin aikaa olio, jonka alailmentymä perivä olio ei varsinaisesti ole. Periminen yleisesti tarkoittaa, että perivä olio täyttää kaiken toiminnallisuuden kuin sen yläolio, ja jotain muuta / kenties eri tavalla. Sekaannuksen välttämiseksi (etenkin jos et työstä ohjelmointiprojektia yksin) on yleensä parasta pitäytyä tässä perimisen määrittelyssä. 

Erityishuomautuksena: tuskin koskaan on järkevää että esimerkiksi taulukko perii funktion. Poikkeuksen tähän muodostavat kielen valmiit perintäsuhteet, kuten sen että Function-funktion __proto__-kenttä osoittaa 'oikeaan' olioon. 
