## <font color='red'>1.2 Configure Ansible</font>
Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 


In this lab we're going to:
* configure ansible controller
* configure ansible nodes

* configure PasswordAuthentication
* create SSH-keys

Node 3 & 4 are not required, so once configured..  suspend.

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
uncomment: PasswordAuthentication yes

restart service:
```
service sshd restart
```

---


#### <font color='red'>Ansible Node(s) Configuration</font>
* add ansadmin to nodes
* ensure it has root priviledges

* set PasswordAuthentication yes

</br>

**add ansadmin**  
ensure that you're logged with CentOS credentials.
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
Note: ignore bad password message..  :)

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
implement password authentication across all the nodes:
```
nano /etc/ssh/sshd_config
```
uncomment PasswordAuthentication yes

restart service:
```
service sshd restart
```

repeat workflow for all nodes, i.e 10.0.0.3 & 10.0.0.4

---

#### <font color='red'>Ansible Node(s) Configuration - Keys</font>

**ensure that you're logged with ansadmin credentials, on the Ansible Controller**  

you have a choice here..!  if you have loads of screens then keep the Lab Guide open in centos account.  If you need to download the Lab Guide to the ansadmin account then:
```
mkdir Course-Materials
cd Course-Materials
git clone --filter=blob:none --sparse https://github.com/hv-support/customer-training.git
cd customer-training
git sparse-checkout add  dst/ansible
cd dst
mv ansible ~/Course-Materials
cd ..
cd ..
rm -rf customer-training
```
in virtual Workspace 1, open a Terminal and enter:
```
code
```
select a color scheme and select Course-Materials/ansible folder...  

</br>

**generate ssh keys** 
next create keys:
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

copy key over to nodes:

```
ssh-copy-id 10.0.0.2
ssh-copy-id 10.0.0.3
ssh-copy-id 10.0.0.4
```

password: ansadmin123
only the public key is copied to the server. The private key should never be copied to another machine.

now check you can log in:
```
ssh 10.0.0.2
ssh 10.0.0.3
ssh 10.0.0.4
```
Note: passwordless authenticated connection.

</br>

if you wish to include localhost in your list of managed nodes:
remove authorized keys (only if present :)
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
ensure that you're logged with ansadmin credentials, on Ansible Controller.
change directory:
```
cd /etc/ansible
ls -al
```
edit hosts file:
```
sudo nano hosts
```
add the node IP:
```
[all]
10.0.0.2
10.0.0.3

[Group1]
10.0.0.2

[Group2]
10.0.0.3

```
check the inventory hosts file has been modified:
```
cat hosts | head -20
```
check connectivity:
```
ansible all -m ping
```
look for ping pong..  & check for python ..  should be 2.7 for the moment.

for a specfic node(s):
```
ansible 10.0.0.2 -m ping
ansible 10.0.0.2:10.0.0.3 -m ping
```
if you want to list your Nodes:
```
ansible group1 -m ping --list-hosts
```

---