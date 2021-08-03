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
can also list inventory groups:
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

from the Ansible Controller SSH into Node1:
```
ssh 10.0.0.2
```
create a script script to show date & time:
```
cd /etc/ansible/facts.d
sudo nano date_time.fact
```
add the following:
```
#!/bin/bash
DATE=`date`
echo "{\"date\" : \"${DATE}\"}"
```
save..

assign execute permissions:
```
chmod +x /etc/ansible/facts.d/date_time.fact
```

</br>

create a playbook on the Ansible Controller:
```
mkdir ansible_projects/demo/playbooks
cd playbooks
nano check_date.yml
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
   - name: Get Custom Date Fact
     debug:
      msg: The custom fact is {{ansible_local.date_time}}
```
Note: the Fact file is appended to the ansible_local variable, which stores all custom facts.

run the playbook and observe Ansible retrieving information saved on the fact file:
```
ansible-playbook check_date.yml
```

you may wish to dynamically add the custom fact.  
```
---
- hosts: 10.0.0.2
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
Note: a bit over engineer'd but you hopefully get the idea..!
Also you will need to switch to Python3..
There's one more arg to add..  around ownership, so I'll leave you to try that one..



here's another example that uses custom fcats to install a samba file server.

from the Ansible Controller SSH into Node1:
```
ssh 10.0.0.2
```
create a script script to show date & time:
```
cd /etc/ansible/facts.d
sudo nano samba.fact
```
add the following:
```
[localfacts]
pkgname = samba
srvc = smb
```
create a playbook on the Ansible Controller:
```
nano samba_custom_facts.yml
```
add the following:
```
---
- name: Install custom facts
  hosts: 10.0.0.2
  vars:
    remote_dir: /etc/ansible/facts.d
    facts_file: customfacts.fact
  tasks:
  - name: Create Facts Dir on Managed Hosts
    file:
      path: "{{ remote_dir }}"
      state: directory
      recurse: yes
  - name: Copy Contents to Facts file
    copy:
      src: "{{ facts_file }}"
      dest: "{{ remote_dir }}"

- name: Install Samba Server with Custom Facts
  hosts: 10.0.0.2
  tasks:
  - name: Install SMB
    package:
      name: "{{ ansible_local.customfacts.localfacts.pkgname }}"
      state: present
  - name: Start SMB Service
    service:
      name: "{{ ansible_local.customfacts.localfacts.srvc }}"
      state: started
      enabled: yes
```

save..
run the playbook and observe Ansible retrieving information saved on the fact file:
```
ansible-playbook samba_custom_facts.yml
```
verify custom facts:
```
ansible 10.0.0.2 -m setup -a "filter=ansible_local"
```
verify samba server’s service status:
``` 
ansible 10.0.0.2 -m command -a "systemctl status smb"
``` 


---