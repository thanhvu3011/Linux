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

'''
drwxr-xr-x. 2 root root     6 Aug  8 10:25 test
-rw-r--r--. 1 root root     0 Aug  8 10:24 testfiles
'''

Ở đây là chúng ta sẽ phân tích các chỉ số phân quyền.

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

##2. Thay đổi quyền cho file/folder bằng lệnh chmod trong linux
###Chmod là gì?
Command này được dùng để đổi quyền của một file hoặc thư mục. Cơ bản, mỗi file có ba loại users tương tác với nó:

|Loại|	Giải thích|
|-----|-----------|
|owner|	Người dùng đã tạo thành file hoặc thư mục đó|
|group|	Tất cả người dùng thuộc cùng một group|
|others|Tất cả người dùng khác, không phải owner hoặc những người dùng trong group.|

Nếu bạn muốn loại user nào có quyền nào với file hoặc folder, thì bạn có thể thực thi lệnh chmod để điều khiển việc này theo ý bạn.Cấu trúc sử dụng lệnh này là:

chmod (tùy chọn) (biểu diễn phân quyền) (tên file hoặc thư mục)
Trong đó, mục (tùy chọn) là không bắt buộc, bao gồm các tùy chọn sau:
-v: hiển thị báo cáo sau khi chạy lệnh. Nếu bạn chmod nhiều file/folder cùng lúc thì cứ mỗi lần nó đổi quyền của 1 file/folder xong là sẽ hiện báo cáo.
-c: giống như trên, nhưng chỉ hiện khi nó đã làm xong tất cả.
-f: set quyền trong cả trường hợp xảy ra lỗi, nếu có lỗi xảy ra nó cũng không thông báo.
-R: nếu bạn CHMOD một folder thì kèm theo -R nghĩa là áp dụng luôn vào các file/folder nằm bên trong nó.
--help: hiển thị thông báo trợ giúp.

Ở phần [biểu diễn phân quyền], ban có thể biểu diễn bằng 2 kiểu:
kiểu ugo: kiểu này sẽ phân quyền cho từng đối tượng phân quyền.
kiểu số: cũng giống như ở trên (644).

- Kiểu số:
Như đã giải thích ở trên
Ví dụ: chmod 777 test
Chmod 644 có nghĩa là gì?
Việc đặt quyền của file thành 644 cho phép chủ sở hữu có thể truy cập và sửa đổi file theo cách họ muốn, trong khi mọi người dùng khác chỉ có thể truy cập mà không thể sửa đổi và không ai có thể thực thi file ngay cả chủ sở hữu. Đây là cài đặt lý tưởng cho những file có thể truy cập công khai vì nó duy trì cân bằng giữa sự linh hoạt và tính bảo mật.
 
Chmod 755 có nghĩa là gì?
Đặt quyền của file thành 755 về cơ bản giống như 644, ngoại trừ mọi người đều có quyền thực thi. Quyền này chủ yếu được sử dụng cho các thư mục có thể truy cập công khai, vì cần có quyền thực thi để thực hiện thay đổi đối với thư mục.

Chmod 555 có nghĩa là gì?
Việc đặt quyền của file thành 555 làm cho file không thể bị sửa đổi bởi bất kỳ ai, ngoại trừ superuser (siêu người dùng) của hệ thống. Quyền này không thường được sử dụng như 644, nhưng việc biết về nó vẫn rất quan trọng, vì cài đặt quyền chỉ đọc ngăn ngừa các thay đổi ngẫu nhiên và/hoặc giả mạo.

Chmod 777 có nghĩa là gì?
Đặt quyền truy cập file thành 777 cho phép mọi người có thể làm bất cứ điều gì họ muốn với file. Đây là một rủi ro bảo mật rất lớn, đặc biệt là trên các máy chủ web! Theo nghĩa đen, bất cứ ai cũng có thể truy cập file, sửa đổi theo cách họ muốn và thực thi nó trên hệ thống. Bạn có thể tưởng tượng thiệt hại tiềm tàng nếu một kẻ lừa đảo nhúng tay vào file này.

- Kiểu ugo:
Trong đó u là quyền của user sở hữu của tệp hay file
         g là quyền của các user thuộc group sở hữu tệp hay file
         o là quyền của các user khác
Ví dụ: 
chmod u=rx testfiles
chmod g=rw testfiles
chmod o=rw testfiles

Bạn có thể sửa đổi quyền cho nhiều lớp, chẳng hạn như ví dụ này cho chủ sở hữu có quyền đọc/ghi/thực thi nhưng nhóm và các người dùng khác chỉ có quyền đọc/thực thi:
'''
chmod u=rwx,g=rw,o=rw testfile
'''
Nhưng lợi ích của việc sử dụng kiểu ugo sẽ được thấy rõ khi bạn chỉ muốn thêm hoặc xóa quyền cho một hành động cụ thể đối với một lớp.
Ví dụ, lệnh sau thêm quyền thực thi cho chủ sở hữu file:
'''
chmod u+x testfiles
'''
Và lệnh này loại bỏ quyền ghi và thực thi cho người dùng khác:
'''
chmod o-wx testfiles
'''
