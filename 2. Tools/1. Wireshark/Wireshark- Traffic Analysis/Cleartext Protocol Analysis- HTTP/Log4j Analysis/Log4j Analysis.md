### “Log4j” Vulnerability

- **Lỗ hổng Log4j**: Lỗ hổng **CVE-2021-44228**, hay còn gọi là **Log4Shell**, là một lỗ hổng nghiêm trọng trong thư viện **Apache,** Log4j cho phép kẻ tấn công thực thi mã từ xa mà không cần xác thực.

⇒ Trước khi sử dụng **Wireshark** để phân tích lưu lượng mạng, việc nắm vững thông tin về **lỗ hổng Log4j** sẽ giúp xác định các mẫu lưu lượng đáng ngờ và các dấu hiệu của việc khai thác lỗ hổng này.

![[image 40.png|image 40.png](../../../../../Image/image%2040.png)

- `**ip contains “jndi”**` ⇒ Quét toàn bộ nội dung của các gói tin IP để tìm kiếm chuỗi **"jndi"**, bao gồm cả phần header và payload.
- `**frame contains "jndi"**` ⇒ Tìm kiếm chuỗi **"jndi"** trong toàn bộ frame (bao gồm cả header và payload của tất cả các giao thức), không chỉ giới hạn ở gói tin IP.

---

## Answer the questions:

1. **Locate the "Log4j" attack starting phase. What is the packet number?**

![[image 1 34.png|image 1 34.png](../../../../../Image/image%201%2034.png)

1. **Locate the "Log4j" attack starting phase and decode the base64 command. What is the IP address contacted by the adversary? (Enter the address in defanged format and exclude "{}".)**

![[image 2 27.png|image 2 27.png](../../../../../Image/image%202%2027.png)

![[image 3 18.png|image 3 18.png](../../../../../Image/image%203%2018.png)

⇒ Để thực hiện tấn công, người tấn công tải **“lh.sh”** từ **“62.210.130.250”** rồi cấp quyền **executation**.