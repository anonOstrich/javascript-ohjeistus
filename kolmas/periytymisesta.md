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