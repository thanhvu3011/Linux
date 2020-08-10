# 1.User và group là gì?

## User là gì ?
- User là người có thể truy cập đến hệ thống.
- User có username và password.
- Có hai loại user: super user và regular user.
- Mỗi user còn có một định danh riêng gọi là UID.
- Định danh của người dùng bình thường sử dụng giá trị bắt đầu từ 500.

## Group là gì ?
- Group là tập hợp nhiều user lại.
- Mỗi user luôn là thành viên của một group.
- Khi tạo một user thì mặc định một group được tạo ra.
- Mỗi group còn có một định danh riêng gọi là GID.
- Định danh của group thường sử dụng giá trị bắt đầu từ 500.

# 2.Các file dữ liệu

## 2.1 /etc/passwd: Lưu thông tin tài khoản của người dùng
– Khảo sát thông tin user   
cat /etc/passwd
cat /etc/passwd | more
=> Mỗi user tạo ra được lưu trên một dòng
Ví dụ: 

![alt](https://sinhvientot.net/wp-content/uploads/2016/04/image001-1.gif)

Khi tạo một user thì mặc định linux tạo thêm một group primary cùng tên với user và chứa user đó.

– User có id = 0 thì đó là user có quyền root

id = 1 – 99: dùng cho system service
id = 100 – 499: dùng cho system cài thêm
id >=500: dùng cho user và group thường
## 2.2 /etc/shadow: Lưu thông tin password của user
Ví dụ:
'''
nv1:$1$T4CJrUJC$hfkHtryc5TprrlKAqGNAr/:15013:0:99999:7:::
'''
– user name
– password đã mã hóa
– ngày đổi passwd sau cùng (1/1/1970 + số ngày đến ngày hôm nay)
– 0: một ngày sau mới có thể đổi passwd
  1: không gia hạn đổi passwd
– ngày hết hạn của passwd 
## 2.3 /etc/group: Lưu thông tin của group
Thông tin 1 group trong file /etc/group như sau:

 [Group name] : [Group password] : [GID] : [Group members]

 Ví dụ : 
 ![alt](https://i.imgur.com/BMMscbk.png)


# 3.Quản lý user và group
## 3.1 Quản lý user
- useradd: tạo user
Các option:
-c: comment, tạo bí danh (chú thích)
-d: home directory (thư mục cá nhân)
-G: đưa user vào group
-M: không tạo thư mục cá nhân
-n: không tạo primary group, user tạo ra sẽ được đưa vào group users
-s: chỉ định shell
- passwd: đặt password cho user
-l: lock user
-u: unlock user
-d: disable passwd
- userdel: xóa user
-r: xóa luôn thư mục các nhân
