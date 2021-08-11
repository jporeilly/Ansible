## <font color='red'> 2.2 Strings & Numbers </font>
You can use arithmetic calculations in Ansible using the Jinja syntax. This is helpful in many situations where you have stored the output of an operation, and you need to manipulate that value. All usual operation like addition, subtraction, multiplication, division, and modulo are possible.

In this lab we're going to cover:
* Numbers
* Arithmetic Operators
* Methods & Filters

---

#### <font color='red'>Arithmetic Operators</font>
create playbook:
```
nano strings_numbers.yaml
```
add the following:
```
---
 - name: Play for Arithmetic Operators
   hosts: localhost
   gather_facts: false
   vars:
    x: 56
    y: 34
   tasks:
   - name: Displaying values
     debug:
       msg:
       - "The value of x is: {{x}}"
       - "The value of y is: {{y}}"
       - "{{x}} + {{y}} = {{x+y}}"
       - "{{x}} - {{y}} = {{x-y}}"
       - "{{x}} * {{y}} = {{x*y}}"
       - "{{x}} / {{y}} = {{x/y}}"
       - "{{x}} % {{y}} = {{x%y}}"
```
save..
run the playbook:
```
ansible-playbook strings_numbers.yaml
```

create playbook:
```
nano arithmetic.yaml
```
add the following:
```
---
 - name: Practice on Arithmetic Operators
   hosts: localhost
   gather_facts: false
   vars:
    x: 45
    y: "{{x+5}}"
   tasks:
   - debug:
       msg:
       - "The value of x is: {{x}}"
       - "The value of y is: {{y}}"
```
save..
run the playbook:
```
ansible-playbook arithmetic.yaml
```

create playbook:
```
nano practice_arithmetic.yaml
```
add the following:
```
---
 - name: Practice on Arithmetic Operators
   hosts: localhost
   gather_facts: false
   vars_prompt:
    - name: x
      prompt: Please enter x value
      private: no
    - name: y
      prompt: Enter y value
      private: no
   vars:
     a: 56
   tasks:
   - debug:
       msg:
       - "The value of x is: {{x}}"
       - "The value of y is: {{y}}"
       - "The additon of {{x}} and {{y}} is {{x|int +y|int}}"
       - "The {{a}} - {{y}} = {{a-y|int}}"
```
save..
run the playbook:
```
ansible-playbook practice_arithmetic.yaml
```

create playbook:
```
nano file_arithmetic.yaml
```
add the following:
```
---
 - name: Practice on Arithmetic Operators
   hosts: localhost
   gather_facts: false
   vars_files:
    - arithmetic.yaml
   tasks:
   - debug:
       msg:
       - "The value of x is: {{x}}"
       - "The value of y is: {{y}}"
```
save..
run the playbook:
```
ansible-playbook file_arithmetic.yaml
```

---

#### <font color='red'>Methods and Filters</font>
Filters let you transform JSON data into YAML data, split a URL to extract the hostname, get the SHA1 hash of a string, add or multiply integers, and much more. You can use the Ansible-specific filters documented here to manipulate your data, or use any of the standard filters shipped with Jinja2.

create playbook:
```
nano filters_arithmetic.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: false
    vars:
      x: "Ansible Filters and Methods"
      y: "56"
      z: [4,5,6,38,0]

    tasks:
      - debug:
          msg:
            - "{{x|lower}}"
            - "{{x|upper}}"
            - "{{x|title}}"
            - "{{x.lower()}}"
            - "{{x.upper()}}"
            - "{{y|int}}"
            - "The max from z is: {{z|max}}"
            - "THe min from z is: {{z|min}}"
            - "{{x.split()}}"
```
save..
run the playbook:
```
ansible-playbook filters_arithmetic.yaml
```

---