## <font color='red'> 1.1 Installation & Configeration of Ansible</font>
Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 


In this lab we're going to:
* install ansible controller
* configure ansible controller
* configure ansible node

* configure PasswordAuthentication
* create SSH-keys

* change working directories

*disable HOST checking

---

#### <font color='red'>Pre-requisties</font> 
The following pre-requisties have to be installed for Ansible Controller on CentOS 7:
* Python 3.8+
* check SSH


**Python 3**  
check python version:
```
python --version
```
install python3 for ansible versions 3.5+..

switch to root:
```
sudo -i
```
make sure everything is up-to-date:
```
yum update -y
```
install python:
```
yum install -y python3
```
verify:
```
python3
```
exit:
```
Ctrl + d
```
install extra packages for enterprise linux (EPEL):
```
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```
Note: this may have  already be done with the upgrade to CentOS 7.9


**SSH**  
check if SSH is running:
```
ps aux | grep sshd
```
check if listening on port 22:
```
netstat -plant | grep :22
```
or check service:
```
sudo systemctl status sshd
```


**Ansible Nodes**  
ensure you have the following information:
* Ansible Node IP address - 10.0.0.2
* account credentials to SSH
* Python 2.7+ / 3.5+ is installed on Node

---

#### <font color='red'>Install Ansible Controller</font>
The next step is to install Ansible controller: 

ensure you're root:
```
sudo -i
```
install Ansible :
```
yum install ansible
```
verify ansible:
```
ansible --version
```
Note: the path to ansible.cfg  path to python & python version..  

browse ansible directory:
```
cd   /etc/ansible
ls -lrt
```
Note: these are the config files.
* roles
* hosts
* ansible.cfg

  > for further information: https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#

---

#### <font color='red'>Ansible Controller Configuration</font>
* add an ansadmin user to ansible controller
* ensure it has root priviledges

* set PasswordAuthentication yes
* switch to ansadmin user to generate ssh keys

* update inventory file with IPs on nodes

**add ansadmin**  
ensure you're root:
```
sudo su -
```
add ansadmin account:
```
useradd ansadmin
```
change password:
```
passwd ansadmin
new password: ansadmin123
```

**root priviledges**  
ensure ansadmin has root priviledges :
```
visudo
```
or
```
nano /etc/sudoers
```
add the following line to bottom of the file:
```
ansadmin  ALL=(ALL)     NOPASSWD: ALL
```
save..

**PasswordAuthentication**  
implement password authentication across the nodes:
```
nano /etc/ssh/sshd_config
```
uncomment PasswordAuthentication yes

restart service:
```
service sshd restart
```

**generate ssh keys**  
switch to ansadmin user:
```
sudo su ansadmin
cd
```
create keys:
```
ssh-keygen
```
just hit enter...
check ssh key:
```
ls -al
```
Note: now have a .ssh
change directory to .ssh
```
cd .ssh
ls -lrt
```
Note: you know have 2 keys: id_rsa (private) id_rsa.pub (public)
copy key over to node:
```
ssh-copy-id 10.0.0.2
```
password: ansadmin123

now check you can log in:
```
ssh 10.0.0.2
```
Note: passwordless authenticated connection.

**update Inventory file**  
change directory:
```
cd /etc/ansible
ls -al
```
edit hosts file:
```
sudo nano hosts
```
add the node IP: 10.0.0.2 to the top of the file
check its been added:
```
cat hosts | head -10
```
check connectivity:
```
ansible all -m ping
```
look for ping pong..  & check for python

for a specfic node(s):
```
ansible 10.0.0.2 -m ping
ansible 10.0.0.2:10.0.0.3 -m ping # just to illustrate
```

---

#### <font color='red'>Ansible Node Configuration</font>
* add ansadmin to node
* ensure it has root priviledges

* set PasswordAuthentication yes

**add ansadmin**  
SSH into ansible node from controller:
```
ssh 10.0.0.2
username: centos
password: centos
```
ensure you're root:
```
sudo su -
```
add ansadmin account:
```
useradd ansadmin
```
change password:
```
passwd ansadmin
new password: ansadmin123
```

**root priviledges**  
ensure ansadmin has root priviledges :
```
visudo
```
or
```
nano /etc/sudoers
```
add the following line to bottom of the file:
```
ansadmin  ALL=(ALL)     NOPASSWD: ALL
```
save..

**PasswordAuthentication**  
implement password authentication across the nodes:
```
nano /etc/ssh/sshd_config
```
uncomment PasswordAuthentication yes

restart service:
```
service sshd restart
```

---

#### <font color='red'>Ansible Directory</font>
* host file
* working directory

tree the ansible directory:
```
tree /etc/ansible
```
ping your host:
```
ansible all -m ping
```
Note: the inventory file be called anything..  its referenced in the ansible.cfg
```
cat /etc/ansible.cfg | head -20
```
Note: paths to directories

lets say there's a few of you using the Ansible controller and you need your own working directory for projects..
```
mkdir ansible_projects/demo
```
change to ansible_projects/demo directory:
```
cd ansible_projects/demo
```
now copy over ansible directory:
```
cp -rpP /etc/ansible/* .
```
Note: now flexibile on refencing different node IPs for different projects..
```
ls -lrt
```
take ownership of files:
```
chown -R ansadmin *
```
if you different 'inventory' files then you can run:
```
ansible all -m ping -i inventory # references renamed inventory file.
```

---

#### <font color='red'>Disable Host checking</font>
* temporary disable 
* uncomment host_key_checking in ansible.cfg

ensure you're in the ansible_projects/demo directory:
```
cd ansible_projects/demo
ls -lrt
```
to temporary disable HOST checking:
```
export ANSIBLE_HOST_KEY_CHECKING=False
```
edit ansible.cfg:
```
cat ansible.cfg | grep -i host_key_
nano ansible.cfg
```
uncomment: host_key_checking = False

check results:

ansible all -m ping

Note: doesnt ask whether you want to connect..