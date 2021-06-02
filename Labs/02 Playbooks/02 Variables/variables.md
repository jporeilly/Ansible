## <font color='red'> 2.2 Variables, Data Collections </font>
Ansible uses variables to manage differences between systems. With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. To represent the variations among those different systems, you can create variables with standard YAML syntax, including lists and dictionaries. You can define these variables in your playbooks, in your inventory, in re-usable files or roles, or at the command line. You can also create variables during a playbook run by registering the return value or values of a task as a new variable.

After you create variables, either by defining them in a file, passing them at the command line, or registering the return value or values of a task as a new variable, you can use those variables in module arguments, in conditional “when” statements, in templates, and in loops. The ansible-examples github repository contains many examples of using variables in Ansible.

  > For further information: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html

In this lab we're going to:
* custom variables
* data collections



---

#### <font color='red'>Custom Variables</font>
Variables are represented as a key : value pair. The varaible must start with a letter.

create a simple variable:
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
    string: "James OReilly""
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
