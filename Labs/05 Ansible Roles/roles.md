## <font color='red'> 5.1 Roles </font>
Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.

Roles can be dropped into Ansible PlayBooks and immediately put to work. You’ll find roles for provisioning infrastructure, deploying applications, and all of the tasks you do everyday. The new Collection format provides a comprehensive package of automation that may include multiple playbooks, roles, modules, and plugins.

Galaxy is a hub for finding and sharing Ansible content.

Use Galaxy to jump-start your automation project with great content from the Ansible community. Galaxy provides pre-packaged units of work known to Ansible as Roles, and new in Galaxy 3.2, Collections.

In this Lab we're going to cover:
* Roles
* Galaxy
---  

#### <font color='red'>Create a Role</font>
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

  > for further info: https://galaxy.ansible.com/

---