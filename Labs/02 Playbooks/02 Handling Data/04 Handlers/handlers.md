## <font color='red'> 2.3 Operators </font>


#### <font color='red'>Comparasion Operators</font>

create playbook:
```
nano comparasion_operators.yaml
```
add the following:
```
---
---
  - hosts: localhost
    gather_facts: false
    become: yes
    tasks:
      - name: Install httpd
        yum:
          name: httpd
          state: present
        notify:
          - start httpd
    handlers:
      - name: start httpd
        service:
          name: httpd
          state: started
```

---
