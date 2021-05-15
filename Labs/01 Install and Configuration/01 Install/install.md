## <font color='red'>1.1 Installation of Ansible</font>
Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 


In this lab we're going to:
* install ansible controller

---

#### <font color='red'>Pre-requisties</font> 
The following pre-requisties have to be installed for Ansible Controller on CentOS 7:
* Python 3.8+
* check SSH

</br>

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

</br>

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
</br>

**Ansible Nodes**  
ensure you have the following information:
* Ansible Node IP address - 10.0.0.2 & 10.0.0.3
* account credentials to SSH
* Python 2.7+ / 3.5+ is installed on Node(s)

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
Note: these are the configuration files.
* roles
* hosts
* ansible.cfg

  > for further information: https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#

clean up:
```
sudo yum remove ansible -y
```

---