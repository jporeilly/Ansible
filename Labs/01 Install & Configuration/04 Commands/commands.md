## <font color='red'>1.4 Ansible Commands</font>
An Ansible ad-hoc command uses the /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes. Ad-hoc commands are quick and easy, but they are not reusable.  

In this lab we're going to:
* ad-hoc commands
* playbooks

---

#### <font color='red'>Ansible Ad-hoc Commands</font>
* modules / tasks
* node directories
* ansible.cfg - Fork

* transfer copy a file
* create / delete files & directories on Nodes
* root priviledges

</br>

as you know there's a whole bunch of unix commands that perform a task:
```
uptime #server uptime
free -m #amount of free memory
```
uptime on Node1:
```
ansible 10.0.0.2 -m shell -a "uptime"
```
to list all the ansible modules:
```
ansible-doc -l
q
```
to find a module:
```
ansible-doc -l | grep shell
```
check disk space on our Nodes:
```
ansible all -a "df -h"
```

  > ansible module information: https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html

so where are the modules / tasks executed..

SSH into Ansible Node1 on 10.0.0.2:
```
ssh 10.0.0.2
```
change directory:
```
cd .ansible
ls -al
```
Note: tmp folder is where modules are copied to..  then cleaned..
If you want to keep remote file..

execute command again on ansible controller:
```
cd ansible_projects/demo
ANSIBLE_KEEP_REMOTE_FILES=1 ansible 10.0.0.2 -m shell -a "uptime"
```
back to Node 1 change directory:
```
cd .ansible/tmp
ls -al
cd ansible-tmp-xxxxxx
ls -al
```
Note: new python directory with the module: AnsiballZ_command.py

---

#### <font color='red'>Execute commands in Parallel v Serial</font>
Ansible works by spinning off forks of itself and talking to many remote systems independently.
By default Ansible will execute the modules / tasks in parallel.  The number of parallel executions is determined by the Forks value in the ansible.cfg.

edit ansible.cfg:
```
sudo nano ansible_projects/demo/ansible.cfg
```
The default value: Fork=5. If you change to 1 then executes in sequentially according to hosts file.
you can also add as an argument:
```
ansible [Group] -a "/sbin/reboot" -f 12
```
Note: will overide default and reboot 12 [Group] servers at a time..

---

#### <font color='red'>Download / Copy file from Node to Controller</font>
Transfer a file using ansible ad-hoc commands. Copy uses SCP (Secure Copy Protocol) to copy files to multiple nodes in parallel.
</br>
Syntax: ansible [-i inventory file] <servers> -m ce/file/path  dest=/dest/location arguments"

On Node1:
```
cd ansible_projects/demo
mkdir source_files
cd source_files
touch transfer_file.txt
nano transfer_file.txt
# just add some stuff to the file - hello world ..!
cat transfer_file.txt
```

**copy**    
on the ansible controller:
ensure you're in the ansible_projects/demo directory:
```
ansible 10.0.0.2 -m copy -a "src=~/ansible_projects/demo/source_files/transfer_file.txt dest=~/ansible_assets/dest_files/"
```
Note: look at the response on the Ansible Controller for Node1:
check the dest_files directory on Node1:
```
cd ansible_assets/dest_files
cat transfer_file.txt
```

**fetch**    
on the ansible controller:  
ensure you're in the ansible_projects/demo directory:
```
ansible 10.0.0.2 -m fetch -a "src=~/ansible_assets/dest_files/transfer_file.txt dest=./dest_files/"
```
Notice that it replicates the Node directory structure under its IP:
```
tree demo
```
to flatten the directory run the command on the ansible controller:
```
ansible 10.0.0.2 -m fetch -a "src=~/ansible_assets/dest_files/transfer_file.txt dest=./dest_files flat=yes"
```
but..!!   what happens if I have the same filename on serveral servers (could also )..  then it will fail..  so you could use a variable based on inventory hostname.

on the ansible controller:
```
ansible 10.0.0.2 -m fetch -a "src=~/ansible_assets/dest_files/transfer_file.txt dest=./dest_files/{{inventory_hostname}}_transfer_file.txt flat=yes"
```
on the ansible controller:
```
tree dest_files
```
Note: now flattened and transfer.txt under the inventory hostname - 10.0.0.2

---

#### <font color='red'>Create / Delete files / directory on Nodes</font>
Managing files and directories. 

</br>

**create**  
Syntax: ansible [-i inventory file] <servers> -m -a file "path=/to/file/file.txt state=touch"

on the ansible controller:
```
ansible 10.0.0.2 -m file -a "path=~/ansible_assets/dest_files/hello.txt state=touch"
```
Note: look at the response..  
can also set the mode, owner, etc..  any of the arguments:
```
ansible 10.0.0.2 -m file -a "path=~/ansible_assets/source_files/new_hello.txt state=touch mode=777"
```
Note: look at the response.. mode changed

</br>

Syntax: ansible [-i inventory file] <servers> -m file -a "path=/directory/sub/ state=directory"

If you require root priviledges:

Syntax: ansible [-i inventory file] <servers> -m file -a "path=/to/file/file.txt state=touch" -b

</br>

**delete**  
Syntax: ansible [-i inventory file] <servers> -m file -a "path=/to /file/file.txt state=absent"

on the ansible controller:
```
ansible 10.0.0.2 -m file -a "path=~/ansible_assets/dest_files/hello.txt state=absent"
```
Note: look at the response..  check that hello.txt has been deleted from 10.0.0.2

  > for further information: https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html#

---

#### <font color='red'>Managing Packages</font>
The Ad-hoc commands are available for yum and apt.

Syntax: ansible [-i inventory file] <servers> -m [package_manager] -a "name=[package] state=present"

on the ansible controller:
```
ansible 10.0.0.2 -m yum -a "name=git state=present" -b
```
Note: Command checks if git package is installed or not, but does not update it.  You need to have root privildeges. If you wish to update: state=latest

check on Node1:
```
git --version
```
to remove git:
on the ansible controller:
```
ansible 10.0.0.2 -m yum -a "name=git state=absent" -b
```
check on Node1:
```
git --version
```
  > checkout yum package manager options: https://docs.ansible.com/ansible/2.3/yum_module.html  

  > checkout apt package manager options: https://docs.ansible.com/ansible/2.3/apt_module.html

  ---

#### <font color='red'>Command Module</font>
If you forget to include a module the default is command.

on the ansible controller:
```
ansible all -a "uptime"
```
If you use command, you cant use some variables and operators as the command is not executed through a shell.
on the ansible controller:
```
ansible 10.0.0.2 -m -a "ls > test.txt"
```
Note: it will fail..  but.. on the ansible controller:
```
ansible 10.0.0.2 -m shell -a "ls > test.txt"
```
check that test.txt has been created on 10.0.0.2

---