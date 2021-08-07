## <font color='red'> 3.2 Debugging </font>
We've already had a look at the debug module and seen how its to print variables or messages during playbook execution.

In this lab were going to cover:
* YAML lint
* Ansible lint
* Molecule

---

#### <font color='red'>YAML lint</font>

yamllint is a simple YAML lint tool.

install YAMLlint & Ansible-lint:
```
pip3 install yamllint ansible-lint molecule
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

  > for further info: https://yamllint.readthedocs.io/en/stable/quickstart.html

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
Again it should pass the yaml lint, but there should be suggestions for the playbook.

  > for further info: https://ansible-lint.readthedocs.io/en/latest/

---

#### <font color='red'>Molecule</font>
Everything up to this point is focused on static testing.  You cant really test against production, esp when modifying or adding functionality.  You could use shell scripts, however you will need a tool geared towards Ansible.  

Molecule is a lightweight Ansible-based tool to help the developement and testing of Ansuble playbooks, roles and collections.  

Molecule uses Galaxy under the hood to generate conventional role layouts. If you’ve ever worked with Ansible roles before, you’ll be right at home. If not, please review the Roles module to see what each folder is responsible for.



  > for further info: https://molecule.readthedocs.io/en/latest/getting-started.html
