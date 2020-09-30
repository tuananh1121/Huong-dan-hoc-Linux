# Cấu tạo ổ HDD
![](/image/disk1.png)

Gồm 4 thành phần chính : Cụm đĩa, cụm đầu đọc, cụm mạch điện, vỏ đĩa cứng.

## 1. Cụm đĩa

### 1.1 Đĩa từ
 * đĩa bằng nhôm, thủy tinh hoặc sứ
 * Có khả năng lưu trữ tốt, không bị nhả từ
 * Đĩa được phủ vật liệu từ ở cả hai mặt và bao bọc bằng lớp vỏ bảo vệ.
 
### 1.2 Trục quay
 * Là bộ phận dịch chuyển đầu từ
 * Nhiều loại đĩa cứng sử dụng mô tơ cuộn dây di động (voice coil motor) hay gọi là mô tơ cuộn dây quay (rotary coil) hoặc servo để điều khiển chuyển động của đầu từ.

### 1.3 Động cơ
 * Yếu tố xác định chất lượng ổ cứng là tốc độ mà đĩa từ lướt qua dưới đầu đọc/ghi. Đĩa từ lướt qua đầu từ với tốc độ khá cao ( ít nhất là 3600 vòng/phút)
 * Mô tơ trục (spindle mô tơ) có chức năng làm quay các đĩa từ
 * Mô tở trục là loại không có chổi quét, chiều cao thấp, dùng điện 1 chiều, tương tự mô tơ trong ổ đĩa mềm.
## 2. Cụm đầu đọc
 * Cấu tạo gồm lõi ferit và cuộn dây (giống như nam châm điện)
 * Đầu đọc trong đĩa cứng đọc dữ liệu dưới dạng từ hóa trên bề mặt đĩa từ hoặc từ hóa lên các mặt đĩa khi ghi dữ liệu.
 * Số đầu đọc ghi luôn bằng số mặt hoạt động được của các đĩa từ
 
## 3. Cụm mạch điện
 * Mạch điều khiển: Có nhiệm vụ điều khiển động cơ đồng trục, điều khiển sự di chuyển của cần di chuyển đầu đọc để đảm bảo đến đúng vị trí bề mặt đĩa.
 * Mạch xử lý dữ liệu: dùng để xử lý những dữ liệu đọc/ghi của ổ đĩa cứng.
 * Bộ nhớ đệm: nơi tạm lưu dữ liệu trong quá trình đọc/ghi dữ liệu. Dữ liệu trên bộ nhớ đệm sẽ mất đi khi ổ đĩa cứng ngừng được cấp điện.
 * Đầu cắm nguồn cung cấp điện cho ổ đĩa cứng.
 * Đầu kết nối giao tiếp với máy tính
 * Các cầu nối (jumper): lựa chọn chế độ làm việc của ổ đĩa cứng (SATA 150 hoặc SATA 300) hay thứ tự trên các kênh giao tiếp IDE (master hoặc slave hoặc tự chọn) lựa chọn các thông số làm việc khác
 
## 4. Vỏ đĩa cứng.
 * Chứa các linh kiện gắn trên nó, phần nắp đậy lại bảo vệ các linh kiện bên trong
 * Vỏ đĩa cứng nhằm định vị các linh kiện, chịu đựng va chạm ở mức thấp để bảo vệ ổ đĩa cứng, không cho bụi vào trong ổ đĩa cứng.
 * Võ đĩa có lỗ thoáng nhằm đảm bảo cản bụi và cân bằng áp suất môi trường ngoài và môi trường trong.
 
## 5. Cấu trúc dữ liệu của đĩa cứng

![](/image/disk2.png)

### 5.1 Track
 * Các vòng tròn đồng tâm trên mặt đĩa dùng để xác định các vùng lưu trữ dữ liệu riêng biệt trên mặt đĩa.
 * Mặc định, các track không cố định, chúng sẽ thay đổi vị trí khi được định dạng ở cấp thấp (low format) nhằm tái cấu trúc cho phù hợp khi đĩa bị hư hỏng (bad block) do xuống cấp của phần cơ.
 * Tập hợp các track cùng bán kính của mặt đĩa khác nhau để tạo thành các trụ (cylinder), có 1024 cylinder ( từ 0-1023 ). Mỗi ổ cứng sẽ có nhiều cylinder vì có nhiều đĩa từ khác nhau.

### 5.2 Sector
 * Mỗi track được chia thành những phần nhỏ bằng các đoạn hướng tâm tạo thành các sector (cung từ). Sector là đơn vị chứa dữ liệu nhỏ nhất. Theo chuẩn thông thường thì một sector chứa (dung lượng) 512 byte
 * Số sector trên các track từ phần rìa đĩa vào đến vùng tâm đĩa là khác nhau, các ổ đĩa cứng đều chia ra hơn 10 vùng mà trong mỗi vùng có số sector/track bằng nhau.
 
### 5.3 Cluster
 * Trong lĩnh vực lưu trữ dữ liệu(đĩa mềm hoặc đĩa cứng) ở mức độ hệ điều hành, cluster là một đơn vị lưu trữ gồm một hoặc nhiều sector.
 * Khi hệ điều hành lưu trữ một tập tin vào đĩa, nó ghi tập tin đó vào hàng chục, có khi hàng trăm cluster liền nhau. Nếu không sẵn cluster liền nhau, hệ điều hành sẽ tìm kiếm cluster còn trống ở kế đó và ghi tệp tin lên đĩa. Quá trình cứ tiếp tục như vậy chho đến khi toàn bộ được cất giữ hết.
 
### 5.4 Cylinder
 * Tập hợp các track cùng bán kính ở các mặt đĩa khác nhau tạo thành các cylinder. Hai mặt trên đĩa, cylinder sẽ bao gồm 1 rãnh của mặt trên và 1 rãnh của mặt dưới.
 * Trên các đĩa cứng sắp xếp cái này chồng lên cái kia, một cylinder gồm các rãnh trên cả hai mặt của tất cả các đĩa. Trên một ổ cứng có nhiều cylinder bởi có nhiều track trên mỗi mặt đĩa từ
 