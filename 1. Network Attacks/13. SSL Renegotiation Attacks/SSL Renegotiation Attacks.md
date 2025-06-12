- Encryption attacks we should be aware of like the `heartbleed vulnerability`
- Reference: [https://heartbleed.com/](https://heartbleed.com/)

## **TLS and SSL Handshakes**
- In order to establish an encrypted connection, the client and server must undergo the handshake process.
- TLS and SSL handshakes are mostly similar in their steps.

![[image 19.png|image 19.png](../../Image/image%2019.png)

### **1. Client Hello**
- **Mục đích**: Máy khách gửi thông tin ban đầu để bắt đầu quá trình kết nối.
- **Nội dung**:
    - **Phiên bản TLS/SSL hỗ trợ**: Ví dụ: TLS 1.2, TLS 1.3.
    - **Danh sách cipher suites**: Các thuật toán mã hóa mà máy khách có thể sử dụng (ví dụ: AES, RSA).
    - **Dữ liệu ngẫu nhiên (nonce)**: Một chuỗi số ngẫu nhiên dùng để tạo khóa phiên (session key).
### **2. Server Hello**
- **Mục đích**: Máy chủ phản hồi và chọn thông số kết nối.
- **Nội dung**:
    - **Phiên bản TLS/SSL được chọn**: Phiên bản cao nhất mà cả hai hỗ trợ.
    - **Cipher suite được chọn**: Thuật toán mã hóa sẽ dùng cho phiên kết nối.
    - **Nonce của server**: Một chuỗi số ngẫu nhiên khác từ máy chủ.
### **3. Certificate Exchange (Trao đổi chứng chỉ)**
- **Mục đích**: Xác thực danh tính máy chủ.
- **Nội dung**:
    - Máy chủ gửi **chứng chỉ số SSL/TLS** cho máy khách.
    - Chứng chỉ chứa **khóa công khai (public key)** của máy chủ, được dùng để mã hóa dữ liệu.
### **4. Key Exchange (Trao đổi khóa)**
- **Mục đích**: Tạo khóa bí mật dùng chung cho phiên kết nối.
- **Quy trình**:
    - Máy khách tạo **premaster secret** (khóa tạm thời).
    - Mã hóa **premaster secret** bằng **public key** của máy chủ và gửi nó đến server.
### **5. Session Key Derivation (Tạo khóa phiên)**
- **Mục đích**: Tạo khóa đối xứng (symmetric key) để mã hóa dữ liệu.
- **Quy trình**:
    - Cả máy khách và máy chủ kết hợp **nonce** (từ Client Hello và Server Hello) với **premaster secret** để tính toán **session key**.
    - **Session key** sẽ dùng để mã hóa/giải mã dữ liệu trong suốt phiên kết nối.
### **6. Finished Messages (Xác nhận hoàn tất)**
- **Mục đích**: Xác minh tính toàn vẹn của quá trình bắt tay.
- **Quy trình**:
    - Cả hai bên gửi thông điệp **Finished** đã được mã hóa bằng **session key**.
    - Thông điệp chứa **hash** (giá trị băm) của tất cả các bước trước đó để đảm bảo không có dữ liệu bị can thiệp.
### **7. Secure Data Exchange (Trao đổi dữ liệu an toàn)**

- **Mục đích**: Truyền dữ liệu qua kênh mã hóa.
- **Quy trình**:
    - Tất cả dữ liệu (trang web, hình ảnh, API requests) được mã hóa bằng **session key**.
    - Chỉ máy khách và máy chủ có thể giải mã dữ liệu này.

## **Diving into SSL Renegotiation Attacks**

- **Lọc thông điệp bắt tay trong Wireshark**:

```Plain
ssl.record.content_type == 22
```

- **Content type 22** đại diện cho các thông điệp bắt tay (Handshake Messages).

![[image 1 15.png|image 1 15.png](../../Image/image%201%2015.png)

1. **Nhiều thông điệp Client Hello**:
    - Một dấu hiệu rõ ràng của tấn công SSL Renegotiation là việc xuất hiện nhiều thông điệp **Client Hello** từ một máy khách trong thời gian ngắn.
    - Kẻ tấn công lặp lại thông điệp này để kích hoạt tái đàm phán, với hy vọng ép máy chủ sử dụng bộ mã hóa yếu hơn.
2. **Thông điệp bắt tay không theo thứ tự**:
    - Trong một số trường hợp, chúng ta có thể thấy các thông điệp bắt tay không theo thứ tự do mất gói tin hoặc các vấn đề khác.
    - Tuy nhiên, nếu máy chủ nhận được thông điệp **Client Hello** sau khi quá trình bắt tay đã hoàn tất, đây có thể là dấu hiệu của tấn công SSL Renegotiation.

### **The objective of SSL Renegotiation Attack**
1. **Từ chối dịch vụ (Denial of Service - DoS)**:
    - Tấn công SSL Renegotiation tiêu tốn nhiều tài nguyên máy chủ, có thể làm máy chủ quá tải và không phản hồi được các yêu cầu hợp lệ.
2. **Khai thác điểm yếu của SSL/TLS**:
    - Kẻ tấn công có thể ép máy chủ sử dụng các bộ mã hóa yếu hơn thông qua tái đàm phán, từ đó dễ dàng giải mã dữ liệu.
3. **Phân tích mật mã (Cryptanalysis)**:
    - Kẻ tấn công có thể sử dụng tái đàm phán như một phần của chiến lược phân tích mật mã để tìm hiểu các mẫu SSL/TLS và tấn công các hệ thống khác.