# VPN-palvelin
### Projektin esittely ja tarkoitus
- Moi
  
### MasterServerin pystytys ja Saltin asennus
- Ensin loin projektille uuden kansion ja siirryin sinne komennolla `mkdir vpn-project && cd vpn-project`. Tämän jälkeen latasin focal64 komennolla `vagrant box add ubuntu/focal64` ja alustin sen ajamalla komennon `vagrant init ubuntu/focal64`. Kun virtuaalikone oli luotu, niin siirryin muokkaamaan Vagrantfile-tiedostoa komennolla `notepad Vagrantfile`.
 ![Näyttökuva (1)](https://github.com/user-attachments/assets/781d1a51-6e0c-4e3e-8dc8-609a984b848f)
- Aiemmissa harjoituksissa olen käyttänyt kurssimateriaaleissa annettuja valmiita koodinpätkiä, mutta projektia varten halusin kokeilla kirjoittaa mahdollisimman paljon itse. En ole kuitenkaan aiemmin kirjoittanut Rubylla enkä ole kovin kokenut koodari, joten otin hieman mallia [Teron materiaaleista](). Päätin aloittaa luomalla ensin pelkästään master-palvelimen ja lähteä siitä sitten laajentamaan projektia vaiheittain. Kuvakaappauksessa olevassa koodissa määrittelen master-palvelimelle nimen, käytettävän käyttöjärjestelmän ja ip-osoitteen. Tämän jälkeen käynnistin virtuaalikoneen komennolla `vagrant up` ja yhdistin sille komennolla `vagrant ssh MasterServer`. 
 ![Näyttökuva (2)](https://github.com/user-attachments/assets/c82d6afb-9634-4cb9-899f-be50aa8884bf)
- Kirjauduttuani palvelimelle hain päivitykset komennolla `sudo apt-get update` ja yritin asentaa salt-masterin komennolla `sudo apt-get -y install salt-master`. Komento ei kuitenkaan toiminut, koska olin unohtanut että Saltin asennukseen tarvitsee repot curlien kautta. Loin avaimille oman hakemiston komennolla `sudo mkdir -p /etc/apt/keyrings`, latasin avaimet [täältä](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html), hain paketit uudestaan komennolla `sudo apt update` ja asensin salt-masterin komennolla `sudo apt-get install salt-master`. Lopuksi poistuin master-palvelimelta `exit` komennolla.

   ![Näyttökuva (4)](https://github.com/user-attachments/assets/91e72cc2-b25e-45fb-95b1-c61f0b5c7326)
  ![Näyttökuva (5)](https://github.com/user-attachments/assets/294744e1-5024-4d06-9c80-d17dafccbd37)
  ![Näyttökuva (6)](https://github.com/user-attachments/assets/7adbd5b6-5f96-4a4f-8c04-58908ced8d94)

### VpnServerin pystytys
- 
### ClientServerin pystytys
- 

## Lähteet:
- VMware, Inc 2025: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html. Luettu 5.5.2025
- 
