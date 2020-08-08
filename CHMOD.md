#Phân quyền trong Linux
- Mục đích của phân quyền: Đối với máy tính hoặc thiết bị sử dụng hệ điều hành Linux và được chia sẻ giữa nhiều người dùng(ví dụ: Linux server,…), việc phân chia quyền hạn cho mỗi user rất quan trọng, nhằm đảm bảo tính bảo mật cũng như tránh xung đột giữa hành vi của các user với nhau.

Ví dụ thực tế: Một cơ quan có sử dụng Server cài hệ điều hành Linux để quản lý, chia sẻ tài liệu dự án giữa các thành viên trong dự án. Có những file, những thư mục quan trọng, chỉ cho phép một số người liên quan có quyền truy cập vào. Nếu không phân quyền, ai cũng có thể truy cập vào được, một ngày đẹp trời, ai đó lỡ tay xóa mất(vì họ không liên quan nên họ không ý thức được tầm quan trọng) thì thật là nguy hiểm…
