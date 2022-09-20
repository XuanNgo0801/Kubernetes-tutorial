- Before you begin: 
- Make sure you read the release notes carefully.
- The cluster should use a static control plane and etcd pods or external etcd.
- Make sure to back up any important components, such as app-level state stored in a database. kubeadm upgrade does not touch your workloads, only components internal to Kubernetes, but backups are always a best practice.
- Swap must be disabled.
This tutorial will explain how to upgrade version of K8s cluster from v1.22.0 to v1.23.0:  
## Upgrading control plane nodes:
```
sudo apt update
sudo apt-cache madison kubeadm (# find the latest 1.23 version in the list)
```
- Upgrade kubeadm:
```
sudo apt-mark unhold kubeadm
sudo apt-get update
sudo apt-get install -y kubeadm=1.23.x-00
sudo apt-mark hold kubeadm
```
+ The reason why we run apt-mark unhold and apt-mark hold is because if we upgrade kubeadm then the installation will automatically upgrade the other components like kubelet to the latest version by default, so we would have a problem. To fix that, we use hold to mark a package as held back, which will prevent the package from being automatically installed, upgraded, or removed
- Verify that the download works and has the expected version:  
`kubeadm version`
- Verify the upgrade plan:
`sudo kubeadm upgrade plan`
- Choose a version to upgrade to, and run the appropriate command. For example:  
`sudo kubeadm upgrade apply v1.23.x`
- Once the command finishes you should see:
```
[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.23.x". Enjoy!

[upgrade/kubelet] Now that your control plane is upgraded, please proceed with upgrading your kubelets if you haven't already done so.
```
- Upgrade kubelet and kubectl:  
```
sudo apt-mark unhold kubelet kubectl
sudo apt-get install -y kubelet=1.23.x-00 kubectl=1.23.x-00
sudo apt-mark hold kubelet kubectl
```
- Restart the kubelet:
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```
## Upgrading other control plane nodes:
- Same as the first control plane node but use:  
`sudo kubeadm upgrade node`
## Upgrade worker nodes
- Upgrade kubeadm:
```
sudo apt-mark unhold kubeadm
sudo apt-get update 
sudo apt-get install -y kubeadm=1.23.x-00
sudo apt-mark hold kubeadm
```
- For worker nodes this upgrades the local kubelet configuration:
`sudo kubeadm upgrade node`  
- Upgrade the kubelet and kubectl:
```
sudo apt-mark unhold kubelet kubectl
sudo apt-get install -y kubelet=1.23.x-00 kubectl=1.23.x-00
sudo apt-mark hold kubelet kubectl
```
- Restart the kubelet:
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```
- Verify the status of the cluster:  
![image](https://user-images.githubusercontent.com/92737759/191154680-37acf2e1-dbc3-4554-bc0f-c32a5f03141f.png)
