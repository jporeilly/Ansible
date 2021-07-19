## <font color='red'> 3.2 Includes </font>
If you have a large playbook, it may be useful to run only specific parts of it instead of running the entire playbook. 


#### <font color='red'>Includes</font>

---
  - name: Simple play to install multiple pkgs
    hosts: web_servers
    gather_facts: true
    become: yes
    tasks:
      - include_tasks: install_webserver_{{ansible_os_family}}.yml
      - include_tasks: install_java_{{ansible_os_family}}.yml





  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html

---
```

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html

---