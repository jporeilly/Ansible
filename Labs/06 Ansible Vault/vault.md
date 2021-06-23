## <font color='red'> 6.1 Ansible Vault </font>
Ansible Vault encrypts variables and files so you can protect sensitive content such as passwords or keys rather than leaving it visible as plaintext in playbooks or roles.  

To use Ansible Vault you need one or more passwords to encrypt and decrypt content. If you store your vault passwords in a third-party tool such as a secret manager, you need a script to access them. 

Use the passwords with the ansible-vault command-line tool to create and view encrypted variables, create encrypted files, encrypt existing files, or edit, re-key, or decrypt files. You can then place encrypted content under source control and share it more safely.

  > Ansible Vault: https://docs.ansible.com/ansible/latest/user_guide/vault.html#vault

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