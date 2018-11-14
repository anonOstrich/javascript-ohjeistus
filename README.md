# javascript-ohjeistus
JavaSript-kurssin aikana kehittyvä ryhmätyö

## Viikko 1 (deadline 14.11.)

* [Tyyppiturvallisuus]()
* [Millaiset algoritmit sopivat JavaScriptiin](eka/algoritmeista.md)
* [Ohjelmointityyli - funtionaalinen vai imperatiivinen?](eka/ohjelmointityyleista.md)


* Tyyppiturvallisuuden tavoittelua
  * Tunnetusti JavaScriptissä muuttujilla yms. ei ole tyyppiä, mutta arvoilla on. Kielessä on joukko arvon tyypin tutkimisen välineitä, mutta ehkei niitä ole tarpeeksi ja ehkeivät ne ole tarpeeksi tiukkoja. Esimerkiksi parseInt hyväksyy numeromerkkien jonon perään mitä vain roskaa, jne.
  * Laatikaa siis ehdotus yhtenäisestä joukosta kirjastofunktioita, joilla arvojen tyyppejä voi tarkastaa. Olisiko totuusarvo vai ehkä undefined paras paluuarvo ilmaisemaan tyypiltään virheellistä arvoa?
  * Tarkistusfunktioita voisi tehdä ainakin kokonaisluvuille, yleensä luvuille, merkkijonoille, pelkästään kokonaislukuja sisältäville taulukoille, pelkästään lukuja sisältäville taulukoille, ..., ym. Keksikää tarpeellisia(?) lisäyksiä.
  * Antakaa hyviä esimerkkejä tyyppiturvallisuutta tavoittelevasta ohjelmointityylistä funktioitanne käyttäen.
  * Voitte pohtia tyyppiturvallisuuden tavoittelua myös yleisemmällä tasolla. Antakaa myös hyviä neuvoja ja ohjeita JavaScript-ohjelmoijalle.
* Algoritmeja ja funktioita
  * Tässä vaiheessa ei vielä periytymisen tekniikkaa ja problematiikkaa!
  * Millaiset algoritmien kirjoittamisen tyylit ovat luontevia JavaScriptille? Erityisesti: miten muuttujien, parametrien ja funktioiden   tyypittömyys pitäisi ottaa huomioon? Pitäisikö tyypittömyyden hyväksikäyttöä välttää ja pyrkiä "javamaiseen" tyyliin? Vai tarjoaako tyypittömyys ehkä sittenkin oivallisia tekniikoita joustavien ja yleiskäyttöisten algoritmien ja funktioiden toteuttamiseen? Perustelut ovat tässä – kuten koko dokumentaatiossa – oleellisia!
  * Funktionaalista vai imperatiivista? Kumpaa paradigmaa kaksiparadigmakielessämme kannattaa suosia? Vai onko yhdistelmä paras vaihtoehto: mitä funktionaalisesti, mitä imperatiivisesti? Selostettuja esimerkkejä!
