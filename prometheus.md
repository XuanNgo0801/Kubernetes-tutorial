# Setup Prometheus and Grafana on Kubernetes using prometheus-operator
## 1. Overview
- Prometheus là một bộ công cụ giám sát và cảnh báo hệ thống mã nguồn mở
- Có khả năng thu thập số liệu (metric) từ các mục tiêu được cấu hình theo các khoảng thời gian nhất định, hiển thị kết quả, kích hoạt cảnh báo nếu một số điều kiện thỏa mãn yêu cầu
## 2. Một số tính năng
- Mô hình dữ liệu đa chiều(time series): được xác định bởi tên của metric và các cặp key-value
- Ngôn ngữ truy vấn linh hoạt
- Hỗ trợ nhiều chế độ biểu đồ
- Nhiều chương trình tích hợp và hỗ trợ bởi bên thứ 3
- Hoạt động cảnh báo vấn đề linh động dễ cấu hình
- Chỉ cần 1 máy chủ là có thể hoạt động được
- Hỗ trợ push các time series thông qua một gateway trung gian
- Các máy chủ/thiết bị giám sát có thể được phát hiện thông qua service discovery hoặc cấu hình tĩnh
## 3. Một số đặc điểm lưu ý
- Prometheus không dùng để lấy dữ liệu log, thay vì vậy thì Prometheus là dịch vụ giám sát, thu thập và xử lý dữ liệu dạng metric (thông số).
- Prometheus sử dụng cơ chế đi lấy (pull) dữ liệu từ máy chủ remote là chính, chứ không sử dụng cơ chế đợi remote đẩy (push) dữ liệu lên ngoại trừ trường hợp sử dụng PushGateway.
- Prometheus sử dụng chương trình cảnh báo Alertmanager để xử lý và gửi cảnh báo đi.
- Prometheus sử dụng mã nguồn Grafana để tích hợp hiển thị biểu đồ dữ liệu.
- Metric của Prometheus sử dụng chuẩn OpenMetrics.
- Prometheus hỗ trợ 3 hình thức cài đặt các thành phần hệ thống gồm : Docker Image, cài đặt từ source với Go và file chương trình chạy sẵn đã được biên dịch sẵn.
## 4. Kiến trúc Prometheus
![image](https://user-images.githubusercontent.com/92737759/176344093-a047f96b-d621-4640-9b8b-a7857a1ca2a7.png)  
- Prometheus thực hiện quá trình lấy các thông số/số liệu (metric) từ các job được chỉ định qua kênh trực tiếp hoặc thông qua dịch vụ Pushgateway trung gian. 
- Prometheus sẽ lưu trữ các dữ liệu thu thập được ở local máy chủ. Tiếp đến sẽ chạy các rule để xử lý các dữ liệu theo nhu cầu cũng như kiểm tra thực hiện các cảnh báo.
## 5. Các thành phần chính trong hệ thống Prometheus
- Máy chủ Prometheus đảm nhận việc lấy dữ liệu và lưu trữ dữ liệu time-series
- Thư viện client cho các ứng dụng
- Push Gateway Prometheus: Hỗ trợ các job có thời gian thực hiện ngắn (tạm thời).  Đơn giản là các tác vụ công việc này không tồn tại lâu đủ để Prometheus chủ động lấy dữ liệu. Vì vậy là mà các dữ liệu chỉ số (metric) sẽ được đẩy về Push Gateway rồi đẩy về Prometheus Server.
- Đa dạng Exporter hỗ trợ giám sát các dịch vụ hệ thống và gửi về Prometheus mong muốn
- AlertManager: Dịch vụ quản lý, xử lý các cảnh báo (alert)
## 6. Một số thuật ngữ 
- **Time-series Data**: Một chuỗi các điểm dữ liệu,bao gồm các phép đo liên tiếp được thực hiện từ cùng một nguồn trong một khoảng thời gian
- **Alert**: một cảnh báo (alert) là kết quả của việc đạt điều kiện thoả mãn một rule cảnh báo được cấu hình trong Prometheus. Các cảnh báo sẽ được gửi đến dịch vụ Alertmanager.
- **Alertmanager**: chương trình đảm nhận nhiệm vụ tiếp nhận, xử lý các hoạt động cảnh báo.
- **Client Library**: một số thư viện hỗ trợ người dùng có thể tự tuỳ chỉnh lập trình phương thức riêng để lấy dữ liệu từ hệ thống và đẩy dữ liệu metric về Prometheus.
- **Endpoint**: nguồn dữ liệu của các chỉ số (metric) mà Prometheus sẽ đi lấy thông tin
- **Exporter**: thu thập, chuyển đổi các metric không ở dạng kiểu dữ liệu chuẩn Prometheus sang chuẩn dữ liệu Prometheus. Sau đấy exporter sẽ expose web service api chứa thông tin các metrics hoặc đẩy về Prometheus.
- **Instance**: một instance là một nhãn (label) dùng để định danh duy nhất cho một target trong một job.
- **Job**: à một tập hợp các target chung một nhóm mục đích. Ví dụ: giám sát một nhóm các dịch vụ database,… thì ta gọi đó là một job.
- **PromQL**: promql là viết tắt của Prometheus Query Language, ngôn ngữ này cho phép bạn thực hiện các hoạt động liên quan đến dữ liệu metric.
- **Sample**: sample là một giá trị đơn lẻ tại một thời điểm thời gian trong khoảng thời gian time series.
- **Target**: một target là định nghĩa một đối tượng sẽ được Prometheus đi lấy dữ liệu (scrape). Ví dụ như: nhãn nào sẽ được sử dụng cho đối tượng, hình thức chứng thực nào sử dụng hoặc các thông tin cần thiết để quá trình đi lấy dữ liệu ở đối tượng được diễn ra.
## 7.Setup
### Step 1: Clone kube-prometheus project
- Sử dụng git command để clone kube-prometheus về máy  
`git clone https://github.com/prometheus-operator/kube-prometheus.git`  
`cd kube-prometheus`  
- Nếu hệ thống máy không có mạng ngoài cần clone qua hệ thống máy khác và scp sang cụm cần cài đặt:
`scp kube-prometheus.tar vht@172.21.5.17:~/`  
![image](https://user-images.githubusercontent.com/92737759/176376395-d25cbe65-528b-43af-9abb-4e35dce46df5.png)
### Step 2: Create monitoring namespace, CustomResourceDefinitions & operator pod
`kubectl create -f manifests/setup`
![image](https://user-images.githubusercontent.com/92737759/176378572-e51284ab-29fa-4d38-b660-2c58c7057c2d.png)
- Xác nhận các pods đang chạy:  
![image](https://user-images.githubusercontent.com/92737759/176382832-006eb7d4-8663-486b-abfd-d941ea835302.png)
- Fix lỗi:








