# H5 Binääri tässä, missä koodit?

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

## lab0

- Lab0:n alottaessa kokeilin ensimmäisenä vain ajaa koodin.

<img width="449" height="129" alt="image" src="https://github.com/user-attachments/assets/ff718d2e-9857-468f-a5f9-f55b1c21748b" />

- Koodissa selvästikin haluttaisiin, että viimeinen rivi ei tulostuisi. Kävin Microlla muokkaamassa neliöidystä kohdasta, jossa oli aiemmin **<=**, ja poistin **=**-merkin.

<img width="682" height="318" alt="image" src="https://github.com/user-attachments/assets/5ade6601-e404-41a8-9aee-6170c65a8b8b" />

- Nyt merkkijono tulostui oikein.

<img width="701" height="182" alt="image" src="https://github.com/user-attachments/assets/62aaad8f-7f23-4718-b08e-b332fa15548c" />

---

## lab1

- Tässä lähdin taas ensimmäisenä ajamaan ohjelmaa. Näyttäisi siltä, että ohjelma päättyy zsh:n virheilmoitukseen, vaikka sen ei pitäisi.

<img width="676" height="106" alt="image" src="https://github.com/user-attachments/assets/5941e60d-b5e5-4b37-a888-6b3ae2d1f312" />

- Lähdin ratkomaan tätä GDB:llä, ja suorittamisen jälkeen GDB kertoi, että rivillä 7 ilmeni ongelma.

<img width="892" height="86" alt="image" src="https://github.com/user-attachments/assets/4cd1d0f5-78dc-4e3b-91a9-2494d3f3e060" />

- `bt`-komennolla eli `backtrace`-komennolla näin, miten tähän päädyttiin, ja sieltä ilmeni, että rivillä 18 oleva **Print_scramblet(bad_message)**-kutsu saattoi olla virheen syy. Muutin rivit 14 ja 18 kommenteiksi, koska ne liittyivät toisiinsa.

<img width="485" height="100" alt="image" src="https://github.com/user-attachments/assets/0d338b22-7f6b-4dc6-ae6b-55910f807fa3" />
<br>
<img width="512" height="204" alt="image" src="https://github.com/user-attachments/assets/31166e1e-eda9-47cc-b2db-a1a3da07217f" />

- Nyt koodi tulostui ilman virheilmoitusta.

<img width="304" height="78" alt="image" src="https://github.com/user-attachments/assets/568e8a44-2f91-4e00-bf3f-2cfc89f22015" />

---

## lab2 käytetty Claude Sonnet 5:tä apuna

- Tässä lähdin heti GDB:llä purkamaan `main`-funktiota. Silmiini iski tämä kohta, koska Teron tehtävässä, kun käytimme Ghidraa, tuli paljon assemblyä tuijoteltua ja siellä oli samankaltainen salasanan tarkistuskohta. Tässä kohdassa näkyy **jne** eli **jump if not equal**, eli salasanan tarkistus on todennäköisesti vain muutaman rivin ylempänä, koska tämä on se kohta, jossa päätetään, hypätäänkö **sorry no bonus** -kohtaan vai jatketaanko suoritusta. `CALL`-kohdassa kutsutaan funktiota **mAsdf3a**, joka on todennäköisesti salasanan tarkistava funktio. Ja **eax**-rekisterissä tarkistetaan funktion **mAsdf3a** palauttama arvo. Tämän perusteella lähdin purkamaan **mAsdf3a**-funktiota.

<img width="1418" height="416" alt="image" src="https://github.com/user-attachments/assets/c2109bf9-66fe-4af1-bef8-dcad0395dbbb" />

- Tästä eteenpäin en saanut mitään aikaiseksi, ja monen tunnin jälkeen käännyin AI:n puoleen, koska olin aivan totaalisesti jumissa. Clauden ohjeiden avulla huomasin, että olin selvästikin oikeilla jäljillä, mutta taidot eivät yksinkertaisesti riittäneet tehtävän ratkaisemiseen yksin.

- Seuraavaksi AI ohjasi minut tekemään breakpointin.

<img width="311" height="53" alt="image" src="https://github.com/user-attachments/assets/0d4ea583-b2e3-4fa7-b2dd-d97d000f1e9e" />

- Tämän jälkeen ajoin ohjelman ja syötin siihen jotain kirjaimia.

<img width="726" height="134" alt="image" src="https://github.com/user-attachments/assets/331fd765-f2f1-4500-9938-f6c1bda37a3a" />

- Sain tämän jälkeen salasanan tulostumaan `x/s $rdi` komenolla, mutta se oli väärässä muodossa. Tässä kohtaa kysyin myös Claudelta apua, ja se kertoi minulle, että koodi lisää ASCII-arvoon **+3**, jos indeksinumero on parillinen, ja vähentää **-7**, jos indeksi on pariton. Esimerkiksi `a -> d`.

<img width="386" height="25" alt="image" src="https://github.com/user-attachments/assets/1375bf81-6e43-4e3e-a68c-149b376394e1" />

- Väänsin taulukon siitä, miten itse ymmärsin koodin toimivan.

| i | merkki | ASCII | parillinen/pariton | laskutoimitus | tulosmerkki |
| - | ------ | ----- | ------------------ | ------------- | ----------- |
| 0 | a      | 97    | parillinen         | 97 + 3        | d           |
| 1 | n      | 110   | pariton            | 110 − 7       | g           |
| 2 | L      | 76    | parillinen         | 76 + 3        | O           |
| 3 | T      | 84    | pariton            | 84 − 7        | M           |
| 4 | j      | 106   | parillinen         | 106 + 3       | m           |
| 5 | 4      | 52    | pariton            | 52 − 7        | -           |
| 6 | u      | 117   | parillinen         | 117 + 3       | x           |
| 7 | 8      | 56    | pariton            | 56 − 7        | 1           |

- Saamani salasana on oikein.

<img width="925" height="142" alt="image" src="https://github.com/user-attachments/assets/ad5fcd57-4154-401f-a7f5-4b0d07265d99" />

---

## lab3

- Tässä tehtävässä valitsin **crackme01**:n ja yritin saada salasanan ulos GDB:llä. Tehtävä oli aika simppeli. Käänsin ensin ohjelman komennolla **gcc crackme01.c -g -Wall -Werror -o crackmegdb01**. Tämän jälkeen ajoin ohjelman GDB:llä, laitoin breakpointin `main`-funktioon ja ajoin ohjelman. Sen jälkeen annoin `list`-komennon, jolla näkyi salasanan tarkistus ja salasana **password1**.

<img width="920" height="322" alt="image" src="https://github.com/user-attachments/assets/066ff3b4-19dc-493f-b551-c7eb351b4c73" />

- Salasana oli oikein.

<img width="378" height="89" alt="image" src="https://github.com/user-attachments/assets/5106318e-6041-43bc-a32a-ce9af2a745fb" />

---

## Mitä opin?
- Näissä tehtävissä opin uusia komentoja GDB:llä ja miten niitä käytetään.
- Lukemaan lisää assenblyä.

---

## Lähteet
- [Terokarvinen.com](https://terokarvinen.com/)
- [gdb - Linux manual pages](https://www.man7.org/linux/man-pages/man1/gdb.1.html)
- [ascii - Linux manual pages](https://www.man7.org/linux/man-pages/man7/ascii.7.html)
- [Claude Sonnet 5](https://claude.ai/)
