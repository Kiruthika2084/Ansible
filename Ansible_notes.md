## Ansible : IT automation tool

open-source

Redhat Ansible tower ia a paid,GUI based tool to manage Ansible automation

Ansible AWX -> open source free software

- Each specific task in ansible is written through modules
- Multiple modules are written in sequntial order
- Multiple modules for related tasks is called a play
- All plays together is called a playbook
- playbook is written in YAML


Web-servers:

	Install httpd -> module
	Enable httpd -> module
	start httpd -> module                   =========> play

 enable http port on furewall -> module         =========> play  +++++>>>>> task
	
	Database servers:
	 Login to DB -> module
	 create a table -> module
	 restart DB -> module                   ==========> play ++++++>>>> task  ))))))))))))))))))set of plays----> playbook
		
**All tasks in YAML are executed in sequential order
		
ansible-playbook --syntax-check pb.yaml ---> syntax check

ansible-playbook --check pb.yaml ---> dry-run


--extra-vars -> to mention the extra variables

Adhoc run : ansible localhost -m ping


Module to print something on the screen : debug
---
- name: my first playbook
  hosts: localhost

  tasks:
  - name : test
    debug: msg="hallo"
~            
Run the playbook:
-------------------
ansible-playbook hallo.yaml


Running multiple tasks:
=========================

- name: run multiple tasks
  hosts: localhost

  tasks:
  - name: test
    debug: msg="hallo"
  - name: ping 
    ping:

Remote client hosts file syntax
--------------------------------
-All remote clients are considered inventory in Ansible
-Ansible keeps its inventory info in host name named : /etc/ansible/hosts, if inventory location is kept different,
 specify ansible-playbook -i inventory-location
-The host file is created during ansible installation

hosts are either identified by fqdn/ip
Can use headers to group clients
[db-servers]
10.133.42.0
10.132.23.0
[app-servers]
10.0.0.1

Ansible has a default for all
[all]
10.0.0.1
''
''

can use iprange 10.0.0.[11-14]

Establish connection to remote hosts
-------------------------------------
Generate SSH keys on the control nodes and copy it over to the clients for passwordless
ssh-keygen
ssh-copy-id 10.0.0.99 (root password of remote is required)

ssh 10.0.0.99 -> success

Extra vars
---------------
tasks:
- name: change password
  user:
     name: george
     update_password: always
     password: "{{ newpassword|password_hash }}"
     
ansible-playbook changepass.yml --extra-vars newpassword="@#jhdj"
     
Download package from URL
---------------------------

---
- name: Download Tomcat from tomcat.apache.org
  hosts: localhost
  tasks:
   - name: Create a Directory /opt/tomcat
     file:
       path: /opt/tomcat
       state: directory
       mode: 0755
       owner: root
       group: root
   - name: Download Tomcat using get_url
     get_url:
       url: https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.78/bin/apache-tomcat-8.5.78.tar.gz
       dest: /opt/tomcat
       mode: 0755
       group: kiruthika
       owner: kiruthika
       
 Loop:
 ----------
 ---
- name: Find a process and kill it 
  hosts: 10.253.1.115
 
  tasks:
    - name: Get running processes from remote host
      ignore_errors: yes
      shell: "ps -few | grep top | awk '{print $2}'"
      register: running_process
 
    - name: Kill running processes
      ignore_errors: yes
      shell: "kill {{ item }}"
      with_items: "{{ running_process.stdout_lines }}"

Start a playbook at a specific task
---------------------------------------
ansible-playbook yamlfile.yaml --start-at-task 'task name'

Ansible adhoc commands
-------------------------

ansible [target] -m [module] -a "[module-options]"

example :  ansible localhost -m ping
	   ansible all -m file -a "path=/path state=touch mode=700"
	   ansible all -m file -a "path=/path state=absent"
	   ansible all -m copy -a "src="" dest=""
	   
setup : getting system info from remote clients
ansible -m all setup

ansible client1 -a "/sbin/reboot"

Roles:
----------
-simplifies long playbooks by grouping tasks into smaller playbooks
-easier to reuse
-Roles are like templates which can be called by playbook

to create roles
/etc/ansible/roles -> create directories(role1,role2) -> create sub-directory(tasks) inside each directory -> main.yaml inside tasks folder

Ansible-galaxy
-----------------
galaxy.ansible.com

can download roles from here
ansible-galaxy install xxxxx

Tags
----------
Tags are references or aliases to the task
tags: tag-name ( aligned with -name in yaml file)

To run particular tag
----------------------------
ansible-playbook playbook.yaml -t tag-name

To list all tags
------------------
ansible-playbook playbook.yaml --list-tags


Variables
---------------
---
- name: Package installation
  hosts: all
  vars:
   pack: httpd 
  
  tasks:
  - name: Install package
    yum:
     name: “{{ pack }}”
     state: present

  - name: Start service
    service:
     name: “{{ pack }}”
     state: started
     
Variables in inventory file
-----------------------------
[abc:vars]
fooserver=foo.abc.example.com
ntpserver=ntp.abc.example.com

server1 ansible_host=10.0.0.1
server1 ansible_host=10.0.0.2

Handlers
----------------
Handlers are executed at end of play once all tasks are finished.
In Ansible, handlers are typically used to start, reload, restart and stop services.

example: reload firewalld, before enabling http service.

handlers are tasks that only run when notified
---
- name: Enable service on firewalld
  hosts: localhost
  tasks:

- name: Open port for http
  firewalld:
   service: http
   permanent: true
   state: enabled
  notify:
  - Reload firewalld
 
- name: Ensure firewalld is running
    service:
     name: firewalld
     state: started  -----> handler will run after this

handlers:
  - name: Reload firewalld
    service:
     name: firewalld
     state: reloaded

Conditions:
-----------------
allow Ansible to take actions based on certain conditions
---
- name: install httpd
  hosts: localhost
  
  tasks:
   - name: install apache
     apt-get:
        name: apache2
	state: present
     when ansible_os_family(builtin variable) = "ubuntu"
     
how to get these built in variables?

variables are gathered from facts
# ansible localhost -m setup

Loops:
-------------
---
- name: Install packages thru loop
  hosts: localhost
  vars:
   packages: [ftp,telnet,htop)  

  tasks:
- name: Install package
  yum
    name: ‘{{items}}’
    state: present
  with_items: ‘{{packages}}’ -> no need with yum, otherwise required
  
 ---
- name: Create users thru loop
  hosts: localhost

  tasks:
- name: Create users
  user:
    name: ”{{ item }}””
  loop:
      - jerry
      - kramer
      - eliane
---------------------------
Ansible vault
------------------

Ansible vault feature password protect the code

# ansible-vault create ansvault.yaml

ansible-playbook ansvault.yaml --> error

TO run
ansible-playbook ansvault.yaml --ask-vault-pass

TO view
ansible-playbook view ansvault.yaml 

TO edit
ansible-playbook edit ansvault.yaml 

To get list of options
ansible-vault --help

if existing file???
ansible-playbook encrypt ansvault.yaml 

Encrypt strings within a playbook
-------------------------------------

ansible-vault encrypt-string httpd
asks vault password:
==> encrypted string

paste it in playbook
# ansible-vault encrypt_string httpd
# ansible-vault create/encrypt outputbystring.yml
---
- name: Test encrypted output
  hosts: localhost
  vars:
    secret: !vault |
$ANSIBLE_VAULT;1.1;AES256
34343066363535633538313838383363616161633163326638303737383537316563633865653166
3237613536323465326636623465343866646332633362630a636533303762636630313830303531
66613766666130346135623436356138303262656162353330623535346135613566333439663230
3265333738653532310a353632373565386138373832656336393861323030643263323535326230
3164
tasks:
- name: Print encrypted string
debug:
var: secret

TO run
ansible-playbook ansvault.yaml --ask-vault-pass


Ansible Management tools
------------------------------

Additional commands
-------------------
ansible-config
shows or modifies ansible configuration

ansible-doc -l
