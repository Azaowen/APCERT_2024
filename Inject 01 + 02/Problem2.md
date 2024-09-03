# Inject 02: Kiểm tra Các Gói Mạng Cung Cấp để Tìm Anomalies



> Kính gửi PANDORA-CERT,
>
>Tôi là Oliver từ Nhóm WOOF-IT. Cảm ơn các bạn đã phản hồi nhanh chóng và hỗ trợ.
>
>Để cung cấp thêm bối cảnh, tôi vừa tham dự một Hội nghị An ninh Mạng Quốc tế, nơi tôi đã biết về một nhóm các tin tặc APT đang nhắm mục tiêu vào các viện nghiên cứu quốc phòng và nghiên cứu khác. Chúng tôi sau đó đã cung cấp thông báo bảo mật thông thường cho các nhân viên của mình. Sau khi nhận được thông báo này, một nhân viên đã báo cáo rằng máy tính của họ thỉnh thoảng bị chậm lại. Mặc dù có thể có nhiều lý do tiềm ẩn khiến một PC bị chậm, với tính chất đặc thù của viện chúng tôi, yêu cầu pháp lý về báo cáo, và các hoạt động gần đây của các nhóm APT, chúng tôi tin rằng cần phải phân tích thêm.
>
>Để điều tra thêm, chúng tôi đã thu thập các tài liệu từ một PC bị ảnh hưởng, nhưng chúng tôi vẫn đang điều tra phạm vi của các máy tính bị ảnh hưởng. Khi chúng tôi đang xác định các máy tính bị ảnh hưởng, chúng tôi yêu cầu PANDORA-CERT thực hiện một số điều tra ban đầu trên các tài liệu đính kèm trong email này. Các nhật ký duy nhất mà nhóm IT có thể thu thập từ PC tại thời điểm này là các tài liệu của Chrome và nhật ký email.
>
>Nếu nhiễm bệnh thực sự là do tải xuống mã độc, vui lòng trả lời email này ngay lập tức với liên kết tải xuống để chúng tôi có thể chặn các tải xuống tiếp theo, và cung cấp bất kỳ phát hiện nào về cách hoặc lý do tải xuống đã được thực hiện.
>
>Cảm ơn.
>
>Trân trọng,
>
>Oliver Reece
>Quản lý trợ lý, Nhóm IT
>Viện Nghiên cứu Quốc phòng WOOF
>
>Tài liệu đính kèm:  Chrome artifact Mail log files (History + workspace_mail_logs.xlsx)

### Giải thích:

**Bối cảnh**: Oliver thông báo rằng một nhân viên đã báo cáo máy tính bị chậm sau khi nhận được thông báo bảo mật, liên quan đến nhóm APT đang nhắm mục tiêu vào các viện nghiên cứu.

**Yêu cầu**: Họ yêu cầu PANDORA-CERT kiểm tra các tài liệu đính kèm (tài liệu Chrome và nhật ký email) để tìm kiếm bất kỳ dấu hiệu bất thường nào.

**Mục tiêu**: Nếu phát hiện nhiễm mã độc từ việc tải xuống, họ yêu cầu liên kết tải xuống ngay lập tức để chặn các tải xuống tiếp theo và giải thích về cách thức hoặc lý do tải xuống.

--- 

### - Tìm hiểu tài nguyên nhận được và trả lời 1 số câu hỏi như sau:

 1. Kẻ tấn công đã dùng hình thức nào để tấn công, xâm nhập vào tổ chức?
    - Khai thác lỗ hổng phần mềm qua CVE
    - Khai thác lỗ hổng web
    - Bruteforce mật khẩu quản trị và khai thác lỗ hổng dịch vụ nội bộ
    - Phishing qua email 
    - Tấn công tài khoản sa vủa MSSQL và thực hiện xp_cmdshell

 2. Email của nạn nhân trong đợt tấn công lần này là?
 3. Email của kẻ tấn công là gì?
 4. Tên tệp tin được nghi ngờ là mã độc
 5. Nạn nhân đã tìm kiếm các thông tin gì?
    - ChatGPT
    - Proton 
    - Gmail 
    - Facebook 
    - Github
 6. Password của tệp nén?
 7. Tên người dùng của máy tính nghi ngờ nhiễm mã độc?


