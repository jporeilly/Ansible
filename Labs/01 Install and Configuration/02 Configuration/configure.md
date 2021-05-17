## <font color='red'>1.2 Configure Ansible</font>
Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 


In this lab we're going to:
* configure ansible controller
* configure ansible node

* configure PasswordAuthentication
* create SSH-keys

---

#### <font color='red'>Ansible Controller Configuration</font>
* add an ansadmin user to ansible controller
* ensure it has root priviledges

* set PasswordAuthentication yes
* switch to ansadmin user to generate ssh keys

* update inventory file with IPs on nodes

</br>

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

</br>

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

</br>

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

</br>

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
ssh-copy-id 10.0.0.3
```
password: ansadmin123
Only the public key is copied to the server. The private key should never be copied to another machine.

now check you can log in:
```
ssh 10.0.0.2
ssh 10.0.0.3
```
Note: passwordless authenticated connection.

</br>

if you wish to include localhost in your list of managed nodes:

remove authorized keys:
```
cd  .ssh/
ls -l
rm -rf authorized_keys
```
connect to localhost:
```
ssh localhost
```
copy key
```
ssh-copy-id localhost
```

</br>

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
add the node IP: 10.0.0.2 & 10.0.0.3 to the top of the file
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
ansible 10.0.0.2:10.0.0.3 -m ping
```
if you want to list your Nodes:
```
ansible group3 -m ping --list-hosts
```
---

#### <font color='red'>Ansible Node Configuration</font>
* add ansadmin to node
* ensure it has root priviledges

* set PasswordAuthentication yes

</br>

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

</br>

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

</br>

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

repeat for all nodes

---