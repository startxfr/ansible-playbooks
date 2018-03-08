# ansible-playbooks
Ansible playbook repository with a collection of playbooks build around Redhat like distributions

This playbook is used to deploy a php application.

This playbook use 2 roles find on github

HTTP role : https://github.com/bertvv/ansible-role-httpd

This role is configured with default role configuration, nothing have been changed.

Mariadb role: https://github.com/bertvv/ansible-role-mariadb

This role is configured with this user :

          mariadb_users:
            - name: devops
              password: redhat
              priv: '*.*:ALL,GRANT'
              host: '192.168.56.%'
    
And configured to listen on the interface with the IP 192.168.56.12 :

          mariadb_bind_address: '192.168.56.12'
          

The rest of the configuration is configurated with default values.

This playbook after install httpd and mariadb service deploy a php app.
You can define the url of this file in the playbook php_appli.yml at the field:

            php_app_url: http://instructor.classroom.local/content/index.php
            
As you can see, default value is http://instructor.classroom.local/content/index.php

This playbook also open your ports with firewalld and configure seboolean to let the httpd service connect to the mariadb socket with a distant host.

You have define the host where you'll deploy you application with the field "hosts:" in the playbook:
So you also have to configure the field "mariadb_bind_address:" and the field "host" in the user definition in the ansible-role-mariadb-master/defaults/main.yml file (as you can see in the role documentation) 


To run the playbook with the role you just have to run the playbook with all field mentionned before configurated and host the 2 hosts defined
