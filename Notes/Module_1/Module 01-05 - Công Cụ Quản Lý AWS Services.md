## Giới thiệu
- Trong thời đại công nghệ thông tin, việc quản lý hạ tầng **vật lý** và **logic** trong môi trường điện toán đám mây ngày càng quan trọng.
- Chương này thảo luận về cách tổ chức và quản lý các dịch vụ trên nền tảng **Amazon Web Services (AWS)**.
- Các công cụ được khám phá bao gồm:
    - **AWS Management Console**: Giao diện web để quản lý các dịch vụ AWS.
    - **AWS Command Line Interface (CLI)**: Công cụ dòng lệnh để tương tác với AWS.
    - **AWS SDK**: Thư viện lập trình hỗ trợ tích hợp AWS vào ứng dụng.
- Việc nắm vững các khái niệm và công cụ này giúp:
    - Tối ưu hóa quy trình làm việc.
    - Đảm bảo an toàn cho tài khoản AWS của người dùng.
## Hạ tầng và Quản lý Tài khoản
### Hạ tầng Vật lý và Logic
- AWS cung cấp hạ tầng **vật lý** thông qua các trung tâm dữ liệu và **logic** thông qua tổ chức các tài nguyên từ trung tâm dữ liệu lên **Availability Zone (AZ)** và từ AZ lên **Region**.
- Hệ thống phân phối này giúp đảm bảo tính khả dụng và độ tin cậy của dịch vụ.
### Tài khoản AWS và Quản lý Đăng nhập
- Để bắt đầu sử dụng AWS, người dùng cần đăng ký một tài khoản, với hai cơ chế đăng nhập chính:
    - **Root User**: Tài khoản chính dùng để đăng ký lần đầu, bao gồm email, tên người dùng và mật khẩu. Root account rất quan trọng và thường được khuyến nghị hạn chế sử dụng.
    - **IAM User**: Tài khoản con được tạo ra để quản lý việc truy cập và sử dụng tài nguyên AWS, giúp bảo mật hơn cho tài khoản chính.
### Quy trình Đăng nhập
- Khi đăng nhập vào **AWS Management Console**, người dùng sẽ thấy danh sách các dịch vụ của AWS. Sau khi đăng nhập bằng root user, người dùng có thể tạo IAM User để quản lý truy cập trong tương lai.
	![[AWS/Notes/Module_1/attachments/Root_Login.png]]
- Để đăng nhập lần thứ hai, người dùng cần cung cấp **Account ID**, một chuỗi gồm 12 chữ số để xác định tài khoản.
	![[AWS/Notes/Module_1/attachments/IAM_Login.png]]
## Giao diện Quản lý Dịch vụ
### Giao diện AWS Management Console
- Giao diện này cho phép người dùng truy cập vào các dịch vụ của AWS và thực hiện các thao tác cần thiết như tạo máy chủ ảo, lưu trữ dữ liệu, và quản lý cơ sở dữ liệu.
	![[AWS/Notes/Module_1/attachments/Management_Console.png]]
- Menu bên phải của giao diện cung cấp quyền truy cập vào **Support Center**, nơi người dùng có thể tạo các yêu cầu hỗ trợ.
	![[AWS/Notes/Module_1/attachments/Support_Center.png]]
### AWS Command Line Interface (CLI)
- AWS CLI là công cụ mã nguồn mở cho phép người dùng tương tác với các dịch vụ AWS thông qua dòng lệnh.
	![[AWS/Notes/Module_1/attachments/AWS_CLI.png]]
- Người dùng có thể triển khai các chức năng tương đương với AWS Management Console bằng cách sử dụng các lệnh API. Cách này thường được sử dụng khi cần tự động hóa các tác vụ.
## Phát triển Ứng dụng với AWS SDK
### Giới thiệu về AWS SDK
- **AWS SDK** giúp đơn giản hóa quá trình phát triển ứng dụng bằng cách cung cấp một bộ thư viện hỗ trợ nhiều ngôn ngữ lập trình khác nhau.
- Việc sử dụng SDK giúp kết nối dễ dàng với các dịch vụ của AWS, như lưu trữ dữ liệu trong cơ sở dữ liệu cloud.
![[AWS/Notes/Module_1/attachments/AWS_SDK.png]]
### Quy trình Sử dụng AWS SDK
- Thay vì người dùng trực tiếp gửi yêu cầu API, ứng dụng sẽ sử dụng **Access Key** và **Secret Access Key** để tương tác với các dịch vụ AWS.
- SDK tự động hóa nhiều tác vụ như quản lý thông tin xác thực, retry khi yêu cầu thất bại, và giải mã dữ liệu, giúp tiết kiệm thời gian và công sức cho lập trình viên.
## Kết luận
Chương này đã cung cấp cái nhìn tổng quan về cách quản lý dịch vụ trên nền tảng AWS thông qua các công cụ như **AWS Management Console**, **AWS CLI**, và **AWS SDK**. Việc hiểu rõ các khái niệm và quy trình này không chỉ giúp người dùng tối ưu hóa trải nghiệm với AWS mà còn đảm bảo an toàn và hiệu quả trong việc quản lý tài nguyên. Việc áp dụng đúng các công cụ và quy trình sẽ tạo điều kiện thuận lợi cho việc phát triển ứng dụng và quản lý hạ tầng trong môi trường điện toán đám mây.
Những kiến thức này là nền tảng quan trọng cho bất kỳ ai muốn khai thác tối đa tiềm năng mà AWS mang lại.