## <font color='red'> 5.1 Roles </font>
Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.

Galaxy is a hub for finding and sharing Ansible content.

Use Galaxy to jump-start your automation project with great content from the Ansible community. Galaxy provides pre-packaged units of work known to Ansible as Roles, and new in Galaxy 3.2, Collections.

Roles can be dropped into Ansible PlayBooks and immediately put to work. You’ll find roles for provisioning infrastructure, deploying applications, and all of the tasks you do everyday. The new Collection format provides a comprehensive package of automation that may include multiple playbooks, roles, modules, and plugins.

  > Galaxy Hub: https://galaxy.ansible.com/

In this Lab we're going to cover:


---  

#### <font color='red'>Create a Role</font>

on the ansible controller:
```
cd ansible_projects/demo
mkdir roles
```
create a role:
```
ansible-galaxy init 
```



```
---
  - hosts: localhost
    gather_facts: false
    tasks:
     - command: "ls /home"
       register: out
     - fail:
        msg: "Failed because rc is 0"
        when: out.rc==0
 ```       
save..
run the playbook:
```
ansible-playbook error_nginx.yaml
```

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_error_handling.html

---