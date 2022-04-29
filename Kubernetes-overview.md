# 1. What is Kubernetes
- K8S là một phần mềm mã nguồn mở, có vai trò là một **container orchestration**, tức là một hệ quản trị các container và các thành phần khác liên quan
- Được phát triển bởi Google
- K8S giúp quản lý, giám sát, kiểm soát lỗi các ứng dụng được đóng gói từ hàng trăm hoặc hàng nghìn các containers trên nhiều môi trường khác nhau: (physical machine, virtual machine, cloud environment, hybrid deployment environment)
- Container scheduling
- Rolling update
- Scaling/Auto Scaling
- Monitor vòng đời và trạng thái của container
- Self-hearing trong trường hợp có lỗi xảy ra (có khả năng tự phát hiện và correct lỗi)
- Service discovery
- Load balancing
- Quản lý data, work node, log
- Infrastructure as code
- Disater recovery: backup and restore
- Sự liên kết và mở rộng với các hệ thống khác
- Chuyển đổi từ Monolith sang Microservices
## 1.1 Monolith Architecture và Microservices Architecture
> **Monolith Architecture**
> 
![image](https://user-images.githubusercontent.com/92737759/165874634-df0f6199-f960-4444-a032-3747a1fb0583.png)
* Monolith phù hợp với các dự án quy mô nhỏ với các ưu điểm sau:
  + Quá trình development đơn giản và trực tiếp, centralized management và những bước phát triển cơ bản sẽ không được gặp lại.
  + Tất cả mọi quá trình development đều nằm trên 1 project. Development flow đơn giản chỉ là submit changes, review, merge code và continue
* Hạn chế kèm theo:
  + Khó khăn trong việc bảo trì: các khối code dính chặt vào nhau, vấn đề đặt ra sẽ khó bắt đầu từ đâu trong 1 khối lớn để xử lý lỗi
  + Quá trình development sẽ mất đi tính linh hoạt: thời gian để build feature sẽ bị dài lên, bị block lần nhau. Bất kì sự thay đổi nhỏ nào cũng cần build toàn bộ lại dự án => tốn rất nhiều thời gian.
  + Tính ổn định không cao. Bất kỳ một lỗi nào cũng có thể khiến toàn bộ application bị crash
  + Khả năng mở rộng khó được đáp ứng trong trường hợp phải đáp ứng một lượng truy cập lớn từ phía yêu cầu của business
> **Microservice Architecture**
>
  Microservices sẽ chia hệ thống ra thành các services. Mỗi service này đều có một logic riêng, 1 trách nhiệm riêng và có thể được deploy riêng biệt. Khái niệm mircoservice đồng thời đề cập đến xu hướng tách biệt architecture ra thành các loose coupling service, mỗi service sẽ được nằm trong 1 context nhất định.  
*  Các thuộc tính của mô hình microservice  
- Autonomous (tính tự trị): Một service sẽ là 1 đơn vị chức năng, cung cấp API để thực hiện việc trao đổi, giao tiếp với các service khác
- Isolated (tính biệt lập): mỗi service sẽ là một đơn vị triển khai. Nó có thể được chỉnh sửa, test và deployed như một đơn vị mà không ảnh hưởng đến những khía cạnh khác.
- Elastic: 1 service là phi trạng thái (stateless) vì vậy nó có thể scale tùy ý khi cần thiết
- Resilient: 1 microservice sẽ được thiết kế để chấp nhận các lỗi, các rủi ro có thể xảy ra và có thể chấp nhận được
- Responsive: Respond cho các request trong khoảng thời gian hợp lý
- Intelligent: Hệ thống có thể tìm thấy các endpoint của các microsevice đã được đăng ký
- Message Oriented: Mô hình micro-service hoạt động dựa trên giao thức HTTP hoặc message bus để tạo nên sự giao tiếp giữa các service. Điều này đảm bảo tính loose coupling, tính biệt lập và có thể cung cấp lỗi dưới dạng message
- Programmable: cung cấp API cho phép truy cập bởi developer và administrator
- Composable: Bao gồm nhiều microsevices
- Automated: Lifecycle của Microsevice được quản lý thông qua automation bao gồm development, build, test, staging, production, distribution.  
*  Lợi ích khi sử dụng kiến trúc Microservice
- Mỗi microservice sẽ được chia nhỏ để tập trung vào một business function cụ thể hoặc business requirement.
- Microservices có thể phát triển độc lập bởi một team nhỏ có thể chỉ từ 2 đến 5 developers.
- Microservice đem lại tính loose-coupling và context riêng cho mỗi service, sẽ dễ dàng trong quá trình development cũng như deploy một cách độc lập.
- Microservices có thể phát triển với nhiều ngôn ngữ khác nhau.
- Quá trình phát triển một service sẽ trở nên dễ dàng và linh động thông qua việc sử dụng CI/CD như Travis, Jenskin, Circle CI..
- 1 serive trong mô hình microserivce là tương đối nhỏ, dễ hiểu và được quản lí bởi các thành viên. Do đó, họ sẽ dễ dàng tập trung vào công việc, nâng cao được hiệu năng.
- Microservices cho phép tận dụng việc áp dụng những công nghệ mới vào dự án.
- Microservices chỉ gồm business logic code và không bao gồm HTML, CSS.
- Việc deploy sẽ mất ít thời gian cho việc configuraton.
- Dễ dàng tích hợp 3rd-party.
- Mỗi service có dung lượng lưu trữ riêng và có thể có cơ sở dữ liệu riêng.
# 2. Kiến trúc Kubernetes
![image](https://user-images.githubusercontent.com/92737759/165878285-092f27be-0ee3-4c30-a6e3-987c4367123d.png)
- Kubernetes cluster bao gồm ít nhất 1 Master node (node = virtual or physical machine) để khi 1 Master node down sẽ có backup 1 master node khác và các Worker node. Master Node và các Worker Node giao tiếp với nhau thông qua Virtual Network  
  + Master node đóng vai trò xử lý các tác vụ quản lý, điều phối của hệ thống  
  + Worker node sẽ là nơi xử lý các work load của hệ thống. Các pod sẽ được tạo và chạy các Worker node này. Worker node sẽ chứa các containers của các ứng dụng khác nhau và tiến hành chạy các ứng dụng này  
- Master Node có 4 thành phần chính:
  + **Api-server**: Đây là core của K8S Master, nó mở ra các HTTP API cho phép người dùng cuối cũng như các thành phần khác nhau trong chính K8S cluster có thể trao đổi thông tin với nhau. K8S API cho phép người dùng lấy thông tin về trạng thái của các đối tượng trong hệ thống như Pods, Namespaces, Services... Hầu hết các tác vụ sử dụng kube-api thông qua lệnh kubectl nhưng cũng có thể gọi trực tiếp REST API.
  + **Controller Manager**: Là một tiến trình chạy nền trên các Master Node. Các tiến trình này chạy liên tục để điều tiết trạng thái của hệ thống Kubernetes. Trong K8S, controller là một vòng lặp điều khiển giám sát trạng thái của cluster được chia sẻ qua các api và thực hiện các thay đổi cần thiết để chuyển trạng thái của cluster tới trạng thái mong muốn. Ví dụ khi một container stop thì nó sẽ điều khiển để restart lại container  
  + **Kube-scheduler**: Đây là service mặc định của K8S làm nhiệm vụ phân phối Pod sẽ được chạy trên node nào. Mỗi Container bên trong Pod có thể có những yêu cầu khác nhau, hoặc ngay các Pod cũng có yêu cầu khác nhau. Do đó nhiệm vụ của Scheduler là tìm kiếm các node thỏa mãn các điều kiện trên và lựa chọn node tối ưu nhất để chạy. Trong trường hợp không có node nào thỏa mãn các điều kiện đặt ra thì Pod sẽ ở trạng thái chưa được lên lịch thực hiện cho tới khi Scheduler tìm được node phù hợp.  
  + **Etcd**: Là một cơ sở dữ liệu dạng key-value có tính khả dụng và đồng nhất cao. Etcd là nơi K8S lưu trữ toàn bộ các thông tin cấu hình của hệ thống, trạng thái dữ liệu bên trong node và các container trong mỗi node  
- Các thành phần của Worker Node:
  + **Kubelet**: Nó đóng vai trò như một "Node Agent" của K8s trên các Worker Node. Nhiệm vụ của nó để các Worker Node được đăng ký và quản lý bởi cụm K8S cũng như là nhận nhiệm vụ triển khai các Pod (thường thông qua kube api-server) và đảm bảo các container đó chạy ổn định. Kubelet không quản lý các container không được tạo bởi Kubernetes  
  + **Kube-proxy**: Kube-proxy là một network proxy chạy trên mỗi node trong K8S cluster, thực hiện một phần Kubernetes Service. Kube-proxy duy trình network rules trên các node. Những network rules này cho phép kết nối mạng đến các pods từ trong hoặc ngoài cluster.  
## 2.1 Các thành phần chính của Kubernetes
> **Pod**
> 
- Pod là đơn vị nhỏ nhất trong Kubernetes, dùng để schedule, deploy một hoặc nhiều container liên quan đến nhau.
- Các container trong cùng 1 pod (thông thường 1 ứng dụng sẽ chạy trên 1 pod) sẽ luôn được schedule trên cùng một node và cùng nhau chia sẻ tài nguyên (filesystem, networking). Nếu node đó dừng hoạt động, các pod nằm trên pod đó sẽ được schedule trên 1 Node khác trong cluster
- Mỗi Pod sẽ được cung cấp 1 địa chỉ IP để giao tiếp với database hay với các pod khác, nếu 1 pod xảy ra lỗi nó sẽ được cấp 1 địa chỉ IP mới
- Các application sẽ chạy trong pod, và ta sẽ tương tác thông qua service
> **Service**
> 
- Service đơn giản là 1 địa chỉ ip tĩnh và mỗi ứng dụng hay pod dữ liệu sẽ có một service riêng
- Vòng đời (lifecycle) của các pod và service không kết nối với nhau nên khi một pod chết nó sẽ được thay thế 1 pod khác mà k cần thay thế endpoint
- Services đại diện cho 1 điểm vào (entry-point) để truy cập vào application
- Thực hiện các chức năng load-balancing cho một nhóm đổi tượng các pods bằng nhãn (labels)
- Scale application mà không ảnh hưởng đến end-user
- Cung cấp quyển truy cập tới resouces bên ngoài Kubernetes cluster







