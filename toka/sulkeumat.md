
Sulkeumista
===========

Näkyvyysalue tarkoittaa sitä, että funktion muodolliset parametrit, paikalliset muuttujat ja paikalliset funktiot ovat käytettävissä vain funktion sisällä.
Sulkeuma on erittäin hyödyllinen ohjelmointitekniikka, jossa funktion sisällä luodaan sisempi funktio. Tässä sisemmän funktion näkyvyysalueilla on pääsy ulompaan koko näkyvyysalueelle, muttei toisinpäin. "Sisältä näkee ulos, ulkoa ei näe sisään"

Sulkeuman sidottu muuttuja on sellainen, joka on olemassa ja merkityksellinen vaan funktion sisällä sen suorituksen aikana.
Sulkeuman vapaan muuttujaan voi viitata, vaikkei sisälly itse funktioon.

### Sulkeumaan suljetut vapaat muuttujat säilyvät sulkeuman suorituksen jälkeiseen aikaan.

```javascript
function lisaa(x) {
  var a = 12;
  function jotain() {
    a -= 2;
    return a
  }
  return a + x;

write(lisaa(2))
```



### Sulkeumaan suljetuttujen vapaiden muuttujien määritellyt funktio päättyy, mutta sulkeuma säilyy.

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


[Olioden käyttämisestä](olioista.md)
