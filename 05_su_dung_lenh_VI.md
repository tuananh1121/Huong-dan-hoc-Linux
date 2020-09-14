# Hướng dẫn sử dụng cơ bản lênh Vi trên Linux

1. **Mở Fiile**

Vi <đường_dẫn_file>

2. **Chế độ Visual và Insert**

Bình thường khi mở file bằng lệnh Vi thì nó ở chế độ mặc định là Visual. Khi muốn chuyển sang chế độ Insert ta bấm phím **"i"** trên bàn phím. Dưới góc màn hình bên trái sẽ hiển thị chế độ Insert. Để quay trở lại chế độ Visual ta sử dụng phím **"ESC"**

3. **Di chuyển tới một dòng bất kỳ**

Ở chế độ Visual nhâp :<số dòng cần di chuyển đến>

VD :2 thì sẽ đưa ta di chuyển tới dòng thứ 2

4. **Di chuyển tới đầu File và cuối File**

Để di chuyển tới đầu File bấm phím **"G"**

Di chuyển tới cuối File bấm tổ hợp phím **"SHIFT+G"**

5. **Copy, paste, xóa một dòng file**

Để copy một dòng ta bấm **"YY"**

Để paste một dòng ta bấm **"P"**

Để xóa một dòng tâ bấm **"DD"**

Bấm phím **"U"** để hoàn tác lại thao tác file bị lỗi

6.**Lưu File**

Lưu File đã chỉnh sửa và không thoát bấm **:w**

Lưu File đã chỉnh sửa và thoát bấm **:wq**

Thoát và không lưu File **:q!**

7. **Tạo thư mục**

**"mkdir"** dùng để tạo thư mục mới

VD: mkdir picture thì sẽ tạo được một thư mục mới là **picture**

8. **Xem và tạo file**

Muốn tạo một File ta dùng lệnh **"cat > test.txt"** hoặc **"> test.txt"** hoặc **"touch test.txt"**

Sau khi đã tạo File, muốn xem file ta dùng **cat test.txt**

9. **Di chuyển và đổi tên File**

Thường thường lệnh **"mv"** dùng để di chuyển File, nhưng nó cũng dùng để đổi tên File.

VD: mv test.txt test.doc


