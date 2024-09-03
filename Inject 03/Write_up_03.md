### 1. Mã hash của tệp mã độc (không phải tệp nén)?

Đầu tiên, ta mở file pcapng bằng phần mềm [wireshark](https://www.wireshark.org/)
Để tìm được file độc hại đó thì ta phải xác định luồng nào là đang thực hiện tải xuống, việc tải xuống từ các trang web thì hay dùng giao thức HTTP, ta lọc theo giao thức này 

![File zip](/Media/pcap1.png "pcapng")

Ngay đầu gói tin thứ 73 đầu tiên, ta thấy được giao thức này đang thực hiện lệnh GET để tải file manual.zip. Ta sẽ tải file này về bằng cách chọn **File** > **Export Object** > **HTTP**

Hộp thoại bật lên, ta có thể thấy có nhiều gói tin lớn nhỏ khác nhau, tập chung vào file zip và chọn **Save**. Từ đây ta có thể dùng các công cụ online để unzip và băm MD5, tôi đã sử dụng một số lệnh trên kali:

![File](/Media/md5_manual_file.png "chm")


    860f86601bc18dd205a5edc0d57a658d

### 2. Địa chỉ của C2 Server được sử dụng để nhận thông tin (IP:PORT)?

Quay lại gói tin wireshark, thường để máy chủ C&C có thể giao tiếp với nạn nhân thì sẽ phải có 1 máy chủ với IP thật. Như vậy, hacker mới có thể giao tiếp cũng như ra lệnh cho các con bot. Để ý gói tin thứ 1125 cho thấy, có command và lệnh POST *(Hacker chọn lệnh POST vì có thể không muốn bị các phần mềm an ninh ghi lại log, đảm bảo quá trình diễn ra bí mật )*.

Ngay kế tiếp dưới nó có một lệnh POST khác upload cái gì đó lên. Như vậy, chúng ta có thể bước đầu xác định đây là cuộc tấn công **Trojan horse** vì có thể con malware trước đã mở backdoor cho một cuộc tấn công khác! 

Thực hiện **Follor HTTP Stream** ta thấy. Máy chủ C&C đã gửi một lệnh **dir**, lệnh này dùng để liệt kê các thư mục trong windows. Ngay ở phần HOST, đó chính là IP và POST của C&C server

![File](/Media/command_execute.png "chm")

    43.201.253.28:5000

### 3. Hãy gửi đường dẫn của tệp chương trình mà kẻ tấn công sử dụng làm backdoor duy trì sự hiện diện?

Để tìm được đường dẫn mà kẻ tấn công duy trì, trước hết cần xem qua file manual.chm  *(Bản chất file này cũng là 1 file nén dặc biệt của windows nên ta cũng có thẻ hoàn toàn unzip nó ra được)*

    