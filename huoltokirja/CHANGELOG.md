## Huoltokirja 0.2.15 – yhtenäinen julkaisuversion hallinta

- Julkaisun versionumero tulee jatkossa lähdekoodin yhdestä `huoltokirja/config.yaml`-tiedostosta.
- GitHub Actions julkaisee tällä samalla versiolla konttikuvat ja Home Assistantin sovelluskaupan tiedot.
- Julkaisu estetään, jos versio ei ole suurempi kuin viimeksi julkaistu.
- Julkaisun ja muutoshistorian versionumerot tarkistetaan automaattisesti.

## Huoltokirja 0.2.8 – ilmoitusten luotettavuus

- Huoltomuistutusten tilat vertaillaan ennen lähettämistä: muuttumattomista huolto-ohjelmista ei enää kutsuta Ilmoituskeskusta turhaan 15 minuutin välein. Kuuden tunnin välein tehdään varmistava uudelleensynkronointi, jotta mahdollisen HA-varmuuskopion palautuksen jälkeen puuttuvat aktiiviset muistutukset saadaan takaisin.
- Puhelin- ja Home Assistant -verkkokutsuja ei tehdä Huoltokirjan tietokantalukituksen sisällä. Tavalliset käyttäjät voivat siis käyttää Huoltokirjaa ilman, että hidas ilmoituspalvelu lukitsee tietokannan.
- Uusi huoltomerkintä tai huolto-ohjelman muutos käynnistää taustatarkistuksen välittömästi eikä vasta seuraavan 15 minuutin ajastuksen jälkeen.
- Vanhentuneita P1-käyttömittaustietoja (yli 180 sekuntia) ei käytetä uusiin tunti- tai käynnistysrajahälytyksiin. Kalenteriperusteinen eräpäivä toimii normaalisti.
- Vanhojen asennusten `plan_notify_active`-taulukko päivitetään automaattisesti. Käyttäjien valitsemat vastaanottajat ja aiemmat huoltomerkinnät säilyvät.
- Ilmoituskeskuksen yhteisen anturisensorin yksityisiä laitetunnisteita ei enää lueta: Huoltokirja käyttää sen julkaisemaa vastaanottajakohtaista laitemäärää.

## P1-laitelistan hallinta

- Piilota tai palauta P1-laitteita Huoltokirjan luettelosta.
- Pura P1-yhdistäminen poistamatta huoltokohteen historiaa, ohjelmia tai liitteitä.
- Listaa vain opetetut tai kytkinlähteeseen yhdistetyt laitteet ja nykyiset linkit.
- Piilotukset säilyvät SQLite-tietokannassa ja ZIP-varmuuskopioissa.

## Huoltokirja 0.2.7 – Ilmoituskeskus-yhteys

- Huolto-ohjelmille voi valita yksittäisiä Home Assistant -käyttäjiä muistutusten vastaanottajiksi.
- Ilmoitukset lähetetään Keijukangas Ilmoituskeskuksen HA-rajapinnan kautta, jos integraatio on asennettu ja puhelimet on rekisteröity.
- Automaattinen taustatarkistus joka 15. minuutti toimii ilman avointa Huoltokirja-näkymää.
- Määräpäivien muistutukset alkavat enintään 14 vrk ennen huoltoa (lyhyissä päivähuoltoväleissä myöhemmin); käyttötuntien 10 h ja käynnistysten 20 kertaa ennen rajaa.
- Erääntyessä lähetetään erillinen uusi hälytys; huollon valmistuessa, sulkeutuessa tai siirtyessä seuraavaan huoltojaksoon vanha hälytys päätetään.
- Ilmoituksia ei oletuksena lähetetä kenellekään. Vanhat huolto-ohjelmat ja historia säilyvät.
- Käyttäjävalinnat sisältyvät Huoltokirjan ZIP-vientiin ja palautukseen.

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

## Huoltokirja 0.2.6 – P1-yhteyden korjaus

- Korjattu S6-käynnistyksen ympäristömuuttujat: `SUPERVISOR_TOKEN` välittyy Python-prosessille `with-contenv`-kääreen kautta.
- P1-yhteysvirheet yksilöidään Huoltokirjan käyttöliittymässä ja lokeissa (puuttuva tunniste, 401/403, 404, aikakatkaisu, virheellinen vastaus) ilman tunnisteiden tulostamista.
- P1 REST -luku odottaa enintään 8 sekuntia; vanhentuneet lukemat eivät korvaa viimeistä vahvistettua huoltomittarilukemaa.
- Huoltokirjan tietokanta ja olemassa olevat huoltohistoriat säilyvät ennallaan.

## 0.2.4 – P1-käyttötunnit Huoltokirjassa

- Lue Home Assistantin P1-käyttömittari turvallisesti Supervisorin Core REST -rajapinnasta. Tarvitaan `homeassistant_api: true`.
- Yhdistä P1-laite aiempaan huoltokohteeseen tai luo uusi laitekohde. Älä kopioi autoja tai autonlämmitystä.
- Tallenna viimeisin vahvistettu käyttötunti-/käynnistyslukema, erota vanhentunut mittaus tuoreesta.
- Huoltovälit kuukausina, käyttö-/käyntitunteina ja käynnistyskertoina. P1:n tuore lukema voidaan käyttää huollon mittarilukemana.
- Tuo olemassa olevat P1-huoltomerkinnät ja huoltorajat kerran. Huoltokirja on ainoa uusien huoltojen tallennuspaikka.
- Vanhojen autojen huoltotiedot, kuitit ja huoltosuunnitelmat säilyvät.

# Muutoshistoria

## 0.1.0

- Ensimmäinen Home Assistant OS -versio Ingress-käyttöliittymällä.
- Useita autoja ja muita huoltokohteita, omat suunnitelmat ja historiat.
- Toistuvat aika- ja mittariperusteiset huollot sekä kiinteät määräpäivät.
- Huoltomerkintöjen kustannukset ja liitteet, yhteenveto ja vienti.
