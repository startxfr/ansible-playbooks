# ansible-playbooks
Ansible playbook repository with a collection of playbooks build around Redhat like distributions
Standalone Mariadb and Apache, php mysql Deployment

    Requires Ansible 2.4 or newer
    Expects CentOS/RHEL 7.4

These playbooks deploy a very basic implementation of Mariadb and Apache(version 2,4), php mysql , . To use them, first edit the "hosts" inventory file to contain the hostnames of the machines on which you want deployed.

Then run the playbook, like this:

ansible-playbook -i hosts play.yml

When the playbook run completes, you should be able to see the Mariadb and Apache php mysql running , on the target machines, and start the services, then add user to mariadb database. after running this playbook, you should to verify user added can connected to Mariadb database.

This is a very simple playbook and could serve as a starting point for more Mariadb and Apache php mysql projects.
Application deployment


Ideas for Improvement

Here are some ideas for ways that this playbook could be extended:

   We would love to see contributions and improvements, so please fork this repository on GitHub project Startx_git_project and send us your changes via pull requests.
