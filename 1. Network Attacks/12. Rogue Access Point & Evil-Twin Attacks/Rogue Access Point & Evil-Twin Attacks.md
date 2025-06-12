## **Rogue Access Point Attack**

![[image 9.png|image 9.png](../../Image/image%209.png)

- Chức năng chính là cung cấp quyền truy cập trái phép vào các phần bị hạn chế của mạng.
- Các điểm truy cập giả mạo được kết nối trực tiếp với mạng.

### **Cách Thức Tấn công**
- Kẻ tấn công có thể bí mật cài đặt một điểm truy cập giả mạo trong mạng bằng cách cắm thiết bị vào cổng Ethernet hoặc lợi dụng các lỗ hổng bảo mật để xâm nhập và cài đặt phần mềm giả lập AP. Khi người dùng kết nối vào Rogue AP, toàn bộ lưu lượng truy cập của họ sẽ đi qua thiết bị này, tạo điều kiện cho kẻ tấn công có thể chặn và đọc các thông tin nhạy cảm như mật khẩu và dữ liệu cá nhân.
## **Evil-Twin Attack**

![[image 1 7.png|image 1 7.png](../../Image/image%201%207.png)

- **Không Kết Nối Với Mạng Chính**: Khác với các **điểm truy cập giả mạo** (rogue access points), các điểm truy cập evil twin thường không kết nối trực tiếp với mạng chính. Thay vào đó, chúng hoạt động độc lập và có thể chứa một máy chủ web hoặc một ứng dụng khác để hoạt động như một "người trung gian" (man-in-the-middle) cho các thiết bị không dây.

### **Cách Thức Của Tấn Công Evil Twin**
1. **Thiết Lập Điểm Truy Cập Giả Mạo**: Kẻ tấn công tìm kiếm một vị trí đông người có Wi-Fi miễn phí, như quán cà phê hoặc sân bay, và sao chép tên mạng (SSID) của điểm truy cập hợp pháp.
2. **Tạo Mạng Wi-Fi Giả**: Kẻ tấn công thiết lập một điểm truy cập mới với tên SSID giống hệt như mạng hợp pháp và cố gắng phát tín hiệu mạnh hơn để thu hút người dùng kết nối.
3. **Kết Nối Người Dùng**: Khi người dùng cố gắng kết nối, thiết bị của họ có thể tự động chọn điểm truy cập evil twin nếu nó phát ra tín hiệu mạnh hơn.
4. **Giám Sát Lưu Lượng**: Sau khi người dùng kết nối, kẻ tấn công có khả năng giám sát lưu lượng mạng của họ, thu thập thông tin nhạy cảm mà người dùng truyền tải qua mạng.

## Solution
- To filter for beacon frames, we could use the following:
    - `(wlan.fc.type == 00) and (wlan.fc.type_subtype == 8)`
        1. `**wlan.fc.type == 00**`:
            - Trường `**fc.type**` trong tiêu đề gói tin không dây chỉ định loại gói tin. Giá trị `**00**` tương ứng với loại gói tin **quản lý** (Management Frame). Các gói tin quản lý được sử dụng để quản lý kết nối không dây, bao gồm các yêu cầu và phản hồi kết nối, xác thực, và thông báo ngắt kết nối.
        2. `**wlan.fc.type_subtype == 8**`:
            - Trường `**fc.type_subtype**` chỉ định kiểu con của gói tin quản lý. Giá trị `**8**` tương ứng với gói tin **beacon**. Gói tin beacon được gửi bởi điểm truy cập (Access Point) để quảng bá sự hiện diện của nó và cung cấp thông tin về mạng, chẳng hạn như SSID, khả năng bảo mật, và các thông tin khác liên quan đến mạng.

![[image 2 5.png|image 2 5.png](../../Image/image%202%205.png)

- Phân tích beacon là một phần quan trọng trong việc phân biệt giữa các điểm truy cập hợp pháp và giả mạo. Một trong những thông tin đầu tiên cần xem xét là thông tin Robust Security Network (RSN), cung cấp dữ liệu quý giá cho các thiết bị khách về các mã hóa được hỗ trợ, cùng với nhiều thông tin khác.
- **Legitimate access point's RSN information:**
    
    ![[image 3 4.png|image 3 4.png](../../Image/image%203%204.png)
    
- **Rogue AP’s RSN information:**

![[image 4 3.png|image 4 3.png](../../Image/image%204%203.png)

- Đặc biệt khi đối phó với các cuộc tấn công evil twin tinh vi hơn. Ví dụ, một kẻ tấn công có thể **sử dụng cùng một mã hóa** mà điểm truy cập của chúng ta sử dụng, làm cho việc phát hiện cuộc tấn công trở nên khó khăn hơn.
- Thông tin cụ thể của nhà cung cấp (vendor-specific information), có khả năng không có ở điểm truy cập của kẻ tấn công.