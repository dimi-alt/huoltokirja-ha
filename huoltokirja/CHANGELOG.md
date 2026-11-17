## Manuaalinen ZIP-tuonti ja -vienti

- Raportit-näkymään lisätty nykyisen ZIP-viennin rinnalle kaikkien Huoltokirjan tietojen turvallinen ZIP-palautus.
- Ennen palautusta tarkistetaan tiedoston rakenne, liitteet, SHA-256/CRC, SQLite-rakenne ja viite-eheys sekä näytetään määrällinen esikatselu ja vaaditaan erillinen korvausvahvistus.
- Liitteet siirretään uusiin nimiin ennen tietokannan transaktiota; virhetilanteessa alkuperäiset tiedot ja tiedostot säilyvät.
- Aikaisemmin luodut Huoltokirjan ZIP-viennit säilyvät tuettuina. Nyt vienti myös varoittaa, jos jokin liite puuttuu.
- Ei erillistä automaattista varmuuskopiointia: käytössä säilyy Home Assistantin järjestelmävarmuuskopiointi.

## Mobiilituonnin korjaus, huolto-ohjeet ja monipuolinen esikatselu

- Tuonti toimii CSV- ja JSON-tiedostoilla, mobiilissa FileReader-yhteensopivasti sekä tekstinä liittämällä.
- Kaikille huoltokohteille opt-in Bosch Climate 6101i / Husqvarna R 214TC -mallipohjat, jotka esikatsellaan ennen lisäämistä.
- Tuonnin valittavat rivit, korjattavat nimet, määräpäivät, mittarirajat, huolto-ohjeet, varaosat, tekijä ja ohjeen URL.
- Tuetut päivävälein toistuvat huollot, esimerkiksi suodattimien 14 päivän puhdistus.
- Vanhat 10-sarakkeiset CSV:t toimivat edelleen, samannimiset tehtävät ohitetaan ja vanha huoltohistoria säilyy.
- P1 näkyy käyttömittariin liittyvissä laitenäkymissä, ei yleisnavigaatiossa tai muihin kategorioihin liittyvissä lomakkeissa.
- Uudet huoltotiedot tallentuvat olemassa olevaan SQLite-tietokantaan idempotentilla skeemapäivityksellä.

## Yleinen CSV-huolto-ohjelmien tuonti ja P1-käyttöliittymän rajaus

- Korvattu työkoneissa näkynyt Husqvarna R 214TC -tuontipainike kaikkien kohteiden Huolto-ohjelma-välilehdellä yleisellä **Tuo CSV** -painikkeella ja ladattavalla CSV-mallipohjalla.
- CSV-tiedosto tukee aika-, mittari-, käynnistys- ja kiinteitä määräpäivärajoja sekä kertaluonteisia huoltoja; tuonti validoidaan ja tallennetaan atomisesti. Samannimiset rivit ohitetaan eikä aiempaan historiaan kosketa.
- Poistettu mallikohtainen pitkä Husqvarna-huomautusteksti ja automaattinen Husqvarna-ohjelmien lisääminen käynnistyksessä. Jo tallennetut ohjelmat säilyvät.
- P1-käyttömittarin yhdistäminen ja sen käyttöliittymäilmoitukset rajattu Laite-kategoriaan. Muut kategoriat eivät enää tarjoa yhdistämistä, eikä rajapinta hyväksy uusia linkityksiä niihin. Vanhat tallennetut linkitykset jätetään koskematta.

## R 214TC 2024 – valmistajan huolto-ohjelma

- Tuotu Husqvarna R 214TC vm. 2024:n 50 h ensihuollot, 100 h määräaikaishuollot ja 200 h vaihteistoöljyn vaihtoväli erillisinä tuntirajoina.
- Automaattinen kertaluonteinen, duplikaatit estävä kohdistus yksiselitteiseen nykyiseen 'Ajettava ruohonleikkuri' -kohteeseen päivityksen jälkeen; muille työkonekohteille turvallinen tuontipainike.
- Ensihuollot suljetaan vain todellisen huoltokirjauksen jälkeen; historia, aiemmat ohjelmat ja mittarilukemat säilyvät.
- Vuosittaisen moottoriöljy-/suodatinhuollon kalenterieräpäivä asetetaan erikseen viimeisen huollon todellisen päivämäärän perusteella.

## Huoltokirja 0.2.6

- Korjattu P1-laitteiden lukeminen Home Assistantin rajapinnasta S6-ympäristössä.
- Lisätty selkeä virheilmoitus, jos API-oikeus puuttuu tai anturia ei löydy. Ei muutoksia olemassa oleviin huoltotietoihin.

## 0.2.4

- P1-laitteiden yhdistäminen Huoltokirjaan ja käyttötuntien/käynnistysten lukeminen Home Assistantin kautta.
- Huolto-ohjelmien käyttötunti- ja käynnistysrajat, vanhojen P1-merkintöjen kertaluonteinen siirto. Ei autonlämmityksen P1-seurantaa.
- Vanhat Huoltokirjan kohteet, huollot ja liitteet säilyvät.

# Muutoshistoria

## 0.2.x

- Mobiilissa Lisää kohde -painike näkyy myös kategorioissa, joissa on jo kohteita.
- Uuden huoltokohteen kategoria esivalitaan avatun kategorian perusteella.
- Mobiilin +-painikkeesta voi lisätä kohteen, huollon tai huolto-ohjelman.

- Korjattu huoltokohteen tallennus Home Assistant Ingressin paloittaisilla JSON-pyynnöillä.
- Tarkennettu virheellisten ja liian suurten pyyntöjen tarkistuksia.

- Yksityinen GitHub Container Registry -jakelu.
- Home Assistantin oma automaattinen päivitys ilman erillistä päivitintä.
- Ensimmäisen Huoltokirja-version ominaisuudet: useita autoja, huoltohistoria,
  toistuvat huollot, katsastukset, kustannukset ja liitteet.
