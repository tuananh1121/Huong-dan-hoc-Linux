# **Hướng dẫn cài đặt Ubuntu Server 18.04 LTS**

Sau khi boot vào máy chủ với file iso hoặc đĩa cài đặt Ubuntu Server, chúng ta thực hiện các bước cài đặt như sau:

1. **Lựa chọn ngôn ngữ**

![](/image/ubuntu1.png)

=> Chọn ngôn ngữ mặc địng là **"ENGLISH"** hoặc ngôn ngữ nào mà bạn mong muốn.

2. **Kiểu bàn phím**

![](/image/ubuntu2.png)

=> Chọn mặc định là **ENGLISH US**

3. **Cấu hình network**

![](/image/ubuntu3.png)

Để mặc định => Bấm [ Done ] để tiếp tục

4. **Cấu hình Proxy Server**

![](/image/ubuntu4.png)

=> Bỏ trống

5. **Cấu hình Ubuntu Mirror**

![](/image/ubuntu5.png)

Đây là dường dẫn tên miền đặt máy chủ có chứa các package của Ubuntu, phục vụ cho việc cài đặt, nâng cấp Ubuntu.Vì mình ở Việt Nam nên muốn cài đặt và nâng cấp các package của Ubuntu được nhanh chóng nên mình thay đổi bằng địa chỉ Server đặt ở VN là http://opensource.xtdv.net/ubuntu

6. **Phân vùng ổ địa và hệ thống tệp tin**

![](/image/ubuntu6.png)

Chọn **“Use An Entire Disk”** nếu bạn là người mới bắt đầu.Chọn ổ đĩa để cài đặt.

![](/image/ubuntu7.png)

Thông tin phân vùng ổ đĩa => Chọn [ Done ] để tiếp tục.

![](/image/ubuntu8.png)

Một cảnh báo sẽ hiển thị, xác nhận ổ đĩa sẽ được format => Chọn [ Continue ] để tiếp tục

Cấu hình thông tin đăng nhập vào hệ thống bao gồm:

* Your name: Tên của bạn

* Your server’s name: Tên Server

* Pick a username: Tên đăng nhập

* Choose a password: Mật khẩu

* Confirm your password: Nhập lại mật khẩu của bạn

![](/image/ubuntu9.png)

7. Cài đặt **OpenSSH** để có thể đăng nhập vào Server bằng giao thức SSH

![](/image/ubuntu10.png)

Tích vào ô **“Install OpenSSH server”** bằng cách sử dụng phím [ **Space** ]

Một số loại phần mềm có sẵn để bạn có thể cài đặt, ở đây mình ko có nhu cầu cài đặt cài nào trong đây. Các bạn có thể chọn nếu muốn cài, sử dụng phím [ **Space** ] để lựa chọn.Quá trình cài đặt sẽ được bắt đầu.

![](/image/ubuntu11.png)

8. Sau khi cài đặt xong => Chọn [ **Reboot Now** ] để reboot lại máy chủ.

![](/image/ubuntu12.png)

Kết quả sau khi reboot, chúng ta thực hiện login vào Server bằng tài khoản đã tạo ở các bước trên.

Như vậy quá trình cài đặt Ubuntu Server 18.04 LTS đã kết thúc. Nếu trong quá trình cài đặt theo hướng dẫn bên trên mà gặp khó khăn gì thì vui lòng để lại comment bên dưới. Mình rất vui lòng được support các bạn.


