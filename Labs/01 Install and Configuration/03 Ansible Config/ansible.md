## <font color='red'>3.1 Ansible Configuration</font>
Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 


In this lab we're going to:

* change working directories

* disable HOST checking

---

#### <font color='red'>Ansible Directories</font>
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
if you've renamed - hosts - 'inventory' files then you can run:
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