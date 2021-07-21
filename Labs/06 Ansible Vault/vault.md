## <font color='red'> 6.1 Ansible Vault </font>
Ansible Vault encrypts variables and files so you can protect sensitive content such as passwords or keys rather than leaving it visible as plaintext in playbooks or roles.  

To use Ansible Vault you need one or more passwords to encrypt and decrypt content. If you store your vault passwords in a third-party tool such as a secret manager, you need a script to access them. 

Use the passwords with the ansible-vault command-line tool to create and view encrypted variables, create encrypted files, encrypt existing files, or edit, re-key, or decrypt files. You can then place encrypted content under source control and share it more safely.

In this Lab we're going to cover:
* Vaults

---

#### <font color='red'>Create Encrypted File</font>

on the ansible controller:
```
cd ansible_projects/demo
mkdir vault
cd vault
```
create a file to encrypt:
```
ansible-vault create test-vault.yml
```
Note: it will ask you to enter a password twice, then you can enter some text..

password: lumada  
text: they're coming to take me away..

to encrypt an existing file:
```
ansible-vault encrypt test-vault.yml
```
to display the contents of encrypted files:
```
ansible-vault view test-vault.yml
```
Note: obviously you will need to enter the password.

if you want to add extra data or remove the data from the encrypted file:
```
ansible-vault edit test-vault.yml
```
to remove encryption:
```
ansible-vault decrypt test-vault.yml
```
to change the vault password of encrypted files:
```
ansible-vault rekey test-vault.yml
```

---

#### <font color='red'>Create Encrypted File</font>









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


       
save..
run the playbook:
```
ansible-playbook error_nginx.yaml
```

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/vault.html

---