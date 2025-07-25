# **Filtering for Web Traffic**

### SSDP là gì?

"SSDP" là viết tắt của **Simple Service Discovery Protocol**

Đây là một giao thức mạng được sử dụng để khám phá các thiết bị và dịch vụ trên mạng cục bộ (LAN). SSDP thường được liên kết với UPnP (Universal Plug and Play), một tập hợp các giao thức cho phép các thiết bị tự động tìm và kết nối với nhau mà không cần cấu hình thủ công.

- **Chức năng**: SSDP cho phép các thiết bị (như máy in, TV thông minh, hoặc máy chủ media) quảng bá sự hiện diện của chúng trên mạng và cho phép các thiết bị khác (như máy tính hoặc điện thoại) tìm kiếm chúng.
- **Cách hoạt động**: Nó sử dụng các thông điệp HTTP đa hướng (multicast) qua UDP, thường trên cổng 1900, để gửi các thông báo "alive" (tôi đang hoạt động) hoặc phản hồi các yêu cầu tìm kiếm.
- **Ví dụ**: Khi bạn cắm một thiết bị UPnP vào mạng, nó có thể gửi thông điệp SSDP để thông báo cho các thiết bị khác biết về sự tồn tại của nó.
- `(http.request or tls.handshake.type eq 1) and !(ssdp)`

![](../../../../Image/image%2027.png)

⇒ `**hxxp://194.55.224[.]9/liuz/five/fre.php**`

⇒ Unencrypted HTTP POST requests associated with **[Loki Bot malware](https://malpedia.caad.fkie.fraunhofer.de/details/win.lokipws)** .

- Các POST request cố đánh cắp thông tin.

⇒ Lọc bằng `(http.request or tls.handshake.type eq 1)` để thấy kết nối HTTPs và các request như `GET` , `POST` .

# **Creating Filter Buttons**

|**Button Label**|**Filter Expression**|
|---|---|
|basic|basic (http.request or tls.handshake.type eq 1) and !(ssdp)|
|basic+|basic (http.request or tls.handshake.type eq 1 or (tcp.flags.syn eq 1 and tcp.flags.ack eq 0)) and !(ssdp)|
|basic+dns|basic (http.request or tls.handshake.type eq 1 or (tcp.flags.syn eq 1 and tcp.flags.ack eq 0) or dns) and !(ssdp)|

- `tcp.flags.syn eq 1`: Lọc các gói TCP có cờ SYN (synchronize) được bật, biểu thị khởi đầu của một kết nối TCP (bước đầu trong TCP 3-way handshake).
- `tcp.flags.ack eq 0`: Đảm bảo cờ ACK (acknowledgment) không được bật, nghĩa là đây là gói SYN đầu tiên, không phải gói phản hồi.
    
    **Mục đích:**
    
    - Hiển thị cả lưu lượng web (HTTP/HTTPS) và lưu lượng không phải web (non-web traffic) trong file PCAP.
    - Phát hiện các **kết nối TCP thất bại**: Nếu có nhiều gói SYN mà không có phản hồi (SYN-ACK), điều này có thể chỉ ra một máy chủ Command-and-Control (C2) không hoạt động tại thời điểm ghi lại PCAP.
- `dns`: Lọc các gói thuộc giao thức DNS (Domain Name System), dùng để phân giải tên miền thành địa chỉ IP.
    
    **Mục đích**
    
    - Mở rộng thêm để bao gồm lưu lượng DNS, vì các hoạt động độc hại thường liên quan đến truy vấn DNS (ví dụ: liên lạc với tên miền của máy chủ C2).
- **Example:**

![](../../../../Image/image%201%2022.png)

⇒ Client đang cố kết nối đến Malicious Server (magiketchinn.com) bằng cách nhận biết các SYN cố kết nối server offline.

⇒ Lọc bằng `tcp.flags.syn eq 1` để thấy.

# **Filtering for Non-Web Traffic**

- We can find a DNS query for adaisreal.ddns[.]net that resolves to `87.121.221[.]212`, then a TCP segment to that IP address with the `SYN flag` over TCP port `7888` .
- This pcap contains post-infection traffic generated by a Remote Access Tool (RAT) malware called **[Ave Maria RAT (also known as Warzone RAT)](https://malpedia.caad.fkie.fraunhofer.de/details/win.ave_maria)**.

![](../../../../Image/image%202%2016.png)

- This is just one example, but different RATs and other types of malware also generate similar types of non-web traffic.

## **Filtering for FTP Traffic**