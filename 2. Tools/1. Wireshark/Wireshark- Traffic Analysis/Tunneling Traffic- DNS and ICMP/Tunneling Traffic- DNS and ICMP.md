- **Tunnelling Traffic** là một khái niệm liên quan đến việc tạo ra một "đường hầm" ảo để truyền tải dữ liệu qua mạng. Kỹ thuật này thường được sử dụng trong các mạng riêng ảo (VPN) để bảo vệ và mã hóa lưu lượng truy cập Internet.
- **Cách hoạt động của Tunnelling**

1. **Đóng gói dữ liệu**: Dữ liệu được đóng gói vào một gói khác trước khi được gửi qua mạng.
2. **Mã hóa dữ liệu**: Sau khi đóng gói, dữ liệu sẽ được mã hóa để ngăn chặn việc truy cập trái phép.

Khi người dùng kết nối qua VPN, giao thức tunneling sẽ "gói" toàn bộ thông tin vào một package khác, mã hóa chúng và gửi qua "tunnel". Tại điểm cuối, các giao thức tunneling sẽ giải mã các package này và kiểm tra tính hợp lệ của thông tin.

- Tunneling traffic có thể được sử dụng để tấn công vì một số lý do chính sau đây:

**1. Ẩn danh và Bảo mật**

Tunneling cho phép kẻ tấn công ẩn danh khi thực hiện các hoạt động độc hại. Bằng cách mã hóa và đóng gói dữ liệu trong các giao thức khác, kẻ tấn công có thể che giấu nguồn gốc và nội dung của lưu lượng truy cập. Điều này làm cho việc phát hiện và theo dõi các hoạt động bất hợp pháp trở nên khó khăn hơn cho các biện pháp an ninh mạng.

**2. Khai thác Lỗ hổng Mạng**

Kẻ tấn công có thể sử dụng tunneling để lợi dụng các lỗ hổng trong mạng. Ví dụ, DNS tunneling cho phép kẻ tấn công gửi dữ liệu qua giao thức DNS, thường không bị kiểm tra nghiêm ngặt. Điều này có thể được sử dụng để thu thập thông tin hoặc điều khiển các thiết bị trong mạng mà không bị phát hiện.

**3. Tấn công Man-in-the-Middle (MitM)**

Tunneling cũng có thể tạo điều kiện cho các cuộc tấn công Man-in-the-Middle, nơi kẻ tấn công chèn vào giữa giao tiếp giữa hai bên. Khi dữ liệu được truyền qua một đường hầm, kẻ tấn công có thể dễ dàng theo dõi hoặc thay đổi thông tin mà không bị phát hiện.

**4. Truyền tải Malware**

Kẻ tấn công có thể sử dụng tunneling để truyền tải malware hoặc phần mềm độc hại qua mạng mà không bị phát hiện. Bằng cách đóng gói malware bên trong các gói dữ liệu hợp lệ, chúng có thể vượt qua các biện pháp bảo mật và xâm nhập vào hệ thống mục tiêu

**5. Bypass Firewall và Kiểm soát Truy cập**

Tunneling cũng cho phép kẻ tấn công vượt qua các rào cản bảo mật như firewall hoặc hệ thống kiểm soát truy cập. Bằng cách sử dụng các giao thức tunneling như SSH hoặc ICMP, họ có thể truyền tải dữ liệu mà không bị chặn bởi các chính sách bảo mật thông thường.

## ICMP Analysis

- **Internet Control Message Protocol (ICMP)** is designed for diagnosing and reporting network communication issues. It is highly used in error reporting and testing. As it is a trusted network layer protocol, sometimes it is used for denial of service (DoS) attacks; also, adversaries use it in data exfiltration and C2 tunnelling activities.

![[image 31.png|image 31.png](../../../../../../../Image/image%2031.png)

![[image 1 26.png|image 1 26.png](../../../../../../../Image/image%201%2026.png)

## DNS Analysis

- **Domain Name System (DNS)** is designed to translate/convert IP domain addresses to IP addresses. It is also known as a phonebook of the internet. As it is the essential part of web services, it is commonly used and trusted, and therefore often ignored. Due to that, adversaries use it in data exfiltration and C2 activities.

![[image 2 20.png|image 2 20.png](../../../../../../../Image/image%202%2020.png)

![[image 3 14.png|image 3 14.png](../../../../../../../Image/image%203%2014.png)

![[image 4 12.png|image 4 12.png](../../../../../../../Image/image%204%2012.png)

![[image 5 10.png|image 5 10.png](../../../../../../../Image/image%205%2010.png)