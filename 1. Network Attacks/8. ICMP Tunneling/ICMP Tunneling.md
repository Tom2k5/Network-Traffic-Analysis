- **ICMP Tunneling** là một kỹ thuật lợi dụng giao thức ICMP (thường được sử dụng để kiểm tra kết nối mạng, ví dụ như lệnh `ping`) để truyền dữ liệu ẩn giữa các máy tính.
- Kẻ tấn công sẽ chèn dữ liệu vào trường **Data** của gói tin ICMP, biến nó thành một kênh truyền tin bí mật.

---

### **Cách phát hiện ICMP Tunneling**

1. **Lọc gói tin ICMP trong Wireshark**:
    - Trong Wireshark, chúng ta có thể lọc các gói tin ICMP bằng cách nhập `ICMP` vào thanh lọc. Điều này giúp tập trung vào các gói tin ICMP Request (yêu cầu) và Reply (phản hồi).
2. **Phát hiện phân mảnh (Fragmentation)**:
    - Nếu chúng ta nhận thấy các gói tin ICMP bị phân mảnh (fragmentation), đây có thể là dấu hiệu của việc truyền một lượng lớn dữ liệu qua ICMP.
    - Ví dụ: Một gói tin ICMP bình thường có kích thước dữ liệu khoảng **48 byte**, nhưng nếu kích thước dữ liệu lên đến **38.000 byte**, điều này rất đáng ngờ.
3. **Kiểm tra nội dung trường Data**:
    - Trong Wireshark, chúng ta có thể xem nội dung của trường **Data** trong gói tin ICMP.
    - Nếu phát hiện các thông tin nhạy cảm như **tên người dùng (Username)** và **mật khẩu (Password)** được truyền qua ICMP, đây là dấu hiệu rõ ràng của ICMP Tunneling.
4. **Phát hiện mã hóa hoặc mã hóa dữ liệu**:
    - Các kẻ tấn công nâng cao có thể sử dụng **mã hóa (encryption)** hoặc **mã hóa (encoding)** để che giấu dữ liệu truyền qua ICMP.
    - Trong trường hợp này, dữ liệu trong trường **Data** có thể xuất hiện dưới dạng các ký tự hoặc chuỗi không rõ ràng, khó hiểu.

---

## **Finding ICMP Tunneling**

- The data is something reasonable like 48 bytes.

![](../../Image/image%2015.png)

- A suspicious ICMP request might have a large data length like 38000 bytes.

![](../../Image/image%201%2013.png)

- This is a direct indication of ICMP tunneling. More advanced adversaries will utilize encoding or encryption when transmitting exfiltrated data, even in the case of ICMP tunneling.

![](../../Image/image%202%2010.png)