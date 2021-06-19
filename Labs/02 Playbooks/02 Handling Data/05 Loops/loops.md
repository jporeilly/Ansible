## <font color='red'> 2.5 Loops </font>


#### <font color='red'>No Loops</font>

create playbook:
```
nano without_loops.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: false
    become: yes
    tasks:
      - yum:
         name: gettext-devel
         state: present
      - yum:
         name: openssl-devel
         state: present
      - yum:
         name: perl-CPAN
         state: present
      - yum:
         name: perl-devel
         state: present
      - yum:
         name: zlib-devel
         state: present

```

---

#### <font color='red'>Loops</font>

create playbook:
```
nano with_loops.yaml
```
add the following:
```
---
  - hosts: localhost
    gather_facts: false
    become: yes
    tasks:
      - yum:
         name: "{{item}}"
         state: absent
        loop:
          - gettext-devel
          - openssl-devel
          - perl-CPAN
          - perl-devel
          - zlib-devel"
```

---