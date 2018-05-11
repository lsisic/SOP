LINUX - CentOS7
===============

Pristup komandnoj liniji
------------------------

Shell je program koji omogućava interakciju između korisnika i operativnog sistema.

bash - podrazumevani shell na najvećem broju distribucija.

Shell komande: Svaka komanda ima konceptualno 3 elementa: komanda, opcije, argumenti .

Skoro sve komande imaju opciju --help.

``cat`` ispisuje sadržaj tekstualne datoteke, npr : ``cat file1`` ; ``cat -b file1`` ispis brojeva linija tekstualne datoteke.

``date`` prikazuje trenutni datum i vreme.

``date +%r`` prikazuje samo vreme u AM/PM formatu (istražiti ostale opcije).

``passwd`` menja lozinku aktuelnog korisnika, ``passwd luka`` superuser resetuje lozinku korisnika luka .

``file`` skenira početak sadržaja datoteke i prikazuje tip datoteke.

``head`` prikazuje prvih nekoliko linija datoteke, podrazumevano 10. Sa parametrom -n može se određen broj prikazati.

Jedan pritisak na TAB dopunjava naziv komande ako je strig jedinstven a dvostruki pritisak na TAB prikazuje moguće opcije ako string nije jedinstven.

Postoji:
Filename completion,
Pathname completion,
Variablename completion,
Username completion,
Hostname completion.

``history`` otkucane komande. ``history 5`` poslednjih 5 otkucanih. CTRL+R pretraga. ``history -c`` briše history iz .bash_history datoteke iz home foldera korisnika.


Upravljanje datotekama iz komandne linije
-----------------------------------------

Direktorijum / je korenski.

Karakter / se takođe koristi kao separator.

Važni direktorijumi:

``/urs`` Instaliran softver, deljene biblioteke, statičke biblioteke. Važni poddirektorijumi ``/usr/bin`` - korisničke komande, ``/usr/sbin`` komande sistemske administracije, ``/usr/local`` lokalno prilagođen softver.

``/etc`` lokalne konfiguracione datoteke operativnog sistema.

``/var`` promenljivi perzistentni podaci (baze podataka, keš direktorijumi, log datoteke ...)

``/run`` runtime podaci za procese startovane u odnosu na prethodno podizanje sistema. Proces ID datoteke, lock datoteke.

``/home`` home direktorijumi za lične i konfiguracione datoteke.

``/root`` home direktorijum super korisnika, root-a.

``/tmp`` prostor za privremene datoteke. Datoteke starije od 10 dana se brišu. Postoji još jedna lokacija za privremene datoteke: ``/var/tmp`` , ovde se datoteke kojima nije promenjen atime, mtime ili ctime više od 30 dana, automatski brišu.

``/boot`` datoteke koje se zahtevaju u procesu podizanja operativnog sistema.

``/dev`` sadrži datoteke koje reprezentuju hardverske uređaje.

``/lib`` biblioteke, uglavnom statičke.

``/mnt`` lokacija za  privremeno montiranje diskova, file share-ova, koja zahteva privilegije superkorisnika.

``/media`` lokacija za privremeno montiranje diskova koja ne zahteva privilegije superkorisnika.

``/proc`` pseudo sistem datoteka, obezbeđuje podatke o procesima i kernelu u obliku datoteka.

``/opt`` predviđeno mesto za instalaciju third party aplikacija.


Apsolutna putanja precizno opisuje lokaciju resursa u hijerarhiji, počinje od root direktorijuma i opisuje svaki direktorijum do ciljanog resursa ``/home/student/luka/file1``

Relativna putanja pokazuje lokaciju resursa u odnosu na aktuelni direktorijum. Npr ako se nalazimo u direktorijumu ``student`` onda datoteka file1 ima relativnu putanju ``luka/file1``


Karakteristike Linux fajl sistema:

1. Putanja ne sme da pređe 4095 bajta, uključujući / karaktere.

2. Svaka komponenta odvojena / karakterom ne sme da pređe 255 karaktera.

3. Ime datoteke može da sadrži  UTF-8  kodirani karakter osim / i NUL.

4. Linux sistemi datoteka ext4, xfs, btrfs, gfs2, GlusterFS su osetljivi na mala i velika slova.

5. VFAT, NTFS, HFS+ nisu osetljivi na mala i velika slova ali se kaže da su Case Preserving.


``pwd`` ispisuje apsolutnu putanju direktorijuma u kojem se nalazi korisnik.

``ls`` lista sadržaj foldera. Opcije -a (prikazuje skrivene resurse), -h (prikaz veličine sadržaja u čitljivom formatu), -l (long listing), -R (rekurzivni prikaz)

``cd`` menja aktuelni direktorijum. Specificni objekti u svakom direktorijumu ``.`` reprezentuje aktuelni direktorijum , ``..`` reprezentuje roditeljski direktorijum. ``~`` reprezentuje home folder korisnika.

``touch file1`` kreira datoteku. ``touch file1 file2 ... fileN`` kreira vise datoteka.

``mkdir folder1`` kreira folder1 u aktuelnom direktorijumu. Može i uz pomoć apsolutne putanje. Sa parametrom -p kreira folder i kreira roditeljski folder ako ne postoji u putanji.

Preimenovanje:

``mv <izvorisni resurs> <odredisni resurs>`` izvodi se u istom direktorijumu.

Kopiranje:

``cp [-r] <izvorisni resurs> <odredisni resurs>`` za kopiranje direktorijuma obavezna je opcija -r .

Brisanje:

``rm -[rf] <resurs1>...<resursN>`` -f argument za brisanje bez potvrde. -r za direktorijume. Postoji i komanda ``rmdir`` za brisanje direktorijuma.

File Name Globbing

Tehnologija slična regularnim izrazima ali ne toliko moćna. Omogućava lociranje više datoteka odjednom korišćenjem jednog izraza.

``*`` bilo koji string dužine 0 ili više karaktera.

``?`` bilo koji pojedinacni karakter.

``[[:alpha:]]`` bilo koji alfabetic karakter

I još dosta opcija. Potražiti na internetu.

Uputstva za rad - MAN
---------------------

Manual pages

``man <sekcija> <pojam>`` npr: ``man passwd``

Brojevi man sekcija:

1. Executable programs and shell commands.
2. System calls provided by the kernel.
3. Library calls provided by program libraries.
4. Device files (usually stored in /dev).
5. File formats
6. Games
7. Miscellaneous (macro packages, conventions, and so on).
8. System administration commands (programs run mostly or exclusively by root).
9. Kernel routines.

Kretanje kroz man stranice:

Up/Down za jednu liniju. PageUp/PageDown za ceo ekran. /<string> pretražuje string. n ponavlja pretragu na dole, N ponavlja pretragu na gore.
g ili CTRL+HOME ide na početak man stranice. G ili CTRL+END ide na kraj man stranice.  q izlaz iz programa.

Čitanje dokumentacije u direktorijumu /usr/share/doc

Kada se paket instalira, datoteke koje čine dokumentaciju se nalaze u direktorijumu ``/usr/share/doc/<nazivpaketa>``


Rad sa tekstualnim datotekama
-----------------------------

``vi`` tekst editor podrazumevano instaliran na svim UNIX-like operativnim sistemima.

i - insert mode.

o - overwrite mode.

:w -snimanje datoteke.

:wq - izlazak i snimanje.

:q! izlazak iz datoteke bez snimanja.

ESC prelazak u komandni mod.

Up/Down/Left/Right PageUp/PageDown Home/End - Prelazak na početak i kraj reda.

w napred jednu reč.

b nazad jednu reč.

G skok na kraj teksta.

gg skok na početak teksta.

155G skok na liniju 155

H povratak na vrh ekrana.

M prelazak na sredinu ekrana.

L prelazak na dno ekrana-

:set number prikaz brojeva linija.

Pretraga: / <text> od početka do kraja datoteke.

n - next result
N - prethodni result.

yy - kopiranje čitave linije teksta.

p - paste.

u - undo.

``nano`` Podrazumevano instaliran na mnogim Linux distribucijama (Ubuntu,Debian...)

CTRL + x –Izlazak iz programa.

CRTL + o –Snimanje datoteke.

CTRL + w –Pretraga.

ALT + w –Pretraga sledećeg rezultata.

ALT + m + a –Omogućava selekciju teksta kursorima.

ALT + 6 –Kopiranje selektovanog teksta ili čitave linije, ako ništa nije selektovano.

CTRL + u –Paste.

``less`` Čitanje sadržaja datoteke, sa kretanjem unazad i unapred.

``cat`` Konkatenira (spaja) datoteke u jednu ili ispisuje sadržaj datoteke.

``cat file1 file2 > file3`` Spaja dve datoteke u jednu.

``join`` Kombinuje ispis dve datoteke prema identičnom polju sadržaja datoteke (ključu).

``paste`` Spaja datoteke liniju po liniju korišćenjem tab karakera.

``od`` Octal dump - prikazuje datoteku kao seriju bajtova.

``wc`` word count.

Regularni izrazi

Omogućavaju pretragu stringa korišćenjem kompleksnih metoda definisanja kriterijuma pretrage.
Potražiti dodatne resurse na internetu.

``grep`` Program za pretragu korišćenjem regularnih izraza.

``grep <regexp> [files]``

Korekcija datoteke Unix | Linux -> DOS | Windows stil

``sed s/$/”\r”/ unixfile.txt > dosfile.txt``

Korekcija datoteke  DOS | Windows  -> Unix | Linux stil

``tr -d \\r < dosfile.txt > unixfile.txt``

Redirekcija - preusmeravanje standardnih ulaznih ili izlaznih kanala procesa.

stdin - standard input
stdout - standard output
strderr - standard error

Primeri redirekcije:

``ls -la >spisak1.txt`` Preusmerava standardni izlaz u datoteku spisak1.txt

``ls -la >>spisak1.txt`` Dodaje standardni izlaz u datoteku spisak1.txt ; Ako datoteka ne postoji, biće kreirana;

Piping ulaza i izlaza

Piping je vezivanje izlaznog kanala procesa sa ulaznim kanalom drugog procesa. Praktično, rezultat rada jedne komande postaje ulazni argument druge komande.

``tee`` Omogućava istovremeni ispis rezultata komande na ekranu i preusmeravanje izlaza u datoteku. npr: ``ls -la /etc | tee /home/student/Desktop/etc.txt``


Upravljanje lokalnim korisničkim nalozima i grupama
---------------------------------------------------

Korisnički nalog
^^^^^^^^^^^^^^^^
Korisnički nalog reprezentuje korisnika na računarskom sistemu.
Implementira se kao objekat sa nekoliko značajnih atributa u datoteci /etc/passwd.
Da bi se prijavio na sistem, korisnik mora da zna atribute korisničko ime i lozinka.
Aktom prijavljivanja korisnika na sistem, vrši se njegova autentifikacija.
Kada korisnik pokrene proizvoljan program, proces programa radi u bezbednosnom kontekstu korisnika koji ga je pokrenuo.
Pristup programa na neku datoteku ili direktorijum kontroliše se pomoću ovlašćenja koja se dodeljuju korisničkim nalozima.
Prilikom pristupa nekog procesa na datoteku ili folder vrši se poređenje bezbednosnog konteksta programa i ovlašćenja dodeljenih resursu.

Komanda ``id`` omogućava pregled podataka o trenutno prijavljenom korisniku.

Komanda ``ls -l``, omogućava pregled informacija o dodeljenim ovlašćenjima i vlasništvu nad objektima.

Komanda ``ps -eF``, omogućava pregled liste aktivnih procesa i njihov bezbednosni identitet.

Format datoteke /etc/passwd

``root:x:0:0:root:/root:/bin/bash``

S leva na desno korisnički nalog ima 7 atributa (polja):
Korisničko ime,
Lozinka (Lozinka se danas nalazi u datoteci /etc/shadow.)
UID - User ID korisnika,
GID - Group ID primarne grupe korisnika,
GECOS polje - proizvoljan tekst, obično ime i prezime korisnika.
Putanja do Home direktorijuma korisnika.
Shell program korisnika.

Grupe
^^^^^

Korišćenje grupa, pojednostavljuje proces dodele ovlašćenja.
Implementira se kao objekat sa nekoliko značajnih atributa u datoteci /etc/group.
Svaki korisnik sigurno pripada tačno jednoj tzv. primarnoj grupi.
Primarnu grupu određuje identifikacioni broj GID - četvrto polje passwd datoteke.
Podrazumevani naziv primarne grupe korisnika je isti kao i njegovo korisničko ime.
Korisnik može biti član jedne ili više dodatnih grupa.

Format datoteka /etc/group

.. code-block:: bash

 root:x:0:
 mail:x:12:postfix


S leva na desno ima četiri atributa (polja):
Naziv grupe,
Lozinka grupe,
GID - Group ID,
Lista članova grupe.


root nalog
^^^^^^^^^^

Nalog koji ima pristup na sistem bez ograničenja, zove se root.
Grubo sličan nalogu Administrator u Windows-ima.
Nalog root ima sve moguće administrativne privilegije, ostali nalozi podrazumevano nemaju nikakve administrativne privilegije.
Iz bezbednosnih razloga ne preporučuje se direktno prijavljivanje na root nalog.
Umesto toga, korisnik koji obavlja administraciju, treba da se prijavljuje sa sopstvenim nalogom bez administrativnih privilegija.
Pomoću programa su ili sudo moguće je privremeno pribaviti neophodne administrativne privilegije.

``su`` – pokreće shell u kontekstu drugog korisnika. Bez argumenata pokreće shell kao root i traži lozinku za root. ``exit`` komadom se shell vraća natrag na prethodni identitet.

``sudo`` – pokreće komandu u kontekstu drugog korisnika.Ima konfiguracionu datoteku sudoers (/etc/sudoers) u kojoj se nalaze konfiguracioni podaci.

Upravljanje lokalnim korisničkim nalozima
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``useradd`` Kreira korisnički nalog. Koristi razuman skup podrazumevanih vrednosti, ako se koristi bez opcija. Podrazumevano ne podešava lozinku. Korisnik ne može da se prijavi dok se lozinka ne podesi.


``usermod`` Modifikuje svojstva korisničkog naloga.

Opcije za ``usermod`` :

-c Podešava GECOS polje naloga.

-g Specificira GID.

-G Definiše listu dodatnih grupa kojima korisnik pripada.

-d Definiše novi Home direktorijum korisnika.

-m Prebacuje Home direktorij korisnika. Koristi se sa d opcijom.

-s Definiše alternativni login shell.

-L Zaključava lozinku.

-U Otključava lozinku.

Listanje grupa kojima pripada korisnik : ``groups`` , ``lid``


``userdel`` Briše korisnički nalog.

``userdel -r luka`` Briše nalog luka i njegov home folder /home/luka istovremeno.


Opsezi UID vrednosti:

UID 0 - root nalog.

UID 1-200 - sistemski naloz dodeljeni sistemskim procesima od strane REDHAT-a.

UID 201-999 - Sistemski nalozi koje koriste sistemski procesi koji nemaju vlasništvo nad datotekama. Koriste se za ograničavanje pristupa na resurse sistema datoteka.

UID 1000+ - regularni korisnici.


Upravljanje lokalnim grupama
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``groupadd`` kreira grupu.

``groupmod`` modifikuje postojeću grupu, npr: ``groupmod -n phpapp webapp`` - Menja ime grupe iz webapp u phpapp; ``groupmod -g 8700 ljudskiresursi`` - Menja GID grupe ljudskiresursi u 8700.

``groupdell`` briše grupu. userdel prema mom iskustvu briše i primarnu grupu korisnika ako je istoimena i ako nije nikada promenjena primarna grupa.


Upravljanje lozinkama
^^^^^^^^^^^^^^^^^^^^^

Smeštene u datoteku /etc/shadow u kriptovanom obliku. Hash funkcije i Salt se koriste za implementaciju kriptovanja lozinke.

Pogledajmo lozinku definisanu shadow datotekom:

$1$gCjLa2/Z$6Pu0EK0AzfCjxjv2hoLOB/

$1$, definiše verziju hash algoritma:
    1 - MD5, 2a - ExBlowfish, 2 - Blowfish, 5 - SHA256,  6 - SHA512.

$gCjLa2/Z$

    Salt, nastaje nasumično.

6Pu0EK0AzfCjxjv2hoLOB/

    Kriptovan hash kombinacije lozinka/salt.


Promena podrazumevanog hash algoritma se može izvršiti pomoću programa authconfig:

``authconfig --passalgo md5|sha256|sha512 --update``  Na CentOS7 podrazumevani Hash algoritam je SHA-512.


Format datoteke  /etc/shadow

Login Name.
Kriptovana lozinka.
Datum poslednje promene lozinke.
Minimalan životni vek lozinke (dana).
Maksimalan životni vek lozinke (dana).
Period upozorenja na vremensko isticanje lozinke (dana) u odnosu na maksimalan vek.
Broj dana neaktivnosti korisnika nakon isticanja lozinke posle kog dolazi do zaključavanja  lozinke.
Datum isteka naloga.
Polje rezervisano za buduće potrebe.

Podrazumevane vrednosti zastarevanja lozinke:

Definiše datoteka /etc/login.defs
Datoteku arhivirati pre promene sadržaja.
Podrazmevane vrednosti:
PASS_MAX_DAYS 		99999
PASS_MIN_DAYS		0
PASS_WARN_AGE		7

``chage`` change password age. Program omogućava podešavanje politike zastarevanja lozinke za neki nalog.

``chage -m <MIN_D> -M <MAX_D> -W <WARN_D> -I <INACTIVE D> -E <YYYY-MM-DD> <username>``

``chage -m 5 -M 45 -W 6 marko``

Minimalan vek lozinke je 5 dana.
Masimalan vek lozinke je 45 dana.
Period upozorenja na isticanje je 6 dana pre isteka lozinke.
Nalog na koji se politika odnosi je marko

``chage -E 20180705 luka``

Lozinka ističe 5. jula 2018.

Forsiranje promene lozinke nalogu kod narednog prijavljivanja:  ``chage -d 0 luka``

Zaključavanje/Otključavanje lozinke

Lozinka se može zaključati sa ``usermod -L <username>.``. Ovim se modifikuje polje lozinke.
!$1$gCjLa2/Z$6Pu0EK0AzfCjxjv2hoLOB/ −lozinka zaključana, primetiti ! ispred lozinke.
$1$gCjLa2/Z$6Pu0EK0AzfCjxjv2hoLOB/ −lozinka nije zaključana.

Međutim, ovo neće sprečiti korišćenje naloga u situacijama kada se ne koristi lozinka za prijavljivanje, npr. ssh PKI.
Da bi se nalog apsolutno onesposobio u svim situacijama, preporučuje se istovremeno zaključavanje lozinke i podešavanje vrednosti datuma isteka lozinke na 1.

``usermod -L -e 1 <username>.`` Ovim se datum isticanja naloga podešava na 01.01.1970.

Ponovno osposobljavanje naloga se može obaviti istovremenim otključavanjem lozinke odnosno uklanjanjem vremenskog isticanja lozinke:

``usermod -U -e -1 <username>``

Podešavanje podrazumevane konfiguracije lozinki

Datoteka /etc/security/pwquality.conf

Parametri:
difok =3 Where we say we need a minimum of 3 different characters in a new password when compared against the current password
minlen = 8 Where we require a password length of a minimum of 8 characters
ucredit = -1 Where we require a least 1 upper case letter
lcredit = -1 Where we require at least 1 lower case letter
dcredit = -1 Where we require at least 1 number
ocredit = -1 Where we require at least 1 non-alphanumeric character
minclass = 2 States that we need at least 2 classes or characters: )upper, lower, digit and other)
maxrepeat = 3 Where we reject passwords that have occurrences a 4 or more repeating identical characters
maxclassrepeat = 2 Where we reject passwords that have 3 or more consecutive characters of the same class


nologin shell
^^^^^^^^^^^^^

Ponekad je neophodno posedovati nalog koji ne zahteva interaktivni shell.
Primer, email autentifikacija i slično.
Uobičajeni postupak je da se korisniku podesi login shell na vrednost /sbin/nologin.
Kada korisnik pokuša da se prijavi, nologin shell zatvara vezu.

Primer podešavanja:

``usermod -s /sbin/nologin luka``


Upravljanje ovlašćenjima
------------------------


Ovlašćenja na nivou sistema datoteka
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Osnovni standard ovlašćenja, POSIX

+--------------------+-------------------------------------------+----------------------------------------------+
| Ovlašćenje         |  Efekat na datoteku                       | Efekat na direktorijum                       |
+--------------------+-------------------------------------------+----------------------------------------------+
| r - read           | Sadržaj datoteke je čitljiv               | Sadržaj direktorijuma može da se lista       |
+--------------------+-------------------------------------------+----------------------------------------------+
| w - write          | Sadržaj datoteke se može modifikovati     | Datoteke se mogu kreirati ili brisati        |
+--------------------+-------------------------------------------+----------------------------------------------+
| x - execute        | Datoteka sa može pokrenuti ako je izvršna | Preduslov za prelazak u direktorijm          |
+----------------------------------------------------------------+----------------------------------------------+

Preciznije:

- Ako korisnik ima samo exec ovlašćenje nad direktorijumom, ne može da vidi datoteke koje postoje u direktorijumu ali ako zna putanju do neke datoteke i ima read ovlašćenje za istu, može joj pristupiti.
- Ako korisnik ima samo read ovlašćenje nad direktorijumom, ne i read nad datotekama, može da vidi datoteke koje postoje u direktorijumu ali ne i druge atribute kao npr. Time Stamps.
- Datoteku može ukloniti korisnik koji ima write ovlašćenje nad direktorijumom u kome se datoteka nalazi. Ovo može biti problem.
- Ne postoji koncept nasleđivanja ovlašćenja.

Struktura ovlašćenja na nivou resursa

- Ovlašćenja za korisnički nalog vlasnika datoteke (Owner).
- Ovlašćenja za primarnu grupu vlasnika datoteke (Group owner). Ovo može da se promeni.
- Ovlašćenja za sve ostale korisnike (Others).
- Osnovno pravilo: Owner overrrides group, group overrides other.

Interpretacija komande ``ls -l`` :

.. code-block:: bash

 luka@luka-desktop:~$ ls -l
 total 68
 drwxrwxr-x 9 luka luka 4096 mar 28 11:16 Ansible
 drwxr-xr-x 3 luka luka 4096 apr 13 16:33 Desktop
 drwxrwxr-x 5 luka luka 4096 mar 20 12:32 DEVOPS
 drwxr-xr-x 2 luka luka 4096 mar  1 11:48 Documents
 drwxrwxr-x 8 luka luka 4096 mar  1 13:51 Dokumentacija
 drwxr-xr-x 2 luka luka 4096 mar  1 11:48 Downloads
 drwxrwxr-x 4 luka luka 4096 mar 21 16:27 ELKAnsible
 -rw-r--r-- 1 luka luka 8980 mar  1 11:36 examples.desktop
 drwxr-xr-x 2 luka luka 4096 mar  1 11:48 Music
 drwxr-xr-x 2 luka luka 4096 mar  1 11:48 Pictures
 drwxr-xr-x 2 luka luka 4096 mar  1 11:48 Public
 drwxrwxr-x 3 luka luka 4096 mar  1 13:11 PycharmProjects
 drwxr-xr-x 3 luka luka 4096 mar  1 13:09 snap
 drwxr-xr-x 2 luka luka 4096 mar  1 11:48 Templates
 drwxr-xr-x 2 luka luka 4096 mar  1 11:48 Videos
 luka@luka-desktop:~$

- Prva kolona: tip objekta i ovlašćenja

    Prvi karakter je tip objekta
    - file
    d – directory
    l – symbolic link
    p – named pipe
    s – socket
    b – block device
    c – character device
    Ostatak su ovlašćenja, npr.
    rw-r--r--.
- Druga kolona: Broj linkova,
- Treća kolona: Naziv vlasnika,
- Četvrta kolona: Grupa vlasnika,
- Peta kolona: Veličina,
- Šesta-osma kolona: datum i vreme poslednje modifikacije
- Deveta kolona: Naziv resursa

-Pogledajmo sledeći primer prve kolone listinga:

    -rwxrw-r--

Prvi karakter: Srednja crta na mestu prvog karaktera opisuje datoteku.

Serija rwx (-rwxrw-r--): Odnosi se na korisnički nalog vlasnika datoteke. Vlasnik ima read, write i execute.

Naredna serija rw- (-rwxrw-r--): Odnosi se na primarnu grupu vlasnika datoteke, ili na neku drugu grupu. Grupa ima read i write. Nema execute.

Poslednja serija r-- : Odnosi se na sve ostale korisnike. Imaju read ovlašćenje. Nemaju write niti execute.


Specijalna ovlašćenja
^^^^^^^^^^^^^^^^^^^^^

+---------------+-------------------------------+-------------------------+-------------------------------+
|Specijalni bit |Efekat nad datotekom           | Efekat nad folderom     | Identifikacija ls -l          |
+---------------+-------------------------------+-------------------------+-------------------------------+
|Set User ID -  |Izvršna datoteka se pokreće u  |Nema efekat              |Na mestu execute ovlašćenja za |
|SUID           |kontekstu vlasnika a ne u      |                         |vlasnika, vidi se  ili S. Ako  |
|               |kontekstu korisnika koji je    |                         |postoji exec, onda je s. Ako   |
|               |pokreće. Namenjeno serverskim  |                         |exec na postoji onda je S.     |
|               |i sistemskim aplikacijama.     |                         |                               |
+---------------+-------------------------------+-------------------------+-------------------------------+
|Set Group ID   |Izvršna datoteka se pokreće u  |Nove datoteke u folderu  |Na mestu execute ovlašćenja za |
|bit - SGID     |kontekstu primarne grupe       |imaju istu grupu         |grupu vlasnika, vidi se s ili  |
|               |vlasnika a ne u kontekstu      |vlasnika kao             |S. Ako postoji exec, onda je s |
|               |primarne grupe korisnika koji  |roditeljski folder.      |a ako ne postoji onda je S     |
|               |je pokreće.                    |                         |                               |
+---------------+-------------------------------+-------------------------+-------------------------------+
|Sticky bit     | Nema efekat                   |Datoteke u folderu mogu  |Na mestu execute ovlašćenja za |
|               |                               |biti obrisane samo od    |ostale, vidi se t ili T. Ako   |
|               |                               |strane vlasnika datoteke |postoji exec onda je t. Ako ne |
|               |                               |,vlasnika foldera ili    |postoji exec onda je T.        |
|               |                               |root naloga.             |                               |
+---------------+-------------------------------+-------------------------+-------------------------------+

Nekoliko primera identifikacije specijalnih ovlašćenja:

- rwsr--r--
    Vlasnik ima rwx, SUID je podešen, grupa ima r, ostali imaju r.

- rwSr--r--
    Vlasnik ima rw, SUID je podešen, grupa ima r, ostali imaju r.

- rwxrws---
    Vlasnik ima r, w, x, grupa ima r, w, x, podešen je SGID, ostali nemaju ovlašćenja.

- rw-rw-r-t
    Vlasnik ima r, w, grupa ima r, w, ostali imaju r, x, podešen je Sticky Bit.

Upravljanje ovlašćenjima iz komandne linije
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Numerički metod promene ovlašćenja

+-------------------------+-----------------------+----------------------------+
| Korisnik                | Grupa                 | Ostali                     |
+-------------------------+-----------------------+----------------------------+
| 4 - r ; 2- w ; 1 - x    | 4 - r ; 2- w ; 1 - x  | 4 - r ; 2- w ; 1 - x       |
+-------------------------+-----------------------+----------------------------+

Ovlašćenja se modifikuju pomoću programa ``chmod``.

Sintaksa:  ``chmod <korisnik><grupa><ostali> <datoteka>``

Primer : ``chmod 740 file1``

- korisnik: 4+2+1 = 7 (read+write+exec)
- grupa: 4+0+0 = 4 (read + 0 + 0)
- ostali: 0+0+0 = ništa

Numerički metod promene specijalnih ovlašćenja

+------------+----------+-----------+
|SUID        |SGID      |Sticky     |
+------------+----------+-----------+
|4           |2         |1          |
+------------+----------+-----------+
|s           |s         |T          |
+------------+----------+-----------+

Ovlašćenja se modifikuju pomoću programa ``chmod``.

Sintaksa:  ``chmod <specijalni bitovi><korisnik><grupa><ostali> <datoteka>``

Primer : ``chmod 7740 file3``

- Specijalni bitovi: 4+2+1 = 7 (SUID+SGID+Sticky)
- korisnik: 4+2+1 = 7 (read+write+exec)
- grupa: 4+0+0 = 4 (read + 0 + 0)
- ostali: 0+0+0 = ništa

Simbolički metod promene ovlašćenja

+----------------+----------+------------+------------+-----------+--------------------------------------+
|Skup ovlašćenja |Značenje  |Kod promene |Značenje    |Ovlašćenja |Značenje                              |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|u               |Vlasnik   |``+``       |Dodavanje   |r          | Read                                 |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|g               |Grupa     |``-``       |Uklanjanje  |w          | Write                                |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|o               |Ostali    |``=``       |Jednakost   |x          | Execute                              |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|a               |Svi       |            |            |s          | SUID ili SGID                        |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|                |          |            |            |t          | Sticky                               |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|                |          |            |            |u          |Postojeća ovlašćenja vlasnika         |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|                |          |            |            |g          |Postojeća ovlašćnja grupe             |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|                |          |            |            |o          |Postojeća ovlašćenja za ostale        |
+----------------+----------+------------+------------+-----------+--------------------------------------+
|                |          |            |            |X          | Kod rekurzivne primene, ovlašćenje   |
|                |          |            |            |           | exec se odnosi samo na direktorijume.|
+----------------+----------+------------+------------+-----------+--------------------------------------+


Primer (rwxr-----) :

- Numerički: ``chmod 740 file1``

- Simbolički: ``chmod u=rwx,g=r,o= file1``

Dodavanje execute ovlašćenja za vlasnika:

``chmod u+x file1``

Ako u se u izlazu komande ls -l na mestu execute ovlascenja nalazi:

S - ovlašćenje exec nedostaje a SUID ili SGID su podeseni

s - ovlašćenje exec je konfigurisano a SUID ili SGID su podeseni

T - ovlascenje exec nedostaje a Sticky bit je podesen

t - ovlascenje exec je konfigurisano a Sticky bit je podesen


Promena vlasništva nad datotekom
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Podrazumevani vlasnik je onaj ko kreira datoteku.
- Podrazumevana grupa vlasnika je primarna grupa korisnika.
- Promena vlasnika datoteke:
    ``chown petar file1``
- Promena grupe vlasnika:
    ``chown :finansije file2``
- Rekurzivna promena vlasništva:
    ``chown -R petar folder1``
- Istovremena promena vlasnika i grupe vlasnika:
    ``chown petar:finansije file3``
- Samo root ima pravo promene naloga vlasnika.
- Grupu vlasnika može promeniti root ili nalog vlasnik.
- Root može dodeliti vlasništvo bilo kome.
- Ostali korisnici mogu dodeliti vlasništvo samo grupama kojima pripadaju.

.. note:: Ako kroz vi editor pokusamo da modifikujemo datoteku nad kojom nemamo write ovlascenje, vi ce ponuditi korisniku da obriše postojeću datoteku i da na njeno mesto kreira drugu novog imena (:wq!). Ovo je moguće ako korisnik koji snima datoteku ima write za folder u kome se datoteka nalazi.


Upravljanje podrazumevanim ovlašćenjima
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Podrazumevana ovlašćenja za datoteke konfigurišu procesi koji kreiraju datoteke.
        Na primer, tekst editori kreiraju datoteke tako da se mogu čitati i pisati (read, write) ali ne tako da se mogu pokretati, nedostaje execute ovlašćenje.
- Podrazumevana ovlašćenja koja se ne smeju podesiti iz shell procesa, mogu se konfigurisati ``umask`` komandom.
- Komanda ``umask`` bez argumenata pokazuje aktuelnu konfiguraciju.
- Vrednost je oktalna bitmaska.

Primer: 0002

-Prvi broj reprezentuje specijalna ovlašćenja,

-Drugi broj ovlašćenja za vlasnika,

-Treći broj ovlašćenja za grupu,

-Četvrti broj ovlašćenja za ostale korisnike.


Modifikovanje umask-e, numerički

``umask 0<vlasnik><grupa><ostali>``

Numeričke vrednosti maske i njeno značenje su dati u tableli:

+-----+--------------------------------------------------------------+
|0    | Može se podesiti bilo koje ovlašćenje.                       |
+-----+--------------------------------------------------------------+
|1    | Execute ovlašćenje se ne sme podesiti.                       |
+-----+--------------------------------------------------------------+
|2    | Write ovlašćenje se ne sme podesiti.                         |
+-----+--------------------------------------------------------------+
|3    | Write i Execute ovlašćenja se ne smeju podesiti              |
+-----+--------------------------------------------------------------+
|4    | Read ovlašćenje se ne sme podesiti.                          |
+-----+--------------------------------------------------------------+
|5    | Read i Execute ovlašćenja s ne smeju podesiti.               |
+-----+--------------------------------------------------------------+
|6    | Read i Write se ovlašćenja se ne smeju podesiti.             |
+-----+--------------------------------------------------------------+
|7    |Nijedno ovlašćenje se ne sme podesiti.                        |
+-----+--------------------------------------------------------------+

Primer: ``umask 0027``

-Sva specijalna ovlašćenja se mogu podesiti.

-Vlasniku se mogu dodeliti sva ovlašćenja.

-Grupi se ne može dodeliti Write.

-Ostalima se ne mogu dodeljivati ovlašćenja.

Rezime podrazumevanih ovlašćenja

-Ovlašćenja za foldere definiše podrazumevana umask vrednost.

    -Provera: ``umask``

    -0002 za korisnike sa user ID-jem > 199, za neprivilegovane korisnike

    -0022 za korisnike sa user ID-jem <=199, za privilegovane korisnike uključujući root nalog

-Ovlašćenja za datoteke određuju procesi koji ih kreiraju.

-Nema načina da se odvoje podrazumevane vrednosti za različite tipove objekata.


Konfiguracione datoteke koje definišu podrazumevane vrednosti

-Za bash shell na nivou sistema:

    ``/etc/profile``

    ``/etc/bashrc``

-Za bash shell korisnika:

    ``~/.bash_profile``

    ``~/bashrc``

-Konfiguracija bash shell-a korisnika je efektivna na posmatranog korisnika


Napredno upravljanje ovlašćenjima
---------------------------------

POSIX liste za kontrolu pristupa (Access Control List)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Koncept liste za kontrolu pristupa

- Standardna Linux ovlašćenja nisu dovoljna za opis željenih prava pristupa u svim situacijama.
- Dodatna ovlašćenja se mogu podesiti korišćenjem liste za kontrolu pristupa (Access Control List - ACL).
- Ovlašćenja se mogu dodeljivati korisnicima i grupama prema nazivu ili prema atributima UID i GID, respektivno.
- Mogu se dodeljivati ovlašćenja r, w, x.
- Vlasnik resursa može dodeljivati ovlašćenja.
- Nove datoteke i poddirektorijumi mogu automatski nasleđivati podrazumevana ovlašćenja ako su podešena.

Veza sa montiranjem sistema datoteka

- Da bi liste za kontrolu pristupa mogle da se koriste, sistem datoteka mora bit montiran sa opcijom za podršku korišćenju listi za kontrolu pristupa.
- XFS sistem datoteka ima podrazumevanu podršku.
- Ext4 sistem datoteka na RHEL-u 7 koristi opciju acl, podrazumevano.
- Ext4 sistem datoteka na starijim verzijama  RHEL-a, može zahtevati eksplicitno korišćenje opcije acl prilikom montiranja sistema datoteka.

Pregled i interpretacija ovlašćenja

Primetimo sledeći rezultat komande ``ls -l``:

.. code-block:: bash

 -rwxrw----+  1  mmarko mmarko  138  Mar  19  23:56 file1.txt

Primetimo karakter + na kraju stringa koji pokazuje standardna ovlašćenja nad datotekom.

Prisustvo karaktera + indicira da postoji lista za kontrolu pristupa za ovu datoteku.

Pomoću komande getfacl moguće je saznati sva ovlašćenja za datoteku:

``getfacl file1.txt``

Pogledajmo rezultat komande:

.. code-block:: bash

 luka@luka-desktop:~/DEVOPS/bootcamp$ getfacl Makefile
 # file: Makefile
 # owner: luka
 # group: luka
 user::rw-
 group::rw-
 other::r--

 luka@luka-desktop:~/DEVOPS/bootcamp$

Vlasnik je korisnik lalas.

Vlasnik grupa je lalas.

Vlasnik ima ovlašćenje rw.

Korisnik mmarko ima ovlašćenje r.

Vlasnik grupa ima ovlašćenje rw.

Grupa marketing ima ovlašćenje rx.

Maska je rwx.

Ostali imaju ovlašćenje r.

Maska
^^^^^

- Maska ograničava efektivno ovlašćenje vlasnik grupe kao i korisnika i grupa kojima su ovlašćenja eksplicitno konfigurisana.
- Maska ne uvodi ograničenja za vlasnika datoteke niti za ostale korisnike.
- Izračunava se automatski kod svake promene ovlašćenja.
- Može biti nasleđena od podrazumevane maske naddirektorijuma.
- Može se manuelno podesiti ``setfacl`` komandom


Algoritam provere prava pristupa

1. Ako User ID procesa odgovara vlasniku datoteke, onda su efektivna ovlašćenja dodeljena vlasniku.
2. Ako User ID procesa odgovara nekom korisniku kome su eksplicitno dodeljena ovlašćenja za datoteku, onda su efektivna ovlašćenja dodeljena tom korisniku.
3. Ako jedan od Group ID-jeva procesa odgovara vlasnik grupi datoteke, a vlasnik grupa ima tražena ovlašćenja, onda su efektivna ovlašćenja dodeljena vlasnik grupi.
4. Ako jedan od Group ID-jeva procesa odgovara nekoj grupi kojoj su eksplicitno dodeljena tražena ovlašćenja za datoteku, onda su efektivna ovlašćenja dodeljena toj grupi.
5. Ako jedan od Group ID-jeva procesa odgovara vlasnik grupi ili nekoj grupi kojoj su eksplicitno dodeljena ovlašćenja za datoteku, a pri tome ni vlasnik grupa niti bilo koja druga grupa kojoj su eksplicitno dodeljena ovlašćenja nema tražena ovlašćenja, onda je pristup zabranjen.
6. Ovlašćenja data ostalim korisnicima (other) određuju pravo pristupa.
7. Ako je rezultat prethodnog algoritma takav da pristup ima vlasnik ili ostali korisnici (others), pristup je dozvoljen.
8. Ako je rezultat prethodnog algoritma takav da pristup imaju korisnik kome su eksplicitno data ovlašćenja, vlasnik grupa ili grupa kojoj su eksplicitno data ovlašćenja, pri čemu istovremeno maska ima tražena ovlašćenja, pristup je dozvoljen.
9. U suprotnom, pristup je zabranjen.

Identitet korisnika se može saznati pomoću komande ``id``.

.. note:: Da bi algoritam ispravno funkcionisao za ovlašćenja dodeljena direktorijumima, potrebno je da svaka grupa ima execute ovlašćenje zbog listanja sadržaja.

Podrazumevana ovlašćenja i maska

- Podrazumevana ovlašćenja i maska se mogu konfigurisati na nivou direktorijuma.
- Omogućavaju definisanje inicijalnih ovlašćenja i maske za buduće datoteke i poddirektorijume.
- Mogu se podesiti za vlasnika, vlasnik grupu, proizvoljnog korisnika, proizvoljnu grupu i ostale korisnike.

Podešavanje ovlašćenja
^^^^^^^^^^^^^^^^^^^^^^

Primeri:

``setfacl -m u::rx file1.txt``   -  Konfiguriše rx ovlašćenje za vlasnika datoteke file1.txt.

``setfacl -m u:mmarko:r file1.txt``   -  Konfiguriše r ovlašćenje za korisnika mmarko nad datotekom file1.txt.

``setfacl -m g::rx file1.txt``    -   Konfiguriše rx ovlašćenje za vlasnik grupu datoteke file1.txt.

``setfacl -m g:marketing:r file1.txt``    -   Konfiguriše r ovlašćenje za grupu marketing nad datotekom file1.txt.

``setfacl -m o::- file1.txt``  - Blokira pristup ostalim korisnicima nad datotekom file1.txt.

Višestruke promene ovlašćenja:

``setfacl -m u::rwx,g:marketing:r,o::- file1.txt``   -  Podešava ovlašćenja za vlasnika, grupu marketing i ostale u jednoj liniji u comma-separated maniru.

Kopiranje ovlašćenja:

``getfacl file1.txt | setfacl --set-file=. file2.txt``   -  Kopira ovlašćenja datoteke file1.txt na datoteku file2.txt

Podešavanje maske:

``setfacl -m m::rw file1.txt``  -  Podešava masku na vrednost rw.

Rekurzivne promene nad folderima:

``setfacl -R -m u:ppetar:rwX .``  -  Podešava ovlašćenje rw korisniku ppetar nad svim podobjektima aktuelnog direktorijuma.

Brisanje iz ACL-a:

``setfacl -x u:ppetar,g:marketing .``


Promena podrazumevanih ovlašćenja

- Promena podrazumevanih ovlašćenja za korisnika.

    ``setfacl -m d:u:mmarko:rx .``  -  Konfiguriše podrazumevana ovlašćenja rx za korisnika mmarko nad aktuelnim direktorijumom.

- Promena podrazumevanih ovlašćenja za grupu.

    ``setfacl -m d:g:marketing:r .``  -  Konfiguriše podrazumevana ovlašćenja r za grupu marketing nad aktuelnim direktorijumom.


Kontrolisanje rada servisa i demona
-----------------------------------

init
^^^^

- Tradicionalno, na Unix operativnim sistemima init (initialization) je prvi proces koji se startuje u toku podizanja operativnog sistema.
- Kernel startuje init.
- Proces init radi dok se računar ne ugasi i postaje direktni ili indirektni predak svih drugih procesa.
- Postoje različite implementacije: System III, System V, Research Unix, BSD.
- Najveći broj Linux distribucija koristi implementaciju donekle kompatibilnu sa System V implementacijom tzv. sysvinit.
- Originalni sinhroni dizajn, sprečava sysvinit da elegantno upravlja zadacima karakterističnim za moderne računare:

    1. Dodavanje i uklanjanje USB uređaja,
    2. Skeniranje novih uređaja za skladištenje podataka, itd.


upstart
^^^^^^^

- Upstart je zamena za tradicionalni init demon, koja se oslanja na koncept upravljanja baziran na događajima.
- Praktično, upstart radi asinhrono.
- Ima odličnu kompatibilnost sa sysvinit demonom.
- Datira iz 2006. i do danas je usvojen na određenom broju distribucija.
- Upstart koriste:
    Ubuntu 6.10 i noviji.
    Fedora 9-14,
    RHEL6 i svi derivati koji odgovaraju ovoj verziji,
    openSUSE 11.3 Milestone 4 ali kao opcioni

systemd
^^^^^^^

- Takođe, zamena za sysvinit.
- Datira iz 2010. godine.
- Kontraverze u vezi napuštanja Unix filosofije (short, simple, clear, modular…)
- Kritike sličnosti sa Microsoft-ovim svchost-om.
- Linux distribucije adaptirane da koriste systemd, podrazumevano:
    Fedora 15, RHEL 7, CENTOS 7,
    Debian 8 (26.04.2015),
    SUSE Linux Enterprise server 12,
    Ubuntu v15.04,
    itd.
- Postaje defakto **Linux standard**.

Osnovi systemd-a

- Prvi proces koji startuje kernel je systemd, PID 1.
- Daljim podizanjem sistema upravlja systemd.
- Ovaj program obezbeđuje metode za aktivaciju sistemskih resursa, serverskih demona i drugih procesa, prilikom podizanja sistema ali i kasnije, u toku rada računara.
- Demoni su procesi koji čekaju ili rade u pozadini, obavljajući različite zadatke.
- Demoni se tipično startuju prilikom podizanja sistema a zaustavljaju prilikom gašenja računara ili na manuelnu intervenciju.
- Za slušanje mrežnih konekcija, demon koristi socket.
- Socket je primarni komunikacioni kanal sa lokalnim ili udaljenim klijentima.
- Socket može kreirati demon, međutim kreiranje socket-a može biti odvojeno od demona.
- Proces systemd, može kreirati socket.
- Socket se prosleđuje demonu kada se uspostavlja veza od strane klijenta.
- Izraz servis se koristi za pokazivanje na jedan ili više demona.

Prednosti systemd-a

- Paralelizacija aktivnosti koja dovodi do znatno kraćeg vremena neophodnog za podizanje operativnog sistema.
- Startovanje demona na zahtev, bez potrebe za posedovanjem dodatnog servisa.
- Automatsko upravljanje zavisnostima između servisa, koje sprečava duge tajmaut-e.
- Postojanje metode praćenja povezanih procesa korišćenjem Linux kontrolnih grupa.


systemctl i systemd jedinica

- Komanda ``systemctl`` se koristi za upravljanje različitim tipovima systemd objekata koje se nazivaju systemd jedinice (systemd units).
- Lista systemd jedinica se može prikazati komandom systemctl -t help.
- Neke od uobičajenih vrsta jedinica su:
    - Servisne jedinice, imaju ekstenziju .servis, koriste se za startovanje najčešće korišćenih demona.
    - Socket jedinice, imaju .socket ekstenziju, reprezentuju IPC. Kontrola nad Socket-om se prosleđuje demonu kada dođe do uspostavljanja veze sa klijentom.
    - Path jedinice, imaju .path ekstenziju, i koriste se za odlaganje aktivacije servisa dok ne dođe do specifične promene na sistemu datoteka. Uobičajeno ih koristi sistem za štampanje.

Praćenje stanja servisa pomoću komande systemctl

Sintaksa:

``systemctl status <name>.<type>``

Komanda systemctl status zamenjuje komandu:

 ``service <name> status``

Statusna stanja su data u tabeli:

+-----------------+----------------------------------------------------------------------------------------------------+
|loaded           | Završeno je procesiranje konfiguracione datoteke jedinice.                                         |
+-----------------+----------------------------------------------------------------------------------------------------+
|active (running) |Radi sa jednim ili više procesa.                                                                    |
+-----------------+----------------------------------------------------------------------------------------------------+
|active (exited)  |Uspešno završena jednokratna konfiguracija.                                                         |
+-----------------+----------------------------------------------------------------------------------------------------+
|active (waiting) | Radi, ali čeka na neki događaj.                                                                    |
+-----------------+----------------------------------------------------------------------------------------------------+
|inactive         | Ne radi.                                                                                           |
+-----------------+----------------------------------------------------------------------------------------------------+
|enabled          | Startovaće se u toku podizanja operativnog sistema.                                                |
+-----------------+----------------------------------------------------------------------------------------------------+
|disabled         | Neće se startovati u toku podizanja operativnog sistema.                                           |
+-----------------+----------------------------------------------------------------------------------------------------+
|static           | Ne može se promeniti u enabled. Mada, može biti startovan od strane drugog servisa koji je enabled.|
+-----------------+----------------------------------------------------------------------------------------------------+


Ostale komade u vezi sa praćenjem statusa servisa:

- Listanje svih servisnih jedinica:

``systemctl list-units --type=service``

``systemctl list-units --type=socket -all``

- Provera da li je servis enabled:

``systemctl is-enabled sshd``

- Provera da li je servis aktivan:

``systemctl is-active sshd``

- Listanje servisa koji se nisu podigli:

``systemctl --failed``

- Lista svih instaliranih servisnih jedinica:

``systemctl list-unit-files --type=service``


Kontrola sistemskih servisa
^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Sintaksa za startovanje servisa:

``systemctl start <naziv servisa>.service``

- Primeri startovanja servisa:

``systemctl start firewalld.service``

``systemctl start httpd.service``

- Sintaksa za stopiranje servisa:

``systemctl stop <naziv servisa>.service``

- Primeri stopiranja servisa:

``systemctl start firewalld.service``

``systemctl start httpd.service``

- Primeri restartovanja servisa:

``systemctl restart firewalld.service``

``systemctl restart httpd.service``

**Reloading je proces ponovnog čitanja konfiguracione datoteke servisa. ID procesa se ne menja.**

- Primeri reloading-a servisa:

``systemctl reload firewalld.service``

``systemctl reload httpd.service``


Zavisnost između systemd jedinica

- Servisi mogu biti startovani kao posledica međusobne zavisnosti servisa.
- Takođe, postoji i međusobna zavisnost između različitih tipova servisnih jedinica.
- Ako se startuje Socket jedinica nekog imena, automatski se startuje servisna jedinica istog imena.
- Startovanje servisa može započeti jedinica tipa path kao rezultat događaja na sistemu datoteka.
- Na primer, kada nastane datoteka u print spool direktorijumu, startuje se cups print service.
- Stopiranje servisa neće nužno zaustaviti i sve druge vrste systemd jedinica koje ga mogu startovati.

``systemctl list-dependencies`` :

- Štampa stablo systemd jedinica od kojih zavisi servis.
    Sintaksa:

    ``systemctl list-dependencies <naziv jedinice>``

    Primer:

    ``systemctl list-dependencies cups.service``

- Opcija reverse pokazuje jedinice koje zavise od posmatrane jedinice.

    ``systemctl list-dependencies cups.service --reverse``

Maskiranje servisa

- Moguća je situacija u kojoj imamo više instaliranih, međusobno konfliktnih servisa.
- Na primer iptables i firewalld.
- Da bi se sprečilo da administrator slučajno startuje neželjeni servis, isti se može maskirati.
- Maskiran servis se nikako ne može startovati dok se ne demaskira.
- Maskiranje servisa

    ``systemctl mask iptables``

- Demaskiranje servisa:

    ``systemctl unmask iptables``

Promene statusa servisa disabled/enabled

- Sprečavanje servisa da se podiže automatski tokom podizanja operativnog sistema:

    ``systemctl disable sshd.service``

- Ovom akcijom se servis ne stopira trenutno.

- Konfigurisanje servisa da se podiže automatski tokom podizanja operativnog sistema:

    ``systemctl enable sshd.service``

- Ovom akcijom se servis ne startuje trenutno.


Konfiguracija servisa
^^^^^^^^^^^^^^^^^^^^^

- Konfiguracione datoteke systemd jedinica se nalaze na putanji /usr/lib/systemd/system.
- Posmatrajmo sadržaj datoteke chronyd.service koja reprezentuje konfiguraciju NTP klijent/server servisa:

.. code-block:: bash

 [Unit]
 Description=Network Time Service
 After=syslog.target ntpdate.service sntp.service

 [Service]
 Type=forking
 EnvironmentFile=-/etc/sysconfig/ntpd
 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS
 PrivateTmp=true

 [Install]
 WantedBy=multi-user.target

- Description: Opis servisa.
- After: Koje jedinice zavise od ove jedinice.
- Conflicts: lista konfliktnih jedinica. Startovanje jedne stopira drugu i obrnuto.
- ExecStart: Definiše putanju komande koja pokreće zajedno sa argumentom i opcijama.
- WantedBy: Koji servisi zavise od ovih servisa.

.. note:: Ove konfiguracione datoteke ne treba editovati.

.. note:: U slučaju potrebe za promenom konfiguracije servisa, servisna datoteka se kopira iz foldera /usr/lib/systemd/system u folder /etc/systemd/system i modifikuje. Na ovoj lokaciji imaju viši prioritet od datoteka u folderu /usr/lib/systemd/system/.


systemd targets

- Prethodne verzije RHEL koje su koristile Upstart ili SysV init, implementirale su predefinisani skup tzv. runlevel-a.
- Ovi runlevel-i, numerisani od 0-6, definišu selekciju sistemskih servisa koji se pokreću u slučaju odabira odgovarajućeg runlevel-a.
- Implementacija systemd-a zamenjuje koncept runlevel-a konceptom systemd target-a.
- Systemd target je reprezentovan jedinicom tipa target sa datotekama koje imaju ekstenziju .target.
- Smisao target-a je grupisanje drugih systemd jedinica u lanac zavisnosti.
- Na primer, target graphical.target koristi se za startovanje grafičke sesije.
- Ovaj target startuje skupinu sistemskih servisa kao što su GNOME Display Manager (gdm.service), Accounts Service (accounts-daemon.service) i mnoge druge.
- RHEL 7 se distribuira sa izvesnim brojem predefnisanih target-a sličnih skupu runlevel-a, prethodnih verzija operativnog sistema.


Tabele poređenja SysV runlevel-a i  systemd target-a

+---------+--------------------------------------+--------------------------------------------+
|RunLevel | Target Units                         | Description                                |
+---------+--------------------------------------+--------------------------------------------+
|0        | runlevel0.target, poweroff.target    | Shut down and power off the system         |
+---------+--------------------------------------+--------------------------------------------+
|1        |runlevel1.target, rescue.target       | Set up a rescue shell                      |
+---------+--------------------------------------+--------------------------------------------+
|2        |runlevel2.target, multi-user.target   | Set up a non-graphical multi-user system.  |
+---------+--------------------------------------+--------------------------------------------+
|3        |runlevel3.target, multi-user.target   | Set up a non-graphical multi-user system.  |
+---------+--------------------------------------+--------------------------------------------+
|4        |runlevel4.target, multi-user.target   | Set up a non-graphical multi-user system.  |
+---------+--------------------------------------+--------------------------------------------+
|5        |runlevel5.target, graphical.target    | Set up a graphical multi-user system.      |
+---------+--------------------------------------+--------------------------------------------+
|6        |runlevel6.target, reboot.target       | Shut down and reboot the system.           |
+---------+--------------------------------------+--------------------------------------------+


+--------------------+-----------------------------------+--------------------------------------+
|Old Command         | New Command                       | Description                          |
+--------------------+-----------------------------------+--------------------------------------+
|runlevel            |systemctl list-units --type target | List currently loaded target units   |
+--------------------+-----------------------------------+--------------------------------------+
|telinit runlevel    |systemctl isolate name.target      | Changes the current target           |
+--------------------+-----------------------------------+--------------------------------------+

Promena zaboravljene lozinke naloga root, na sistemu koji koristi systemd
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Restartovati računar komandom ``shutdown -r now``
2. Pritiskom na taster ``e`` za željenu konfiguraciju ulazi se u prompt za editovanje konfiguracije
3. Ovde tražimo prvu liniju sa stringom **linux16** i u njoj tražimo uzastopne stringove **rhgb quiet** koje treba obrisati. Linija se možda ne vidi pa treba pritiskati DOWN taster dok se ne pojavi.
4. Na kraju reda napisati **init=/bin/sh**
5. Pritisnuti **CTRL+x**
6. Startuje se Bourne shell (sh shell)
7. Pokrenuti komandu ``mount`` Poslednja linija izlaza komande mount sadrži string type xfs (ro…) što znaci da je sistem datoteka - read only. To mora da se promeni.
8. ``mount -o remount,rw /``
9. Proveriti promenu komandom ``mount``
10. Komandom ``passwd`` promeniti lozinku. Zbog toga što je SELinux – Security Enhanced Linux je uključen, ima još koraka.
11. ``touch /.autorelabel``  kreira datoteku u root-u sistema datoteka koja SELinux-u naglašava da je došlo do promene,  inače sistem ne bi mogao da se startuje.
12. ``exec /sbin/init`` Ako je file system veliki akcija može da potraje.



Podešavanje mrežnih parametara
------------------------------


Nazivi mrežnih adaptera
^^^^^^^^^^^^^^^^^^^^^^^

- Tradicionalno: eth0, eth1…
- RHEL 7 imenovanje je drugačije.
- Naziv počinje sa dva karaktera:
    - Ethernet interfejs, en,
    - WLAN interfejs, wl,
    - WWAN interfejs, ww.
- Sledeći karakter reprezentuje tip adaptera:
    - Onboard, o,
    - Hotplug Slot, s,
    - PCO adapter, p.
- Broj koji reprezentuje indeks, id ili broj porta adaptera.
- Primeri: eno0, enp2s0

.. note:: Hyper-V virtuelne mašine pod CENTOS-om 7 koriste standardni sistem imenovanja adaptera eth0…


Pregled i validacija mrežne konfiguracije
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Pregled IP adrese

Zastarele ali rade na svim Linux-ima:

``ifconfig``

``route``

ip

    ``ip a``, ``ip addr``, ``ip address``

        sinonimi

    ``ip a show eth0``

    ``ip route``

    ``ip -s link`` - statistika svih adaptera

``ping <opcije> <ip adresa ili internet ime>``

``traceroute <opcije> <ip adresa ili internet ime>``

``tracepath <opcije> <ip adresa ili internet ime>``

``ping6``

``traceroute6``

``tracepath6``

Pregled stanja mrežne komunikacije
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``netstat``

``ss``

``ss -ta`` - Prikazuje sve TCP socket-e.

``ss -ua`` - Prikazuje sve UDP socket-e.

``ss -p`` - Prikaz socket-a i naziva procesa koji ih  koriste.

``ss -l`` - Prikaz socket-a u stanju osluškivanja.


Definisanje mrežne konfiguracije
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Network manager

- Network Manager je demon koji prati i upravlja mrežnom konfiguracijom.
- Pojam device reprezentuje mrežni interfejs.
- Pojam connection reprezentuje jednu skupinu konfiguracionih svojstava mrežnog uređaja.
- Mogući su višestruki connection objekti za jedan mrežni uređaj ali samo jedan može biti aktivan u jednom trenutku.
- Na ovaj način je moguće imati više različitih mrežnih konfiguracija za jedan uređaj.
- Tekstualne komande i grafički  alati koji se koriste u kombinaciji sa Network Manager-om snimaju konfiguracione datoteke u direktorijum /etc/sysconfig/network-scripts.

Pregled mrežne konfiguracije pomoću programa nmcli

``nmcli con show`` - Lista sve mrežne konekcije.

``nmcli con show --active`` - Lista sve aktivne mrežne konekcije.

``nmcli con show „eth0“`` - Prikazuje sve detalje konekcije eth0.

``nmcli dev status`` - Prikazuje status svih uređaja.

Kreiranje i aktivacija mrežne konekcije pomoću nmcli

``nmcli con add con-name „default“ type ethernet ifname eth0`` - Kreira mrežnu konekciju pod imenom default za adapter eth0. Podrazumevana IP konfiguracija je dinamička.

``nmcli con add con-name „static“ type ethernet ifname eth0 autoconnect no ip4 172.25.8.10/24 gw4 172.25.8.1`` - Kreira mrežnu konekciju pod imenom static za adapter eth0 sa statičkom IPv4 konfiguracijom.

``nmcli con up static`` - startuje konekciju static

``nmcli con up eth0`` - startuje konekciju eth0

Modifikovanje mrežne konekcije pomoću nmcli

``nmcli con mod „static“ ipv4.dns 172.25.8.1`` - Promena adrese DNS servera.

``nmcli con mod „static“ +ipv4.dns 8.8.8.8`` - Dodavanje adrese DNS servera.

``nmcli con mod „static“ ipv4.addresses 172.25.8.11/24`` - Promena IP adrese.

``nmcli con mod „static“ ipv4.gateway 172.25.8.2`` - Promena adrese podrazumevanog rutera.

``nmcli con mod „static“ +ipv4.addresses 172.25.8.12/24`` - Dodavanje sekundarne IP adrese.

Ostale akcije sa nmcli

``nmcli dev status`` - Pregled statusa mrežnih uređaja.

``nmcli con reload`` - Učitava sve konfiguracione datoteke.

``nmcli con delete static`` - Briše konekciju static.

Druge metode promene IP konfiguracije
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``nmtui``

Meni sistem.

Veoma ugodan za rad.


Promene mrežnih konfiguracionih datoteka
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Konfiguraciona datoteka mrežnog adaptera

- Direktorijum: **/etc/sysconfig/network-scripts**
- Datoteka: **ifcfg-<naziv mrežne konekcije>**, npr. **ifcfg-eth0**
- BOOTPROTO
    - none
    - bootp
    - dhcp
- IPADDR0 = prva IP adresa.
- IPADDR1= druga IP adresa.
- PREFIX = dužina subnet maske, npr. 24.
- GATEWAY0 = adresa rutera.
- DEFROUTE = yes
- DNS1 = prva IP adresa DNS servera.
- DEVICE = oznaka mrežnog adaptera.
- ETHTOOL_OPTS = ”autoneg off”
    - Ethernet opcije
- NM_CONTROLED = yes|no
- ONBOOT = yes|no
    - no ne podiže konekciju kod podizanja os-a.
    - ako se u toku instalacije osa zaboravi uključiti mrežni adapter, to ovde valja naknadno podesiti.
- PEERDNS = yes|no
    - Whether the /etc/resolv.conf file used for DNS resolution contains information obtained from the DHCP server.
- Ako se datoteka menja manuelno treba obaviti sledeće, da bi promena bila efektivna:

    ``nmcli con reload``

    ``nmcli con down <naziv adaptera>``

    ``nmcli con up <naziv adaptera>``

Konfigurisanje imena hosta i razrešavanja imena
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Linux Hostname

- Tri imena:
    - STATIC,
        - Ekvivalentno hostname-u.
        - Podrazumevano ime je localhost.localdomain.
    - TRANSIENT
        - Kernel kopija STATIC name-a.
        - Prilikom inicijalizacije kernela, TRANSIENT ima istu vrednost kao STATIC.
        - Mrežni servisi kao što je DHCP, mogu dinamički promeniti TRANSIENT ime.
    - PRETTY
        - Opis računara, slobodna forma.
- Dobra praksa:
    - DNS FQDN i hostname treba da budu isti, npr. centos01.endava.rs.


Upravljanje imenima

- Podrazumevano sva tri oblika su ista.
- Pregled STATIC imena:

    ``hostname``

    Konfiguraciona datoteka /etc/hostname

    Prethodne verzije RHEL-a: /etc/sysconfig/network
- Pregled svih imena

    ``hostnamectl status``

- Podešavanje sva tri imena odjednom:

    ``hostnamectl set-hostname centos01.endava.rs``

- Podešavanje oblika imena posebno:

``hostnamectl set-hostname --static centos01.endava.rs``

``hostnamectl set-hostname --transient centos011.endava.rs``

``hostnamectl set-hostname --pretty mojracunar``

Statičko razrešavanje imena

- Editovanjem datoteke **/etc/hosts**

- Testiranje:
    ``getent hosts <ime>``

    ``ping <ime>``

- Podrazumevani redosled razrešavanja stub resolver-a (DNS klijent):

    1. hosts datoteka
    2. DNS Server

- Upravljanje radom stub resolver-a:
    - /etc/resolv.conf
    - resolv.conf(5)



Instalacija i ažuriranje softverskih paketa
-------------------------------------------

Instalacija rpm paketa
^^^^^^^^^^^^^^^^^^^^^^

- Softver se distribuira u formi instalacionih paketa
- Na Linux-ima uopšteno, dominiraju dve forme paketa: deb i rpm paketi.
- Karakteristike paketa:
    - Kolekcija datoteka koje se instaliraju na računar,
    - Često se distribuiraju kao tar arhive komprimovane pomoću gzip ili bzip2 programa,
    - Sadrže podatke kojima se koriste sistemi za upravljanje paketima tzv. metapodatke.
    - Sadrže skripte.

- Nakon što se operativni sistem instalira, dodatni softverski paketi se podrazumevano instaliraju korišćenjem mrežnih repozitorijuma paketa.
- Komanda ``rpm``, može da se koristi za instalaciju, može da detektuje zavisnost od drugih paketa ali ne vrši razrešavanje zavisnosti automatski.
- Softveri za upravljanje paketima (Package Manager-i) pojednostavljuju proces lociranja i instalacije paketa, uključujući automatsko razrešavanje zavisnosti.
    - Yellow Dog Updater, Modified – yum.
    - PackageKit.
- yum konfiguraciona datoteka: **/etc/yum.conf**

- ``yum help`` – prikazuje uputstvo za korišćenje.
- ``yum update`` – ažuriranje.
- ``yum update``, ažuriranje svih paketa bez potvrde upita.
- ``yum update -y``, ažuriranje svih paketa sa potvrdom upita.
- ``yum update parted``, ažurira i instalira isključivo paket parted.
    - Nema prepreke da se ažuriranje radi bilo kada.
    - Ispravke kernela će biti efektivne tek nakon restarta.
    - Logika testiranje je slična kao na Windows-u, u smislu testiranja. Realno, ispravka se ne sme instalirati bez prethodnog testiranja. Primeri problema sa ažuriranjem PHP-a, Apache-a itd.
- ``yum install <package>`` – instalira paket.
   - ``yum install gparted``, instalira gparted paket.
- ``yum remove <package>`` – deinstalira paket i sve zavisne pakete
   - ``yum remove gparted``, deinstalira gparted paket.

Lociranje paketa

- ``yum list`` - lista pakete
    - ``yum list``
    - ``yum list *lsb*`` , prikazuje samo pakete koji imaju reč lsb u nazivu.
    - ``yum list installed``, lista sve instalirane pakete.

- ``yum search <ključna reč>`` – lociranje paketa.
    - ``yum search lsb``, traži izraz lsb u poljima name i summary rpm paketa.
    - ``yum search all lsb``, traži izraz lsb u svim metapodacima rpm paketa
    - 32 bitne pakete prepoznajemo kao 686, 586…
    - 64 bitne pakete prepoznajemo kao x86_64.
    - Postoje paketi i za druge procesore.
- ``yum provides <feature>``
    - Omogućava traženje paketa koji obezbeđuje neko svojstvo ili datoteku.
    - ``yum provides lsb_release``, lociranje paketa koji sadrži program lsb_release.
    - ``yum provides sshd``, lociranje paketa koji sadrži Open SSH demon.
- ``yum info <paket>``  – prikazuje podatke o paketu.
    - ``yum info httpd``

Predefinisane grupe paketa

- ``yum group list``  – ispisuje listu dostupnih grupa paketa.
    - ``yum grouplist``
    - Grupe su podeljene na: environment groups i groups
- ``yum group info`` – detalji o dostupnim grupama.
    - ``yum group info „CIFS file server“``, nazivi sa razmacima moraju da se stave pod navodnike.
- ``yum group install`` – instalira grupu lociranu preko yum group list.
    - ``yum group install “File and Print Server”``
- Ponašanje komande yum u radu sa grupama paketa na verziji RHEL7 je nešto promenjeno u odnosu na prethodne verzije.
    - Kod ažuriranja grupe, pod pretpostavkom da je grupi u međuvremenu dodat novi obavezni ili podrazumevani paket, dodatni paket će takođe biti instaliran.

Prethodne operacije

- Sve yum operacije se snimaju u log datoteku **/var/log/yum.log**
    - ``cat /var/log/yum.log``
- Rezime svih operacija.
    - ``yum history``
- Opoziv operacije.
    ``yum history undo 6``

Repozitorijumi paketa

- Listanje svih dostupnih repozitorijuma.
    - ``yum repolist all``
- Listanje korišćenih repozitorijuma.
    - ``yum repolist``
- Osposobljavanje repozitorijuma za korišćenje:
    - ``yum-config-manager --enable C7.0.1406-extras``
- Onesposobljavanje repozitorijuma za korišćenje:
    - ``yum-config-manager --disable C7.0.1406-extras``

Ostali repozitorijumi (Third Party)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Izvor repozitorijuma nije Red Hat korporacija.
- Dodatni repozitorijumi se definišu pomoću konfiguracionih datoteka smeštenih u **/etc/yum.repos.d**
- Datoteke imaju ekstenziju **.repo**.
- Mogu se manuelno kreirati.
- Takođe se mogu kreirati pomoću alata **yum-config-manager**.
    - ``yum-config-manager --add-repo= "http://dl .fedoraproject.org/pub/epel/beta/7/x86_64/“``
- Uklanjanje brisanjem repo datoteke.
- Postojeći korišćeni repozitorij može da sadrži rpm paket koji omogućava instalaciju repozitorijuma:
    - ``yum install epel-release`` (Extra Packages for Enterprise Linux )

repo datoteka

- Može da sadrži više alternativnih izvora iste kolekcije paketa, tzv. mirrorlist.
- Da bi se repozitorij pretraživao potrebna je linija:
    - **enabled=1**
- Ako su paketi repozitorijuma digitalno potpisani potrebno je prethodno instalirati RPM GPG ključ, koji takođe obezbeđuje repozitorij.
- Argument **--nogpgcheck alata yum**, omogućava ignorisanje nedostatka RPM GPG ključa.


Arhiviranje podataka
--------------------

tar
^^^

- Program tar, omogućava arhiviranje i kompresiju datoteka.
- Istorijski gledano, datoteke bi bile smeštane u jednu datoteku koja je posle snimana na traku, otuda naziv Tape Archiver - tar.
- Koristi se za potrebe kreiranja rezervne kopije podataka kao i za potrebe transfera datoteka preko mreže.
- Arhiva može biti komprimovana korišćenjem gzip , bzip2 ili xz kompresionog algoritma.
- Program je takođe sposoban da lista sadržaj arhive kao i da vrši ekstrakciju datoteke.
- Podrazumevano‚ tar arhivira podatke o vlasništvu i osnovnim ovlašćenjima.
- Podrazumevano, tar ne arhivira SELinux kontekst i ACL-ove.

Opcije tar programa

- **c**
    - Kreira arhivu.
- **t**
    - Lista sadržaj arhive.
- **x**
    - Obavlja ekstrakciju arhive.
- **f <ime datoteke>**
    - Datoteka nad kojom se operacija izvodi.
- **v**
    - Nivo detaljnost ispisa.
- **--acl**
    - Zadržava dodatne ACL-ove kod kreiranja arhive i nakon ekstrakcije. Za osnovni skup ovlašćenja (vlasnik, vlasnik grupa, ostali) nije potrebno koristiti ovu opciju.

Upravljanje arhivama

- Primeri arhivacije:
    - ``tar cf archive.tar file1 file2 file3``
- Arhivira datoteke file1, file2, file3
    - ``tar cf /root/etc.tar /etc``
    - Arhivira folder /etc
- Primer listanja sadržaja arhive:
    - ``tar tf /root/etc.rar``
- Primeri ekstrakcije arhive:
    - ``tar xf /root/etc.tar``
- Preporučuje se ekstrakcija u prazan direktorij da bi se izbegli problemi sa postojećim datotekama.
- Ako datoteke ekstraktuje root nalog, zadržava se originalno vlasništvo nad datotekama.
- Ako regularan korisnik vrši ekstrakciju, on je vlasnik ekstraktovanih datoteka.

Kreiranje tar arhive

- Karakteristike algoritama kompresije.
    - **gzip** −najbrži i najstariji.
    - **bzip2** −bolji nivo kompresije ali znatno sporiji.
    - **xz** −najbolji nivo kompresije.
- Opcije:
    - **z** −gzip algoritam.
    - **j** −bzip2 algoritam.
    - **J** −xz algoritam.
- Primeri:
    - ``tar czf /root/etcbackup.tar.gz /etc`` −Arhivacija korišćenjem gzip algoritma
    - ``tar cjf /root/etcbackup.tar.bz2 /etc`` −Arhivacija korišćenjem bzip2 algoritma
    - ``tar cJf /root/etcbackup.tar.xz /etc`` −Arhivacija korišćenjem xz algoritma

Ekstrakcija arhive

- Primeri:
    - ``tar xzf /root/etcbackup.tar.gz`` −Ekstrakcija arhive komprimovane gzip algoritmom
    - ``tar xjf /root/etcbackup.tar.bz2`` −Ekstrakcija arhive komprimovane bzip2 algoritmom
    - ``tar xJf /root/etcbackup.tar.xz`` −Ekstrakcija arhive komprimovane xz algoritmom

Dodatni primeri:

- Kreiranje arhive koja zadržava i dodatne ACL-ove, korišćenjem gzip algoritma:
    - ``tar --acl -czf /root/etcbackup.tar.gz /etc``
- Ekstrakcija arhive sa zadržavanjem dodatnih ACL-ova, korišćenjem gzip algoritma u lokalni folder:
    - ``tar --acl -xzf /root/etcbackup.tar.gz``









