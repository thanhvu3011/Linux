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
- User có id = 0 thì đó là user có quyền root

id = 1 – 99: dùng cho system service

id = 100 – 499: dùng cho system cài thêm

id >=500: dùng cho user và group thường

Thông tin của 1 user trong file /etc/passwd

[username] :[x]:[UID]:[GID]:[Comment]:[Home directory]:[Default shell]

 
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

## 3.2 Quản lý group

- Tạo 1 group

'''
sudo groupadd group1
'''

- Tạo nhiều group

'''
sudo groupadd group1, group2, group3
'''

- Liệt kê danh sách User trong Groups

'''

sudo groups
sudo groups user1

'''

-  Xóa 1 group

'''

sudo groupdel group1

'''

# 4.Lệnh CHOWN 

## 1.Chown cho Files – đổi quyền sở hữu của file
Để thay đổi owner của file (chủ sở hữu của file), lệnh cơ bản sẽ như sau:

'''

chown user filename

'''

Lấy ví dụ file trên là test.txt, chúng tôi thay đổi từ user sở hữu là root sang một user khác có tên là whales. Thực hiện lệnh như sau:

'''

chown whales test.txt

'''

Để kiểm tra thay đổi có được thực thi, bạn có thể dùng lại lệnh ls -l. Output sẽ hiện lên như sau:

-rw-r--r-- 1 whales root 0 Feb 20 17:45 chownSample.txt
Lệnh trên có thể chỉnh một chút để thay đổi quyền group owner, như sau:

chown user[:group] filename(s)
Lấy ví dụ trên nếu bạn muốn đổi group owner của chownSample.txt thành group aquatic, vậy lệnh sẽ cần được thực thi như sau:

chown whales:aquatic chownSample.txt
Để kiểm tra file đã được đổi thành công chưa, bạn dùng lại lệnh ls -l. Kết quả sẽ như sau:

-rw-r--r-- 1 whales aquatic 0 Feb 20 17:50 chownSample.txt
Nếu chỉ có group cần thay đổi, mà giữ nguyên owner, thì bạn gõ lệnh sau như sau:

chown :aquatic chownSample.txt
Chown có chức năng tương tự như lệnh chgrp khi bạn không đưa ra thông tin owner.

Tóm lại, cấu trúc của lệnh chown command với các tùy chọn là:

chown [OPTIONS] [USER] [:GROUP] filename(s)

## 2.Chown cho thư mục – đổi ownership của thư mục
Chown cũng có thể áp dụng cho thư mục. Thư mục này chỉ chứa files hoặc thư mục hoặc cả hai.

Lấy ví dụ chúng tôi có thư mục TestUnix, với các quyền khi dùng lệnh ls -l liệt kê có kết quả như sau:

drwxr-xr-x 2 root root 4096 Feb 20 17:35 TestUnix
Như bạn thấy đoạn đầu drwxr-xr-x, đại diện của việc phân quyền thư mục. Còn phần hai có 2 chữ root là đại diện cho quyền sở hữu. Root đầu tiên là thông tin user sở hữu và root thứ hai là thông tin nhóm sở hữu. TestUnix trong ví dụ này vì vậy có chủ sở hữu là root và thuộc về nhóm sở hữu là root.

Giống với files, bạn có thể thay chủ sở hữu cho thư mục này. Bạn thực hiện lệnh như sau để đổi chủ sở hữu của thư mục này thành whales:

chown whales /TestUnix
Để đổi group sở hữu, dùng lệnh:

chown :aquatic /TestUnix
Để thay đổi cả owner và group owner, bạn dùng lệnh sau:

chown whales:aquatic /TestUnix
Lệnh này cũng dùng được cho nhiều file và thư mục. Bạn thao tác như cấu trúc bên dưới:

chown [OPTIONS] [USER][:GROUP] file1 file2
Ví dụ của lệnh trên là:

chown whales:aquatic /tmp/TestUnix/chownSample.txt /tmp/TestUnix

## 3.Chown cho Links
Chown command có thể được dùng trên symbolic link và soft link. Symbolic link là liên kết tham chiếu tới vị trí file gốc vật lý đã tồn tại. Lệnh ln được dùng để tạo soft links. Ví dụ như file chownSample.txt, chúng tôi tạo symbolic link bằng lệnh sau::

ln -s chownSample.txt symlink
Để xác nhậ chủ sở hữu và nhóm chủ sở hữu, chúng tôi lại dùng lệnh ls -l. Lệnh này sẽ xuất kết quả như bên dưới:

-rw-r--r--  1 root root  0 Feb 19 22:01 chownSample.txt
lrwxr-xr-x  1 root root 5 Feb 19  7 22:01 symlink -> chownSample.txt
Có 2 kết quả. Một là file vật lý và 2 là file symbolic link. Để ví dụ, chúng tôi sẽ muốn đổi ownership của file symlink này, lệnh sẽ như sau:

chown whales symlink
Lệnh ở trên thay đổi ownership của file chownSample.txt. Khi thực hiện lệnh ls -l ta sẽ thấy kết quả như sau:

-rw-r--r--  1 whales root  0 Feb 19 22:01 chownSample.txt
lrwxr-xr-x  1 root root 5 Feb 19  7 22:01 symlink -> chownSample.txt
Nếu bạn muốn thực sự đổi ownership của symbolic vậy bạn cần dùng option -h. Lệnh này như sau:

chown -h whales symlink
Tại đây nếu bạn sử dụng command ls -l, vậy output sẽ như sau:

-rw-r--r--  1 whales root  0 Feb 19 22:01 chownSample.txt
lrwxr-xr-x  1 whales root  5 Feb 19 7 22:01 symlink -> chownSample.txt

## 4.Sử dụng chown đệ quy (recursive)
Chown command áp dụng lên thư mục, tuy nhiên, nếu dùng thông thường thì không áp dụng được cho file và thư mục con bên trong thư mục áp dụng. Vậy để thay đổi ownership cho toàn bộ thư mục con và file bên trong thư mục đó, chúng ta cần thực hiện lệnh một cách đệ quy.

Rất đơn giản, chúng ta chỉ cần thêm tùy chọn -R khi chạy lệnh, kết quả sẽ như sau:

chown -R [USER][:GROUP] Directory
Nếu ạn có thư mục TestUnix với nhiều thư mục con, lệnh trên sẽ thay đổi user owner của toàn bộ thư mục con đó và thư mục chính sang user whales.

chown -R whales /TestUnix