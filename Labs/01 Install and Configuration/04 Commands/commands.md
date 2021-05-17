## <font color='red'>1.4 Ansible Commands</font>
An Ansible ad-hoc command uses the /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes. Ad-hoc commands are quick and easy, but they are not reusable.  


In this lab we're going to:
* ad-hoc commands
* playbooks

* disable HOSTS checking

---

#### <font color='red'>Ansible Ad-hoc Commands</font>
* hosts file
* working directory

* disable HOSTS checking

</br>

as you know there's a whole bunch of unix commands that perform a task:
```
uptime #server uptime
free -m #amount of free memory
```
uptime on ansible controller:
```
ansible 10.0.0.1 -m shell -a "uptime"
```
to list all the ansible modules:
```
ansible-doc -l
```
to find a module:
```
ansible-doc -l | grep shell
```

  > ansible module information: https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html

---