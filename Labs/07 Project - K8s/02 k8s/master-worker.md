## <font color='red'>6.2 Master & Worker Nodes</font>
Here's the disclaimer..  this project is definately not intended for production, but to highlight the steps required to install and configure K8s on a 3 node cluster - 1 master node, 2 worker nodes. Before you can install K8s on the cluster a number of pre-flight checks / configurations ahve toi be completed.

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

## <font color='red'>Configuration Steps</font>
Ensure that the pre-flight steps have completed succesfully. 
* Run the following command to install the K8s packages on the Kubernetes Master node.
  * ansible-playbook setup_master_node.yml
* Check that the Master Node Pods are up and running.

Once the master node is ready, run the following command to set up the worker nodes:
 * ansible-playbook setup_worker_nodes.yml


Once the workers have joined the cluster, run the following command to check the status of the worker nodes.
 * kubectl get nodes
The output will be as follows.