# h4 Some Disassembly Required 



## Tiivistelmä
- Videossa käytiin läpi kuinka ghidralla saatiin salasana jolla sai lipun annettuun haasteeseen.
- Ghidran heksadesimaalinumero vastaus syötettiin vielä python3 ohjelmassa niin että saatiin desimaali numero.

---
# a)  
Asensin ghidran komenlla 
````bash
sudo apt install ghidra -y
````
<img width="1306" height="879" alt="image" src="https://github.com/user-attachments/assets/8cc22e96-4f2d-4695-8d56-c279ce6f4d66" />

---
## b)

<img width="700" height="362" alt="image" src="https://github.com/user-attachments/assets/13cb08d2-31e6-47f0-b6c2-95a5d4a0af28" />
- Hetken koodia ihmeteltyäni huomasin että ohjeissa mainitaan packd purkamien joten kävin suorittamassa tämän komennolla
  ````bash
  upx -d packd -o upackd
  ````
  
- nyt koodi näkyi selvästi laajemmin. Löysin funktion jossa lukee aika selvästi että muuttuja iVarl on piilos-AnAnAs.

<img width="1045" height="196" alt="image" src="https://github.com/user-attachments/assets/15fd08c2-a90a-4eb6-bc66-087ac3560d57" />

<img width="518" height="306" alt="image" src="https://github.com/user-attachments/assets/7c04a93c-c1dc-4f5b-b9aa-e3f2920dab04" />

- Mitä itse olen ymmärtäny niin kohdassa jossa salasana lukee vertaillaan käyttäjän antamaa salasanaa **piilos-AnAnAs** merkkijonoon. Jos palautus arvo on 0 palautetaan lippu ja jos jokin muu palautetaan **Sorry no bonus**

## c)
- Tässä tehtävässä tein uuden projektin ja importasin siihen passtr ohjelman.
- Menin heti tutkimaan main funktiota. koodi näyttää samalta kuin packd tehtävässä joten aloin tutkimaan kohtaa jossa tarkastetaan onko salasana oiken. Kun maalasin tämän koodin se näytti minulle kohdan assembly koodissa 
  <img width="533" height="292" alt="image" src="https://github.com/user-attachments/assets/115239c3-4102-4310-9598-f651171cb03c" />
  
- Kun maalasin tämän koodin se näytti minulle kohdan assembly koodissa
  - **TEST eax, eax** kohdassa tarkistetaan onko eax arvo 0 eli **iVar1 == 0**
  - JNZ kohta tarkoittaa **Jump if Not Zero** eli jos tulos ei ole nolla hypätään kohtaan **001011b6**. Kun oikea klikkasin **001011b6** ja painoin show refreces to addres se näytti minulle kohdan mikä näyttäisi olevan rivi jossa tulostuu **sorry no bonus**  
<img width="1014" height="33" alt="image" src="https://github.com/user-attachments/assets/14e2e1b0-886f-4149-a695-baa6acf675ef" />

<img width="685" height="50" alt="image" src="https://github.com/user-attachments/assets/6f5d7836-56a3-4d28-8e2b-5d2f587013bd" />

- Seuraavaksi minulla tuli vahva epäilys siitä että minun pitäisi kääntää **JNZ** kohta jotenkin ympäri ja tämä tosiaan onnistuu kirjoittamalla sen tilalle **JZ** eli **jump if zero** eli toimitäänkin väärin päin jolloin kaikki muut salasanat pitäisi olla oikein ja oikea salasana väärin.
- 
<img width="997" height="47" alt="image" src="https://github.com/user-attachments/assets/433a8c17-4208-48c1-bdf5-5720d1d23f53" />

- koodi järjestys muuttui Decompile ikkunassa
  
<img width="539" height="305" alt="image" src="https://github.com/user-attachments/assets/f5be6166-29cb-46c2-a611-45a158274c6e" />

- 
<img width="434" height="281" alt="image" src="https://github.com/user-attachments/assets/4552974b-9433-4196-9bc2-92ba3b8a6655" />

<img width="1643" height="653" alt="image" src="https://github.com/user-attachments/assets/88ecb6ab-d086-4ae5-aca1-dfa788d05711" />

<img width="779" height="592" alt="image" src="https://github.com/user-attachments/assets/f0e232e4-3bce-453a-b15e-1a0ccb7acd85" />

<img width="2008" height="762" alt="image" src="https://github.com/user-attachments/assets/79bbb001-3bb7-459e-b6cd-65c0a47eb0b0" />
<img width="1146" height="380" alt="image" src="https://github.com/user-attachments/assets/4d1bbbaa-9952-455a-a3c3-d3a110b990a7" />







