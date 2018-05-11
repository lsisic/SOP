Bash for Windows
================

Bash for Windows je podsistem windowsa koji pruža podršku za linux.
Baziran je na Ubuntu sistemu i po defaultu koristi bash kao shell/command language.
Bash for windows nije kontejner ili virtuelna mašina vec subproces windowsa.

.. note:: Iako su predstavljena dva nacina za neophodne korake dovoljno je koristi jedan u zavisnosti od afiniteta.

Prvi korak
^^^^^^^^^^

Neophodno je omoguciti Developer mode na Windowsu. Ovo možemo uraditi na nekoliko nacina.

Kombinacijom WinKey tastera i termina za pretragu "Use developer features":

WINKEY > "Use developer features" > Developer mode (izabrati od ponudenih opcija)

Drugi nacin sa upotrebom PowerShell-a sa administratorskim privilegijama.

PowerShell možemo pokrenuti na sledeci nacin:

WINKEY > "PowerShell" + desni klik "Run as administrator"

U PowerShell je neophodno uneti sledecu komandu:

``.\reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"``



Drugi korak
^^^^^^^^^^^

Bash for Windows je takode moguce pokrenuti na nekoliko nacina.

Kombinacijom tastera WINKEY, termina za pretragu "Turn windows features on or off" i biranjem opcije (štikliranje checkbox-a)
ispred opcije Windows Subsystem For Linux (Beta). Posle ovoga ce biti neophodno restartovanje sistema.

I u ovom koraku možemo upotrebiti PowerShell sa administratorskim privilegijama i uneti komadnu:

``Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux``


Treći korak (pokretanje Bash for Windows)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

CommandPromtp > Run as administrator > uneti komandu "bash"

> C:\windows\system32>bash

Nakon unete komande dobijamo pitanje da li želimo da instaliramo Ubuntu on Windows.
Na prompt odgovaramo sa "y"
Neophodno je sacekati nekoliko minuta nakon cega se ocekuje unos novog korisnickog imena kao i lozinke.
Nakon svih ovih akcija imamo operativan Bash for Windows!

Pokretanje je moguce na više nacina:

1.`CommandPrompt > Run as administrator > komanda "bash"`
> C:\windows\system32>bash

2.`WINKEY` > "Bash on Ubuntu on Windows"