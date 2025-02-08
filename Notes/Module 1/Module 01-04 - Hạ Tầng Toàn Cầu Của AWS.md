## Giới thiệu
- **Hạ tầng toàn cầu của AWS** là một chủ đề quan trọng trong lĩnh vực điện toán đám mây, ảnh hưởng sâu sắc đến cách triển khai và quản lý ứng dụng trên môi trường cloud.
- **Trung tâm dữ liệu** là khái niệm cốt lõi:
    - Là nơi chứa hàng chục nghìn máy chủ.
    - Được thiết kế với quy mô lớn và sử dụng thiết bị tối ưu hóa cho hoạt động của AWS.
- **Hiệu suất** của AWS tạo ra sự khác biệt rõ rệt so với các nhà cung cấp dịch vụ khác.
- **So sánh** giữa hiệu suất thực tế và cấu hình phần cứng đơn thuần là cần thiết để đánh giá đúng giá trị của dịch vụ.
## Trung Tâm Dữ Liệu và Hiệu Suất
- **Trung tâm dữ liệu** của AWS sử dụng thiết bị phần cứng được tối ưu hóa, điều này dẫn đến sự khác biệt trong hiệu suất so với các nhà cung cấp khác.
- Để so sánh hiệu suất giữa các nhà cung cấp, cần sử dụng các công cụ **benchmark** thay vì chỉ dựa vào thông số cấu hình như CPU hay RAM.
- Ví dụ, ứng dụng **SAP** có bộ công cụ đo hiệu suất riêng, cho phép so sánh hiệu suất của dịch vụ trên các nền tảng khác nhau.
![](AWS/Notes/Module%201/attachments/Pasted%20image%2020250207100729.png)
## Availability Zone và Isolation
- Một **Availability Zone (AZ)** bao gồm một hoặc nhiều trung tâm dữ liệu và được thiết kế để không xảy ra sự cố ảnh hưởng đồng thời đến nhiều AZ.
- **Force isolation** là một khái niệm quan trọng, đảm bảo rằng các sự cố tự nhiên như động đất chỉ ảnh hưởng đến một AZ.
- Để đảm bảo tính sẵn sàng cao cho ứng dụng, AWS khuyến nghị triển khai trên ít nhất hai AZ. Điều này tương tự như việc thiết kế môi trường **Active-Active** trong các trung tâm dữ liệu truyền thống.
![[Pasted image 20250208144736.png]]
## Region và Chiến Lược Triển Khai
- Một **Region** của AWS bao gồm ít nhất 3 AZ kết nối với nhau thông qua mạng backbone của AWS và hiện có hơn 25 region toàn cầu.
- Quyết định lựa chọn region dựa vào vị trí địa lý của người dùng để giảm thiểu độ trễ, đồng thời cũng phụ thuộc vào các dịch vụ có sẵn và chi phí.
- Ví dụ, nếu người dùng chủ yếu ở Việt Nam, việc chọn region **Singapore** sẽ giúp giảm độ trễ trong kết nối.
![[Pasted image 20250208145011.png]]
## Edge Location và Các Dịch Vụ Mạng
- **Edge Location** là mạng lưới trung tâm dữ liệu thứ cấp, thiết kế để hỗ trợ các dịch vụ mạng biên, chẳng hạn như **Amazon CloudFront** - một dịch vụ **Content Delivery Network (CDN)**.
- Tại Việt Nam, đã có hai edge location ở **Hồ Chí Minh** và **Hà Nội**, cho phép sử dụng dịch vụ CDN hiệu quả.
- Ngoài CloudFront, còn có các dịch vụ khác như **Web Application Firewall (WAF)** và **Route 53** được triển khai tại edge location, cung cấp lớp bảo vệ cho ứng dụng web và quản lý DNS.
![[Pasted image 20250208145357.png]]
## Kết luận
Hạ tầng toàn cầu của AWS không chỉ là một hệ thống phức tạp mà còn là một công cụ mạnh mẽ cho phép các doanh nghiệp tối ưu hóa hiệu suất và tiết kiệm chi phí. Hiểu rõ về trung tâm dữ liệu, availability zone, region, và edge location sẽ giúp các nhà phát triển và doanh nghiệp đưa ra quyết định chính xác hơn trong việc triển khai và quản lý ứng dụng. Sự chú trọng vào hiệu suất và tính sẵn sàng cao sẽ mang lại lợi ích lớn cho doanh nghiệp trong môi trường cạnh tranh hiện nay.

## Next
[[Module 01-05 - Công Cụ Quản Lý AWS Services]]