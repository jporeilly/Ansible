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
you can also display a 'variable' value:
```
ansible all -m debug -a "var=inventory_hostname"
```
can also list groups:
```
ansible localhost -m debug -a "var=groups"
ansible localhost -m debug -a "var=groups.keys()" # returns just group name
```

---

#### <font color='red'>Create Ansible Custom Facts</font>
Its pretty easy to define your own custom facts. 

from the Ansible Controller SSH into Node 1:
```
ssh 10.0.0.2
```
On Node 1 - 10.0.0.2 create the following directory: 
```
sudo mkdir -p /etc/ansible/facts.d
```
create the following file:
```
cd facts.d
sudo nano custom_facts.fact
```
Note: Custom Facts file end with the extension .fact

add the following:
```
[general]
foo=bar
```
Note: this is just data. If you wish to execute a script then change the file permissions - chmod +x
save file..

test the custom fact:
```
ansible 10.0.0.2 -m setup -a "filter=ansible_local"
```
Note: you may need to take ownership of the facts.d folder..


**executable script**

if you want to try a script:
```
sudo nano date_time.fact
```
add the following:
```
#!/bin/bash
DATE='date'
echo "{\"date\" : \"${DATE}\"}"
```
save..

assign execute permissions:
```
chmod +x /etc/ansible/facts.d/date_time.fact
```
create a playbook on the Ansible Controller:
```
nano check_date.yml
```
add the following:
```
---
- hosts: 10.0.0.2
  tasks:
   - name: Get custom facts
     debug:
      msg: The custom fact is {{ansible_local.date_time}}
```
Note: the Fact file is appended to the ansible_local variable, which stores all custom facts.

run the playbook and observe Ansible retrieving information saved on the fact file:
```
ansible-playbook check_date.yml
```

---

you may wish to dynamically add the custom fact.  
```
---
- hosts: all
  become: true
  tasks:
   - name: Create fact directory
     file:
       path: /etc/ansible/facts.d/
       state: directory
   - name: Create a static custom fact foo
     copy:
       content: '"bar"'
       dest: /etc/ansible/facts.d/foo.fact
   - name: Create a dynamic custom fact foobar
     copy:
       dest: /etc/ansible/facts.d/foobar.fact
       mode: 0775
       content: |
         #!/usr/bin/python3
         import json
         def render_data(data):
            return json.dumps(data)
         arbitrary_data = {}
         arbitrary_data["foobar"] = []
         arbitrary_data["foobar"].append("foo")
         if True:
            arbitrary_data["foobar"].append("bar")
         print(render_data(arbitrary_data["foobar"]))
```

---


#### <font color='red'>Ansible Custom Facts</font>
You can set the scope of custom facts in Ansible.
* Global facts: These facts are accessible from every host in your inventory file.
* Group facts: These facts are only accessible from a specific set of hosts or a host group.
* Host facts: These facts are only accessible from a particular host.

in our ansible_projects directory:
```
cd ansible_projects/demo
mkdir -pv custom_facts/{playbooks,host_vars,group_vars}
```

</br>

**Global Facts**  
we're going to say allo to all our nodes..!
create our first playbook:
```
cd custom_facts/playbooks
nano print_global_fact.yaml
```
copy the following lines:
```
- hosts: all
  vars:
    greeting: allo...!

  tasks:
    - name: Print the value of global fact 'greeting'
      debug:
        msg: '{{greeting}}'
```
Note: to print a message, use debug module with '{{  }}'
save..

run playbook:
```
ansible-playbook print_global_fact.yaml
```
Note: all the hosts in my inventory file can access the global fact greeting. Best practice is to add global facts in a separate file. 

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
Note: the hosts in the web group can access the domain_name and database_backend group facts/variables.


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