## <font color='red'> 2.3 Operators </font>
Sometimes you want a task to run only when a change is made on a machine. For example, you may want to restart a service if a task updates the configuration of that service, but not if the configuration is unchanged. Ansible uses handlers to address this use case. Handlers are tasks that only run when notified. Each handler should have a globally unique name.



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
