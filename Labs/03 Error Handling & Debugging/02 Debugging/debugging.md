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
```

```
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

#### <font color='red'>Ansible lint</font>
In addition to linting structural YAMl issues, Ansible tasks and playbooks can be linted using ansible-lint.



create playbook:
```
nano ansible-lint.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: false
    connection: local

    tasks:
      - shell: uptime
        register: system_uptime

      - name: Print the registered output of the 'uptime' command.
        debug: 
         var: system_uptime.stout
```
save..
run the playbook:
```
yamllint .
./ansible-lint.yaml 
ansible-lint ansible-lint.yaml
```

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_error_handling.html

---