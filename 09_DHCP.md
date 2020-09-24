# DHCP (Dynamic Host Configuration Protocol)

### 1. DHCP là gì?
DHCP (Dynamic Host Configuration Protocol) là giao thức cấu hình tự động cho host.

### 2. DHCP dùng để làm gì
Nó cung cấp cho các host địa chỉ IP, Subnet Mask, default Gateway. Nó cung cấp 1 database trung tâm để theo dõi tất cả các máy tính trong hệ thống mạng. Mục đích là tránh trường hợp 2 máy tính khác nhau lại chung 1 địa chỉ IP. Ngoài việc cung cấp địa chỉ IP, DHCP còn cung cấp các thông tin cấu hình khác, cụ thể là DNS.

Và nó thường được cấp phát bởi DHPC server được tích hợp sẵn trên router.

DHCP giao tiếp bằng UDP và sử dụng port 67 và 68. DHCP server sử dụng port 67 để nghe thông tin từ các client và sử dụng port 68 để reply thông tin. Hiện nay DHCP có 2 version: IPv4 và IPv6.

#### Ưu và nhược điểm khi sử dụng DHCP:
##### Ưu điểm:
* giúp các thiết bị kết nối mạng nhanh chóng từ máy tính, laptop, điện thoại,...
* Quản lý địa chỉ IP một cách khoa học, tránh trường hợp trùng IP, đảm bảo cấu hình tự động cho mọi thiết bị kết nối qua mạng.
* Quản lý địa chỉ IP và các tham số TCP/IP dễ dàng qua các trạm.
* Các nhà quản trị mạng có thể thay đổi cấu hình và thông số IP để nâng cấp cơ sở hạ tầng.
* Các thiết bị có thể di chuyển tự do từ mạng này sang mạng khác và nhận IP tự động.

##### Nhược điểm:
* Việc sử dụng IP động của dhcp không phù hợp với các thiết bị cố định và cần truy cập liên tục như máy in, file server.
* DHCP thương chỉ được sử dụng tại các hộ gia đình hoặc mô hình mạng nhỏ.

#### Nguyên lý hoạt động của DHCP
![](/image/dhcp1.png)

Thành phần chính gồm 4 gói tin: DHCP DISCOVER,DHCP OFFER,DHCP REQUEST,DHCP ACK.

**DCHP DISCOVER**
Client tạo ra bản tin DISCOVER để yêu cầu cấp phát địa chỉ IP và gửi đi tới các server. Do chưa có địa chỉ IP nên nó sẽ lấy 0.0.0.0 làm địa chỉ IP nguồn và dùng địa chỉ 255.255.255.255 ( địa chỉ broadcast) làm địa chỉ đích và dùng để quảng bá tới các thiết bị đã kết nối.
**DHCP OFFER**
Sau khi nhận được bản tin DISCOVER Server sẽ kiểm tra hệ thống xem có địa chỉ IP nào phù hợp để cấp cho client. Server sẽ gửi bản tin OFFER bao gồm ( thông tin địa chỉ IP, subnetmask, thời gian thuê,...). Server sẽ gửi bản tin OFFER dưới dạng broadcast.IP nguồn là địa chỉ IP của server và IP đích là địa chỉ broadcast 255.255.255.255. Do có địa chỉ MAC của máy client trong gói tin nên tất cả các thiết bị trong mạng sẽ từ chối gói tin do nhận thấy gói tin đấy không dành cho mình.
**DHCP REQUEST**
Sau khi client nhận được bản tin OFFER thì nó sẽ gửi lại một bản tin REQUEST tới máy chủ.Trong trường hợp có nhiều máy chủ thì nó sẽ nhận gói tin đầu tiên của máy chủ nào gửi đến, rồi sẽ gửi bản tin phản hồi lại tất cả các máy chủ.
**DHCP ACK**
Server nhận được bản tin REQUEST thì sẽ trả lời lại client bằng một bản tin ACK xác nhận cấp cho client dùng địa chỉ IP đấy.


