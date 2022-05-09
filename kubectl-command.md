## 1. Kubectl characteristic
- Kubectl là công cụ cấu hình dòng lệnh để tương tác với các cụm Kubernetes sử dụng máy chủ Kubernetes API
- Kubectl cho phép người dùng tạo, kiểm tra, cập nhật và xóa các đối tượng Kubernetes
- Mọi lệnh Kubernetes đều có điểm cuối API với mục đích chính là thực hiện các yêu cầu HTTP tới API
## 2. How Kubectl works
- Khi chạy lệnh với Kubectl, nó sẽ built một HTTP REST API request bên dưới và gửi request này tới Kubernetes API server, sau đó truy xuất kết quả và hiển thị trên thiết bị đầu cuối
- Nếu muốn thực thi bất kỳ hoạt động nào của Kubernetes, chỉ cần thực hiện một yêu cầu HTTP tới điểm cuối API tương ứng của nó.
![image](https://user-images.githubusercontent.com/92737759/167388449-b5893847-6d8b-4d76-9a65-3a7fee59eed2.png)
## 3. Most Common Kubectl Commands:
### 3.1. Cluster management: 
- Một Kubernetes cluster tập hợp các Nodes chạy các ứng dụng dựa trên containers
- Cho phép Containers chạy trên nhiều môi trường: máy ảo, vật lý, nền tảng đám mây
- Các lệnh Kubectl sau dùng để quản lý một cụm:
  + 
