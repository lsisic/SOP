rpm
===

Softwer se distribuira u formi instalacionih paketa. Na Linuksima uopšteno dominiraju dve forme paketa: deb i rpm paketi. 

RPM (Red Hat Package Manager) je paket menadžer koji se može koristiti za build, instalaciju, upite, verifikaciju, ažuriranje i uklanjanje/brisanje paketa. Paket se sastoji od datoteke i informacije o paketu koja sadrži ime, verziju i opis.

Potekli su iz korporacije Red Hat i podržavaju operativne sisteme Fedora, Red Hat, Centos, SUSE... Takođe podržavaju više CPU arhitehtura u zavisnosti od distribucije.

Konvencija imenovanja rpm paketa: 

``<ime paketa>-<vesrion>-<release>.<architecture>.rpm``

Na primer:

``httpd-tools-2.4.6-el7.x86_64.rpm``

.. note:: Najbolje je koristiti rpm paket dizajniran za sopstvenu distribuciju iako idealno rpm paket napravljen za jednu distribuciju radi i na ostalim distribucijama.


Komanda ``rpm`` može da se koristi za instalaciju, može da detektuje zavisnost od drugih paketa ali ne vrši rezrešavanje zavisnosti automatski. 

Pretraga paketa
---------------

Pretragu paketa možete vršiti na nekom od sledećih sajtova:

https://rpmfind.net/

https://freshrpms.net/

http://rpm.pbone.net/

Spisak komandi
--------------

+----------------------------+--------------------------------------+------------------------------------------------+
|         Komanda            |             Opis komande             |                Primer komande                  |
|                            |                                      |                                                |
+============================+======================================+================================================+
| rpm -ivh {rpm-file}        | Instalira paket                      | rpm -ivh mozilla-mail-1.7.5-17.i586.rpm        |
|                            |                                      | rpm -ivh --test mozilla-mail-1.7.5-17.i586.rpm |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -ql {package}          | Prikazuje putanju do instalacije     |                                                |
|                            | paketa                               |                                                |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -Uvh {rpm-file}        | Nadograđuje paket                    | rpm -Uvh mozilla-mail-1.7.6-12.i586.rpm        |
|                            |                                      | rpm -Uvh --test mozilla-mail-1.7.6-12.i586.rpm |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -ev {package}          | Briše/uklanja instaliran paket       | rpm -ev mozilla-mail                           |
|                            |                                      |                                                |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -ev --nodeps {package} | Briše/uklanja bez provere zavisnosti | rpm -ev --nodeps mozilla-mail                  |
|                            |                                      |                                                |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -qi {package}          | Prikazuje informacije o instaliranom | rpm -qi mozilla-mail                           |
|                            | paketu sa verzijom i kratkim opisom  |                                                |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -qa                    | Prikazuje listu svih instaliranih    | rpm -qa                                        |
|                            | paketa                               | rpm -qa | less                                 |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -qf {/path/to/file}    | Prikazuje koji fajl pripada          | rpm -qf /etc/passwd                            |
|                            | određenom paketu                     | rpm -qf /bin/bash                              |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -qc {package-name}     | Prikazuje listu konfiguracionih      |	rpm -qc httpd                                |
|                            | fajlova za određen paket             |                                                |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -qcf {/path/to/file}   | Prikazuje listu konfiguracionih      | rpm -qcf /usr/X11R6/bin/xeyes                  |
|                            | fajlova za komandu                   |                                                |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -qa --last             | Prikazuje listu nedavno instaliranih | rpm -qa --last                                 |
|                            | paketa                               | rpm -qa --last | less                          |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -qpR {.rpm-file}       | Prikazuje zavisnosti rpm paketa      | rpm -qpR mediawiki-1.4rc1-4.i586.rpm           |
| rpm -qR {package}          |                                      | rpm -qR bash                                   |
+----------------------------+--------------------------------------+------------------------------------------------+
| rpm -qipl {package}        | Prikazuje kompletan info i sadržај   | Slučaj kada rpm nije instaliran                |
|                            | paketa                               |                                                |
+----------------------------+--------------------------------------+------------------------------------------------+


Build rpm paketa
----------------

Neophodno je prvo instalirati rpmbuild:

``sudo yum install rpm-build``

Dodatno možete kreirati ``.rpmmacros`` fajl u vašem home direktorijumu koji ``rpmbuild`` može da koristi za određivanje različitih postavki. Pomoćno podešavanje je ``%_topdir`` koje govori ``rpmbuild``-u gde će biti potrebne komponente za build paketa:

``%_topdir $HOME/rpmbuild``

Kada se po prvi put pokrene ``rpmbuild`` on će konstruisati nekoliko direktorijuma pod ``%_topdir`` ili možemo i sami da to uradimo: 

``mkdir -p ~/rpmbuild/{BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS}``

Hello World
^^^^^^^^^^^

Na primeru programa "Hello World" će biti prikazan ceo postupak. Prvo bi trebalo kreirati direktorijum za source file i Makefile: 

``mkdir helloworld-1.0``

Potom se kreiraju main.c i Makefile fajlovi.

``main.c`` sadrži:

.. code-block:: c

 #include <stdio.h>
  int main (int argc, char argv[]) {
   printf("Hi\n");
   return 0;
 }

``Makefile`` :

.. code-block:: bash

 DESTDIR ?=
 PREFIX ?= /usr/local

 helloworld:
  gcc main.c -o helloworld

 install: helloworld
  install -m 0755 -d $(DESTDIR)$(PREFIX)/bin
  install -m 0755 helloworld $(DESTDIR)$(PREFIX)/bin
 

Building and Testing
^^^^^^^^^^^^^^^^^^^^

Pokretanjem komande ``make`` se build-uje program i biće kreirana binary fajl helloworld koji se može izvršiti pokretanjem komande ``./helloworld`` . 
Ako je sve urađeno kako treba trebalo bi da se dobije "Hi" kao rezultat. 

Nakon ovoga možemo instalirati binary koristeći ``sudo make install`` komandu. Nakon ovoga če naš program "helloworld" biti smešten u ``/usr/local/bin``

Komandom:

``sudo rm -rf /usr/local/bin/helloworld``

uklanjamo naš binary.

Pakovanje našeg programa
^^^^^^^^^^^^^^^^^^^^^^^^

Pakujemo source fajlove u arhivu:

``tar -czvf helloworld-1.0.tar.gz helloworld-1.0/``

``rpmbuild`` može za source da koristi URL ili lokalni fajl. U ovom primeru se koristi lokalni tako da će ``rpmbuild`` tražiti SOURCES direktorijum pod ``%_topdir``


Kreiranje SPEC fajla
^^^^^^^^^^^^^^^^^^^^

SPEC fajl govori ``rpmbuild``-u kako da build-uje i pakuje software. U ovom slučaju u pitanju je C program koji mora da se kompajlira i kopira u propisani direktorijum za instalaciju. 

.. code-block:: bash

  
 Name:           helloworld
 Version:        1.0
 Release:        1%{?dist}
 Summary:        A hello world program
 
 License:        GPLv3+
 URL:            https://blog.packagecloud.io
 Source0:        helloworld-1.0.tar.gz
 
 Requires(post): info
 Requires(preun): info

 %description
 A helloworld program from the packagecloud.io blog!
 
 %prep
 %setup
 
 %build
 make PREFIX=/usr %{?_smp_mflags}
 
 %install
 make PREFIX=/usr DESTDIR=%{?buildroot} install
 
 %clean
 rm -rf %{buildroot} 

 %files
 %{_bindir}/helloworld
 
 
.. note: Source0 u fajlu iznad može biti URL


Build RPM uz pomoć rpmbuild-a
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Nakon svega možemo pokrenuti ``rpmbuild`` da kreiramo RPM paket od SPEC fajla.

``rpmbuild -ba helloworld.spec``

``-ba`` flag će build-ovati source RPM (.src.rpm) i binary RPM (.rpm) pakete. 

Prilikom ovoga ćemo dobiti dosta debug poruka koje nam mogu biti od koristi.  ``rpmbuil`` će izlistati fajlove koje kreira:

``Wrote: /home/luka.sisic/rpmbuild/SRPMS/helloworld-1.0-1.el6.src.rpm``
``/home/luka.sisic/rpmbuild/RPMS/x86_64/helloworld-1.0-1.el6.x86_64.rpm``


Sada se mogu kopirati iz OUTPUT direktorijuma u YUM repozitorijum.
