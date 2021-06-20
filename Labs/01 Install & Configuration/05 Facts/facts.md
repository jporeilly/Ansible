## <font color='red'>1.5 Ansible Facts</font>
Ansible facts are the host-specific system data and properties to which you connect. A fact can be the IP address, BIOS information, a system's software information, and even hardware information. Ansible facts help the admin to manage the hosts based on their current condition rather than taking the actions directly without having any info about the system's health.


In this lab we're going to:
* default Facts
* custom Facts
* introduce Variables

---

#### <font color='red'>Ansible Default Facts</font>
You can gather Facts / Variables using the setup command - run as default with Playbooks.


Syntax: ansible [-i inventory file] <servers> -m setup -a "filter=[value]"

on the ansible controller:
```
ansible 10.0.0.2 -m setup
```
Note: returns all the facts about Node1..

to return Facts / Variable just on mounts:
```
ansible 10.0.0.2 -m setp -a "filter=ansible_mounts"
```
you can also display the variable value:
```
ansible all -m debug -a "var=inventory_hostname"
```
can also list groups:
```
ansible localhost -m debug -a "var=groups"
ansible localhost -m debug -a "var=groups.keys()" # returns just group name
```

---

#### <font color='red'>Ansible Custom Facts</font>
You can define three types of custom facts in Ansible.
* Global facts: These facts are accessible from every host in your inventory file.
* Group facts: These facts are only accessible from a specific set of hosts or a host group.
* Host facts: These facts are only accessible from a particular host.


lets create a projects directory
```
cd ansible_projects
mkdir -pv custom_facts/{playbooks,host_vars,group_vars}
```
change to ansible_projects/custom_facts directory:
```
cd ansible_projects/custom_facts
```
create an ansible.cfg:
```
nano ansible.cfg
```
copy the following lines your ansible.cfg file:
```
[defaults]
inventory           = hosts
host_key_checking   = False
```

</br>

**Global Facts**  
create an Ansible inventory file hosts:
```
nano hosts
```
add the Nodes:
```
10.0.0.2
10.0.0.3

[all:vars]
web_url=https://learning.lumada.hitachivantara.com/
```
save..

create a new playbook:
```
nano playbooks/print_global_fact.yaml
```
copy the following lines:
```
- hosts: all
  user: ansadmin
  tasks:
    - name: Print the value of global fact 'web_url'
      debug:
        msg: 'Web URL: {{web_url}}'
```
Note: to print a message, use debug module with {{}}
save..

run playbook:
```
ansible-playbook playbooks/print_global_fact.yaml
```
Note: all the hosts in my inventory file can access the global fact web_url. Best practice is to add global facts in a separate file. This way, you can keep the inventory file clean.

remove the global facts from the host’s inventory file:
```
nano hosts
```
then remove:
```
[all:vars]
web_url=https://learning.lumada.hitachivantara.com/
```
save..

create a new file all in the group_vars/ directory:
```
nano group_vars/all
```
add the global fact web_url, type in the following line in the group_vars/all file:
```
web_url: https://learning.lumada.hitachivantara.com/
```
save..

run playbook:
```
ansible-playbook playbooks/print_global_fact.yaml
```
Note: all the hosts in my inventory file can access the global fact web_url.

</br>

**Group Facts**  
open the host’s inventory file
```
nano hosts
```
add group facts/variables for that host group:
```
[web]
10.0.0.2

[web:vars]
domain_name=web.learning.lumada.hitachivantara.com/
database_backend=MySQL
```
create a new playbook print_group_facts.yaml in the playbooks/ directory:
```
nano playbooks/print_group_facts.yaml
```
add the following lines in your print_group_facts.yaml file:
```
- hosts: web
  user: ansadmin
  tasks:
    - name: Print group facts
      debug:
        msg: 'Domain Name: {{domain_name}} Database Backend: {{database_backend}}'
```
save..

run playbook:
```
ansible-playbook playbooks/print_group_fact.yaml
```
Note: he hosts in the web group can access the domain_name and database_backend group facts/variables.


clean up the inventory file and see how to add group facts/variables in a separate file.
remove the global facts from the host’s inventory file:
```
nano hosts
```
then remove:
```
[web:vars]
domain_name=web.learning.lumada.hitachivantara.com/
database_backend=MySQL
```
adding group variables for the web host group, create a new file web (same as the group name) in the group_vars directory:
```
nano group_vars/web
```
add the group facts domain_name and database_backend for the web host group, add the following lines in the group_vars/web file:
```
domain_name=web.learning.lumada.hitachivantara.com/
database_backend=PgSQL
```
save..

run playbook:
```
ansible-playbook playbooks/print_group_fact.yaml
```
Note: the hosts in the web group can access the domain_name and database_backend group facts/variables.

</br>

**Hosts Facts**  
open the host’s inventory file
```
nano hosts
```
add hosts facts/variables for that host:
```
[web]
10.0.0.2   domain_name=web1.learning.lumada.hitachivantara.com database_backend=MySQL
10.0.0.3   domain_name=web2.learning.lumada.hitachivantara.com database_backend=PgSQL
```
save..

run playbook:
```
ansible-playbook playbooks/print_group_fact.yaml
```
Note: The values are different for each host. You can add host facts/variables in a separate file, as with the global and group facts/variables.

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html

---