## <font color='red'> 3.1 Error Handling </font>
When Ansible receives a non-zero return code from a command or a failure from a module, by default it stops executing on that host and continues on other hosts. However, in some circumstances you may want different behavior. Sometimes a non-zero return code indicates success. Sometimes you want a failure on one host to stop execution on all hosts. Ansible provides tools and settings to handle these situations and help you get the behavior, output, and reporting you want.

In this lab were going to cover:
* ignore_error variable
* return codes

---

#### <font color='red'>Error Handling</font>

create playbook:
```
nano ignore_errors.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: false
    tasks:
      - command: "ls /homee"  # spelling mistake will generate error and stop playbook.
        register: home_out
        ignore_errors: false     # change the value. If "true" then will ignore error and execute the other tasks.
      - debug: var=home_out
      - command: "ls /tmp"
        register: tmp_out
      - debug: var=tmp_out
```
save..
run the playbook:
```
ansible-playbook ignore_errors.yaml  
```
Note: change the ignore_errors value to: yes

---

#### <font color='red'>Error Handling - fail on return codes</font>

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