# Mục lục
# A. Sơ lược Microservice architecture và Monolithic architecture
- Software architecture là tổ chức hệ thống bao gồm rất nhiều các thành phần như Web Server, database, bộ nhớ và các lớp layer thực hiện việc giao tiếp. Chúng liên kết với nhau hoặc với một môi trường nhất định. Mục tiêu cuối cùng của thiết kế hệ thống (system architecture) là giải quyết vấn đề của doanh nghiệp. Và Microservice, Monolithic chính là 2 mô hình pattern của software architecture đang phổ biến hiện nay.
- Monolithic, hiểu đơn giản toàn bộ code được đóng gói và phát triển trên duy nhất một project/source code. Điều này dẫn đến vấn đề là gây khó khăn và tốn sức cho việc scale, update hệ thống, không đảm bảo tính sẵn sàng. Nó không phù hợp nếu hệ thống có quy mô lớn, cũng gây khó khăn cho các kỹ sư khi update code. 
- Microservices là một kiến trúc phần mềm mà ứng dụng được phân chia thành các thành phần nhỏ và độc lập, được gọi là microservices. Mỗi microservice thực hiện một chức năng cụ thể của ứng dụng và có thể được triển khai, mở rộng và quản lý độc lập. Microservices thường được đóng gói vào các container để cung cấp tính di động, sẵn sàng cao và đảm bảo tính nhất quán giữa môi trường phát triển và môi trường triển khai. Kiến trúc phù hợp với các hệ thống có quy mô lớn.
# B. Tổng quan về đồ án
Ở phạm vi đồ án, chúng tôi tìm hiểu và triển khai security features trong microservice. Tiếp tục, chúng tôi lựa chọn Kubescape làm công cụ để kiểm thử mức độ an toàn của triển khai. Đồ án tập trung khai thác Kubescape để hiểu sâu hơn và áp dụng được những đóng góp của tool để nâng cao khả năng bảo mật của hệ thống. Có 3 công nghệ chúng tôi sử dụng: Docker, Minikube, Kubescape.

# C. Docker
## C.1. Khái niệm Docker
- Docker là một **container runtime**, một nền tảng phần mềm cho phép chúng ta chạy, kiểm thử và triển khai ứng dụng một cách nhanh chóng trên các môi trường, phiên bản và hệ điều hành khác nhau. Docker tạo ra các **môi trường cách ly được gọi là container** và **đóng gói** tất cả những gì cần thiết để ứng dụng của chúng ta có thể chạy được vào trong containers này.
- Ví dụ: Bạn nấu món bún bò rất ngon và được bạn bè hỏi xin cách làm. Có 2 cách: 1 là bạn sẽ cho họ công thức, nhưng khá phức tạp để làm theo; do đó bạn chọn cách 2: bạn chuẩn bị sẵn 1 túi bún, 1 túi rau, 1 túi chả và thịt bò, nấu sẵn nước dùng, sau đó đóng gói tất cả vào 1 thùng lớn và ship đi. Họ nhận được, để sử dụng thì chỉ cần trộn tất cả vào nhau. Bằng cách này, bạn bè đã có thể thưởng thức món bún bò với cùng hương vị mà bạn đã ăn. Thùng lớn chứa tất cả nguyên liệu của món bún bò chính là hình ảnh của Docker Container.

## C.2. Images
- Docker image là một **template** chỉ đọc (read-only) **dùng để tạo container**. Image là **tập tin** chứa mọi thứ cần thiết để chạy một ứng dụng, bao gồm mã nguồn, thư viện, biến môi trường, và các tệp cấu hình. Image file thường được tải xuống từ một registry như Docker Hub hoặc tạo ra từ một Dockerfile - định nghĩa cách ứng dụng được cấu hình và triển khai. Mỗi lệnh trong Dockerfile sẽ tạo một lớp trong hình ảnh. Khi bạn thay đổi Dockerfile và xây dựng lại hình ảnh, chỉ những lớp đã thay đổi mới được xây dựng lại. Đây là một phần lý do khiến hình ảnh trở nên nhẹ, nhỏ và nhanh khi so sánh với các công nghệ ảo hóa khác.
- Ví dụ: Dockerfile là công thức nấu bún bò, Image là món bún bò, Container là các suất bún bò được tạo ra từ cùng một công thức bún bò nhưng khô/nước, có/không có ớt thì tùy thuộc vào từng cài đặt cụ thể.
- Images thuận tiện vì dễ dàng di chuyển, ví dụ bạn đang chạy nginx và muốn share nó cho người bạn của mình, thì bạn chỉ cần "đóng gói" nginx vào image và gửi file đi là được!
- Tóm lại là, theo anh Khalid Dinh, ta có hình để phân biệt như sau:
  
![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/60993b4c-1be3-4530-a3a6-7159893ff4b2)

## C.3. Container
- Docker cung cấp khả năng đóng gói và chạy ứng dụng trong một môi trường cách ly gọi là container. Việc cách ly và bảo mật này cho phép bạn chạy nhiều container đồng thời trên một máy chủ nhất định. Các container nhẹ và chứa mọi thứ cần thiết để chạy ứng dụng, do đó bạn không cần phải phụ thuộc vào những gì được cài đặt trên máy chủ. Bạn có thể chia sẻ các container trong quá trình làm việc và đảm bảo rằng mọi người bạn chia sẻ đều nhận được cùng một container hoạt động theo cách giống nhau.
- Điều quan trọng là mỗi container có thể được viết bằng một ngôn ngữ khác nhau, container A dùng Java, B dùng Python, C dùng NodeJS nhưng sẽ không vấn đề gì vì nó được chạy trong môi trường riêng biệt và độc lập.
- Tại sao cần container?
  
  ⇒ Mỗi service cần được chạy trên 1 thiết bị (1 PC/ laptop/ Cloud VPS…), do đó khi triển khai sẽ rất tốn kém.
  
  ⇒ Giải quyết: sử dụng containerized application (điển hình là Docker), một container sẽ bao gồm 1 hoặc nhiều services.
- Làm cách nào 1 container có thể thông báo cho các container khác khi nó có sự update?

  ⇒ Nhờ vào Kubenetes nằm trên master node. Tìm hiểu kỹ hơn ở mục Kubenetes nhé! 

## C.4. Docker architecture
<img src="https://docs.docker.com/get-started/images/docker-architecture.webp">

Chúng ta sẽ quan tâm đến 3 thành phần chính:
- Docker Client: tiếp nhận câu lệnh từ phía user như `docker run` hay `docker pull`. Sau đó gửi đến dockerrd ( Docker Daemon - nằm trên docker server) để xử lý các yêu cầu.
- Docker Server: lắng nghe các yêu cầu Docker API và quản lý các đối tượng Docker như hình ảnh, vùng chứa, mạng và ổ đĩa. Kết quả cuối cùng là tạo ra các containers.
- Docker Registry: lưu trữ images Docker và một kho ứng dụng.

# D. Kubernetes
## D.1. Khái niệm Kubernetes
- Kubernetes (viết tắt là K8s) là một hệ thống mã nguồn mở được phát triển bởi Google, dùng để tự động hóa triển khai, mở rộng và quản lý các ứng dụng được đóng gói trong các container.
- Nhưng Kubernetes không chỉ là một **công cụ điều phối container (container orchestration platform)**. Nó có thể được coi là **hệ điều hành dành cho các ứng dụng gốc đám mây (cloud-native applications)** theo nghĩa nó là nền tảng mà các ứng dụng chạy trên đó, giống như các ứng dụng máy tính chạy trên MacOS, Windows hoặc Linux.
- K8s đảm bảo cho các service hoạt động trơn tru theo hướng dẫn từ file config - có đuôi yaml hoặc yml.
- Kubernetes cung cấp các tính năng như tự động cân bằng tải, tự động khôi phục sau lỗi và tự động mở rộng để giúp quản lý một lượng lớn các microservices một cách hiệu quả và đáng tin cậy.
- Một hình ảnh ẩn dụ dễ hiểu: K8s giống như nhạc trưởng **điều phối** các container là các nhạc công, file config là nhà soạn nhạc.
## D.2. Kiến trúc Kubenetes

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/6469bffd-05d7-4fde-b76f-755f2fecda07)

## D.3. Các khái niệm quan trọng trong K8s
### D.3.1. Master Nodes
- Control plane là hệ thống duy trì bản ghi của tất cả các đối tượng Kubernetes. Nó liên tục quản lý các trạng thái của đối tượng, phản ứng với những thay đổi trong cụm; nó cũng giúp làm cho trạng thái thực tế của các đối tượng hệ thống khớp với trạng thái mong muốn.
- Control plane được tạo thành từ ba thành phần chính: kube-apiserver, kube-controller-manager và kube-scheduler. Tất cả đều có thể chạy trên một nút chính hoặc có thể được sao chép trên nhiều nút chính để có tính sẵn sàng cao.
  - API server: cung cấp các API để hỗ trợ điều phối vòng đời (mở rộng quy mô, cập nhật,...) cho các loại ứng dụng khác nhau. Nó cũng hoạt động như **một cổng vào cụm**, vì vậy API server phải có thể truy cập được bởi các máy khách từ bên ngoài cụm. Khách hàng xác thực thông qua API server và cũng sử dụng nó làm proxy/tunnel cho các nodes và pods (và services).
  - Scheduler: Bộ lập lịch chịu trách nhiệm lập lịch cho các pods trong worker nodes; nó tính đến nhiều ràng buộc khác nhau, chẳng hạn như các giới hạn hoặc đảm bảo về tài nguyên cũng như các thông số kỹ thuật.
  - Controller manager: chạy controllers để điều khiển trạng thái của cluster.
  - etcd: lưu trữ, truy xuất thông tin về cụm.
### D.3.2. Cluster Nodes (kubelet)

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/f3d1434a-792e-4624-86e2-6c53b9b10c98)

- Cluster nodes là các máy chạy containers và được quản lý bởi master nodes. **Kubelet là bộ điều khiển chính và quan trọng nhất trong Kubernetes**. Nó chịu trách nhiệm điều khiển lớp thực thi container.
- Có thể là máy ảo hoặc máy vật lý, tuỳ thuộc vào cluster.
- Một Node có thể chứa nhiều Pods và Kubernetes master tự động xử lí việc lên lịch trình các Pods thuộc các Nodes ở trong cluster. Việc lên lịch trình tự động của Master sẽ tính đến các tài nguyên có sẵn trên mỗi Node.
- Mỗi Node ở Kubernetes chạy ít nhất:
  - Kubelet: một quy trình chịu trách nhiệm liên lạc giữa Kubernetes Master và Worker Node thông qua Control Plane; nhận hướng dẫn để biết cần chạy Pods nào và duy trì trạng thái của Pods.
  - Một container runtime (như Docker, rkt): chịu trách nhiệm lấy container image từ registry, giải nén container và chạy các containers. Các containers chỉ nên được lên lịch trình cùng nhau trong một Pod duy nhất nếu chúng được liên kết chặt chẽ.

#### D.3.2.1. Pods

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/cd63276c-7558-459a-a6bd-f4b59d33c152)

- Một Pod luôn chạy trên một Node.
- Pod là đơn vị nhỏ nhất trong K8s, được sử dụng để lưu **phiên bản chạy của ứng dụng**.
- Pod đại diện cho một nhóm gồm một hoặc nhiều ứng dụng containers (ví dụ như Docker hoặc rkt) và một số tài nguyên được chia sẻ cho các containers đó. Những tài nguyên đó bao gồm:
  - Pod mount volumes: Về cốt lõi, một volume chỉ là một thư mục, có thể chứa một số dữ liệu trong đó, được chia sẻ giữa các containers.
  - Kết nối mạng, mối Pod có 1 địa chỉ IP cố định được sử dụng trong nội bộ cluster. Mỗi container trong pod kết nối bằng localhost trên các cổng khác nhau. Các Pods giao tiếp với nhau bằng địa chỉ IP. 
  - Thông tin về cách chạy từng container, chẳng hạn như phiên bản container image hoặc các ports cụ thể để sử dụng.
    
#### D.3.2.2. Ingress và Service

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/29955758-db06-4928-b224-afce91ebb3cb)

- Tại sao cần Ingress và Service?
   Vì Pods có IP cố định nên khi 1 Pod chết, Pod khác thay thế nhưng IP cũng thay đổi, dẫn đến cần chỉnh lại cấu hình, và việc này gây mất thời gian.

    => Giải quyết: sử dụng service và ingress.
  
- Ingrees:
  - Quản lý và kiểm soát truy cập bên ngoài vào các dịch vụ trong cụm K8s.
  - Ingress là cầu nối giữa request bên ngoài với service trong cụm.
- Service:
  - IP cố định.
  - Tạo ra 1 tập hợp các endpoints (bao gồm port và IP) để kết nối các pods trong 1 cụm K8s. 

#### D.3.2.3. Kube-proxy
Trên Worker Node còn có Kube-proxy với 2 chức năng:
  - Định tuyến traffic đến đúng Pods.
  - Cung cấp Load Balancing cho Pods và đảm bảo traffic được phân bổ đồng đều cho các Pods.

## D.4. OWASP (Open Web Application Security Project) Kubernetes Top 10 

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/4b78c437-af07-4f8c-94cb-5d3775434e58)

- Link bài gốc: https://medium.com/@seifeddinerajhi/owasp-kubernetes-top-10-a-comprehensive-guide-f03af6fd66ed

# Summary: Kiến trúc K8s + Docker

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/9871cfab-ebc8-4d5a-81d7-556960da6758)


# E. Minikube

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/8b54896c-27cc-4079-8964-6afb2f0b6b90)


- Minikube là cluster bao gồm một master processes và một worker processes chạy trên cùng một node. Minikube sẽ tạo một virtual box trong máy tính local và 1 node như hình trên sẽ được chạy trong virtual box này.
- Các bạn có thể tham khảo cách cài đặt và triển khai node minikube cơ bản tại: https://minikube.sigs.k8s.io/docs/start/
  
# F. Kubescape
## F.1. Tổng quan về Kubescape
- Kubescape là một nền tảng bảo mật Kubernetes mã nguồn mở. Nhắm mục tiêu vào người thực hành DevSecOps hoặc kỹ sư nền tảng, nó cung cấp giao diện CLI dễ sử dụng, định dạng đầu ra linh hoạt và khả năng quét tự động. Nó giúp người dùng và quản trị viên Kubernetes tiết kiệm thời gian, công sức và tài nguyên quý giá.
- Kubescape có thể **chạy dưới dạng một bộ các microservices bên trong cluster K8s**. Điều này cho phép bạn liên tục theo dõi trạng thái của một cụm.
- Kubescape scans:
  - Cluster.
  - Manifest files (YAML files, Helm Chart).
  - Code repositories.
  - Container registries.
  - Images.

    => Phát hiện: cấu hình sai thông qua 3 khung NSA, MITRE, CIS; lỗ hổng phần mềm và tính toán được mức điểm của risks.
    
- Chi tiết 3 khung mà Kubescape sử dụng:
  - NSA-CISA (National Security Agency - Cybersecurity and Infrastructure Security Agency): gồm 24 controls, đề cập những vấn đề về: cấu hình cluster, quyền truy cập và xác thực, pod và container, bảo mật mạng, quản lý bản vá.
  - MITRE (dựa trên MITRE ATT&CK): tập trung vào các chiến thuật, kỹ thuật và quy trình đã biết liên quan đến các cuộc tấn công mạng => Tạo ra ma trận mối đe dọa (threat matrix):

    ![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/d43932b0-e801-4210-91ff-d8b3514de5a9)

  - CIS (center for information security): một bộ cấu hình an toàn cho K8s: control plane components, etcd, scheduler, policies,... Gồm 3 versions:
    - Default K8S cluster: cis-v1.23-t1.0.1
    - Azure K8s service: cis-aks-t1.2.0
    - Amazon Elastic K8s service: cis-eks-t1.2.0 
## F.2. Vị trí của Kubescape trong kiến trúc
## F.3. Vulnerability scanning
- Mục này xuất hiện là vì thầy của mình yêu cầu nhóm focus vào vấn đề này, link gốc các bạn có thể xem tại https://kubescape.io/docs/operator/vulnerabilities/
- 
## ARMO Platform

- ARMO Platform là một giải pháp SaaS (Software as a service).
- Kubescape được tích hợp thêm ARMO Platform sẽ giúp nhìn kết quả scan trực quan, dễ dàng hơn:

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/44feac88-e164-40b1-8c74-ce261edd832d)

# G. Kiến trúc trong đồ án: Minikube + Docker + Kubescape

![image](https://github.com/Roses21/NT534.O21.ANTN-GROUP9-Kubescape/assets/147015288/9c7b99cf-e8d4-4eab-8f04-dd50d066414f)

https://www.linkedin.com/pulse/deploying-container-kubernetes-biswajit-patnaik/ 
# H. Kết quả thực hiện
Chúng tôi thực hiện 5 kịch bản demo, sẽ xoay quanh những tính năng mạnh mẽ và nổi bật của Kubescape. Bạn có thể xem video https://youtu.be/B7eazHTsIv8?si=-PSzdXG9JzmCJJGx để hiểu rõ hơn, đây là video được tài trợ bởi ARMO Teams nên nó đáng để xem đấy!
## Kịch bản 1: Setting CPU and memory limits
- Mô tả: quan trọng khi đảm bảo rằng giới hạn bộ nhớ đã được thiết lập cho các container trong Kubernetes vì nó giúp:
    
    - **Quản lý tài nguyên**: hệ thống biết được mức bộ nhớ tối đa mà mỗi container có thể sử dụng, từ đó tránh được việc sử dụng quá nhiều bộ nhớ và gây ra hiện tượng quá tải (overload).
    - **Phòng tránh sự cạnh tranh tài nguyên**: tránh tình trạng một container chiếm hết tài nguyên và ảnh hưởng đến hiệu suất của các container khác.
    - **Phòng tránh tình trạng OOM (Out Of Memory)**.
    - **Dễ dàng quản lý hiệu suất và khắc phục sự cố**: Khi giới hạn bộ nhớ được đặt, bạn có thể dễ dàng theo dõi và quản lý hiệu suất của container. Nếu có vấn đề về bộ nhớ, bạn có thể dễ dàng xác định container nào gây ra vấn đề và thực hiện các biện pháp khắc phục một cách hiệu quả.
- Làm sao để sửa lỗi?
  
  => Cách sửa: Giới hạn memory bằng cách chỉnh sửa file cấu hình YAML của đối tượng. Minikube Dashboard cung cấp một giao diện đồ họa tiện lợi để quản lý và giám sát cluster Kubernetes của bạn trong môi trường Minikube. Do đó, sửa lại cấu hình thông qua giao diện GUI của dashboard.

## Kịch bản 2: Configure Security Context to prevent Escalating Privilege

## Kịch bản 3: Kubescape in CI/CD using Github Actions
- Tham khảo: https://hub.armosec.io/docs/github-1
- Mô tả: Kịch bản này thảo luận về cách GitHub Actions có thể tăng cường tính bảo mật của quy trình CI/CD bằng cách tự động hóa các tác vụ liên quan đến bảo mật và cung cấp khả năng tích hợp với các công cụ bảo mật, kiểm soát phiên bản, kiểm soát truy cập và kiểm tra khác.

Một thành phần quan trọng của quy trình phát triển phần mềm là quy trình tích hợp và triển khai liên tục (CI/CD), tự động hóa việc xây dựng, thử nghiệm và triển khai phần mềm. Tuy nhiên, quy trình CI/CD cũng dễ bị đe dọa bởi các mối đe dọa bảo mật và việc vi phạm bảo mật trong quy trình có thể gây ra hậu quả nghiêm trọng cho tổ chức. May mắn thay, hiện có sẵn các công cụ có thể giúp cải thiện tính bảo mật của quy trình CI/CD và đảm bảo quá trình xây dựng an toàn.

## Kịch bản 4: Creating your own Framework/Controls
- Mô tả: Những frameworks tiêu chuẩn như NSA, MITREE, CIS rất tốt nhưng nó có rất nhiều yêu cầu cần phải đáp ứng. Trong khi đó, đối với từng cụm K8s, sẽ có nhiều yêu cầu không cần thiết, hơn nữa, có nhiều vul không thể fix được. Và việc tạo một framework mới sẽ giúp Kubescape tập trung đánh giá những khía cạnh quan trọng đối với triển khai của bạn.
- Tham khảo: https://hub.armosec.io/docs/creating-your-own-controls
## Kịch bản 5: Registry Scanning & Repository Scanning
  
