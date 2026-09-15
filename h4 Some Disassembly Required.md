# h4 Some Disassembly Required

## Testiympäristö

- Oracle VirtualBox
  - Version 7.2.4 r170995 (Qt6.8.0 on Windows)
- Kali Linux Debian 64bit
  - 6 ydintä
  - RAM 9100 MB
  - HDD 20 GB
- Selain: Firefox
  - Versio ESR 140.13.0 (64-bit)

---

## Tiivistelmä

- Videossa käytiin läpi, kuinka Ghidralla saatiin salasana, jolla sai lipun annettuun haasteeseen.
- Ghidran heksadesimaaliluku syötettiin vielä Python3-ohjelmaan niin, että saatiin desimaaliluku.

---

# a) Install Ghidra

- Asensin Ghidran komennolla

```bash
sudo apt install ghidra -y
```

<img width="1306" height="879" alt="image" src="https://github.com/user-attachments/assets/8cc22e96-4f2d-4695-8d56-c279ce6f4d66" />

---

## b) rever-C

- Aloitin tehtävän avaamalla packd-ohjelman Ghidrassa

<img width="700" height="362" alt="image" src="https://github.com/user-attachments/assets/13cb08d2-31e6-47f0-b6c2-95a5d4a0af28" />

- Hetken koodia ihmeteltyäni huomasin, että tipeissä mainitaan packd:n purkaminen, joten kävin suorittamassa tämän komennolla

  ```bash
  upx -d packd -o upackd
  ```

- Nyt koodi näkyi selvästi laajemmin. Löysin funktion, jossa lukee aika selvästi, että muuttuja iVarl on **piilos-AnAnAs**.

<img width="1045" height="196" alt="image" src="https://github.com/user-attachments/assets/15fd08c2-a90a-4eb6-bc66-087ac3560d57" />

<img width="518" height="306" alt="image" src="https://github.com/user-attachments/assets/7c04a93c-c1dc-4f5b-b9aa-e3f2920dab04" />

- Mitä itse olen ymmärtänyt, niin käyttäjän antamaa salasanaa verrataan **piilos-AnAnAs**-merkkijonoon. Jos palautusarvo on 0, palautetaan lippu, ja jos jokin muu, palautetaan **Sorry no bonus**.

---

## c) If backwards

- Tässä tehtävässä tein uuden projektin ja importasin siihen passtr-ohjelman.

- Menin heti tutkimaan main-funktiota. Koodi näyttää samalta kuin packd-tehtävässä, joten aloin tutkimaan kohtaa, jossa tarkastetaan, onko salasana oikein.

  <img width="533" height="292" alt="image" src="https://github.com/user-attachments/assets/115239c3-4102-4310-9598-f651171cb03c" />

- Kun maalasin tämän koodin, se näytti minulle kohdan assembly-koodissa.
- **TEST EAX, EAX** -kohdassa tarkistetaan, onko eax-arvo 0 eli **iVar1 == 0**.
- **JNZ**-kohta tarkoittaa **Jump if Not Zero**, eli jos tulos ei ole nolla, hypätään kohtaan **001011b6**. Napsautin oikealla hiiren painikkeella **001011b6** ja painoin Show References to Address. Se näytti minulle kohdan, mikä näyttäisi olevan rivi, jossa tulostuu **sorry no bonus**.
   - Eli jos **EAX** on jokin muu kuin 0, hypätään **001011b6** eli **sorry no bonus**.

<img width="1014" height="33" alt="image" src="https://github.com/user-attachments/assets/14e2e1b0-886f-4149-a695-baa6acf675ef" />

<img width="685" height="50" alt="image" src="https://github.com/user-attachments/assets/6f5d7836-56a3-4d28-8e2b-5d2f587013bd" />

- Seuraavaksi minulle tuli vahva epäilys siitä, että minun pitäisi kääntää **JNZ**-kohta jotenkin ympäri. Hetken tutkittuani tämä tosiaan onnistuu kirjoittamalla **JNZ** tilalle **JZ** eli **Jump if Zero**, jolloin toimitaankin väärinpäin: kaikki muut salasanat pitäisi olla oikein ja oikea salasana väärin.

<img width="997" height="47" alt="image" src="https://github.com/user-attachments/assets/433a8c17-4208-48c1-bdf5-5720d1d23f53" />

- Koodin järjestys muuttui Decompile-ikkunassa.

<img width="539" height="305" alt="image" src="https://github.com/user-attachments/assets/f5be6166-29cb-46c2-a611-45a158274c6e" />

- Seuraavaksi exporttasin ohjelman C-kielenä.

<img width="434" height="281" alt="image" src="https://github.com/user-attachments/assets/4552974b-9433-4196-9bc2-92ba3b8a6655" />

- Kun yritin muuntaa tiedostoa, tuli paljon virheilmoituksia, mikä kertoi, että jokin oli pielessä koodissa.

<img width="1643" height="653" alt="image" src="https://github.com/user-attachments/assets/88ecb6ab-d086-4ae5-aca1-dfa788d05711" />

- Koska en osaa juuri paljoa C-kieltä, lähdin korjaamaan koodia jo ennestään toimivan koodin perusteella.

<img width="2008" height="762" alt="image" src="https://github.com/user-attachments/assets/79bbb001-3bb7-459e-b6cd-65c0a47eb0b0" />

- Korjaamani koodi toimi.

<img width="1146" height="380" alt="image" src="https://github.com/user-attachments/assets/4d1bbbaa-9952-455a-a3c3-d3a110b990a7" />

---

## d) Nora CrackMe

- Ensimmäisenä latasin tehtävän tiedostot ja tarvittavat ohjelmat komennoilla.

```bash
git clone https://github.com/NoraCodes/crackmes.git
```

```bash
sudo apt install build-essential gcc xxd binutils
```

---

## e)  Nora crackme01 & Nora crackme01e.

### Nora crackme01

- Aivan ensimmäisenä tein koodista ohjelman komennolla

```bash
make cracme01
```

- Seuraavaksi avasin crackme01-koodin Ghidrassa. Päätin lähteä ratkomaan tätä tehtävää kokeilemalla eri näkymiä **Window**-kohdassa
   - Defined Strings kohdassa löysin **password1**.

<img width="984" height="36" alt="image" src="https://github.com/user-attachments/assets/87e89e73-b7d2-4531-b964-5b179b906a04" />

- Halusin vielä nähdä, missä tätä käytetään, joten napsautin oikealla ja painoin References -> Show References, mikä vei minut salasanan tarkastusfunktioon.

<img width="477" height="410" alt="image" src="https://github.com/user-attachments/assets/a4307290-adb7-4c0a-88df-f45260558bf4" />

- Lähdin näillä tiedoilla kokeilemaan salasanaa. Ja kappas, se oli oikein.

<img width="508" height="117" alt="image" src="https://github.com/user-attachments/assets/79442695-a37c-459f-b749-f7850b1587e0" />

---

### Nora crackme01e.

- Tässä tehtävässä tein tismalleen samat temput kuin aiemmassa, ja oikea salasana löytyi.

<img width="658" height="30" alt="image" src="https://github.com/user-attachments/assets/77b94001-6456-4728-bc12-425f13ca3077" />

- Seuraavaksi sain virheilmoituksen.

<img width="559" height="107" alt="image" src="https://github.com/user-attachments/assets/156f6492-a53a-403a-85b1-3bf8da2c5288" />

- Hetken ratkaisua etsittyäni löysin, että bash **!** tarkoittaa, että etsitään edellistä komentoa, eli tässä tapauksessa yritetään etsiä **paak.k.** Salasanan täytyy olla suljettu siis **'** merkeillä.

<img width="543" height="118" alt="image" src="https://github.com/user-attachments/assets/a549a51f-97c2-4efa-9d99-c71cdb8cff6a" />

---

## f) Nora crackme02.

### Tässä kohtaa AI käytetty apuna **ChatGPT-5.6 Luna**.

- Taas käännettyäni lähdekoodin ohjelmaksi laitoin sen Ghidraan ja avasin main-funktion

<img width="711" height="577" alt="image" src="https://github.com/user-attachments/assets/58d07415-8cc9-4a9e-a552-4069b34f1f2a" />

- Muuttujien nimet voisivat olla esim.

  - `param_1` -> `argc`
  - `param_2` -> `argv`
  - `pcVar1` -> `input`
  - `pcVar4` -> `input_ptr`
  - `pcVar5` -> `password_ptr`
  - `cVar2` -> `previous_char`
  - `uVar3` -> `return_value`

- Koska en ole kovin hyvä C-kielessä, käytin tässä kohtaa **ChatGPT**-apuna. Miten itse ymmärsin, niin ohjelmassa oleva `password1` toimii ikään kuin mallina, mutta ohjelma odottaa jokaisesta merkistä ASCII-arvoltaan yhden pienempää merkkiä syötteessä. Siksi oikea salasana on o\rrvnqc0`.

- Ja taas salasana **'** sisälle.

<img width="517" height="243" alt="image" src="https://github.com/user-attachments/assets/24ed5184-80df-4838-885a-d9f3fa172bde" />

---

## lähteet

- Can't use exclamation mark (!) in bash? Luettavissa:https://unix.stackexchange.com/questions/33339/cant-use-exclamation-mark-in-bash. Luettu: 15.9.2026
- Some Crackmes Luettavissa:https://github.com/NoraCodes/crackmes Luettu: 15.9.2026
- An Intro to x86_64 Reverse Engineering. Leonora Tindall 2017. Luettavissa:https://nora.codes/tutorial/an-intro-to-x86_64-reverse-engineering/ Luettu: 15.9.2026
- GHIDRA for Reverse Engineering. John Hammond katsottavissa:https://www.youtube.com/watch?v=oTD_ki86c9I katsottu 15.9.2026
- (f kohta) ja oikeinkirjoitus **ChatGPT-5.6 Luna**
