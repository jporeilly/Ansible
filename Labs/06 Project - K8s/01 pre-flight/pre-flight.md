## <font color='red'>6.1 Pre-flight K8s</font>
Here's the disclaimer..  this project is definately not intended for production, but to highlight the steps required to install and configure K8s on a 3 node cluster - 1 master node, 2 worker nodes. Before you can install K8s on the cluster a number of pre-flight checks / configurations ahve toi be completed.

**Supported OS**
* CentOS 7

**Ansible-K8s-Architecture**
This will setup a kubernetes cluster on Centos7 machines using ansible.
You need these machines:
1. Ansible controller - controller.example.com - 10.0.0.1 
2. Kubernetes Master  - m-k8s.example.com      - 10.0.0.2 
3. Kubernetes Node1   - n1-k8s.example.com     - 10.0.0.3 
4. Kubernetes Node2   - n2-k8s.example.com     - 10.0.0.4 

Check the IPs of your cluster. If different, you will then need to update the inventory - hosts file.


**Tasks in the Role**
This role contains tasks to:
- Install basic packages required
- Setup standard system requirements - Disable Swap, Modify sysctl, Disable SELinux
- Install and configure a container runtime of your Choice - cri-o, Docker, Containerd
- Install the Kubernetes packages - kubelet, kubeadm and kubectl
- Configure Firewalld on Kubernetes Master and Worker nodes

---



## How to use this role

- Clone the Project:

```
$ git clone https://github.com/jmutai/k8s-pre-bootstrap.git
```

- Update your inventory, e.g:

```
$ vim hosts
[k8s-nodes]
172.21.200.10
172.21.200.11
172.21.200.12
```

- Update variables in playbook file

```
$ vim k8s-prep.yml
---
- name: Setup Proxy
  hosts: k8s-nodes
  remote_user: ansadmin
  become: yes
  become_method: sudo
  #gather_facts: no
  vars:
    k8s_cni: calico                                      # calico, flannel
    container_runtime: docker                            # docker, cri-o, containerd
    configure_firewalld: true                            # true / false
    # Docker registry
    setup_proxy: false                                   # Set to true to configure proxy
    proxy_server: "proxy.example.com:8080"               # Proxy server address and port
    docker_proxy_exclude: "localhost,127.0.0.1"          # Adresses to exclude from proxy
  roles:
    - kubernetes-bootstrap
```

If you are using non root remote user, then set username and enable sudo:

```
become: yes
become_method: sudo
```

To enable proxy, set the value of `setup_proxy` to `true` and provide proxy details.

## Running Playbook

Once all values are updated, you can then run the playbook against your nodes.

**NOTE**: For firewall configuration to open relevant ports for master and worker nodes, a pattern in hostname is required.

Check file:

```
$ vim roles/kubernetes-bootstrap/tasks/configure_firewalld.yml
....
- name: Configure firewalld on master nodes
  firewalld:
    port: "{{ item }}/tcp"
    permanent: yes
    state: enabled
  with_items: '{{ k8s_master_ports }}'
  when: "'master' in ansible_hostname"

- name: Configure firewalld on worker nodes
  firewalld:
    port: "{{ item }}/tcp"
    permanent: yes
    state: enabled
  with_items: '{{ k8s_worker_ports }}'
  when: "'node' in ansible_hostname"
```

If your master nodes doesn't contain `master` and nodes doesn't have `node` as part of hostname, update the file to reflect your naming pattern. My nodes are named like below:

```
k8smaster01
k8snode01
k8snode02
k8snode03
```

Playbook executed as root user - with ssh key:

```
$ ansible-playbook -i hosts k8s-prep.yml
```

Playbook executed as root user - with password:

```
$ ansible-playbook -i hosts k8s-prep.yml --ask-pass
```

Playbook executed as sudo user - with password:

```
$ ansible-playbook -i hosts k8s-prep.yml --ask-pass --ask-become-pass
```

Playbook executed as sudo user - with ssh key and sudo password:

```
$ ansible-playbook -i hosts k8s-prep.yml --ask-become-pass
```

Playbook executed as sudo user - with ssh key and passwordless sudo:

```
$ ansible-playbook -i hosts k8s-prep.yml --ask-become-pass
```

Execution should be successful without errors:

```
TASK [kubernetes-bootstrap : Reload firewalld] *********************************************************************************************************
changed: [k8smaster01]
changed: [k8snode01]
changed: [k8snode02]

PLAY RECAP *********************************************************************************************************************************************
k8smaster01                : ok=23   changed=3    unreachable=0    failed=0    skipped=11   rescued=0    ignored=0
k8snode01                  : ok=23   changed=3    unreachable=0    failed=0    skipped=11   rescued=0    ignored=0
k8snode02                  : ok=23   changed=3    unreachable=0    failed=0    skipped=11   rescued=0    ignored=0
```
