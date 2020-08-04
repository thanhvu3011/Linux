# Làm việc với hệ thống file phân cấp
Để quản lý một hệ thống Linux, bạn cần làm quen với các thư mục mặc định (default directories) có trong hầu hết các hệ thống Linux. Phần này mô tả những thư mục này và giải thích cách 

## Xác định hệ thống file phân cấp
Hệ thống file trong hầu hết các hệ thống Linux được tổ chức theo cách tương tự nhau. Cách bố trí của hệ thống file Linux được định nghĩa trong tiêu chuẩn phân cấp hệ thống tập tin (FHS), và hệ thống này được mô tả trong man 7 hier.
Bảng sau cho thấy tổng quan về các thư mục quan trọng(theo quy định của FHS):
| Thư mục |Mô tả                                                                                                      |
|---------|-----------------------------------------------------------------------------------------------------------|
|/        |Thư mục root. Nơi bắt đầu của các hệ thống cây file                                                        |
|/b       |Tại đây, bạn tìm thấy các chương trình thực thi cần thiết để sửa chữa hệ thống ở chế độ khắc phục sự cố. Thư mục này cần thiết trong quá trình khởi động|
|/boot    |Chứa tất cả các file và thư mục cần thiết để khởi động Linux kernel|
|/dev	  |Các file thiết bị được sử dụng để truy cập vào các thiết bị vật lý. Thư mục này cần thiết trong quá trình khởi động|
|/etc	  |Chứa các file cấu hình được sử dụng bởi các chương trình và dịch vụ chạy trên sever. Thư mục này cần thiết trong quá trình khởi động|
|/home	  |Thư mục này chứa tất cả các file cá nhân của người dùng|
|/lib,/lib64|	Thư viện dung chung được được sử dụng bởi các chương trình trong /boot, /bin, /sbin
|/media,/mnt |Các thư mục được sử dụng để gắn các thiết bị trong hệ thống cây file
|/opt	|Thư mục này được sử dụng cho các gói tùy chọn, nó có thể cài đặt trong máy chủ của bạn
|/proc|	Thông tin về các tiến trình đang chạy sẽ được lưu trong /proc dưới dạng một hệ thống file thư mục mô phỏng. Đây là một cấu trúc hệ thống tập tin cho phép truy cập vào thông tin kernel|
|/root|	Thư mục home của thư mục root. Chỉ có root user mới có có quyền ghi trong thư mục này.|
|/run |Chứa tiến trình và thông tin cụ thể của người dung, nó được tạo ra từ lần khởi động cuối cùng|
|/sbin|	Cũng giống như /bin, /sbinn cũng chứa các chương trình thực thi, nhưng chúng là những chương trình của admin, dành cho việc bảo trì hệ thống|
|/srv|	Thư mục này có thể được sử dụng cho dữ liệu được sử dụng bởi các dịch vụ như NFS, FTP và HTTP|
|/sys|	Thư mục được sử dụng làm giao diện cho các thiết bị phần cứng khác nhau và nó được quản lý bởi kernel và các tiến trình liên quan|
|/tmp|	Chứa các file tạm thời và các file này có thể xóa mà không gặp cảnh báo khi khởi động|
|/usr|	Thư mục chứa các thư mục con với các file chương trình, thư viện cho các file chương trình và tài liệu về chúng. Thông thường, nhiều thư mục con tồn tại trong thư mục này bắt chước nội dung của thư mục /. Nội dung của /usr không cần thiết khi khởi động.|
|/var|	Thông tin về các biến của hệ thống được lưu trong thư mục này|

## Hiểu biết về Mounts
Đề hiểu về cách tổ chức của hệ thống file trong Linux, khái niệm mounting rất quan trọng. Một hệ thống file trong Linux được trình bày dưới dạng một hệ thống phân cấp với thư mục gốc (/) là điểm bắt đầu. Hệ thống phân cấp này có thể được phân phối trên các thiết bị khác nhau và thậm chí trên các hệ thống máy tính được gắn vào thư mục gốc. Gắn thiết bị cho phép tổ chức hệ thống file Linux một cách linh hoạt. Có một số nhược điểm khi lưu trữ tất cả các tệp chỉ trong một hệ thống file:
-	Hoạt động cao trong một khu vực có thể lấp đầy toàn bộ hệ thống file, điều này sẽ tác động tiêu cực đến các dịch vụ đang chạy trên máy chủ.
-	Nếu tất cả các file trên cùng một thiết bị, rất khó để bảo mật truy cập và phân biệt giữa các khu vực khác nhau của hệ thống file với các nhu cầu bảo mật khác. Bằng cách gắn một hệ thống file riêng biệt, các tùy chọn mount có thể được thêm vào để đáp ứng các nhu cầu bảo mật cụ thể
-	Nếu một hệ thống file trên một thiết bị được lấp đầy hoàn toàn, có thể khó cung cấp thêm dung lượng lưu trữ.
Để tránh những điều này, thông thường các hệ thống file trong Linux được tổ chức trên các thiết bị khác nhau (và thậm chí chia sẻ trên các hệ thống máy tính khác), như phân vùng đĩa và khối lượng logic, và gắn các thiết bị này vào hệ thống phân cấp hệ thống file. Bằng cách cấu hình thiết bị như là giá treo, nó cũng có thể sử dụng tùy chọn gắn kết để có thể hạn chế quyền truy cập vào thiết bị. Một số thư mục thường được gắn trên các thiết bị chuyên dụng:
-	**/boot**: Thư mục này thường được gắn trên một thiết bị riêng biệt vì nó
yêu cầu thông tin cần thiết máy tính của bạn cần phải khởi động. Vì thư mục gốc (/) thường nằm trên ổ đĩa logic của  Logical Volume Manager (LVM).
từ việc Linux không thể khởi động, kernel và các file liên quan cần được lưu trữ riêng trên thiết bị /boot chuyên dụng.
-	**/var**: Thư mục này thường nằm trên một thiết bị chuyên dụng vì nó phát triển một cách năng động và không kiểm soát. Bằng cách đặt nó trên một thiết bị chuyên dụng, bạn có thể đảm bảo rằng nó sẽ không lấp đầy tất cả bộ nhớ trên máy chủ của bạn
-	**/home**: Thư mục này thường nằm trên một thiết bị chuyên dụng vì lý do bảo mật. Bởi đặt nó trên một thiết bị chuyên dụng, nó có thể được gắn với các tùy chọn cụ thể để tăng cường bảo mật cho máy chủ. Khi cài đặt lại hệ điều hành, việc có các thư mục chính trong một hệ thống file riêng là một lợi thế. Các thư mục home sau đó có thể tồn tại khi cài đặt lại hệ thống.
-	**/usr**: Thư mục này chỉ chứa các tệp hệ điều hành, mà bình thường người dùng không cần bất kỳ quyền ghi. Đặt nó trên một thiết bị chuyên dụng cho phép quản trị viên cấu hình nó dưới dạng read-only mount

 

