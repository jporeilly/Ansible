## <font color='red'>6.1 Pre-flight K8s</font>
Here's the disclaimer..  this project is definately not intended for production, but to highlight the steps required to install and configure K8s on a 3 node cluster - 1 master node, 2 worker nodes. Before you can install K8s on the cluster a number of pre-flight checks / configurations have to be completed.

**Supported OS**
* CentOS 7

**Ansible-K8s-Architecture**
This will setup a kubernetes cluster on Centos7 machines using ansible.
You need these machines:
1. Ansible controller - controller.example.com    - 10.0.0.1 
2. Kubernetes Master  - kubernetes.master         - 10.0.0.2 
3. Kubernetes Node1   - kubernetes.worker1        - 10.0.0.3 
4. Kubernetes Node2   - kubernetes.worker2        - 10.0.0.4 

Check the IPs of your cluster. If different, you will then need to update the inventory - hosts file.


**Tasks in the Role**
This role contains tasks to:
- Install basic packages required
- Setup standard system requirements - Disable Swap, Modify sysctl, Disable SELinux
- Install and configure a container runtime of your Choice - cri-o, Docker, Containerd
- Install the Kubernetes packages - kubelet, kubeadm and kubectl
- Configure Firewalld on Kubernetes Master and Worker nodes

---

<<<<<<< HEAD
## <font color='red'>Pre-requiste Configuration Steps</font>
Ansible has already been installed with a projects/demo directory.   
lets create another directory 
=======
#### <font color='red'>Resolve Domain names</font>
To ensure that each machine can communicate with each other, we need to modify their /etc/hosts file.  Ensure that all servers are up and running

on the Ansible Controller:
```
sudo nano /etc/hosts
```





## How to use this role

- Clone the Project:

```
$ git clone https://github.com/jmutai/k8s-pre-bootstrap.git
>>>>>>> 9352bddef11de1128cc48dc90495a033f7e931c3
```
cd
sudo mkdir ansible-projects/k8s
```
change to ansible_projects/k8s directory:
```
cd ansible_projects/k8s
```
now copy over ansible directory:
```
sudo cp -rpP /etc/ansible/* .
```



#### <font color='red'>Configuration Steps</font>
Once you have successfully cloned the repository into your Ansible Server, navigate to the centos directory and make the following changes.
* Change the “ad_addr” in the env_variables file with the IP address of the Kubernetes master node.
* Add the IP Addresses of the worker nodes and the master node in the “hosts” file.
* Run the following command to configure the Kubernetes Master / Worker nodes.
  * ansible-playbook pre-flight.yml

Once the pre-flight configurations tasks have completed, the check:
* the correct hostnames have been added to all /etc/hosts machines (obviously you wouldn't normally do this, its just useful the first time to note the task output).
* check that you can ping machines using the domain names instead of IPs





```

