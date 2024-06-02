# Mục lục
# Sơ lược Microservice architecture và Monolithic architecture
- Software architecture là tổ chức hệ thống bao gồm rất nhiều các thành phần như Web Server, database, bộ nhớ và các lớp layer thực hiện việc giao tiếp. Chúng liên kết với nhau hoặc với một môi trường nhất định. Mục tiêu cuối cùng của thiết kế hệ thống (system architecture) là giải quyết vấn đề của doanh nghiệp. Và Microservice, Monolithic chính là 2 mô hình pattern của software architecture đang phổ biến hiện nay.
- Monolithic, hiểu đơn giản toàn bộ code được đóng gói và phát triển trên duy nhất một project/source code. Điều này dẫn đến vấn đề là gây khó khăn và tốn sức cho việc scale, update hệ thống, không đảm bảo tính sẵn sàng. Nó không phù hợp nếu hệ thống có quy mô lớn, cũng gây khó khăn cho các kỹ sư khi update code. 
- Microservices là một kiến trúc phần mềm mà ứng dụng được phân chia thành các thành phần nhỏ và độc lập, được gọi là microservices. Mỗi microservice thực hiện một chức năng cụ thể của ứng dụng và có thể được triển khai, mở rộng và quản lý độc lập. Microservices thường được đóng gói vào các container để cung cấp tính di động, sẵn sàng cao và đảm bảo tính nhất quán giữa môi trường phát triển và môi trường triển khai. Kiến trúc phù hợp với các hệ thống có quy mô lớn.
# Tổng quan về đồ án
Ở phạm vi đồ án, chúng tôi tìm hiểu và triển khai security features trong microservice. Tiếp tục, chúng tôi lựa chọn Kubescape làm công cụ để kiểm thử mức độ an toàn của triển khai. Đồ án tập trung khai thác Kubescape để hiểu sâu hơn và áp dụng được những đóng góp của tool để nâng cao khả năng bảo mật của hệ thống. Có 3 công nghệ chúng tôi sử dụng: Docker, Minikube, Kubescape.
## Mối quan hệ giữa Docker và Microservices
Docker cung cấp một cách để đóng gói và chạy các microservice vào các container. Bằng cách sử dụng Docker, các nhà phát triển có thể đóng gói mỗi microservice và tất cả các phụ thuộc của nó (bao gồm mã nguồn, thư viện, và cấu hình) vào một container, giúp cho việc phát triển, thử nghiệm và triển khai microservices trở nên dễ dàng hơn. 
## Kubernetes và Docker
Kubernetes không phải là một nền tảng container như Docker, thay vào đó, nó quản lý các container được triển khai trong một môi trường phân tán. Kubernetes có khả năng tương tác với các container runtime như Docker để triển khai và quản lý các container chứa microservices. Kubernetes không bắt buộc việc sử dụng Docker, nhưng Docker vẫn là một trong những container runtime phổ biến được sử dụng bởi Kubernetes.
# Docker
## Khái niệm Docker
- Docker là một nền tảng phần mềm cho phép chúng ta chạy, kiểm thử và triển khai ứng dụng một cách nhanh chóng trên các môi trường, phiên bản và hệ điều hành khác nhau. Docker tạo ra các **môi trường cách ly được gọi là container** và **đóng gói** tất cả những gì cần thiết để ứng dụng của chúng ta có thể chạy được vào trong containers này.
- Ví dụ: Bạn nấu món bún bò rất ngon và được bạn bè hỏi xin cách làm. Có 2 cách: 1 là bạn sẽ cho họ công thức, nhưng khá phức tạp để làm theo; do đó bạn chọn cách 2: bạn chuẩn bị sẵn 1 túi bún, 1 túi rau, 1 túi chả và thịt bò, nấu sẵn nước dùng, sau đó đóng gói tất cả vào 1 thùng lớn và ship đi. Họ nhận được, để sử dụng thì chỉ cần trộn tất cả vào nhau. Bằng cách này, bạn bè đã có thể thưởng thức món bún bò với cùng hương vị mà bạn đã ăn. Thùng lớn chứa tất cả nguyên liệu của món bún bò chính là hình ảnh của Docker Container.

## Images
- Docker image là một **template** chỉ đọc (read-only) **dùng để tạo container**. Image là **tập tin** chứa mọi thứ cần thiết để chạy một ứng dụng, bao gồm mã nguồn, thư viện, biến môi trường, và các tệp cấu hình. Image file thường được tải xuống từ một registry như Docker Hub hoặc tạo ra từ một Dockerfile - định nghĩa cách ứng dụng được cấu hình và triển khai. Mỗi lệnh trong Dockerfile sẽ tạo một lớp trong hình ảnh. Khi bạn thay đổi Dockerfile và xây dựng lại hình ảnh, chỉ những lớp đã thay đổi mới được xây dựng lại. Đây là một phần lý do khiến hình ảnh trở nên nhẹ, nhỏ và nhanh khi so sánh với các công nghệ ảo hóa khác.
- Ví dụ: Dockerfile là công thức nấu bún bò, Image là món bún bò, Container là các suất bún bò được tạo ra từ cùng một công thức bún bò nhưng khô/nước, có/không có ớt thì tùy thuộc vào từng cài đặt cụ thể.
- Images thuận tiện vì dễ dàng di chuyển, ví dụ bạn đang chạy nginx và muốn share nó cho người bạn của mình, thì bạn chỉ cần "đóng gói" nginx vào image và gửi file đi là được!
- Tóm lại là, theo anh Khalid Dinh, ta có hình để phân biệt như sau:
  
![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/60993b4c-1be3-4530-a3a6-7159893ff4b2)

## Container
- Docker cung cấp khả năng đóng gói và chạy ứng dụng trong một môi trường cách ly gọi là container. Việc cách ly và bảo mật này cho phép bạn chạy nhiều container đồng thời trên một máy chủ nhất định. Các container nhẹ và chứa mọi thứ cần thiết để chạy ứng dụng, do đó bạn không cần phải phụ thuộc vào những gì được cài đặt trên máy chủ. Bạn có thể chia sẻ các container trong quá trình làm việc và đảm bảo rằng mọi người bạn chia sẻ đều nhận được cùng một container hoạt động theo cách giống nhau.
- Điều quan trọng là mỗi container có thể được viết bằng một ngôn ngữ khác nhau, container A dùng Java, B dùng Python, C dùng NodeJS nhưng sẽ không vấn đề gì vì nó được chạy trong môi trường riêng biệt và độc lập.

## Docker architecture
<img src="https://docs.docker.com/get-started/images/docker-architecture.webp">

Chúng ta sẽ quan tâm đến 3 thành phần chính:
- Docker Client: tiếp nhận câu lệnh từ phía user như `docker run` hay `docker pull`. Sau đó gửi đến dockerrd ( Docker Daemon - nằm trên docker server) để xử lý các yêu cầu.
- Docker Server: lắng nghe các yêu cầu Docker API và quản lý các đối tượng Docker như hình ảnh, vùng chứa, mạng và ổ đĩa. Kết quả cuối cùng là tạo ra các containers.
- Docker Registry: lưu trữ images Docker và một kho ứng dụng.

# Kubernetes
## Khái niệm Kubernetes
- Kubernetes (viết tắt là K8s) là một hệ thống mã nguồn mở được phát triển bởi Google, dùng để tự động hóa triển khai, mở rộng và quản lý các ứng dụng được đóng gói trong các container. Kubernetes cung cấp một cách để quản lý các ứng dụng được triển khai trong các môi trường đám mây, dữ liệu trung tâm hoặc trên máy tính cá nhân.
- Kubernetes cung cấp các tính năng như tự động cân bằng tải, tự động khôi phục sau lỗi và tự động mở rộng để giúp quản lý một lượng lớn các microservices một cách hiệu quả và đáng tin cậy.
## Các components quan trọng trong K8s

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
