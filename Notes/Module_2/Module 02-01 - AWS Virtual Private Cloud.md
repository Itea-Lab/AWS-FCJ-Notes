### 1. Giới thiệu chung về AWS VPC
- **VPC (Virtual Private Cloud)** là một dịch vụ cốt lõi của AWS, cho phép người dùng tạo ra một mạng riêng ảo được cô lập logic khỏi các mạng khác trên AWS. Đây là nền tảng quan trọng để triển khai các tài nguyên như máy chủ ảo (EC2), cơ sở dữ liệu, và các dịch vụ khác trong một môi trường mạng riêng tư.
	![](attachments/Pasted%20image%2020250213112200.png)
- **VPC** cho phép người dùng kiểm soát hoàn toàn môi trường mạng của mình, bao gồm việc định nghĩa dải địa chỉ IP, tạo các subnet, cấu hình bảng định tuyến, và thiết lập các cổng kết nối Internet (Internet Gateway) hoặc NAT Gateway.
	![VPC architecture Example](attachments/Pasted%20image%2020250213120133.png)
- **Khác biệt giữa VPC và Private Cloud truyền thống**:
	- **Private Cloud truyền thống**: Thường được xây dựng trên cơ sở hạ tầng vật lý tại chỗ (on-premise), sử dụng hệ thống ảo hóa tập trung để mô phỏng các tính năng của đám mây.
	- **VPC trên AWS**: Là một mạng ảo riêng trong đám mây, không yêu cầu cơ sở hạ tầng vật lý, và được quản lý hoàn toàn bởi AWS.
#### 2. Thực hành và lưu ý
![Caution](attachments/Pasted%20image%2020250213120353.png)
##### **Thực hành**:
  - Để hiểu rõ cách hoạt động của VPC, cần thực hành tạo VPC, subnet, route table, internet gateway, và NAT gateway.
  - Thực hành cấu hình máy chủ trong Public Subnet và Private Subnet để kết nối Internet.
##### **Lưu ý**:
  - VPC chỉ cô lập ở mức mạng, không cô lập hoàn toàn các tài nguyên giữa các VPC.
  - Để cô lập hoàn toàn tài nguyên giữa các môi trường (dev, test, production), cần tạo nhiều AWS account thay vì nhiều VPC.
### **2. Các thành phần chính của VPC**
##### **CIDR (Classless Inter-Domain Routing)**:
  - Khi tạo VPC, bạn cần khai báo một dải địa chỉ IP (IPv4 hoặc IPv6) để định nghĩa không gian mạng của VPC. Ví dụ: `10.10.0.0/16` là một dải địa chỉ IP cho VPC.
  - CIDR giúp xác định phạm vi địa chỉ IP mà VPC có thể sử dụng, và các subnet sẽ được chia nhỏ từ dải địa chỉ này.
##### **Subnet (Mạng con)**:
  - Subnet là một phần của VPC, được chia nhỏ từ dải địa chỉ IP của VPC.
  - Mỗi subnet phải nằm trong một **Availability Zone (AZ)** cụ thể. 
	  - Ví dụ: Subnet A nằm trong AZ 1, Subnet B nằm trong AZ 2.
![Subnet](attachments/Pasted%20image%2020250213135123.png)
  - Subnet có thể là **Public Subnet** (cho phép truy cập Internet) hoặc **Private Subnet** (không cho phép truy cập Internet trực tiếp).
  - AWS giữ lại 5 địa chỉ IP trong mỗi subnet (địa chỉ network, broadcast, router, DNS, v.v.), nên khi tính toán số lượng địa chỉ IP có thể sử dụng, bạn cần trừ đi 5 địa chỉ này.
	![Subnet_2](attachments/Pasted%20image%2020250213135710.png)
##### **Route Table (Bảng định tuyến)**:
![Route_Table](attachments/Pasted%20image%2020250213135955.png)
  - Mỗi VPC có một **Default Route Table** mặc định, không thể xóa, cho phép các subnet trong VPC liên lạc với nhau.
  - Bạn có thể tạo **Custom Route Table** để định tuyến lưu lượng mạng, ví dụ: định tuyến ra Internet thông qua Internet Gateway.
  - Route Table được gán vào các subnet để xác định cách lưu lượng mạng được định tuyến.
##### **Elastic Network Interface (ENI)**:
![ENI](attachments/Pasted%20image%2020250213140317.png)
  - Là card mạng ảo gắn vào máy chủ ảo (EC2), chứa địa chỉ IP private và có thể gán thêm địa chỉ IP public (**Elastic IP**).
	![Elastic_IP](attachments/Pasted%20image%2020250213142957.png)
  - ENI có thể tháo ra và gán vào máy chủ khác mà vẫn giữ nguyên địa chỉ IP.
  - ENI giúp linh hoạt trong việc quản lý địa chỉ IP và kết nối mạng giữa các máy chủ.
  - **Gán Subnet**:
	  ![Public_Subnet](attachments/Pasted%20image%2020250213143332.png)
##### **VPC Endpoint**:
![VPC_EndPoint](attachments/Pasted%20image%2020250213143425.png)
  - Cho phép kết nối từ các tài nguyên trong VPC đến các dịch vụ AWS khác (như S3, DynamoDB) mà không cần đi qua Internet.
  - Có hai loại VPC Endpoint: 
    - **Interface Endpoint**: Sử dụng Elastic Network Interface (ENI) để kết nối đến các dịch vụ hỗ trợ Interface Endpoint.
    - **Gateway Endpoint**: Sử dụng Route Table để định tuyến lưu lượng đến các dịch vụ hỗ trợ Gateway Endpoint (hiện chỉ hỗ trợ S3 và DynamoDB).
- Architecture:
	![VPC_EndPoint_Architecture](attachments/Pasted%20image%2020250213144353.png)
##### Internet Gateway (IGW):
![Internet_Gateway](attachments/Pasted%20image%2020250213144416.png)
  - Là một thành phần của VPC, cho phép các máy chủ trong Public Subnet kết nối ra Internet.
  - IGW được quản lý bởi AWS, không cần cấu hình phức tạp, tự động mở rộng theo nhu cầu.
  - IGW chỉ hỗ trợ kết nối ra Internet, không hỗ trợ kết nối từ Internet vào máy chủ trong Private Subnet.
  - **Internet Gateway Architecture**:
	  ![](attachments/Pasted%20image%2020250213144824.png)

##### NAT Gateway:
![NAT_Gateway](attachments/Pasted%20image%2020250213145503.png)
  - Cho phép các máy chủ trong Private Subnet kết nối ra Internet một chiều (chỉ ra, không vào).
  - NAT Gateway phải được đặt trong Public Subnet và sử dụng địa chỉ IP public để kết nối ra Internet.
  - NAT Gateway giúp các máy chủ trong Private Subnet có thể tải các bản vá, gọi API từ Internet mà không cần phải đặt máy chủ trong Public Subnet.
  - **NAT Gateway Architecture:**
	  ![NAT_Gateway_Architecture](attachments/Pasted%20image%2020250213145702.png)
### 3. Cách thức hoạt động của VPC
##### Tạo VPC:
  - Khi tạo VPC, bạn cần chọn **Region** (ví dụ: Singapore) và khai báo dải địa chỉ IP (CIDR). Ví dụ: `10.10.0.0/16`.
  - VPC có thể bao phủ nhiều **Availability Zone (AZ)** trong cùng một Region. Ví dụ: VPC có thể bao phủ AZ 1 và AZ 2 trong Region Singapore.
##### Tạo Subnet:
  - Subnet được tạo trong một AZ cụ thể và phải là tập con của dải địa chỉ IP của VPC. Ví dụ: Subnet A có dải địa chỉ `10.10.1.0/24` nằm trong AZ 1.
  - AWS giữ lại 5 địa chỉ IP trong mỗi subnet (địa chỉ network, broadcast, router, DNS, v.v.), nên khi tính toán số lượng địa chỉ IP có thể sử dụng, bạn cần trừ đi 5 địa chỉ này.
##### Public Subnet vs Private Subnet:
  - **Public Subnet**: Cho phép các máy chủ trong subnet kết nối ra Internet thông qua Internet Gateway.
    - Để máy chủ trong Public Subnet kết nối ra Internet, cần:
      1. Gán địa chỉ IP public (Elastic IP) cho máy chủ.
      2. Tạo Internet Gateway và gán vào VPC.
      3. Cấu hình Route Table để định tuyến lưu lượng ra Internet thông qua Internet Gateway.
  - **Private Subnet**: Các máy chủ trong subnet không thể kết nối trực tiếp ra Internet, nhưng có thể sử dụng NAT Gateway để kết nối một chiều.
    - Để máy chủ trong Private Subnet kết nối ra Internet, cần:
      1. Tạo NAT Gateway trong Public Subnet.
      2. Cấu hình Route Table trong Private Subnet để định tuyến lưu lượng ra Internet thông qua NAT Gateway.
##### Kết nối Internet:
  - **Internet Gateway (IGW)**: Cho phép máy chủ trong Public Subnet kết nối ra Internet.
  - **NAT Gateway**: Cho phép máy chủ trong Private Subnet kết nối ra Internet một chiều.
##### Architecture:
![Architecture](attachments/Pasted%20image%2020250213150715.png)
### **4. Kết luận**
- VPC là nền tảng quan trọng để xây dựng hệ thống mạng trên AWS, giúp kiểm soát và bảo mật tài nguyên.
- Hiểu rõ cách hoạt động của VPC, subnet, route table, và các thành phần liên quan là bước đầu tiên để triển khai ứng dụng trên AWS một cách hiệu quả và an toàn.
---
### Tóm tắt:
- **VPC** là mạng riêng ảo trên AWS, cho phép kiểm soát hoàn toàn môi trường mạng.
- **Subnet** là mạng con trong VPC, có thể là Public hoặc Private.
- **Internet Gateway** và **NAT Gateway** giúp kết nối ra Internet.
- **Route Table** định tuyến lưu lượng mạng trong VPC.
- **VPC Endpoint** cho phép kết nối an toàn đến các dịch vụ AWS khác mà không cần Internet.
- **Thực hành** là cách tốt nhất để hiểu và vận dụng các khái niệm về VPC.