# VPN-palvelin
### Projektin esittely ja tarkoitus
- Moi

### Sisällysluettelo
[1. Master-palvelimen pystytys ja Salt-masterin asennus](https://github.com/Hutiluz/vpn-project/blob/main/frontpage.md#1-master-palvelimen-pystytys-ja-salt-masterin-asennus)
[2. Vpn-palvelimen pystytys ja Salt-minionin asennus](https://github.com/Hutiluz/vpn-project/blob/main/frontpage.md#2-vpn-palvelimen-pystytys-ja-salt-minionin-asennus)
 - [
 - [3. Client-palvelimen pystytys ja Salt-minionin asennus]
 - [Lähteet]
  
### 1. Master-palvelimen pystytys ja Salt-masterin asennus
- Ensin loin projektille uuden kansion ja siirryin sinne komennolla `mkdir vpn-project && cd vpn-project`. Tämän jälkeen latasin focal64 komennolla `vagrant box add ubuntu/focal64` ja alustin sen ajamalla komennon `vagrant init ubuntu/focal64`. Kun virtuaalikone oli luotu, niin siirryin muokkaamaan Vagrantfile-tiedostoa komennolla `notepad Vagrantfile`.
 ![Näyttökuva (1)](https://github.com/user-attachments/assets/781d1a51-6e0c-4e3e-8dc8-609a984b848f)
- Aiemmissa harjoituksissa olen käyttänyt kurssimateriaaleissa annettuja valmiita koodinpätkiä, mutta projektia varten halusin kokeilla kirjoittaa mahdollisimman paljon itse. En ole kuitenkaan aiemmin kirjoittanut Rubylla enkä ole kovin kokenut koodari, joten otin hieman mallia [Teron materiaaleista](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/). Päätin aloittaa luomalla ensin pelkästään master-palvelimen ja lähteä siitä sitten laajentamaan projektia vaiheittain. Kuvakaappauksessa olevassa koodissa määrittelen master-palvelimelle nimen, käytettävän käyttöjärjestelmän ja ip-osoitteen. Tämän jälkeen käynnistin virtuaalikoneen komennolla `vagrant up` ja yhdistin sille komennolla `vagrant ssh MasterServer`. 
 ![Näyttökuva (2)](https://github.com/user-attachments/assets/c82d6afb-9634-4cb9-899f-be50aa8884bf)
- Kirjauduttuani palvelimelle hain päivitykset komennolla `sudo apt-get update` ja yritin asentaa salt-masterin komennolla `sudo apt-get -y install salt-master`. Komento ei kuitenkaan toiminut, koska olin unohtanut että Saltin asennukseen tarvitsee repot curlien kautta. Loin avaimille oman hakemiston komennolla `sudo mkdir -p /etc/apt/keyrings`, latasin avaimet [täältä](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html), hain paketit uudestaan komennolla `sudo apt update` ja asensin salt-masterin komennolla `sudo apt-get install salt-master`. Lopuksi poistuin master-palvelimelta `exit` komennolla.

  ![Näyttökuva (3)](https://github.com/user-attachments/assets/91e72cc2-b25e-45fb-95b1-c61f0b5c7326)
  ![Näyttökuva (4)](https://github.com/user-attachments/assets/294744e1-5024-4d06-9c80-d17dafccbd37)
  ![Näyttökuva (5)](https://github.com/user-attachments/assets/7adbd5b6-5f96-4a4f-8c04-58908ced8d94)

### 2. Vpn-palvelimen pystytys ja Salt-minionin asennus
- Kirjauduttuani ulos master-palvelimelta siirryin takaisin muokkaamaan Vagrantfilea komennolla `notepad Vagrantfile`. Master-palvelinta varten luotu koodinpätkä oli toimiva, joten kopioin sen sellaisenaan vpn-palvelinta varten muuttaen palvelimen nimen ja ip-osoitteen. Nyt kun olin asentanut Salt-masterin manuaalisesti, niin halusin kokeilla luoda Vagrantfileen skriptin, joka automaattisesti asentaa Salt-minionin. Katsoin tähän mallia [täältä](https://developer.hashicorp.com/vagrant/docs/provisioning/shell#inline-scripts) ja tein sen pohjalta kuvakaappauksen mukaisen skriptin.
  ![Näyttökuva (6)](https://github.com/user-attachments/assets/4d4975ce-43a2-486e-84bb-169904e27107)
- Käynnistin koneet uudelleen komennolla `vagrant up` ja master-palvelin käynnistyy normaalisti, mutta vpn-palvelin heittää timeouttia. Kokeilin kirjautua vpn-palvelimelle komennolla `vagrant ssh VpnServer` ja tarkastin salt-minionin tilan komennolla `systemctl status salt-minion`. Kuten kuvakaappauksesta näkyy, pääsin kirjautumaan normaalisti sisälle, mutta laitteen nimi on väärin eikä salt-minion ollut asentunut. Kirjaudun ulos koneelta `exit` komennolla ja ajoin uudelleen minion_scriptin komennolla `vagrant provision VpnServer`.
  ![Näyttökuva (7)](https://github.com/user-attachments/assets/534ae1a1-7bc6-4565-8e71-3e4e0957686e)
  ![Näyttökuva (8)](https://github.com/user-attachments/assets/0514fe47-3c07-4031-869d-9f1a3b405cfc)
- Kirjauduin uudelleen palvelimelle ja tarkistin salt-minionin tilan komennolla `systemctl status salt-minion`. Tällä kertaa salt-minion oli asentunut, mutta palvelimen nimi oli edelleen väärin. Muutin sen manuaalisesti komennolla `sudo hostnamectl set-hostname vpn` ja käynnistin koneen uudelleen. Kun yhdistin takaisin koneelle, niin palvelimen nimi oli myös oikein. Lopuksi poistuin palvelimelta komennolla `exit`.
  ![Näyttökuva (10)](https://github.com/user-attachments/assets/b97b0ab0-6633-4a48-b8f2-68791a467df5)
  ![Näyttökuva (11)](https://github.com/user-attachments/assets/ed48c5e7-55eb-43f1-968f-7b953717e571)
  
### 3. Client-palvelimen pystytys, Salt-minionin asennus ja Vagrantfilen viimeistely
- Vagrantfilessa sekä koneen luonti että minion_script selkeästi toimivat, mutta vpn-palvelimen kanssa prosessi ei ollut täysin automatisoitu kuten toivoin. Halusin varmistaa koodin toimivuuden kopioimalla sen client-palvelimelle ja muuttamalla hostnamen ja ip-osoitteen. Alla olevassa kuvakaappauksessa näkyy muokattu Vagrantfile.
  ![Näyttökuva (12)](https://github.com/user-attachments/assets/7e0a580d-b55a-456b-a59a-f1db433aa613)
- Käynnistin koneet jälleen `vagrant up` komennolla, mutta tällä kertaa kaikki sujui hyvin eikä tullut timeouttia. Yhdistin client-palvelimelle komennolla `vagrant ssh ClientServer` ja tarkistin salt-minionin tilan komennolla `systemctl status salt-minion`. Salt-minion oli asentunut kerralla oikein ja palvelimen nimikin oli oikein. Kirjauduinkin ulos palvelimelta ja siirryin takaisin Vagrantfileen tekemään muutamia hienosäätöjä.
  ![Näyttökuva (13)](https://github.com/user-attachments/assets/618e8621-b5e5-4540-a73e-74d59535b093)
- moi


## Lähteet:
- Karvinen 2025: https://terokarvinen.com/palvelinten-hallinta/#laksyt. Luettu 4.5.2025.
- Karvinen 2021: https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/. Luettu 5.5.2025.
- VMware, Inc 2025: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html. Luettu 5.5.2025.
- HashiCopr Developer s.a: https://developer.hashicorp.com/vagrant/docs/provisioning/shell#inline-scripts. Luettu 5.5.2025.
- 
