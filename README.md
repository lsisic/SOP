# Zajednička dokumentacija

Da biste bili u mogućnosti da koristite zajedničku dokumentaciju morate prvo instalirati Sphinx.
Više o Sphinx-u možete pronaći na [ovom linku](http://www.sphinx-doc.org/en/master/), 
dok na [ovom linku](https://www.youtube.com/watch?v=qrcj7sVuvUA&t=834s) možete pogledati tutorijal koji će vam pružiti bolji uvid u to kako radi Sphinx.

## Instalacija Sphinxa

Pre instalacije je neophodno proveriti sledeće:

Provera verzije Phytona koja sa nalazi na vašem sistemu (u Bash for Windows-u dolazi sa verzijom 3.5)

NOTE: Mozete koristiti bilo koju verziju pythona

Komanda: `python3 --version`

>Output: Python 3.5.2

Pošto ćemo instalirati Sphinx u virtual environment-u neophodno je pre toga instalirati python-pip paket menadžer.

`sudo apt install python-pip -y` 

Zatim je neophodno instalirtati virtualenv. Virtual environment je vrsta sandboxa za python aplikacije (izoluje ih od ostatka sistema).

Komanda:
`sudo pip install virtualenv`

Sa ovim smo završili sa instalacijom neophodnih paketa za instalaciju Sphinx-a.

#### Instalacija sphinxa:

Prvi korak: 
Kreiranje virtual environment-a za Sphinx.


Napraviti folder `~/.local/venv` i u njemu napraviti novi virtualenv.

`virtualenv ~/.local/venv/Dokumentacija`

Aktivacija virtual environment-a:
`source ~/.local/venv/Dokumentacija/bin/activate`

Instaliramo Sphinx:

`sudo pip install sphinx`

Nakon ovoga je Sphinx instaliran.

## Sphinx upotreba

Prvo je neophodno da napravimo direktorijum za našu dokumentaciju:

`mkdir ~/Dokumentacija`

Prelazimo u kreirani direktorijum:

`cd ~/Dokumentacija`

Inicijalizacija Sphinx-a:

Komanda: 

`sphinx-quickstart`

Nakon ovoga se dobija promtp gde će od nas biti očekivano da odgovorimo na nekoliko pitanja:
	
	>> Root path for the documentation [.]:				        <ENTER> 
	> Separate source and build directories (y/N) [n]:		    y
	> Name prefix for templates and static dir [_]:	            <ENTER>
	> Project name:	                                            ime_vaseg_projekta
	> Author name(s):						                    Vase ime
	> Project version:						                    npr: 0.0.1
	> Project release [0.0.1]:					                <ENTER>
	> Source file suffix [.rst]:					            <ENTER>
	> Name of your master document (without suffix) [index]:	<ENTER>
	Nakon ovoga na svaki prompt "odgovorite" sa 			    <ENTER>

Posle uspešnog quick-starta dobićete sledeći output:

>Finished: An initial directory structure has been created.
You should now populate your master file .\source\index.rst and create other documentation
source files. Use the sphinx-build command to build the docs, like so:
   sphinx-build -b builder .\source .\build
where "builder" is one of the supported builders, e.g. html, latex or linkcheck.

Nakon ovoga unosimo komandu:

`make html`

Ova komanda nam "bilduje" osnovni templejt. Takođe će se uz pomoć ove komande "bildovati" nakon svake promene naše dokumentacije.


Sama dokumentacija se menja i pise unutar index.rst falja koji se nalazi u našem direktorijumu:

~/Dokumentacija/index.rst

Primer:
Ako pravite dokumentaciju za SSH, potrebno je:

Napraviti fajl u .rst formatu

`touch ssh.rst`

Editovati fajl uz pomoć tekst editora (Vim ili Nano):

`vim ssh.rst`

Primer pravilnog pisanja i modifikovanja fajla:

```
Naslov
==========

Podnaslov
------------

Sekcija
^^^^^^^^^
```

Nakon što smo kreirali ssh.rst fajl moramo ga uključiti unutar glavnog index.rst fajla:

`vim index.rst`

Default izgled index.rst fajla:
```
.. Dokumentacija documentation master file, created by
   sphinx-quickstart on Thu Feb  8 10:38:16 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Dokumentacija's documentation!
=========================================
.. toctree::
   :maxdepth: 2
   :caption: Contents:

 ssh


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```

Jednino što je dodato je SSH. Kao što se može primetiti "ssh" je bez ekstenzije .rst 

Posle ovih akcija neophodno je opet pokrenuti komandu:

`make clean && make html`

da bi se naše promene primenile.
