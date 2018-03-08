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
  - User devops has wheel as the secondary group 
```[devops@client1 ~]$ id devops
  uid=1000(devops) gid=1000(devops) groups=1000(devops),10(wheel)```
  - /etc/sudoers contains : 
```%wheel  ALL=(ALL)       NOPASSWD: ALL```