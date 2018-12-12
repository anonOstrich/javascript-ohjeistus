Algoritmeista
=============

Millaiset algoritmien kirjoittamisen tyylit ovat luontevia JavaScriptille?
--------------------------------------------------------------------------

JavaScript sallii hyvin monenlaiset käytännöt tyypittömyydellään, joten se sallii myös erittäin monia tapoja kirjoittaa ja käyttää algoritmeja. Tässäkään asiassa tuskin on syytä täysin sulkea pois eriskummallisiä käytäntöjä; saattaa olla yksittäistapauksia, joissa esimerkiksi Javalle haastava/mahdoton funktio on yksinkertaisin tapaus.

### Miten muuttujien/parametrien/funktioiden tyypittömyys pitäisi ottaa huomioon?

Funktioiden kirjoittaja tekee lähes aina oletuksia siitä, millaisia syötteitä funktio tulee saamaan. Teknisesti mikään ei estä minkääntyyppisen parametrin arvoa, mutta esimerkiksi matemaattiset funktiot odottavat aina 'number'-tyyppistä parametria. Etenkin kun funktiota käyttävät mahdollisesti muutkin kuin sen kirjoittaja, on funktion tarpeen tehdä jonkinlaista tyypin tarkastamista parametreilleen. Esimerkiksi esittelemämme [HIENO INTERTEKSTUAALINEN VIITTAUS] voi tarkastaa, että annettu parametri on sopiva luku. Tietysti jos jokaisen muuttujan tyyppi tarkastetaan hyvin usein funktioissa, menetetään osa tyypittömyyden eduista - eikö olisi kätevämpää vain käyttää tyyppejä, jolloin voi olla varma esimerkiksi funktion saamien parametrien laadusta? Mahdollisesti tarkastamista voi jättää vähemmälle kattavalla dokumentaatiolla, josta selviää tarkasti minkälaisilla parametreilla funktion voi odottaa toimivan oikein. Silti tyyppitarkastukset estävät mahdolliset huolimattomuusvirheistä syntyvät epäselvyydet, joissa virheen paikallistaminen voi olla haastavaa. 

Funktioiden on myös mahdollista palauttaa minkätyyppisiä arvoja tahansa; kenties joskus olisi houkuttelevaa palauttaa esimerkiksi tietyssä tapauksessa merkkijono, toisessa luku ja kolmannessa erityyppinen olio. Mielestämme tämänlaista käyttäytymistä pitäisi ehdottomasti välttää yleisenä sääntönä, sillä se vaikeuttaa funktion toiminnan ymmärtämistä. Pääsääntönä funktion pitäisi palauttaa vain yhdentyyppisiä arvoja, kuten kokonaislukuja. Erityistapauksena esimerkiksi 'undefined', jonka palauttamalla funktio voi ilmoittaa että sen toiminnassa on tapahtunut jotain poikkeuksellista. Jos tästä kehotuksesta huolimatta kokee parhaimmaksi vaihtoehdoksi jossain tilanteessa tällaisen mahdollisesti erityyppisiä arvoja palauttavan funktion kirjoittamisen, tulee sen toiminta dokumentoida huolella. Muuten sen toiminnan silmäilijä saattaa esimerkiksi vilkaista nopeasti viimeisen rivin palautettavan arvon tyypin, ja olettaa että kaikki palautettavat arvot ovat samantyyppisiä - näinhän asia olisi esimerkiksi Javassa.

Tyypittömyyden hyötyjä on se, että voi kirjoittaa funktioita jotka toimivat monenlaisilla olioilla, joilla on riittäviä samanlaisuuksia. Javassakin tosin vastaavanlainen onnistuu luokkien perimisen ja erityisesti toiminnallisuuden määrittelevien rajapintojen kautta. Rajapintojen käytön hyvä puoli on se, että rajapinnan nimi on usein hyvin kuvaava; JavaScriptissä saman voi tehdä esimerkiksi funktion sopivalla nimeämisellä ja selittävillä kommenteilla. Ilman riittävää dokumentaatiota abstraktoitu monia tyyppejä varten kirjoitettu JavaScript-koodi voi olla vaikeatulkintaista. Tyypittömyys mahdollistaa siis toisaalta nopean koodin kirjoittamisen, mutta riskinä on virheet jotka aiheutuvat epätoivotuista tyypeistä ja mahdollinen koodin vaikeaselkoisuus, kun tyypit ja luokkien/rajapintojen nimet eivät ole näkyvissä koodissa. Ongelman lieventämiseksi suosittelemme yhtenäisiä ohjelmistoratkaisuja ja riittävää koodin dokumentaatiota.  



### [Funktioiden käytöstä](../toka/sulkeumat.md)
