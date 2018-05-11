Virtualbox
==========

Instalacija Ubuntu 16.04 - VirtualBox
-------------------------------------

Zbog zahteva kursa neophodno je da svako na svom računaru ima podignutu virtuelnu mašinu sa Ubuntu 16.04 operativnim sistemom.
Za virtuelizaciju ćemo koristiti Oracle VirtualBox.
ISO file Ubuntu 16.04 desktop-a možete preuzeti sa sledećeg linka:

https://www.ubuntu.com/download/desktop/contribute?version=16.04.3&architecture=amd64

Kreiranje virtuelne mašine:
^^^^^^^^^^^^^^^^^^^^^^^^^^^

U gornjem levom uglu kliknuti na “New”. Nakon klika dobijamo prozor u kom je neophodno izabrati sledeće:
- Name: Unosite ime vaše virtuelne mašine (npr. Ubuntu16.04)
- Type:Linux
- Version: Ubuntu (64-bit)

.. note:: Primer imena je naveden zato što VirtualBox na osnovu unetog imena prepozna tip i verziju sistema.

Kliknemo na “Next”
U sledećem koraku dodeljujemo našoj virtuelnoj mašini RAM memoriju. Preporučeno je uneti 8GB (8192MB).
Kliknemo na “Next”
Zatim proveriti i ostaviti default postavke (Create a virtual hard disk now) i kliknemo na “Create”.
U sledećem koraku kliknuti na dugme “Expert Mode” i jedina stvar koju treba promeniti je veličina diska i povećati je sa 8GB na 30GB. Ostale default postavke ostaviti iste (VDI, Dynamically allocated). Kliknuti na “Create”. Ovim je kreirana virtuelna mašina.

Neophodno je podesiti mrežu koju će koristiit naša virtuelna mašina:

Desni klik na našu virtuelnu mašinu > Settings > Network > Adapter 1 > Attached to : [NAT Network] > OK

Instalacija Ubuntu operativnog sistema unutar virtuelne mašine
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Uključiti virtuelnu mašinu klikom na “Start” ili dvoklikom na selektovanu virtuelnu mašinu.
- U pop-up prozoru morate izabrati vaš Ubuntu16.04 iso fajl koji ćete koristiti za instalaciju.
- Otvara se Ubuntu instaler i potrebno je izabrati "Intsall Ubuntu".
- Zatim čekirati obe ponuđene opcije i kliknuti na "Continue".
- U sledećem koraku ostaviti default postavke i kliknuti na "Install now" pa u pop-up prozoru kliknuti na "Continue".
- Nakon toga Ubuntu prepoznaje našu lokaciju. Instalaciju nastaviti klikom na "Continue".
- U nastavku je prepioručljivo ostaviti default postavke za tastaturu "English(US)" i kliknuti na "Continue".
- U sledećem koraku je neopodno uneti vaše ime, naziv računara, vaše korisničko ime i lozinku i preporučljivo je da ostavite default postavku (Require my password to log in) i kliknuti na "Continue". Ostaje samo da se saceka kraj instalacije i reboot-uje sistem.


Instalacija CentOS 7 minimal - VirtualBox
-----------------------------------------

Zbog zahteva kursa je neophodno da svako ima na svom računaru podignutu virtuelnu mašinu sa CentOS 7
operativnim sistemom. Za virtuelizaciju koristiti Oracle VirtualBox.

ISO file CentOS 7 minimal sistema možete preuzeti sa sledećeg linka:

http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso 

Kreiranje virtuelne mašine:
^^^^^^^^^^^^^^^^^^^^^^^^^^^

U gornjem levom uglu kliknuti na New. Nakon klika dobijamo prozor u kom je neophodno izabrati sledeće:

Name: Unosite ime vaše virtuelne mašine (npr. CentOS7Minimal)
Type: Linux
Version: Red Hat (64-bit)

.. note:: Primer imena je naveden zato što VirtualBox na osnovu unetog imena prepozna tip i verziju sistema.

Kliknemo na Next 

U sledećem koraku dodeljujemo našoj virtuelnoj mašini RAM memoriju. Preporučeno je uneti minimum 2GB (2048MB). 

Kliknemo na Next

Takođe u sledećem koraku proverimo i ostavimo default postavke (Create a virtual hard disk now) i kliknemo na Create.

U sledećem koraku kliknuti na dugme Expert Mode i jedina stvar koju ćemo promeniti je veličina diska koju ćemo povećati
sa 8GB na 12GB. Ostale default postavke ostaviti iste (VDI, Dynamically allocated). Kliknuti na Create

Ovim je kreirana naša virtuelna mašina.

Neophodno je podesiti mrežu koju će koristiit naša virtuelna mašina:

Desni klik na našu virtuelnu mašinu > Settings > Network > Adapter 1 > Attached to : [NAT Network] > OK
 
Instalacija CentOS 7 minimal sistema unutar virtuelne mašine:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Uključiti virtuelnu mašinu.

U pop-up prozoru morate izabrati vaš CentOS 7 minimal iso fajl koji će te koristiti za instalaciju. 

U sledećem koraku biramo stralicom na gore Install Linux. Potvrdom instalacije se otvara Anaconda installer za CentOS 7. 
Dobra stvar sa Anaconda instalerom je što nam pruža sve blagodeti grafičkog okruženja.

Prva stvar koju moramo podesiti i izabati je jezik (i unos sa tastature) koji ćemo koristiti prilikom instalacije (preporuka ostaviti English(United States), preći na sledeći korak klikom na Continue.

U sledećem koraku neophodno je podesiti:
Localization:
Date & Time: Europe/Balgrade
Keyboard : English (US)  -> Mogu se ostaviti default-na podešavanja
Language support -> Takođe se ostavlja default postavka


Software:
Ostaviti default postavke i ne menjati postavke (jedino je preporučljivo verifikovati installation source, koji je po defaultu izabran) klik na Verify , pa klik na Done 

System:
Instalation destination : Neophodno je ući na opciju i potvrditi sa klikom na Done
Kdump: Ostaviti default postavku.
Network & Hostname : Uključiti enp0s3 klikom na "ON"
Security Policy: Ostaviti default postavku.

Kliknuti na Begin Installation

Dok traje proces instalacije podesiti ROOT PASSWORD i kreirati korisnika (USER CREATION). 
Nakon što se instalacija privede kraju kliknuti na Finish installation i sačekati još malo i konačno kliknuti na Reboot.

Nakon reboota se možemo ulogovati na našu virtuelnu mašinu.

.. note:: Pristup virtuelnoj mašini iz Bash for Windows-a

 Da bismo omogućili pristup našoj mašini preko ssh moramo podesiti nekoliko stavki.
 Prvo je neophodno isključiti mašinu, zatim dodati Host-Only adapter. To možemo uraditi na sledeći način:

 Desni klik na našu virtuelnu mašinu > Settings > Network > Adapter 2 > Attached to : [Host-Only Adapter] > OK

 Nakon ove akcije ponovo startovati mašinu, sačekati da se bootuje i uneti komandu:

 ip address

 Output komande bi trebalo da izgleda ovako:

 .. code-block:: bash

   [root@localhost ~]# ip address
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
     inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
     inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
   2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
      link/ether 08:00:27:76:1f:fa brd ff:ff:ff:ff:ff:ff
      inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic enp0s3
         valid_lft 80095sec preferred_lft 80095sec
      inet6 fe80::9fa7:df3a:5c59:f23c/64 scope link
        valid_lft forever preferred_lft forever
   3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
      link/ether 08:00:27:ca:09:cb brd ff:ff:ff:ff:ff:ff
      inet 192.168.56.101/24 brd 192.168.56.255 scope global dynamic enp0s8
        valid_lft 609sec preferred_lft 609sec
      inet6 fe80::f3ae:64bd:5e98:251/64 scope link
         valid_lft forever preferred_lft forever

 IP adresa koja vas interesuje počinje sa 192.168... U ovom slučaju `192.168.56.101`

 Pređite u Bash for Windows i po već postojećem uputstvu za SSH (SSH_Dokumentacija) se konektujte na vašu virtuelnu mašinu i kopirajte ključeve.
