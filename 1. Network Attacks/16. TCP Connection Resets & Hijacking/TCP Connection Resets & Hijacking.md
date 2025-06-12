## **TCP Connection Termination**

- An adversary wanted to cause denial-of-service conditions within our network. They might employ a simple TCP RST Packet injection attack, or TCP connection termination in simple terms.

This attack is a combination of a few conditions:

1. `The attacker will spoof the source address to be the affected machine's` (ARP poisoning, ARP spoofing).
2. `The attacker will modify the TCP packet to contain the RST flag to terminate the connection`
3. `The attacker will specify the destination port to be the same as one currently in use by one of our machines.`

![[image 14.png|image 14.png](../../../../../Image/image%2014.png)

## **TCP Connection Hijacking**

**TCP Connection Hijacking** là một kỹ thuật tấn công trong đó kẻ tấn công chiếm quyền điều khiển một kết nối TCP đang hoạt động giữa hai máy tính. Kẻ tấn công sẽ chèn các gói tin độc hại vào kết nối này, giả mạo một trong hai bên để kiểm soát luồng dữ liệu.

### **Cách thức tấn công**

1. **Theo dõi kết nối mục tiêu**: Kẻ tấn công sẽ chủ động theo dõi kết nối TCP mà chúng muốn chiếm quyền. Chúng phân tích lưu lượng mạng để xác định các thông tin quan trọng như **số thứ tự (sequence number)** và **số báo nhận (acknowledgment number)** của các gói tin.
2. **Dự đoán số thứ tự (Sequence Number Prediction)**:
    - Để chèn các gói tin độc hại vào kết nối, kẻ tấn công cần dự đoán chính xác số thứ tự của các gói tin tiếp theo.
    - Nếu dự đoán thành công, kẻ tấn công có thể gửi các gói tin giả mạo với số thứ tự phù hợp, khiến máy tính đích chấp nhận chúng như một phần của kết nối hợp lệ.
3. **Giả mạo địa chỉ IP nguồn**: Kẻ tấn công sẽ giả mạo địa chỉ IP nguồn của gói tin để trông như thể nó đến từ một trong hai máy tính đang giao tiếp.
4. **Chặn gói tin ACK**: Để duy trì quyền kiểm soát kết nối, kẻ tấn công cần ngăn chặn các gói tin **ACK (Acknowledgment)** từ máy tính đích đến máy tính nguồn. Chúng có thể làm điều này bằng cách:
    - **Trì hoãn** các gói tin ACK.
    - **Chặn hoàn toàn** các gói tin ACK bằng cách sử dụng các kỹ thuật như **ARP Poisoning** (Đầu độc ARP).

![[image 1 12.png|image 1 12.png](../../../../../Image/image%201%2012.png)