# ansible-playbooks - groupe 2
Collection de playbooks utilisée par le groupe 2 lors des différents challenges



## Challenge 1

Ce challenge nous a permis d'installer une RHEL 7.4 sur une machine virtuel ( via Virtual Box ). 

Il a fallu trouver la doc et une nouvelle ISO pour remplacer notre ISO corrompue.



## Challenge 2

Dans ce challenge, il a fallu changer la méthode de configuration d'un serveur. L'utilisation de Ansible et d'un playbook `runme.yml` a remplacé l'utilisation de scripts Shell. 



## Challenge 3

Dans ce challenge, il a été demandé d'installer et de configurer un serveur Ansible Tower ainsi que 3 nodes gérées via le Tower.

Playbook de test utilisé pour tester notre installation de Ansible Tower :

`ansibletowa.yml`



Ce playbook a été joué via Tower sur les différentes nodes de notre architecture. 



##### Architecture

| Server | IP Address | OS | NICs | Hostname |
| ------ | ---------- | -- | ---- | -------- |
| Tower | 192.168.56.10 | CentOS 7.4 | 2 nics | tower.g2.lab.com |
| node1 | 192.168.56.11 | CentOS 7.4 | 2 nics | node1.g2.lab.com |
| node2 | 192.168.56.12 | CentOS 7.4 | 2 nics | node2.g2.lab.com |
| node3 | 192.168.56.13 | CentOS 7.4 | 2 nics | node3.g2.lab.com |



## Challenge 4

Dans ce challenge, il a été demandé d'installer et configurer deux serveurs ( web et database ) en utilisant un playbook Ansible. Ce playbook sera aussi utilisé via Tower. 

Playbook utilisé 
`challenge4.yml`



Ce playbook a été pensé pour utiliser des  expressions conditionnelles afin de définir sur quels hôtes sera appliqué la configuration.

Ces conditions se  basent sur l'appartenance de l'hôte à un groupe ( via la variable magique groups ). 

Le fichier inventory utilisé comporte deux groupes webserver et database. 

```
node1.g2.lab.com
node2.g2.lab.com
node3.g2.lab.com

[webserver]
node1.g2.lab.com

[database]
node2.g2.lab.com

```



On aurait pu séparer en deux plays. 



## Challenge 5

Ce challenge a dispau dans la ansible-galaxy. 



## Challenge 6

Ce challenge consiste à installer et configurer un serveur Satellite 6 sur une RHEL7 et d'y attacher un serveur CentOS7.



## Hors challenge

Playbook pour le fun
`helloworld.yml`



