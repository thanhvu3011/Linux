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
|* |	Đề cập đến một số lượng không giới hạn của tất cả các nhân vật. (ví dụ: ls *, hiển thị tất cả các tệp trong thư mục hiện tại (ngoại trừ những tệp có tên bắt đầu bằng dấu chấm)).
|?|	Được sử dụng để chỉ một nhân vật cụ thể có thể là bất kỳ nhân vật nào.Ví dụ ls c?t nó sẽ phù hợp với cat cũng như cut|
|[auto]|	Đề cập đến một ký tự có thể được chọn từ phạm vi được chỉ định giữa các dấu ngoặc vuông. ls c[auo]t sẽ phù hợp với cat, cut và cot.|

## Quản lý và làm việc với các thư mục
Để tổ chức các tệp(files), Linux hoạt động với các thư mục(directories) (còn được gọi là folders). Bạn đã đọc về một số thư mục mặc định theo định nghĩa của FHS. Khi người dùng bắt đầu tạo tập tin và lưu trữ chúng trên máy chủ, việc cung cấp cấu trúc thư mục cũng rất hợp lý. Là một quản trị viên, bạn phải có khả năng biết về cấu trúc thư mục
Exercise 2: Làm việc với thư mục
Trong Ex này bạn học cách làm việc với thư mục
1. Mở một Shell như một người dung. Gõ **cd**. Tiếp theo, gõ **pwd**, viết tắt của thư mục làm việc print. Bạn sẽ thấy rằng bạn hiện đang ở trong thư mục nhà của bạn, một thư mục có tên /home/< tên người dùng >
2. Gõ **touch file1**. Lệnh này để tạo một tệp rỗng tên là file1 trên máy chủ của bạn. Vì bạn đang ở thư mục home nên có thể tạo ra bất cứ tệp nào bạn muốn
3. Gõ **cd /**. Điều này thay đổi thư mục hiện tại thành thư mục gốc (/). Gõ **touch file2** bạn sẽ thấy tin nhắn “permission denied”. Người dùng thông thường chỉ có thể tạo tệp trong thư mục nơi họ có quyền cần thiết.
4. Gõ **cd /tmp**. Điều này mang bạn đến thư mục **/tmp**, nơi mà người dùng có giấy phép để viết. Gõ lại **touch file2** bạn có thể thấy tạo ra tệp file2 trong thư mục **/tmp** (trừ khi đã có tệp file2 trong thư mục này được tạo ra trước đó)
5. Gõ **cd** để đưa bạn trở lại thư mục nhà của bạn.
6. Gõ **mkdir files**. Điều này tạo ra thư mục tên files trong thư mục hiện tại của bạn. Lệnh **mkdir** sử dụng tên của thư mục cần được tạo như một tên đường dẫn tương đối, nó liên quan đến vị trí bạn đang ở.
7. Gõ **mkdir /home/$USER/files**. Trong lệnh này, bạn đang sử dụng biến $USER, thay thế cho tên người dùng hiện tại của bạn. Đối số hoàn chỉnh của mkdir là một tên tệp tuyệt đối cho các tệp thư mục bạn đang cố gắng tạo. Bởi vì thư mục này đã tồn tại, bạn sẽ nhận được một tin nhắn “file exists”.
8. Gõ **rmdir files** để xóa các tệp thư mục bạn vừa tạo. Lệnh rmdir cho phép bạn xóa các thư mục, nhưng nó chỉ hoạt động nếu thư mục trống và không chứa bất kỳ tệp nào

## Làm việc với các tên đường dẫn tuyệt đối và tương đối
Trong phần trước, bạn đã làm việc với các lệnh cd và mkdir. Bạn đã sử dụng các lệnh này để duyệt qua cấu trúc thư mục. Bạn cũng đã làm việc với một tên tệp tương đối và một tên tệp tuyệt đối.
Tên tệp tuyệt đối hoặc tên đường dẫn tuyệt đối là tham chiếu đường dẫn đầy đủ đến tệp hoặc thư mục bạn muốn làm việc. Tên đường dẫn này bắt đầu với thư mục gốc, theo sau là tất cả các thư mục con cho đến tên tệp thực tế. Bất kể thư mục hiện tại của bạn là gì, tên tệp tuyệt đối sẽ luôn hoạt động. Ví dụ về tên tệp tuyệt đồi là /home/lisa/file1.
Một tên tệp tương đối liên quan đến thư mục hiện tại như được hiển thị với lệnh **pwd**. Nó chỉ chứa các yếu tố được yêu cầu để có được từ thư mục hiện tại cho đến mục bạn cần. Giả sử rằng thư mục hiện tại của bạn là /home (như được hiển thị bởi lệnh pwd). Khi bạn đề cập đến tên tệp tương đối lisa/file1, bạn đang đề cập đến tên tệp tuyệt đối /home/lisa/file1
Khi làm việc với tên tệp tương đối, đôi khi rất hữu ích khi di chuyển lên một cấp trong cấu trúc phân cấp. Hãy tưởng tượng bạn đã đăng nhập với quyền root và bạn muốn sao chép tệp /home/lisa/file1 vào thư mục /home/lara. Một vài giải pháp có thể dùng:
-	Sử dụng **cp /home/lisa/file1 /home/lara**. Bởi vì trong lệnh này bạn đang sử dụng tên đường dẫn tuyệt đối, lệnh này sẽ hoạt động mọi lúc
-	Đảm bảo thư mục hiện tại của bạn là **/home** và sử dụng **cp lisa/file1 lara**. Lưu ý rằng cả tệp nguồn và tệp đích được gọi là tên tệp tương đối vì lý do đó không bắt đầu bằng /
-	Nếu thư mục hiện tại được đặt thành /home/lisa, bạn cũng có thể sử dụng **cp file1 ../lara**. Trong lệnh này, tên của tệp đích sử dụng .., có nghĩa là tăng lên một cấp. .. được theo sau bởi /lara, do đó, toàn bộ tên của tệp mục tiêu sẽ được hiểu là một lần đi lên một cấp độ (vì vậy bạn sẽ ở /home), và từ đó, hãy tìm thư mục con /lara
**TIP: Nếu bạn chưa quen với Linux, việc hiểu tên tệp tương đối không phải lúc nào cũng dễ dàng. Có một cách giải quyết dễ dàng. Chỉ cần chắc chắn rằng bạn luôn làm việc với tên đường dẫn tuyệt đối. Nó gõ nhiều hơn, nhưng nó dễ hơn và do đó, bạn sẽ ít mắc lỗi hơn.**

##Lập danh sách tập tin và thư mục
Trong khi làm việc với các tệp và thư mục, sẽ hữu ích nếu bạn có thể hiển thị nội dung của thư mục hiện tại. Để làm điều này, bạn có thể sử dụng lệnh **ls**. Nếu được sử dụng mà không có đối số, **ls** hiển thị nội dung của thư mục hiện tại. Một số đối số phổ biến làm cho làm việc với ls dễ dàng hơn.

Bảng tổng quan về ls
|Lệnh|	Cách sử dụng|
|-------|--------------------|
|ls -l|	Hiển thị một danh sách, bao gồm thông tin về các thuộc tính tệp, chẳng hạn như ngày tạo và quyền|
|ls -a|	Hiển thị tất cả các tệp, bao gồm các tệp ẩn.|
|ls -lrt|	Đây là một lệnh rất hữu ích. Nó hiển thị các lệnh được sắp xếp và ngày sửa đổi. Bạn sẽ thấy các tập tin được sửa đổi gần đây nhất trong danh sách này.|
|ls -d|	Hiển thị tên của các thư mục, không phải nội dung của tất cả các thư mục khớp với các ký tự đại diện đã được sử dụng với lệnh ls|
|ls -R|	Hiển thị nội dung của thư mục hiện tại, thêm vào đó là tất cả các thư mục con của nó, đó là đệ quy(Recursively) xuống tất cả các thư mục con|

**TIP: Một tệp ẩn trên Linux là một tệp có tên bắt đầu bằng dấu chấm. Thử câu lệnh sau: touch .hidden. Sau đó gõ ls, bạn sẽ chưa thấy nó. Gõ ls –a, bạn sẽ thấy**
Khi sử dụng **ls** và **ls -l**, bạn sẽ thấy các tệp được mã hóa màu. Các màu khác nhau được sử dụng cho các loại tệp khác nhau giúp phân biệt giữa chúng dễ dàng hơn. Tuy nhiên, đừng tập trung quá nhiều vào chúng, vì màu sắc được sử dụng là kết quả của một cài đặt biến, chúng có thể khác nhau trong các vỏ Linux khác nhau hoặc trên các máy chủ Linux khác nhau.

## Sao chép tập tin và thư mục
Để tổ chức các tệp trên máy chủ của bạn, bạn sẽ thường xuyên sao chép các tệp. Lệnh **cp** giúp bạn làm như vậy. Sao chép một tập tin không khó: Chỉ cần sử dụng **cp /path/to/ file /path/to/destination**. Ví dụ, để sao chép tệp **/etc/hosts** vào thư mục **/tmp**, hãy sử dụng **cp/etc/hosts/tmp**. Điều này dẫn đến các máy chủ tệp được ghi vào **/tmp**
Với lệnh **cp**, bạn cũng có thể sao chép toàn bộ thư mục con, cùng với nội dung của nó và mọi thứ bên dưới nó. Để làm như vậy, sử dụng tùy chọn **–R**, viết tắt của Recursive. (Bạn cũng sẽ thấy tùy chọn -R với nhiều lệnh Linux khác). Để sao chép thư mục **/etc** và mọi thứ trong đó vào thư mục **/tmp**, bạn sẽ sử dụng lệnh:**cp -R/etc/tmp**
Trong khi sử dụng lệnh **cp**, quyền và các thuộc tính khác của tệp sẽ được xem xét. Nếu không có tùy chọn bổ sung, bạn có nguy cơ không được sao chép. Nếu bạn muốn đảm bảo rằng bạn giữ các quyền hiện tại, hãy sử dụng tùy chọn **-a**, **cp** sẽ hoạt động ở chế độ lưu trữ. Tùy chọn này đảm bảo rằng các quyền và tất cả các tệp khác sẽ được giữ trong khi sao chép. Vì vậy, để sao chép trạng thái chính xác của thư mục chính của bạn và mọi thứ trong đó vào thư mục **/tmp**, hãy sử dụng lệnh: **cp -a ~ /tmp**.

Một trường hợp đặc biệt khi làm việc với **cp** là các tập tin ẩn. Theo mặc định, các tập tin ẩn không được sao chép. Có ba giải pháp để làm việc với các tập tin ẩn:
-	**cp /somedir/.* /tmp** điều này sao chép tất cả các tệp có tên bắt đầu bằng dấu chấm (các tệp ẩn) thành /tmp. Nó đưa ra một thông báo lỗi cho các thư mục có tên bắt đầu bằng dấu chấm trong /somedir, bởi vì tùy chọn -R không được sử dụng
-	**cp -a / somedir /.**   điều này sao chép toàn bộ thư mục /somedir, bao gồm cả nội dung của nó, vào thư mục hiện tại. Vì vậy, kết quả là một thư mục con somedir sẽ được tạo trong thư mục hiện tại.
-	**cp -a / somedir /. .**  điều này sao chép tất cả các tệp, thường và ẩn, vào thư mục hiện tại

## Di chuyển tệp
Để di chuyển tập tin, bạn sử dụng lệnh **mv**. Lệnh này sẽ xóa tệp khỏi vị trí hiện tại của nó và đặt nó vào vị trí mới. Bạn cũng có thể sử dụng nó để đổi tên một tệp (trong thực tế, không có gì khác hơn là sao chép và xóa tệp gốc). Hãy cùng xem một số ví dụ:
-	**mv myfile /tmp** di chuyển tệp myfile từ thư mục hiện tại sang /tmp
-	**mkdir somefiles**; **mv somefiles /tmp** đầu tiên tạo một thư mục có tên somefiles và sau đó di chuyển thư mục này sang /tmp. Lưu ý rằng điều này cũng hoạt động nếu thư mục chứa các tập tin.
-	**mv myfile mynewfile** Đổi tên tệp myfile thành mynewfile.

## Xóa tệp
Nhiệm vụ quản trị tập tin phổ biến cuối cùng là xóa tập tin. Để xóa các tập tin và thư mục, bạn sử dụng lệnh **rm**. Khi được sử dụng trên một tệp duy nhất, tệp sẽ bị xóa. Bạn cũng có thể sử dụng nó trên các thư mục có chứa các tập tin. Để làm như vậy, thêm tùy chọn **-r**, một lần nữa là viết tắt của recursive.
**NOTE: Nhiều lệnh có một tùy chọn tạo hành vi đệ quy. Trên một số lệnh bạn sử dụng tùy chọn -R, trên các lệnh khác bạn sử dụng tùy chọn -r. Điều đó thật khó hiểu, nhưng nó chỉ là như vậy.**

Trên RHEL 7, lệnh **rm** sẽ nhắc xác nhận. Nếu bạn không thích điều đó, bạn có thể sử dụng tùy chọn **-f**. Hãy chắc chắn rằng bạn biết những gì bạn đang làm khi sử dụng tùy chọn này, bởi vì sau khi sử dụng nó, không có cách nào khác ngoài băng dự phòng
Exercise 3: Làm việc với tệp
Trong bài tập này, bạn làm việc với các tiện ích quản lý tệp phổ biến. Hình 3.1 cung cấp tổng quan về cấu trúc thư mục mà bạn đang làm việc trong bài tập này.

![alt](https://i.imgur.com/hkfdwWp.png)
 
1. Mở shell như một người dùng bình thường                                                                                           
2. Gõ **pwd**. Bạn nên ở trong thư mục **/home/$USER**.
3. Gõ **mfdir newfiles** và **mkdir oldfiles**. Gõ **ls** và bạn sẽ thấy các thư mục bạn vừa tạo
4. Nhập **touch newfiles/.hidden** và **touch newfiles/unsidden**. Điều này tạo ra hai tập tin trong thư mục newfiles
5. Gõ **cd oldfiles** 
6. Gõ **ls -al** điều này chỉ hiển thị hai mục:**.** , đề cập đến thư mục hiện tại; và **..** , đề cập đến mục ở trên này (thư mục cha).
7. Nhập **ls -al ../newfiles**. Trong lệnh này, bạn đang sử dụng tên đường dẫn tương đối để chỉ nội dung của thư mục /home/$ USER/newfiles
8. Sử dụng lệnh **cp -a newfiles/.**.
9. Gõ **ls -a**. Bạn thấy rằng bạn đã tạo các tệp mới thư mục con vào thư mục oldfiles
10. Đảm bảo rằng bạn vẫn ở /home/$ USER/oldfiles và nhập **rm –rf newfiles**
11. Bây giờ sử dụng lệnh **cp -a newfiles/*.**. Nhập **ls -l** để xem những gì đã được sao chép ngay bây giờ. Bạn có thể thấy rằng tập tin ẩn chưa được sao chép
12. Để đảm bảo rằng bạn sẽ sao chép các tệp ẩn cũng như các tệp thông thường, hãy sử dụng **cp –a newfiles/.** .
13. Xác nhận lệnh làm việc này, sử dụng **ls -al**. Bạn có thể nhận thấy rằng các tệp ẩn cũng như các tệp thông thường đã được sao chép thành công.

# Sử dụng các liên kết
Liên kết trên Linux giống như các bí danh được gán cho một tệp. Có liên kết tượng trưng, và có liên kết cứng. Để hiểu một liên kết, bạn cần biết một chút về tổ chức của hệ thống tệp trong Linux.
## Liên kết cứng
Linux lưu trữ dữ liệu quản trị về các tập tin trong các inode. Mọi tệp trên Linux đều có một inode và trong inode, thông tin quan trọng về tệp được lưu trữ:
-	Khối dữ liệu nơi nội dung tệp được lưu trữ
-	Ngày tạo, truy cập và sửa đổi
-	Quyền của tệp
-	Chủ sở hữu của tệp
Chỉ một phần thông tin quan trọng không được lưu trong inode: tên. Tên được lưu trữ trong thư mục và mỗi tên tệp sẽ biết địa chỉ inode nào để truy cập vào thông tin tệp. Thật thú vị khi biết rằng một inode không biết tệp có tên gì; nó chỉ biết có bao nhiêu tệp được liên kết với inode. Những tên này được gọi là liên kết cứng.
Khi bạn tạo một tập tin, bạn đặt tên cho nó. Về cơ bản, tên này là một liên kết cứng. Trên hệ thống tệp Linux, nhiều liên kết cứng có thể được tạo thành một tệp. Điều này có thể hữu ích, vì nó cho phép bạn truy cập tệp từ nhiều vị trí khác nhau. Một số hạn chế áp dụng cho các liên kết cứng:
-	Tất cả liên kết cứng phải tồn tại trên cùng một thiết bị.
-	Không thể tạo liên kết cứng đến thư mục.
-	Số lượng bí danh của tập tin gốc. Khi tên cuối cùng được xóa, nội dung cũng được loại bỏ.
Điều hay ho về liên kết cứng là không có sự khác biệt tồn tại giữa liên kết cứng thứ nhất và liên kết cứng thứ hai. Cả hai chỉ là các liên kết cứng và nếu liên kết cứng đầu tiên cho một tệp bị xóa, điều đó không ảnh hưởng đến các liên kết cứng khác vẫn còn tồn tại. Hệ điều hành Linux sử dụng các liên kết trên nhiều vị trí để làm các tệp dễ tiếp cận hơn.

## Hiểu vể các liên kết tượng trưng
Một liên kết tượng trưng (còn được gọi là liên kết mềm) không liên kết trực tiếp đến inode mà là tên của tệp. Điều này làm cho các liên kết tượng trưng linh hoạt hơn nhiều, nhưng nó cũng có một số nhược điểm. Ưu điểm của liên kết tượng trưng là chúng có thể liên kết đến các tệp trên các thiết bị khác, cũng như trên các thư mục. Nhược điểm chính là khi tệp gốc bị xóa, liên kết tượng trưng trở nên không hợp lệ và không hoạt động.
Hình 3.2 cho thấy một tổng quan sơ đồ về các nút, liên kết cứng và các liên kết tượng trưng liên quan đến nhau

![alt](Imgur::doLogin: Missing "access_token".)








 

