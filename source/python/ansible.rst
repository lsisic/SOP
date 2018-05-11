Ansible
=======

Instalacija virtual environment-a:
-----------------------------------

``sudo apt install python-virtualenv``

Kreiranje virtualenv za Ansible:

``virtualenv Ansible``

Ovom komandom kreiramo direktorijum Ansible u trenutnom direktorijumu.
Ovaj direktorijum (Ansible) sadrži kopiju Python-a koja će instalirati module unutar tog direktorijuma.

Da bismo koristili virtualenv moramo ga aktivirati:

``source Ansible/bin/activate``

Nakon toga dobijamo prompt:

``(Ansible) $``

Sada kada je aktivan virtualenv možemo koristiti Python komande kao npr. "pip" koji će instalirati module unutar virtualenv.

Instalacija Anisble-a:
------------------------
``(Ansible) $ pip install ansible``

Proverimo instaliranu verziju:

``(Ansible) $ ansible --version``

Output ove komande bi trebalo da izgleda:

.. code-block:: bash

 ansible 2.4.3.0
		  config file = None
		  configured module search path = [u'/home/luka/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
	 	  ansible python module location = /home/luka/Ansible/local/lib/python2.7/site-packages/ansible
		  executable location = /home/luka/Ansible/bin/ansible
	 	  python version = 2.7.12 (default, Dec  4 2017, 14:50:18) [GCC 5.4.0 20160609]
 


Za ispravno funkcionisanje ansibla je neophodno još kreirati i ansible.cfg fajl kao i inventory fajl. To možemo uraditi uz pomoć vim-a ili nano-a.

Izgled asible.cfg fajla:

.. code-block:: bash

 [defaults]
 remote_user = root
 sudo_user = root
 private_key_file = ~/.ssh/id_rsa
 host_key_checking = False
 hostfile = inventory

 [ssh_connection]
 pipelining = True


Izgled inventory fajla:

.. code-block:: bash

 [test]

 192.168.1.2

Naveden je samo primer kako se dodaju hostovi u inventory file. 

Oba ova fajla se moraju nalaziti unutar kreiranog Ansible direktorijuma (virtual environment-a)

Nakon ovoga možemo proveriti da li ansible radi tako što ćemo pingovati sve hostove komandom:

``ansible all -m ping``
