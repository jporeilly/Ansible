## <font color='red'>1.3 Ansible Configuration</font>
Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 


In this lab we're going to:
* change working directories

* disable HOSTS checking

---

#### <font color='red'>Ansible Directories</font>
* hosts file
* working directory

* disable HOSTS checking

</br>

**hosts**  
tree the ansible directory:
```
tree /etc/ansible
```
ping your nodes:
```
ansible all -m ping
```
Note: the inventory file be called anything..  its referenced in the ansible.cfg
```
cat /etc/ansible.cfg | head -20
```
Note: paths to directories
manually entering IPs or FQDNs can be a pain...  You can reference 'grouped' nodes..

edit hosts - inventory - file:
```
nano /etc/hosts
```
you can 'group' the nodes to you're required configuration:
```
[all]
10.0.0.2
10.0.0.3

[group1]
localhost
10.0.0.2

[group2]
10.0.0.3

[group3]
group1
group2
```
save..
ping your group node(s):
```
ansible group2 -m ping
```
could also reference groups:
```
ansible group1:group2 -m ping
```
can also group groups:
```
ansible group3 -m ping
```

---

</br>

#### <font color='red'>Ansible Directories - Projects</font>  

lets say there's a few of you using the Ansible controller and you need your own working directory for projects..
```
sudo mkdir ansible_projects/demo
```
change to ansible_projects/demo directory:
```
cd ansible_projects/demo
```
now copy over ansible directory:
```
sudo cp -rpP /etc/ansible/* .
```
Note: now flexibile on refencing different node IPs for different projects..
The projects could have different ansible.cfg files..  as there are several ansible.cfg files, there's a priority.
* ANSIBLE_CONFIG - env variable
* ./ansible.cfg - current directory
* ~./ansible.cfg - home directory
* /etc/ansible.cfg - default directory
```
ls -lrt
```
take ownership of files:
```
sudo chown -R ansadmin *
```
if you've renamed - hosts - 'inventory' files then you can run:
```
ansible all -m ping -i inventory # references renamed inventory file.
```

---

#### <font color='red'>Disable Host checking</font>
This will help automate the process of logging authenticated users onto the nodes.  
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
```
ansible all -m ping
```
Note: doesnt ask whether you want to connect..

---