Git
===

Git osnovne komande
--------------------

1.Konfiguracija gita
^^^^^^^^^^^^^^^^^^^^

Podesite ime koje će biti "povezano" sa vašim commit-ima i tagovima.

git config --global user.name "Vase Ime"

Podesite email adresu koja će biti "povezana" sa vašim commit-ima i tagovima

git config --global user.email "you@example.com"

2. Počnite sa projektom
^^^^^^^^^^^^^^^^^^^^^^^

Sa:

git init [project_name]

Kreira novi lokalni repozitorijum. Ako je koriščeno project_name git će kreirati novi
direktorijum sa imenom project_name i inicijalizovaće se unutar tog direktorijuma.
Ako se ne prosledi project_name novi repozitorijum će se inicijalizovati u trenutnom direktorijumu.

Da bismo preuzeli celu istoriju projekta sa udaljenog repozitorijuma:

git clone [project_url]

Ako ne želimo da sačuvamo u istoriju projekta neke datoteke (npr: logove, binary ...) to radimo uz
pomoć .gitignore datoteke.

.gitignore

Npr ako nećemo .class, .swo, .swp fajlove .gitignore datoteka će izgledati ovako

.. code-block:: bash

 *.swp
 *.swo
 *.class

3. Day-to-day work
^^^^^^^^^^^^^^^^^^
   
git status - status vašeg rada ( new, staged, modified files, current branch).

git diff [file] - pokazuje promene između working directory-a i staging area-e.

git diff --staged [file] - pokazuje promene u staging area-i koje nisu commit-ovane.

git checkout -- [file] - odbacuje promene u working directory-u. OPREZNO SA OVOM KOMANDOM!

git add [file] - dodaje fajl u staging.

git add . - dodaje sve promene iz aktuelnog direktorijuma.

git reset [file] - vraća fajl iz staging-a u working directory.

git commit -m "komentar" - kreira novi commit od promena koje su dodate u staging area-u. Commit MORA IMATI KOMENTAR!

git rm  [file] - briše fajl iz working directory-a

git stash - Prebacuje trenutne promene u stash.

git stash pop - Dodaje sadržaj stash-a u working directory i čisti stash.

git stash drop - Briše sadržaj stash-a bez dodavajna u working directory.


4. Git branching model
^^^^^^^^^^^^^^^^^^^^^^

git branch [-a] - lista sve lokalne branch-eve u repozitorijumu, a sa paramterom -a prikazuje sve branch-eve (remote).

git branch [name] - Kreira novi branch i postavlja HEAD da pokazuje na taj branch.

git checkout [-b] [name] - prebacuje working directory na određeni branch. Sa parametrom -b git će kreirati određeni branch ako ne postoji.

git merge - [from name] - merdžuje određeni branch na trenutni branch na kom se nalazimo.

git branch -d [name] - brise odabrani branch ako je merdžovan u neki drugi. -D umesto -b za permanentono brisanje


5. Pregled vašeg rada 
^^^^^^^^^^^^^^^^^^^^^

git log  [-n count] - lista istoriju commit-a. Parametar -n se koristi za broj poslednjih commit-a.

git reflog - lista izvršene operacije (checkout, commit ...) napravljene u lokalnom repozitorijumu.


6. Tagovanje poznatih commit-a
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

git tag - lista sve tagove

git tag [name] [commit sha] - kreira tag sa imenom za trenutni commit, a ako upotrebimo opciju sa commit sha tagujemo određeni commit (čiji je sha) a ne trenutni.

git tag -d [name] - briše tag iz lokalnog repozitorijuma.


7. Vraćanje promena
^^^^^^^^^^^^^^^^^^^

git reset [--hard] [target reference] - Prebacuje trenutni branch na target reference i ostavlja razliku kao ne commit-ovane promene. Kada se koristi --hard sve promene odbaci.

git revert [commit sha] - kreira novi commit vraćajući promene sa određenog commit-a (onaj čiji je sha). 

8. Sinhronizacija repozitorijuma
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


git fetch [remote] - preuzima promene sa udaljenog repozitorijuma ali ne ažurira praćene branch-eve.

git fetch --prune [remote] - Ukoliko uz fetch želite da vam se i obrisu svi branch-evi koje je neko drugi obrisao na udaljenom repozitorijumu.

git pull [remote] - preuzima promene sa udaljenog repozitorijuma i merdžuje trenutni branch 

git push [--tags] [remote] - Push-uje lokalne promene na udaljeni repozitorijum. Koristi --tags da push-uje tagove.

git push -u [remote] [branch] - Push-uje lokalni branch na udaljeni repozitorijum. 

Indeks pojmova
^^^^^^^^^^^^^^

commit - an object;
bracnh - a reference to a commit;
tag - a reference (standard) or an object (annotated);
HEAD - a place where your working directory is now;
working directory - you do all the hacking right here;
index (staging area) - only index will be committed;
stash - for the mess you don't want to include.;
repository - changes commited here will be safe IF YOU ARE DOING BACKUPS;
