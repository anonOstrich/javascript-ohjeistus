* Antakaa hyviä esimerkkejä tyyppiturvallisuutta tavoittelevasta ohjelmointityylistä funktioitanne käyttäen.

* Voitte pohtia tyyppiturvallisuuden tavoittelua myös yleisemmällä tasolla. Antakaa myös hyviä neuvoja ja ohjeita JavaScript-ohjelmoijalle.


## Tyyppiturvallisuudesta

Javascriptille ominaista on muuttujien dynaaminen määritys. Se mahdollistaa monipuolisten funktioiden kirjoittamisen, koska sama funktio voi käsitellä eri muuttuja tyyppejä. Kuitenkin, koska javascriptissä muuttujilla ei ole tyyppiä vaan ainoastaan niiden arvolla, tulisi kehittäjän ottaa erityisesti huomioon olioiden tyypit ja niiden mahdollinen muuttuminen ohjelman aikana.

### kokonaisluku

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

### Yleisiesti jokin luku

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

### Luku taulukot

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

### Muita tarkistuksia

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
