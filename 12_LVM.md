# Giới thiệu về Logical Volume Manager (LVM)
 * Logical Volume Manager (LVM): là phương pháp cho phép ấn định không gian đĩa cứng thành những logical Volume khiến cho việc thay đổi kích thước trở nên dễ dàng hơn (so với partition)
 * Với kỹ thuật Logical Volume Manager (LVM) bạn có thể thay đổi kích thước mà không cần phải sửa lại table của OS. Điều này thật hữu ich với những trường hợp bạn đã sử dụng hết phần bộ nhớ còn trống của partition và muốn mở rộng dung lượng của nó

## 1. Vai trò của LVM
LVM là kỹ thuật quản lý việc thay đổi kích thước lưu trữ của ổ cứng
 * Không để hệ thống bị gián đoạn hoạt động
 * Không làm hỏng dịch vụ
 * Có thể kết hợp Hot Swapping (thao tác thay thế nóng các thành phần bên trong máy tính)
## 2. Các thành phần trong LVM
![](/image/lvm1.png)

### Hard Drives - Drives

Thiết bị lưu trữ dữ liệu, ví dụ như trong linux nó là `/dev/sda`

### Partition

Partitions là các phân vùng của Hard drives, mỗi Hard drives có 4 partition, trong đó partition bao gồm 2 loại là primary partition và extended partition
 * Primaty partition:
   * Phân vùng chính, có thể khởi động
   * Mỗi đĩa cứng có thể có tối đa 4 phân vùng này.
 * Extended partition:
   * Phân vùng mở rộng, có thể tạo những vùng luân lý.

### Physical Volumes

Là một cách gọi khác của partition trong kỹ thuật LVM, nó là những thành phần cơ bản được sử dụng bởi LVM. Một Physical Volume không thể mở rộng ra ngoài phạm vi một ổ đĩa.

Chúng ta có thể kết hợp nhiều Physical Volume thành Volume Groups

### Volume Group
![](/image/lvm2.png)

 * Nhiều Physical Volume trên những ổ đĩa khác nhau kết hợp lại thành 1 Volume Group
 * Volume Group được dùng để tạo ra các Logical Volume , trong đó người dùng có thể tạo , thay đổi kích thước , gỡ bỏ và sử dụng
**Lưu ý : Boot Loader không thể đọc /boot khi nó nằm trên Logical Volume Group . Do đó không thể sử dụng LVM với boot mount point**

### Logical volume
            ![](/image/lvm3.png)

 * Volume Group được chia nhỏ thành nhiều Logical Volume, mỗi Logical Volume có ý nghĩa tương tự như partition. Nó được dùng cho các mount point và được format với những định dạng khác nhau như ext2, ext3, ext4,…

 * Khi dung lượng của Logical Volume được sử dụng hết ta có thể đưa thêm ổ đĩa mới bổ sung cho Volume Group và do đó tăng được dung lượng của Logical Volume.

 * Ví dụ bạn có 4 ổ đĩa mỗi ổ 5GB khi bạn kết hợp nó lại thành 1 volume group 20GB, và bạn có thể tạo ra 2 logical volumes mỗi disk 10GB.

             ![](lvm4.png)

### File Systems
 * Tổ chức và kiểm soát các tập tin
 * Được lưu trữ trên ổ đĩa cho phép truy cập nhanh chóng và an toàn
 * Sắp xếp dữ liệu trên đĩa cứng máy tính
 * Quản lý vị trí vật lý của mọi thành phần dữ liệu 

## 3. linear volume
             ![](/image/lvm5.png)

 * Là một Logical Volume bình thường được tạo ra từ Volume Group
 * Các Physical Volume thành phần tạo nên Volume Group không nhất thiết phải giống nhau về dung lượng
 * Ghi dữ liệu theo từng tầng một, dữ liệu sẽ được lưu hết phân vùng này rồi bắt đầu chuyển sang phân vùng khác để lưu trữ
 * Các dữ liệu tập trung vào một phân vùng sẽ dễ quản lý.
 * Khi bị mất dữ liệu sẽ mất hết dữ liệu của một phần đó. Làm việc chậm hơn bởi vì chỉ có một phân vùng mà trong khi các khu vừng khác không hoạt động
 * Linear không có khả năng đáp ứng vấn đề an toàn dữ liệu ( Fault Tolerancing ) , và tốc độ xử lý dữ liệu ( Load Balancing )
 
## 4. Striped volume

             ![](/image/lvm6.png)

 * Dữ liệu sẽ được chia đều ra và lưu vào các phân vùng đã có.
 * Tốc độ sẽ nhanh hơn vì tất cả các phân vùng sẽ cùng làm việc. Tốc độ đọc và ghi cũng nhannh hơn phương pháp Linear.
 * Khi mất dữ liệu ở một phân vùng thì sẽ bị mất và ảnh hưởng rất nhiều dữ liệu bởi vì mỗi dữ liệu đều được lưu ở nhiều phân vùng khi sử dụng phương pháp striped.
 * Striped đáp ứng được vấn đề tốc độ xử lý dữ liệu ( Load Balancing ) , tuy nhiên không đáp ứng được vấn đề an toàn dữ liệu ( Fault Tolerancing ).

#### Fault Tolerancing
 * Đề cập đến khả năng của một hệ thống (computer, network, cloud cluster,) có khả năng tiếp tục hoạt động mà không bị gián đoạn khi một hoặc nhiều thành phần của nó gặp sự cố
 * Mục tiêu của việc tạo ra một hệ thống chịu lỗi (fault-tolerant system) là ngăn chặn sự gián đoạn phát sinh từ một điểm thất bại duy nhất, đảm bảo tính sẵn sàng cao (high availability) và tính liên tục (business continuity) của các ứng dụng hoặc hệ thống thực hiện các nhiệm vụ quan trọng.
 * Fault-tolerant system sử dụng các thành phần backup tự động thay thế các thành phần có vấn đề, nhằm đảm bảo không gây gián đoạn dịch vụ. Bao gồm:
   * **Hardware systems**: được sao lưu bằng hệ thống giống hệt hoặc tương đương. Ví dụ, một server có thể được thực hiện fault tolerant bằng cách sử dụng một server giống hệt nhau chạy song song, với tất cả các hoạt động giống đối xứng với backup server.
   * **Software systems**: được sao lưu bởi các phiên bản phần mềm khác. Ví dụ, một cơ sở dữ liệu với thông tin khách hàng có thể được sao chép liên tục đến một máy khác. Nếu cơ sở dữ liệu chính bị hỏng, các hoạt động có thể được tự động chuyển hướng đến cơ sở dữ liệu thứ hai.
   * **Power sources**: Hệ thống nguồn điện thay thế. Ví dụ, nhiều tổ chức có máy phát điện có thể trở thành nguồn điện thay thế trong trường hợp nguồn điện chính gặp sự cố (UPS).
### Snapshot Volumes
 * Tính năng LVM Snapshot cung cấp tạo ra 1 bản sao ổ đĩa tại thời điểm hiện tại mà không làm gián đoạn các dịch vụ.
 * Khi thực hiện snapshot trên ổ đĩa gốc , tính năng này sẽ thực hiện tạo ra 1 bản sao của vùng dữ liệu đang có trên máy tính và có thể dùng nó để khôi phục lại trạng thái cũ
 * Snapshot chỉ thực hiện tạo ra 1 bản sao ảo, không thể thay thế hoàn toàn quá trình sao lưu dữ liệu.

 
 
 

