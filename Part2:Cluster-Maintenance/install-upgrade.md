##  Installing a Cluster

The primary tool for installing new nodes and upgrading a node version is **kubeadm**.
To install kubeadm, follow the installation instructions step-by-step in the official kubernetes documentation. 


https://v1-29.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

In the CKA exam the kubeaddm executable has been preinstalled for you. 🤖

Our task is to create a Kubernetes cluster with kubeadm.

We'll use an example where we need to install one control plane node and one worker node. If your requirements demand more worker nodes, simply repeat the worker node installation process to add more nodes to the cluster.


1. `ssh kube-controlplne`
2.  we need to add this following two command-line options : <br>
 --pod-network-cidr by running <br>
 --apiserver-advertise-address <br>
  The information we retrieve from : `ip add show` <br>
  `kubeadm init --pod-network-cidr=10.244.0.0/16  --apiserver-advertise-address=192.111.3.0 ` <br>
 to start using the cluster we need to run these commands, which are also part of the kubeadm init output:

    ```bash
        mkdir -p $HOME/.kube
        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```
    Then, let's save the kubeadm join commands for later. 
3. We must deploy a Container Run Network Interface (CNI) plugin so that pods can communicate with each other <br>
The CKA exam might specify the exact CNI (Container Network Interface) plugins you'll need to use, such as Flannel, Calico, or Weave. While the general installation process might be similar for these plugins, it's crucial to verify the official Kubernetes documentation for each specific plugin to ensure proper configuration.<br>
   `kubectl apply -f <add-on.yaml>`
4. `ssh worker-node`
5. Now, we can use the kubeadm join command provided by the kubeadm init console output to join a worker node to the cluster. <br>
`kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>`
6. `kubectl get nodes` to verify that the worker node is now part of the cluster. <br>
 We can repeat the process for any other worker node we want to add to the cluster.



## Operating System Upgrade

The process of upgrading would be as follows :

1. Upgrade a primary control plane node
2. Upgrade additional control plane nodes
3. Upgrade worker nodes

Before we begin the upgrade, we need to safely evict all pods from the node that will be undergoing maintenance, with : 
`kubectl drain  <node_name> --ignore-daemonsets `

**Note:** We cannot drain a node that contains a pod without a replica set managing it.
Solution: 
` kubectl cordon node <node_name>` 
to prevent new pods from being scheduled on the node while existing pods continue to run.
`kubectl uncordon node-name`
Used to bring a node back online.

1. `ssh controleplane`
2. Check the nodes and thier kubernetes versions: `kubectl get nodes ` 
3. `sudo apt update ` && `sudo apt-cache madison kubeadm`
4. Install new kubeadm version: 
    ```bash
        sudo apt-mark unhold kubeadm && \
        sudo apt-get update && sudo apt-get install -y kubeadm='1.X.x-*' && \
        sudo apt-mark hold kubeadm
    ```
5. Verfiy that is the correct version :`kubeadm version` 
6. Verify the upgrade plan: `sudo kubeadm upgrade plan`
7. Upgrade the control plane: `sudo kubeadm upgrade apply v1.X.x`
```bash
[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.X.x". Enjoy! 🚀
```
8. Any new workload won't be schdulable on the node until uncordoned: `kubectl drain controlplane --ignore-daemonsets`
9. Upgrade the kubelet and kubectl: 
    ``` bash 
        sudo apt-mark unhold kubelet kubectl && \
        sudo apt-get update && sudo apt-get install -y kubelet='1.X.x-*' kubectl='1.X.x-*' && \
        sudo apt-mark hold kubelet kubectl
    ```
10.  ``` bash
        sudo systemctl daemon-reload
        sudo systemctl restart kubelet
     ```
11. Reeanable the kubelet process:  `kubectl uncordon controlplane`
12. Repeat the process for the other control plane nodes.
The upgrade procedure on worker nodes should be executed as the controle just remplace step
1. `ssh worker-node`
2. `sudo apt-get install -y kubeadm=1.X.x `
3. `kubeadm version`
4. `sudo kubeadm upgrade node`
5. `kubectl drain worker-node --ignore-daemonsets`
6. `sudo apt-mark unhold kubelet kubectl && \  sudo apt-get install -y kubelet=1.X.x kubectl=1.X.x `
7. `sudo systemctl daemon-reload`
8. `sudo systemctl restart kubelet`
9. `kubectl uncordon worker-node`
10. `kubectl get nodes` <br>
The `STATUS` column should show `Ready` for all nodes.
