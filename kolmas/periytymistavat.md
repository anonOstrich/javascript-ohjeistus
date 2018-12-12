
# Olioiden periytyminen

JavaScript-kielessä on mahdollista luoda periytyviä olioita. Tämä tarkoittaa sitä, että me luomme olion, joka perii ominaisuuksia toiselta oliolta. Perinnän rooli on tärkeä, kun haluamme välttää koodin toistuvuutta ohjelman koodissa. Esimerkiksi, jos mallintaisimme koulun oppilaita sekä henkilökuntaa, voisimme luoda olion, joka määrittää kaikki arvot, jotka esiintyvät kaikilla olioilla, kuten nimi sekä ikä. 
## Perintä

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

## ECMAScript 2015 classes

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
## [Periytymisestä muutoin](periytymisesta.md)
