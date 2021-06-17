## <font color='red'> 2.2 Strings & Numbers </font>


create playbook:
```
nano custom_variables.yaml
```
add the following:
```
---
 - name: Play for Arithmetic OPerators
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
       - "{{x}} - {{y}} = {{y-x}}"
       - "{{x}} * {{y}} = {{y*x}}"
       - "{{x}} / {{y}} = {{x/y}}"
       - "{{x}} % {{y}} = {{x%y}}"
```

#### <font color='red'>Arithmetic Operators</font>

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
       - "THe additon of {{x}} and {{y}} is {{x|int +y|int}}"
       - "The {{a}} - {{y}} = {{a-y|int}}"
```

```
---
 - name: Practice on Arithmetic Operators
   hosts: localhost
   gather_facts: false
   vars_files:
    - my_vars.yml
   tasks:
   - debug:
       msg:
       - "The value of x is: {{x}}"
       - "The value of y is: {{y}}"
```

---

#### <font color='red'>Methods</font>

```
---
  - hosts: localhost
    gather_facts: false
    vars:
      x: "ThiS IS abOut Ansible FilTERS AND mETHODS"
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