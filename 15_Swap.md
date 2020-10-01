# Swap file
 * Swap là khái niệm bộ nhớ ảo thường được sử dụng trên Linux. Khi hệ thống hoạt động , nếu hết RAM hệ thống sẽ tự động dùng 1 phần ổ cứng để làm bộ nhớ cho các ứng dụng hoạt động. Nói cách khác bạn có thể hiểu swap chính là RAM ảo.
 * Do sử dụng ổ cứng có tốc độ chậm hơn RAM , nhất là những Server không dùng SSD , do đó không nên thường xuyên sử dụng swap , sẽ làm giảm hiệu năng hệ thống .
## Tại sao phải dùng Swap?
Một trong những trường hợp quan trọng cần đến Swap là khi RAM đầy. Theo đó, Swap sẽ hạn chế các sự cố liên quan đến vấn đề bảo mật thông tin, nhất là trong hệ thống điều hành Linux.
Khi nào cần dùng đến Swap?
 * Khi dùng một phần mềm yêu cầu hệ thống có hỗ trợ bộ nhớ Swap trong phần cài đặt
 * Khi hệ thống của bạn yêu cầu nhiều RAM hơn so với dung lượng RAM vật lý bạn có
 * Khi muốn tăng cường sự ổn định hệ thông vì như đã nói ở trên bạn sẽ không bao giờ lường trước chính xác chương trình của bạn ngốn bao nhiêu RAM
 * Giảm chi phí khi dùng server.Khi mà server cần thêm RAM để chạy. Swap là giải pháp duy nhất.
## Các loại Swap
Có 2 loại swap: 
 * swap partition : Như tên của nó đã nói , đây là 1 phân vùng độc lập trên ổ đĩa cứng
 * Swap file: Là một file đặc biệt nằm trong hệ thống file
## Các bước tạo Swap file
 * B1 : Kiểm tra phân vùng swap 
 ```
 swapon -s
 ```
 * B2 : Kiểm tra dung lượng còn trống trên ổ cứng
 ```
 df -h 
 ```
 * B3 : Tạo file Swap
 ```
 dd if=/dev/zero of=/var/swapfile bs=1M count=2048
 ```
   * Trong đó:
	 * `bs` là đơn vị tính (M,G,K)
	 * `count` là số lượng `bs` cấp cho swap file
	 => Cách tính swap file = `count*bs`
 * B4 : Tạo phân vùng swap
 ```
 mkswap /var/swapfile
 ```
 * B5 : Bật swap
 ```
 swapon /var/swapfile
 ```
 * B6 : Kiểm tra lại trạng thái swap
 ```
 swapon -s
 ```
 * B7 : Lưu cấu hình vào file `/etc/fstab`
 ```
 echo /var/swapfile none swap defaults 0 0 >> /etc/fstab
 ```
 * B8 : Bảo mật file swap
 ```
 chown root:root /var/swapfile
 chmod 0600 /var/swapfile
 ```
## Swappiness là gì?
Swappiness cho biết thời điểm hệ thống sẽ chuyển từ bộ nhớ vật lý (RAM) sang bộ nhớ tạm Swap. Giá trị của Swappiness dao động từ 0 đến 100 và mặc định là 30
 * Trường hợp tham số Swappiness có giá trị bằng 0 thì sẽ hệ thống sẽ không sử dụng Swap
 * Trường hợp tham số Swappiness có giá trị bằng 100 thì hệ thống sẽ sử dụng nhiều Swap hơn và giữ cho bộ nhớ vật lý có nhiều không gian trống nhất có thể
 * Trường hợp tham số Swappiness có giá trị bằng 10 thì hệ thống sẽ không sử dụng Swap cho đến khi bộ nhớ vật lý chỉ còn lại 10%
**Lưu ý: như đã nói ở trên Swap có tốc độ xử lý chậm hơn rất nhiều so với RAM. Vì thế, bạn không nên để máy tính sử dụng Swap quá nhiều, tốt nhất nên đặt Swappiness = 10**
## Cấu hình swappiness
 * Kiểm tra mức độ dùng swap của hệ thống bằng lệnh 
 ```
 cat /proc/sys/vm/swappiness
 ```
 * Chỉnh thông số swappiness mặc định thành 10
 ```
 sysctl vm.swappiness=10
 ```
 * Lưu thông số swappiness vào file `/etc/sysctl.conf`
 ```
 vi /etc/sysctl.conf
 Thêm dòng "vm.swappiness = 10"
 Khởi động lại Server và kiểm tra .
## Xóa file Swap
 * B1 : Tắt swap
 ```
 swapoff -v /swapfile
 ```
 * B2 : Xóa dòng khai báo swap
 ```
 /swapfile swap swap defaults 0 0 tại file /etc/fstab
 ```
 * B3 : Cuối cùng để Xóa ta dùng lệnh `rm`
 ```
 rm /swapfile
 ```
## Nhược điểm của Swap
Điểm bất lợi chính của swap đó chính là tốc độ xử lý của nó . Trong khi RAM hoạt động với tốc độ tính bằng nanoseconds thì việc truy cập ổ đĩa cứng thì lại tính băng milliseconds đâu đó chậm hơn từ 10 -> 1000 lần. Tùy thuộc vào ổ đĩa của bạn. Chính vì thế bạn nên xem xét khi sử dụng Swap. Nên dùng khi có ổ cứng có tốc độ đọc ghi nhanh. Nếu tốc độ đọc ghi chậm mà cố tình sử dụng Swap thì khéo khi còn tội tệ hơn.