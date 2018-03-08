# SX Challenges
This project contains a collection of playbooks built for StartX Challenges in march 2018 by group 3 (Meiling and Erwin)

These playbooks are supposed to be ran on an Ansible Tower Architecture, mainly composed by a Tower server and 3 Ansible/Tower clients configured as following:

Server | Function | Memory | Storage | IP addresses | OS
---|---|---|---|---|---
tower1.lab.com  | Tower | 4 GB | 16 GiB | 192.168.56.10 (NIC Host-only adapter, static) 10.0.2.15/24 gw: 10.0.2.2 (NIC NAT, dhcp) | CentOS 7.4
client1.lab.com | Ansible client | 2 GB | 10 GiB | 192.168.56.11 (NIC Host-only adapter, static) 10.0.2.15/24 gw: 10.0.2.2 (NIC NAT, dhcp) | CentOS 7.4
client2.lab.com | Ansible client | 2 GB | 10 GiB | 192.168.56.12 (NIC Host-only adapter, static) 10.0.2.15/24 gw: 10.0.2.2 (NIC NAT, dhcp) | CentOS 7.4
client3.lab.com | Ansible client | 2 GB | 10 GiB | 192.168.56.13 (NIC Host-only adapter, static) 10.0.2.15/24 gw: 10.0.2.2 (NIC NAT, dhcp) | CentOS 7.4

All of theses servers are hosted by VirtualBox and deployed using the procedure SXchallenge #1 (Installation of a RHEL Server) using the ISO image CentOS-7-x86_64-DVD-1708.iso available here : http://instructor.classroom.local/content/CentOS-7-x86_64-DVD-1708.iso

With a proper network configuration (as described in the given table), servers can use the YUM repositories of CentOS as configured during the installation.

Ansible is installed on tower1.lab.com directly from the extras YUM repository. Tower is installed from a tarball downloaded on the Red Hat Website.

Note: An ssh key has been deployed on the 3 clients. This key enable devops user from tower1 to reach all clients through ssh without using a password. In other words, ssh devops@clientX.lab.com from tower1.lab.com will not ask for a password :
    • User devops has wheel as the secondary group 
[devops@client1 ~]$ id devops
uid=1000(devops) gid=1000(devops) groups=1000(devops),10(wheel)
    • /etc/sudoers contains : 
%wheel  ALL=(ALL)       NOPASSWD: ALL
Installation of Tower
Tower is installed on tower1.lab.com
    • Ansible is installed from the extras YUM repository
[root@tower1 ansible-tower-setup-3.2.3]# yum install ansible
Loaded plugins: fastestmirror, langpacks
base                                                                                                            | 3.6 kB  00:00:00     
extras                                                                                                          | 3.4 kB  00:00:00     
updates                                                                                                         | 3.4 kB  00:00:00     
(1/4): base/7/x86_64/group_gz                                                                                   | 156 kB  00:00:00     
(2/4): extras/7/x86_64/primary_db                                                                               | 167 kB  00:00:00     
(3/4): updates/7/x86_64/primary_db                                                                              | 6.0 MB  00:00:10     
(4/4): base/7/x86_64/primary_db                                                                                 | 5.7 MB  00:00:22     
Determining fastest mirrors
 * base: centos.quelquesmots.fr
 * extras: rep-centos-fr.upress.io
 * updates: fr.mirror.babylon.network
Resolving Dependencies

... Output omitted ...

Installed:
  ansible.noarch 0:2.4.2.0-2.el7                                                                                                       

Dependency Installed:
  PyYAML.x86_64 0:3.10-11.el7                                              libyaml.x86_64 0:0.1.4-11.el7_0                            
  python-babel.noarch 0:0.9.6-8.el7                                        python-backports.x86_64 0:1.0-8.el7                        
  python-backports-ssl_match_hostname.noarch 0:3.4.0.2-4.el7               python-cffi.x86_64 0:1.6.0-5.el7                           
  python-enum34.noarch 0:1.0.4-1.el7                                       python-httplib2.noarch 0:0.9.2-1.el7                       
  python-idna.noarch 0:2.4-1.el7                                           python-ipaddress.noarch 0:1.0.16-2.el7                     
  python-jinja2.noarch 0:2.7.2-2.el7                                       python-markupsafe.x86_64 0:0.11-10.el7                     
  python-paramiko.noarch 0:2.1.1-2.el7                                     python-passlib.noarch 0:1.6.5-2.el7                        
  python-ply.noarch 0:3.4-11.el7                                           python-pycparser.noarch 0:2.14-1.el7                       
  python-setuptools.noarch 0:0.9.8-7.el7                                   python2-cryptography.x86_64 0:1.7.2-1.el7_4.1              
  python2-jmespath.noarch 0:0.9.0-3.el7                                    python2-pyasn1.noarch 0:0.1.9-7.el7                        
  sshpass.x86_64 0:1.06-2.el7                                             

Complete!

    • A tarball of Tower can be downloaded from the Red Hat Download center: https://www.ansible.com/tower-trial
      
    • This tarball should be uploaded to tower1.lab.com and uncompressed on it

[root@tower1 ~]$ tar xf ~devops/ansible-tower-setup-latest.tar.gz
[root@tower1 ~]$ cd ~devops/ansible-tower-setup-3.2.3

    • List the content of the installation directory
      
[root@tower1 ansible-tower-setup-3.2.3]# ll
total 56
-rw-r--r--.  1 devops devops   143 Feb 20 01:47 backup.yml
drwxr-xr-x.  2 devops devops    17 Feb 20 01:47 group_vars
-rw-r--r--.  1 devops devops  7773 Feb 20 01:47 install.yml
-rw-r--r--.  1 devops devops   531 Mar  6 15:17 inventory
drwxr-xr-x.  2 devops devops 12288 Feb 20 01:47 licenses
-rw-r--r--.  1 devops devops  2526 Feb 20 01:47 README.md
-rw-r--r--.  1 devops devops  1361 Feb 20 01:47 restore.yml
drwxr-xr-x. 20 devops devops  4096 Feb 20 01:47 roles
-rwxr-xr-x.  1 devops devops  9628 Feb 20 01:47 setup.sh

    • In that directory, the inventory file needs to be edited in order to set passwords for the Ansible Tower admin account (admin_password), the PostgreSQL database user account (pg_password), and the RabbitMQ messaging user account (rabbitmq_password). 
      
[root@tower1 ansible-tower-setup-3.2.3]# grep password inventory 
admin_password='******'
pg_password='******'
rabbitmq_password='******'

    • Then, run the setup.sh script

[root@tower1 ansible-tower-setup-3.2.3]# ./setup.sh 
Using /etc/ansible/ansible.cfg as config file

... Output omitted ...

PLAY [Install Tower isolated node(s)] ****************************************************************
skipping: no hosts matched

PLAY RECAP ******************************************************************************************************
localhost                  : ok=133  changed=63   unreachable=0    failed=0   

The setup process completed successfully.
Setup log saved to /var/log/tower/setup-2018-03-06-15:20:20.log

Registration of Tower
Access to Tower by using the Web GUI : https://tower1.lab.com
Connect as “admin” with the password given in the inventory file before installing Tower
Click on “REQUEST LICENCE” button and choose the “FREE Ansible TOWER TRIAL - LIMITED FEATURES UP TO 10 NODES”. The Ansible Tower Basic Edition trial license key will be send on the email address given during the registration. Save the .txt attached file and go back on the Tower Web GUI.
Click on “BROWSE” and choose the license key file previously saved, check the checkbox “I agree to the End User License Agreement” and click on “SUBMIT”
Using Tower (without GIT)
Before using GIT integrated with Tower, this is the way to use Tower with only local resources.
Connect as “admin” to the Web GUI : https://tower1.lab.com
Create a credential
The credential is the one used by tower to connect to clients using SSH. In this case, the devops user is used.
Go to “Settings” (in the top right corner) and click on “CREDENTIALS”. Create a new credential by clicking on “+ Add” (the green button on right). Fill the following fields:
    • Name: choose one (devops_user for example)
    • Credential type: Machine
    • Username: the user supposed to connect to clients without password (devops for example)
    • Password: the password of the user
Then, click on save.
Create an inventory
This inventory will contain all the Ansible clients.
Go to “Inventories” and click on “+ Add” (the green button on right). Fill the following fields:
    • Name: choose one (clients for example)
    • Organization: Default
Click on “Hosts”, then “+ Add Host” (the green button on right) for adding all clients.
Eventually, click on “Groups” for create one and add clients into it.
Then, click on save.
Create a project
The project will contain the playbook created for SXChallenge #2.
Firstly, create a subdirectory in /var/lib/awx/projects/ and copy the playbook into it. 
For example, creating the subdirectory localone and copying the playbook called runme.yml will result :
[root@tower1 ~]# ls -lR /var/lib/awx/projects/
/var/lib/awx/projects/:
total 0
drwxr-xr-x. 3 awx  awx  58 Mar  6 17:42 _4__demo_project
-rwxr-xr-x. 1 awx  awx   0 Mar  6 17:14 _4__demo_project.lock
drwxr-xr-x. 2 root root 24 Mar  6 17:43 localone

/var/lib/awx/projects/_4__demo_project:
total 8
-rw-r--r--. 1 awx awx 117 Mar  6 17:14 hello_world.yml
-rw-r--r--. 1 awx awx  55 Mar  6 17:14 README.md

/var/lib/awx/projects/localone:
total 8
-rw-r--r--. 1 root root 4967 Mar  6 17:43 runmme.yml

Then, go back to the Web GUI and go to “Projects”. Click on “+ Add” (the green button on right) and fill the following fields:
    • Name: Choose one (localone for example)
    • Orgnaization: Default
    • SCM Type: Manual
    • Project Base Path: /var/lib/awx/projects (by default)
    • Playbook Directory: Choose the previously created directory (localone here)
Then, click on save.
Create a template
The template will contain alll the elements previously created (Credential, Inventory, Project).
Go to “Inventories” and click on “+ Add” (the green button on right). Fill the following fields:
    • Name: Choose one (localone for example)
    • Job Type: Run
    • Inventory: Choose the previously created one (here: clients)
    • Project: Choose the previously created one (here: localone)
    • Playbook: Choose on available on the directory (here: runme.yml)
    • Credential: Choose the previously created one (here: devops_user)
    • Verbosity: 0 (by default)
Then, click on save.


Now, a run can be launched :-)

Go to “Templates” and click on the rocket located at the end of line of the created template.
