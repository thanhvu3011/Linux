#Phân quyền trong Linux
- Mục đích của phân quyền: Đối với máy tính hoặc thiết bị sử dụng hệ điều hành Linux và được chia sẻ giữa nhiều người dùng(ví dụ: Linux server,…), việc phân chia quyền hạn cho mỗi user rất quan trọng, nhằm đảm bảo tính bảo mật cũng như tránh xung đột giữa hành vi của các user với nhau.

Ví dụ thực tế: Một cơ quan có sử dụng Server cài hệ điều hành Linux để quản lý, chia sẻ tài liệu dự án giữa các thành viên trong dự án. Có những file, những thư mục quan trọng, chỉ cho phép một số người liên quan có quyền truy cập vào. Nếu không phân quyền, ai cũng có thể truy cập vào được, một ngày đẹp trời, ai đó lỡ tay xóa mất(vì họ không liên quan nên họ không ý thức được tầm quan trọng) thì thật là nguy hiểm…

##1. Phân quyền là như thế nào
Một file hay thư mục trong hệ thống có 4 quyền cơ bản sau:

- Read (r): Đối với một file thì quyền Read chính là quyền được xem nội dung của file, còn đối với một folder thì quyền Read chính là quyền xem được danh sách các subfolder và file bên trong folder đó.

- Write (w): Đối với một file thì quyền Write là cho phép thêm, sửa nội dùng file, còn đối với một folder thì Write cho phép thêm, xóa một subfolder hay file trong thư mục đó.

- Execute (x): Đây là quyền thực thi. Đối với một file thì Execute cho phép thực thi file trong trường hợp file này thuộc dạng program hoặc script, còn đối với một folder Execute cho phép cd vào thư mục này.

- Deny (-): Không có quyền làm một thao tác gì đó đối với một file hay folder xác định.

- Ví dụ về quyền của một file và một folder:
drwxr-xr-x. 2 root root     6 Aug  8 10:25 test
-rw-r--r--. 1 root root     0 Aug  8 10:24 testfiles

Vấn đề quan trọng ở đây là chúng ta sẽ phân tích các chỉ số phân quyền.

rw-: Đối tượng thứ nhất chính là quyền dành cho user sở hữu nó.
r--: Đối tượng thứ hai chính là quyền dành cho CÁC user thuộc group đang sở hữu nó.
r--: Đối tượng thứ ba chính là quyền dành cho MỌI user không thuộc quyền sở hữu và không thuộc group sở hữu.
Vậy cái đoạn rw-r--r-- nghĩa là “User root được phép đọc và sửa file, các user thuộc group root và những người còn lại là chỉ được đọc file“.

Nhưng đó chỉ là 1 trong kiểu biểu diễn quyền của tập tin, còn 1 kiểu biểu diễn nữa đó là ở dạng số. Cụ thể:

Quyền r được biểu diễn bằng số 4.
Quyền w được biểu diễn bằng số 2.
Quyền x được biểu diễn bằng số 1.
Quyền - được biểu diễn bằng số 0.

Nếu một đối tượng mà có đủ 3 quyền này thì bạn cứ lấy cả 3 cộng lại là  4 + 2 + 1 = 7, vậy quyền số 7 nghĩa là nó được phép đọc, sửa và thực thi file. Ví dụ như đoạn rw-rw-r-- thì mình có một phép tính như sau:
  rw- tương đương với 4+2+0 =6
  rw- tương đương với 4+2+0 =6
  r-- tương đương với 4+0+0 =4
Vậy kết luận rằng, đoạn rw-rw-r-- sẽ được biểu diễn bằng số là 664

##2. Thay đổi phân quyền cho file/folder

