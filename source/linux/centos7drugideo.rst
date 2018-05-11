LINUX - CentOS7 II
==================

Praćenje i upravljanje procesima
--------------------------------

Karakteristike Linux procesa

- Proces je instanca programa u izvršavanju.
- Proces se sastoji od:
    - Adresnog prostora alocirane memorije,
    - Jedne ili više niti izvršavanja (nit = thread),
    - Stanja procesa,
    - Bezbednosnih svojstava,
- Okruženje procesa uključuje:
    - Lokalne i globalne varijable,
    - Trenutni tzv. Scheduling Context,
    - Alocirane sistemske resurse kao što su deskriptori datoteka i mrežni portovi.
- Postojeći, roditeljski proces duplicira sopstveni adresni prostor (fork operacija) da bi kreirao strukturu novog podprocesa (child process).
- Svaki proces ima svoj jedinstveni identifikacioni broj, tzv. Process ID - PID.
- PID se koristi radi praćenja procesa a takođe i iz bezbednosnih razloga.
- PID procesa i PID njegovog roditeljskog procesa, tzv. Parent PID - PPID su deo okruženja procesa.
- Svaki proces može kreirati podproces.
- Svi procesi su direktni ili indirektni potomci prvog procesa koji se naziva systemd (1) na RHEL-u 7.
- Pomoću fork rutine, podproces nasleđuje bezbednosne identitete, deskriptore datoteka, portove, privilegije nad resursima, varijable okruženja i programski kod, roditeljskog procesa.
- Nakon toga, podproces pokreće sopstveni programski kod.
- Roditeljski proces tada tipično ide u stanje spavanja.
- Kada se podproces završava, odbacuje sve korišćene resurse i okruženje, šalje roditelju exit signal i prelazi u zombi stanje.

Dokumentacija stanja Linux procesa

- **Running**
    - **R** - TASK_RUNNING. Proces se ili izvršava ili čeka na izvršavanje.
- **Sleeping**
    - **S** - TASK_INTERRUPTIBLE. Proces čeka da se neki uslov zadovolji: hardverski zahtev, pristup na sistemski resurs ili signal. Kada se uslov zadovolji status procesa se menja u Running.
    - **D** - TASK_UNINTERRUPTIBLE. Ne šalje odgovor na bilo kakav signal. Koristi se u situacijama kada bi prekid procesa uzrokovao nepredviđene situacije u vezi sa hardverom.
    - **K** - TASK_KILLABLE. Slično TASK_UNINTERRUPTIBLE stanju, samo što dopušta primanje signala za gašenje procesa. Programi ga često prikazuju kao D  stanje.
- **Stopped**
    - **T** - TASK_STOPPED. Proces je zaustavljen obično na signal korisnika ili drugog procesa. Proces može biti nastavljen.
    - **T** - TASK_TRACED. Proces se debaguje.
- **Zombie**
    - **Z** - EXIT_ZOMBIE. Podproces signalizira roditelju na prekida rada. Svi resursi osim PID-a su otpušteni.
    - **X** - EXIT_DEAD. Kada roditeljski proces očisti sve strukture podataka podprocesa, tada je podproces u potpunosti završen. Ovo stanje se, kod programa koji listaju procese, nikada ne vidi.

ps
^^

- Ispisuje podatke o aktivnim procesima.
- Vrste opcija:
    - UNIX98
        - ispred opcije je srednja crtica -
        - mogu se grupisati
    - Primer: ``ps -p <PID>``
    - BSD
        - ne koriste srednju crticu -
        - mogu se grupisati
        - Primer: ``ps p <PID>``
    - GNU long
        - ispred opcije su dve srednje crtice --
        - Primer: ``ps --pid <PID>``

ps, primeri

- Zanimljive BSD opcije.
- ``ps ax``
        - Svi procesi.
- ``ps axu``
        - Svi procesi, tzv. "user oriented" format
- ``ps axf``
        - Stablo svih procesa.
- Zanimljive kombinovane opcije:
- ``ps -U root -u root u``
    - Samo procesi naloga root, "user oriented" format.

- Zanimljive UNIX98 opcije.
- ``ps -e``
        - svi procesi
- ``ps -ef``
        - svi procesi, tzv. "full" format
- ``ps -eF``
        - svi procesi, tzv. "extra full" format
- ``ps -el``
        - svi procesi, tzv. "long" format
- ``ps -eH``
        - Stablo svih procesa

ps, dokumentacija prikazanih kolona

- **F** – Flags.
- **PID** – Process ID.
- **PPID** – Parent Process ID.
- **C** – % Processor utilization.
- **STIME** – Start time.
- **TTY** – Terminal associated with the process.
- **TIME** – Cumulative CPU time.
- **CMD** – Process name.
- **SZ** – Size in physical page.
- **RSS** – Resident Set Size (KB).
- **PSR** – Processor allocated to process.
- **S** – Process status code .
- **PRI** – Process priority.
- **NI** – Nice value (priority).
    - -20 najviši.
    - 19 ili 20 najniži.
    - podrazumevana je vrednost roditeljskog procesa.
    - najčešće 0 za korisnički program.
- **WCHAN** – Memory address of the event. the process is waiting for.
- **ADDR** – Memory address of the process.


Kontrola procesa
^^^^^^^^^^^^^^^^

Procesi u prvom planu i sesije

- **Job Control** je shell svojstvo koje omogućava jednoj instanci shell-a da pokreće više komandi istovremeno, komande se mogu selektivno suspendovati, nastaviti i pokretati asinhrono.
- Bez ovoga, nakon forkovanja podprocesa, roditeljski proces nastavlja da spava.
- Proces u prvom planu (Foreground process) je komanda pokrenuta u terminalskom prozoru.
- Terminal koji kontroliše proces je opisan identifikacionim brojem terminala (Terminal Device ID - tty).
- Procesi u prvom planu primaju ulaz sa tastature, signale, dozvoljeno im je čitanje i pisanje na terminal.
- Sesija procesa (množina) nastaje startovanjem terminala ili konzole. Svi podprocesi nastali iz ovog terminala ili konzole dele isti Session ID.
- Unutar sesije samo jedan proces može biti u prvom planu.

Procesi u pozadini

- Procesi u pozadini (Background processes ili Background Jobs) se startuju bez kontrolnog terminala, jer im interakcija sa terminalom nije neophodna.
- Lista procesa koju prikazuje program ps, za procese u pozadini, u koloni TTY prikazuje ?.
- Startovanje procesa u pozadini se može obaviti dodavanjem karaktera & na kraju komandne linije.
    - Primer: ``gedit &``
- Bash shell potom prikazuje Job Number, jedinstven za sesiju i PID novog procesa.
- Ako ima više povezanih komandi, da bi sve istovremeno radile u pozadini, potrebno je staviti ih u zagradu pa onda dodati & nakon zatvorene zagrade.
    - Primer: ``(ls -laR ~ | sort &> file) &``
    - Opis primera: Rezultat rekurzivnog listanja korisničkog home direktorijuma se sortira a regularan završetak komande sort ili greška se ispisuje u datoteku file. Obe vezane komande (ls i sort) rade u pozadini.

Kontrolisanje pozadinskih procesa

- ``jobs``
    - Daje listu svih pozadinskih procesa sa Job ID -jem u zagradi.
- ``fg %1``
    - Prebacivanje procesa sa Job ID-jem 1 u prvi plan. (Povezivanje sa terminalom)
- ``bg %1``
    - Prebacivanje procesa sa Job ID-jem 1 u pozadinu.
- ``ps j``
    - Prikazuje listu svih procesa u pozadini na sistemu.

Ubijanje procesa
^^^^^^^^^^^^^^^^

Kontrolisanje rada procesa

- Kontrolisanje rada procesa vrši se pomoću prekidnih signala (interrupt, interrupt signal).
- U narednoj tabeli je data dokumentacija prekidnih signala koje administratori mogu koristiti za upravljanje procesima.
    - Brojevi signala mogu da variraju na različitim Linux hardverskim platformama. U narednoj tabeli, dati su brojevi za x86 sisteme.

Prekidni signali

+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|Broj signala |Kratki naziv |Naziv         |Smisao                                                                         |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|1            | HUP         |Hangup        |Koristi se za raportiranje u vezi prekida kontrolnog procesa. Može da se       |
|             |             |              |koristi i za slanje zahteva za reinicijalizaciju procesa bez gašenja (ponovno  |
|             |             |              |učitavanje konfiguracije).                                                     |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|2            |INT          |Keyboard      |Prekida program. Pokreće se sa ctrl+c (INTR karakter).                         |
|             |             |Interrupt     |                                                                               |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|3            |QUIT         |Keyboard quit |Slično SIGINT ali proizvodi Dump procesa. Pokreće se sa ctrl+\                 |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|9            |KILL         |Kill,         |Prouzrokuje momentalni prekid rada programa. Uvek fatalan.                     |
|             |             |unblockable   |                                                                               |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|15 default   |TERM         |Terminate     |Prekida rad programa. Za razliku od SIGKILL, može da se blokira, ignoriše ili  |
|             |             |              |upravlja. Uljudan način gašenja programa. Dopušta programu Self-Cleanup.       |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|18           |CONT         |Continue      |Nastavlja rad stopiranog procesa. Ne može se blokirati.                        |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|19           |STOP         |Stop,         |Stopira proces. Ne može se blokirati niti upravljati.                          |
|             |             |unblockable   |                                                                               |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+
|20           |TSTP         |Keyboard stop | Može se blokirati ignorisati ili upravljati procesom. Pokreće se sa ctrl+z    |
+-------------+-------------+--------------+-------------------------------------------------------------------------------+

Komande za slanje signala

- Kombinacije tastera **ctrl+c, ctrl+\, ctrl+z** rade samo sa procesima u prvom planu.
- Slanje signala programu koji radi u pozadini zahteva korišćenje adekvatne komande.
- Slede komande za slanje signala za upravljanje procesima.
- **kill -s <signal> <pid> ili kill %<jobid>**
    - Ubija proces prema kriterijumu ID-ja procesa
    - <signal> najčešće vrednosti:
        - 1 SIGHUP
        - 9 SIGKILL
        - 15 SIGTERM (Default)
    - Primeri:
        - ``kill 4760``
        - ``kill -s 9 4760``
        - ``kill %2``

killall
^^^^^^^

- Ubija procese određenog naziva.
- Sintaksa:
    - ``killall -s <signal> <naziv>``
    - ``killall -s <signal> -i <naziv>``
        - Gašenje sa potvrdom.
    - ``killaall -u <user> <naziv>``
        - Gašenje svih procesa određenog naziva koje je pokrenuo korisnik <user>.
- Primeri:
    - ``killall gedit``
        - Gasi sve instance gedit-a (SIGTERM).
    - ``killall -s 9 gedit``
        - Gasi sve instance gedit-a (SIGKILL).
    - ``killall -i gedit``
        - Gasi sve instance gedit-a (SIGTERM) sa potvrdom.

Administrativno odjavljivanje korisnika
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Komanda ``w`` lista sve trenutno aktivne terminale i virtuelne konzole.
- Svi pseudoterminali, tj. terminali u grafičkom okruženju vide se kao **pts/<broj>**
- Sve virtuelne konzole se vide kao **tty<broj>**.
- Kolona FROM pokazuje naziv udaljenog sistema sa koga se veza uspostavlja.
- Kolona JCPU pokazuje koliko CPU vremena su konzumirali svi procesi povezani sa kontrolnim procesom.
- Kolona PCPU pokazuje konzumaciju CPU od strane procesa čije ime je vidljivo u WHAT polju.

Gašenje svih procesa jednog korisnika

- Uobičajeno je koristiti program ``pkill``.
- Program ``pgrep`` pomaže identifikaciji procesa koje pokreće neki korisnik.
- Pregled svih procesa jednog korisnika:
    - ``pgrep -l -u  marija``
- Gašenje svih procesa korisnika:
    - ``pkill -SIGKILL -u marija``
- Gašenje terminala/virtuelne konzole i svih podprocesa (ali ne nužno i svih procesa nekog korisnika):
    - ``pkill -SIGKILL -t tty2``
- Pregled svih terminala/virtuelnih konzola
    - ``w``

Praćenje aktivnosti procesa
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Load Average

- **Load Average** parametar, pokazuje opterećenje procesora. Izračunava se korišćenjem parametara **Active Requests** i **Load Number**.
- **Active Requests** ili broj aktivnih zahteva je broj niti raznih procesa u redu za čekanje procesora (per-CPU queues) i broj niti koji čekaju na obavljanje ulazno izlaznih operacija.
- **Load number** nastaje kao zbir svih aktivnih zahteva za sve procesore.
- **Load Average** nastaje usrednjavanjem (**Moving average**) uzastopnih vrednosti parametra **Load number**. Izračunava se na svako pet sekundi.
- Programi **uptime**, **top**, **w**, **gnome-system-monitor** prikazuju **Load average**.

Analiza Load Average vrednosti

- Pretpostavimo da komanda **uptime** prikazuje sledeći rezultat:
    - 15:11:13 up 2:11, 3 users, load average: 2.95, 4.40,  5.20
- Vrednosti 2.95, 4.40,  5.20, prikazuju Load Average za poslednjih 1, 5 i 15 minuta respektivno, za sve procesore odjednom.
- Sve tri vrednosti treba podeliti sa ukupnim brojem procesora da bi se dobila vrednost po jednom procesoru.
- Jedan procesor je ili jedno jezgro ili HT logički  procesor.
- Podatke o svim procesorima sadrži pseudo datoteka /proc/cpuinfo.
    - ``grep “model name” /proc/cpuinfo``
- Pretpostavimo da system ima 4 procesora, tada je Load average po procesoru.
    - load average: 0,73 1,1  1,3

Interpretacija Load average vrednosti

- Load average po procesoru: 0,73 1,1  1,3
    - Iskorišćenje svih procesora je u poslednjoj minuti ~73%
    - Za poslednjih 5 minuta, sistem je bio preopterećen za 11%.
    - Za poslednjih 15 minuta, sistem je bio preopterećen za 30%.
    - Opterećenje sistema opada.
- Sve dok ne dođe do iscrpljenja resursa, Load Average će biti ispod 1.
- Load Average se uvećava kada zahtevi čekaju u redu.
- Kada se iskorišćenje resursa približava 100% svaki dodatni zahtev doživljava dodatno čekanje.

Praćenje procesa u realnom vremenu pomoću komande top
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+-------+--------------------------------------------------------------------------------------------------------------+
|Taster | Namena                                                                                                       |
+-------+--------------------------------------------------------------------------------------------------------------+
|h      | Pomoć.                                                                                                       |
+-------+--------------------------------------------------------------------------------------------------------------+
|I,t,m  | Uključuje/isključuje opcije load, threads, memory iz zaglavlja prikaza.                                      |
+-------+--------------------------------------------------------------------------------------------------------------+
|l      | Uključuje/isključuje prikaz individualnih CPU ili svih CPU u zaglavlju.                                      |
+-------+--------------------------------------------------------------------------------------------------------------+
|s      | Menja period osvežavanja (podrazumevano 3s)                                                                  |
+-------+--------------------------------------------------------------------------------------------------------------+
|b      | Uključuje obrnuto naznačen prikaz procesa u running stanju. Podrazumevano se prikazuju masnim slovima.       |
+-------+--------------------------------------------------------------------------------------------------------------+
|u il U |Prikazuje procese nekog korisnika.                                                                            |
+-------+--------------------------------------------------------------------------------------------------------------+
|M      |Sortira procese po zauzeću memorije, od onih sa najvećim zauzećem do onih sa najmanjim.                       |
+-------+--------------------------------------------------------------------------------------------------------------+
|P      |Sortira procese po CPU zauzeću, od onih sa najvećim zauzećem do onih sa najmanjim.                            |
+-------+--------------------------------------------------------------------------------------------------------------+
|k      | Ubija proces.                                                                                                |
+-------+--------------------------------------------------------------------------------------------------------------+
|r      | Menja Nice vrednost.                                                                                         |
+-------+--------------------------------------------------------------------------------------------------------------+
|q      | Izlazi iz programa.                                                                                          |
+-------+--------------------------------------------------------------------------------------------------------------+
|W      |Snima aktuelnu konfiguraciju prikaza do sledećeg restarta.                                                    |
+-------+--------------------------------------------------------------------------------------------------------------+


Dnevnici događaja i NTP
-----------------------

Sistemski dnevnici događaja
^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Jezgro operativnog sistema (kernel) i procesi koji u njegovom okruženju rade, moraju negde snimati informacije o tekućim događajima.
- Za ove potrebe se koriste datoteke sistemskih dnevnika događaja (System Log Files).
- Prema konvenciji, datoteke sistemskih dnevnika događaja smeštene su u direktorijum **/var/log**.
- Za potrebe snimanja događaja, RHEL koristi Syslog protokol.
- Snimanjem poruka u datoteke dnevnika događaja upravljaju dva servisa, **systemd-journald** i **rsyslogd**.
- Demon **systemd-journald** prima poruke od kernela i iz rane faze procesa podizanja operativnog sistema, odnosno od strane demona koji se pokreću prilikom podizanja operativnog sistema, itd.
- Potom, **systemd-journald** snima ove poruke u neperzistentnu, struktuiranu, indeksiranu i binarnu datoteku, tzv. žurnal datoteku koja se ne zadržava prilikom restartovanja sistema.
- Nakon toga, servis **rsyslog** sortira poruke po tipu i prioritetu i upisuje ih u perzistentne datoteke dnevnika u direktorijumu **/var/log**.

Pregled datoteka sistemskih dnevnika događaja

+----------------------+-----------------------------------------------------------------------------------------------+
|Datoteka dnevnika     | Namena                                                                                        |
+----------------------+-----------------------------------------------------------------------------------------------+
|/var/log/messages     |Najveći broj poruka se snima ovde, sve osim onih koje se snimaju u druge datoteke dnevnika.    |
+----------------------+-----------------------------------------------------------------------------------------------+
|/var/log/secure       | Bezbednosne i autentifikacione poruke.                                                        |
+----------------------+-----------------------------------------------------------------------------------------------+
|/var/log/maillog      | Poruke u vezi rada servera elektronske pošte.                                                 |
+----------------------+-----------------------------------------------------------------------------------------------+
|/var/log/cron         | Poruke u vezi zadataka koji se periodično pokreću.                                            |
+----------------------+-----------------------------------------------------------------------------------------------+
|/var/log/boot.log     | Poruke u vezi procesa podizanja operativnog sistema.                                          |
+----------------------+-----------------------------------------------------------------------------------------------+


Tipovi poruka
^^^^^^^^^^^^^

- Svaka poruka, snimljena u datoteku dnevnika, svrstava se prema tipu i prioritetu poruke.
- Prioriteti poruka su prikazani u sledećoj tabeli.

+-----+-----------+--------------------------------------------------------+
|Kod  | Prioritet | Ozbiljnost poruke                                      |
+-----+-----------+--------------------------------------------------------+
|0    |emerg      | Sistem je nestabilan.                                  |
+-----+-----------+--------------------------------------------------------+
|1    |alret      | Potrebna trenutna akcija.                              |
+-----+-----------+--------------------------------------------------------+
|2    |crit       | Kriticno stanje.                                       |
+-----+-----------+--------------------------------------------------------+
|3    | err       | Stanje nekritične greške.                              |
+-----+-----------+--------------------------------------------------------+
|4    |warning    | Upozorenje na događaj.                                 |
+-----+-----------+--------------------------------------------------------+
|5    |notice     | Uobičajen ali značajan događaj.                        |
+-----+-----------+--------------------------------------------------------+
|6    |info       | Informacioni događaj.                                  |
+-----+-----------+--------------------------------------------------------+
|7    |debug      | Poruka nivoa ispravljanja grešaka (Debugging Level     |
|     |           | message).                                              |
+-----+-----------+--------------------------------------------------------+


Konfigurisanje rsyslogd servisa
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Za konfigurisanje rsyslogd servisa koristi se konfiguraciona datoteka **/etc/rsyslog.conf** odnosno ***.conf datoteke**  direktorijuma **/etc/rsyslog.d**.
- Sekcija #### RULES #### datoteke **/etc/rsyslog.conf** sadrži direktive koje određuju u koje se datoteke poruke snimaju. Takođe, mogu određivati tip i prioritet poruke.
- Na primer, linija koja počinje sa authpriv.* određuje da se sve autentifikacione i bezbednosne poruke snimaju u datoteku **/var/log/secure** .
- Treba napomenuti da servisi kao što su Apache Web Server ili Samba imaju sopstvene datoteke dnevnika u odgovarajućem poddirektorijumu **/var/log** direktorijuma.

Rotacija datoteka dnevnika događaja
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Rotacija datoteka dnevnika je operacija koja se izvodi pomoću programa **logrotate**, konfigurisanog da se startuje automatski pomoću servisa cron kao tzv. planirani zadatak (Scheduled Job) s ciljem kasnijeg brisanja zastarelih datoteka dnevnika.
- Rotacija je proces koji uključuje preimenovanje datoteke dnevnika i kreiranje nove aktuelne datoteke dnevnika.
- Na primer, datoteka **/var/log/messages**, dana 30.10.2016, biva rotirana. To znači da se originalna datoteka preimenuje u **/var/log/messages-20161030** a istovremeno nastaje nova aktuelna datoteka **/var/log/messages**.
- Nakon izvesnog broja rotacija, tipično nakon četiri sedmice, zastarele datoteke dnevnika se brišu da bi se oslobodio disk prostor.
- Naveći broj datoteka dnevnika se rotira sedmično ali neki se rotiraju brže ili sporije zavisno od konfiguracije programa **logrotate**.

Analiza zapisanog događaja u datoteci dnevnika događaja

- Poruke se zapisuju po redosledu od najstarije ka najnovijoj, od vrha ka dnu datoteke.
- Pogledajmo primer zapisa događaja:

.. code-block:: bash

 Apr 17 13:15:12 localhost systemd-logind: New session 1 of user root.

- Format zapisa je sledeći:
    1. Datum i vreme kada se događaj desio,
    2. Računar koji je poslao poruku,
    3. Program ili proces koji je poslao poruku,
    4. Poruka.

Praćenje događaja u realnom vremenu i generisanje poruka
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Pomoću komande **tail -f <putanja do datoteke>** moguće je pratiti aktivnost pojave događaja u realnom vremenu, na primer:
    - ``tail -f /var/log/messages``
- Tačnije, komanda ispisuje poslednjih deset linija odabrane datoteke i nastavlja sa ispisom novih linija, kako se pojavljuju.
- Pomoću komande **logger**, moguće je slanje poruka rsyslog servisu, koje se onda zapisuju u odgovarajuću datoteku dnevnika.
- Primer slanja poruke u datoteku messages:
    - ``logger  "moja poruka"``
- Primer slanja poruke prioriteta notice u datoteku boot.log:
    - ``logger  -p local7.notice "poruka tipa notice"``

Pregled systemd žurnala
^^^^^^^^^^^^^^^^^^^^^^^

Pretraga događaja pomoću komande journalctl

- Servis **systemd-journald** snima informacije o događajima u struktuiranu, indeksiranu, binarnu datoteku tzv. žurnal datoteku.
- Ovde se snimaju dodatni podaci o događaju kao što su na primer tip i prioritet događaja.
- Žurnal je smešten u direktorijum **/run/log/journal**.
- Pregled žurnala, može se obaviti pomoću komande **journalctl**.
    - Poruke se prikazuju od najstarije ka najnovijoj, kada je pokreće korisnik root.
    - Komanda označava masnim slovima poruke prioriteta **notice** i **warning**.
    - Poruke prioriteta **error** i viših prioriteta, označene su crvenom bojom.

- ``journalctl -n``
    - Prikazuje deset poslednjih događaja.
- ``journalctl -n 5``
    - Prikazuje pet poslednjih događaja.
- ``journalctl -p err``
    - Prikazuje samo error događaje.
- ``journalctl -f``
    - Prikazuje deset poslednjih događaja i kao i svaki događaj koji pristiže u realnom vremenu.
- ``journalctl --since today``
    - Prikazuje samo događaje koji su se danas desili.
- ``journalctl --since "2014-02-10 20:30:00" --until now``
    - Prikazuje događaje koji su se desili u određenom vremenskom intervalu.
- ``journalctl --since "2014-02-10 20:30:00" --until "2014-02-12 12:00:00"``
    - Prikazuje događaje koji su se desili u određenom vremenskom intervalu.
- ``journalctl -o verbose``
    - Prikazuje sve događaje i sve detalje svih događaja.
- ``journalctl _SYSTEMD_UNIT=sshd.service``
    - Prikazuje sve događaje u vezi sa systemd jedinicom sshd.service.
- ``journalctl PRIORITY=6``
    - Prikazuje sve događaje prioriteta info

Čuvanje žurnala
^^^^^^^^^^^^^^^

Permanentno čuvanje žurnala

- Već smo videli da je žurnal smešten u direktorijum **/run/log/journal**, što znači da dolazi do njegovog brisanja svaki put kada dolazi do restartovanja sistema.
- Permanentno čuvanje žurnala obezbeđuje se serijom akcija:
    1. Kreirati direktorijum /var/log/journal,
       - ``mkdir -p /var/log/journal``
    2. Podesiti da je vlasnik direktorijuma nalog root,
    3. Podesiti da je vlasnik grupa direktorijuma systemd-journal,
       - ``chown root:systemd-journal /var/log/journal``
    4. Definisati nad direktorijumom ovlašćenja 2755
       - ``chmod 2755 /var/log/journal``
    5. Restartovati sistem ili poslati USR1 signal systemd-journald procesu
       - ``killall -USR1 systemd-journald``
- Nakon svega, moguće je prikazivati sadržaj žurnala od poslednjeg restarta komandom:
    - ``journalctl -b``

Održavanje tačnog vremena na računarima
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Standardni metod za obezbeđivanje tačnog vremena je Network Time Protocol - NTP.
- Korišćenjem NTP klijenta, računar može sinhronizovati svoje vreme sa nekim javnim NTP serverom na internetu ili lokalnim NTP serverom.
- Sinhronizacija sistemskog  vremena je vrlo važna zbog sledećih razloga:
    - zbog analize sadržaja dnevnika na višestrukim sistemima, posebno u bezbednosnom smislu,
    - smanjenje konfuzije oko vremena modifikacije kod deljenih sistema datoteka,
    - kod servisa za naplatu koji moraju da rade vremenski precizno,
    - kod finansijskih servisa koji zakonski zahtevaju visoko precizno održavanje tačnog vremena.

Elementi procesa sinhronizacije vremena

- Cezijumski, rubidijumski, hidrogenski ili stroncijumski časovnik, tzv atomski časovnik.
- Zemaljska stanica, povezana sa atomskim časovnikom, koja emituje radio signal tačnog vremena:
    - DCF-77, nemački
    - WWV, američki
    - RWM, ruski
- Satelit sa atomskim časovnikom čije se tačno vreme dobavlja preko adekvatnog protokola kao što je:
    - GPS
    - GLONASS
- Uređaj u lokalnoj mreži ili na internetu koji ima sposobnost sinhronizacije vremena preko bilo koje od pomenutih tehnologija i koji nudi drugim računarima sinhronizaciju vremena preko NTP protokola.
- Ostali računari koji koriste NTP da bi pribavili tačno vreme.


Pregled lokalnog vremena i vremenska zona

- Komanda ``timedatectl`` obezbeđuje pregled najvažnijih informacija u vezi sistemskog vremena kao što su trenutno vreme, vremenska zona, podešavanja sinhronizacije preko NTP-a.


Upravljanje vremenskim zonama

- Komanda ``timedatectl list-timezones`` obezbeđuje listu svih raspoloživih vremenskih zona koju održava organizacija IANA.
- Komanda ``tzselect`` omogućava identifikaciju korektne vremenske zone tako što obezbeđuje seriju pitanja korisniku. Komanda ne menja sistemsku konfiguraciju vremenske zone.
- Primeri podešavanja vremenske zone:
    - ``timedatectl set-timezone America/Phoenix``
    - ``timedatectl set-timezone Europe/Belgrade``

Upravljanje vremenom

- Primer podešavanja klijenta tako da se oslanja na NTP:
    - ``timedatectl set-ntp true``
- Isključenje automatske sinhronizacije putem NTP-a:
    - ``timedatectl set-ntp false``
- Primer manuelnog podešavanja vremena:
    - ``timedatectl set-time 22:00:00``
- Manuelno podešavanje vremena moguće je samo ako je isključena automatska sinhronizacija.

chronyd
^^^^^^^

- Servis **chronyd** je podrazumevani NTP klijent/server.
- Kao NTP klijent, obezbeđuje sinhronizaciju vremena sa prethodno konfigurisanim NTP serverima.
- Podrazumevano, chronyd koristi NTP servere klastera NTP Pool Project.
- NTP Pool Project ima 3833 servera širom sveta, od toga 14 u Srbiji, u trenutku pisanja prezentacije.
- Radi bolje preciznosti, ukoliko ne postoji veza sa NTP serverima, servis koristi prethodno izračunato vreme kašnjenja RTC časovnika snimljenog u tzv. **driftfile**, koji je definisan konfiguracionom datotekom **chronyd**  servisa na putanji **/etc/chrony.conf**.


Konfiguracija chronyd servisa

- Pogledajmo najvažniji deo konfiguracione datoteke **/etc/chrony.conf** .

- Linija koja počinje stringom server, pokazuje internet ime ili IP adresu NTP servera koji se koristi za sinhronizaciju vremena.
- Opcija **iburst** je preporučena jer omogućava da **chronyd** servis, nakon startovanja, izvrši četiri merenja u kratkom vremenskom intervalu radi što preciznije inicijalne sinhronizacije.
- Promena servera, na koji se sistem oslanja u sinhronizaciji, podrazumeva definisanje posebne server linije i uklanjanje postojećih server linija.
- Takođe potrebno je restartovati **chronyd** servis: ``systemctl restart chronyd``

- Komanda ``chronyc`` se ponaša kao klijent **chronyd** servisa.
- Detaljni podaci o izvorima sinhronizacije vremena mogu se saznati pomoću komande ``chronyc sources -v``.


Pristup na udaljene sisteme pomoću OpenSSH servisa
--------------------------------------------------

Uopšteno o OpenSSH
^^^^^^^^^^^^^^^^^^

- Izraz OpenSSH se odnosi na implementaciju tzv. Secure Shell softvera.
- Koristi se za bezbedno pokretanje shell programa na udaljenom računaru.
- Kao preduslov, potrebno je da korisnik ima korisnički nalog na udaljenom sistemu.
- Komanda kojom se pokreće povezivanje na udaljeni sistem je ssh.
- Sintaksa i primer povezivanja korišćenjem naloga čije je korisničko ime isto kao na lokalnom sistemu:
    - ``ssh <ime ili IP adresa udaljenog računara>``
    - ``ssh alpha``
- Sintaksa i primer povezivanja korišćenjem specifičnog naloga:
    - ``ssh <korisničko ime>@<ime ili IP adresa udaljenog računara>``
    - ``ssh student@alpha``

Pregled prijavljenih korisnika

- Komanda ``w``  prikazuje listu svih prijavljenih korisnika.
- Uz eventualnu upotrebu argumenta **-f** moguće je videti polje **from** na osnovu čega je moguće donositi zaključke o tome da li se korisnik prijavljuje sa udaljenog sistema.
- U našem primeru, komanda ``w`` ispisuje polje **from** i bez argumenta **-f**.

.. code-block:: bash

 [root@localhost ~]# w
 14:57:47 up  1:43,  1 user,  load average: 0,00, 0,01, 0,05
 USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
 root     pts/0    10.0.2.13        14:57    2.00s  0.01s  0.01s w

SSH host key
^^^^^^^^^^^^

- Kada se ssh klijent povezuje na ssh server, pre prijavljivanja korisnika, server klijentu šalje kopiju svog JAVNOG ključa.
- Ovo omogućava uspostavljanje kriptovanog komunikacionog kanala a takođe omogućava autentifikaciju servera.
- Kada korisnik prvi put koristi ssh za povezivanje na neki server, ssh komanda snima javni ključ servera u tekstualnu datoteku **~/.ssh/known_hosts**.
- Ako dođe do naknadne promene javnog ključa servera, komunikacija neće moći da se uspostavi sve dok klijent ne ažurira svoju kopiju javnog ključa servera.
- Najjednostavniji način rešavanja ovog problema je brisanje ključa iz **~-.ssh/known_hosts** datoteke, nakon čega će, kod prvog narednog uspostavljanja konekcije, server poslati klijentu novu kopiju svog javnog ključa.
- Za smeštaj javnih ključeva ssh servera može se koristiti i sistemska datoteka **/etc/ssh/ssh_known_hosts**. Datoteka podrazumevano ne postoji.

Autentifikacija bazirana na paru PKI ključeva
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Kod ssh pristupa, korisnici se mogu autentificirati i bez lozinke, korišćenjem sistema autentifikacije baziranih na paru PKI ključeva.
- Kod ovog sistema dolazi do kreiranja para ključeva za korisnika: javnog i privatnog.
- Privatni i javni ključ se generišu na računaru sa koga se udaljeno pristupa.
- Privatni ključ mora biti obezbeđen.
- Javni ključ se kopira na računare na koje se vrši udaljeni pristup.
- Za generisanje para ključeva podrazumevano se koristi RSA algoritam.
- Ključevi su matematički povezani ali je zbog teške faktorizacije ogromnih prostih brojeva, kod RSA algoritma, teško saznati privatni ključ na bazi javnog.
- Generisanje para ključeva se radi **ssh-keygen** komandom.
- Lokacija privatnog ključa je **~/.ssh/id_rsa**.
- Lokacija javnog ključa je **~/.ssh/id_rsa.pub**.
- Preporučena ovlašćenja su: **600 za privatni ključ i 644 za javni ključ**.
- Pre korišćenja javni ključ mora biti prekopiran na udaljeni računar.
- Ovo se može uraditi komandom  **ssh-copy-id**.
    - ``ssh-copy-id <ime ili IP adresa udaljenog računara>``
    - ``ssh-copy-id <korisničko ime>@<ime ili IP adresa udaljenog računara>``

ssh passphrase
^^^^^^^^^^^^^^

- U procesu generisanja para ključeva, postoji opcija da se definiše lozinka (passphrase) kod pristupa na javni ključ.
- Preporučuje se njeno korišćenje iz bezbednosnih razloga.
- Naime, čak i u slučaju gubitka privatnog ključa, istome se jako teško može pristupiti kada je podešena lozinka.
- S druge strane, proces prijavljivanja na udaljeni računar se komplikuje potrebom da se lozinka unosi prilikom svakog prijavljivanja.
- Da bi se izbeglo stalno unošenje lozinke potrebno je pokrenuti sledeće komande:

.. code-block:: bash

 ssh-agent bash
 ssh-add
 unos lozinke

Promena nije permanentna, vredi samo za aktuelnu sesiju.


Ograničavanje pristupa preko ssh servisa
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Datoteka sshd_config

- Konfiguraciona datoteka **/etc/ssh/sshd_config**, sadrži parametre koji omogućavaju blokiranje korišćenja ssh sesije.
- Moguće je blokirati spoljni pristup na nalog root izmenom parametra **PermitRootLogin**:
    - Originalna vrednost parametra: **#PermitRootLogin yes**
    - Treba promeniti u:	**PermitRootLogin no**
- Moguće je zabraniti korisnicima pristup korišćenjem lozinki i time ograničiti pristup na metod para PKI ključeva, promenom parametra **PasswordAuthetication**.
    - Originalna vrednost parametra: **PasswordAuthentication yes**
    - Treba promeniti u:	 **PasswordAuthentication no**
- Da bi promena bila efektivna, treba restartovati ssh demon:
    - ``systemctl restart sshd``

Konfigurisanje planiranih zadataka - CronJob
--------------------------------------------

Konfigurisanje jednokratnih planiranih zadataka
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Planirani zadatak (Scheduled Task)

- Ponekad je potrebno pokrenuti komandu ili skup vezanih komandi u određenom trenutku u budućnosti.
- Najčešći praktični primeri uključuju kreiranje rezervnih kopija datoteka, obavljanje planiranog restarta servera i slično.
- Ovako definisane planirane komande ili serije komandi nazivaju se planirani zadaci (Scheduled Tasks) ili planirani poslovi (Scheduled Jobs).

Konfigurisanje jednokratnih planiranih zadataka pomoću demona atd

- Jedan od načina da se na RHEL Linux-u konfiguriše planirani zadatak je pomoću sistemskog demona ``atd``.
- Interakcija sa demonom obavlja se korišćenjem skupa komandi kao što su ``at`` i ``atq``.
- Podrazumevana instalacija RHEL-a uključuje instalaciju demona ``atd``.
- Ukoliko demon nije instaliran može se doinstalirati kao deo paketa ``at``.
- Definisanje planiranih zadataka obavlja se pomoću komande ``at``.
- Planirani zadaci se konfigurišu kao deo reda (queue) planiranih zadataka koji čekaju na izvršavanje.
- Demon ``atd`` obezbeđuje do 26 redova za izvršavanje planiranih zadataka **a-z** sa opadajućim prioritetom od reda **a** ka redu **z**.

Definisanje planiranih zadataka
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Za izvršavanje skripti može se sledeći metod:
    - Sintaksa: ``at <TIMESPEC> < <SCRIPT>``
    - Primer: ``at now +5min < backupscript``
- Za pokretanje pojedinačnih komandi može se koristiti sintaksa oblika:
    - Sintaksa: ``<COMMAND> | at <TIMESPEC>``
    - Primer: ``echo "Hello" > echo.log | at now +1min``

- Podrazumevani red za čekanje je **a** red. Promena podrazumevanog reda za čekanje obavlja se pomoću opcije ``-q <QUEUENUMBER>``.

Opcija TIMESPEC

- Opcija **TIMESPEC** obezbeđuje veliki broj različitih načina za definisanje vremena u koje će se planirani zadatak izvršiti.

- Pogledajmo nekoliko zanimljivih primera:
    - now +5min		- za pet minuta
    - teatime tomorrow		- sutra u 16h
    - noon +4 days		- u podne za 4 dana
    - 5pm august 3 2016		- trećeg avgusta 2016. u 17h
    - 18:00 +3days		- u 18h za tri dana
- Primer:
    - ``echo hello > hello.log | at 18:00 march 24 2016``

Pregled i upravljanje zadacima

- Pregled planiranih zadataka koji čekaju na izvršavanje, može se obaviti pomoću komande **atq** ili pomoću komande **at -l**.

Prikaz komande planiranog zadatka

- Pregled komande, koja se izvršava kao deo planiranog zadatka, može se izvesti pomoću komande ``at -c <TASKNUMBER>``, na primer ``at -c 8``.
- Komanda ``at`` prvo ispisuje okruženje zadatka (job environment) nakon čega sledi komanda ili skripta koja je deo planiranog zadatka.

Brisanje planiranog zadatka

- Brisanje planiranog zadatka se obavlja pomoću komande ``atrm <JOBNUMBER>``, na primer ``atrm 8``.


Konfigurisanje ponavljajućih korisničkih planiranih zadataka
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Deamon cron

- Linux sistemi tipično se isporučuju sa instaliranim demonom cron (crond) koji omogućava definisanje ponavljajućih planiranih zadataka.
- Radom **cron-a**, upravlja se pomoću niza konfiguracionih datoteka.
- Postoji po jedna konfiguraciona datoteka specifična za svakog korisnika posebno. Može se modifikovati pomoću komande **crontab**.
- Takođe, postoji više sistemskih konfiguracionih datoteka.
- **Cron** je deo paketa pod nazivom **cronie**.
- Prilikom korišćenja demona **cron** razlikujemo korisničke planirane zadatke i sistemske planirane zadatke.
    - Korisnički planirani zadaci se izvršavaju u bezbednosnom kontekstu korisnika koji ih definiše.
    - Sistemski planirani zadaci se izvršavaju u proizvoljnom bezbednosnom kontekstu.

- Korisnici bez administrativnih privilegija mogu koristiti komandu **crontab** za upravljanje njihovim planiranim zadacima.
- Komanda se može pokrenuti na četiri različita načina, prema tabeli.

+---------------------+----------------------------------------------------------------------+
|Komanda              | Namena                                                               |
+---------------------+----------------------------------------------------------------------+
|crontab -l           |Lista sve planirane zadatke aktuelnog korisnika.                      |
+---------------------+----------------------------------------------------------------------+
|crontab -r           |Uklanja sve planirane zadatke aktuelnog korisnika.                    |
+---------------------+----------------------------------------------------------------------+
|crontab -e           | Kreira ili modifikuje planirane zadatke aktuelnog korisnika.         |
+---------------------+----------------------------------------------------------------------+
|crontab <naziv dat>  |Uklanja sve planirane zadatke i zamenjuje ih zadacima pročitanim iz   |
|                     |datoteke. Ako datoteka nije specificirana koristi se stdin.           |
+---------------------+----------------------------------------------------------------------+

- Komanda ``crontab -e``, efektivno modifikuje konfiguracionu datoteku **/var/spool/cron/<username>**.
- Datoteka ne sme direktno da se menja.
- Nalog **root** može koristiti opciju **-u <username>** za upravljanje zadacima drugih korisnika.
- Komanda **crontab** se ne koristi za upravljanje sistemskim planiranim zadacima.

Opšta pravila modifikacije konfiguracione datoteke

- Prilikom pokretanja komande **crontab -e**, startuje se podrazumevano vi editor, osim ako varijabla **EDITOR** nije modifikovana.
- Jedan planirani zadatak zauzima jednu liniju teksta.
- Prazne linije teksta su dozvoljene.
- Komentari započinju karakterom taraba **#**.
- Varijable okruženja se mogu koristiti pomoću formata **NAME=value** i odnosiće se na sve linije ispod linije u kojoj je definisana.
- Uobičajene varijable su **SHELL** i **MAILTO**.
- Varijabla **SHELL** menja shell aplikaciju koja izvršava komande ispod linije u kojoj je definisana.
- Varijabla **MAILTO** menja **e-mail** adresu na koji će biti poslat izlaz. Slanje elektronske pošte zahteva konfigurisanje SMTP servera.

**Format zapisa**

- Svaki planirani zadatak sastoji se od šest polja koji određuju kada i šta će biti pokrenuto.
- Po redosledu, polja reprezentuju sledeće elemente:
    - minut,
    - sat,
    - dan u mesecu,
    - mesec,
    - dan u sedmici,
    - komandu.
- Ukoliko su istovremeno definisana polja **dan u mesecu** i **dan u sedmici** različita od * (sa značenjem svi dani u sedmici ili svi dani u mesecu), komanda će se izvršiti u oba termina.


**Sintaksa zapisa**

- Prvih pet polja ima ista sintaksna pravila:
    - Karakter * znači uvek,
    - Dani u nedelji se reprezentuju brojem, 0=nedelja, 1=ponedeljak, 2=utorak … 6=subota, 7= nedelja,
    - Opseg se definiše kao x-y i uključuje termine x i y,
    - Lista termina se definiše kao x,y,
    - Lista u koloni minut može da uključuje i opsege. Na primer, 5,10-13,17,
    - U koloni minut izraz ``*/x`` indicira interval od x. Na primer, ``*/7`` znači da zadatak započinje na svako 7 minuta.
- U koloni mesec moguće je koristiti engleske skraćenice Jan, Feb…
- U koloni dan u sedmici moguće je koristiti engleske skraćenice Mon, Tue, Wed…
- Komandu će izvršiti /bin/sh osim ako varijabla SHELL nije definisana.
- Ako komada sadrži karakter % isti će biti tretiran kao karakter newline i sve zapisano nakon ovog karaktera će biti prosleđeno komandi stdin-a.

Primeri definicija planiranih zadataka

- Sintaksa:
    - **minut     sat    dan u mesecu    mesec      dan u sedmici**
- Primer 1:
    - ``0 9 2 2 * /usr/local/bin/yearly_backup``
        - izvršava komandu **/usr/local/bin/yearly_backup** u 9h drugog februara svake godine.
- Primer 2:
    - ``*/7 9-16 * Jul 5 echo "Chime"``
        - šalje e-mail poruku koja sadrži reč Chime na svako 7 minuta u periodu 9-16h svakog petka u julu.
- Primer 3:
    - ``58 23 * * 1-5 /usr/local/bin/daily_report``
        - izvršava komandu **/usr/local/bin/daily_report** u 23h 58min, svakog radnog dana.

Konfigurisanje ponavljajućih sistemskih planiranih zadataka
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Sistemski planirani zadaci


- Sistemski planirani zadaci se konfigurišu direktnim modifikovanjem konfiguracionih datoteka.
- Primarna konfiguraciona datoteka je **/etc/crontab**.
- Alternativne konfiguracione datoteke mogu da se smeste u direktorijum **/etc/cron.d**.
- Takođe, postoje predefinisani planirani zadaci koji se izvršavaju svakog sata, dana, sedmice i meseca. Oni izvršavaju sve skripte smeštene u direktorijume **/etc/cron.hourly**, **/etc/cron.daily**, **/etc/cron.weekly**, **/etc/cron.monthly**, respektivno.
    - Treba još jednom napomenuti da se u ovim direktorijumima ne nalaze konfiguracione datoteke već izvršne skripte.
- Glavna razlika između korisničkih planiranih zadataka i sistemskih planiranih zadataka je u tome što konfiguraciona datoteka sistemskog planiranog zadatka ima dodatno polje koje definiše nalog u čijem kontekstu se izvršava planirani skript ili komanda.

Datoteka /etc/crontab

- Podrazumevani sadržaj datoteke je takav da pomaže razumevanju konfiguracionih polja.
- Praktično, sintaksa je ista kao kod korisničkih zadataka, osim dodatnog konfiguracionog polja koje reprezentuje korisnika u čijem kontekstu se zadatak izvršava.

.. code-block:: bash

 [root@localhost ~]# cat /etc/crontab
 SHELL=/bin/bash
 PATH=/sbin:/bin:/usr/sbin:/usr/bin
 MAILTO=root

 # For details see man 4 crontabs

 # Example of job definition:
 # .---------------- minute (0 - 59)
 # |  .------------- hour (0 - 23)
 # |  |  .---------- day of month (1 - 31)
 # |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
 # |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
 # |  |  |  |  |
 # *  *  *  *  * user-name  command to be executed


- Skripte iz direktorijuma **/etc/cron.hourly** izvršavaju se pomoću komande **run-parts**, definisane datotekom **/etc/cron.d/0hourly**.
- Dnevni, sedmični i mesečni planirani zadaci se takođe izvršavaju pomoću **run-parts** komande ali se koristi različita konfiguraciona datoteka **/etc/anacrontab**.
- Sintaksa datoteke **anacrontab** je nešto drugačija u odnosu na ostale konfiguracione datoteke demona **cron**.
- Ima sledeća polja:
    - **Period in days**	- koliko često se zadatak izvršava, računajući u danima.
    - **Delay in minutes**	- koliko se čeka pre izvršenja zadatka.
    - **Job identifier**	- naziv datoteke iz direktorijuma **/var/spool/anacron** koji se koristi da bi se proverilo da li je zadatak pokrenut. Prilikom provere, koristi se atribut **timestamp** pomenute datoteke koji se ažurira kod svakog pokretanja zadatka.
    - **Command**		- komanda koja se pokreće.

Varijable datoteke anacrontab

- Datoteka /etc/anacrontab može da sadrži deklaracije varijabli.

.. code-block:: bash

 [root@localhost ~]# cat /etc/anacrontab
 # /etc/anacrontab: configuration file for anacron

 # See anacron(8) and anacrontab(5) for details.

 SHELL=/bin/sh
 PATH=/sbin:/bin:/usr/sbin:/usr/bin
 MAILTO=root
 # the maximal random delay added to the base delay of the jobs
 RANDOM_DELAY=45
 # the jobs will be started during the following hours only
 START_HOURS_RANGE=3-22

 #period in days   delay in minutes   job-identifier   command
 1	5	cron.daily		nice run-parts /etc/cron.daily
 7	25	cron.weekly		nice run-parts /etc/cron.weekly
 @monthly 45	cron.monthly		nice run-parts /etc/cron.monthly
 [root@localhost ~]#

- Posebno je zanimljiva varijabla **START_HOURS_RANGE**.
- Planirani zadaci se ne mogu izvršavati van opsega časova definisanih ovom varijablom.


Dodavanje diskova, particija i sistema datoteka
-----------------------------------------------

Dodavanje particija, sistema datoteka i perzistentno montiranje
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Strukture podataka koje omogućavaju definisanje particija**

- Master Boot Record.
    - Omogućava podizanje operativnog sistema na računarima koji koriste klasični BIOS.
    - Poseduje tabelu particija.
    - Tabela particija poseduje do 4 polja za opis particija.
    - Poseduje deo sa izvršnim programom koji omogućava podizanje računara u ranoj fazi.
- GUID Partition Table.
    - Omogućava podizanje operativnog sistema na računarima koji koriste UEFI firmware.
    - Poseduje tabelu particija.
    - Kopija na početku i na kraju diska radi redundantnosti.
    - Tabela particija poseduje do 128 polja za opis particija.

**Particionisanje diskova**

- MBR diskovi.
    - Koriste MBR strukturu podataka.
    - Do 4 primarne particije.
    - Dozvoljeno je koristiti jednu extended particiju umesto primarne.
    - Na extended particiji je moguće kreirati logičke particije.
    - Veličina particije do 2 TiB.
- GPT diskovi.
    - Koriste GPT strukturu podataka.
    - Do 128 particija.
    - Veličina particije do 8 ZiB (8 zebibajta - 8 milijardi TiB)

**Rana faza procesa podizanja sistema koji koristi BIOS**

- BIOS POST
    - Boot sequence
        - zavisi od konfiguracije BIOS setup programa
    - Učitavanje boot sector-a uređaja (prvih 512 bajta podataka) –MBR
        - MBR tabela particija (4 max)
        - MBR Boot Code (bootstrap code)
            - Windows
                - tipično u tabeli traži particiju koja je markirana kao aktivna
                - startuje boot sector aktivne particije (prvi sektor)
                - u boot sector-u je program koji dalje učitava OS
            - Linux
                - specifičan boot loader npr. GRUB
                - GRUB delimično staje u 446B boot sector-a diska
                - GRUB dalje učitava OS.

**Rana faza procesa podizanja sistema koji koristi Native UEFI**

- BIOS POST
- Boot sequence
    - zavisi od konfiguracije UEFI firmware programa.
- Učitavanje boot programa na specijalnoj particiji formatiranoj pod FAT fajl sistemom, tzv. EFI sistemska particija:
    - Na x64 sistemu /EFI/BOOT/BOOTX64.EFI
    - Datoteka mora imati izvršni format definisan UEFI specifikacijom.
    - Struktura particija definisana GPT standardom.
- Učitani program nastavlja proces podizanja operativnog sistema.

**Upravljanje MBR particijama, pomoću programa fdisk**

- Pokretanje aplikacije:
    - ``fdisk /dev/sdb``
- Pomoć u vezi opcija:
    - Taster **m**.
- Listanje tabele particija:
    - Taster **p**.
- Snimanje promena i izlaz
    - Taster **w**.
    - Promene tabele particija prvo keširaju a snimaju se tek nakon akcije snimanja promena.
- Izlaz bez snimanja:
    - Taster **q**.
- Listanje svih dostupnih tipova particija:
    - Taster **l**.
- Promena tipa particije:
    - Taster **t**.
- Kreiranje nove particije:
    - Taster **n**.
    - Potom u stilu čarobnjaka treba definisati:
        1. Broj particije,
        2. Prvi sektor nove particije,
        3. Poslednji sektor nove particije ili veličinu particije (npr. +20MB),
        4. Potvrda kreiranja.
    - Taster **w**.
    - Iniciranje čitanja MBR-a od strane kernela:
        - Komanda ``partprobe``
- Brisanje particije:
    - Taster **d**.
    - Uneti broj particije koja se uklanja.
    - Taster **w**.

**Upravljanje GPT particijama pomoću programa gdisk**

- Pokretanje aplikacije:
    - ``fdisk /dev/sdb``
- Pomoć u vezi opcija:
    - Taster **?**.
- Listanje tabele particija:
    - Taster **p**.
- Snimanje promena i izlaz
    - Taster **w**.
    - Promene tabele particija prvo keširaju a snimaju se tek nakon akcije snimanja promena.
- Izlaz bez snimanja:
    - Taster **q**.
- Listanje svih dostupnih tipova particija:
    - Taster **l**.
- Promena tipa particije:
    - Taster **t**.
- Kreiranje nove particije:
    - Taster **n**.
    - Potom u stilu čarobnjaka treba definisati:
        1. Broj particije,
        2. Prvi sektor nove particije,
        3. Poslednji sektor nove particije ili veličinu particije (npr. +20MiB ili +20MB),
        4. Tip particije.
    - Taster **w**.
    - Iniciranje čitanja GPT-a od strane kernela:
        - Komanda **partprobe**
- Brisanje particije:
    - Taster **d**.
    - Uneti broj particije koja se uklanja.
    - Taster **w**.

Poređenje sistema datoteka
^^^^^^^^^^^^^^^^^^^^^^^^^^

+----------------------------+-------------------------------+----------------------------+----------------------------+
|                            | ext4                          |xfs                         |btrfs                       |
+----------------------------+-------------------------------+----------------------------+----------------------------+
|Maks dužina imena datoteke  | 255                           | 255                        | 255                        |
+----------------------------+-------------------------------+----------------------------+----------------------------+
|Maks veličina datoteke      | 16TB                          | 8EB                        | 8TB                        |
+----------------------------+-------------------------------+----------------------------+----------------------------+
|Maks veličina volumena      | 1EB                           | 16EB                       | 16TB                       |
+----------------------------+-------------------------------+----------------------------+----------------------------+
|Najpozitivnija strana       | Kompatibilnost sa ext3,2..    | Za veoma velike volumene   | Samopopravljanje, sve ili  |
|                            |                               |                            | ništa.                     |
+----------------------------+-------------------------------+----------------------------+----------------------------+
|Status                      | OK                            | OK                         | Eksperimentalni            |
+----------------------------+-------------------------------+----------------------------+----------------------------+
|Anaconda default            | Ne                            | Da                         | Ne                         |
+----------------------------+-------------------------------+----------------------------+----------------------------+


**Kreiranje sistema datoteka (formatiranje particije)**

- Sintaksa:
    - ``mkfs -t <tip sistema datoteka> <putanja do datoteke particije>``
- Primeri:
    - ``mkfs -t xfs /dev/sdb1``
    - ``mkfs -t ext4 /dev/sdb2``
- mkfs je wrapper komanda, tj. poziva odgovarajuću komandu u zavisnosti od fajl sistema.
    - Npr. mke2fs kreira ext2, ext3 ili ext4..

**Montiranje sistema datoteka**

- Manuelno montiranje:
    - `mount /dev/sdb1 /mnt``
    - Nakon restarta operativnog sistema, neće doći do ponovnog montiranja.
- Perzistentno montiranje
    - Datoteka **/etc/fstab**.
    - U svakoj liniji je šest polja koja definišu parametre za montiranje.
    - Nakon restarta operativnog sistema, automatski dolazi do ponovnog montiranja.
- Lista montiranih sistema datoteka:
    - ``mount``
- Lista sistema datoteka tipa xfs:
    - ``mount -t xfs``
- Montiranje sistema datoteka definisanih u fsabtab datoteci bez restarta:
    - ``mount -a``

**Demontiranje sistema datoteka**

- Manuelno demontiranje, komandom umount:
    - ``umount /dev/sdc1 /test`` demontira particiju **sdc1** sa **/test** foldera.
    - Ako linija u fstab datoteci postoji dovoljno je **umount /test**.
- Perzistentno demontiranje:
    - Uklanjanjem adekvatne linije iz fstab datoteke.

**fstab**

.. code-block:: bash

 [root@localhost ~]# cat /etc/fstab

 #
 # /etc/fstab
 # Created by anaconda on Wed Feb 28 16:47:34 2018
 #
 # Accessible filesystems, by reference, are maintained under '/dev/disk'
 # See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
 #
 /dev/mapper/centos-root /                       xfs     defaults        0 0
 UUID=816d9585-b091-4970-9309-82fd355cda86 /boot                   xfs     defaults        0 0
 /dev/mapper/centos-swap swap                    swap    defaults        0 0
 [root@localhost ~]#

**Detaljan opis polja fstab datoteke**

- Prvo polje: UUID uređaja ili putanja do datoteke uređaja.
    - UUID se može saznati pomoću komande ``blkid``, npr. ``blkid /dev/sdb1``.
- Drugo polje: Tačka montiranja.
    - Npr. **/mnt**.
- Treće polje: Tip sistema datoteka.
    - Npr. ext4, xfs…
- Četvrto polje: Opcije
    - Skup podrazumevanih opcija se definiše stringom **defaults**.
- Peto polje: Dump Flag
    - Indikator komande **dump** koji određuje da li će doći do kreiranja rezervne kopije uređaja.
- Šesto polje: fsck order.
    - Redni broj particije čiji integritet treba proveriti, pomoću programa **fsck**, u toku podizanja operativnog sistema, nakon nekorektnog demontiranja particije. Broj 0 znači da neće doći do provere.

- Primer: ``/dev/sdb1  /mnt  xfs  defaults 0 3``

Upravljanje Swap prostorom
^^^^^^^^^^^^^^^^^^^^^^^^^^
- Prostor diska koji koristi podsistem Linux kernela za upravljanje memorijom.
- Koristi se kao dodatak radnoj memoriji za potrebe smeštaja neaktivnih stranica.
- Zbir sadržaja operativne memorije (RAM) i swap prostora se naziva virtuelna memorija.
- Kada potrošnja operativne memorije pređe definisani limit, kernel češlja operativnu memoriju u potrazi za neaktivnim (idle) stranicama.
- Zatim se neaktivna stranica upisuje u swap prostor a onda se njena adresa dodeljuje drugom procesu.
- Po potrebi, stranica se prebacuje nazad u operativnu memoriju.

**Kreiranje i korišćenje swap prostora**

- Sastoji se iz četiri koraka:
    - Kreiranja particije,
    - Podešavanja tipa particije na vrednost 82 Linux Swap za MBR disk ili na vrednost 8200 za GPT disk.
    - Kreiranja swap potpisa particije: ``mkswap /dev/sdb3``
    - Aktivacije swap prostora:
        - Neperzistentna: ``swapon /dev/sdb3``
        - Perzistentna: Particija mora biti konfigurisana u fstab datoteci.
            - Primer: UUID=fbd7fa60 -b781-44a8-961b-37ac3ef572bf  swap  swap  defaults  0  0
            - Komanda ``swapon -a`` aktivira sve swap prostore definisane u datoteci fstab.
- Deaktivacija swap prostora:
    - Komanda ``swapoff /dev/sdb3``
- Swap prostori imaju prioritete koji su konfigurabilni opcijom pri=<proioritet>
- Pregled aktuelnih prioriteta komandom ``swapon -s``.

- ``du``
    - Iskorišćenje disk prostora po direktorijumu.
    - ``du /home``.
    - ``du -ch /home``.
    - h –koristi lako čitljive jedinice veličine.
    - c –dodaje total na kraj izlaza.

- ``df``
    - iskorišćenje disk prostora po sistemu datoteka.
    - ``df -h``.
    - ``df -h /dev/sdb1``.
    - h –koristi lako čitljive jedinice veličine.

- ``fsck``
    - Provera i popravka sistema datoteka.
    - ``fsck /dev/sdc2``.
    - Zahteva demontiranje diska nad kojim se provera vrši.
    - Ne radi podrazumevano rešavanje grešaka.
    - -p automatically repair.
    - -y potvrda.
    - -v verbose.

Apstrakcija disk prostora korišćenjem LVM-a
-------------------------------------------

Koncepti LVM-a (Logical Volume Manager)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- LVM se može klasifikovati kao tehnologija apstrakcije disk prostora.
- Implementira se na nivou drajvera operativnog sistema.
- Omogućava grupisanje više fizičkih diskova u jedno skladište.
- Volumeni mogu oponašati karakteristike RAID sistema.
- Obezbeđuje fleksibilnije upravljanje disk prostorom u odnosu na klasični sistem particionisanja diska.

- **Fizički uređaji (Physical devices)** su blok uređaji koji predstavljaju osnovu za smeštaj podataka. To mogu biti:
    - Diskovi,
    - Particije na disku,
    - RAID sistemi,
    - SAN diskovi.
- **Fizički volumen (Physical Volume - PV)** se koristi za registrovanje fizičkog uređaja kao dela grupe. Fizički volumen se segmentira na elemente koji se zovu fizičke mere (physical extent).
- **Grupa volumena (Volume Group - VG)** je kolekcija fizičkih volumena. Fizički volumen se može dodeliti samo jednoj grupi volumena.
- **Logički volumen (Logical Volume - LV)** se kreira na prostoru grupe volumena i predstavlja LVM ekvivalent particijama. Tehnički, LV je kolekcija logičkih mera koje odgovaraju fizičkim merama, jedan na jedan.



Upravljanje logičkim volumenima
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Kreiranje logičkih volumena**

- Kreiranje je logičkih volumena je proces koji se sastoji iz četiri koraka:
    1. Priprema fizičkog uređaja,
    2. Kreiranje fizičkog volumena na pripremljenim uređajima,
    3. Kreiranje grupe volumena na fizičkom volumenu,
    4. Kreiranje logičkih volumena na grupi volumena.


**Korak 1. priprema fizičkog uređaja**

- Odabir particije kao fizičkog uređaja zahteva prethodno kreiranje iste.
- Za particionisanje se mogu koristiti programi fdisk, gdisk, parted i drugi.
- Konfigurisati tip particije kao Linux LVM 8e na MBR diskovima ili Linux LVM 8e00 na GPT diskovima.
- Alternativno, mogu se koristiti čitavi diskovi, RAID nizovi ili SAN diskovi.
- U slučaju da se koriste čitavi diskovi, nikakva priprema, osim eventualno uklanjanje struktura podataka MBR ili GPT, nije neophodna.
- MBR ili GPT se mogu se ukloniti pomoću programa gdisk (opcije x i z uzastopno). Takođe, program **pvcreate**, koji se koristi u narednom koraku, može ponuditi da ukloni DOS MBR.


**Korak 2, kreiranje fizičkog volumena**

- Markirati fizičke uređaje za korišćenje od strane LVM sistema komandom ``pvcreate``.
- Sintaksa:
    - ``pvcreate <fizički disk 1> …<fizički disk n>``

- Primer:
    -  ``pvcreate /dev/sdc /dev/sdd /dev/sde``


**Korak 3, kreiranje grupe volumena**

- Obavlja se pomoću komande ``vgcreate``.
- Sintaksa:
    - ``vgcreate <naziv grupe volumena> <fizički disk 1> …<fizički disk n>``

- Primeri:

    - ``vgcreate vg-alpha /dev/sdc /dev/sdd /dev/sde``

**Korak 4, kreiranje fiksnih linearnih logičkih volumena**

- Obavlja se pomoću komande ``lvcreate``.
- Sintaksa kreiranja volumena:
    - ``lvcreate -n <naziv logičkog volumena> -L <veličina volumena> <naziv grupe volumena>``

- Primer:
    - ``lvcreate -n lv-userdata -L 3G vg-alpha``


**Korak 4, kreiranje tankih linearnih logičkih volumena**

- Tanki (Thin Provisioned) logički volumen može imati proizvoljnu veličinu nezavisnu od realnog raspoloživog disk prostora.
- Raspoloživi disk prostor se popunjava po potrebi dok se ne iscrpi.
- Procedura kreiranja se sastoji iz dva koraka: kreiranje thin pool-a, kreiranje logičkog volumena:
    - ``lvcreate -L 380G -T vg-alpha/tp1 ili lvcreate -l 100%FREE -T vg-alpha/tp1``
        - opcije l ili L definišu veličinu thin pool-a.
    - ``lvcreate -V500G -T vg-alpha/tp1 -n tlv-userdata``
        - V opcija definiše veličinu thin volumena, koja može da prelazi granice dimenzija thin pool-a.


**Korak 4, kreiranje RAID logičkih volumena**

- RAID1 sintaksa (Mirror):
    - ``lvcreate --type raid1 -m <broj kopija> -L <veličina> -n <naziv volumena> <naziv grupe volumena>``
- RAID1 primer:
    - ``lvcreate --type raid1 -m 1 -L 1G -n lvraid1-userdata vg-alpha``
- RAID5 sintaksa:
    - ``lvcreate --type raid5 -i <broj traka> -L <veličina> -n <naziv volumena> <naziv grupe volumena>``
    - ``lvcreate --type raid5 -l <veličina u procentima> -n <naziv volumena> <naziv grupe volumena>``
- RAID5 primeri:
    - ``lvcreate --type raid5 -i 2 -L 1G -n lvraid5-userdata vg-alpha``
    - ``lvcreate --type raid5 -l 100%FREE -n lvraid5-userdata vg-alpha``
- Podržani su takođe sistemi tipa RAID4, RAID6, RAID10

.. note:: Broj traka ne može biti veći od broja diskova - 1. Parametar -i određuje broj traka (2 znači tri trake)

**Korak 5, kreiranje sistema datoteka i montiranje volumena**

- Kreirati sistem datoteka:
    - ``mkfs -t xfs /dev/vg-alpha/lv-userdata``
- Kreirati direktorij pogodan za montiranje:
    - ``mkdir /mnt/lv-userdata``
- Dodati liniju u fstab datoteku:
    - ``/dev/vg-alpha/lv-userdata /mnt/lv-userdata xfs  defaults 1 3``
- Montirati sve sisteme datoteka, definisane u fstab datoteci:
    - ``mount -a``

**Primeri pregled informacija**

- ``pvdisplay``
    - prikazuje podatke o fizičkim volumenima
    - pvdisplay -v  visok nivo detaljnosti podataka
    - pvdisplay -vv viši nivo detaljnosti podataka
    - pvdisplay -vvv najviši nivoa detaljnosti
- ``pvs``
- ``vgdisplay``
    - prikazuje podatke o grupama volumena
    - vgdisplay
    - vgdisplay /dev/vg-alpha
- ``vgs``
    - lvdisplay
    - prikazuje podatke o logičkim volumenima
    - lvdisplay
    - lvdisplay /dev/vg-alpha/lv-userdata
- ``lvs``


**Uklanjanje logičkog volumena, grupe volumena i fizičkih volumena**

1. ``umount /mnt/lv-userdata``
2. ``lvremove /dev/vg-alpha/lv-userdata``
3. ``vgremove vg-alpha``
4. ``pvremove /dev/sdc1 /dev/sdd1 /dev/sde1``

Proširivanje logičkih volumena
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Uvećavanje i smanjivanje grupa volumena**

- Grupa volumena može uključiti dodatni disk prostor, dodavanjem fizičkih volumena.
- Ovaj proces se naziva uvećanje grupe volumena.
- Takođe, neiskorišćeni fizički volumeni se mogu ukloniti iz grupe volumena.
- Ovaj proces se naziva smanjenje grupe volumena.
- Pomoću programa **pvmove** moguće je premestiti podatke sa jednog fizičkog volumena na drugi u istoj grupi volumena.
- Operacija se može obavljati čak i dok su logički volumeni grupe u upotrebi.

**Uvećanje grupe volumena**

- Uvećanje grupe volumena se izvodi u četiri koraka:

 1. Priprema fizičkog diska,
 2. Kreiranje fizičkog volumena,
 3. Proširenje grupe volumena,
 4. Verifikacija dodatnog prostora.

**Korak 1, priprema fizičkog diska**

- Obavlja se na isti način na koji se radi priprema za nov volumen.
- Ako se koriste particije kao fizički uređaji, treba ih kreirati pomoću programa fdisk, gdisk ili parted.
- Ako se koriste fizički diskovi, ne smeju sadržati strukture podataka MBR ili GPT.
- Mogu se takođe koristiti i RAID nizovi kao i SAN diskovi.

**Korak 2, kreiranje fizičkog volumena**

- Markirati sve dodatne fizičke uređaje za korišćenje od strane LVM sistema komandom **pvcreate**.
- Sintaksa:
        - ``pvcreate <fizički disk 1> …<fizički disk n>``

- Primer:
    - ``pvcreate /dev/sdc /dev/sdd /dev/sde``

**Korak 3, proširivanje grupe volumena**

- Obavlja se pomoću programa vgextend.
- Sintaksa:
    - ``vgextend <grupa volumena> <uređaj>``
- Primer:
    - ``vgextend vg-alpha /dev/sdf``

**Korak 4, verifikacija dodatnog prostora**

- Može se obaviti pomoću programa pvdisplay, pvs, vgdisplay ili vgs
- Primeri:
    - ``vgdisplay vg-alpha``
    - ``vgs``

**Smanjenje grupe volumena**

- Može se obaviti samo ako ima dovoljno raspoloživih fizičkih mera (physical extents) za prebacivanje sadržaja uređaja koji se uklanja.
- Odvija se u dva koraka:
    - Prebacivanje fizičkih mera na druge fizičke uređaje, ``pvmove /dev/sdc``.
    - Smanjenje grupe volumena, ``vgreduce vg-alpha /dev/sdc``.

**Uvećavanje logičkog volumena i xfs sistema datoteka**

- Odvija se u tri koraka:
    1. Verifikacija da ima dovoljno fizičkih mera za željeno uvećanje. ``vgdisplay vg-alpha``. Proveriti liniju Free PE / Size.
    2. Proširenje logičkog volumena. ``lvextend -L +10G /dev/vg-alpha/lv1`` (dodaje 10G preostalog slobodnog prstora u lv1) ili ``lvextend -l +50%FREE /dev/vg-alpha/lv1`` (dodaje 50% preostalog slobodnog prstora u lv1)
    3. Proširenje sistema datoteka: xfs_growfs /<tačka montiranja>. ``xfs_growfs /mnt/lv1``.

**Uvećavanje logičkog volumena i ext4 sistema datoteka**

- Odvija se u tri koraka:
    1. Verifikacija da ima dovoljno fizičkih mera za željeno uvećanje. ``vgdisplay vg-alpha``. Proveriti liniju Free PE / Size.
    2. Proširenje logičkog volumena. ``lvextend -L +10G /dev/vg-alpha/lv1`` (dodaje 10G preostalog slobodnog prstora u lv1) ili ``lvextend -l +50%FREE /dev/vg-alpha/lv1`` (dodaje 50% preostalog slobodnog prstora u lv1)
    3. Proširenje sistema datoteka: resize2fs  /<uređaj>. ``resize2fs /dev/vg-alpha/lv1``
