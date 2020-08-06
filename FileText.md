# File-Text-Linux
##I. MỤC LỤC
1. Giới thiệu

2. Các lệnh trong file text
  
##II. File text
1. Giới thiệu
 - Hệ điều hành Linux đã cung cấp nhiều lệnh để chúng ta có thể làm việc với tập tin và thư mục.
 - Trong một hệ thống Linux, để đơn giản hóa quá trình xử lý dữ liệu văn bản, các nhà phát triển đã tạo ra các công cụ cơ bản để xử lý dữ liệu text trên tiêu chí một chương trình chỉ làm một việc nhưng sẽ làm việc đó một cách tốt nhất có thể. Và trên hết là các chương trình này đều không yêu cầu kỹ năng lập trình nhưng vẫn có thể dễ dàng sử dụng được.
 - Ở cả các phiên bản như Linux 7 hay Ret Hat thì làm việc với file text vẫn là 1 kỹ năng quan trọng cần có. Bạn có thể dễ dàng tìm kiếm thông tin mà bạn cần. 
 - Việc thành thạo làm việc với file text sẽ giúp bạn sử dụng hệ điều hành Linux dễ dàng hơn và cũng là thứ cần thiết cho việc thực hiện bài test về RHCSA.

2.Các lệnh ( command ) trong file text :

 2.1 Lệnh head ,tail : lấy ra một phần văn bản ở đầu hoặc cuối văn bản
 
```
head -n filename.txt sẽ lấy ra n dòng đầu của văn bản. 
```
![alt](https://i.imgur.com/HD5xOqm.png)

```   
tail -n filename.txt sẽ lấy ra n dòng cuối của văn bản.
```
![alt](https://i.imgur.com/hEOu2p5.png)

 2.2 Lệnh less : hiển thị văn bản của file 
    
   - Less: Mở các text file bằng các trang điều này cho phép đọc file text dễ dàng hơn.
   
   ![alt](https://i.imgur.com/ML1mYuA.png)  
   
 2.3 Lệnh wc (wordcount) : thống kê, đếm số lượng dữ liệu có trong file
 
```
wc filename.txt
```
![alt](https://i.imgur.com/jS3BMZJ.png)

   - Sẽ nhận giá trị [ 15 20 86 của file thanh5.txt ] giá trị đầu là số dòng(line); giá trị thứ hai là số từ (word); giá trị thứ 3 là số ký tự(character); và cuối cùng là tên file.
   
```
wc -l filename.txt
```
   - prints số dòng trong một file. 
		  
```
wc -w filename.txt
```
   - prints số từ trong một file. 
		  
```
wc -c filename.txt
```
   - hiển thị số bytes trong một file. 
		  
```
wc -m filename.txt
```
   - prints số kí tự trong một file. 
		  
```
wc -L filename.txt 
```
   - prints độ dài của dòng dài nhất trong một file.
   
	  
	 ![wc 1](https://user-images.githubusercontent.com/68736233/89208623-e63a5b80-d5e6-11ea-8279-67ad239572b7.png)

   - Để đếm số lượng thư mục :
  
```
find . -type f | wc -l
```
     ![dem file](https://user-images.githubusercontent.com/68736233/89322915-2a912e80-d6af-11ea-94bc-4f0a597e4c36.png)

		  

 2.4 Lệnh cat : Lệnh này có tham số là tên file và option quyết định nó sẽ làm tác vụ gì.
   - Cấu trúc : cat [OPTION] [FILE] 
   
   
   - Tạo file 
   
```
cat > filename.txt
``` 
   - Tạo file, nhập nội dung . Sau khi hoàn tất nhấn CTRL + D để thoát file.
   
   - Xem nội dung của file [ cat file.txt ]
   

![cat cal](https://user-images.githubusercontent.com/68736233/89328451-9d9ea300-d6b7-11ea-9817-7be07403a6cc.png)
   
     - bạn có thể dùng | more để xuất kết quả ít hơn : [ cat filename.txt | more ]
	 
	 - bạn có thể dùng | less để xuất kết quả nhiều hơn : [ cat filename.txt | less ]
   
   - Copy nội dung file  [ cat file1.txt file2.txt > filename.txt ]
   
   - Thêm nội dung ở cuối file [ cat text1.txt >> text2.txt ]
   - Đánh dấu kết thúc dòng bằng ký tự $ ở cuối mỗi dòng.  [ cat -E filename.txt]
   
   - Hiển thị nội dung file và đánh số dòng cho chúng ở mỗi dòng [ cat -n filename.txt ]
   
   - Để cắt bớt dòng trống [ cat -s filename.txt]
   
   - Để xem nội dung file theo thứ tự đảo nghịch, từ dưới lên, bắt đầu bằng dòng cuối cùng và kết thúc ở dòng đầu tiên : [ tac filename.txt ] 
   
 2.5 Lệnh [sort] : được sử dụng để sắp xếp các dòng của tệp văn bản theo thứ tự tăng dần hoặc giảm dần, theo một khoá sắp xếp. Khóa sắp xếp mặc định là thứ tự của các ký tự ASCII (theo thứ tự bảng chữ cái). Cú pháp của lệnh:
 
```
 sort  <option> <file> 
```
	
   ![sort](https://user-images.githubusercontent.com/68736233/89208882-6bbe0b80-d5e7-11ea-8a94-abdd7f34f1f0.png)


   
   - Sắp xếp theo thứ tự từ bé đến lớn : [ sort -n filename.txt ]
   
   - Sắp xếp theo thứ tự từ lớn đến bé : [ sort -nr filename.txt ]
   
   - Sắp xếp ngẫu nhiên : [ sort -R filename.txt ]
   
   - Sắp xếp các tên tệp được phân biệt bằng số : [ sort -V filename.txt ]
   
 2.6. Lệnh [ cut ] : Chúng ta có thể cần cắt 1 văn bản theo cột chứ không phải theo dòng. Giả sử rằng chúng ta có 1 tập tin văn bản chứa các báo cáo về sinh viên với nhiều cột, như là STT, Tên, Điểm … 
   - cut là 1 tiện ích nhỏ giúp chúng ta cắt, trích xuất nội dung của tập tin theo cột. Nó cũng có thể chỉ ra dấu phân cách phân biệt các cột. Trong thuật ngữ của cut, các cột được gọi là các trường (field).

```   
cut <option> <filename.txt>
```
   
   - Hiển thị cột : [ cut -f 1 -d : filename.txt ] : cột thứ 1 sẽ được hiển thị. 
   
      ![cut](https://user-images.githubusercontent.com/68736233/89208465-9e1b3900-d5e6-11ea-8bb4-b022e2d8f1b9.png)

			
 2.7 Lệnh ps (hay Process Status) :   
 
   - Xem thông tin đầy đủ của tiến trình:

```     	 
ps aux
```
		 
![ps aux](https://user-images.githubusercontent.com/68736233/89208906-78426400-d5e7-11ea-94bd-b832be9be3dc.png)


 2.7 Lệnh awk :
     - Nó xem 1 file text giống như bảng dữ liệu, bao gồm các bản ghi và các trường 
	 
	 - Câu lệnh được viết với awk sẽ nhận đầu vào là một file hoặc một input có dạng chuẩn, rồi tạo ra output theo chuẩn của nó. Awk chỉ làm việc với các file text.
     
	 - Cú pháp câu lệnh : 
	 
```
awk option file
```

```
awk '{print}' file.tx
```
     - VD : Lệnh này hiển thị dòng thứ 3 từ thư mục calfile 
	 
![awk cal](https://user-images.githubusercontent.com/68736233/89328705-f8d09580-d6b7-11ea-8cbd-860975c24b5d.png)

 2.8 Lệnh sed :
   - Sed là một tiện ích rất mạnh mẽ để lọc văn bản từ các tệp văn bản (như grep), nhưng nó có lợi ích là nó cho phép bạn áp dụng các sửa đổi cho văn bản trong các file . 
   - Câu lệnh :
```
sed (option) commands [file]
```

   - Để xem các option ta dùng 
```
man sed   
```
   - Lệnh in : option -n và command p 
  
   ![sed in](https://user-images.githubusercontent.com/68736233/89332960-726b8200-d6be-11ea-8db2-09a4639e0179.png)
   
   - Sử dụng in và thay thế :
   
   ![IS](https://user-images.githubusercontent.com/68736233/89333146-ca09ed80-d6be-11ea-9ff6-b56fd1df635a.png)

   - Lọc dòng : 
   
   ![loc dong](https://user-images.githubusercontent.com/68736233/89333361-0dfcf280-d6bf-11ea-8ce2-06ba758852ad.png)

   - Chỉnh sửa trong file 
     - vd : thay đổi chữ work thành work hard 
   
   ![workhard](https://user-images.githubusercontent.com/68736233/89334053-24577e00-d6c0-11ea-89a2-c817fdb126de.png)
   
     - vd : Thay đổi Phuong thành PHUONG và lưu thành tệp mới 
	 
	 ![workhard](https://user-images.githubusercontent.com/68736233/89334053-24577e00-d6c0-11ea-89a2-c817fdb126de.png)


   
   





