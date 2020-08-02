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
/root	Thư mục home của thư mục root. Chỉ có root user mới có có quyền ghi trong thư mục này.
/run	Chứa tiến trình và thông tin cụ thể của người dung, nó được tạo ra từ lần khởi động cuối cùng
 

