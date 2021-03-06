## <font color='red'> 4.1 Project - Tomcat </font>
Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.

#### <font color='red'>Tomcat</font>
Take a look at the Tomcat playbook.

create playbook:
```
nano tomcat.yaml
```
add the following:
```
---
  - name: Install and Configure Tomcat
    hosts: 10.0.0.3
    gather_facts: false
    vars:
      req_java: java-1.8.0-openjdk
      set_java: jre-1.8.0-openjdk
      req_tomcat_ver: 9.0.52
      tomcat_url: https://apache.mirrors.nublue.co.uk/tomcat/tomcat-{{req_tomcat_ver.split('.')[0]}}/v{{req_tomcat_ver}}/bin/apache-tomcat-{{req_tomcat_ver}}.tar.gz
    become: yes
    tasks:
      - name: Updating Repos
        yum:
          name: "*"
          state: latest
      - name: Installing required Java
        yum:
          name: "{{req_java}}"
          state: present
      - name: Setting default Java
        alternatives:
          name: java
          link: /usr/bin/java
          path: /usr/lib/jvm/{{set_java}}/bin/java
      - name: Downloading required Tomcat
        get_url:
          url: "{{tomcat_url}}"
          dest: /usr/local
      - name: Extracting downloaded Tomcat
        unarchive:
          src: "/usr/local/apache-tomcat-{{req_tomcat_ver}}.tar.gz"
          dest: /usr/local
          remote_src: yes
      - name: Moving Tomcat - Latest
        command: mv /usr/local/apache-tomcat-{{req_tomcat_ver}} /usr/local/latest
      - name: Starting Tomcat
        shell:  nohup /usr/local/latest/bin/startup.sh &
```
save in ansible_projects/demo/playbooks/tomcat.yaml

run the playbook:
```
ansible-playbook tomcat.yaml
```
Note: this approach can become complex and error prone.  Best practice is to install based on defined 'Roles', which you'll cover in the next section.
This installs Tomcat in the root..  so you'll need to log into:  
Username: Centos  
Pasword: centos  
not ideal  can you modify so that it gets installed under ansadmin account:  

  > test the installation: http://localhost:8080

---