# Khái niệm về K8S
- Kubernetes (viết tắt là K8s) là một hệ thống mã nguồn mở được phát triển bởi Google, dùng để tự động hóa triển khai, mở rộng và quản lý các ứng dụng được đóng gói trong các container. Kubernetes cung cấp một cách để quản lý các ứng dụng được triển khai trong các môi trường đám mây, dữ liệu trung tâm hoặc trên máy tính cá nhân.
- Microservices là một kiến trúc phần mềm mà ứng dụng được phân chia thành các thành phần nhỏ và độc lập, được gọi là microservices. Mỗi microservice thực hiện một chức năng cụ thể của ứng dụng và có thể được triển khai, mở rộng và quản lý độc lập. Microservices thường được đóng gói vào các container để cung cấp tính di động và đảm bảo tính nhất quán giữa môi trường phát triển và môi trường triển khai.
- Mối quan hệ giữa Docker và Microservices: Docker cung cấp một cách để đóng gói và chạy các microservice vào các container. Bằng cách sử dụng Docker, các nhà phát triển có thể đóng gói mỗi microservice và tất cả các phụ thuộc của nó (bao gồm mã nguồn, thư viện, và cấu hình) vào một container, giúp cho việc phát triển, thử nghiệm và triển khai microservices trở nên dễ dàng hơn. Kubernetes cung cấp các tính năng như tự động cân bằng tải, tự động khôi phục sau lỗi và tự động mở rộng để giúp quản lý một lượng lớn các microservices một cách hiệu quả và đáng tin cậy.
- Kubernetes và Docker: Kubernetes không phải là một nền tảng container như Docker, thay vào đó, nó quản lý các container được triển khai trong một môi trường phân tán. Kubernetes có khả năng tương tác với các container runtime như Docker để triển khai và quản lý các container chứa microservices. Kubernetes không bắt buộc việc sử dụng Docker, nhưng Docker vẫn là một trong những container runtime phổ biến được sử dụng bởi Kubernetes.
# Các components quan trọng trong K8s
# Kubescape
- Kubescape là một nền tảng bảo mật Kubernetes mã nguồn mở. Nó bao gồm phân tích rủi ro, tuân thủ bảo mật và quét cấu hình sai. Nhắm mục tiêu vào người thực hành DevSecOps hoặc kỹ sư nền tảng, nó cung cấp giao diện CLI dễ sử dụng, định dạng đầu ra linh hoạt và khả năng quét tự động. Nó giúp người dùng và quản trị viên Kubernetes tiết kiệm thời gian, công sức và tài nguyên quý giá.
- Kubescape quét các cụm (cluster), tệp YAML và biểu đồ Helm. Nó phát hiện các cấu hình sai theo nhiều khung (bao gồm NSA-CISA, MITER ATT&CK® và CIS Benchmark).
# Cài đặt Minikube và triển khai Kubescape
## 1. Minikube: 
- Trong phạm vi đồ án này, chúng tôi sẽ triển khai Minikube. Minikube cho phép bạn triển khai một cụm Kubernetes đơn giản trên máy tính của mình thông qua một máy ảo cụ thể, chẳng hạn như VirtualBox hoặc Docker (tôi sẽ dùng Docker). Nó cung cấp một cách dễ dàng để bạn có thể tạo ra các pods, deployments và services, thử nghiệm các tính năng như load balancing, scaling, và quản lý tài nguyên trong một môi trường cục bộ.
- Các bạn có thể tham khảo cách cài đặt và triển khai node minikube cơ bản tại: https://minikube.sigs.k8s.io/docs/start/ 
## 2. Kubescape: 
Tham khảo tại trang https://kubescape.io/ 
# Kết quả thực hiện
