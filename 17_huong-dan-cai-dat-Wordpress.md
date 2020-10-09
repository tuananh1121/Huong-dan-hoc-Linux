# Hướng dẫn cài đặt Wordpress
## Tổng quan
Wordpress là một mã nguồn web mở được phát hành đầu tiên vào ngày 27/5/2003 bởi Matt Mullenweg và Mike Little, nó được phát triển để quản trị nội dung (CMS – Content Management System ) và cũng là một nền tảng blog (Blog Platform) được viết trên ngôn ngữ PHP sử dụng hệ quản trị cơ sở dữ liệu MySQL. Đến nay WordPress đã trở thành một trong những hệ thống quản lí website phổ biến nhất thế giới với hơn 60 triệu website (số liệu năm 2019).
## Cài đặt WordPress
### Chuẩn bị 
Chuẩn bị 2 máy ảo. 1 máy chạy HĐH centos7 và 1 máy chạy HĐH Window 10.

Cấu hình máy Window:
  * RAM 2GB
  * 1 CPU
  * VMnet8 (NAT)
  
Cấu hình máy Centos 7:
  * RAM 2GB
  * 1 CPU
  * ens33
### Mô hình bài lab như sau

![](/image/wp1.png)

Vì đây là môi trường lab nên ta tiến hành tắt firewalld trên máy ảo centos7.
```
seteforce 0
systemctl stop firewalld
```
## Cấu hình
### Trên máy Centos
 * Bước 1: Cài đặt MySQL Server
```
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm

rpm -ivh mysql-community-release-el7-5.noarch.rpm

yum install mysql-server
```

### Khởi động MySQL
```
systemctl start mysql
```

Chạy lệnh sau để thiết lập tài khoản `root` cho MySQL
```
mysql_secure_installation
```

Để kiểm tra ta đăng nhập thử với tài khoản `root`

 * Bước 2: Tạo Database cho wordpress
Đăng nhập vào MySQL với user `root`
```
mysql -u root -p
```
Tạo user và database để sử dụng cho wordpress
```
create database tên-database;

create user 'user'@'IP' identified by 'pass';

grant all privileges on tên-database to 'user'@'IP';

flush privileges;
```
Trong đó 
   * tên-database là tên database sử dụng cho wordpress
   * user là tên user sử dụng để wordpress kết nối vào MySQL.
   * IP là địa chỉ của máy Web Server để truy cập MySQL

![](/image/wp2.png)

### Cài Apache
Chạy lệnh sau để cài đặt 
```
yum install httpd -y
```
Khởi động Apache và cho phép khởi động cùng hệ thống
```
systemctl start httpd

systemctl enable httpd
```
Để kiểm tra ta mở trình duyệt bên window và truy cập vào địa chỉ IP
```
http://192.168.44.138
```
![](/image/wp3.png)

### Cài đặt WordPress
* Cài đặt `wget`
```
yum install wget
```
* Tiến hành cài đặt wordpress phiên bản mới nhất
```
wget http://wordpress.org/latest.tar.gz
```
* giải nén file `latest.tar.gz`
```
tar -xvfz latest.tar.gz
```
* Copy các file trong thư mục WordPress tới đường dẫn /var/www/html
```
cp -Rvf /root/wordpress/* /var/www/html
```
### Cấu hình wordpress
* Truy cập vào file cài đặt của wordpress
```
cd /var/www/html
```
* File cấu hình wordpress là wp-config.php. Tuy nhiên tại đây chỉ có file wp-config-sample.php. Tiến hành sửa lại tên file cấu hình như sau
```
mv wp-config-sample.php wp-config.php
```
* Mở file config để chỉnh sửa
```
vi wp-config.php
```
![](/image/wp4.png)

Lưu file cấu hình và thoát.

### Cài đặt php
Phiên bản có sẵn trong repo của CentOS đang là 5.4. Phiên bản này khá cũ và sẽ khiến bạn gặp một số vấn đề xảy ra khi tiến hành cài đặt wordpress. Vì vậy bạn cần phải cài đặt phiên bản 7x để khắc phục. Bạn cần tiến hành thêm vào kho Remi và EPEL trên hệ thống CentOS:
* Cài đặt EPEL
```
yum install epel-release
```
* Cài đặt kho lưu trữ Remi
```
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
```
* Tiến hành cài đặt php. Ở đây ta cần lưu ý về phiên bản cài đặt như sau:
    * Phiên bản 7.1
```
yum --enablerepo = Rem-php71
```

    * Phiên bản 7.2
	
```
yum --enablerepo = Rem-php72
```

    * Phiên bản 7.3
	
```
yum --enablerepo = Rem-php73
```

    * Phiên bản 7.4 
	
```
yum --enablerepo = Rem-php74
```
Ở đây mình dừng phiên bản 7.3
* Cài đặt gói hỗ trợ `php-dg`
```
yum install php-gd
```
Để kiểm tra ta vào Window mở trình duyệt và truy cập vào địa chỉ `http://192.168.44.138/info.php`
![](/image/wp5.png)

### Cài đặt giao diện
Truy cập vào địa chỉ `http://192.168.44.138/wp-admin/install.php` để tiến hành cài đặt.
Màn hình đăng nhập sau khi cài đặt xong
![](/image/wp6.png)




