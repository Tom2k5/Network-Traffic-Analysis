### **Fuzzing**

- **Fuzzing** là một kỹ thuật tấn công trong đó kẻ tấn công gửi một lượng lớn dữ liệu không hợp lệ hoặc ngẫu nhiên đến một ứng dụng hoặc dịch vụ (ví dụ: máy chủ web) để tìm kiếm các lỗ hổng bảo mật.
- **Mục đích:**
    - **Thu thập thông tin**: Xác định cách ứng dụng xử lý dữ liệu đầu vào bất thường.
    - **Khai thác lỗ hổng**: Phát hiện các điểm yếu có thể bị khai thác, chẳng hạn như tràn bộ đệm (buffer overflow) hoặc lỗi xử lý dữ liệu.

---

### **Cách phát hiện tấn công Fuzzing**

1. **Lưu lượng HTTP/HTTPS bất thường từ một máy chủ**:
    - Nếu chúng ta nhận thấy một máy chủ cụ thể đang tạo ra lượng lớn lưu lượng HTTP hoặc HTTPS đến máy chủ web của mình, đây có thể là dấu hiệu của tấn công fuzzing.
    - Ví dụ: Một địa chỉ IP duy nhất gửi hàng nghìn yêu cầu HTTP trong thời gian ngắn.
2. **Kiểm tra nhật ký truy cập (Access Logs)**:
    - Nhật ký truy cập của máy chủ web có thể cung cấp thông tin chi tiết về các yêu cầu HTTP/HTTPS.
    - Chúng ta có thể kiểm tra nhật ký để tìm các mẫu hành vi bất thường, chẳng hạn như:
        - Các yêu cầu lặp lại với dữ liệu đầu vào khác nhau.
        - Các yêu cầu chứa dữ liệu không hợp lệ hoặc ngẫu nhiên.
## Directory Fuzzing
- **Directory fuzzing** là một kỹ thuật được kẻ tấn công sử dụng để tìm kiếm tất cả các trang web và vị trí có thể có trên ứng dụng web của bạn. Bạn có thể phát hiện điều này bằng cách phân tích lưu lượng mạng, giới hạn phạm vi quan sát của Wireshark (một công cụ phân tích gói tin) chỉ còn lưu lượng HTTP.
- **Việc phát hiện "directory fuzzing" khá đơn giản, vì nó thường có các dấu hiệu sau:**
    - Một máy chủ liên tục cố gắng truy cập các tập tin không tồn tại trên máy chủ web của bạn (thường trả về mã lỗi 404).
    - Máy chủ đó gửi các yêu cầu này một cách nhanh chóng và liên tục.

![](../../Image/image%2016.png)

- **Cách kẻ tấn công tránh bị phát hiện:**
    - **Gửi các yêu cầu rải rác trong một khoảng thời gian dài hơn:** Thay vì gửi yêu cầu liên tục, kẻ tấn công có thể làm chậm tốc độ để tránh bị phát hiện.
    - **Gửi các yêu cầu từ nhiều máy chủ hoặc địa chỉ nguồn khác nhau:** Kẻ tấn công có thể sử dụng nhiều máy tính hoặc địa chỉ IP khác nhau để thực hiện các yêu cầu fuzzing, khiến việc theo dõi và chặn trở nên khó khăn hơn.