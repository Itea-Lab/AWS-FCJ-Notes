### Kiến Trúc Hiện Tại và Kết Nối Máy Chủ
- Nhiều máy chủ nằm trong VPC có khả năng kết nối với nhau.
- Máy chủ trong public subnet có thể kết nối đến máy chủ trong private subnet.
- Có nhu cầu hạn chế kết nối đến một cổng nhất định ở private subnet.
![](attachments/Pasted%20image%2020250224140554.png)
### Tính Năng Tường Lửa trong VPC
#### Security Group
![](attachments/Pasted%20image%2020250224140616.png)
- **Khái niệm**: Tường lửa ảo có khả năng lưu giữ trạng thái (stateful).
- **Chức năng**:
    - Kiểm soát lưu lượng truy cập đến và đi trong VPC.
    - Nếu một kết nối cho phép, kết nối ra cũng sẽ tự động cho phép mà không cần cấu hình thêm.
    - Ví dụ: Khi mở cổng 80, không cần mở cổng ra cho web server.
- **Cấu hình**:
    - Hạn chế theo giao thức, địa chỉ IP nguồn, cổng kết nối, hoặc quy định tên của Security Group khác.
    - Chỉ tạo rule allow, không có rule deny.
    - Áp dụng lên Elastic Network Interface (ENI) thay vì máy chủ ảo trực tiếp.
#### Ví dụ Cấu Hình Security Group
![](attachments/Pasted%20image%2020250224143311.png)
- **Tên Group**: Web Server Security Group.
- **Cấu hình Inbound**:
    - Kết nối SSH, HTTP, HTTPS.
    - Chỉ định protocol như TCP/UDP và cổng cần mở.
    - Nguồn: có thể chọn anywhere hoặc địa chỉ IP cụ thể.
- **Mặc định**: Chặn mọi truy cập đến, cho phép mọi truy cập đi.
### Quy Trình Tạo Security Group
![](attachments/Pasted%20image%2020250224151753.png)
- Tạo group mới sẽ chặn mọi truy cập đến và cho phép mọi truy cập đi.
- Cần kiểm tra kỹ Security Group trước khi tạo máy chủ ảo để có thể remote desktop/SSH.
### Ví Dụ Cấu Hình An Ninh cho Web Server
![](attachments/Pasted%20image%2020250224152139.png)
- **Inbound**:
    - Chỉ định cổng 80 cho phép tất cả địa chỉ IP.
- **Outbound**:
    - Mặc định mở, không cần cấu hình thêm.
### Mối Quan Hệ Giữa Các Security Group
![](attachments/Pasted%20image%2020250224152438.png)
- **Web Server Security Group**: Cho phép truy cập từ mọi địa chỉ vào cổng 80.
- **Database Security Group**: Chỉ cho phép kết nối từ Web Server qua cổng 3306.
    - Nguồn: Chỉ định Security Group tên cụ thể.
    - Tính tự động hỗ trợ khi IP của máy chủ web thay đổi.
### Network Access Control List (NACL)
![](attachments/Pasted%20image%2020250224152757.png)
- **Khái niệm**: Tường lửa không lưu giữ trạng thái (stateless).
    - Cần cấu hình cả chiều đi và chiều đến.
- Áp dụng cho toàn bộ subnet. Ảnh hưởng đến tất cả máy chủ và dịch vụ trong subnet, không chỉ cho từng máy chủ riêng lẻ.
- **Cấu hình mặc định**: 
	![](attachments/Pasted%20image%2020250224153739.png)
	- Cho phép mọi truy cập đến và đi. 
	- Có thể gây rủi ro bảo mật nếu không được cấu hình lại.
- **Quy tắc**
	![](attachments/Pasted%20image%2020250224153420.png)
    - Đọc từ trên xuống dưới theo thứ tự ưu tiên
    - Quy tắc đầu tiên khớp sẽ quyết định hành vi của gói tin.
	-  **Mỗi quy tắc bao gồm**
	    - Số thứ tự quy tắc
	    - Hành động (Allow hoặc Deny)
	    - Địa chỉ IP nguồn và đích
	    - Giao thức (TCP hoặc UDP) và số cổng
### VPC Flow Log
![](attachments/Pasted%20image%2020250224153846.png)
- **Chức năng**: Ghi lại thông tin lưu lượng giữa ENI của VPC. Capture những hành vi bất thường trong hệ thống.
- **Xuất bản thông tin**: Có thể gửi lên CloudWatch Log hoặc Amazon S3.
- **Thông tin thu thập**: Bao gồm account ID, ID ENI, địa chỉ IP nguồn/đích, port, protocol, packet size, thời gian, và trạng thái kết nối (accept/reject).
	![](attachments/Pasted%20image%2020250224154012.png)
### VPC Peering và Transit Gateway
#### VPC Peering
![](attachments/Pasted%20image%2020250224154617.png)
- **Khái niệm**: Kết nối 1:1 giữa hai hoặc nhiều VPC không thông qua internet.
    - Không hỗ trợ định tuyến bắc cầu (transitive routing).
    - Không hỗ trợ VPC có IP address space trùng lặp.
- **Cấu hình**: Cần thiết lập bảng định tuyến (Route table) sau khi tạo Peering connection.
- **Ví dụ về VPC Peering**:
	![](attachments/Pasted%20image%2020250224160001.png)
    - Giả sử bạn có hai VPC: 
	    - VPC A có CIDR là 10.10.0.0/16 và có máy chủ ảo là 10.10.2.0/24.
	    - VPC B có CIDR là 10.11.0.0/16 và có máy chủ ảo là 10.11.2.0/24.
    - Bạn có thể thiết lập một kết nối Peering giữa chúng để cho phép các tài nguyên trong VPC A truy cập các tài nguyên trong VPC B mà không cần qua Internet.
    - Sau khi thiết lập, cần cập nhật bảng định tuyến của mỗi VPC để có thể định tuyến lưu lượng giữa chúng.
- **VPC Peering trong thực tế**:
	![](attachments/Pasted%20image%2020250224161438.png)
    - VPC Peering cho phép kết nối giữa các VPC khác nhau, kể cả khi chúng thuộc các AWS account hoặc region khác nhau.
    - Tuy nhiên, việc thiết lập nhiều kết nối peering giữa các VPC có thể trở nên phức tạp và tốn kém về mặt công sức và quản lý.
	- **Thách thức khi sử dụng VPC Peering**:
	    - **Số lượng kết nối tăng nhanh**: Nếu một VPC cần kết nối với nhiều VPC khác nhau ở các region khác nhau, số lượng peering connection cần thiết sẽ tăng lên đáng kể.
	    - **Khó khăn trong quản lý**: Việc thiết lập và duy trì nhiều peering connection có thể trở nên phức tạp, đặc biệt là trong các môi trường lớn với nhiều VPC.
	- **Giải pháp thay thế**:
	    - **Giảm thiểu sử dụng VPC Peering**: Trong thực tế, các tổ chức thường cố gắng giảm thiểu việc sử dụng VPC Peering để tránh sự phức tạp và chi phí quản lý.
	    - **Sử dụng các giải pháp khác**: Các giải pháp như **AWS Transit Gateway** hoặc **VPN/Direct Connect** có thể được sử dụng để kết nối nhiều VPC một cách hiệu quả hơn, đặc biệt là trong các môi trường đa region hoặc đa account.
#### Transit Gateway
![](attachments/Pasted%20image%2020250224162108.png)
- **Khái niệm**:
    - Transit Gateway là một nền tảng kết nối mạnh mẽ cho phép quản lý và kết nối nhiều VPC (Virtual Private Cloud) cùng với các mạng on-premise (hạ tầng vật lý).
    - Cung cấp một giải pháp linh hoạt cho việc quản lý kết nối mạng trong môi trường điện toán đám mây.
- **Chức năng**:
    - **Giảm bớt sự phức tạp trong việc cấu hình mạng:** giúp đơn giản hóa việc quản lý kết nối giữa nhiều VPC bằng cách thay thế việc cần thiết lập các Peering connection phức tạp, tối ưu hóa cấu trúc mạng tổng thể.
    - **Kết nối đồng thời:** cho phép kết nối nhiều VPC, hỗ trợ việc quản lý lưu lượng giữa các mạng một cách dễ dàng và hiệu quả.
- **Transit Gateway Attachment**: 
	- Để thiết lập kết nối giữa VPC và Transit Gateway, người dùng sẽ sử dụng Transit Gateway Attachment.
	- **Hoạt Động Ở Quy Mô AZ**: Transit Gateway Attachment hoạt động ở cấp độ Availability Zone (AZ) với mỗi một AZ = 1 transit gateway, cho phép tối ưu hóa lưu lượng mạng.
		- Giả sử VPC có subnet rải đều 4 AZ thì sẽ cần 4 Transit Gateway Attachment và mỗi transit gateway sẽ có một bảng định tuyến (Route table để dẫn tới VPC mong muốn)
			![](attachments/Pasted%20image%2020250224164235.png)
	- **Tham Gia Mạng**: Transit Gateway Attachment cho phép các VPC tham gia vào mạng lưới qua Transit Gateway, tạo ra kết nối mạng nhất quán và mở rộng.
	- **Cầu Nối Giữa Các VPC**: Có thể hiểu Transit Gateway Attachment như một “cầu nối” giữa các VPC và Transit Gateway.
	- **Chia Sẻ Tài Nguyên**: Giúp chia sẻ tài nguyên và truyền tải dữ liệu hiệu quả hơn giữa các VPC.
- **Thay thế nhiều Peering Connections**:
    - Có khả năng kết nối nhiều VPC thông qua một Transit Gateway, giảm thiểu chi phí và tăng cường khả năng mở rộng của cấu trúc mạng.
### Tóm Tắt
- Sử dụng Security Group để áp dụng tường lửa ở mức ENI, NACL ở mức subnet.
- Cấu hình cẩn thận Security Group và NACL giúp bảo mật và quản lý tốt hơn trong môi trường VPC.
- Công cụ như VPC Flow Log, VPC Peering và Transit Gateway cung cấp các giải pháp linh hoạt để quản lý và bảo vệ môi trường mạng.