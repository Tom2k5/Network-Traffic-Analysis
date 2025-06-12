- U**ser-Agent** là một trong những nguồn tài nguyên hữu ích để phát hiện các bất thường trong lưu lượng HTTP.
- **User-Agent**: Là một chuỗi thông tin được gửi từ trình duyệt web đến máy chủ, cho biết thông tin về trình duyệt, hệ điều hành và thiết bị mà người dùng đang sử dụng.
- Không thể chỉ dựa vào trường **user-agent** để phát hiện bất thường.
- Không bao giờ nên đưa **user-agent** vào **whitelist**, ngay cả khi nó trông tự nhiên vì kẻ tấn công có thể thành công trong việc sửa đổi dữ liệu **user-agent**, khiến nó trông rất tự nhiên.

![](../../../../../Image/image%2039.png)

![](../../../../../Image/image%201%2033.png)

---

## Answer the questions:

1. Investigate the user agents. What is the number of anomalous "user-agent" types?

![](../../../../../Image/image%202%2026.png)

![](../../../../../Image/image%203%2017.png)

1. What is the packet number with a subtle spelling difference in the user agent field?

![](../../../../../Image/image%204%2015.png)