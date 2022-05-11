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
  + `kubectl cluster-info`: Hiển thị thông tin điểm cuối về các node master và services trong 1 cụm
  + `kubectl version`: Hiển thị thông tin version của Kubenetes trên client và server
  + `kubectl config view`: Lấy thông tin cấu hình của một cụm
  + `kubectl api-resource`: Liệt kê API resource có sẵn
  + `kubectl api-versions`: Liệt kê API versions có sẵn
  + `kubectl get all --all -namespaces`: Liệt kê tất cả
### 3.2. Listing Resources: 
- Kubernets resource là các đối tượng Kubernetes được liên kết với một không gian tên cụ thể
- Một số lệnh để thao tác hiển thị Kubenetes resources:
  + `kubectl get namespaces`: Tạo ra một văn bản dạng text bao gồm tất cả các namespaces
  + `kubectl get pods`: List ra tất cả các pods hiện có
  + `kubectl get pods -o wide`: Hiển thị thông tin chi tiết của tất cả các pods dưới dạng text
  + `kubectl get pods-field-selector=spec.nodeName=[server-name]`: Hiển thị một list các pods đang chạy trên node server
  + `kubectl get replicationcontroller [replication-controller-name]`: Liệt kê một bộ điều khiển sao chép cụ thể ở dạng text
  + `kubectl get replicationcontroller, services`: Tạo ra danh sách dạng text các bộ điều khiển sao chép và các services
### 3.3.Daemonsets
- Câu lệnh sử dụng **Daemonsets** đảm bảo rằng tất cả hoặc một vài Nodes chạy một bản sao của Pod
- Khi một Node được thêm/xóa vào cụm, các Pods cũng sẽ được added/xóa vào Nodes
- Một số câu lệnh cơ bản:
  + `kubectl get daemonset`: List một hoặc nhiều các **Daemonset**
  + `kubectl edit daemonset<daemonset_name>`: Edit hoặc update các định nghĩa của 1 hoặc vài daemonset
  + `kubectl delete daemonset <daemonset_name>`: Xóa
  + `kubectl create daemonset <daemonset_name>`: Tạo mới một daemonset
  + `kubectl rollout daemonset`: Quản lý việc triển khai một daemonset
  + `kubectl describe ds <daemonset_name> -n <namespace_name>`: Hiển thị trạng thái chi tiết của daemonsets với namespace
### 3.3. Deployments
- Kubernetes Deployments được sử dụng để tạo hoặc biến đổi các phiên bản của các pods chưa các ứng dụng dựa trên container
- Việc Deployment có thể mở rộng số lượng pods bản sao, cho phép triển khai mã cập nhật theo cách có kiểm soát hoặc quay trở lại phiên bản triển khai trước đó nếu cần.
- Một số câu lệnh cơ bản:
  + `kubectl get deployment`; List 1 hoặc nhiều deployments
  + `kubectl describe deployment <deployment-name>`: Hiển thị trạng thái chi tiết của một hoặc nhiều Deployment
  + `kubectl edit deployment <deployment_name>`: Edit và update các deployment trên server
  + `kubectl create deployment <deployment_name>`: Tạo mới một deployment
  + `kubectl delete deployment <deployment_name>`: Xóa
  + `kubectl rollout status deployment <deployment_name>`:Xem các trạng thái triển khai của deployment
### 3.4. Events
Các sự kiện Kubernetes là các đối tượng hiển thị cho bạn những gì đang xảy ra bên trong một cụm, chẳng hạn như quyết định nào được đưa ra bởi bộ lập lịch hoặc lý do tại sao một số nhóm bị loại bỏ khỏi nút. Sự kiện là thứ đầu tiên cần xem xét đối với ứng dụng, cũng như hoạt động của cơ sở hạ tầng khi có thứ gì đó không hoạt động như mong đợi. Sau đây là các lệnh kubectl để nhận các sự kiện.
