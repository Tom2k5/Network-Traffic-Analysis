- **Weird headers:**
    1. `Weird Hosts (Host: )`
    2. `Unusual HTTP Verbs`
    3. `Changed User Agents`
## **Finding Strange Host Headers**
1. **Lọc lưu lượng HTTP**:
- Trong Wireshark, chúng ta có thể lọc các yêu cầu và phản hồi HTTP bằng cách nhập `http` vào thanh lọc.
1. **Tìm kiếm Host Header bất thường**:
- Sử dụng bộ lọc sau để tìm các yêu cầu HTTP có Host Header không khớp với địa chỉ IP hoặc tên miền hợp lệ của máy chủ web:

```Plain
    http.request and (!(http.host == "192.168.10.7"))
```

- Trong đó, `192.168.10.7` là địa chỉ IP thực của máy chủ web. Bộ lọc này sẽ loại trừ các yêu cầu có Host Header hợp lệ và chỉ hiển thị các yêu cầu có Host Header bất thường.
3. **Phân tích kết quả**:
- Nếu bộ lọc trả về kết quả, chúng ta có thể kiểm tra chi tiết các yêu cầu HTTP để xác định các Host Header đáng ngờ.
- Ví dụ về các Host Header bất thường:
	- `127.0.0.1`: Địa chỉ loopback, thường được sử dụng để tham chiếu đến chính máy chủ.
	- `admin`: Có thể là một tên miền hoặc tên máy chủ giả mạo.

## **Analyzing Code 400s and Request Smuggling**

- Notice some bad responses from our web server, like code 400s. These codes indicate a bad request from the client, so they can be a good place to start when detecting malicious actions via http/https. In order to filter for these, we can use the following
    - `http.response.code == 400`
    
![](../../Image/image%2017.png)

### **Tấn công HTTP Request Smuggling**

- **HTTP Request Smuggling** là một kỹ thuật tấn công trong đó kẻ tấn công gửi một yêu cầu HTTP được thiết kế đặc biệt để máy chủ hiểu sai và xử lý thành nhiều yêu cầu khác nhau.
- Ví dụ về một yêu cầu HTTP độc hại:

    ![](../../Image/image%201%2014.png)
    
```
GET%20%2flogin.php%3fid%3d1%20HTTP%2f1.1%0d%0aHost%3a%20192.168.10.5%0d%0a%0d%0aGET%20%2fuploads%2fcmd2.php%20HTTP%2f1.1%0d%0aHost%3a%20127.0.0.1%3a8080%0d%0a%0d%0a%20HTTP%2f1.1 Host: 192.168.10.5
```
    
- Khi được giải mã, yêu cầu này sẽ trở thành:
```Plain
GET /login.php?id=1 HTTP/1.1
Host: 192.168.10.5
        
GET /uploads/cmd2.php HTTP/1.1
Host: 127.0.0.1:8080
        
HTTP/1.1
Host: 192.168.10.5
```
        
- Nếu máy chủ bị lỗ hổng, nó có thể xử lý yêu cầu này thành hai yêu cầu riêng biệt, cho phép kẻ tấn công truy cập trái phép vào các tài nguyên hoặc dịch vụ.

---
### **Lỗ hổng cấu hình Apache**
Đoạn văn cung cấp một ví dụ về cấu hình Apache có thể dễ bị tấn công HTTP Request Smuggling:
```
<VirtualHost *:80>
    RewriteEngine on
    RewriteRule "^/categories/(.*)" "http://192.168.10.100:8080/categories.php /id=$1" [P]
    ProxyPassReverse "/categories/" "http://192.168.10.100:8080/"
</VirtualHost>
```
- Cấu hình này sử dụng `RewriteRule` và `ProxyPassReverse` để chuyển hướng các yêu cầu đến một máy chủ khác.
- Nếu không được cấu hình đúng cách, kẻ tấn công có thể lợi dụng để thực hiện các cuộc tấn công như HTTP Request Smuggling.