# <span style='color:red'>Installation d'un webserver et server de base de données</span>

Groupe 1 - GURUPU

[TOC]

<b style='color:green'>Distribution</b>

Pour tout le monde.

<b style='color:green'>Confidentialité</b>

Non.

<b style='color:green'>Copies supplémentaires</b>

Des copies supplémentaires de ce document peuvent être offerte a tous.

<b style='color:green'>Révisions et versions du document</b>

Gurupu, 08/03/18, version 1



## 1. A propos de ce document

Ce document decrit le playbook d'installation sur 2 machine, webserver et base de données pour le Workshop du 8 Mars 2018.

### 1.1 Audience

Ce document s'addresse a Riad

### 1.2 Sigles et Acronymes

| Acronyme | Description                |
| :------- | :------------------------- |
| NIC      | Network Interface Card     |
| NAT      | Network Adress Translation |

## 2. Configuration des serveurs

### 2.1 Hardware

Les serveurs ont la configuration suivante:

- Distribution: CentOS 7
- Nom: Tower, node1
- Mémoire: 8 Go
- Disques internes: sda: 8 Go

###  2.2 Système d'exploitation

Le serveur a été installé en CentOs 7 x86_64 avec une installation minimal.



### 2.3 Configuration du réseaux

| Serveur | IP addresse   | Système  | NICs                                               |
| ------- | ------------- | -------- | -------------------------------------------------- |
| Tower   | 192.168.56.10 | CentOS 7 | 1 nic: NAT<br />1 nic: configuré en hote seulement |
| node1   | 192.168.56.11 | CentOS 7 | 1 nic: NAT<br />1 nic: configuré en hote seulement |
| node2   | 192.168.56.12 | CentOS 7 | 1 nic: NAT<br />1 nic: configuré en hote seulement |

#### 2.3.1 Configuration du webserver depuis Ansible

Un playbook est utilise pour l'installation:

- Packages (httpd, mod_ssl, php, php-mysql, mariadb, libsemanage-python)
- Firewall (http, https)
- Activation des services httpd
- SELinux (httpd_can_network_connect, httpd_can_network_connect_db)
- Fichier PHP (index.php)

#### 2.3.2 Configuration du server de base de donnees

Un playbook est utilise pour l'installation:

- Packages (mariadb-server, MySQL-python)
- Firewall (mysql)
- Activation des services (mariadb)
- Creation du user mysql 

## Documentation

http://docs.ansible.com/ansible/latest/index.html