## <font color='red'> 3.1 Import</font>
You can include playbooks inside other playbooks by using the import_playbook directive. There are a number of reasons you might want to move playbooks into separate files when using Ansible:

* Refactoring - chop up unfeasibly large playbooks to make the whole thing easier to read, or reduce repetition (DRY) by putting common plays into a file that can be imported by multiple playbooks.
* Conditionally including playbooks - you may wish to have certain plays run conditionally based on some variable.

In this Lab we're going to cover:
* Imports

#### <font color='red'>Imports</font>
The following playbook simply install a webserver based on OS..
```
---
  - name: Simple play to install multiple pkgs
    hosts: web_servers
    gather_facts: true
    become: yes
    tasks:
      - name: Installing Webserver on RedHat family
        yum:
          name: httpd
          state: present
        when: ansible_os_family=="RedHat"
      - name: Installing Webserver on Debian family
        apt:
          name: apache2
          state: present
        when: ansible_os_family=="Debian"
      - name: Installing java on RedHat family
        yum:
          name: java-1.8.0-openjdk
          state: present
        when: ansible_os_family=="RedHat"
      - name: Installing java on Debian family
        apt:
          name: openjdk-8-jdk
          state: present
        when: ansible_os_family=="Debian"
```
So if the OS is RedHat then httpd with jave-1.8.0 gets installed..  and for Debian, apache2 and openJDK ..  using the relevent package managers: yum and apt.

To reduce the complexity of the installation, each task can be delcalred in its own reusable playbook..

For RedHat Webserver - install_webserver_redhat.yml:
```
---
 - name: Installing webserver on RedHat family
   yum:
    name: httpd
    state: present
```
and Java - install_java_redhat.yml 
```
---
 - name: Installing java on RedHat family
   yum:
    name: java-1.8.0-openjdk
    state: present
```

For Debian Weserver - install_webserver_debian.yml
```
---
 - name: Installing webserver on Debian family
   apt:
    name: apache2
    state: present
```
and Java - install_java_debian.yml
```
---
  - name: Installing java on Debian family
    apt:
      name: openjdk-8-jdk
      state: present
```
can now just import the tasks, based on a fact:
```
---
  - name: Simple play to install multiple pkgs
    hosts: web_servers
    gather_facts: true
    become: yes
    tasks:
      - import_tasks: install_webserver_redhat.yml
        when: ansible_os_family=="RedHat"
      - import_tasks: install_webserver_debian.yml
        when: ansible_os_family=="Debian"
      - include_tasks: install_Redhat.yml
        when: ansible_os_family=="RedHat"
      - include_tasks: install_java_debian.yml
        when: ansible_os_family=="Debian"
```



> for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html

---

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