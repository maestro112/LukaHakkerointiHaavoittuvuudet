# h5 Binääri tässä, missä koodit?

## lab0

- Lab0 alottaessa kokeilin ensimmäisenä vain ajaa koodin. 
  
<img width="449" height="129" alt="image" src="https://github.com/user-attachments/assets/ff718d2e-9857-468f-a5f9-f55b1c21748b" />

- Koodissa selvästikkin haluttaisiin että viimeinen rivi ei tulostuisi. Kävin microlla muokkaamassa neliöidystä kohdasta missä oli aijemin **<=** ja poistin **=**.  

<img width="682" height="318" alt="image" src="https://github.com/user-attachments/assets/5ade6601-e404-41a8-9aee-6170c65a8b8b" />

- nyt merkki jono tulostui oikein
  
<img width="701" height="182" alt="image" src="https://github.com/user-attachments/assets/62aaad8f-7f23-4718-b08e-b332fa15548c" />

---

## lab1

- Tässä lähdin taas ensimmäisenä ajamaan ohjelmaa. Ja näyttäisi siltä että zsh virhe ei saisi tulostua.

<img width="676" height="106" alt="image" src="https://github.com/user-attachments/assets/5941e60d-b5e5-4b37-a888-6b3ae2d1f312" />

- Lähdin ratkomaan tätä gdb:llä ja suorittamisen jälkeen gdb kertoi että rivillä 7 ilmeni ongelma 

<img width="892" height="86" alt="image" src="https://github.com/user-attachments/assets/4cd1d0f5-78dc-4e3b-91a9-2494d3f3e060" />

- bt komenolla eli backtrace näin miten tähän päädyttin ja sieltä ilmeni että rivillä 18 **Print_scramblet(bad_message)** koodi saattoi olla virheen syy. Muutin 14 ja 18 rivit kommenteiksi koska ne liittyivät toisiinsa.

<img width="485" height="100" alt="image" src="https://github.com/user-attachments/assets/0d338b22-7f6b-4dc6-ae6b-55910f807fa3" />
<br>
<img width="512" height="204" alt="image" src="https://github.com/user-attachments/assets/31166e1e-eda9-47cc-b2db-a1a3da07217f" />

- Nyt koodi tulostui ilman virhe ilmoitusta

<img width="304" height="78" alt="image" src="https://github.com/user-attachments/assets/568e8a44-2f91-4e00-bf3f-2cfc89f22015" />

---

## lab2 käytetty Claude sonnet 5 apuna. 

- Tässä lähin heti gdb:llä purkamaan main funktiota. Silmiini iski tämä kohta koska teron tehtävässä kun käytimme ghidraa niin tuli paljon assemblyä tujjoteltua ja siellä oli saman kaltainen sallasanan tarkistus kohta. Tässä kohtaa näkyy **jne** eli **jump if not equal** eli salsanan tarkistus on todennäkoisesti vain muutaman rivin ylempänä koska tämä on se kohta jossa päätetään että hypätäänkö **sorry no bonus** kohtaan vai jatketaanko suoritusta. CALL kohdassa kutsutaan funktiota **mAsdf3a** eli todennäköisesti salasanan tarkistus funktio. Ja **eax** kohdassa tarkistetaan funktion **mAsdf3a** tulostama arvo. Tämän perusteella lähdin purkamaan **mAsdf3a** funktiota. 

<img width="1418" height="416" alt="image" src="https://github.com/user-attachments/assets/c2109bf9-66fe-4af1-bef8-dcad0395dbbb" />

- Tästä eteenpäin en saanut mitään aikaseksi ja monen tunnin jälkeen käännyin ai puoleen koska olin aivan totaalisesti jumissa. mitä Claude ohjasi minua huomasin että olin selvästikkin oikeilla jäljillä mutta taidot ei yksin kertaisesti riittänyt.  
  
- Seuraavaksi Ai ohjasi minut tekemään breakpointin 

<img width="311" height="53" alt="image" src="https://github.com/user-attachments/assets/0d4ea583-b2e3-4fa7-b2dd-d97d000f1e9e" />

- Tämän jälkeen ajoin ohjelman ja syötin jotain kirjaimia siihen.

<img width="726" height="134" alt="image" src="https://github.com/user-attachments/assets/331fd765-f2f1-4500-9938-f6c1bda37a3a" />

- sain tämän jälkeen salasanan tulostumaan mmutta se oli väärässä muodossa. Tässä kohtaa kysyin myös claudelta apua ja se kertoi minulle että koodi lisää +3 sen ASCII järjestyksen jos sen indeksi on parillinen ja erottaa -7 jos pariton esim. a -> d.

<img width="386" height="25" alt="image" src="https://github.com/user-attachments/assets/1375bf81-6e43-4e3e-a68c-149b376394e1" />

- väänsin taulukon siitä miten itse ymmärsin koodin toimivan
  
| i | merkki | ASCII | parillinen/pariton | laskutoimitus | tulos merkki |
|---|--------|-------|---------------------|----------------|--------------|
| 0 | a      | 97    | parillinen          | 97+3           | d            |
| 1 | n      | 110   | pariton             | 110−7          | g            |
| 2 | L      | 76    | parillinen          | 76+3           | O            |
| 3 | T      | 84    | pariton             | 84−7           | M            |
| 4 | j      | 106   | parillinen          | 106+3          | m            |
| 5 | 4      | 52    | pariton             | 52−7           | -            |
| 6 | u      | 117   | parillinen          | 117+3          | x            |
| 7 | 8      | 56    | pariton             | 56−7           | 1            |

- Saamani salasana on oikein

<img width="925" height="142" alt="image" src="https://github.com/user-attachments/assets/ad5fcd57-4154-401f-a7f5-4b0d07265d99" />

 
---

## lab3

- Tässä tehtävässä valitsin crackme01 ja yritin saada salasanan ulos gdb:llä. Tehtävä oli aika simppeli käänsin eka ohjelman komenolla **gcc crackme01.c p -g -Wall -Werror -o crackmegdb01**. Tämän jälkeen ajoin ohjelmaa gdb:llä laitoin break pointin main funktioon ja ajoin jonka jälkeen annoin list komennon jossa näkyi salasana **password1**

<img width="920" height="322" alt="image" src="https://github.com/user-attachments/assets/066ff3b4-19dc-493f-b551-c7eb351b4c73" />

- salasana oli oikein

<img width="378" height="89" alt="image" src="https://github.com/user-attachments/assets/5106318e-6041-43bc-a32a-ce9af2a745fb" />



---

## Lähteet 
- 









