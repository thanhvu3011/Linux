# Khái niệm về Systemd
Nhắc đến Systemd, nếu như là lần đầu nghe thấy, thì có lẽ không chỉ riêng mình mà rất nhiều người sẽ đặt câu hỏi: liệu tên nó có ý nghĩ gì. Nếu là vì nó dùng để quản lý hệ thống, vậy thì sao không để là system management mà lại là systemd, chữ d ở đây nghĩa là gì. d có ý nghĩa là daemon, có ý chỉ một cái gì đó âm thầm lặng lẽ hoạt động mà bình thường ta không biết được, và ở trong hệ thống Linux thì nó chính là các tiến trình chạy dưới nền (background process). Các tiến trình này cần phải hoạt động liên tục nhưng cũng không thể để người dùng ngồi nhìn nó chạy mãi được. Chính vì vậy, nó được chạy một cách "âm thầm", thuật ngữ gọi là chạy ngầm. Người dùng nếu không để ý hoặc tìm hiểu về nó thì không thể biết được tiến trình đó đang hoạt động. Nhưng systemd cũng không phải là để chỉ các tiến trình chạy ngầm đó, mà nó là một nhóm các chương trình đặc biệt sẽ quản lý, vận hành và theo dõi các tiến trình khác hoạt động.

# Vai trò của Systemd trong hệ thống

## Bắt đầu là khởi tạo
Bất cứ một chương trình nào trong Linux đều cần được thực thi dưới dạng một tiến trình, và **systemd** cũng không ngoại lệ. Một trong các thành phần quan trọng này là khởi tạo hệ thống. **Systemd** cung cấp một chương trình đặc biệt là '/sbin/init' và nó sẽ là chương trình đầu tiên được khởi động trong hệ thống (PID = 1). Và khi hoạt động,'/sbin/init' sẽ giữ vai trò kích hoạt các file cấu hình cần thiết cho hệ thống, và các chương trình này sẽ tiếp nối để hoàn tất công đoạn khởi tạo.

## Các thành phần của Systemd
Về cơ bản thì systemd tương đương với một chương trình quản lý hệ thống và các dịch vụ trong Linux. Nó cung cấp một số các tiện ích như sau:

- **systemctl** dùng để quản lý trạng thái của các dịch vụ hệ thống (bắt đầu, kết thúc, khởi động lại hoặc kiểm tra trạng thái hiện tại)

- **journald** dùng để quản lý nhật ký hoạt động của hệ thống (hay còn gọi là ghi log)

- **logind** dùng để quản lý và theo dõi việc đăng nhập/đăng xuất của người dùng

- **networkd** dùng để quản lý các kết nối mạng thông qua các cấu hình mạng

- **timedated** dùng để quản lý thời gian hệ thống hoặc thời gian mạng

- **udev** dùng để quản lý các thiết bị và firmware

## Unit file
Tất cả các chương trình được quản lý bởi **systemd** đều được thực thi dưới dạng daemon hay background bên dưới nền và được cấu hình thành 1 file configuration gọi là unit file. Các unit file này sẽ bao gồm 12 loại:

- service (các file quản lý hoạt động của 1 số chương trình)
- socket (quản lý các kết nối)
- device (quản lý thiết bị)
- mount (gắn thiết bị)
- automount (tự đống gắn thiết bị)
- swap (vùng không gian bộ nhớ trên đĩa cứng)
- target (quản lý tạo liên kết)
- path (quản lý các đường dẫn)
- timer (dùng cho cron-job để lập lịch)
- snapshot (sao lưu)
- slice (dùng cho quản lý tiến trình)
- scope (quy định không gian hoạt động)

## Service
Mặc dù là có 12 loại unit file trong systemd, tuy nhiên có lẽ service là loại thường được quan tâm nhất. Loại này sẽ được khởi động khi bật máy và luôn chạy ở chế độ nền (daemon hoặc background) Các service thường sẽ được cấu hình trong các file riêng biệt và được quản lý thông qua câu lệnh systemctl 

Ta có thể sử dụng câu lệnh sau để xem các service đã được kích hoạt bởi hệ thống: *systemctl list-units | grep -e '.service'* hoặc *systemctl -t service*

Bộ ba tùy chọn quen thuộc của systemctl sẽ dùng khi muốn bật/tắt một service

- start: bật service
- stop: tắt service
- restart: tắt service rồi bật lại (ngoài ra còn có reload để tải lại file cấu hình tuy nhiên chỉ có 1 số chương trình hỗ trợ như Apache/Nginx ...) 

Ba tùy chọn trên sẽ được sử dụng khi hệ thống đang hoạt động, tuy nhiên systemctl cũng cung cấp 2 tùy chọn khác để điều khiển việc hoạt động của service từ lúc khởi động hệ thống:

- enable: service sẽ được khởi động cùng hệ thống
- disable: service sẽ không được khởi động cùng hệ thống

# Systemd Unit
## Giới thiệu
Trong systemd, một unit dùng để chỉ bất kỳ tài nguyên giúp hệ thống biết cách làm thế nào để hoạt động và quản lý. Đây là đối tượng chính mà các công cụ systemd biết làm thế nào để xử lý. Các nguồn tài nguyên được xác định qua các tập tin cấu hình gọi là unit files.

## Systemd unit mang lại cho bạn những gì?
Units là các đối tượng mà systemd biết làm thế nào để quản lý. Đây là những điều cơ bản và đại diện tiêu chuẩn hóa cho các tài nguyên hệ thống có thể được quản lý bởi daemon và thao túng bởi các tiện ích được cung cấp.

Các đơn vị trong một số cách có thể nói là tương tự như dịch vụ hoặc công việc trong hệ thống init khác. Tuy nhiên, một đơn vị có thể định nghĩa rộng hơn nhiều, vì chúng có thể được sử dụng cho các abstract services, tài nguyên mạng, các thiết bị, gắn kết hệ thống tập tin, và tài nguyên pool bị cô lập.

Một số tính năng mà các đơn vị có thể thực hiện một cách dễ dàng là:

- **socket-based activation**: Socket gắn liền với một dịch vụ được chia ra tốt nhất của bản thân các daemon để có thể được xử lý riêng biệt. Điều này cung cấp một số lợi thế, chẳng hạn như trì hoãn sự khởi đầu của một dịch vụ cho đến khi socket liên kết là lần đầu tiên truy cập. Điều này cũng cho phép hệ thống để tạo ra tất cả các socket đầu tiên trong quá trình khởi động, làm cho nó có thể khởi động các dịch vụ liên quan đến song song.
- **bus-based activation**: Các đơn vị cũng có thể được kích hoạt trên giao diện bus được cung cấp bởi D-Bus. Một đơn vị có thể được bắt đầu khi một bus liên quan được công bố
- **path-based activation**: Một đơn vị có thể được bắt đầu dựa trên hoạt động hoặc sự sẵn có của đường dẫn hệ thống tập tin nhất định.
- **device-based activation**: Các đơn vị cũng có thể được bắt đầu vào sự sẵn có đầu tiên của phần cứng liên quan bằng cách tận dụng sự kiện udev.
- **implicit dependency mapping**: Hầu hết các cây phụ thuộc cho các đơn vị có thể được xây dựng bởi bản thân systemd. Bạn vẫn có thể thêm phụ thuộc và thông tin yêu cầu, nhưng hầu hết các vấn đề nâng cao systemd sẽ xử lý cho bạn.
- **instances and templates**: các tập tin đơn vị mẫu có thể được sử dụng để tạo ra nhiều trường hợp của các đơn vị nói chung. Điều này cho phép cho các biến thể nhẹ hoặc đơn vị, tất cả chúng đều được cung cấp các chức năng chung.
- **easy security hardening**: đơn vị có thể thực hiện một số tính năng bảo mật khá tốt bằng cách thêm các chỉ thị đơn giản Ví dụ, bạn có thể chỉ định không có hoặc chỉ đọc truy cập vào một phần của hệ thống tập tin, khả năng giới hạn hạt nhân, và chỉ định thư mục riêng /tmp và truy cập mạng.
- **drop-ins and snippets**: Các đơn vị có thể dễ dàng được mở rộng bằng cách cung cấp các đoạn mà sẽ ghi đè lên các bộ phận của tập tin đơn vị của hệ thống. Điều này làm cho nó dễ dàng để chuyển đổi giữa nguyên gốc và đơn vị triển khai tùy chỉnh.

## Systemd Unit Files được tìn thấy ở đâu?
Các tập tin xác định cách mà systemd sẽ xử lý một đơn vị có thể được tìm thấy ở nhiều địa điểm khác nhau, mỗi trong số đó có những ưu tiên và những tác động khác nhau.
Bản sao của hệ thống các tập tin đơn vị thường được lưu giữ trong thư mục **/lib/systemd/system**. Khi phần mềm cài đặt tập tin đơn vị trên hệ thống, đây là vị trí nơi họ được đặt theo mặc định.

Các tập tin đơn vị lưu trữ ở đây có thể được bắt đầu và dừng lại theo yêu cầu trong một phiên. Tập tin nguyên gốc thường được viết bởi upstream project's maintainers mà nên làm việc trên bất kỳ hệ thống triển khai systemd trong tiêu chuẩn của nó. Bạn không nên chỉnh sửa các tập tin trong thư mục này. Thay vào đó bạn nên ghi đè lên các tập tin, nếu cần thiết, sử dụng một vị trí tập tin đơn vị này sẽ thay thế các tập tin ở vị trí này.

Nếu bạn muốn thay đổi cách mà một đơn vị chức năng, vị trí tốt nhất để làm điều đó là trong thư mục **/etc/systemd/system/**. Đơn vị file được tìm thấy ở vị trí thư mục này được ưu tiên hơn bất kỳ địa điểm khác trên hệ thống tập tin. Nếu bạn cần phải sửa đổi bản sao của hệ thống về một tập tin đơn vị, đặt một thay thế trong thư mục này là cách an toàn nhất và linh hoạt nhất để làm điều này.

Nếu bạn muốn ghi đè lên các chỉ thị cụ thể từ các tập tin đơn vị của hệ thống, bạn thực sự có thể cung cấp các đoạn tập tin đơn vị trong một thư mục con. Điều này sẽ nối thêm hoặc sửa đổi các chỉ thị của bản sao của hệ thống, cho phép bạn xác định các tùy chọn bạn muốn thay đổi.

Cách chính xác để làm điều này là tạo ra một thư mục có tên sau tập tin đơn vị với .d nối vào cuối. Vì vậy, đối với một đơn vị gọi là **example.service**, một thư mục con gọi là **example.service.d** có thể được tạo ra. Trong thư mục này một tập tin kết thúc với .conf có thể được sử dụng để ghi đè hoặc mở rộng các thuộc tính của tập tin đơn vị của hệ thống.

Ngoài ra còn có một vị trí cho định nghĩa đơn vị run-time ở **/run/systemd/system**. Đơn vị các file tìm thấy trong thư mục này có một đích ưu tiên giữa những người trong **/etc/systemd/system** and **/lib/systemd/system**. Tập tin ở vị trí này được cho trọng số ít hơn so với vị trí cũ, nhưng trọng lượng hơn sau này.

Quá trình systemd sử dụng địa điểm này cho các tập tin đơn vị tự động tạo ra khi chạy. Thư mục này có thể được sử dụng để thay đổi hành vi đơn vị của hệ thống trong suốt thời gian của phiên giao dịch. Tất cả những thay đổi trong thư mục này sẽ bị mất khi máy chủ được khởi động lại.

## Các loại đơn vị (unit)
Các đơn vị của Systemd dựa trên loại tài nguyên mà chúng mô tả. Cách dễ nhất để xác định loại của một đơn vị là với loại hậu tố của nó, được nối vào cuối của tên tài nguyên. Danh sách sau đây mô tả các loại của các đơn vị có sẵn của systemd:
- **.service**: Một đơn vị dịch vụ mô tả làm thế nào để quản lý các dịch vụ hoặc ứng dụng trên máy chủ. Điều này sẽ bao gồm làm thế nào để bắt đầu hoặc dừng dịch vụ, theo đó trường hợp nó sẽ tự động bắt đầu, và sự phụ thuộc và thông tin yêu cầu cho các phần mềm liên quan.
- **.socket**: Một tập tin đơn vị socket mô tả một mạng hoặc socket IPC, hoặc một bộ đệm FIFO rằng systemd sử dụng để kích hoạt socket. Lúc nào cũng có một file .service liên quan sẽ được bắt đầu khi hoạt động được nhìn thấy trên các socket mà đơn vị này xác định.
- **.device**: Một đơn vị mô tả một thiết bị đã được chỉ định là cần quản lý systemd bởi udev hoặc hệ thống tập tin sysfs. Không phải tất cả các thiết bị sẽ có tập tin .device. Một số kịch bản mà .device có thể cần thiết được cho yêu cầu, gắn kết, và truy cập vào thiết bị.
- **.mount**: Đơn vị này xác định điểm gắn kết trên hệ thống được quản lý bởi systemd. Chúng được đặt tên theo con đường gắn kết, với dấu gạch chéo thay đổi đến dấu gạch ngang. Mục trong /etc/fstab có thể có những đơn vị tạo ra tự động.
- **.automount**: Một đơn vị .automount cấu hình điểm gắn kết sẽ được tự động gắn kết. Đây phải được đặt tên sau khi các điểm gắn kết mà họ hướng tới và phải có một đơn vị trùng .mount để xác định các chi tiết cụ thể của sự gắn kết.
- **.swap**: đơn vị này mô tả không gian swap trên hệ thống. Tên của đơn vị này phải phản ánh các thiết bị hoặc đường dẫn tập tin của không gian.
- **.target**: Một đơn vị target được sử dụng để cung cấp các điểm đồng bộ cho các đơn vị khác khi khởi động hoặc thay đổi trạng thái. Chúng cũng có thể được sử dụng để mang lại hệ thống về trạng thái mới.
- **.path**: Đơn vị này xác định một đường dẫn mà có thể được sử dụng cho path-based activation. Theo mặc định, một đơn    vị .service của tên cơ sở tương tự sẽ được bắt đầu khi con đường đạt đến trạng thái nhất định. Điều này sẽ sử dụng inotify giám sát các đường dẫn cho sự thay đổi.
- **.timer**: Một đơn vị .timer định nghĩa một bộ đếm thời gian sẽ được quản lý bởi systemd, tương tự như một công việc định kỳ để kích hoạt chậm hoặc theo lịch trình. Một đơn vị trùng khớp sẽ được bắt đầu khi bộ đếm thời gian đạt đủ điều kiện.
- **.snapshot**: Một đơn vị .snapshot được tạo ra tự động bởi lệnh systemctl snapshot. Nó cho phép bạn tái tạo lại trạng thái hiện tại của hệ thống sau khi thực hiện thay đổi. Snapshots không tồn tại trên phiên và được sử dụng để quay trở lại trạng thái tạm thời.
- **.slice**: Một đơn vị .slice được liên kết với Linux Control Group nodes, cho phép nguồn tài nguyên có thể bị hạn chế hoặc chỉ định cho bất kỳ quá trình liên kết với các slice. Tên đơn vị này phản ánh vị trí thứ bậc của nó trong cây cgroup. Các đơn vị được đặt trong slice nhất định theo mặc định tùy thuộc vào kiểu của chúng.
- **.scope**: đơn vị .scope được tạo ra tự động bởi systemd từ các thông tin nhận được từ giao diện bus của nó. Chúng được sử dụng để quản lý các tập quy trình hệ thống được tạo ra bên ngoài.

Như bạn có thể thấy, có rất nhiều đơn vị khác nhau mà systemd biết làm thế nào để quản lý. Nhiều trong số các loại đơn vị làm việc với nhau để thêm chức năng. Ví dụ, một số đơn vị được sử dụng để kích hoạt các đơn vị khác và cung cấp chức năng kích hoạt.
Chúng ta chủ yếu sẽ tập trung vào các đơn vị .service do công năng sử dụng và tính nhất quán mà các nhà quản trị cần phải hiểu biết khi quản lý các đơn vị này.

## Cấu trúc của một tập tin đơn vị

1. [Unit] Section Directives
Phần đầu tiên được tìm thấy trong hầu hết các tập tin đơn vị là phần [Unit]. Điều này thường được sử dụng để xác định siêu dữ liệu cho các đơn vị và cấu hình các mối quan hệ của các đơn vị đến các đơn vị khác.
Mặc dù phần yêu cầu không quan trọng đối với systemd khi phân tích các tập tin, phần này thường được đặt ở đầu vì nó cung cấp một cái nhìn tổng quan của đơn vị. Một số chỉ thị phổ biến mà bạn sẽ tìm thấy trong phần [Unit] là:
- **Description=**: Chỉ thị này có thể được sử dụng để mô tả tên và chức năng cơ bản của các đơn vị. Nó được trả về bởi các công cụ systemd khác nhau.
- **Documentation=**: Chỉ thị này cung cấp một vị trí cho một danh sách các URI cho tài liệu. Đây có thể là một trong trang nội bộ có sẵn hoặc URL truy cập web. Lệnh systemctl status sẽ phơi bày thông tin này, cho phép dễ dàng khám phá hơn.
- **Requires=**: Chỉ thị này liệt kê bất kỳ đơn vị mà đơn vị này chủ yếu phụ thuộc. Nếu đơn vị hiện tại được kích hoạt, các đơn vị được liệt kê ở đây phải kích hoạt thành công, nếu không đơn vị này sẽ thất bại. Các đơn vị này bắt đầu song song với các đơn vị hiện tại theo mặc định.
- **Wants=**: Chỉ thị này cũng tương tự như Requires=, nhưng ít nghiêm ngặt hơn. Systemd sẽ cố gắng để bất kỳ đơn vị được liệt kê ở đây đều kích hoạt thành công khi đơn vị này được kích hoạt. Nếu các đơn vị này không tìm thấy hoặc không khởi động được, đơn vị hiện tại sẽ tiếp tục hoạt động.
- **BindsTo=**: Chỉ thị này cũng tương tự như Requires=, các đơn vị hiện tại dừng lại khi các đơn vị liên quan chấm dứt.
- **Before=**: Các đơn vị được liệt kê trong chỉ thị này sẽ không được bắt đầu cho đến khi đơn vị hiện tại được đánh dấu là bắt đầu nếu chúng được kích hoạt cùng một lúc.
- **After=**: Các đơn vị được liệt kê trong chỉ thị này sẽ được bắt đầu trước khi bắt đầu các đơn vị hiện tại.
- **Conflicts=**: Điều này có thể được sử dụng để liệt kê các đơn vị mà không thể chạy đồng thời là đơn vị hiện tại. Bắt đầu từ một đơn vị có mối quan hệ này sẽ gây ra các đơn vị khác để được dừng lại.
- **Condition...=**: Có một số chỉ thị bắt đầu với Condition cho phép các quản trị viên để kiểm tra các điều kiện nhất định trước khi bắt đầu các đơn vị. Điều này có thể được sử dụng để cung cấp một tập tin đơn vị chung chung mà sẽ chỉ được chạy khi trên các hệ thống thích hợp. Nếu tình trạng này không được đáp ứng, các đơn vị được bỏ qua.
- **Assert...=**: Tương tự như các chỉ thị bắt đầu bằng Condition, các chỉ thị kiểm tra các khía cạnh khác nhau của môi trường đang chạy để quyết định xem các đơn vị có nên kích hoạt.