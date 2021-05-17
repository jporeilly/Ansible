## <font color='red'>1.4 Ansible Facts</font>
Ansible facts are the host-specific system data and properties to which you connect. A fact can be the IP address, BIOS information, a system's software information, and even hardware information. Ansible facts help the admin to manage the hosts based on their current condition rather than taking the actions directly without having any info about the system's health.


In this lab we're going to:
* default Facts
* custom Facts

---

#### <font color='red'>Ansible Default Facts</font>
You can gather Facts using the setup command.


Syntax: ansible [-i inventory file] <servers> -m setup -a "filter=[value]"

on the ansible controller:
```
ansible 10.0.0.2 -m setup
```
Note: returns all the facts about Node1..

to return Facts just on mounts:
```
ansible 10.0.0.2 -m setp -a "filter=ansible_mounts"
```

---

#### <font color='red'>Ansible Custom Facts</font>


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
chown -R ansadmin *
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