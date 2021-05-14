## <font color='red'> 1.1 Installation of Ansible </font>
Kustomize lets you lets you create an entire Kubernetes application out of individual pieces — without touching the YAML configuration filesfor the individual components.  For example, you can combine pieces from different sources, keep your customizations — or kustomizations, as the case may be — in source control, and create overlays for specific situations. 


And it is part of Kubernetes 1.14 or later. Kustomize enables you to do that by creating a file that ties everything together, or optionally includes “overrides” for individual parameters.

In this lab we're going to:
* install minikube
* check kustomize
* deploy nginx app with kubectl
* deploy app with kustomize

---

#### <font color='red'>Pre-requisties</font> 
The following pre-requisties have to be installed for Ansible Controller on CentOS 7:
* Python 3.8+
* check SSH


**Python 3**
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

ps aux | grep sshd

check if listening on port 22:

netstat -plant | grep :22

---


#### <font color='red'>Install Ansible Controller</font>
The next step is to create Kubernetes cluster: 
* install minikube
* check kustomize
* deploy nginx app with kubectl

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

ensure you're root:
```
sudo su -
```
create ansadmin user:
```
useradd ansadmin
```
change password:
```
passwd ansadmin
new password: ansadmin123
```

---

#### <font color='red'>Ansible Node Configuration</font>
* add ansadmin to node
* ensure it has root
