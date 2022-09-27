## 1. Overview
- Ory là một phần mềm mã nguồn mở và cung cấp các dịch vụ và API giúp xác thực, ủy quyền, kiểm soát truy cập người dùng (OAuth2 & OpenID Connect)
- Gồm 4 thành phần chính:
  + Ory Kratos: quản lý danh tính và nhận dạng người dùng dựa trên API
    * Dịch vụ tự đăng ký, đăng nhập tài khoản
    * Xác thực đa yếu tố
    * Xác minh tài khoản
    * Khôi phục tài khoản
    * Quản lý hồ sơ và tài khoản người dùng
    * Quản trị các APIs: Thêm, cập nhật, xóa danh tính
  + Ory Oathkeeper: Điều phối mọi truy cập, các request đi từ keto, hydra đều qua oathkeeper
  + Ory Keto: Quản lý truy cập và phân quyền cho người dùng
  + Ory Hydra: Cung cấp các chuẩn OAuth 2.0 và kết nối OpenID. File chứa các token cho phép ngườ dùng thứ 3 truy cập vào
## 2. Install tutorial
### 2.1. Oathkeeper
![image](https://user-images.githubusercontent.com/92737759/170169778-930bfc4d-2998-4e72-8786-bc76b09925a7.png)
![image](https://user-images.githubusercontent.com/92737759/170171124-326619ae-d91c-4c2b-92ab-5d9944e46222.png)
`helm install oathkeeper -n ory .`
### 2.2. Hydra
![image](https://user-images.githubusercontent.com/92737759/170167156-6d334180-2c71-4b63-be7a-7896ac2449bd.png)
![image](https://user-images.githubusercontent.com/92737759/170166922-e58d3dc4-d234-44a8-9435-997b6e348069.png)
![image](https://user-images.githubusercontent.com/92737759/170167033-3cc62daf-27c8-4899-990e-329fb22ce1cb.png)  
`helm install hydra -n ory .`  
`helm install hydra -f values.yaml -n ory .`  
- Cần tạo một database làm nơi lưu trữ dữ liệu:
`CREATE DATABASE hydra;`
```
k delete pod -n ory hydra-d9fbdb4f-8m9g6
clear
k get pod -n ory
```
```
helm uninstall hydra -n ory
helm install hydra -n ory
k get pod -n ory
helm upgrade --install hydra -n ory .
watch kubectl get pod -n ory
helm upgrade --install hydra -n ory .
```
### 2.3. Keto
![image](https://user-images.githubusercontent.com/92737759/170172185-6b142976-d6e0-4b70-8151-54abb4299c33.png)
```
helm install keto -n ory .
helm upgrade --install keto -n ory .
kubectl get pod -n ory
helm uninstall keto -n ory
helm install keto -n ory .
```
- Cần tạo một database lưu trữ dữ liệu
`CREATE DATABASE keto;`


