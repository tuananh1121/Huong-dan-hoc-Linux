# Disk Quota
 * Quota được dùng để thể hiện việc sử dụng và giới hạn đĩa cứng với người dùng.
 * Không phải áp dụng quota cho tất cả các hệ thống tập tin nào cần thiết ta mới dùng quota
 * Khi được gọi , quota sẽ đọc tập tin `/etc/fstab` và kiểm tra những tập tin trong hệ thống này .
 * Các khái niệm cơ bản:
   * Giới hạn cứng ( Hard Limit ) : Định nghĩa dung lượng tối đa mà user có thể sử dụng . Nếu user cố tình lưu thêm dữ liệu vào thì dữ liệu này có thể bị hỏng . Việc giới hạn này mạnh mẽ và cần thiết đến 1 số user .
   * Giới hạn mềm ( Soft limit ) : Định nghĩa dung lượng đĩa cứng tối đa mà user có thể sử dụng . Tuy nhiên , không giống như hard limit , soft limit cho phép user sử dụng vượt quá dung lượng cho phép trong 1 khoảng thời gian nào đó . Thời gian này được xác định trước và gọi là thời gian gia hạn ( grace period ) . Khi user sử dụng vượt quá dung lượng cho phép , họ sẽ nhận được 1 lời cảnh báo trước
   * Thời gian gia hạn ( Grace Period ) : Là thời gian cho phép user vượt quá dung lượng đĩa cứng được cấp phép trong soft limit .

## Các bước triển khai 
Để tạo một phân vùng mới trước hết ta cần xác định trước tên thiết bị đó là gì và dung lượng của phân vùng muốn tạo. Có chú ý rằng dung lượng của phân vùng muốn tạo phải nhỏ hơn hoặc bằng với dung lượng còn trống của đĩa mà chưa cấp cho phân vùng nào khác. Ở đây ta sẽ tạo một phân vùng cho ổ `/dev/sdb` là ổ vừa add thêm vào.
![](/image/quota1.png)

Ở đây ổ mới có dung lượng 5G và ta muốn tạo một phân vùng mới có dung lượng 3G. Sử dụng lệnh `fdisk /dev/sdb`

![](/image/quota2.png)

Nhập `m` để in ra một các tùy chọn

![](/image/quota3.png)

Nhấn `n` để tiến hành tạo một phân vùng mới. Bây giờ ta chỉ cần làm tuần tự theo hướng dẫn

![](/image/quota4.png)

Khi tạo nó sẽ hỏi bạn lựa chọn `extended` hoặc `primary` nếu bạn chọn `extended` thì bạn nhập vào e còn chọn `primary` thì bạn nhập p hoặc không cần nhập. Với mỗi ổ đĩa chỉ cho tạo tối đa là 4 phân vùng `primary`. Sau đó sẽ hỏi bạn là phân vùng số mấy nếu bạn không chọn thì nó mặc định là phân vùng tiếp theo của phân vùng trước đó (cái này thường để mặc định). Tiếp theo nó sẽ hỏi `first sector` cái này thường để mặc định. Và `last sector` đây là điều cần chú ý. Bạn muốn dung lượng của phân vùng này là bao nhiêu thì bạn nhập vào `+sizeG`. Ví dụ ở đây tôi tạo phân vùng `3G` tôi nhập vào là `+3G`

![](/image/quota5.png)

Khi tạo xong thì kiểu của phân vùng sẽ mặc định là kiểu linux nếu muốn thay đổi thì nhập lại `m` sau đó nhập `t` và làm tiếp theo hướng dẫn.

Khi đã thực hiện xong thì nhấn `w` để lưu lại những thay đổi và sẽ thoát ra. Sau khi tạo phân vùng xong để muốn sử dụng ta cần format và mount nó vào một thư mục để sử dụng. Để format ta sử dụng lệnh `mkfs -t tên_định_dạng tên_phân_vùng`. Ta muốn định dang `ext4` cho phân vùng vủa tạo là `/dev/sdb1` ta dùng lệnh
```
mkfs -t ext4 /dev/sdb1
```


