JavaScript - hyvät käytännöt
============================

Kirjoittaneet: Eemil Haapsaari, Thierry Botty ja Jesper Kuutti

## Sisällysluettelo:


### Ohjelmointityyleistä

* [Ohjelmointityyli - funtionaalinen vai imperatiivinen?](eka/ohjelmointityyleista.md)


### Funktioiden käytöstä
* [Sulkeumat](../toka/sulkeumat.md)

### Olioista

* [Tyyppiturvallisuus](../eka/tyyppiturvallisuudesta.md)
* [Olioden käyttämisestä](../toka/olioista.md)
* [Periytymisen tapoja](../kolmas/periytymistavat.md)
* [Periytymisestä muutoin](../kolmas/periytymisesta.md)



## 1. Tietotyypit

### Tyyppiturvallisuudesta

Javascriptille ominaista on muuttujien dynaaminen määritys. Se mahdollistaa monipuolisten funktioiden kirjoittamisen, koska sama funktio voi käsitellä eri muuttuja tyyppejä. Kuitenkin, koska javascriptissä muuttujilla ei ole tyyppiä vaan ainoastaan niiden arvolla, tulisi kehittäjän ottaa erityisesti huomioon olioiden tyypit ja niiden mahdollinen muuttuminen ohjelman aikana.

#### kokonaisluku

Javascriptissa voimme ohjelman aikana tarkistaa simppelillä jos-lauseella, että onko muuttuja jotain tiettyä tyyppiä. Esimerkiksi voimme kysyä onko tyyppi numero funktiolla 'typeof'.

```javascript

  var a = 1;

  if(typeof(a) == 'number'){
    // tee jotain
  }

```

Jos haluamme luvusta lisää tietoa, esimerkiksi onko luku kokonaisluku. Voimme jakaa luvun luvulla 1 ja sen jakojäännöksen tulisi olla 0.

```JavaScript

  var a = 1;

  if(typeof(a) == 'number' && a % 1 == 0){
    // tee jotain
  }

```

Tällöin voimme myös muodostaa function, joka tarkistaa onko luku kokonaisluku.


```JavaScript

  function isInteger(x){
    return typeof(x) == 'number' && x % 1 == 0;
  }  

```

Kuitenkin, jos luku muuttuja olio luodaan käyttäen konstruktoria, ei juuri kirjoittamamme funktio toimi enää halutulla tavalla, vaan meidä täytyy käyttää tarkistuksessa apuna "Object.prototype.toString.call(input)" sen sijaan, että käyttäisimme "typeof(input)" funktiota. Object.prototype.toString.call(input) palauttaa tiedon muuttujan arvosta muodossa "[object <type>]" ja näiden muutosten jälkeen uusi, valmis funktiomme on:

```JavaScript

  function isInteger(x){
    return Object.prototype.toString.call(x) == '[object Number]' && x % 1 == 0;
  }  

```

#### Yleisesti jokin luku

Aikaisemmassa vaiheessa tarkistimme onko luku kokonaisluku. Kuitenkin luomamme funktio palauttaa arvon "false", jos arvo on numeerinen muttei kokonaisluku. Esimerkiksisyötteet "NaN" ja "infinity" palauttavat arvon "false", vaikka ne ovat valideja numeerisia arvoja, koska ne voivat olla osana laskutoimitusta esim. tuloksena. Kuitenkin, koska voimme saada vastauksen "NaN" myös laittomasta toimituksesta, voimme todeta, että meidän tulisi suodattaa myös ne pois tarkistaessamme onko arvo numeerinen. Myös "infinity" voidaan rajata pois haluamistamme syötteistä lisäämällä "isFinite(input)". Tällöin voimme muodostaa funktion:

```JavaScript

  function isNumber(x){
    return Object.prototype.toString.call(x) == '[object Number]' && !isNaN(x) && isFinite(x);
  }

```

Nyt kun meillä on oma tarkistus sille, onko arvona jokin luku, voisimme myös muokata kokonaisluvun tarkistuksen seuraavanlaiseksi.


```JavaScript

  function isInteger(x){
    return isNumber(x) && x % 1 == 0;
  }

```

#### Lukutaulukot

Kuvitellaan tilanne, jossa käsittelemmä taulukkoa, jonka arvoina pitäisi olla pelkästään kokonaislukuja esimerkiksi [1,2,3]. Haluaisimme varmistaa lukujen olevan kokonaislukuja. Voisimme ratkaista tämän ongelman luomalla funktion, joka tarkistaa yksitellen jokaisen arvon ja palauttaa false, jos törmää arvoon, joka ei ole kokonais luku. Voimme käyttää tässä apuna aiemmin luomaamme funktiota kokonaisluvun tarkistukseen. Jos taulukosta puolestaan ei löydy muita kuin kokonaislukuja palauttaa funktio true.

```JavaScript

  function containsOnlyIntegers(arr){
    for( var i in arr){
      if(!isInteger(arr[i])){
        return false;
      }
    }
    return true;
  }

```

Vastaavasti voisimme luoda tarkistuksen sille sisältääkö taulukko vain lukuja.

```JavaScript

  function containsOnlyIntegers(arr){
    for( var i in arr){
      if(!isNumber(arr[i])){
        return false;
      }
    }
    return true;
  }

```

#### Muita tarkistuksia

Äsken tarkistimme sitä ovatko arvot numeerisia. Vastaavasti voimme tarkistaa myös sen ovatko luvut merkkijonoja taikka totuusarvoja.


```JavaScript

  // merkkijono
  function isString(x){
    return Object.prototype.toString.call(input) == '[object String]';
  }

  // totuusarvo
  function isBoolean(x){
    return Object.prototype.toString.call(input) == '[object Boolean]';
  }
```


## Miten algoritmien pitäisi huomioida tyypit

JavaScript sallii hyvin monenlaiset käytännöt tyypittömyydellään, joten se sallii myös erittäin monia tapoja kirjoittaa ja käyttää algoritmeja. Tässäkään asiassa tuskin on syytä täysin sulkea pois eriskummallisiä käytäntöjä; saattaa olla yksittäistapauksia, joissa esimerkiksi Javalle haastava/mahdoton funktio on yksinkertaisin tapaus.

Funktioiden kirjoittaja tekee lähes aina oletuksia siitä, millaisia syötteitä funktio tulee saamaan. Teknisesti mikään ei estä minkääntyyppisen parametrin arvoa, mutta esimerkiksi matemaattiset funktiot odottavat aina 'number'-tyyppistä parametria. Etenkin kun funktiota käyttävät mahdollisesti muutkin kuin sen kirjoittaja, on funktion tarpeen tehdä jonkinlaista tyypin tarkastamista parametreilleen. Esimerkiksi esittelemämme [HIENO INTERTEKSTUAALINEN VIITTAUS] voi tarkastaa, että annettu parametri on sopiva luku. Tietysti jos jokaisen muuttujan tyyppi tarkastetaan hyvin usein funktioissa, menetetään osa tyypittömyyden eduista - eikö olisi kätevämpää vain käyttää tyyppejä, jolloin voi olla varma esimerkiksi funktion saamien parametrien laadusta? Mahdollisesti tarkastamista voi jättää vähemmälle kattavalla dokumentaatiolla, josta selviää tarkasti minkälaisilla parametreilla funktion voi odottaa toimivan oikein. Silti tyyppitarkastukset estävät mahdolliset huolimattomuusvirheistä syntyvät epäselvyydet, joissa virheen paikallistaminen voi olla haastavaa. 

Funktioiden on myös mahdollista palauttaa minkätyyppisiä arvoja tahansa; kenties joskus olisi houkuttelevaa palauttaa esimerkiksi tietyssä tapauksessa merkkijono, toisessa luku ja kolmannessa erityyppinen olio. Mielestämme tämänlaista käyttäytymistä pitäisi ehdottomasti välttää yleisenä sääntönä, sillä se vaikeuttaa funktion toiminnan ymmärtämistä. Pääsääntönä funktion pitäisi palauttaa vain yhdentyyppisiä arvoja, kuten kokonaislukuja. Erityistapauksena esimerkiksi 'undefined', jonka palauttamalla funktio voi ilmoittaa että sen toiminnassa on tapahtunut jotain poikkeuksellista. Jos tästä kehotuksesta huolimatta kokee parhaimmaksi vaihtoehdoksi jossain tilanteessa tällaisen mahdollisesti erityyppisiä arvoja palauttavan funktion kirjoittamisen, tulee sen toiminta dokumentoida huolella. Muuten sen toiminnan silmäilijä saattaa esimerkiksi vilkaista nopeasti viimeisen rivin palautettavan arvon tyypin, ja olettaa että kaikki palautettavat arvot ovat samantyyppisiä - näinhän asia olisi esimerkiksi Javassa.

Tyypittömyyden hyötyjä on se, että voi kirjoittaa funktioita jotka toimivat monenlaisilla olioilla, joilla on riittäviä samanlaisuuksia. Javassakin tosin vastaavanlainen onnistuu luokkien perimisen ja erityisesti toiminnallisuuden määrittelevien rajapintojen kautta. Rajapintojen käytön hyvä puoli on se, että rajapinnan nimi on usein hyvin kuvaava; JavaScriptissä saman voi tehdä esimerkiksi funktion sopivalla nimeämisellä ja selittävillä kommenteilla. Ilman riittävää dokumentaatiota abstraktoitu monia tyyppejä varten kirjoitettu JavaScript-koodi voi olla vaikeatulkintaista. Tyypittömyys mahdollistaa siis toisaalta nopean koodin kirjoittamisen, mutta riskinä on virheet jotka aiheutuvat epätoivotuista tyypeistä ja mahdollinen koodin vaikeaselkoisuus, kun tyypit ja luokkien/rajapintojen nimet eivät ole näkyvissä koodissa. Ongelman lieventämiseksi suosittelemme yhtenäisiä ohjelmistoratkaisuja ja riittävää koodin dokumentaatiota.  



## 2. Ohjelmointityyleistä
Ohjelmointityylivalinnan tulisi perustua niillä saavutettaviin hyötyihin, eikä esimerkiksi yhden paradigman vankkumattomaan kannatukseen. Koska JavaScript mahdollistaa monenlaiset ohjelmointityylit, kannattaa ohjelmoijan valita kuhunkin tilanteeseen sopiva lähestymistapa. Joitain hyvän ohjelmoinnin tunnuspiirteitä ovat helppous (aina ei kannata hakea tyylikkäintä mahdollista ratkaisua, jos melko tyylikäs toteutuu murto-osassa tämän vaatimasta ajasta), selkeys, yhteneväisyys ja ylläpidettävyys. Yhteneväisyyden vuoksi ei kannata valita aivan satunnaiseseti vuorotellen imperatiivisia ja funktionaalisia ratkaisuja, vaan pyrkiä tekemään samanlaiset asiat samalla tyylillä useimmiten.

### Funktionaalisen tyylin hyötyjä

#### puhtaat funktiot

Funktionaalisen ohjelmoinnin tunnuspiirteitä ovat puhtaat (pure) funktiot. Jos tällaiselle funktiolle annetaan samat parametrit, palauttaa se aina saman arvon. Lisäksi sen suorittamisella ei ole sivuvaikutuksia. Puhtailla funktioilla on monia hyviä puolia. Niiden testaaminen ja toiminnan ennakointi on erittäin helppoa, sillä funktion suorituskohdan kontekstia ei tarvitse miettiä lainkaan. Koska niillä ei ole sivuvaikutuksia, voi niitä varmuudella hyödyntää missä tahansa kohtaa koodia, jos ne palauttavat halutunkaltaisia arvoja. Äärimmäisen yksinkertaistettuna seuraavaksi imperatiivisen mahdollistama ja sitten funktionaalinen esimerkki.

```javascript

var i;

function f(){
  return i*2;  
}
```

``` javascript

function f(i){
  return i*2;
}
```
Selvästi funktionaalinen on parempi, vaikka harva varmasti ensimmäisellä tavalla ohjelmoisikaan. Jos ottaa tavakseen pyrkiä kirjoittamaan mahdollisimman puhtaita funktioita, ei koodiin pitäisi päätyä funktioita joilla on yllättäviä sivuvaikutuksia tai jotka palauttavat eri aikoina eri tuloksia. Lisäksi koodia lukeneiden on helpompi ymmärtää funktioiden toimintaa, kun sitä voi tarkastella täysin eristyksissä ympäröivästä koodista.

#### toivotun toiminnan kuvaus

Funktionaalinen ohjelmointityyli on deklaratiivista, eli koodissa pyritään painottamaan *mitä* halutaan tehdä. Tämä voikin selkeyttää koodin ymmärtämistä.

```javascript
// imperatiivinen

t = [1, 3, 7, 8];

for(let i = 0; i < t.length; i++){
  t[i] = t[i] * t[i];
}

```

```javascript
//funktionaalinen

t = [1, 3, 7, 8];

t = t.map(x => x*x);
```

Tässä imperatiivisessa esimerkissä lukija tietää suunnilleen mitä ohjelmoija haluaa tehdä, kun lukee funktion nimen *map*: halutaan palauttaa lista, jonka jokaista jäsentä on muokattu jollain annetulla funktiolla. Sen sijaan ylemmässä esimerkissä on luettava toteutuksen yksityiskohdat kokonaisuudessaan ennen kuin näkee halutun toiminnan. Tässä yksinkertaisessa tapauksessa tämä on helppoa, ja imperatiivisessakin tyylissä voi tietenkin jakaa toiminnallisuutta funktioiden avulla. Yleisemmin kuitenkin hyvinnimittyjen funktioiden ketjuttaminen helpottaa koko toiminnan ymmärtämistä, koska nimet kertovat mitä kyseiseltä ohjelmakoodin palalta odotetaan. Ylempi esimerkki on mahdoton funktionaalista paradigmaa seuratessa.

 Taulukon arvojen suora muokkaaminen vaatii vähemmän tilaa, mikä on imperatiivisen tyylin eduksi. Uuden taulukon luominen ja sen sijoittaminen t:hen on puhtaampaa, sillä tällöin samalla syötteellä saadaan aina sama palautusarvo; kuitenkin esimerkiksi taulukon viitteen antaminen parametrina ja taulukon arvojen muuttaminen yksi kerrallaan ei vaadi lainkaan ylimääräisen muistin varaamista toista taulukkoa varten.



#### ensimmäisen luokan arvot

JavaScript käsittelee funktioita ensimmäisen luokan arvoina: niitä voi määritellä nimettöminä, antaa parametreina, palauttaa arvoina ja säilöä muuttujaan. Ominaisuudet vaaditaan funktionaaliseen ohjelmointityyliin, ja jokaisen JavaScript-ohjelmoijan kannattaa ehdottomasti hyödyntää ominaisuuksia ainakin ajoittain.

##### abstraktimmat funktiot

Funktioiden antaminen parametreina ja palauttaminen arvoina mahdollistaa hyvin tehokkaan toiston välttämisen ja abstraktit funktiot. Seuraavassa esimerkissä saadaan aikaan funktioperhe, joista jokainen on funktio, joka lisää saamaansa parametriin tietyn luvun:

```javascript

function sum(x){
  function f(y){
    return y + x;
  }
  return f;  
}

lisaa3 = sum(3);
write(lisaa3(5))
// 8
```

Jos aivan kaiken abstraktoi, voi koodin selkeys karsia - etenkin ilman riittävää dokumentointia. Luonnollisesti esimerkin tyyppistä funktiogeneraattoria ei kannata luoda, jos koodissa ei esiinny monia toiminnaltaan toisiinsa liittyviä funktioita. Koodin myöhemmässä refaktoroinnissa tällaisia yhdistämismahdollisuuksia voi huomata helpommin kuin ensimmäistä kertaa kirjoittaessa.

##### funktioliteraalit

Jos funktiota käyttää vain kerran ohjelmassa, on selkeintä määritellä se käyttökohdan yhteydessä. Se ei tarvitse nimeä, ja sijoitus eri kohtaan koodia vie turhaan tilaa. Esimerkiksi yksittäinen merkkijonotaulukon eriskummallinen muutos:

```javascript
taulukko.map(merkkijono => merkkijono + "ankka")
```


### Imperatiivisen tyylin hyötyjä


Ohjelmiston tilan muokkaaminen on selvästi helpompaa ja nopeampaa, kun funktioiden ei tarvitse olla puhtaita. Esimerkiksi tiedostojen lukemisessa ja kirjoittamisessa imperatiivinen tyyli voi olla helpompaa, sillä siinäkin on helposti kyse ei-puhtaista funktioista. Tiedonsiirtoonkin on olemassa funktionaalisia välineitä, mutta ainakin kevyellä tutustumisella ne vaikuttivat haastavammilta ymmärtää.

Olio-ohjelmointi on laajalle levinnyt imperatiivisen ohjelmoinnin alatyyli, ja se voi olla looginen tapa jäsentää ohjelman toimintaa. Usein olion omat funktiot muokkaavat myös olion muiden muuttujien arvoja, jolloin kyse ei ole puhtaista funktioista. Jos olioilla järjestyksen saaminen tuntuu luontevalta tavalta, kannattaa noudataa sitä paradigmaa.

 Imperatiivisen ohjelmoinnin suoritusjärjestys voi olla melko luonnollisen näköistä ja helposti seurattavaa. Silmukat ja ehtojen avulla suorituksen ohjaaminen ovat nopea ja selkeä ratkaisu useassa tilanteessa.


## Funktiot JavaScriptissä


 
### Sulkeumista


Näkyvyysalue tarkoittaa sitä, että funktion muodolliset parametrit, paikalliset muuttujat ja paikalliset funktiot ovat käytettävissä vain funktion sisällä.
Sulkeuma on erittäin hyödyllinen ohjelmointitekniikka, jossa funktion sisällä luodaan sisempi funktio. Tässä sisemmän funktion näkyvyysalueilla on pääsy ulompaan koko näkyvyysalueelle, muttei toisinpäin. "Sisältä näkee ulos, ulkoa ei näe sisään"

Sulkeuman sidottu muuttuja on sellainen, joka on olemassa ja merkityksellinen vaan funktion sisällä sen suorituksen aikana.
Sulkeuman vapaan muuttujaan voi viitata, vaikkei sisälly itse funktioon.

#### Sulkeumaan suljetut vapaat muuttujat säilyvät sulkeuman suorituksen jälkeiseen aikaan.

```javascript
function kasvatayhdella() {
  var summa=0;
  return function() {
    summa++;
    return summa;
  }
}

var f = kasvatayhdella()
write(f()) //1
write(f()) //2
write(f()) //3
```

#### Sulkeumaan suljetuttujen vapaiden muuttujien määritellyt funktio päättyy, mutta sulkeuma säilyy.

```javascript
function Sulkeuma(){
  var sulkeuman_arvo = 5; // Luo sulkeumaan muuttujan sulkeuman arvo

  return {
    setSulkeumaArvo: function(arvo) { sulkeuma_arvo = arvo },
    getSulkeumaArvo: function() { return sulkeuma_arvo; }
  } // luo sisäiset funktiot sulkeuma_arvo:lle
}

/*
  Näin olemme luoneet funktion jota voi käsitellä luokan tavoin ja sen tiedot säilyvät, vaikka sen suoritus olisikin loppunut.
*/
sulkeuma = Sulkeuma();
sulkeuma.setSulkeumaArvo(99); // asettaa arvon 99
sulkeuma.getSulkeumaArvo(); // palauttaa 99
```

## 3. Oliot

### Olioiden luonti


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

### Class


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

### Getterit ja setterit


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





### Dynaamisuus ja vaarallisuus

Olion kenttiin voi viitata kahdella tavalla: pistenotaatiolla (`olio.kentanNimi`) tai merkkijonolla, joka on hakasuluissa (`olio["kentanNimi"]`). Jälkimmäinen tapa mahdollistaa hyvin dynaamisen tavan olion kenttien käsittelylle, sillä merkkijono voidaan muodostaa muuttujista, joiden arvot selviävät vasta ajon aikana. Samoin yleiskäyttöisten työkalufunktioiden luominen on mahdollista: aikaisemmin esittelemämme setAttribute toimii, koska on mahdollista tutkia olion kenttää, jonka nimi selviää vasta funktiota kutsuttaessa. On siis selkeästi tilanteita, joissa tämän dynaamisemman merkkijonoviitteen käyttäminen on hyvin perusteltua. 

Toisaalta vapautta ei ole syytä käyttää tarpeettomasti. Pistenotaatiolla on ainakin selvää, mihin kenttään viitataan; hakasulkutekniikalla tätä ei välttämättä näe kenttään viittaamiskohdassa, sillä merkkijonon muodostaminen on saattanut tapahtua muualla. Erityisesti kannattaa harkita tarkoin, antaako ohjelman käyttäjän vaikuttaa viitattavaan kenttään - tämä antaa käyttäjille voimakkaan työkalun viitata mihin tahansa olion tai sen yliolioiden kenttään. 

### Periytymisestä 

JavaScript-kielessä on mahdollista luoda periytyviä olioita. Tämä tarkoittaa sitä, että me luomme olion, joka perii ominaisuuksia toiselta oliolta. Perinnän rooli on tärkeä, kun haluamme välttää koodin toistuvuutta ohjelman koodissa. Esimerkiksi, jos mallintaisimme koulun oppilaita sekä henkilökuntaa, voisimme luoda olion, joka määrittää kaikki arvot, jotka esiintyvät kaikilla olioilla, kuten nimi sekä ikä. 

#### Perintä

```javascript

// Yläluokka
function Henkilo(nimi, ika){
  this.nimi = nimi;
  this.ika = ika;
}

// Perivät luokat
function Oppilas(nimi, ika){
  Henkilo.call(this, nimi, ika);
}

function Opettaja(nimi, ika, aine){
  Henkilo.call(this, nimi, ika);

  this.aine = aine;
}

// prototyypin osoittaminen
Oppilas.prototype = Object.create(Henkilo.prototype);
Opettaja.prototype = Object.create(Henkilo.prototype);

var oppilas = new Oppilas('Matti', 12);
var opettaja = new Opettaja('Kalle', 3, 'cs');

```

Koska haluamme, että opettajilla sekä oppilailla on yhteisiä kenttiä, on meidän järkevä luoda olio, jonka molemmat perivät. Tässä esimerkissä koulun oppilaat ja opettajat perivät Henkilö-konstruktorilla luodun olion. Ne asettavat alkuarvot arvot käyttämällä Henkilö-funktion call-funktiota. Call-funtiossa ensimmäinen arvo 'this' määrittää perivän luokan ja loput parametrit ovat arvot, jotka perittävä luokka asettaa.

Yläpuolella luomme perinnät käyttäen prototype:ä sekä Object.create funktiota. Nyt luoduille opettajalla sekä oppilaalla on molemmilla henkilö sekä oman luokkansa ominaisuudet.

```javascript

oppilas instanceof Henkilo; // tosi
oppilas instanceof Oppilas; // tosi
oppilas instanceof Opettaja; // epätosi

opettaja instanceof Henkilo; // tosi
opettaja instanceof Oppilas; // epätosi
opettaja instanceof Opettaja; // tosi

```

### ECMAScript 2015 classes

ECMAScript 2015 tuo mukanaan selvemmän enemmän javaa muistuttavan tavan luoda luokkia sekä periytymisiä. Se on syntaksiltaan yksinkertaisempi sekä helpompi luettava.

```JavaScript

// Luodaa periytyvä luokka
class Henkilo{
  contructor(nimi, ika){
    this.nimi = nimi;
    this.ika = ika;
  }
}

// Luodaan perivä luokka
class Opettaja extends Henkilo { // periytyminen lisäämällä extends
  contructor(nimi, ika, aine){
    super(nimi, ika); // syötetään arvot super/periytyvälle luokalle

    // Luokan omat muuttujat
    this.aine = aine;
  }
}

```

### Super
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


### Perintä ongelma-alueen loogisena jäsentelytapana

Perintä on kaiken kaikkiaan JavaScriptissä hyvin helppoa: riittää muuttaa yksittäistä viitettä osoittamaan eri olioon. Koodin uudelleenkirjoittamisen vähentämiseksi saattaa ollakin houkutus periä ainakin joksikin aikaa olio, jonka alailmentymä perivä olio ei varsinaisesti ole. Periminen yleisesti tarkoittaa, että perivä olio täyttää kaiken toiminnallisuuden kuin sen yläolio, ja jotain muuta / kenties eri tavalla. Sekaannuksen välttämiseksi (etenkin jos et työstä ohjelmointiprojektia yksin) on yleensä parasta pitäytyä tässä perimisen määrittelyssä. 

Erityishuomautuksena: tuskin koskaan on järkevää että esimerkiksi taulukko perii funktion. Poikkeuksen tähän muodostavat kielen valmiit perintäsuhteet, kuten sen että Function-funktion __proto__-kenttä osoittaa 'oikeaan' olioon. 
