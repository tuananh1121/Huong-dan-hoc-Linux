# File System
 * File system là những thành phần quan trọng trong 1 hệ điều hành , được sử dụng để điều khiển cách dữ liệu được lưu trữ và truy vấn..
 * Linux là hệ điều hành có khả năng hỗ trợ nhiều loại file system nhất hiện nay

## Cơ chế Journaling 
 * là theo dõi các thay đổi chưa được ghi lại đối với hệ thống file. Ngay cả sau khi có sự cố hoặc tắt máy đột xuất, bạn vẫn có thể truy cập phiên bản file mới nhất với khả năng bị lỗi thấp hơn.
![](/image/file1.png)

 * Journaling chỉ được sử dụng khi ghi dữ liệu lên ổ cứng và đóng vai trò như những chiếc đục lỗ để ghi thông tin vào phân vùng . Đồng thời , nó cũng khắc phục vấn đề xảy ra khi ổ cứng gặp lỗi trong quá trình này , nếu không có journal thì hệ điều hành sẽ không thể biết được file dữ liệu có được ghi đầy đủ tới ổ cứng hay chưa 
 * Trước tiên file sẽ được ghi vào journal , đẩy vào bên trong lớp quản lý dữ liệu , sau đó journal sẽ ghi file đó vào phân vùng ổ cứng khi đã sẵn sàng . Và khi thành công , file sẽ được xóa bỏ khỏi journal , đẩy ngược ra bên ngoài và quá trình hoàn tất . Nếu xảy ra lỗi trong khi thực hiện thì file system có thể kiểm tra lại journal và tất cả các thao tác chưa được hoàn tất , đồng thời ghi nhớ lại đúng vị trí xảy ra lỗi đó
 * Tuy nhiên , nhược điểm của việc sử dụng journaling là phải “đánh đổi” hiệu suất trong việc ghi dữ liệu với tính ổn định . Bên cạnh đó , còn có nhiều công đoạn khác để ghi dữ liệu vào ổ cứng nhưng với journal thì quá trình không thực sự là như vậy. Thay vào đó thì chỉ có file metadata , inode hoặc vị trí của file được ghi lại trước khi thực sự ghi vào ổ cứng.

## Các định dạng file system trên Centos 7

### 1. `ext`
 * Là định dạng file hệ thống đầu tiên được thiết kế dành riêng cho Linux .
 * Là phần nâng cấp từ file hệ thống Minix .
 * Không nên sử dụng ext vì có nhiều hạn chế , không còn được hỗ trợ trên nhiều distribution.
### 2. `ext2`
 * Thực chất không phải là file hệ thống journaling , được phát triển để kế thừa các thuộc tính của file system cũ .
 * Ext2 không sử dụng journal cho nên sẽ có ít dữ liệu được ghi vào ổ đĩa hơn .
 * Do lượng yêu cầu viết và xóa dữ liệu khá thấp, cho nên rất phù hợp với những thiết bị lưu trữ bên ngoài như thẻ nhớ, ổ USB...
### 3. `ext3`
 * về căn bản chỉ là Ext2 đi kèm với journaling
 * Mục đích chính của Ext3 là tương thích ngược với Ext2, và do vậy những ổ đĩa, phân vùng có thể dễ dàng được chuyển đổi giữa 2 chế độ mà không cần phải format như trước kia
 * vẫn còn tồn tại ở đây là những giới hạn của Ext2 vẫn còn nguyên trong Ext3, và ưu điểm của Ext3 là hoạt động nhanh, ổn định hơn rất nhiều
 * Không thực sự phù hợp để làm file hệ thống dành cho máy chủ bởi vì không hỗ trợ tính năng tạo disk snapshot và file được khôi phục sẽ rất khó để xóa bỏ sau này.
### 4. `ext4`
 * cũng giống như Ext3, lưu giữ được những ưu điểm và tính tương thích ngược với phiên bản trước đó
 * Trên thực tế, Ext4 có thể giảm bớt hiện tượng phân mảnh dữ liệu trong ổ cứng, hỗ trợ các file và phân vùng có dung lượng lớn
 * Thích hợp với ổ SSD so với Ext3, tốc độ hoạt động nhanh hơn so với 2 phiên bản Ext trước đó, cũng khá phù hợp để hoạt động trên server, nhưng lại không bằng Ext3.
### 5. `ReiserFS`
 * Không hỗ trợ đầy đủ hệ thống kernel của Linux .
 * lần đầu được công bố vào năm 2001 với nhiều tính năng mới mà file hệ thống Ext khó có thể đạt được.
 * Đạt hiệu suất hoạt động rất cao đối với những file nhỏ như file log, phù hợp với database và server email.
### 6. `btrfs`
 * đang trong giai đoạn phát triển bởi Oracle và có nhiều tính năng giống với ReiserFS
 * hỗ trợ tính năng pool trên ổ cứng, tạo và lưu trữ snapshot, nén dữ liệu ở mức độ cao, chống phân mảnh dữ liệu nhanh chóng... được thiết kế riêng biệt dành cho các doanh nghiệp có quy mô lớn.
 * BtrFS rất phù hợp để hoạt động với server dựa vào hiệu suất làm việc cao, khả năng tạo snapshot nhanh chóng cũng như hỗ trợ nhiều tính năng đa dạng khác.
### 7. `xfs`
 * Khá tương đồng với Ext4 về một số mặt nào đó, chẳng hạn như hạn chế được tình trạng phân mảnh dữ liệu, không cho phép các snapshot tự động kết hợp với nhau, hỗ trợ nhiều file dung lượng lớn, có thể thay đổi kích thước file dữ liệu... nhưng không thể shrink – chia nhỏ phân vùng XFS
 * khá phù hợp với việc áp dụng vào mô hình server media vì khả năng truyền tải file video rất tốt
 * hiệu suất hoạt động với các file dung lượng nhỏ không bằng được khi so với các định dạng file hệ thống khác, do vậy sẽ không thể áp dụng với mô hình database, email và một vài loại server có nhiều file log
 * Nếu dùng với máy tính cá nhân, thì đây cũng không phải là sự lựa chọn tốt nên so sánh với Ext, vì hiệu suất hoạt động không khả thi, ngoài ra cũng không có gì nổi trội về hiệu năng, quản lý so với Ext3/4.
### 8. `jfs`
 * tiêu tốn ít tài nguyên hệ thống, đạt hiệu suất hoạt động tốt với nhiều file dung lượng lớn và nhỏ khác nhau
 * Các phân vùng JFS có thể thay đổi kích thước được nhưng lại không thể shrink như ReiserFS và XFS, tuy nhiên nó lại có tốc độ kiểm tra ổ đĩa nhanh nhất so với các phiên bản Ext
### 9. `zfs`
 * nhiều tính năng tương tự như Btrfs và ReiserFS
 * Phụ thuộc vào thỏa thuận điều khoản sử dụng, Sun CDDL thì ZFS không tương thích với hệ thống nhân kernel của Linux, tuy nhiên vẫn hỗ trợ toàn bộ Linux’s Filesystem in Userspace – FUSE để có thể sử dụng được ZFS
### 10. `swap`
 * có thể coi thực sự không phải là 1 dạng file hệ thống, bởi vì cơ chế hoạt động khá khác biệt, được sử dụng dưới 1 dạng bộ nhớ ảo và không có cấu trúc file hệ thống cụ thể.
 * Không thể kết hợp và đọc dữ liệu được, nhưng lại chỉ có thể được dùng bởi kernel để ghi thay đổi vào ổ cứng
 * Thông thường, nó chỉ được sử dụng khi hệ thống thiếu hụt bộ nhớ RAM hoặc chuyển trạng thái của máy tính về chế độ Hibernate.