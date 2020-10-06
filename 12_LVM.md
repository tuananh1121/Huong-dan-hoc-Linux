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

![](/image/lvm4.png)

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

### LVM Thin Provisioning
Thin Provisioning là tính năng cấp phát ổ cứng dựa trên sự linh hoạt của LVM. Giả sử ta có một Volume Group, ta sẽ tạo ra 1 Thin Pool từ VG này với dung lượng là 20GB cho nhiều khách hàng sử dụng. Giả sử ta có 3 khách hàng, mỗi khách hàng được cấp 6GB lưu trữ. Như vậy ta có 3 x 6GB là 18GB. Với kỹ thuật cấp phát truyền thống thì ta chỉ có thể cấp phát thêm 2GB cho khách hàng thứ 4
![](/image/lvm7.png)

Nhưng với kỹ thuật Thin Provisioning, ta vẫn có thể cấp thêm 6GB nữa cho khách hàng thứ 4. Tức là 4 x 6GB = 24GB > 20GB lúc đầu. Sở dĩ ta có thể làm được như vậy là do mỗi user tuy được cấp 6GB nhưng thường thì họ sẽ không xài hết số dung lượng này (nếu 4 khách hàng đều xài hết thì ta sẽ gặp tình trạng Over Provisioning). Ta sẽ giả dụ là họ không xài hết dung lượng được cấp thì trên danh nghĩa mỗi người sẽ được 6GB, nhưng thực tế thì họ xài đến đâu, hệ thống sẽ cấp thêm dung lượng đến đó.
  *  Over Provisioning(OP) có nghĩa là một phần dung lượng được dành riêng cho hoạt động của chip điều khiển, và người dùng không thể sử dụng phần dung lượng này. Chip điều khiển có thể dùng phần dung lượng này cho các hoạt động như wear-leveling, garbage collection hay các tính năng tối ưu hiệu năng khác.
## Cấu hình LVM
* Thêm 2 ổ cứng `sdb` `sdc`
* B1: Kiểm tra các hard drive có trên hệ thống.
```
# lsblk
```
* B2: Tạo Partition:
  * Từ các hard disk trên hệ thống tạo các partition.
  * Dùng lệnh `fdisk` để tạo
```
# fdisk /dev/sdb
```
![](/image/lvm8.png)

Nhấn `n` để tiến hành tạo một phân vùng mới. Bây giờ ta chỉ cần làm tuần tự theo hướng dẫn

![](/image/lvm9.png)

Khi tạo nó sẽ hỏi bạn lựa chọn `extended` hoặc `primary` nếu bạn chọn `extended` thì bạn nhập vào e còn chọn `primary` thì bạn nhập p hoặc không cần nhập. Với mỗi ổ đĩa chỉ cho tạo tối đa là 4 phân vùng `primary`. Sau đó sẽ hỏi bạn là phân vùng số mấy nếu bạn không chọn thì nó mặc định là phân vùng tiếp theo của phân vùng trước đó (cái này thường để mặc định). Tiếp theo nó sẽ hỏi `first sector` cái này thường để mặc định. Và `last sector` đây là điều cần chú ý. Bạn muốn dung lượng của phân vùng này là bao nhiêu thì bạn nhập vào `+sizeG`. Ví dụ ở đây ta tạo phân vùng `60G` tôi nhập vào là `+60G`

![](/image/lvm10.png)

Tiếp theo ta tạo thêm phân vùng `sdb2`

![](/image/lvm11.png)

Khi đã thực hiện xong thì nhấn `w` để lưu lại những thay đổi và sẽ thoát ra. Làm tương tự như trên ta tạo ra phân vùng `sdc1` và `sdc2` từ `/dev/sdc`

* B3: Tạo physical volume :
  * Tạo các physical volume là `/dev/sdb1` , `/dev/sdc1` , `/dev/sdb2` , `/dev/sdc2`
```
# pvcreate /dev/sdb1 /dev/sdc1 /dev/sdb2 /dev/sdc2
```
![](/image/lvm12.png)

* B4: Tạo volume group :
  * Nhóm các physical volume thành 1 volume group bằng câu lệnh
```
vgcreate vg-demo1 /dev/sdb1 /dev/sdc1    ( vg-demo1 là tên volume group )
```
![](/image/lvm13.png)

* B5 : Tạo logical volume
  * Từ 1 volume group , có thể tạo ra các logical volume bằng lệnh 
```
# lvcreate -L 50G -n lv-xfs vg-demo1
# lvcreate -L 50G -n lv-ext4 vg-demo1
```
  * `-L` là dung lượng của logical volume
  * `-n` là tên của logical volume

![](/image/lvm14.png)

* B6 : Format logical volume
```
# mkfs.xfs /dev/vg-demo1/lv-xfs
```
![](/image/lvm15.png)

```
# mkfs.ext4 /dev/vg-demo1/lv-ext4
```

![](/image/lvm16.png)

B7 : Mount và sử dụng :
```
# mkdir dataxfs
# mount /dev/vg-demo1/lv-xfs dataxfs
# mkdir dataext4
# mount /dev/vg-demo1/lv-ext4 dataext4
# df -h => kiểm tra
```

![](/image/lvm17.png)

* B8 : Lưu cấu hình vào file /etc/fstab
```
# echo /dev/vg-demo1/lv-xfs dataxfs xfs defaults 0 0 >> /etc/fstab
# echo /dev/vg-demo1/lv-ext4 dataext4 ext4 defaults 0 0 >> /etc/fstab
```

## Thay đổi dung lượng Logical volume trên LVM

### Tăng kích thước dung lượng logical volume :
```
# lvextend -L +5G /dev/vg-demo1/lv-xfs
```
### Giảm kích thước logical volume :
  * Trước tiên phải unmount logical volume muốn giảm 
```
# umount /dev/vg-demo1/lv-xfs
```
  * Giảm kích thước của logical volume :
```
# lvextend -L 5G /dev/vg-demo1/lv-xfs
```
  * Format lại logical volume :
```
# mkfs.xfs /dev/vg-demo1/lv-xfs
```
  * Mount lại logical volume 
```
# mount /dev/vg-demo1/lv-xfs dataxfs
```

## Thay đổi dung lượng Volume Group trên LVM

### thêm 1 partition vào volume group :
```
# vgextend /dev/vg-demo1 /dev/sdb2
```
![](/image/lvm18.png)
### Bỏ 1 partition ra khỏi volume group
```
vgreduce /dev/vg-demo1 /dev/sdb2
```
![](/image/lvm19.png)

## Cách xóa Logical Volumes , Volume Group , Physical Volume
### Xóa Logical Volume
 * B1 : Unmount logical volume 
```
# umount /dev/vg-demo1/lv-xfs
```
 * B2 : Xóa logical volume 
```
# lvremove /dev/vg-demo1/lv-xfs
```
### Xóa Volume Group
 * Trước khi xóa volume group , phải đảm bảo xóa hết logical volume :
 * Xóa volume group bằng lệnh :
```
# vgremove /dev/vg-demo1
```
### Xóa Physical Volume
```
# pvremove /dev/sdb2
```


 
 

