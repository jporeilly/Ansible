## <font color='red'> 5.1 Roles & Galaxy </font>
Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.

Roles can be dropped into Ansible PlayBooks and immediately put to work. You’ll find roles for provisioning infrastructure, deploying applications, and all of the tasks you do everyday. The new Collection format provides a comprehensive package of automation that may include multiple playbooks, roles, modules, and plugins.

Galaxy is a hub for finding and sharing Ansible content.

Use Galaxy to jump-start your automation project with great content from the Ansible community. Galaxy provides pre-packaged units of work known to Ansible as Roles, and new in Galaxy 3.2, Collections.

In this Lab we're going to cover:
* Roles
* Galaxy

---  

#### <font color='red'>Tomcat Role</font>
An Ansible role has a defined directory structure with eight main standard directories. You must include at least one of these directories in each role. You can omit any directories the role does not use.   

on the ansible controller:
```
cd ansible_projects/demo
mkdir roles
```
create a role:
```
ansible-galaxy init tomcat
```
Note: By default Ansible will look in each directory within a role for a main.yml file for relevant content (also main.yaml and main):
lets take a look at the structure:
```
tree roles
```
- defaults: contains default variables for the role. Variables in default have the lowest priority so they are easy to override.
- vars: contains variables for the role. Variables in vars have higher priority than variables in defaults directory.
- tasks: contains the main list of steps to be executed by the role.
- files: contains files which we want to be copied to the remote host. We don’t need to specify a path of resources stored in this directory.
- templates: contains file template which supports modifications from the role. We use the Jinja2 templating language for creating templates.
- meta: contains metadata of role like an author, support platforms, dependencies.
- handlers: contains handlers which can be invoked by “notify” directives and are associated with service.

So lets set the scope..   we want to install Tomcat on CentOS, RedHat Debian and Ubuntu.

  > browse to: https://github.com/hv-support/customer-training/tree/master/dst/ansible-tomcat

-----------------------------------

## <font color='red'>Role Info</font>
> Ansible Role to Install Tomcat 9 on CentOS, Fedora, Debian and Ubuntu Linux.

#### <font color='red'>Tested on the following operating systems</font>
- CentOS 8
- CentOS 7
- Fedora 31
- Ubuntu 18.04
- Debian 10

#### <font color='red'> Tasks in the Tomcat Role </font>
This role contains tasks to:
- Install basic packages required
- Install Java
- Add tomcat user and group
- Download tomcat and install - configure systemd
- Configure firewall

#### <font color='red'> Instructions </font>
- Clone the Project:

```
cd tomcat-ansible
git clone https://github.com/hv-spport/customer-training.git
cd .. (back up to roles)
sudo chown -R ansadmin ansible-tomcat
```
Note: important to take ownership otherwise the 'tomcat' group will have the incorrect inherited permissions.


- Update your inventory, e.g:
```
nano hosts
[tomcat-nodes]
10.0.0.2       # Node1 IP
```

- Update variables in playbook file - Set Tomcat version, remote user and Tomcat UI access credentials
```
nano tomcat-setup.yml
- name: Tomcat deployment playbook
  hosts: tomcat-nodes       # Inventory hosts group / server to act on
  become: yes               # If to escalate privilege
  become_method: sudo       # Set become method
  remote_user: ansadmin     # Update username for remote server
  vars:
    tomcat_ver: 9.0.30                          # Tomcat version to install
    ui_manager_user: manager                    # User who can access the UI manager section only
    ui_manager_pass: Str0ngManagerP@ssw3rd      # UI manager user password
    ui_admin_username: admin                    # User who can access bpth manager and admin UI sections
    ui_admin_pass: Str0ngAdminP@ssw3rd          # UI admin password
  roles:
    - tomcat
```
If you are using non root remote user, then set username and enable sudo:
```
become: yes
become_method: sudo
```

## Running Playbook
Once all values are updated, you can then run the playbook against your nodes.
Playbook executed as <ansadmin> user - with ssh key:
```
ansible-playbook -i hosts tomcat-setup.yml
```
Playbook executed as ansadmin user - with password:
```
ansible-playbook -i hosts tomcat-setup.yml --ask-pass
```
Playbook executed as sudo user - with password:
```
ansible-playbook -i hosts tomcat-setup.yml --ask-pass --ask-become-pass
```
Playbook executed as sudo user - with ssh key and sudo password:
```
ansible-playbook -i hosts tomcat-setup.yml --ask-become-pass
```
Playbook executed as sudo user - with ssh key and passwordless sudo:
```
ansible-playbook -i hosts tomcat-setup.yml --ask-become-pass
```
Execution should be successful without errors :)

-------------------------------


  > to check the installation: http://localhost:8080  obviously on Node 1..!

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html


---

#### <font color='red'> Galaxy </font>
The idea behind Galaxy is to give greater visibility to one of Ansible’s most exciting features: reusable Roles for server configuration or application installation. Many people share roles on Ansible Galaxy.

Ansible roles are consists of many playbooks which are way to group multiple tasks together into one container to do the automation in very effective manner with clean directory structures.


> for further info: https://galaxy.ansible.com/