## <font color='red'> 2.1 Variables, Data Collections </font>
Ansible uses variables to manage differences between systems. With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. To represent the variations among those different systems, you can create variables with standard YAML syntax, including lists and dictionaries. You can define these variables in your playbooks, in your inventory, in re-usable files or roles, or at the command line. You can also create variables during a playbook run by registering the return value or values of a task as a new variable.

After you create variables, either by defining them in a file, passing them at the command line, or registering the return value or values of a task as a new variable, you can use those variables in module arguments, in conditional “when” statements, in templates, and in loops. The ansible-examples github repository contains many examples of using variables in Ansible.

  > For further information: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html

In this lab we're going to:
* custom variables
* data collections
* registers
* prompts
* read from files
* inventory variables
* magic variables

---

#### <font color='red'>Custom Variables</font>
Variables are represented as a key : value pair. The varaible must start with a letter.

create playbook:
```
nano custom_variables.yaml
```
add the following:
```
---
 - hosts: 10.0.0.1
   vars:
    x: 23

   tasks:
   - debug: var=x
```
run the playbook:
```
ansible-playbook custom_variables.yaml
```


the playbook can also be formatted:
```
---
 - hosts: 10.0.0.1
   vars:
    x: 23

   tasks:
   - debug
      var: x
```
edit the custom_variables.yaml:
```
---
 - hosts: 10.0.0.1
   vars:
    x: 23
    float_number: 45.67
    string: "James OReilly"
    boolean: false
    
   gather_facts: false
   tasks:
   - debug:
      msg:
       - "The value of x: {{x}} and type: {{x|type_debug}}"
       - "THe value of float_number: {{float_number}} and type : {{float_number|type_debug}}"
       - "The value of string: {{string}} and type: {{string|type_debug}}"
       - "THe value of boolean: {{boolean}} and type: {{boolean|type_debug}}"
```
save..
run the playbook:
```
ansible-playbook custom_variables.yaml
```

---

#### <font color='red'>Data Collections</font>
Data Collections are used to store multiple data / values. The values are stored in a list or sequence [].

create playbook to hold a list of values:
```
nano data_collection.yaml
```
add the following:
```
---
 - hosts: localhost
   vars:
    x: 34
    #pakgs: ['vim','nano','httpd','nginx']
    pakgs:
    - 'vim'
    - 'nano'
    - 'httpd'
    - 'nginx'
    #web_servers: {'CentOS': 'yum', 'Ubuntu': 'apt'}
    web_servers:
     'CentOS': 'yum'
     'Ubuntu': 'apt'
   gather_facts: false
   tasks:
   #- debug: var=pakgs[0]
   #- debug: var=web_servers.keys()
   #- debug: var=web_servers['CentOS']
   #- debug: var=web_servers.get('Ubuntu')
```
Note: uncomment and comment some of the lines out so you can see how lists and sequences are resolved.  web_servers use a variable to select a value - a map or dictionary. You can also use an index to reference specific key:value pairs.

save..
run the playbook:
```
ansible-playbook data_collection.yaml
```

---

#### <font color='red'>Registers</font>
You can create variables from the output of an Ansible task with the task keyword register. You can use registered variables in any later tasks in your play.

create playbook to hold in a registry:
```
nano bash_version.yaml
```
add the following:
```
---
 - hosts: 10.0.0.1
   gather_facts: false
   tasks:
   - shell: "bash --version"
     register: bash_ver
   - set_fact:
      bash_version: "{{bash_ver.stdout.split('\n')[0].split()[3]}}"
   - debug: var=bash_version
```
Note: bash_version is retrieved set as a register and then set as a fact (variable) with the stdout argument. split \n outputs as separate lines.  We just need line [0] and split again for line [3] ..  just the version.

save..
run the playbook:
```
ansible-playbook bash_version.yaml
```

---


#### <font color='red'>Prompts</font>
If you want your playbook to prompt the user for certain input, add a ‘vars_prompt’ section. Prompting the user for variables lets you avoid recording sensitive data like passwords. In addition to security, prompts support flexibility. For example, if you use one playbook across multiple software releases, you could prompt for the particular release version.

create playbook with prompt variables:
```
nano prompts.yaml
```
add the following:
```
---
 - hosts: 10.0.0.1
   vars_prompt:
    - name: user_name
      prompt: Enter your user name 
      private: no
    - name: password
      prompt: Enter your password 
      private: yes
      unsafe: yes
   gather_facts: false
   tasks:
   - debug:
      msg: "The username is: {{user_name}} and password is: {{password}}" 
```
Note: If you need to accept special characters, use the unsafe option.

save..
run the playbook:
```
ansible-playbook prompts.yaml
```

---

#### <font color='red'>Read from files</font>
When it comes to automation, you might need to read ifferent types of data and files such as csv, txt, etc..

create a text file:
```
nano external_values.txt
```
add the following:
```
key_file: /etc/nginx/ssl/nginx.key
cert_file: /etc/nginx/ssl/nginx.crt
conf_file: /etc/nginx/sites-available/default
server_name: localhost
```
save..

create a playbook:
```
nano file_values.yaml
```
add the following:
```
---
 - hosts: localhost
   vars_files: external_values.txt
   gather_facts: false
   tasks:
   - debug:
     var=server_name
```

save..
run the playbook:
```
ansible-playbook file_values.yaml
```

---


#### <font color='red'>Inventory Variables</font>
You can set the scope of custom facts in Ansible.
* Global facts: These facts are accessible from every host in your inventory file.
* Group facts: These facts are only accessible from a specific set of hosts or a host group.
* Host facts: These facts are only accessible from a particular host.

</br>

**Global Facts**  
we're going to say allo to all our nodes..!
create our first playbook:
```
cd demo/playbooks
nano global_fact.yaml
```
copy the following lines:
```
- hosts: all
  vars:
    greeting: a global allo...!

  tasks:
    - name: Print the value of global fact 'greeting'
      debug:
        msg: '{{greeting}}'
```
Note: to print a message, use debug module with '{{  }}'
save..

run playbook:
```
ansible-playbook global_fact.yaml
```
Note: all the hosts in my inventory file can access the global fact greeting. Best practice is to add global facts in a separate file. 



</br>

**Group Facts**  
edit the hosts inventory file:
```
nano hosts
```
add group facts/variables for that host group:
```
[web]
10.0.0.2

[web:vars]
domain_name=web.learning.lumada.hitachivantara.com/
database_backend=MySQL
```
create a new playbook print_group_facts.yaml in the playbooks/ directory:
```
nano playbooks/print_group_facts.yaml
```
add the following lines in your print_group_facts.yaml file:
```
- hosts: web
  user: ansadmin
  tasks:
    - name: Print group facts
      debug:
        msg: 'Domain Name: {{domain_name}} Database Backend: {{database_backend}}'
```
save..

run playbook:
```
ansible-playbook playbooks/print_group_fact.yaml
```
Note: the hosts in the web group can access the domain_name and database_backend group facts/variables.


clean up the inventory file and see how to add group facts/variables in a separate file.
remove the global facts from the host’s inventory file:
```
nano hosts
```
then remove:
```
[web:vars]
domain_name=web.learning.lumada.hitachivantara.com/
database_backend=MySQL
```
adding group variables for the web host group, create a new file web (same as the group name) in the group_vars directory:
```
nano group_vars/web
```
add the group facts domain_name and database_backend for the web host group:
```
domain_name=web.learning.lumada.hitachivantara.com/
database_backend=PgSQL
```
save..

run playbook:
```
ansible-playbook playbooks/print_group_fact.yaml
```
Note: the hosts in the web group can access the domain_name and database_backend group facts/variables.

</br>

**Hosts Facts**  
open the host’s inventory file
```
nano hosts
```
add hosts facts/variables for that host:
```
[web]
10.0.0.2   domain_name=web1.learning.lumada.hitachivantara.com database_backend=MySQL
10.0.0.3   domain_name=web2.learning.lumada.hitachivantara.com database_backend=PgSQL
```
save..

run playbook:
```
ansible-playbook playbooks/print_group_fact.yaml
```
Note: The values are different for each host. You can add host facts/variables in a separate file, as with the global and group facts/variables.

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html

---

#### <font color='red'>Magic Variables</font>
You can access information about Ansible operations, including the python version being used, the hosts and groups in inventory, and the directories for playbooks and roles, using “magic” variables.

The most commonly used magic variables are hostvars, groups, group_names, and inventory_hostname. With hostvars, you can access variables defined for any host in the play, at any point in a playbook. You can access Ansible facts using the hostvars variable too, but only after you have gathered (or cached) facts.

If you want to configure your database server using the value of a ‘fact’ from another node, or the value of an inventory variable assigned to another node, you can use hostvars in a template or on an action line.

create a playbook:
```
nano hostvars.yaml
```
add the following:
```
---
- hosts: 10.0.0.1
  gather_facts: false
  tasks:
    - debug:
        var=inventory_hostname
        var=hostvars[inventory_hostname] # target server
```

save..
run the playbook:
```
ansible-playbook hostvars.yaml
```
Note: inventory_hostname returns all the variables for all the servers.

  > for further info: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html

---