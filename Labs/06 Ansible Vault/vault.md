## <font color='red'> 6.1 Ansible Vault </font>
Ansible Vault encrypts variables and files so you can protect sensitive content such as passwords or keys rather than leaving it visible as plaintext in playbooks or roles.  

To use Ansible Vault you need one or more passwords to encrypt and decrypt content. If you store your vault passwords in a third-party tool such as a secret manager, you need a script to access them. 

Use the passwords with the ansible-vault command-line tool to create and view encrypted variables, create encrypted files, encrypt existing files, or edit, re-key, or decrypt files. You can then place encrypted content under source control and share it more safely.

In this Lab we're going to cover:
* Vault
* Vault - playbook
---

#### <font color='red'>Encrypted File</font>

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
```
password: lumada  
text: they're coming to take me away..
:wq
```
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
there may be a requirement to encrypt a specific variable:
```
ansible-vault encrypt_string
```
Note: Ansible vault will prompt you for the password and later require you to confirm it. Next, type the string value that you want to encrypt. Finally, press ctrl + d
you can begin assigning the encrypted value in a playbook:
```
ansible-vault encrypt_string 'string' --name 'variable_name'
```

---

#### <font color='red'>Vault - Playbook</font>
In this Lab we're going to prompt for an API key..
* the API key is going to be held in an encrpted file
* pass the file as a regular variable into the prompt

on the ansible controller:
```
cd ansible_projects/demo/playbooks
```
create playbook:
```
nano vault_config.yml
```
add the following:
```
---
- hosts: 10.0.0.2
  gather_facts: false
  vars_prompt:
    - name: api_key
      prompt: Enter the API key
  tasks:
    - name: Ensure API key is present in config file
      ansible.builtin.lineinfile:
        path: /etc/app/configuration.ini
        line: "API_KEY={{ api_key }}"
```
save..

now for the configuration file:
```
cd ..
cd vault
sudo nano secrets_file.enc
sudo chown -R ansadmin secrets_file.enc
```
add the following:
```
api_key: SuperSecretPassword
```
lets encrypt:
```
ansible-vault encrypt secrets_file.enc
```
Note: enter a password.
check the file has been encrypted:
```
cat secrets_file.enc
```
before you execute the playbook, you will need to create the dest file, configuration.ini on Node 1 - 10.0.0.2:
```
cd /etc
sudo mkdir app
sudo chown -R ansadmin app
touch configuration.ini
```

run the playbook:
```
cd ..
cd playbooks
ansible-playbook vault_config.yml -e vault/secrets_file.enc --ask-vault-pass  
```
Note:  you will first be promted for the file password: lumada then for the API_key: SuperSecretPassword

Many organizations already have tools, such as HashiCorp Vault or Thycotic Secret Server. The Ansible community has written a number of custom modules for interacting with these types of systems.

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/vault.html

---