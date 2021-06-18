## <font color='red'> 2.3 Operators </font>


#### <font color='red'>Comparasion Operators</font>

create playbook:
```
nano comparasion_operators.yaml
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

---

#### <font color='red'>Test Operators</font>

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
      my_path: '/home/ansadmin/my_ansible_nprod/member_ship_op.yml'
      my_link_path: '/home/ansadmin/my_ansible_nprod/operators.yml'
    tasks:
      - debug:
          msg:
            - "x is defined:   {{ x is defined }}"
            - "y is defined:   {{ y is defined }}"
            - "z is undefined: {{ z is undefined }}"
            - "my_name is lower: {{my_name is lower}}"
            - "my_name is upper: {{my_name is upper}}"
            - "my_name is string: {{my_name is string}}"
            - "x is divisibleby 2: {{x is divisibleby 2}}"
            - "x is even: {{ x is even }}"
            - "x is odd: {{ x is odd }}"
            - "x is number: {{ x is number}}"
            - "The given path is: {{my_path}}"
            - "my_path is file:   {{my_path is file}}"
            - "my_path is directory: {{my_path is directory}}"
            - "my_path is exists:   {{my_path is exists}}"
            - "my_link_path is link  {{my_link_path is link }}"
```

---