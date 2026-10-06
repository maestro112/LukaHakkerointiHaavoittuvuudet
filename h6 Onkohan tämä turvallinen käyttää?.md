# h6 Onkohan tämä turvallinen käyttää?

## Testiympäristö

* Oracle VirtualBox

  * Version 7.2.4 r170995 (Qt6.8.0 on Windows)
* Kali Linux Debian 64bit

  * 6 ydintä
  * RAM 9100 MB
  * HDD 50 GB

---

## Tehtävä

* Elikkä tässä tehtävässä tutkin Tapo C200 -kameran ohjelmiston turvallisuutta sekä yritin löytää sen root passwordin.

* Aloitin tehtävän lataamalla Moodlesta kaikki tarvittavat tiedostot ja purkamalla ne. Seuraavaksi annoin **make**-komennon lähdekoodin kääntämiseksi. Tämän jälkeen ajoin preinstall-ohjelman, jotta kaikki tarvittavat riippuvuudet ja muut työkalut löytyisivät.

<img width="384" height="85" alt="image" src="https://github.com/user-attachments/assets/81c2d34d-3f49-4975-afa8-0a795b813854" />

* Seuraavaksi lähdin purkamaan ohjelmistoa tp-link-decryptin avulla. Ohjelma tulosti avaimen, joka näytti olevan näkyvissä melko helposti.

<img width="501" height="301" alt="image" src="https://github.com/user-attachments/assets/2ae19e16-b6ea-40dd-acd6-6f09112a6a87" />

* Tämän jälkeen tiedostosta oli ilmestynyt uusi versio.

<img width="228" height="27" alt="image" src="https://github.com/user-attachments/assets/b4b62576-4623-4352-922a-fa0d578c61ec" />

* Seuraavaksi ajoin tälle uudelle tiedostolle **strings**- ja **binwalk**-komennot. Molempien tulosteissa oli todella paljon erilaista tietoa, joten niiden tarkastelu sellaisenaan oli melko hankalaa. Kysyin seuraavaksi Claudelta hieman apua siitä, mikä tulosteessa näyttäisi olevan tärkeää. Se huomautti, että binwalkin tulosteen pohjalla näkyi laitteen käyttöjärjestelmän tiedostorakenne.

<img width="901" height="30" alt="image" src="https://github.com/user-attachments/assets/ce727a48-6507-4ce6-91df-5ed631d01d86" />

* Yritin seuraavaksi lähteä purkamaan tätä kohtaa **binwalk -e**-komennolla. Tämän jälkeen tulostui uusi purettu hakemisto. Löysin täältä **squashfs-root**-hakemiston ja sen sisältä vielä **bin/main**-tiedoston. Kokeilin ajaa strings-komennon myös sille, mutta tulosteessa oli niin paljon tavaraa, että siitä oli vaikea löytää mitään olennaista.

<img width="844" height="454" alt="image" src="https://github.com/user-attachments/assets/4b85599a-98cd-4676-be5a-0c397b21558b" />

<img width="673" height="64" alt="image" src="https://github.com/user-attachments/assets/d7759378-9153-4317-95e1-a1154e219cf5" />

* Lähdin siis seuraavaksi käyttämään grep-komentoa suodattamaan tulosta yleisillä hakusanoilla, kuten salasanaan tai käyttäjätunnuksiin liittyvillä sanoilla.

<img width="715" height="42" alt="image" src="https://github.com/user-attachments/assets/6d1a74a4-fccb-4fca-9d3c-7928fa2eaf23" />

* Löytyi monta eri kohtaa, mutta en usko, että salasana löytyy näin helposti. Löysin esimerkiksi tämän kohdan, jossa näyttäisi olevan jonkinlainen tarkistuskohta, mutta en itse löytänyt siitä mitään selvää haavoittuvuutta tai tapaa, jolla sitä voisi hyödyntää.

<img width="840" height="256" alt="image" src="https://github.com/user-attachments/assets/a38479b3-e658-4fec-a038-b22ba70391a2" />

* Nyt monen tunnin analysoinnin jälkeen uskon, että tästä kohdasta ei löydy helposti mitään hyödyllistä. Lähdin seuraavaksi purkamaan dump-tiedostoa komennolla **binwalk -e dump-tapo-c200v3-1.4.2.bin**. Sieltä löytyi jälleen sama **squashfs-root**-tiedosto.

---

## loppu ajatukset

* Yritin myös käyttää Ghidraa saadakseni selville salasanan tai löytääkseni mahdollisia haavoittuvuuksia. Ghidralla löytyi paljon erilaista tietoa ohjelman toiminnasta, mutta en onnistunut löytämään selkeää kohtaa, josta root password olisi ollut suoraan löydettävissä.

* Tehtävän aikana opin kuitenkin paljon laitteen firmware-ohjelmiston tutkimisesta. Sain purettua ohjelmiston eri osiin, tarkasteltua tiedostorakennetta sekä tutkittua binääritiedostoja esimerkiksi **strings**-, **grep**- ja **binwalk**-komennoilla. Lisäksi kokeilin analysoida binääriä Ghidralla. Vaikka en saanut root passwordia selville, sain mielestäni hyvän käsityksen siitä, millaista tietoa laitteen firmwaresta voidaan löytää ja miten sitä voidaan lähteä tutkimaan.

* Analyysin perusteella en myöskään löytänyt mitään sellaista, jonka perusteella voisin varmasti sanoa, että laitteessa olisi helposti hyödynnettävä haavoittuvuus. Tämä ei tietenkään tarkoita, etteikö haavoittuvuutta voisi olla olemassa, vaan ainoastaan sitä, etten onnistunut löytämään tai todentamaan sellaista tämän analyysin aikana.

* Lopulta päätin lopettaa analyysin tähän, koska en enää löytänyt uusia selkeitä tutkimussuuntia. En myöskään halunnut tehdä pelkkiä arvauksia siitä, mikä voisi olla salasana tai haavoittuvuus ilman, että pystyisin todentamaan asian.

---

### Arvioijalle

* Kertokaa ihmeessä, jos saitte tämän tehtävän aikana root passwordin selville. Olisi kiinnostavaa kuulla, olinko edes vähän oikeilla jäljillä vai meninkö täysin väärään suuntaan. Kiitos!

## Lähteet
- tp-link-decrypt robbins 2026 Luettavissa:https://github.com/robbins/tp-link-decrypt Luettu: 6.10.2026
- Rooting the TP-Link Tapo C200 Rev.5 2025 Luettavissa:https://quentinkaiser.be/security/2025/07/25/rooting-tapo-c200/ Luettu: 6.10.2026
- **Claude Sonnet 5 apuna**
