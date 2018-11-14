Ohjelmointityyleistä
====================

Ohjelmointityylivalinnan tulisi perustua niillä saavutettaviin hyötyihin, eikä esimerkiksi yhden paradigman vankkumattomaan kannatukseen. Koska JavaScript mahdollistaa monenlaiset ohjelmointityylit, kannattaa ohjelmoijan valita kuhunkin tilanteeseen sopiva lähestymistapa. Joitain hyvän ohjelmoinnin tunnuspiirteitä ovat helppous (aina ei kannata hakea tyylikkäintä mahdollista ratkaisua, jos melko tyylikäs toteutuu murto-osassa tämän vaatimasta ajasta), selkeys, yhteneväisyys ja ylläpidettävyys. Yhteneväisyyden vuoksi ei kannata valita aivan satunnaiseseti vuorotellen imperatiivisia ja funktionaalisia ratkaisuja, vaan pyrkiä tekemään samanlaiset asiat samalla tyylillä useimmiten.

Funktionaalisen tyylin hyötyjä
------------------------------

### puhtaat funktiot

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

### toivotun toiminnan kuvaus

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



### ensimmäisen luokan arvot

JavaScript käsittelee funktioita ensimmäisen luokan arvoina: niitä voi määritellä nimettöminä, antaa parametreina, palauttaa arvoina ja säilöä muuttujaan. Ominaisuudet vaaditaan funktionaaliseen ohjelmointityyliin, ja jokaisen JavaScript-ohjelmoijan kannattaa ehdottomasti hyödyntää ominaisuuksia ainakin ajoittain.

#### abstraktimmat funktiot

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

#### funktioliteraalit

Jos funktiota käyttää vain kerran ohjelmassa, on selkeintä määritellä se käyttökohdan yhteydessä. Se ei tarvitse nimeä, ja sijoitus eri kohtaan koodia vie turhaan tilaa. Esimerkiksi yksittäinen merkkijonotaulukon eriskummallinen muutos:

```javascript
taulukko.map(merkkijono => merkkijono + "ankka")
```


 Imperatiivisen tyylin hyötyjä
 -----------------------------

Ohjelmiston tilan muokkaaminen on selvästi helpompaa ja nopeampaa, kun funktioiden ei tarvitse olla puhtaita. Esimerkiksi tiedostojen lukemisessa ja kirjoittamisessa imperatiivinen tyyli voi olla helpompaa, sillä siinäkin on helposti kyse ei-puhtaista funktioista. Tiedonsiirtoonkin on olemassa funktionaalisia välineitä, mutta ainakin kevyellä tutustumisella ne vaikuttivat haastavammilta ymmärtää.

Olio-ohjelmointi on laajalle levinnyt imperatiivisen ohjelmoinnin alatyyli, ja se voi olla looginen tapa jäsentää ohjelman toimintaa. Usein olion omat funktiot muokkaavat myös olion muiden muuttujien arvoja, jolloin kyse ei ole puhtaista funktioista. Jos olioilla järjestyksen saaminen tuntuu luontevalta tavalta, kannattaa noudataa sitä paradigmaa.

 Imperatiivisen ohjelmoinnin suoritusjärjestys voi olla melko luonnollisen näköistä ja helposti seurattavaa. Silmukat ja ehtojen avulla suorituksen ohjaaminen ovat nopea ja selkeä ratkaisu useassa tilanteessa.
