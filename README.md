# Adding-user-in-ansible-client-machine-using-playbook
Adding user in ansible client machine using playbook

# Description
This is an ansible playbook for creating a user with password on client node from Ansible Master node.

# Pre-Requests
 Need to install Ansible on Manager node and client node then add the client node on hosts of master node.
 
 # Steps
 
 1. installing ansible on manager node and client node
 2. adding client node in hosts of manager node
 3. creating yaml file to add user
 4. Checking the syntax and running the file

Now lets get into it..

## Step 1

Installing ansible on manager node and client node

~~~
sudo amazon-linux-extras install ansible2 -y

 ansible --version
ansible 2.9.23
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/ec2-user/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.18 (default, Jun 10 2021, 00:11:02) [GCC 7.3.1 20180712 (Red Hat 7.3.1-13)]

~~~

# Step 2

Adding client node in hosts of manager node


For this i have created a file "hosts" under the user's home directory.

~~~
[ansible]                                                                                        
<server IP> ansible_user="ec2-user" ansible_port=22 ansible_ssh_private_key_file="key.pem"
~~~

Key.pem is the private key that im using for accessing the client node. [ansible] is the group name i have given to the client nodes.


# Step 3

Creating yaml file to add user

useradd.yaml

~~~
---

-  name: "adding user"
   become: true
   hosts: ansible
   tasks:


     - name: "Creating user in ansible client"
       user:
         name: jibincl
         uid: 2026
         group: ec2-user
         password: "{{ passwd|password_hash('sha512') }}"
         shell: /bin/bash
         createhome: yes
         home: /home/jibincl
         state: present

~~~


By running this playbook, a user named "jibincl" will be created on the client node. 

## Step 4

checing the syntax and running the file


Checking the syntax

~~~
 ansible-playbook -i hosts useradd.yaml --syntax-check

playbook: useradd.yml
~~~

Running the file.

~~~
ansible-playbook -i hosts useradd.yml --extra-vars passwd=jibin@123
~~~

Here, im implimenting the password with the command using "--extra-vars passwd=jibin@123"



