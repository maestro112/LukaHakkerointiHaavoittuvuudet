# h2 Break & Unbreak

## Testiympäristö
VirtualBox kali linux 

Selain: FireFox

---
## Tiivistelmät
* **Karvinen 2006: Report Writing (in Finnish)**
  * Raportin täytyy olla täsmällinen ja tekstin helppolukuista.

* **Find Hidden Web Directories - Fuzz URLs with ffuf**
   * Ei halutut vastaukset täytyy filtteröidä pois tässä tapauksessa koon (bytes) mukaan.
   * kaikki fuff parametrit näkee komennolla ./fuff

* **A01:2021 – Broken Access Control**
  * Broken Access Control tarkoittaa käyttöoikeuksien hallinnan puutteita, joiden avulla käyttäjä voi päästä käsiksi tietoihin tai toimintoihin, joihin hänellä ei pitäisi olla oikeuksia.
  * Sovelluksessa tulisi käyttää keskitettyjä käyttöoikeus käytäntöjä sekä varmistaa, että käyttäjä voi käsitellä vain omia tietojaan ja suorittaa heille sallittuja toimintoja. 

* **Access control vulnerabilities and privilege escalation**
  * Pääsynvalvonta säätelee pysty-, vaaka- ja kontekstiriippuvaisesti sitä, mitä toimintoja ja resursseja kukin käyttäjä saa käyttää.
---
## Tehtävät 

### a) 

- Testiympäristön pystyyn saadessani aivan ensimmäisenä kokkeilin itse http linkkiin erilaisia asioita kuten /admin/ toivoen että jotain tapahtuisi. Yllätys mitään ei tapahtunut. 

- Seuraavaksi vaihdoin lähestymis tapaani. Koska kenttään ei voinut syöttää kuin numeroita lähdin leikkimään sivun html koodin kanssa. Löysin kohdan input **type=”number”**. Kokeilin poistaa koodista number kohdan eli jäljelle jäi **input type=” ”**. Tämän jälkeen kun kokeilin kirjoittaa kenttään **-** vastaukseksi tuli **(not found)** eikä sivu herjannut siitä että kentässä ei ollut numeroa. 

<img width="257" height="47" alt="image" src="https://github.com/user-attachments/assets/efd0f9b6-e62a-48f9-a8c8-4d1c165279fb" />


- Kokeilin seuraavaksi ‘ Select *  jolloin koko sivu meni rikki. 

<img width="948" height="129" alt="image" src="https://github.com/user-attachments/assets/fe149424-25e7-43bb-aeba-253acfb2511f" />

- Seuraavaksi kokeilin “klassista” ‘ OR 1=1– jolloin salasana kohtaan tulostui foo.

<img width="740" height="241" alt="image" src="https://github.com/user-attachments/assets/42c8c18b-621b-4295-b7d0-5fc4b18ed25e" />


- Tämän jälkeen en saanut muita tuloksia enkä osannut liikkua eteenpäin nykyisellä osaamisellani joten käännyin vinkkien puoleen. Tämänkään jälkeen en oikein osannut liikkua eteenpäin joten käännyin Robin Niinemets 2024: h2 - Break and Unbreak (solution) puoleen. 

- Robinin ratkaisua ihmetellessä olin iloinen nähdessäni että olin sentään edes hieman oikealla polulla. Ymmärsin myös hyökkäyksen logiikkaa paremmin kun katselin Robinin luomaa taulukkoa. Kävin vielä itse kokeilemassa Robinin vastausta PIN kenttään. 

<img width="1038" height="354" alt="image" src="https://github.com/user-attachments/assets/23f8e096-11fe-49e8-99b7-60341b9058b4" />

---
### b)

- Korjausta miettiessäni aloin epäilemään että käyttäjältä tulevaa syötettä ei suodateta mitenkään. Epäilin näin löytämäni koodipätkän vuoksi ja siksi että se on hyvin yleinen syy SQL injektioissa.

<img width="479" height="20" alt="image" src="https://github.com/user-attachments/assets/7f6c74f5-dd6d-41e3-b22d-f50bbed5384b" />


- Eli käyttäjältä tuleva syöte pitäisi filtteröidä. En osannut tähänkään luoda omaa vastausta joten katsoin taas Robinilta apua. Kokeilin tämän jälkeen itse lisätä Robinin näyttämät korjaukset sovelluksen koodiin jonka jälkeen kyseinen hyökkäys tapa ei enään onnistunut.

<img width="1031" height="348" alt="image" src="https://github.com/user-attachments/assets/255865a0-76d2-4fce-8776-67cef930de76" />

---
### c

- Kun olin saanut harjoitus sivuston toimimaan lähdin heti ensimmäisenä kokeilemaan tunnilla esiteltyä ffuf:ia. Latasin **Find Hidden Web Directories** ohjeista common.txt tiedoston jolla lähdin kokeilemaan. 

<img width="410" height="18" alt="image" src="https://github.com/user-attachments/assets/0232a268-8cae-4104-b274-5cf855fb8f68" />


- Tämän jälkeen sain vastaukseksi. 

<img width="734" height="21" alt="image" src="https://github.com/user-attachments/assets/f7e3a215-fbf9-4daa-8557-528ae23011c1" />


- Lähdin heti tämän jälkeen kokeilemaan hakukentään osoitetta http://127.0.0.1:8000/admin-console/ mutta mitään ei tapahtunut. Kokeilin tämän jälkeen luoda käyttäjän ja kirjautua sisään jonka jälkeen syötin hakukenttään taas  http://127.0.0.1:8000/admin-console/ ja boom salainen sivu löytyi. 

<img width="1076" height="491" alt="image" src="https://github.com/user-attachments/assets/b35e78e7-987c-4771-8d2f-688e84a2386c" />

---
### d)

- Lähtiessäni korjaamaan tätä ongelmaa uskoin että näin tapahtui koska minun käyttäjäni oikeuksia ei tarkastettu kunnolla joten pääsin admin secret sivulle.

- Selasin jonkin aikaa eri python tiedostoja kunnes päädyin **/020-your-eyes-only/logtin/hats/views.py** tiedostoon. Täällä tämä kohta iski silmiini. Tässä kohtaa selvästikkin tarkistetaan käyttäjän oikeuksia.

<img width="689" height="332" alt="image" src="https://github.com/user-attachments/assets/6696f6d3-986f-41f7-86c8-13236843083d" />


- Huomasin että **AdminShowALLView** kohdassa näyttäisi puuttuvan oikeuksien tarkastus ja katsotaan vain että kyseessä on käyttäjä.

<img width="669" height="158" alt="image" src="https://github.com/user-attachments/assets/1c46a725-9d80-405b-a587-949bd04ade1e" />


- Tämän jälkeen ainut asia mitä oikeastaan tein oli että kopioin ylemmästä koodi pätkästä lopun ja lisäsin sen alempaan.

<img width="693" height="174" alt="image" src="https://github.com/user-attachments/assets/ef4020ac-68f4-4c56-8901-41ed0901784b" />


- Tämän jälkeen en päässyt enään sivulle.

<img width="1074" height="236" alt="image" src="https://github.com/user-attachments/assets/f5f6e3d2-81af-4a66-b955-3cd2654937a1" />

## Lähteet

A01:2021 – Broken Access Control Luettavissa:https://owasp.org/Top10/2021/A01_2021-Broken_Access_Control/index.html Luettu: 1.9.2026

Karvinen 2023 Find Hidden Web Directories - Fuzz URLs with ffuf Luettavissa:https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/ Luettu: 1.9.2026

PortSwigger Access control vulnerabilities and privilege escalation Luettavissa:https://portswigger.net/web-security/access-control Luettu: 1.9.2026

Karvinen 2006: Report Writing Luettavissa:https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ Luettu: 1.9.2026

Robin Niinemets 2024: h2 - Break and Unbreak (solution) Luettavissa:https://rbin.dev/writing/SH24-002 Luettu: 1.9.2026








