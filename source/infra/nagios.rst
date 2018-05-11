Nagios
======

Nagios instalacija
------------------

Nagios ćemo instalirati uz pomoć Ansibla (kojeg smo ranije instalirali).

Prvo je neophodno aktivirati Ansible:

.. code-block:: bash

 source putanja_do_virtenv_za_ansible/bin/activate

Za lakši rad sa Ansible-om je preporučeno da se nalazimo unutar virtenv za naš Ansible:

.. code-block:: bash

 cd putanja_do_virtenv_za_ansible

Sledeći korak koji mora da se odradi je dodavanje hosta na koji želimo da instaliramo nagios. U inventory fajl dodamo:

.. code-block:: bash

 [nagios]
 192.168.1.1

.. note:: IP adresa iznad je uzeta samo za primer. Na njenom mestu upisujete IP adresu hosta na kome želite da instalirate nagios. 


Nakon ovoga imamo podešenu infrastrukturu. Neophodno je preuzeti ansible playbook za instalaciju nagiosa. Isti možete preuzeti sa:

 http://git.linux.in.rs/infra/nagios/tree/test

Nakon kloniranja sa gitlab-a neophodno je kopirati direktorijum ansible-nagios u direktorijum u kome se nalazi vasa instalacija Ansibla.

.. note:: Password za nagios možete promeniti unutar group_vars/all fajla. Default password je Password1change .

Komandom :

.. code-block:: bash

 ansible-playbook ansible-nagios/site.yml 


instalirate nagios na željeni host. Ako instalacija "pukne" ponovite komandu. 

NRPE
----

NRPE je dodatak (plugin) za Nagios. Ime NRPE je skraćeno od Nagios Remote Plugin Executor i iz samog imena možemo zaključiti za šta se koristi.
NRPE obezbeđuje izvršavanje plaginova na udaljenom hostu. Sam program NRPE predstavlja samo serverski deo klijent/server programskog para za indirektne provere 
koje se izvršavaju na nadgledanom hostu. Klijentski deo aplikacije čini check_nrpe plagin pomoću kojeg Nagios core komunicira sa nrpe agentom.

Način rada: Na strani nadgledajućeg hosta, Nagios pokreće check_nrpe plagin koji se koristi da pošalje zehteve nrpe agentu za izvršavanje određenih plaginova na udaljenom hostu. Agent nrpe prima zahteve, pokreeće zahtevane plaginove i vraća rezultat provere check_nrpe plaginu. Plagin check_nrpe preuzima izlazne rezultate udaljenih plaginova i šalje ih Nagiosu kado da su njegovi.

Primeri
-------

Provera pinga udaljenog hosta
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Za demonstraciju ce se koristiti kao target host , host sa podignutim kvm-om. IP adresa ce biti proizvoljna , npr: 192.168.56.72

Prvo je neophodno kreirati kreirati fajl kvm.cfg na lokaciji ``/usr/local/nagios/etc/objects/kvm.cfg``

.. code-block:: bash

 define host  {
        use                          linux-server
        host_name                    kvm_server
        alias                        kvmserver Host
        address                      192.168.56.72
        register                     1
 }

 define service {
      host_name                       kvm_server
      service_description             PING
      check_command                   check_ping!100.0,20%!500.0,60%
      max_check_attempts              2
      check_interval                  2
      retry_interval                  2
      check_period                    24x7
      check_freshness                 1
      contact_groups                  admins
      notification_interval           2
      notification_period             24x7
      notifications_enabled           1
      register                        1
 }

Nakon sto se napravi kvm.cfg fajl moramo putanju do istog dodati i u nagios.cfg fajlu koji se nalazi na sledecoj putanji:

``/usr/local/nagios/etc/nagios.cfg``

.. code-block:: bash

    cfg_file=/usr/local/nagios/etc/objects/kvm.cfg

Ovu liniju koda mozemo dodati posle linije za neki slican konfiguracioni fajl, npr za localhost koji se nalazi u 35 redu konfiguracionog fajla ``nagios.cfg`` .

.. note:: Detaljna uputstva kako se dodaju i definisu hostovi i servisi mozete pronaci na zvanicnoj Nagios dokumentaciji.


Dodavanje check_memory skripte
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Skripta koja ce se koristiti u ovom primeru se moze preuzeti sa:

https://www.dropbox.com/s/twef87ni1jbhc5x/check_memory.sh?dl=0

Neophodno je istu sacuvati na sledecoj lokaciji ``/usr/local/nagios/libexec/check_memory.sh`` i uz pomoc ``chmod +x check_memory`` naciniti je izvrsnom.

Posto je predvidjeno da target host bude localhost menjamo ``/usr/local/nagios/etc/objects/localhost.cfg`` fajl tako sto dodajemo sledecih par linija koda unutar SERVICE DEFINITIONS bloka:

.. code-block:: bash


 define service{
	 use			local-service
	 host_name		localhost
	 service_description	Memory
	 check_command		check_memory!-w 80 -c 90
	 }


Takodje se mora izmeniti i ``/usr/local/nagios/etc/objects/commands.cfg`` fajl gde u bloku SAMPLE SERVICE CHECK COMMANDS dodajemo sledeci kod:

.. code-block:: bash

  # 'check_memory' command definiton
  define command{
	 command_name	check_memory
	 command_line	$USER1$/check_memory.sh -w -c
     }

.. note:: Nakon izmena moramo restartovati nagios sa ``systemctl restart nagios``

webinject.pl
^^^^^^^^^^^^

WebInject je besplatan alat za automatsko testiranje web aplikacija i web servisa.
Može se koristiti samostalno ili u vidu nagios plugina (naš slučaj).

.. note:: Više o alatu WebInject možete saznati na sledećem linku http://www.webinject.org

Za demonstraciju mogućnosti ćemo uzeti najprostiji primer. Provera dostupnosti Endavninog veb sajta.

Prvo je neophodno preuzeti webinject.pl plugin. Dobra praksa je da se sačuva na lokaciji ``/usr/local/nagios/plugins/webinject/webinject.pl`` .

Zatim je neophodno kreirati konfiguracione fajlove unutar direktorijuma ``../webinject`` .

``config.xml``

.. code-block:: bash

 <gbalhttplog>onfail</globalhttplog>
 <globaltimeout>10</globaltimeout>
 <testcasefile>testcases/endava.xml</testcasefile>
 <reporttype>nagios</reporttype>


``devel-config.xml``

.. code-block:: bash

 <gbalhttplog>yes</globalhttplog>
 <globaltimeout>20</globaltimeout>

``testcases/endava.xml``

.. code-block:: bash

  <testcases repeat="1">

  <case
      id="1"
      description1="SAMPLE TEST CASE - main Web page"
      method="get"
      url="http://www.endava.com/en/"
      verifyresponsecode="200"
  />

 </testcases>

Potom je neophodno kao i u prethodnom primeru definisati i samu komandu unutar ``../commands.cfg`` konfiguracione datoteke.
Dodajemo sledeće linije koda:

.. code-block:: bash

  # webinject.pl
  define command {
     command_name    webinject
     command_line    /usr/local/nagios/plugins/webinject/webinject.pl -c $ARG1$ $ARG2$
  }

Nakon ovoga je ostalo samo da još kreiramo konfiguracioni fajl ``endava.cfg`` :

.. code-block:: bash

  define host  {
         use                          linux-server
         host_name                    endava
         alias                        endava Host
         address                      www.endava.com
         register                     1
  }

 define service{
     use                      generic-service
     host_name                endava
     service_description      Endava site check
     contact_groups           admins
     check_command            webinject!/usr/local/nagios/plugins/webinject/devel-config.xml!/usr/local/nagios/plugins/webinject/testcases/endava.xml
 }

.. note:: Voditi računa o tome da Nagios ima sva potrebna prava nad webinject.pl plaginom kao i da je neophodno restartovati nagios da bi se promene primenile.









