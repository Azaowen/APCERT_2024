### 1. Kẻ tấn công đã dùng hình thức nào để tấn công, xâm nhập vào tổ chức

Đầu tiên, ta chú ý đến file log excel được ghi lại 

![mail Logs](/Media/excel_log.png "excel")

Nhìn lướt qua một lượt từ trên xuống dưới, ta thấy 4,5 logs đầu là hoạt đọngo truy cập bình thường, tuy nhiên điểm chú ý ở log thứ 6 ta thấy ai đó đã gửi đính kèm 1 file zip từ một địa chỉ email lạ là anderson và tên miền thuộc 1 công ty khác là proton.me. Điểu này rất là khả nghi khi có thể dò xét các log dưới cho thấy đều là giao tiếp nội bộ của công ty có tên miền woof.com. suy ra kẻ tấn công đã dùng hình thức 

    Phishing qua email

### 2. Email của nạn nhân trong đợt tấn công lần này là?

Khi xác định được cuộc tấn công từ câu tra, ta có thể dễ dàng dóng sang cột "To" để xem email của nạn nhân

    eve@woof.com

### 3. Email của kẻ tấn công là gì?

     anderson.daniel.A@proton.me


### 4. Tên tệp tin được nghi ngờ là mã độc

Trong thông tin nội dung log mail ta thấy file zip được tải về và được kích hoạt 

    manual.zip

### 5. Nạn nhân đã tìm kiếm các thông tin gì?

Lúc này ta mở file history để xem log các sự kiện mà nạn nhân đã làm

![History](/Media/sqlite_overview.png "slqite")

File này chứa một số dữ liệu như **url** sẽ chứa các đường dẫn mà nạn nhân truy cập, **visit** chứa lịch sử các try cập, download chứa các file đã tải về,... Trong đó, để mắt tới trường **keyword_search_terms** - trường này chứa nội dung các từ khóa đã tìm kiếm

![Keyword](/Media/keyword_searching_sqlite.png "slqite")

    ChatGPT và Gmail

### 6. Password của tệp nén?

Pasword của tệp nén được đính kèm theo email lúc gửi cho nạn nhân

    password: share

### 7. Tên người dùng của máy tính nghi ngờ nhiễm mã độc?

Chuyển tới trường download 

![downloaded files](/Media/Download_log.png "slqite")

Như ta thấy,file nghi ngờ được tải về, cấu trúc đường dẫn thư mục của windows thường sẽ là tên của người dùng ngay sau **C:\User\<tên người dùng>** 

    username: user