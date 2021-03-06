# Các lệnh cơ bản Linux hay sử dụng (2)

### 1. `cat`
 * Lệnh `cat` được dùng để hiển thị file ra màn hình hoặc cũng có thể copy nội dung tập tin, tạo mới tập tin,...
 ```
 cat [options] [file]
 ```
    * Options:
	   * `-n` hiển thị nội dung file cùng số dòng
	   * `-e` hiển thị `$` ở cuối dòng
	   * `-T` convert khoảng trắng TAB thành "`^I`"
 * Hiển thị nội dung file cùng 1 lúc:
```
cat /etc/file1 /etc/file2
= cat /etc/file1 ; cat /etc/file2
```
 * Tạo file rỗng mới:
``` 
cat file_rong.txt (file_rong.txt chưa tồn tại)
= touch file_rong.txt
```
* Copy nội dung file : sử dụng điều hướng Standard Output (stdout) với option `>` để copy nội dung file nguồn lên file đích. Lưu ý là nội dung file nguồn sẽ ghi đè lên file đích làm mất nội dung file đích nếu có:
```
cat file1.txt > file2.txt
```
* Thêm nội dung vào cuối file sử dụng option `>>` chuyển hướng mở rộng nội dung stdout:
```
cat file1.txt >> file2.txt
```
* Tạo và nhập nội dung vào file
```
cat > file1.txt
```
=> Lưu và thoát gõ `ctrl + z`

### 2. `more`
*  Tính năng gần giống `cat` nhưng trình bày theo từng trang và có thể dùng `space` để sang trang.
```
more [file]
```

### 3. `less`
* Ưu việt hơn cả `more` và `cat`
* Sử dụng phím `G` để trở về văn bản hoặc `shift + G` để đến cuối văn bản.
* Gõ "`/[text]`" để tìm kiếm 1 đoạn văn bản, gõ `N` để đi lại giữa các kết quả tìm kiếm và `shift + N` để quay lại các kết quả tìm kiếm trước.
* Gõ `Q` để thoát lệnh
```
less[file]
```
### 4. `head`
* Là lệnh cho phép hiển thị phần đầu file
```
head [options] [file]
```
   * Options:
      * `-n [number]` số dòng muốn hiển thị (mặc định là 10)
	  
### 5. `tail`
* Là lệnh cho phép hiển thị phần cuối file
```
tail [options] [file]
```
   * Options:
     * `-n [number]` số dòng muốn hiển thị (mặc định là 10)
	 
**Note**: Có thể dùng xem nội dung file `log`
```
tail -f /var/log/boot.log
```
### 5. `grep`
* Là lệnh cho phép lọc dữ liệu theo dòng
```
grep [options] [text] [file]
```
  * Options:
    * `-c` đếm số lần xuất hiện của `text` trong file
	* `-n` hiển thị số dòng có chứ `text` trong file
	* `-i` tìm kiếm không phân biệt chữ hoa và chữ thường
	* `-w` tìm kiếm chính xác nội dung của `text`
### 6. `cut`
* là lệnh lọc dữ liệu theo cột
```
cut [options] [file]
```
  * Options:
    * `-d signal` : "signal" dấu hiệu nhận biết để lọc dữ liệu
	* `-f` : vị trí cột muốn lấy
	* `-c` : số kí tự muốn lấy
### 7. `wc` 
* Được sử dụng để tìm kiếm thông tin về số lượng dòng,từ, byte hoặc số lượng ký tự của 1 file. Kết quả output mặc định hiển thị số lượng dòng, từ, byte của file
```
wc [options] [file]
```
  * Options:
    * `-w` số lượng từ
	* `-l` số lượng dòng
	* `-c` số lượng byte 
	* `-m` đếm số lượng kí tự
   








