## P1-käyttötunnit

Huoltokirjaan voi yhdistää P1-tunnistimen laitteet ja lukea niiden käyttötunnit sekä käynnistyskerrat Home Assistantin kautta. Huoltovälit ja tehdyt huollot kuuluvat vain Huoltokirjaan; Järjestelmä-dashboard näyttää edelleen käyttötilastot. P1:n vanhat huoltorajat ja huoltomerkinnät siirretään kertaluonteisesti, kun P1-laite yhdistetään huoltokohteeseen. Edellyttää `homeassistant_api: true` ja P1-versiota 0.5.3. Jos P1 on poissa käytöstä, vanhentuneita lukemia ei kerätä uutena mittauksena.

---

# Huoltokirja

Huoltokirja on Home Assistant OS -sovellus useille autoille, kiinteistöille,
laitteille ja työkoneille. Kirjaa huollot, katsastukset, mittarilukemat,
toistuvat huoltovälit, kustannukset sekä kuitit.

## Asennus

Tämä asennusrepo sisältää vain Home Assistantin julkiset asennustiedot.
Sovelluskuva on **yksityinen** GitHub Container Registryssa. Home Assistantin
Supervisorille on ensin annettava lukuoikeus rekisteriin `ghcr.io`.

Rekisterin tunnistautumista varten tarvitaan GitHubin personal access token
**(classic)**, jolla on vain `read:packages`-oikeus ja pääsy yksityiseen
Huoltokirja-pakettiin. Älä tallenna tokenia tähän repositorioon tai
Home Assistantin yleiseen `configuration.yaml`-tiedostoon.

Asenna Huoltokirja Home Assistantin sovelluskaupasta, käynnistä sovellus ja
avaa sen Ingress-käyttöliittymä. Salli Huoltokirjan asetuksista automaattiset
päivitykset. Erillistä päivityssovellusta ei asenneta.

## Tiedot ja varmuuskopiot

Huoltokirjan tietokanta sekä liitteet tallennetaan Home Assistantin
sovellusdataan (`/data`). Ota Home Assistant -varmuuskopio ennen ensimmäistä
asennusta, suuria päivityksiä ja mahdollisia tietojen siirtoja.

Jos olet käyttänyt vanhaa paikallista `local_huoltokirja`-asennusta, sen data
**ei siirry automaattisesti** tämän GitHubista asennetun sovelluksen dataan.
Älä poista vanhaa asennusta ennen tietojen talteenottoa.

Huoltokirjan lähdekoodi, rakennettu konttikuva ja henkilötietoja sisältävä
sovellusdata eivät ole julkisia.
