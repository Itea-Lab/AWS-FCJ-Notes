### Kết Nối Môi Trường On-Premise và Cloud
- **Khái niệm môi trường hybrid**: Kết nối giữa ứng dụng hoặc dịch vụ trên môi trường on-premise và AWS.
#### VPN Site-to-Site
![](attachments/Pasted%20image%2020250224172557.png)
- **Định nghĩa**: Kết nối toàn bộ một site on-premise (ví dụ: trung tâm dữ liệu) với môi trường VPC của AWS.
- **Hoạt động**: Tạo kết nối giữa Virtual Private Gateway (AWS) và Customer Gateway (thiết bị on-premise).
- **Áp dụng**:
    - Cho phép liên lạc giữa hai dải IP khác nhau (vd: 10.12 với 10.11).
    - Chạy tốt cho những kết nối giữa site on-premise với AWS.
- [AWS docs](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)
#### VPN Client
![](attachments/Pasted%20image%2020250224173103.png)
- **Mục tiêu**: Kết nối cho nhân viên di động hoặc chi nhánh tới tài nguyên mạng nội bộ mà không qua internet.
- **Chi phí**: Thường cao, vì vậy khuyến nghị sử dụng dịch vụ third-party (vd: Cisco Meraki VPN).
### Direct Connect
![](attachments/Pasted%20image%2020250224173127.png)
- **Định nghĩa**: Kết nối dedicated từ trung tâm dữ liệu on-premise đến AWS.
- **Độ trễ**: Khoảng 20 ms đến 30 ms tại Việt Nam.
- **Chi phí băng thông**: Có thể điều chỉnh tăng/giảm theo nhu cầu sử dụng, đặc biệt khi replicate dữ liệu lớn.
- **Yêu cầu**: Thông qua các đối tác Direct Connect như Viettel, FPT (Hosted connections) hoặc trực tiếp tới AWS (Dedicated Connections).
- **Lưu ý**: Direct Connect không mã hóa dữ liệu, cần kết hợp với VPN site-to-site để mã hóa.
### Elastic Load Balancing
![](attachments/Pasted%20image%2020250224180938.png)
- **Định nghĩa**: Dịch vụ quản lý bởi AWS dùng để cân bằng tải các request từ client tới nhiều máy chủ hoặc container khác nhau.
- Có thể nằm ở public hoặc private subnet.
- **Giao thức hỗ trợ**: HTTP, HTTPS, TCP, SSL.
#### Các loại Load Balancer
![](attachments/Pasted%20image%2020250224190203.png)
1. **Application Load Balancer (ALB)**
	![](attachments/Pasted%20image%2020250225111337.png)
    - Hoạt động tại layer 7.
    - Hỗ trợ tính năng **path-based routing** (Mobile hoặc Desktop sẽ được route tới 2 target group khác nhau).
    - Có khả năng route traffic tới target ngoài VPC, bao gồm serverless functions (Lambda).
2. **Network Load Balancer (NLB)**
	![](attachments/Pasted%20image%2020250225112140.png)
    - Hoạt động tại layer 4.
    - Hỗ trợ IP tĩnh và có hiệu suất cao.
    - Thích hợp cho lượng lớn yêu cầu.
3. **Classic Load Balancer (CLB)**
	![](attachments/Pasted%20image%2020250225112619.png)
    - Hoạt động cả layer 4 và layer 7, ít được dùng hiện nay.
4. **Gateway Load Balancer (GWLB)**
	![](attachments/Pasted%20image%2020250225112854.png)
    - Hoạt động tại layer 3, thường dùng cho bảo mật.
    - Có thể sử dụng với các thiết bị bảo mật bên thứ ba.
    - Danh sách vendor hỗ trợ: [Partner Vendors](https://aws.amazon.com/elasticloadbalancing/partners/)
    - Mô hình thường thấy trong load balancer
	    ![](attachments/Pasted%20image%2020250225113154.png)
	    -  Biểu đồ mô tả cách thức GWLB kết nối giữa các client và các thiết bị bảo mật như firewall hoặc intrusion detection systems (IDS).
		- GWLB cung cấp khả năng mở rộng cho các thiết bị bảo mật, cho phép xử lý lưu lượng lớn mà không làm giảm hiệu suất.
	    - Tích hợp mượt mà với các dịch vụ AWS khác, cho phép người dùng duy trì sự an toàn của các ứng dụng phân tán trong môi trường đám mây.
#### Tính năng của Load Balancer
![](attachments/Pasted%20image%2020250225110958.png)
- **Health Check**: Kiểm tra tình trạng hoạt động của máy chủ phía sau.
- **Sticky Session**: Gán các kết nối tới cùng một target để giữ trạng thái người dùng trong phiên làm việc.
	- Hoạt động trên Network Load Balancer, Application Load Balancer, Classic Load Balancer.
- **Log truy cập**: Lưu trữ vào Amazon S3 để phân tích và troubleshooting.
### Thực Hành
- **Bài lab 1**: Khởi tạo VPC, cấu hình tường lửa và site-to-site VPN.
	![](attachments/Pasted%20image%2020250225113845.png)
- **Bài lab 2**: Tìm hiểu về Session Manager và port forwarding.
	![](attachments/Pasted%20image%2020250225113857.png)
- **Bài lab 3**: VPC peering và DNS.
	![](attachments/Pasted%20image%2020250225113939.png)
- **Bài lab 4**: Thiết lập Transit Gateway cho nhiều VPC.
	![](attachments/Pasted%20image%2020250225113952.png)
- **Bài lab 5**: Hybrid DNS sử dụng Route 53.
	![](attachments/Pasted%20image%2020250225114003.png)
### Tài Liệu Tham Khảo
- **"Certified AWS Networking Official Study"**: Tài liệu hữu ích cho kiến thức và thi AWS Certified Advanced Networking Specialty.