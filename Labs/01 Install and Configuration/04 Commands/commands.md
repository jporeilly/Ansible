## <font color='red'>1.4 Ansible Commands</font>
An Ansible ad-hoc command uses the /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes. Ad-hoc commands are quick and easy, but they are not reusable.  

In this lab we're going to:
* ad-hoc commands
* playbooks

---

#### <font color='red'>Ansible Ad-hoc Commands</font>
* modules / tasks
* node directories
* fork setting in ansible.cfg

* transfer a file


</br>

as you know there's a whole bunch of unix commands that perform a task:
```
uptime #server uptime
free -m #amount of free memory
```
uptime on ansible controller:
```
ansible 10.0.0.2 -m shell -a "uptime"
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

so where are the modules / tasks executed..

SSH into Ansible Node1 on 10.0.0.2:
```
ssh 10.0.0.2
```
change directory:
```
cd
ls -al
tree .ansible/
```
Note: tmp folder is where modules are copied to..  then cleaned..
If you want to keep remote file..

execute command again on ansible controller:
```
ANSIBLE_KEEP_REMOTE_FILES=1 ansible 10.0.0.2 -m shell "uptime"
```
back to Node 1 change directory:
```
cd
ls -al
tree .ansible/
```
Note: new python directory with the module..

---

#### <font color='red'>Execute commands in Parallel v Serial</font>
By default Ansible will execute the modules / tasks in parallel.  The number of parallel executions is determined by the Forks value in the ansible.cfg.

edit ansible.cfg:
```
sudo nano projects/demo/ansible.cfg
```
The default value: Fork=5. If you change to 1 then executes in sequentially according to hosts file.

---

#### <font color='red'>Execute commands in Parallel v Serial</font>
Transfer a file using ansible ad-hoc commands.