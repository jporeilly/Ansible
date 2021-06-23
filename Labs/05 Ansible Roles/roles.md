## <font color='red'> 5.1 Roles </font>
Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.

Galaxy is a hub for finding and sharing Ansible content.

Use Galaxy to jump-start your automation project with great content from the Ansible community. Galaxy provides pre-packaged units of work known to Ansible as Roles, and new in Galaxy 3.2, Collections.

Roles can be dropped into Ansible PlayBooks and immediately put to work. You’ll find roles for provisioning infrastructure, deploying applications, and all of the tasks you do everyday. The new Collection format provides a comprehensive package of automation that may include multiple playbooks, roles, modules, and plugins.

  > Galaxy Hub: https://galaxy.ansible.com/

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





create playbook:
```
nano error_spelling.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: false
    tasks:
      - command: "ls /homee"  # spelling mistake will generate error and stop playbook.
        register: home_out
        ignore_errors: no     # change the value. If "yes" then will ignore error and execute the other tasks.
      - debug: var=home_out
      - command: "ls /tmp"
        register: tmp_out
      - debug: var=tmp_out

save..
run the playbook:
```
ansible-playbook tags.yaml  

#### <font color='red'>Error Handling - fail on return codes</font>
create playbook:
```
nano error_nginx.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: fasle
    become: yes
    tasks:
      - name: starting nginx
        service:
          name: nginx
          state: started
        ignore_errors: no      # change the value. If "yes" then will ignore error and execute the other tasks.
      - name: starting httpd
        service:
          name: httpd
          state: started
```
save..
run the playbook:
```
ansible-playbook error_nginx.yaml
```
---
  - hosts: localhost
    gather_facts: false
    tasks:
     - command: "ls /home"
       register: out
       failed_when: out.rc==0  # purposely fail the task..
     - debug: var=out
``` 

another way to fail the task.

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