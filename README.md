# KubernetesProject

Kubernetes is a container cluster manager designed to automatically scale and deploy containers across multiple hosts. It is designed to have rolling updates to containers and setup load balancing to duplicates of the same container. Very inclusive no limit to types of applications supported in terms of language or framework.

In Kubernetes containers are grouped into pods of containers running as parts of a larger service within the cluster. To orchestrate this cluster the tool kubeadm can be used to act as the master of the pod network.

## Installation Guide

This is mostly taken from the official installation guide: [Installing Kubernetes on Linux with Kubeadm](http://kubernetes.io/docs/getting-started-guides/kubeadm/)

### OS requirement
  - Ubuntu 16.04
  - CentOS 7
  
  
Using an AWS instance, check version of Ubuntu is high enough. Can run `do-release-upgrade` to get the latest version of Ubuntu.
You will likely need to install curl too `sudo apt-get install curl`

Input the following commands as root (switch with `sudo su`)

```bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat << EOF > /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y docker.io
apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```
### The tools installed are:
* **Docker** as the underlying container runtime .
* **kubeadm** bootstraps the cluster, still in alpha development.
* **kubectl** command line tool to control the cluster.
* **kubelet** essential component of Kubernetes, runs on all cluster machines.

To start the master run:
```bash
kubeadm init
```
This will produce several lines of output as it downloads and installs the essential cluster database and "control plane" componenets. Eventually the initialisation will end with:
```bash
Kubernetes master initialised successfully!

You can connect any number of nodes by running:

kubeadm join --token <token> <master-ip>
```
`kubeadm join` will have a token unique to your instance that allows a node to be added to your cluster.

A common task in development is to setup a test pod on the master, a single machine Kubernetes cluster. By default this is disabled for security reasons but you can disable this with `kubectl taint nodes --all dedicated-`

### Adding a pod network addon

If you check the `kube-system` name space at this point (`kubectl get pods -n kube-system`) you will see that the `kube-dns` pod is not currently running.

In order for the master to orchestrate the pod cluster it requires a Container Network Interface (CNI) addon. For this short installation guide Weave Net has been chosen which can be easily added to the cluster with:
```bash
kubectl create -f https://git.io/weave-kube
```
Once the addon is installed confirm `kube-dns` is running.

### Joining nodes to the cluster

To add a new machine to the cluster access the terminal as root (eg. by logging in with SSH). Make sure the four tools: Docker, kubeadm, kubectl and kubelet are installed and run the `kubeadm join` command.
```bash
kubeadm join --token <token> <master-ip>
```
With the token and ip that were given when `kubeadm init` was run on the master.

Once the machine has been added to the cluster it can be seen in the list of nodes when `kubectl get nodes` is run on the master.
