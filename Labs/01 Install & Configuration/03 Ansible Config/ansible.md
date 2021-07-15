## <font color='red'>1.3 Ansible Configuration</font>
Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 

In this lab we're going to:
* change working directories
* disable HOSTS checking

---

#### <font color='red'>Ansible Directories</font>
* working directory
* hosts file
* disable HOSTS checking


**working directory** 

lets say there's a few of you using the Ansible controller and you need your own working directory for projects..
```
cd
mkdir ansible_projects
cd ansible_projects
mkdir demo
/demo
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
cd
sudo chown -R ansadmin ansible_projects
```
if you've renamed - hosts - 'inventory' files then you can run:
```
ansible all -m ping -i inventory # references renamed inventory file.
```

---

</br>

**hosts**  
tree the ansible directory:
```
tree ansible_projects
```
so first lets modify the ~/ansible_projects/demo/hosts
```
cd ansible_projects/demo
```
edit hosts - inventory - file:
```
sudo nano hosts
```
edit the hosts file:
```
[all]
10.0.0.2
10.0.0.3
10.0.0.4

[group1]
10.0.0.2

[group2]
10.0.0.3

[group:children]
group1
group2
```
save..
Note: the inventory file be called anything..  its referenced in the ansible.cfg  
you can 'group' the nodes to you're required configuration.

edit the ansible_projects/demo/ansible.cfg:
```
nano ansible.cfg
```
modify path to inventory: 
```
inventory   = ./hosts
```
Note: paths to directories
save..

ping your nodes:
```
ansible all -m ping
```
ping your Group node(s):
```
ansible group2 -m ping
```
could also reference groups:
```
ansible group1:group2 -m ping
```
you can also use --list-hosts to check:
```
ansible --list-hosts group
```

  > for further information: https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html

---

</br>

#### <font color='red'>Disable Host checking</font>
This will help automate the process of logging authenticated users onto the nodes.  
* temporary disable HOST checking
  - uncomment host_key_checking in ansible.cfg

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