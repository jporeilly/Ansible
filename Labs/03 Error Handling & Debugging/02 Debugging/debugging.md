## <font color='red'> 3.2 Debugging </font>
We've already had a look at the debug module and seen how its to print variables or messages during playbook execution.

In this lab were going to cover:
* YAML lint
* Ansible lint
* Molecule

---

#### <font color='red'>YAML lint</font>

yamllint is a simple YAML lint tool.

install YAMLlint:



create playbook:
```
nano yaml-lint.yaml
```
add the following:
```
  - hosts: localhost
    gather_facts: no
    tasks:
      - name: Register the output of the 'uptime' command.
        command: uptime
        register: system_uptime  # comment

      - name: Print the registered output of the 'uptime' command.
        debug: "ls /tmp"
         var: system_uptime.stout
```
save..
run the yamllint:
```
cd playbooks
yamllint .
./yaml-lint.yaml 
```
Note: the output helps pinpoint errors..

you can configure YAML lint, to allow for example 'yes' / 'no' values.

create a file in the same directory named .yamllint:
```
nano .yamllint
```
add the following:
```
---
extends: default

rules:
  truthy:
    allowed-values:
      - 'true'
      - 'false'
      - 'yes'
      - 'no'
```
if you've fixed the other errrors, then running yamllint again should be fine.

---

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

create playbook:
```
nano error_return_code.yaml
```
add the following:
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

or another way to fail the task.

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
ansible-playbook error_return_code.yaml
```

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_error_handling.html

---