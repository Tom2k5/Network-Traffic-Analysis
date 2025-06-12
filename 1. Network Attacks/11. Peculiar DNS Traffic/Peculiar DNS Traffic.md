## **Finding DNS Enumeration Attempts**

- Notice a significant amount of DNS traffic from one host when we start to look at our raw output in Wireshark.

![](../../Image/image%2020.png)

- Notice this traffic concluded with something like `ANY`:

![](../../Image/image%201%2016.png)

## **Finding DNS Tunneling**

- **DNS Tunneling** là một kỹ thuật lợi dụng giao thức DNS để truyền dữ liệu ẩn giữa các máy tính.
- Kẻ tấn công sẽ chèn dữ liệu cần truyền vào các trường của gói tin DNS, chẳng hạn như trường **TXT** (Text Record), và sử dụng các truy vấn DNS để gửi dữ liệu này ra khỏi mạng.

![](../../Image/image%202%2011.png)

---

1. **Phân tích lưu lượng DNS**:
- Nếu chúng ta nhận thấy một lượng lớn các bản ghi **TXT** từ một máy chủ cụ thể, đây có thể là dấu hiệu của DNS Tunneling.
- Ví dụ: Các truy vấn DNS chứa dữ liệu không rõ ràng hoặc có cấu trúc bất thường.

![](../../Image/image%203%207.png)

1. **Kiểm tra nội dung trường TXT**:
    - Trong Wireshark, chúng ta có thể xem nội dung của trường **TXT** trong các gói tin DNS.
    - Nếu phát hiện các chuỗi dữ liệu được mã hóa hoặc không rõ ràng, đây có thể là dấu hiệu của DNS Tunneling.
2. **Giải mã dữ liệu**:
    - Kẻ tấn công thường mã hóa dữ liệu trước khi chèn vào các truy vấn DNS. Ví dụ, họ có thể sử dụng **Base64** để mã hóa dữ liệu.
3. **Phát hiện mã hóa hoặc mã hóa dữ liệu**:
    - Nếu dữ liệu trong trường **TXT** xuất hiện dưới dạng các ký tự hoặc chuỗi không rõ ràng, đây có thể là dấu hiệu của việc mã hóa hoặc mã hóa dữ liệu.