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

Ngoài các thư mục này, bạn có thể tìm thấy máy chủ có các thư mục khác được gắn trên các phân vùng hoặc khối lượng chuyên dụng. Sau cùng, tùy theo quyết định của quản trị viên để quyết định thư mục nào được dành riêng thiết bị.
Để có cái nhìn tổng quan về tất cả các thiết bị và điểm gắn kết của chúng, bạn có thể sử dụng các lệnh khác nhau:
-	Lệnh **mount** cho biết tổng quan về tất cả các thiết bị được gắn. Để có được thông tin này, tệp / Proc / mounts được đọc, trong đó kernel giữ thông tin về tất cả các mount hiện tại. Nó cũng hiển thị các giao diện kernel, điều này có thể dẫn đến một danh sách dài các thiết bị được gắn kết được hiển thị
-	Lệnh **df -Th** được thiết kế để hiển thị dung lượng đĩa trống trên các thiết bị được gắn; nó bao gồm hầu hết các hệ thống gắn kết. Bởi vì nó sẽ xem xét được tất cả các hệ thống file được gắn kết, đây là một lệnh thuận tiện để có được cái nhìn tổng quan về các hệ thống gắn kết hiện tại. Tùy chọn -h tóm tắt đầu ra của lệnh theo cách có thể đọc được của người dùng và tùy chọn -T cho biết loại hệ thống tệp nào được sử dụng trên các giá trị gắn kết khác nhau.
-	Lệnh **findmnt** hiển thị các mount và mối quan hệ tồn tại giữa các mount khác nhau. Vì đầu ra của lệnh **mount** hơi dài, bạn có thể thích đầu ra của **findmnt**


### Đầu ra của df được hiển thị trong bảy cột:
-	**Filesystem**: Tên của tệp thiết bị tương tác với thiết bị đĩa được sử dụng. Các thiết bị thực bắt đầu bằng /dev (dùng để chỉ thư mục được sử dụng để lưu trữ các tệp thiết bị). Bạn cũng có thể thấy một vài thiết bị tmpfs. Đây là những thiết bị kernel được sử dụng để tạo một hệ thống tệp tạm thời trong RAM
-	**Type**: Loại hệ thống file đã được sử dụng
-	**Used**: Dung lượng của ổ đĩa mà thiết bị đang sử dụng
-	**Avail**: Dung lượng ổ đĩa không sử dụng
-	**Use%**: Phần trăm của thiết bị hiện đang được sử dụng.
-	**Mount** on: Thư mục mà thiết bị hiện được gắn vào.
*Lưu ý: khi sử dụng lệnh df, kích thước được báo cáo bằng kibibytes. Tùy chọn -m sẽ hiển thị chúng bằng mebibytes và sử dụng -h sẽ sử dụng định dạng có thể đọc được của con người trong KiB, MiB, GiB, TiB hoặc PiB*

#Quản lý các file
Là quản trị viên, bạn cần có khả năng thực hiện các tác vụ quản lý file. Những nhiệm vụ này bao gồm:
-	Làm việc với các ký tự đại diện
-	Quản lý và làm việc với các thư mục
-	Làm việc với các đường dẫn tuyệt đối và tương đối(absolute and relative)
-	Liệt kê các tập tin và thư mục
-	Sao chép tập tin và thư mục
-	Di chuyển tập tin và thư mục
-	Xóa các tập tin và thư mục

## Làm việc với các ký tự đại diện (Wildcards)
Khi làm việc với các tệp, sử dụng ký tự đại diện có thể giúp công việc của bạn dễ dàng hơn rất nhiều. Ký tự đại diện là một tính năng hệ vỏ giúp bạn tham khảo nhiều tệp một cách dễ dàng.
Bảng tổng quan :

|Wildcards|	Cách sử dụng|
|---------|--------------------------------------------------------------|
|*|	Đề cập đến một số lượng không giới hạn của tất cả các nhân vật. (ví dụ: ls *, hiển thị tất cả các tệp trong thư mục hiện tại (ngoại trừ những tệp có tên bắt đầu bằng dấu chấm)).
|?|	Được sử dụng để chỉ một nhân vật cụ thể có thể là bất kỳ nhân vật nào.Ví dụ ls c?t nó sẽ phù hợp với cat cũng như cut|
|[auto]|	Đề cập đến một ký tự có thể được chọn từ phạm vi được chỉ định giữa các dấu ngoặc vuông. ls c[auo]t sẽ phù hợp với cat, cut và cot.|




 

