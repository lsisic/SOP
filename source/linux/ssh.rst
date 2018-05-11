SSH
===

Generisanje ssh ključeva
^^^^^^^^^^^^^^^^^^^^^^^^
SSH - Secure Shell je mrežni protokol koji korisnicima omogucava uspostavljnanje sigurnog
komunikacijskog kanala izmedu uredaja kroz mrežu.

Kao alat za SSH cemo koristi OpenSSH

Izraz OpenSSH se odnosi na implementaciju tzv. Secure Shell softvera.
Koristi se za bezbedno pokretanje shell programa na udaljenom racunaru.
Preduslov za to je da korisnik ima korisnicki nalog na udaljenom racunaru.

Za prijavljivanje putem ssh na drugi sistem koristi se:

``ssh <remote_username>@<hostname ili IP adresa udaljenog hosta>``

primer:

``ssh root@192.168.5.16``


Kod ssh pristupa, korisnici se mogu autentifikovati i bez lozinke,
korišcenjem sistema autentifikacije baziranih na paru PKI kljuceva - privatnog i javnog (public) kljuca.
Privatni i javni kljuc se generišu na racunaru sa koga se udaljeno pristupa (naš racunar).
Privatni kljuc mora biti obezbeden ( ne delimo ga ni sa kim) i mora imati password.
Javni kljuc se kopira na racunare na koje se vrši udaljeni pristup i to je kljuc koji se deli.
Generisanje para kljuceva se radi ssh-keygen komandom na linux sistemima. U našem slucaju u Bash for Windowsu.

Postoji nekoliko opcija ssh-keygen komande ali best-practice je upotreba rsa algoritma,
dužine 4096 bita i sa komentarom koji treba da sadrži vaše ime i prezime (radi lakšeg rukovanja sa kljucevima).
Generisanje ovakvog kljuca je moguce uz pomoc sledece komande:

``ssh-keygen -t rsa -b 4096  -C ime.prezime``

Nakon ove komande od vas se ocekuje da unesete PassPhrase (obavezno je zapamtite)!

Lokacija privatnog kljuca je:

``/home/vas_user_name/.ssh/id_rsa``

Lokacija javnog kljuca je:

``/home/vas_user_name/.ssh/id_rsa.pub``

Najlakši nacin da dodete do svog home foldera je uz pomoc komande:

``cd ~``

Unutar home folera se nalazi sakriveni .ssh direktorijum unutar kojeg se nalaze kljucevi.

Preporucena ovlašcenja su: 600 za privatni kljuc i 644 za javni kljuc.

Podešavanje ovlašcenja za javni kljuc:

``chmod 644 /home/vas_username/.ssh/id_rsa.pub``

Podešavanje ovlašcenja za privatni kljuc:

``chmod 600 /home/vas_username/.ssh/id_rsa``


Pre korišcenja javni kljuc mora biti prekopiran na udaljeni racunar.
Ovo se može uraditi komandom  ssh-copy-id.

Sintaksa komande:

``ssh-copy-id <remote_username>@<hostname ili IP adresa udaljenog sistema>``

Primer komande:

``ssh-copy-id root@192.168.5.16``

U procesu generisanja para kljuceva, potrebno je da se definiše lozinka (passphrase) kod pristupa na javni kljuc.
Obavezno je njeno korišcenje iz bezbednosnih razloga.
Naime, cak i u slucaju gubitka privatnog kljuca, istome se jako teško može pristupiti kada je podešena lozinka.
S druge strane, proces prijavljivanja na udaljeni racunar se komplikuje potrebom da se lozinka unosi prilikom svakog prijavljivanja.
Da bi se izbeglo stalno unošenje lozinke potrebno je pokrenuti sledece komande:

.. code-block:: bash

  ssh-agent bash
  ssh-add
  unos lozinke

Promena nije permanentna, vredi samo za aktuelnu sesiju.
