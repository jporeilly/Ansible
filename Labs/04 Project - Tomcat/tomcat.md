## <font color='red'> 4.1 Project - Tomcat </font>
Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.

#### <font color='red'>Tomcat</font>

create playbook:
```
nano tomcat.yaml
```
add the following:
```
---
  - name: Install and configure tomcat
    hosts: 10.0.0.2
    gather_facts: false
    vars:
      req_java: java-1.8.0-openjdk
      set_java: jre-1.8.0-openjdk
      req_tomcat_ver: 10.0.8
      tomcat_url: https://apache.mirrors.nublue.co.uk/tomcat/tomcat-{{req_tomcat_ver.split('.')[0]}}/v{{req_tomcat_ver}}/bin/apache-tomcat-{{req_tomcat_ver}}.tar.gz
      tomcat_port: 8090
    become: yes
    tasks:
      - name: Updating repos
        yum:
          name: "*"
          state: latest
      - name: Installing required java
        yum:
          name: "{{req_java}}"
          state: present
      - name: Setting default java
        alternatives:
          name: java
          link: /usr/bin/java
          path: /usr/lib/jvm/{{set_java}}/bin/java
      - name: Downloading required tomcat
        get_url:
          url: "{{tomcat_url}}"
          dest: /usr/local
      - name: Extracting downloaded tomcat
        unarchive:
          src: "/usr/local/apache-tomcat-{{req_tomcat_ver}}.tar.gz"
          dest: /usr/local
          remote_src: yes
      - name: Renaming tocmcat home
        command: mv /usr/local/apache-tomcat-{{req_tomcat_ver}} /usr/local/latest
      - name: Replacing default port with required port
        template:
          src: server.xml.j2
          dest: /usr/local/latest/conf/server.xml
      - name: Starting tomcat
        shell:  nohup /usr/local/latest/bin/startup.sh &
```
save in ansible_projects/demo/playbooks/tomcat.yaml

run the playbook:
```
ansible-playbook tomcat.yaml
```
Note: this approach can become complex and error prone.  Best practice is to install based on defined 'Roles', which you'll cover in the next section.

---