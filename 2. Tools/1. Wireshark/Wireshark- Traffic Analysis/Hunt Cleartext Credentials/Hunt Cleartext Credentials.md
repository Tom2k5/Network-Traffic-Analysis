- Trong trường hợp tìm kiếm thông tin xác thực dưới dạng văn bản rõ, việc nhận diện nhiều lần nhập thông tin xác thực có thể không dễ dàng, và bạn sẽ phải quyết định xem có phải là một cuộc tấn công **brute-force** hay chỉ là một người dùng bình thường đã nhập sai thông tin.
- **Chức năng của Wireshark**: Một số dissectors của Wireshark (như FTP, HTTP, IMAP, POP và SMTP) được lập trình để trích xuất mật khẩu dưới dạng văn bản rõ từ tệp **capture**. Người dùng có thể xem các thông tin xác thực đã được phát hiện bằng cách sử dụng menu **"Tools --> Credentials".** 

⇒ Khuyến nghị thực hiện kiểm tra thủ công và không hoàn toàn dựa vào tính năng này để quyết định xem có thông tin xác thực dưới dạng văn bản rõ trong lưu lượng hay không.

![[image 34.png|image 34.png](../../../../../../../Image/image%2034.png)

---

### **Answer the question below:**

1. **What is the packet number of the credentials using "HTTP Basic Auth"?**

![[image 1 29.png|image 1 29.png](../../../../../../../Image/image%201%2029.png)

1. **What is the packet number where "empty password" was submitted?**
    
    ![[image 2 23.png|image 2 23.png](../../../../../../../Image/image%202%2023.png)