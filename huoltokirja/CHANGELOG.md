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
