## Ilmoituskeskuksen huoltomuistutukset (0.2.7)

1. Ota Keijukangas Ilmoituskeskus -integraatio käyttöön Home Assistantissa ja varmista, että `sensor.keijukangas_alerts` on käytettävissä.
2. Luo jokaiselle perheenjäsenelle oma Home Assistant -käyttäjä ja asenna Home Assistant Companion hänen puhelimeensa.
3. Avaa **Ilmoituskeskus** sivupalkista ja yhdistä jokaiselle käyttäjälle hänen `notify.mobile_app_...`-toimintonsa. Testaa kunkin vastaanottajan ilmoitus.
4. Avaa **Huoltokirja → Kohteet → huoltokohde → Huolto-ohjelma → Muokkaa**.
5. Valitse **Huoltomuistutusten vastaanottajat** -kohdassa henkilöt, joille kyseisen huollon muistutukset lähetetään.
6. Tallenna. Ilmoitus lähtee automaattisesti ennen huoltorajaa tai huollon erääntyessä; jo tallennetuille huolloille ei aseteta vastaanottajia automaattisesti.

**Rajat:** kalenterihuolloissa enintään 14 päivää ennen eräpäivää (lyhyissä päiväperusteisissa huoltoväleissä aikaisintaan neljäsosan huoltovälistä ennen), käyttömittariin perustuvissa huolloissa 10 h tai 20 käynnistyskertaa ennen rajaa. Ilmoitukset ovat tiedoksi-/varoitustasoa. Ilmoituskeskuksen kuittaus ei merkitse huoltoa suoritetuksi. Ainoastaan Huoltokirjaan tehty todellinen huoltomerkintä siirtää huoltorajaa. Puuttuva tai epäluotettava mittaustieto ei tarkoita nollaa. Jos HA/ilmoituskeskus ei vastaa, sovellus yrittää uudelleen seuraavalla tarkistuskerralla.

**Varmuuskopiot:** valitut vastaanottajat sisältyvät ZIP-vientiin ja -palautukseen. Hälytyshistoria kuuluu Home Assistantin varmuuskopioihin, ei Huoltokirjan ZIP-tiedostoon.

---

## Huoltokirjan tietojen manuaalinen vienti ja palautus

Home Assistantin koko järjestelmän varmuuskopiointi jää ensisijaiseksi varmuuskopiointitavaksi. Huoltokirjaan on lisätty vain **manuaalinen, siirrettävä ZIP-vienti ja ZIP-palautus**. Uutta ajastettua varmuuskopiointia, erillistä varmuuskopiohistoriaa tai tallennuspaikkaa ei toteuteta.

### Vie tiedot

Avaa Huoltokirja → **Raportit → Vie kaikki tiedot (ZIP)**. Vienti sisältää huoltokohteet, ohjelmat (myös päivä-/tuntivälit, työohjeet ja varaosat), suoritetut huollot, mittarilukemat, kustannukset, liitteiden metatiedot ja kaikki kuitit/kuvat/PDF:t. ZIP-tiedoston `huoltokirja.json` sisältää tietokannan sallitut taulut, `liitteet/` varsinaiset tiedostot ja `manifest.json` tarkistussummat. Vienti keskeytyy, jos jokin tietokannan mainitsema liite puuttuu tai sen koko poikkeaa tietokannasta; sovellus ei tee puutteellista ZIP-vientiä hiljaisesti.

### Palauta tiedot

1. Avaa Huoltokirja → **Raportit → Tuo tiedot ZIP-tiedostosta**.
2. Valitse Huoltokirjasta aiemmin viety ZIP (puhelimen Tiedostot-sovelluksesta tai tietokoneelta), enintään 23 Mt.
3. Paina **Tarkista ZIP-tiedosto**. Sovellus purkaa tiedot ainoastaan tarkistusta varten ja näyttää vanhojen sekä palautettavien huoltokohteiden, ohjelmien, merkintöjen, lukemien ja liitteiden määrät. Vahvistamattomassa vaiheessa ei muuteta nykyisiä tietoja.
4. Tarkista tiedoston sisältö. Palautus **korvaa kaikki nykyiset tiedot**; se ei yhdistä tai täydennä yksittäisiä kohteita. Valitse vahvistusruutu ja kirjoita **PALAUTA**. Paina **Korvaa Huoltokirjan tiedot**.
5. Kun onnistumisviesti näkyy, tarkista kohteet, huoltohistoria ja liitteet Huoltokirjassa.

Palautuksessa tarkistetaan ZIP:n tiedostopolut, symlinksuojaus, tiedostokoot ja määrät, liitteiden viittaukset, ZIP-tiedoston CRC, vientitiedoston SHA-256-tarkistussummat (jos manifesti on mukana), SQL-skeema sekä tietokannan viite-eheys. Vanhemmat Huoltokirjasta viedyt ZIP:t ilman `manifest.json`-tiedostoa hyväksytään, jos niiden sisältö on muuten ehjä. Tuntemattomat tai korruptoituneet ZIP:t hylätään. Järjestelmä tarkistaa myös, ettei Huoltokirjan sisältö ole muuttunut esikatselun ja vahvistuksen välissä.

Palautus luo uusille liitteille uudet sisäiset tallennusnimet ja kirjoittaa ne kokonaan levylle ennen tietokannan yksittäistä SQLite-transaktiota. Vanhoja liitteitä poistetaan vasta onnistuneen transaktion jälkeen. Tavallisen palautusvirheen tai epäonnistuneen tietokantapäivityksen yhteydessä vanhat tiedot ja liitteet säilyvät. Palautuksesta ei muodosteta erillistä ajastettua varmuuskopiota: **ota tarvittaessa ZIP-vienti ja varmista Home Assistantin varmuuskopio ennen vanhojen tietojen korvaamista**.

**Rajoitukset:** ZIP max 23 Mt (mobiilin JSON-siirtorajan vuoksi); purettu sisältö max 80 Mt; yksittäinen liite max 5 Mt. Suuremmat kokonaisuudet palautetaan tarvittaessa Home Assistantin sovellusvarmuuskopiosta. Yksittäisen auton tai laitteen huoltohistorian yhdistäminen nykyiseen tietokantaan ei kuulu tähän versioon. Älä tallenna ZIP-vientiä julkiseen GitHub-repositorioon, koska se voi sisältää henkilötietoja, kuitteja ja tunnisteita.

---

## Huolto-ohjelmien tuonti ja mobiilikäyttö (uusi)

Kaikille kohteille: **Kohteet → [oma kohde] → Huolto-ohjelma → Tuo ohjelma**. Valitse jokin kolmesta tavasta:

1. Valitse puhelimen tiedostoista **CSV** tai **JSON**.
2. Jos tiedostovalitsin ei toimi Home Assistant -sovelluksen sisällä, kopioi tiedoston teksti ja liitä se **CSV/JSON-teksti**-kenttään.
3. Valitse **Bosch Climate Class 6101i**- tai **Husqvarna R 214TC 2024** -mallipohja. Nämä ovat vapaaehtoisia esimerkkiohjelmia, eivät automaattisesti lisättäviä huoltoja. Tarkista laitemalli ja valmistajan huolto-ohjeet.

Valitse **Esikatsele huollot**. Esikatselussa valitaan halutut rivit ja täydennetään huollon seuraavat **todelliset** määräpäivät sekä mittari- ja käynnistysrajat. Kunkin rivin **Muokkaa muita huoltotietoja** -osiosta voidaan muuttaa myös nimi, työohje, varaosat, huollon tekijä sekä huolto-ohjeen URL. Virheelliset rivit ilmoitetaan, eikä niitä voi tallentaa ennen korjaamista tai poisvalintaa. **Tuo valitut huollot** tallentaa kaikki valitut rivit yhtenä kokonaisuutena; saman kohteen samannimiset ohjelmat ohitetaan, eikä vanhoja tai jo suljettuja huoltoja muuteta.

### CSV-sarakkeet

Vanha 10-sarakkeinen CSV on edelleen yhteensopiva. Uudessa mallipohjassa ovat entisten lisäksi **interval_days**, **instructions**, **parts**, **service_by** ja **source_url**. Tuonti hyväksyy myös suppean CSV:n, jossa on vähintään otsakkeet `title` ja `mode` sekä tarvittavat huoltorajat.

```csv
title;mode;interval_days;next_due_date;service_by;instructions;parts;source_url
Suodattimien puhdistus;interval;14;2026-11-30;self;Katkaise virta ennen suodattimen puhdistusta;Harja;https://example.com/ohje
Ammattilaisen tarkistus;interval;730;2028-11-30;professional;Huolto ammattilaisen toimesta;;
```

- `interval_days` tai `interval_months`: toistuvuus päivinä tai kuukausina. Päivä- ja kuukausiväli ovat toisensa poissulkevia. Vastaava `next_due_date` on annettava.
- `interval_meter` (ja `meter_unit` = `km` tai `h`) sekä `next_due_meter`: huoltoväli ja seuraava mittariraja. Voi yhdistää päivään tai kuukauteen.
- `interval_starts` ja `next_due_starts`: käynnistyskertojen rajat.
- `mode` = `interval` (toistuva) tai `fixed` (kiinteä eräpäivä); `one_time` = `1` vain kertaluonteisille toistuva-tyyppisille huolloille.
- `instructions`: huollon työohje, enintään 5 000 merkkiä.
- `parts`: varaosat/tarvikkeet, enintään 2 000 merkkiä.
- `service_by`: `self` (omatoiminen), `professional` (ammattilainen), `either` (kumpi tahansa).
- `source_url`: valmistajan ohjeen http/https-osoite.

**Huom.** Valmiissa mallipohjissa ei arvata puuttuvaa huoltopäivää. Boschin 14 päivän suodatinpuhdistus on nyt oikeasti toistuva: huoltohetken päivästä lasketaan automaattisesti seuraava 14 päivän määräpäivä. Husqvarnan 50 h / 100 h / 200 h -rajoja pitää verrata todellisiin käyttötunteihin. Mallipohjan valmistajan tai tämän sovelluksen ehdottamat huoltovälit on tarkistettava oikeasta käyttöohjeesta ennen käyttöönottoa.

JSON-tuonnissa käytetään objektia `{"plans":[{...}]}` tai suoraan ohjelmaolioiden taulukkoa. Kenttien nimet ovat samat kuin CSV:ssä. Molemmissa muodoissa max 256 kt / 250 huoltoa. Tuonti ei koskaan tee huoltomerkintöjä eikä yliaja vanhoja merkintöjä.

### Käyttömittarit

Käyttömittarin yhdistäminen näytetään vain **Laite**-kategorian tarpeellisissa näkymissä. Etusivun, yleisen navigaation ja muiden kohteiden toistuvat P1-ilmoitukset on poistettu; yhteys ja sen virhetilanne näytetään erillisessä **Käyttömittarit**-näkymässä. Itse mittaritietojen keräystä, vanhoja linkityksiä ja varmistuksia ei poisteta.

---

## Huolto-ohjelmien tuonti CSV-tiedostosta

Kaikilla huoltokohteilla (Autot, Kiinteistö, Työkoneet ja Laitteet) on Huolto-ohjelma-välilehdellä **Tuo ohjelma** -painike. Lataa samasta ikkunasta **CSV-mallipohja** ja täytä tiedosto Excelissä, LibreOfficessa tai tekstieditorissa. Käytössä ovat UTF-8-merkistö, puolipiste- tai pilkkuerotin ja lainausmerkeillä ympäröidyt kentät. Mallin otsakerivi on pakollinen; järjestys voi vaihdella, mutta kaikkien otsakkeiden täytyy olla mukana.

```csv
title;mode;interval_months;interval_meter;meter_unit;next_due_date;next_due_meter;interval_starts;next_due_starts;one_time
Moottoriöljyn vaihto;interval;12;100;h;2027-03-01;150;;;0
Kertaluonteinen ensihuolto;interval;;50;h;;50;;;1
Katsastus;fixed;;;;2027-09-01;;;;0
```

Rivit ovat **vain esimerkkejä**, eivät oletuksena asennettavia huoltoja. Päivämäärät ja seuraavat mittarirajat annetaan oman kohteen todellisten huoltojen ja mittarilukemien mukaan.

- `title`: toimenpiteen nimi (enintään 150 merkkiä); `mode`: `interval` eli toistuva tai `fixed` eli kiinteä eräpäivä.
- `interval_months` + `next_due_date`: toistuvuus kuukausissa sekä seuraava todellinen eräpäivä (VVVV-KK-PP).
- `interval_meter` + `meter_unit` + `next_due_meter`: mittariväli, `km` tai `h`, ja seuraava todellinen mittariraja. Käytettävissä yhtä aikaa kuukausivälin kanssa.
- `interval_starts` + `next_due_starts`: tarvittaessa käynnistyskertojen huoltoväli ja seuraava raja.
- `one_time`: `1` kertaluonteiselle ensihuollolle (`interval`-tila), muuten `0`. Kertaluonteinen huolto suljetaan vasta, kun se kirjataan suoritetuksi.

Maksimi 250 toimenpidettä ja 256 kt per CSV. CSV tuodaan **vain valittuun kohteeseen**; muiden kohteiden huolto-ohjelmiin ei kosketa. Jos toimenpiteen nimi löytyy jo samalta kohteelta (myös suljetusta ohjelmasta), se ohitetaan. Vanhat huolto-ohjelmat, niiden rajat, huoltohistoria ja kuitit säilytetään. Virhe yhdessäkin tuontirivissä peruuttaa kaikki saman tiedoston lisäykset. Tuonti ei kirjaa yhtään huoltoa tehdyksi.

Aiemmin tuodut Husqvarna R 214TC -huollot säilyvät ennallaan, mutta sovellus ei enää lisää niitä automaattisesti uudelleenkäynnistyksen yhteydessä eikä näytä mallikohtaista tuontipainiketta.

### P1-käyttömittarit

P1-käyttömittarin voi yhdistää **vain Laite-kategorian** kohteeseen. Muissa kategorioissa P1-yhdistämispainikkeet, yhdistämisviestit ja P1-mittariarvon näyttö on piilotettu. P1-laitteiden valintaikkuna näyttää vain Laite-kohteet, ja palvelin estää uusien linkitysten tekemisen muihin kategorioihin. Mahdollisia ennestään tallennettuja linkityksiä ei poisteta automaattisesti.

---

## Husqvarna R 214TC (vuosimalli 2024) – virallinen huolto-ohjelma

Huoltokirja sisältää Husqvarna R 214TC:n vuoden 2024 käyttöohjeen (1686-005, 02.12.2024, s. 89–91) mukaisen **käyttötuntiperusteisen** huolto-ohjelman. Huoltokohde käyttää tunnuksia **h**, ei kilometrejä. Huolto-ohjelma sisältää 25 yksilöityä tarkastusta/vaihtoa: kertaluonteiset 50 h ensihuollot, toistuvat 100 h määräaikaishuollot ja vaihteistoöljyn 200 h huollon. AWD-mallin tehtäviä ei lisätä R 214TC:lle. Valmistajan huoltoliikkeelle osoitetut toimenpiteet on merkitty.

- Aiemmin automaattisesti lisätyt R 214TC -huolto-ohjelmat säilyvät, mutta uusi tuonti tapahtuu yleisen **Tuo ohjelma** -toiminnon kautta.\n- Tuonti on idempotentti: samannimistä huolto-ohjelmaa ei lisätä uudelleen, eikä olemassa olevien suunnitelmien rajalukemia, huoltohistoriaa, kuitteja tai mittarilukemia muuteta. Aiemmin suljettuja huoltoja ei avata.
- **50 h ensihuollot ovat kertaluonteisia:** ne suljetaan vasta, kun käyttäjä kirjaa todellisen huollon tehdyksi. Täytetyt ensihuoltorivit eivät toistu.
- **Moottoriöljy ja suodatin:** valmistaja määrää 50 h tai 1 vuoden ensihuollon ja sen jälkeen 100 h tai vuosittaisen huollon. Käyttötuntirajat voidaan tuoda, mutta **vuosipäivää ei arvata** koneen iästä, valmistusvuodesta eikä päivityspäivästä. Lisää eräpäivä Huolto-ohjelman Muokkaa-toiminnolla viimeisimmän oikean huoltopäivän tai hankintapäivän perusteella.
- Koneen tähänastinen käyttötuntimäärä ei ole Home Assistantista automaattisesti saatavilla ilman siihen erikseen kytkettyä anturia. Syötä sen mekaanisesta tuntimittarista todellinen lukema sekä tarvittaessa muokkaa tulevat rajat viimeksi tehtyjen huoltojen mukaan. Tuonti **ei luo valheellisia huoltomerkintöjä**.

**Ennen jokaista käyttöä:** tarkista öljy- ja polttoainevuodot, moottorin ja voimansiirron öljymäärä, rengas- ja jarrutoiminnot, ohjausvaijerit, turvakytkimet ja terät. Puhdista leikkuulaite, jäähdytysilmanotto ja tarvittaessa äänenvaimentimen ympäristö. Nämä ovat käyttöä edeltäviä tarkistuksia, eivät tuntivälillä erääntyviä huoltorivejä.

Valmistajan suomenkielinen [käyttöohje (PDF)](https://www-static-nw.husqvarna.com/hbd/tdrdownload/v2/pub000091722/doc000221919/OM/jnBHnXacqesICn3ZV7zCegUpeVU?httproute=True), huoltokaavio sivuilla 89–91.

---

## P1-yhteys ja virhetilanteet (0.2.6)

Huoltokirja lukee P1-tunnistimen entiteetistä `sensor.p1_load_analytics` Supervisorin Core API -välityksellä. Lisäosa tarvitsee `homeassistant_api: true` ja Python-prosessin käyttöön Supervisorin ympäristömuuttujan `SUPERVISOR_TOKEN` (tai vanhan `HASSIO_TOKEN`-nimen). S6-käynnistyksessä ympäristömuuttujat otetaan käyttöön komennolla `/command/with-contenv`.

Jos P1-laitteiden luettelo ei lataudu, **Käyttömittarit**-sivu näyttää tarkemman selityksen: puuttuva API-tunniste, HTTP 401/403 (oikeusongelma), HTTP 404 (anturi puuttuu), yhteysongelma tai vanhentunut P1-mittaus. Lokissa näkyy virheen luokka tai HTTP-koodi, ei tunnistetta. P1:n mittaustietojen puuttuminen ei poista vanhoja huolto- eikä käyttötietoja.

Päivitä Huoltokirjan sovelluskaupan versio automaattijulkaisun jälkeen ja käynnistä Huoltokirja uudelleen. P1-tunnistinta ei tarvitse asentaa uudelleen.

---
## P1-käyttötunnit ja huollettavat laitteet

P1-tunnistin **0.5.3** jatkaa käyttötuntien ja arvioitujen käynnistysten laskentaa, mutta huoltovälit ja huoltomerkinnät tallennetaan vain Huoltokirjaan.

1. Avaa Huoltokirja → **Käyttömittarit** tai **Kohteet → P1-laitteet**.
2. Valitse laite ja yhdistä se olemassa olevaan huoltokohteeseen. Luo uusi `Laite`-kohde vain, ellei kohdetta jo ole.
3. Huoltokirja lukee P1-tiedot Home Assistantin `sensor.p1_load_analytics`-entiteetin kautta. Vain tuoreet onnistuneet P1-mittaukset tallennetaan. Kun P1 on poissa käytöstä, viimeisin vahvistettu lukema säilyy ja merkitään vanhentuneeksi.
4. Huoltokirjan **Huolto-ohjelma** -kohdassa voit valita käynnistysvälin, käyttötuntivälin tai kalenterivälin. Erääntyminen lasketaan automaattisesti kytketyn P1-laitteen käyttötuntien ja käynnistyskertojen mukaan. P1-käynnistykset ovat tunnistuksen arvioita; releen OFF→ON-siirtymä on vahvempi havainto.
5. Huolto kirjataan vain **Kirjaa huolto** -toiminnolla. Tuore P1-lukema ehdotetaan lomakkeeseen. Sen voi korjata huoltohetken todelliseen lukemaan.

P1:ssä aiemmin tallennetut huoltorajat ja huoltomerkinnät siirretään yhdistämisen yhteydessä kerran Huoltokirjaan, mutta jo olemassa olevia kohteita tai saman laitteen aktiivisia vastaavia tunti-/käynnistysohjelmia ei kopioida. Alkuperäinen P1 SQLite säilyy varmistusta ja tarvittaessa tarkistusta varten; enintään P1-raportin saatavissa olevat 500 viimeisintä aiempaa merkintää voi siirtää automaattisesti. Muut vanhat merkinnät jäävät alkuperäiseen P1-tietokantaan.

**Huom.** Automatisoidut tunnit kuuluvat vain yhdistetyille laitteille. Autonlämmitys jätetään P1-työajanseurannan ulkopuolelle, mutta autojen tavallinen Huoltokirja toimii entiseen tapaan. Tämä lukuoikeus edellyttää Home Assistantin `homeassistant_api: true` -asetusta. P1:n ja Huoltokirjan tietokantoja ei yhdistetä fyysisesti eikä sähkölaitteita ohjata.

---

# Huoltokirja – käyttöohje

## Ensimmäinen käyttöönotto

1. Käynnistä Huoltokirja Home Assistantin **Sovellukset**-näkymästä ja avaa sen käyttöliittymä.
2. Valitse **Kohteet → Lisää kohde** ja lisää autot yksitellen. Myös toinen tai kolmas auto on aina oma kohteensa.
3. Lisää tarvittaessa kiinteistö, työkoneet ja muut huollettavat laitteet.
4. Avaa kohde ja lisää sille huolto-ohjelma. Esimerkiksi **Öljynvaihto: 12 kuukautta tai 15 000 km**.
5. Lisää katsastukset tai muut tarkat määräpäivät kiinteinä huolto-ohjelmina.
6. Huollon jälkeen valitse **Kirjaa huolto**, anna päivämäärä ja tarvittaessa käsin luettu kilometrilukema/käyttötunnit sekä kuitit.

Huolto-ohjelma, historia ja mittarilukema kuuluvat aina valittuun kohteeseen. Eri autojen tiedot eivät sekoitu keskenään.

## Huoltomuistutukset

- **Toistuva huolto:** huolto voidaan määritellä kuukausi- ja/tai mittarivälin mukaan. Kirjattu huolto siirtää seuraavaa määräaikaa eteenpäin.
- **Kiinteä määräpäivä:** esimerkiksi katsastus. Kun se merkitään tehdyksi, anna seuraava määräpäivä käsin. Ellei sitä anneta, huolto-ohjelma suljetaan odottamaan uutta määräpäivää.
- **Mittarilukema:** lisätään käsin huollon yhteydessä; sovellus ei lue ajoneuvojen tai työkoneiden mittareita automaattisesti.

## Kuitit ja tiedot

Huoltomerkintään voi lisätä enintään neljä kuvaa tai PDF-tiedostoa (kukin enintään 5 Mt). **Raportit**-näkymästä saa ZIP-viennin huoltotiedoista ja liitteistä. ZIP-viennin voi palauttaa Raportit-sivun **Tuo tiedot ZIP-tiedostosta** -toiminnolla, joka korvaa kaikki nykyiset tiedot vahvistuksen jälkeen.

Sovelluksen huoltotiedot säilytetään `/data`-kansiossa ja kuuluvat Home Assistantin sovellusvarmuuskopioihin. Älä julkaise ZIP-vientipakettia GitHubissa: siinä voi olla henkilötietoja, rekisterinumeroita ja kuitteja.

## Käyttäjät ja tietoturva

Sovellus käyttää Home Assistantin kirjautumista ja avautuu vain sen Ingress-välityksen kautta. Perheenjäsenet näkevät samat kohteet ja voivat kirjata huoltoja, jos he pääsevät avaamaan sovelluksen. Erillisiä luku- ja muokkausoikeuksia ei ole vielä toteutettu.

## Kehittäminen

Lähdekoodi, asennusohjeet ja testauskomennot ovat [GitHub-repositorion etusivulla](https://github.com/dimi-alt/huoltokirja).
