#Phân quyền trong Linux
- Mục đích của phân quyền: Đối với máy tính hoặc thiết bị sử dụng hệ điều hành Linux và được chia sẻ giữa nhiều người dùng(ví dụ: Linux server,…), việc phân chia quyền hạn cho mỗi user rất quan trọng, nhằm đảm bảo tính bảo mật cũng như tránh xung đột giữa hành vi của các user với nhau.

Ví dụ thực tế: Một cơ quan có sử dụng Server cài hệ điều hành Linux để quản lý, chia sẻ tài liệu dự án giữa các thành viên trong dự án. Có những file, những thư mục quan trọng, chỉ cho phép một số người liên quan có quyền truy cập vào. Nếu không phân quyền, ai cũng có thể truy cập vào được, một ngày đẹp trời, ai đó lỡ tay xóa mất(vì họ không liên quan nên họ không ý thức được tầm quan trọng) thì thật là nguy hiểm…

##1. Phân quyền là như thế nào
Một file hay thư mục trong hệ thống có 4 quyền cơ bản sau:

a. Read (r): Đối với một file thì quyền Read chính là quyền được xem nội dung của file, còn đối với một folder thì quyền Read chính là quyền xem được danh sách các subfolder và file bên trong folder đó.

b. Write (w): Đối với một file thì quyền Write là cho phép thêm, sửa nội dùng file, còn đối với một folder thì Write cho phép thêm, xóa một subfolder hay file trong thư mục đó.

c. Execute (x): Đây là quyền thực thi. Đối với một file thì Execute cho phép thực thi file trong trường hợp file này thuộc dạng program hoặc script, còn đối với một folder Execute cho phép cd vào thư mục này.

d. Deny (-): Không có quyền làm một thao tác gì đó đối với một file hay folder xác định.

- Ví dụ về quyền của một file và một folder

