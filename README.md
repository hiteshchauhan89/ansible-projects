# Tasks To Be Performed:
1. Setup Ansible cluster with 3 nodes
2. On slave 1 install Java
3. On slave 2 install MySQL server
Do the above tasks using Ansible Playbooks

# install the java 
---
- name: Installing Java
hosts: test
become: true
tasks:
- name: Install Java
apt:
name: openjdk-11-jdk
state: latest
# install mysql
- name: Installing MySQL
hosts: prod
become: true
tasks:
- name: Install MySQL
apt:
name: mysql-server
state: latest
---------------------------------------------------------------------------------------------------------------------------------------------
# Tasks To Be Performed:
1. Create a script which can add text “This text has been added by custom
script” to /tmp.1.txt
2. Run this script using Ansible on all the hosts

1. Create a script which can add text “This text has been added by custom
script” to /tmp.1.txt
Step 1: Create the Shell Script (add_text.sh)
Create a script in the same directory as your Ansible playbook:
#!/bin/bash
echo "This text has been added by custom script" >> /tmp/1.txt
Give it execute permissions:
chmod +x add_text.sh
Step 2: Create the Ansible Playbook (add_text_playbook.yml)
This playbook will:
1.
Copy add_text.sh to all hosts.
2.
Ensure it has execution permission.
3.
Run the script on each host.
First We need to verify that hosts is available or not in ansible master server.
In My case I added only one server.
Then Need to verify the server ping or not
So in this case I used to ansible -m ping all command.so you will see like this output.
Then Create add_text_playbook.yml file and add below contain.
This is the add_text_playbook.yml
---
- name: Run Custom Script to Append Text
hosts: all
become: true
tasks:
- name: Copy script to remote hosts
copy:
src: add_text.sh
dest: /tmp/add_text.sh
mode: '0755'
- name: Execute script on remote hosts
command: /tmp/add_text.sh

2. Run this script using Ansible on all the hosts
Step 3: Run the Ansible Playbook
Then after executing the playbook with:
ansible-playbook add_text_playbook.yml

After execution, the file /tmp/1.txt on all hosts should contain:

After check the slave server you will see the one notepad has been created in text format.

------------------------------------------------------------------------------------------------------------------------------

# Tasks To Be Performed:
1. Create 2 Ansible roles
2. Install Apache2 on slave1 using one role and NGINX on slave2 using the
other role
3. Above should be implemented using different Ansible roles


1.Create 2 Ansible roles
To create ansible roles:
sudo ansible-galaxy init apache
sudo ansible-galaxy init nginx

2. Install Apache2 on slave1 using one role and NGINX on slave2 using the other role
path of Ansible Apache role
/etc/ansible/roles/apache/tasks/
path of Ansible nginx role
/etc/ansible/roles/nginx/tasks/
add the yaml playbook files inside tasks
First need to set the apache task
So to this path /etc/ansible/roles/apache/tasks/ here You need to create the file name is install.yaml
sudo nano install.yaml write this code and save it then exit this file
---
-
name: install apache
apt:
name: apache2
state: present

Then after you will main.yaml file open this file
Sudo nano main.yaml.write this code in main.yaml and save it this and exit from file.
---
-
name: include apache installation tasks
include_tasks: install.yaml

Then go to /etc/ansible/roles/nginx/tasks/ path
Create one file like install.yaml.write this code
---
-
name: Install Nginx
apt:
name: nginx
state: present

Sudo nano main.yaml.write this code in main.yaml and save it this and exit from file.
---
-
name: include Nginx installation tasks
include_tasks: install.yaml

Then go to this path cd /etc/ansible and create one more file for installing
Apache and nginx like name is play.yaml

finally create a playbook to execute the tasks of the role and write this below code in play.yaml
---
— name: installing apache2
hosts: webserver
become: true
roles:
— apache
- name: installing nginx
hosts: prod
become: true
roles:
— nginx

Now Save it this yaml.we need to deploy the both apache and nginx through role based.
Now Run this command.the command is
ansible-playbook play3.yaml

Now we need to see the output in both server.
After complete the deployment go to aws instance and copy the public ip and paste it to browser.
Ansible Slave 1 and Ansible Slave 2
-------------------------------------------------------------------------------------------------------------------------------------------
# Tasks To Be Performed:
1. Use the previous deployment of Ansible cluster
2. Configure the files folder in the role with index.html which should be
replaced with the original index.html
All of the above should only happen on the slave which has NGINX installed
using the role.
1.Create 2 Ansible roles
To create ansible roles:
sudo ansible-galaxy init apache
sudo ansible-galaxy init nginx

2. Install Apache2 on slave1 using one role and NGINX on slave2 using the other role
path of Ansible Apache role
/etc/ansible/roles/apache/tasks/
path of Ansible nginx role
/etc/ansible/roles/nginx/tasks/
add the yaml playbook files inside tasks
First need to set the apache task
So to this path /etc/ansible/roles/apache/tasks/ here You need to create the file name is install.yaml
sudo nano install.yaml write this code and save it then exit this file
---
-
name: install apache
apt:
name: apache2
state: present

Then after you will main.yaml file open this file
Sudo nano main.yaml.write this code in main.yaml and save it this and exit from file.
---
-
name: include apache installation tasks
include_tasks: install.yaml

Then go to /etc/ansible/roles/nginx/tasks/ path
Create one file like install.yaml.write this code
---
-
name: Install Nginx
apt:
name: nginx
state: present

Sudo nano main.yaml.write this code in main.yaml and save it this and exit from file.
---
-
name: include Nginx installation tasks
include_tasks: install.yaml

Then go to this path cd /etc/ansible and create one more file for installing
Apache and nginx like name is play.yaml

finally create a playbook to execute the tasks of the role and write this below code in play.yaml
---
— name: installing apache2
hosts: webserver
become: true
roles:
— apache
- name: installing nginx
hosts: prod
become: true
roles:
— nginx

Now Save it this yaml.we need to deploy the both apache and nginx through role based.
Now Run this command.the command is
ansible-playbook play3.yaml

Now we need to see the output in both server.
After complete the deployment go to aws instance and copy the public ip and paste it to browser.
Ansible Slave 1 and Ansible Slave 2

2. Configure the files folder in the role with index.html which should be
replaced with the original index.html

go to /etc/ansible/roles/apache/files

create one file index.html
in my case I write in index.html
<h1>Hi $user</h1>
After write this code and save this code.
Now then go to go to /etc/ansible/roles/apache/tasks
and create a new file which is copy.yaml


---
- name: copy index.html
  copy: src=/etc/ansible/roles/apache/files/index.html dest=/var/www/html

Then Save this files.
include the copy.yaml file in the main.yaml

- include_tasks: copy.yaml

- 

