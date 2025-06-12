- Nhà phân tích bảo mật cần biết cách sử dụng các tệp khóa để giải mã lưu lượng đã được mã hóa và điều tra hoạt động của lưu lượng.
- Các gói dữ liệu sẽ xuất hiện với màu sắc khác nhau khi lưu lượng HTTP được mã hóa. Thông tin chi tiết về giao thức và thông tin (địa chỉ URL thực tế và dữ liệu trả về từ máy chủ) sẽ không hoàn toàn hiển thị.

![[image 33.png|image 33.png](../../../../Image/image%2033.png)

![[image 1 28.png|image 1 28.png](../../../../Image/image%201%2028.png)

## **Quy trình bắt tay TLS**

- The TLS protocol has its handshake process. The first two steps contain **"Client Hello"** and **"Server Hello"** messages.
- **The given filters show the initial hello packets in a capture file:**

⇒ These filters are helpful to spot which IP addresses are involved in the TLS handshake:

- **Client Hello:** `**(http.request or tls.handshake.type == 1) and !(ssdp)**`
- **Server Hello:** `**(http.request or tls.handshake.type == 2) and !(ssdp)**`

![[image 2 22.png|image 2 22.png](../../../../Image/image%202%2022.png)

## Encryption key log file

- **Encryption key log file** là một tệp văn bản chứa các cặp khóa duy nhất để giải mã phiên lưu lượng đã được mã hóa. Những cặp khóa này được tạo tự động (per session) khi một kết nối được thiết lập với một trang web hỗ trợ SSL/TLS.
- Để thực hiện điều này, bạn cần cấu hình hệ thống của mình và sử dụng trình duyệt phù hợp (Chrome và Firefox hỗ trợ) để lưu những giá trị này dưới dạng tệp nhật ký khóa. Bạn cần thiết lập một biến môi trường và tạo tệp **SSLKEYLOGFILE**, sau đó trình duyệt sẽ ghi lại các khóa vào tệp này khi bạn duyệt vào web.
- **Adding key log files with the** `**"right-click"**`**menu:**

![[image 3 16.png|image 3 16.png](../../../../Image/image%203%2016.png)

- **Adding key log files with the** `**"Edit --> Preferences --> Protocols --> TLS"**` **menu:**

![[image 4 14.png|image 4 14.png](../../../../Image/image%204%2014.png)

- **Viewing the traffic with/without the key log files:**

![[image 5 12.png|image 5 12.png](../../../../Image/image%205%2012.png)

⇒ Khi không sử dụng tệp khóa TLS (key log file), bạn sẽ không thấy gói tin HTTP nào trong lưu lượng HTTPS vì các gói tin này được mã hóa.

⇒ **Mã hóa dữ liệu**: Giao thức HTTPS sử dụng TLS để mã hóa dữ liệu giữa trình duyệt và máy chủ. Khi dữ liệu được mã hóa, nội dung của các gói tin không thể được đọc hoặc phân tích nếu không có khóa giải mã. Tệp khóa TLS chứa các cặp khóa cần thiết để giải mã những gói tin này.

### Giải mã bằng TLS key

- Khi giải mã lưu lượng HTTPS bằng tệp khóa TLS, **các gói tin HTTP có thể bị chia thành nhiều phần.** Điều này xảy ra vì một số lý do sau:
    - **Cấu trúc gói tin**: Giao thức TLS mã hóa dữ liệu và chia nó thành các gói tin nhỏ hơn để truyền tải qua mạng. Khi dữ liệu được gửi từ client đến server (hoặc ngược lại), nó có thể được chia thành nhiều đoạn (chunks) để đảm bảo việc truyền tải an toàn và hiệu quả.
    - **Giải mã và tái tạo**: Khi sử dụng tệp khóa TLS để giải mã, các gói tin HTTP có thể xuất hiện dưới dạng nhiều phần khác nhau, tùy thuộc vào cách mà dữ liệu đã được chia nhỏ trong quá trình truyền tải.
    - **Các định dạng dữ liệu khác nhau**: Sau khi giải mã, bạn có thể thấy rằng thông tin trong các gói tin HTTP được trình bày ở nhiều định dạng khác nhau (như tiêu đề đã giải nén, gói tin TLS đã giải mã, v.v.), điều này cho phép phân tích chi tiết hơn về nội dung và cấu trúc của thông điệp.
- Lưu ý rằng thông tin chi tiết về gói tin và bảng byte cung cấp dữ liệu ở nhiều định dạng khác nhau để phục vụ cho việc điều tra. Thông tin tiêu đề đã giải nén và chi tiết gói HTTP2 có sẵn sau khi giải mã lưu lượng. Tùy thuộc vào chi tiết của gói tin, bạn cũng có thể có các định dạng dữ liệu sau:
    - **Frame**: Định dạng khung, thể hiện cấu trúc của gói tin.
    - **Decrypted TLS**: TLS đã được giải mã, cho phép xem nội dung bên trong.
    - **Decompressed Header**: Tiêu đề đã được giải nén, cung cấp thông tin chi tiết hơn về gói tin.
    - **Reassembled TCP**: TCP đã được tái tạo, cho phép xem luồng dữ liệu liên tục.
    - **Reassembled SSL**: SSL đã được tái tạo, cho phép phân tích các kết nối bảo mật.

⇒ Việc phát hiện các hoạt động đáng ngờ trong các **tệp chunked** (tệp chia nhỏ) trở nên dễ dàng hơn và là một cách tuyệt vời để học cách tập trung vào các chi tiết.

---

## Answer the questions:

1. **What is the frame number of the "Client Hello" message sent to "[accounts.google.com](http://accounts.google.com/)"?**

![[image 6 7.png|image 6 7.png](../../../../Image/image%206%207.png)

- `**ssl.handshake.type == 1**` filters for **"Client Hello"** messages.
- `**ssl.handshake.extensions_server_name == "accounts.google.com"**` ensures that you are specifically looking for messages directed at **"accounts.google.com"** as there are many domain name used on an IP address.

1. **Decrypt the traffic with the "KeysLogFile.txt" file. What is the number of HTTP2 packets?**
    
    ⇒ `http` ⇒ **115**
    
2. **Go to Frame 322. What is the authority header of the HTTP2 packet? (Enter the address in defanged format.)**  
    • Với   
    `**:authority**`, HTTP/2 cung cấp một nguồn duy nhất và rõ ràng cho thông tin về máy chủ.
    
    ⇒ Xác định máy chủ mà client muốn kết nối, giúp cải thiện tính nhất quán và hiệu quả trong quá trình truyền tải dữ liệu.
    

![[image 7 5.png|image 7 5.png](../../../../Image/image%207%205.png)

1. **Investigate the decrypted packets and find the flag! What is the flag?**

![[image 8 3.png|image 8 3.png](../../../../Image/image%208%203.png)

- Export Object để lấy flag.