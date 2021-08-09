## <font color='red'> 2.1 Ansible Playbooks </font>
Playbooks record and execute Ansible’s configuration, deployment, and orchestration functions. They can describe a policy you want your remote systems to enforce, or a set of steps in a general IT process.

If Ansible modules are the tools in your workshop, playbooks are your instruction manuals, and your inventory of hosts are your raw material.

At a basic level, playbooks can be used to manage configurations of and deployments to remote machines. At a more advanced level, they can sequence multi-tier rollouts involving rolling updates, and can delegate actions to other hosts, interacting with monitoring servers and load balancers along the way.

Playbooks are designed to be human-readable and are developed in a basic text language - YAML. There are multiple ways to organize playbooks and the files they include, and we’ll offer up some suggestions to get the most out of Ansible.

In this lab we're going to:
* create a simple playbook
* add ansible-playbook path
* syntax checking

  > Ansible Playbook examples: https://github.com/ansible/ansible-examples

---

#### <font color='red'> Ansible Playbooks </font>
Playbooks are expressed in YAML format with a minimum of syntax. A playbook is composed of one or more ‘plays’ in an ordered list. The terms ‘playbook’ and ‘play’ are sports analogies. Each play executes part of the overall goal of the playbook, running one or more tasks. Each task calls an Ansible module.

A playbook runs in order from top to bottom. Within each play, tasks also run in order from top to bottom. Playbooks with multiple ‘plays’ can orchestrate multi-machine deployments, running one play on your webservers, then another play on your database servers, then a third play on your network infrastructure, and so on. At a minimum, each play defines two things:
* the managed nodes to target, using a pattern
* at least one task to execute

create a playbook on the Ansible Controller:
```
mkdir ansible_projects/demo/playbooks
cd playbooks
nano playbook.yaml
```
copy the following:
```
---
- name: Simple Playbook
  hosts: 10.0.0.2
  become: false
  vars:
    - username: ansadmin
    - home: /home/ansadmin
  tasks:
    - name: print variables
      debug:
        msg: "Username: {{ username }}, Home dir: {{ home }}"
```
save..
```
Ctrl+O
return
Ctrl+X
```

**name**  
This tag specifies the name of the Ansible playbook. As in what this playbook will be doing. Any logical name can be given to the playbook.

**hosts**  
This tag specifies the lists of hosts or host group against which we want to run the task. The hosts field/tag is mandatory. It tells Ansible on which hosts to run the listed tasks. The tasks can be run on the same machine or on a remote machine. One can run the tasks on multiple machines and hence hosts tag can have a group of hosts’ entry as well.

**become**  
Ansible allows you to ‘become’ another user, different from the user that logged into the machine (remote user).

**vars**  
Vars tag lets you define the variables which you can use in your playbook. Usage is similar to variables in any programming language.

**tasks**  
All playbooks should contain tasks or a list of tasks to be executed. Tasks are a list of actions one needs to perform. A tasks field contains the name of the task. This works as the help text for the user. It is not mandatory but proves useful in debugging the playbook. Each task internally links to a piece of code called a module. A module that should be executed, and arguments that are required for the module you want to execute.

to run the playbook:
```
ansible-playbook playbook.yaml
```
can also run multiple tasks against different hosts (ensure you're running Python 2 on the Nodes otherwise switch to Python 3 and run dnf version):
```
nano wget.yaml
```
copy the following:
```
---
 - hosts: 10.0.0.2
   become: true
   name: install httpd

   tasks:
   - name: install httpd
     yum: name=httpd state=present 
 
 - hosts: 10.0.0.3
   become: true
   name: install wget

   tasks:
   - name: install wget
     yum: name=wget state=present
```
save..

to run the playbook:
```
ansible-playbook wget.yaml
```
lets clean up:
```
nano wget.yaml
```
copy the following:
```
---
 - hosts: 10.0.0.2
   become: true
   name: uninstall httpd

   tasks:
   - name: uninstall httpd
    yum: name=httpd state=absent 
 
 - hosts: 10.0.0.3
   become: true
   name: uninstall wget

   tasks:
   - name: uninstall wget
     yum: name=wget state=absent
```
save..

to run the playbook:
```
ansible-playbook wget.yaml
```

bit of a pain running command: ansible-playbook, so add command inside of playbook:
```
which ansible-playbook
```
Note: its something like.. /usr/bin/ansible-playbook
```
nano wget.yaml
```
copy the following:
```
#!/usr/bin/ansible-playbook
 - hosts: 10.0.0.2
   become: true
   name: install httpd

   tasks:
   name: install httpd
   yum: name=httpd state=latest
```
save..

set permissions:
```
chmod +x wget.yaml
```
</br>

**syntax-checker**  
check the yaml syntax:
```
./wget.yaml --syntax-check
```
Note: the error and fix.  
Solution: Its the task name..  needs to be - name and the yum to be correctly indented.

run again:
```
./wget.yaml --syntax-check
```
Note: if the syntax is correct then the output: playbook: ./wget.yaml

can test yaml with a dry-run:
```
./wget.yaml --check
```
if you need a verbose output:
```
./wget.yaml -v #or just increase number of -v's for detailed logs. You can also set verbosity level as a value.
```

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks.html

---