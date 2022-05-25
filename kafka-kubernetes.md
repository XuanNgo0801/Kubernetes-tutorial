![image](https://user-images.githubusercontent.com/92737759/170175354-7555906e-f01d-455d-bf22-77bcb5167f3c.png)
![image](https://user-images.githubusercontent.com/92737759/170176126-4b856f3a-a28d-4aaa-8d27-5c5e3613b89b.png)

## 1. How to install nfs-server
### Step 1) Install the NFS kernel Server package
```
$ sudo apt update
$ sudo apt install nfs-kernel-server
$ sudo systemctl status nfs-server
```
![image](https://user-images.githubusercontent.com/92737759/170175956-d08729e6-0847-4488-a5ba-82804002a8ca.png)
### Step 2) Create an NFS directory share
- Tạo một thư mục chia sẻ NFS trong thư mục /mnt/
```
$ sudo mkdir /mnt/viot_nfsserver
## Câu lệnh cho phép tất cả các clients có thể truy cập vào thư mục
$ sudo chown nobody:nogroup /mnt/viot_nfsserver
$ sudo chmod -R 777 /mnt/viot_nfsserver
```
### Step 3) Grant the NFS Server access to clients
- Cấp quyền truy cập máy chủ NFS server cho các máy khách
`$ sudo vi /etc/exports`
![image](https://user-images.githubusercontent.com/92737759/170177875-699d1e55-1f0a-4999-9b7a-74cc9e9e77ad.png)
- Cho phép tất cả các máy khách trong mạng con 172.21.5.0 truy cập vào máy chủ
### Step 4) Export the shared directory
`$ sudo exportfs -a`
### Step 5) Configure the firewall rule for NFS Server
`$ sudo ufw allow from 172.21.5.0/24 to any port nfs`
![image](https://user-images.githubusercontent.com/92737759/170178194-86187ba5-bee6-41f0-9519-3d3fb8e51791.png)
### Step 6) Configure the Client system
- ssh vào 3 máy khách để cấu hình
```
$ sudo apt update
$ sudo apt install nfs-common
```

------------------------------------------------------------------
## 2. How to install kafka
```
k create ns viot-kafka 
helm repo add bitnami http://172.20.1.22:8081/repository/helm-bitnami/ 
helm repo update 
helm install viot-kafka bitnami/kafka -n viot-kafka 
```
![image](https://user-images.githubusercontent.com/92737759/170180641-841b3e00-0d9f-4278-bb90-dc82e9af18e9.png)
`k apply -f nfs-deployment.yaml -n viot-kafka `
- Chỉnh sửa PVC (PersistentVolumeClaim)
  + PersistentVolume (pv) là một phần không gian lưu trữ dữ liệu tronnng cluster, các PersistentVolume giống với Volume bình thường tuy nhiên nó tồn tại độc lập với POD (pod bị xóa PV vẫn tồn tại), có nhiều loại PersistentVolume có thể triển khai như NFS, Clusterfs ... (xem tại Các kiểu PersistentVolume )

  + PersistentVolumeClaim (pvc) là yêu cầu sử dụng không gian lưu trữ (sử dụng PV). Hình dung PV giống như Node, PVC giống như POD. POD chạy nó sử dụng các tài nguyên của NODE, PVC hoạt động nó sử dụng tài nguyên của PV.
![image](https://user-images.githubusercontent.com/92737759/170180956-de39164c-3ebb-41c3-86d0-600b2c014217.png)

