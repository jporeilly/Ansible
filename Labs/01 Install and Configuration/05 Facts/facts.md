## <font color='red'>1.4 Ansible Facts</font>
Ansible facts are the host-specific system data and properties to which you connect. A fact can be the IP address, BIOS information, a system's software information, and even hardware information. Ansible facts help the admin to manage the hosts based on their current condition rather than taking the actions directly without having any info about the system's health.


In this lab we're going to:
* default Facts
* custom Facts

---

#### <font color='red'>Ansible Default Facts</font>
You can gather Facts using the setup command.


Syntax: ansible [-i inventory file] <servers> -m setup -a "filter=[value]"

on the ansible controller:
```
ansible 10.0.0.2 -m setup
```
Note: returns all the facts about Node1..

to return Facts just on mounts:
```
ansible 10.0.0.2 -m setp -a "filter=ansible_mounts"
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
type in the following lines your ansible.cfg file:
```
[defaults]
inventory           = hosts
host_key_checking   = False
```
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

```

---