## <font color='red'> 2.5 Loops </font>
Ansible offers the loop, with_<lookup>, and until keywords to execute a task multiple times. Examples of commonly-used loops include changing ownership on several files and/or directories with the file module, creating multiple users with the user module, and repeating a polling step until a certain result is reached.

#### <font color='red'>No Loops</font>

create playbook:
```
nano without_loops.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: false
    become: yes
    tasks:
      - yum:
         name: gettext-devel
         state: present
      - yum:
         name: openssl-devel
         state: present
      - yum:
         name: perl-CPAN
         state: present
      - yum:
         name: perl-devel
         state: present
      - yum:
         name: zlib-devel
         state: present
```
save..
run the playbook:
```
ansible-playbook without_loops.yaml
```

---

#### <font color='red'>Loops</font>

create playbook:
```
nano with_loops.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: false
    become: yes
    tasks:
      - yum:
         name: "{{item}}"
         state: absent
        loop:
          - gettext-devel
          - openssl-devel
          - perl-CPAN
          - perl-devel
          - zlib-devel"
```
save..
run the playbook:
```
ansible-playbook with_loops.yaml
```

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html

---