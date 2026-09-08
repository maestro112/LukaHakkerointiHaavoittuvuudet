# H3 No Strings Attached

## Testiympäristö
- Oracle VirtualBox
  - Version 7.2.4 r170995 (Qt6.8.0 on windows)
- Kali Linux Debian 64bit
  - 6 ydintä
  - RAM 9100 MB
  - HDD 20 GB
- Selain: Firefox
  - Versio ESR 140.13.0 (64-bit)

---

## a)

* Aloitin tämän tehtävän lataamalla ja purkamalla ezbin-challenges.zip tiedoston. Tämän jälkeen käynnistin ohjelman ja aloitin itse tehtävän.

  <img width="389" height="199" alt="image" src="https://github.com/user-attachments/assets/cd7b514d-d7ad-4d34-9be9-c9055e3a8669" />

* Tehtävässä täytyy löytää salasana, jota en tiedä. Sen perusteella, että tehtävänannossa lukee **Did you know you can get information from binaries before running them?** ja tipeissä luki, että käytä **strings** nimistä ohjelmaa, päätin kokeilla sitä ensimmäisenä. Googletin ensin kuitenkin, mikä strings-ohjelma on. Strings-työkalulla voidaan tulostaa nopeasti binääritiedostoista luettavia tekstiosuuksia. Tämän jälkeen kokeilin strings-komentoa, ja kuten kuvassa näkyy, oikea salasana taisi löytyä.

<img width="561" height="49" alt="image" src="https://github.com/user-attachments/assets/98a25530-030a-4559-b0c4-c26fa63105e1" />

* Kokeilin vielä itse, ja salasana oli oikein.

<img width="574" height="92" alt="image" src="https://github.com/user-attachments/assets/3be11f49-b8e1-4f8a-a6f9-66a60f851b71" />

---

## b)

* Tähän kohtaan en saanut aikaseksi mitään vastausta. Tavallaan tiesin, mitä pitäisi tehdä, mutta en osannut toimia. Joten päädyin kysymään **Claude AI:lta (Sonnet 5)**, miten se korjaisi koodin niin, että salasana ei näy strings komennolla.

* Tässä on Claude AI:n vastaus:

```c
#include <stdio.h>
#include <string.h>

void deobfuscate(char *buf, const unsigned char *enc, size_t len, unsigned char key) {
    for (size_t i = 0; i < len; i++) {
        buf[i] = enc[i] ^ key;
    }
    buf[len] = '\0';
}

int main() {
    char password[20];
    unsigned char key = 0x5A;
    // "sala-hakkeri-321" pre-XOR'd with 0x5A (compute this offline)
    unsigned char enc[] = {
        's'^0x5A,'a'^0x5A,'l'^0x5A,'a'^0x5A,'-'^0x5A,
        'h'^0x5A,'a'^0x5A,'k'^0x5A,'k'^0x5A,'e'^0x5A,
        'r'^0x5A,'i'^0x5A,'-'^0x5A,'3'^0x5A,'2'^0x5A,'1'^0x5A
    };
    char real_pw[17];
    deobfuscate(real_pw, enc, sizeof(enc), key);

    printf("What's the password?\n");
    scanf("%19s", password);
    if (0 == strcmp(password, real_pw)) {
        printf("Yes! That's the password. FLAG{...}\n");
    } else {
        printf("Sorry, no bonus.\n");
    }
    return 0;
}
```

* Kokeilin seuraavaksi Claude AI:n antamaa koodia. Koodi näyttäisi toimivan.

<img width="621" height="922" alt="image" src="https://github.com/user-attachments/assets/24f7de5a-84f5-4918-8f7e-1118194da6ed" />

<img width="619" height="91" alt="image" src="https://github.com/user-attachments/assets/914910a1-6db0-41c8-9804-4c4e813fda57" />

* Mitä itse ymmärsin, niin Claude on päätynyt käyttämään ratkaisussaan XOR-obfuscation metodia.

* Päätin vielä C tehtävän **jälkeen** kokeilla pakata passtr tiedoston käyttäen UPX ohjelmaa. En usko, että tätä lasketaan, koska salasanasta näyttäisi puuttuvan vain kirjaimet **ha**, jotka voisi vain arvata, mutta mielenkiinnosta kokeilin silti.

<img width="211" height="53" alt="image" src="https://github.com/user-attachments/assets/1f6476ea-d109-4026-8405-760c7c15f992" />

---

## c)

* Tässä tehtävässä lähdin heti ajamaan packd-tiedostoa strings-komennolla. Tulostuksessa näkyy password piilos-An, mutta se ei ole oikein.

<img width="286" height="65" alt="image" src="https://github.com/user-attachments/assets/eca1a337-8b8d-4686-b0a0-2e4819ac6610" />

* Hetken katseltuani tulostunutta merkkijonoa löysin tämän. Joten päätin ladata UPX ohjelman komennolla:

```bash
sudo apt install upx
```

* ja kokeilla purkaa tiedoston komennolla:

```bash
upx -d packd
```

<img width="685" height="38" alt="image" src="https://github.com/user-attachments/assets/cad220e8-0f68-47e5-bc89-8dd8dcbeaef5" />

* Tulostunut merkkijono näytti heti erilaiselta, ja löysin oikean salasanan ja lipun.

<img width="667" height="32" alt="image" src="https://github.com/user-attachments/assets/7060e86c-e9a3-4e11-b2ce-e245838723e4" />

<img width="591" height="104" alt="image" src="https://github.com/user-attachments/assets/b3acea86-b88f-43fa-9474-952fb259b9dd" />

---

## Lähteet

**(a kohta)**
2009 Free Software Foundation Inc. strings(1) - Linux man page. Luettavissa: https://linux.die.net/man/1/strings. Luettu: 8.9.2026

**(c kohta)**
The Ultimate Packer for eXecutables 1996–2026, Markus Oberhumer, Laszlo Molnar & John Reiser. Luettavissa: https://github.com/upx/upx/blob/devel/doc/upx-doc.txt. Luettu: 8.9.2026
