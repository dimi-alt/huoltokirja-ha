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
