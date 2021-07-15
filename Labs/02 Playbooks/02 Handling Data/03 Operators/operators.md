## <font color='red'> 2.3 Operators </font>
Programming languages typically support a set of operators: constructs which behave generally like functions, but which differ syntactically or semantically from usual functions. Common simple examples include arithmetic (addition with +), comparison (with >), and logical operations (such as AND or &&). More involved examples include assignment (usually = or :=), field access in a record or object (usually .), and the scope resolution operator (often ::). Languages usually define a set of built-in operators, and in some cases allow users to add new meanings to existing operators or even define completely new operators.

As already referenced in the variables section, Ansible uses Jinja2 templating to enable dynamic expressions and access to variables. Ansible supports all the operators provided by Jinja2 like math, comparison, logical and other special Jinja2 operators.

In these Labs we're going to cover:
* Comparison Operators
* Membership Operators
* Test Operators
* Logical Operators
* Special Operators

---

#### <font color='red'>Comparison Operators</font>
Comparison operators are used to compare between two values.

create playbook:
```
nano comparison_operators.yaml
```
add the following:
```
---
  - name: Working with comparison operators
    hosts: localhost
    gather_facts: false
    vars:
      x: 6
      y: 10
      p: 'hi'
      q: 'bye'
      r: 'bye'
    tasks:
      - debug:
          msg:
            - "THe value of x is: {{x}} and The value of y is: {{y}}"
            - "x == y: {{ x == y }}"
            - "x != y: {{ x != y }}"
            - "x < y : {{ x < y}}"
            - "x > y : {{x > y}}"
            - "x <= y: {{ x <= y }}"
            - "x >= y: {{ x >= y }}"
            - "Below are for strings:"
            - "p= {{p}}  q={{q}} r={{r}}"
            - "p == q: {{ p == q }}"
            - "p != q: {{ p != q }}"
            - "q == r: {{ q == r }}"
```
save..
run the playbook:
```
ansible-playbook comparison_operators.yaml
```

---

#### <font color='red'>Membership Operators</font>

create playbook:
```
nano membership_operators.yaml
```
add the following:
```
---
  - name: This is about membership operators
    hosts: localhost
    gather_facts: false
    vars:
      x: [3, 4, 5]
      y: 5
    tasks:
      - debug:
          msg:
            - "The list or sequence x is: {{x}} and y value is: {{y}}"
            - "y  in x: {{ y in x}}"
            - "10 in x: {{ 10 in x }}"
            - "20 not in x: {{ 20 not in x }}"
            - " y not x:  {{ y not in x }}"
```
save..
run the playbook:
```
ansible-playbook membership_operators.yaml
```

---

#### <font color='red'>Test Operators</font>
Tests in Jinja are a way of evaluating template expressions and returning True or False.

create playbook:
```
nano membership_operators.yaml
```
add the following:
```
---
  - name: This is about test operators
    hosts: localhost
    gather_facts: false
    vars:
      x: 40
      my_name: 'ansible'
      my_path: '/home/ansadmin/ansible_projects/demo/playbooks/membership_operators.yml'
      my_link_path: '/home/ansadmin/ansible_projects/demo/playbooks/operators.yml'
    tasks:
      - debug:
          msg:
            - "x is defined:   {{ x is defined }}"
            - "y is defined:   {{ y is defined }}"
            - "z is undefined: {{ z is undefined }}"
            - "my_name is lower: {{my_name is lower}}"
            - "my_name is upper: {{my_name is upper}}"
            - "my_name is string: {{my_name is string}}"
            - "x is even: {{ x is even }}"
            - "x is odd: {{ x is odd }}"
            - "x is number: {{ x is number}}"
            - "The given path is: {{my_path}}"
            - "my_path is file:   {{my_path is file}}"
            - "my_path is directory: {{my_path is directory}}"
            - "my_path is exists:   {{my_path is exists}}"
            - "my_link_path is link  {{my_link_path is link }}"
```
save..
run the playbook:
```
ansible-playbook test_operators.yaml
```

---

#### <font color='red'>Logical Operators</font>
Logical operators are used to perform logical operations like AND, OR etc.

create playbook:
```
nano logical_operators.yaml
```
add the following:
```
---
- hosts: localhost
  gather_facts: false
  tasks:
  - name: Logical Operators
    vars:
      a: 12
      b: 3
      c: 5
    debug:
      msg: "{{ a }} is > than {{ b }} and {{ a }} is > than {{ c }}"
    when: a > b and a > c
```
save..
run the playbook:
```
ansible-playbook logical_operators.yaml
```

---

#### <font color='red'>Special Operators</font>
Jinja2 provides a set of special operators like filter to perform special operations.

<b> in </b> : Perform a sequence / mapping containment test. Returns true if the left operand is contained in the right.  
<b> {{ 1 in [1, 2, 3] }} </b>would, for example, return true.  
<b> is </b>: Performs a test.  
<b> |</b> : Applies a filter.  
<b> ~ </b>: Converts all operands into strings and concatenates them.  
<b> {{ "Hello " ~ name ~ "!" }} </b>would return (assuming name is set to 'John') Hello John!.  
<b>() </b> : Call a callable: {{ post.render() }}. Inside of the parentheses you can use positional arguments and
     keyword arguments like in Python: {{ post.render(user, full=true) }}.  
<b> . / [ ] </b>: Get an attribute of an object.  

create playbook:
```
nano special_operators.yaml
```
add the following:
```
---
- hosts: localhost
  gather_facts: false
  tasks:
  - name: Special Operators
    vars:
      a: ['bob','deb','alex']
      b: 'bob'
    debug:
      msg: "{{ a ~ ' contains ' ~ b }}"
    when: b in a
```
save..
run the playbook:
```
ansible-playbook special_operators.yaml
```

  > for further info: https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html

---