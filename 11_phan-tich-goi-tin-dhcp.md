# Phân tích gói tin DHCP
## lệnh `tcpdump`
`tcpdump` là 1 câu lệnh dùng để bắt và phân tích gói của 1 giao thức trong máy tính.

### Các option lệnh của `tcpdump`
* Hiển thị các giao diện mạng: `# tcpdump -D`
* Bắt gói tin ở các giao diện mạng: `# tcpdump -i <giao_diện_mạng(ens33, ens37,...)>`
* Bắt gói tin với n gói tin: `# tcpdump -i <giao_diện_mạng> -c <n>`
* Hiển thị gói tin dưới dạng HEX và ASCII: `# tcpdump -xx -i <giao_diện_mạng> -c 2`
* Bắt gói tin với 1 port cụ thể: `# tcpdump -i <giao_diện_mạng> port <số_hiệu_port>`

### Kiểm tra và cài đặt `tcpdump`
```
# yum install tcpdump
# tcpdump -D
```
Sau đó kiểm tra lại ta thấy được các interface đang có sẵn để theo dõi:

![](/image/dhcp4.png)

Để bắt gói tin DHCP cấp lần đầu tiên, ta phải setup lại Client không nhận IP từ DHCP Server. Sửa file sau: `/etc/sysconfig/network-scripts/ifcfg-ens37`

![](/image/dhcp5.png)

Sau đó reboot lại

Bây giờ, interface `ens37` chưa được set địa chỉ IP. Ta sẽ mở 2 terminal. 1 bên chỉnh sửa file cấu hình interface `ens37`- ta đặt `BOOTPROTO = "dhcp"`, 1 bên sử dụng tcpdump để bắt gói tin khi DHCP cung cấp IP

![](/image/dhcp6.png)
![](/image/dhcp7.png)

Ta sẽ giải phóng IP đã gán cho card ens37 khi khởi động:
```
dhclient -r
```

Sau đó Yêu cầu cấp lại địa chỉ IP:
```
dhclient -v 
```

Ta sẽ bắt được 4 gói tin

![](/image/dhcp8.png)

## Kiểm tra file log DHCP

### Danh sách địa chỉ IP đã cấp phát
Để xem danh sách những địa chỉ IP đã cấp phát ta dùng lệnh `cat /var/lib/dhcpd/dhcpd.leases`

![](/image/dhcp9.png)

### Quá trình cấp phát địa chỉ của DHCP

![](/image/dhcp10.png)




