# Install Kubernetes Kubeadm on Ubuntu
![kube](https://github.com/JRK0421/kubeadm-installation/blob/main/k8s_kubeadm.jpg)

Kubernetes is an open-source container orchestration tool that helps deploy, scale, and manage containerized applications. Google initially designed Kubernetes and is now maintained by the Cloud Native Computing Foundation. With Kubernetes, you can freely make use of the hybrid, on-premise, and public cloud infrastructure to run deployment tasks of your project.<br>

Kubernetes works with Docker, Containerd, and CRI-O currently.<br>
####
We can run Kubernetes locally using the below methods:<br>

  ### 2. [Kubeadm](https://github.com/JRK0421/kubeadm-installation/edit/main/README.md):  A multi-node Kubernetes cluster

####
Here, we will see how to deploy a multi-node Kubernetes cluster using the 
### Follwing Steps are for Installing Kubernetes Kubeadm on Ubuntu

### Before you begin
  - 2 or more Linux servers running Ubuntu 22.04
  - root privileges
  - 3.75 GB or more of Ram - for better performance, use 6 GB
  - 2 CPUs or more
  - Full network connectivity between all machines in the cluster (public or private network is fine)
  - Unique hostname, MAC address, and product_uuid for every node.
  - Certain ports are open on your machines.
    
## Step 1:
### Bellow Step Going on Master-Worker Node
#### Install-Kubernetes-Kubeadm-on-Ubuntu for (Master-Worker) Node
    rm -rf install-kubeadm
    git clone https://github.com/JRK0421/kubeadm-installation/blob/main/kubeadm-ubuntu.sh
    sudo chmod 755 kubeadm.sh
    ./kubeadm.sh


### Step 2 - Configuring Network Plugins
A Pod Network is a way to allow communication between different nodes in the cluster. 

We have Differents Types Network Plugins:

If we use the Calico virtual network:
####
    sudo kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml
If we use the flannel virtual network:
####
    sudo kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

Allow the process to complete.

Verify To display the network status, use the following command:
####
    kubectl get pods --all-namespaces
Check cluster status:
####
    kubectl cluster-info
#
### Step 3 - Joining worker node to a Kubernetes Cluster
On each worker node, use the <b>kubeadm join</b> command on each worker node to connect it to the cluster.
View the master join token:
####
    kubeadm token create --print-join-command
<b>Output Like:</b>
> kubeadm join 10.173.xxx.xxx:6443 --token 1blbo6.flvvj6mnbda6gc00 \
--discovery-token-ca-cert-hash sha256:16623c83249cb1072443d8e1e3691e1296aec9e83b860172fdadf3028xxxxx
    
Verify all tokens:

    kubeadm token list
#
### This Step perform on Worker Node for Joining with k8s Cluster
####
Copy the kubeadm join token & Paste on <b>Worker Node</b>
####
    kubeadm join 10.173.xxx.xxx:6443 --token 1blbo6.flvvj6mnbda6gc00 \
--discovery-token-ca-cert-hash sha256:16623c83249cb1072443d8e1e3691e1296aec9e83b860172fdadf3028xxxxx
####
#

</details>

#
## Automated Installation
#### Configure as Kubeadm Master Node (Master)
~~~
rm -rf install-kubeadm
git clone https://github.com/JRK0421/kubeadm-installation/blob/main/master.sh
sudo chmod 755 master.sh
./master.sh
~~~
#
## Step 4
### This Step perform on Worker Node for Joining with k8s Cluster
#### Joining worker node with Kubernetes Cluster
Copy the kubeadm join token & Paste on <b>Worker Node</b>
~~~
 kubeadm join 10.173.xxx.xxx:6443 --token 1blbo6.flvvj6mnbda6gc00 \
--discovery-token-ca-cert-hash sha256:16623c83249cb1072443d8e1e3691e1296aec9e83b860172fdadf3028xxxxx
~~~
## Verify from Master Node
~~~
kubectl get nodes
~~~
~~~
kubectl get nodes -o wide
~~~
The system should display the worker nodes that you joined to the cluster.

Check cluster status:
~~~
kubectl cluster-info
~~~
#
## Step 5 
### Test Kubernetes Cluster (From Master Node)
To test Kubernetes installation, let’s try to deploy nginx based application and try to access it.
####
    kubectl create deployment mynginx --image=nginx --replicas=2
Check the status of nginx-app deployment
####
    kubectl get deployment mynginx
Expose the deployment as NodePort
####
    kubectl expose deployment mynginx --type=NodePort --port=80 
Run following commands to view service status
####
    kubectl get svc mynginx
####
    kubectl describe svc mynginx
Use following curl command to access nginx based application,
####
    curl http://<woker-node-ip-addres>:31246

Conclusion
After following the steps mentioned in this article carefully, you should now have Kubernetes installed on Ubuntu. Kubernetes allows you to launch and manage Docker containers across multiple servers in the pod.

#### In this article, we have explained the installation of the Kubernetes container management system on Ubuntu 22.04. Kubernetes has a lot of functionality and features to offer. The Kubernetes Official Documentation is the best place to learn.


