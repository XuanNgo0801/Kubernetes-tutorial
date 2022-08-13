## Step 1: Install Kubernetes Servers
- Update the servers:
```
$ sudo apt update
$ sudo apt -y full-upgrade
$ [ -f /var/run/reboot-required ] && sudo reboot -f
```
## Step 2: Install kubelet, kubeadm and kubectl
- Once the servers are rebooted, add Kubernetes repository for Ubuntu 20.04 to all the servers.
```
$ sudo apt -y install curl apt-transport-https
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
- Install required packages:
```
$ sudo apt update
$ sudo apt -y install vim git curl wget kubelet kubeadm kubectl
$ sudo apt-mark hold kubelet kubeadm kubectl
```
- Check the version of kubectl
`$ kubectl version --client && kubeadm version`
![image](https://user-images.githubusercontent.com/92737759/165881934-55110bf6-9587-4266-a3f4-cc6b3039d22a.png)
## Step 3: Disable Swap
```
$ sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
$ sudo swapoff -a
```
- Enable kernel modules
```
# Enable kernel modules
$ sudo modprobe overlay
$ sudo modprobe br_netfilter

# Add some settings to sysctl
$ sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

# Reload sysctl
$ sudo sysctl --system
```
## Step 4: Install Container runtime
- Installing Docker runtime
```
# Add repo and Install packages
$ sudo apt update
$ sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt update
$ sudo apt install -y containerd.io docker-ce docker-ce-cli

# Create required directories
$ sudo mkdir -p /etc/systemd/system/docker.service.d

# Create daemon json config file
$ sudo tee /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

# Start and enable Services
$ sudo systemctl daemon-reload 
$ sudo systemctl restart docker
$ sudo systemctl enable docker
```
## Step 5: Initialize master node
- `lsmod | grep br_netfilter`
- Enable kubelet service  
`sudo systemctl enable kubelet`
- Pull container images:
- On the Master Node (control-plane node) perform a boostrap to initialize the Kubernetes Cluster  
`kubeadm init --apiserver-advertise-address=172.17.0.1 --pod-network-cidr=10.100.0.0/16`
  + 172.17.0.1: The ip address of Master Node, this address needs to be able to communicate with the rest of the nodes in the cluster.
  + 10.100.0.0/16: The dependent network address range that the network technology will use for Pods to communicate with each other in the Kubernetes Cluster
- Configure kubectl using commands in the output:
```
$ mkdir -p $HOME/.kube
$ sudo cp -f /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
- Check cluster status:
`$ kubectl cluster-info`
![image](https://user-images.githubusercontent.com/92737759/165884205-524356ad-e0dc-42e1-8135-45ea111b4ab0.png)
## Step 6: Install network plugin on Master
- In this guide we’ll use Calico. 
```
$ kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml 
$ kubectl create -f https://docs.projectcalico.org/manifests/custom-resources.yaml
```
- Confirm that all of the pods are running:  
`watch kubectl get pods --all-namespaces`  
![image](https://user-images.githubusercontent.com/92737759/165884524-7e51b140-5719-42f6-b0bc-709f85ad9db4.png)
- Confirm master node is ready:  
- `kubectl get nodes -o wide`  
![image](https://user-images.githubusercontent.com/92737759/165884638-265f549f-dea9-4582-bf01-233b063fdca8.png)

## Step 7: Cấu hình Worker Node tham gia Kubernetes Cluster
```
$ kubeadm token create --print-join-command
$ kubeadm join 172.21.5.17:6443 --token xt9jyv.l34wvpt1b4ddbuse  
--discovery-token-ca-cert-hash sha256:ef849283f86ec8f3658f22f6f2b25576baf875a257da8487b0673e9d154d0510
$ k get node -A
```
![image](https://user-images.githubusercontent.com/92737759/184492192-95b86132-b08f-4bdd-9d0e-842b95757823.png)








