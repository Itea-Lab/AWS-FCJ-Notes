#### CIDR là gì?
- CIDR (Classless Inter-Domain Routing) là một phương pháp định vị địa chỉ IP (IP addressing). 
- Bên cạnh việc giúp cải thiện việc phân bổ các địa chỉ IP, nó còn thay thế các hệ thống cũ dựa trên các lớp A, B và C. Ngoài ra, phương pháp này cũng giúp kéo dài “tuổi thọ” của IPv4, cũng như làm chậm sự gia tăng của các routing table.
![CIDR_La_Gi?](attachments/Pasted%20image%2020250213154648.png)
#### Vai trò của CIDR
- CIDR cung cấp cơ chế **supernetting**, giúp thu gọn định tuyến và hợp nhất thông tin route, giảm kích thước bảng định tuyến của Router, tăng tốc tìm kiếm.
	- **Supernetting** cho phép nhóm nhiều mạng con thành một mạng lớn hơn, tiết kiệm địa chỉ IP và cải thiện hiệu suất định tuyến.
- CIDR tổng hợp các mạng phân lớp thành một mạng lớn hơn, giảm số lượng entry trong bảng định tuyến và tăng số lượng host được cấp phát trong mạng.
- Sử dụng CIDR, doanh nghiệp sẽ chỉ cần yêu cầu địa chỉ từ ISP thay vì từ trung tâm có thẩm quyền. ISP sẽ đánh giá và cấp phát vùng từ block địa chỉ CIDR.
#### CIDR hoạt động như thế nào?
- CIDR hoạt động dựa trên **variable-length subnet masking (VLSM)**, cho phép xác định các tiền tố có độ dài tùy ý, giúp cải thiện hiệu suất so với hệ thống cũ.
- **Địa chỉ CIDR gồm hai phần:** 
	- Phần đầu là địa chỉ IP theo dạng prefix (Tiền tố) (ví dụ: 192.255.255.255). 
	- Phần sau là subnet mask, chỉ số bit trong địa chỉ (ví dụ: /12).
	- Ooga booga language in the nutshell: **CIDR là địa chỉ IP theo prefix + Subnet mask dưới dạng decimal**
- Kết hợp lại, địa chỉ IP CIDR có dạng: `192.255.255.255/12`.
	- Tiền tố (Prefix) mạng chỉ định số lượng bit của địa chỉ mạng, với 12 bit đầu tiên cho phần mạng và 20 bit cuối dành cho địa chỉ host.
- Block table của IPv4 CIDR cung cấp cái nhìn tổng quan về cách các định dạng địa chỉ khác nhau hiển thị và phân loại theo cách sử dụng phổ biến:
	![CIDR_Chart](attachments/Pasted%20image%2020250213162004.png)
#### Ưu điểm
- CIDR giúp giảm lãng phí không gian địa chỉ IPv4 và hạn chế số lượng mục nhập trong bảng định tuyến. Nó cho phép một mục nhập đại diện cho nhiều mạng (supernet), chỉ cần bộ định tuyến gần đích biết chi tiết.
- CIDR hiện là hệ thống định tuyến chính trên internet, được sử dụng rộng rãi trong các mạng backbone và bởi hầu hết ISP. Nó được hỗ trợ bởi các giao thức như Border Gateway Protocol (BGP) và Open Shortest Path First (OSPF).
- Các giao thức cũ hơn, như Giao thức cổng bên ngoài và Giao thức thông tin định tuyến, không hỗ trợ CIDR.
#### CIDR Blocks (Khối CIDR)
- **Khối CIDR**: Là nhóm địa chỉ IP có cùng tiền tố. Nhiều khối CIDR kết hợp lại tạo thành một **supernet**.
- **Kích thước khối**:
    - **Tiền tố (Prefix) ngắn**: Cho phép nhiều địa chỉ hơn, tạo thành khối lớn.
    - **Tiền tố (Prefix) dài**: Có ít địa chỉ hơn, tạo thành khối nhỏ.
- **Quá trình phân phối**:
    1. **IANA** (Tổ chức quản lý địa chỉ IP) phân phối các khối lớn cho các **RIR** (Cơ quan đăng ký khu vực).
    2. Các RIR chia nhỏ khối lớn thành khối nhỏ hơn và gửi cho **LIR** (Cơ quan đăng ký cục bộ).
    3. Cuối cùng, những khối này đến tay người dùng cuối.
- **Người dùng cuối (End-User)**: Hầu hết nhận khối địa chỉ từ nhà cung cấp dịch vụ internet (ISP). Nhưng những tổ chức có nhiều ISP có thể nhận khối trực tiếp từ RIR hoặc LIR.
#### Ví dụ về CIDR
Sau khi tìm hiểu sơ qua về CIDR, hay xét một số ví dụ về nó để hiểu rõ hơn. Có một công cụ tên [CIDR Calculation](https://www.ipaddressguide.com/cidr), rất hữu ích trong việc xác định dải IP tương đương với một địa chỉ CIDR cụ thể. Chỉ cần nhập địa chỉ CIDR vào công cụ, và click Calculate. Kết quả trả về sẽ bao gồm các thông tin cơ bản, như IP đầu, IP cuối, số lượng host…
![CIDR-sang-IP](https://static.vietnix.vn/wp-content/uploads/2021/04/CIDR-2.webp "CIDR là gì? CIDR hoạt động như thế nào? 9")

Tương tự, ta cũng có chuyển đổi dải IP thành CIDR. Chẳng hạn ta muốn tìm địa chỉ CIDR cho dải IP nằm giữa `192.0.0.0` và `192.0.0.255`. Chỉ cần nhập hai còn số này và công cụ sẽ trả về địa chỉ CIDR tương ứng.
![IP-sang-CIDR](https://static.vietnix.vn/wp-content/uploads/2021/04/CIDR-3.webp "CIDR là gì? CIDR hoạt động như thế nào? 10")
