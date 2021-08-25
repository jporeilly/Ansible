## <font color='red'>1.1 Installation of Ansible</font>
Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 


In this lab we're going to:
* Install ansible controller

#### Note: Yum doesn't support Python3
#### You are also going to find some minor differences between the videos and the Lab due to updates being released. Usually some patch or Firefox..!
---

#### <font color='red'>Pre-requisties</font> 
The following pre-requisties have to be installed and confifgured for Ansible on CentOS 7:
* Development Tools
* Python 3.5+
* Git 2.x
* Check SSH  

<font color='red'>These pre-requisite checks have to be completed on all Nodes.</font>

</br>

**Development Tools**  
not really required but useful:  
need to be root:
```
sudo -i
yum update -y
```
ensures that openssl is up-to-date:
```
yum groupinstall -y "Development Tools" && yum install gcc openssl-devel bzip2-devel libffi-devel -y
```


**Python 3**   
It is recommended to install Python3 to ensure that your tasks and commands are executed correctly.  However, there will be times when you need to switch bewteen the 2 versions due to using an old version of CentOS. For the majority of the course we will be using the pre-installed Python 2.75  

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
set as default:
```
alternatives --install /usr/bin/python python /usr/bin/python2 50
alternatives --install /usr/bin/python python /usr/bin/python3.6 60
alternatives --config python
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

**Git 2.x**  
Git version 2.30.1 has already been installed. There's no need to 
check Git Version:
```
git --version
```
to install the latest version:
```
sudo yum -y remove git
sudo yum -y install https://packages.endpoint.com/rhel/7/os/x86_64/endpoint-repo-1.9-1.x86_64.rpm
sudo yum install git
```

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
* Account credentials to SSH
  - user: centos  
  - password: centos  
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
Note: you will get a keyboard error if you have set Python 3.5+ as default. Switch back to Python 2 - see section 'Python 3' 
(alternatives --config python).1

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