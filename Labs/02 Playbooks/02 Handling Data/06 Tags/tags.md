## <font color='red'> 2.6 Tags </font>
If you have a large playbook, it may be useful to run only specific parts of it instead of running the entire playbook. 

#### <font color='red'>Tags</font>

create playbook:
```
nano tags.yaml
```
add the following:
```
---
  - name: Play with 5 tasks
    hosts: localhost
    gather_facts: false
    tasks:
      - debug:
          msg: "This is a first task"
        tags:
          - first
          - common
          - always
      - debug:
          msg: "This is a second task"
        tags:
          - second
          - never
      - debug:
          msg: "This is a third task"
        tags:
          - third
          - common
          - never
      - debug:
          msg: "This is a fourth task"
        tags:
          - fourth
          - never
      - debug:
          msg: "This is a fifth task"
        tags:
          - fifth
          - never
```
save..
run the playbook:
```
ansible-playbook tags.yaml
```

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html

---