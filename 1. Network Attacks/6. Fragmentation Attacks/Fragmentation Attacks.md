## **IP Fragmentation**

- **Mục Đích Chính Đáng:** Phân mảnh là một quá trình, trong đó các gói tin lớn được chia thành các mảnh nhỏ hơn để có thể truyền qua các mạng có kích thước đơn vị truyền tối đa (MTU) nhỏ hơn. Sau đó, máy chủ đích sẽ tập hợp lại các mảnh này.
## Fragmentation Attack
### Popular Technique
- **IPS/IDS Evasion:** Nếu một IPS/IDS không tập hợp lại các gói tin bị phân mảnh, kẻ tấn công có thể chia nhỏ các cuộc tấn công của họ (ví dụ: `nmap`) thành nhiều mảnh. IPS/IDS sẽ không phát hiện ra toàn bộ chữ ký tấn công, nhưng máy chủ đích vẫn sẽ tập hợp lại các mảnh này.
- **Firewall Evasion:** Tương tự như vượt qua IPS/IDS, kẻ tấn công có thể vượt qua các quy tắc tường lửa nếu không tập hợp lại các gói tin đã bị phân mảnh.
- **Firewall/IPS/IDS Resource Exhaustion:** Kẻ tấn công có thể gửi các gói tin được phân mảnh thành các MTU rất nhỏ (10, 15, 20,...). Các thiết bị an ninh có thể gặp khó khăn trong việc tập hợp lại các gói tin này do giới hạn tài nguyên, cho phép cuộc tấn công tiếp diễn. Việc này buộc hệ thống phải xử lý một số lượng lớn các gói tin, làm cạn kiệt CPU và bộ nhớ.
    - **Gửi các mảnh gói tin chồng chéo hoặc không đầy đủ:** Các mảnh gói tin được tạo ra sao cho khi tập hợp lại sẽ gây ra lỗi hoặc chồng chéo, buộc hệ thống phải tiêu tốn tài nguyên để xử lý và loại bỏ chúng.
    - **Gửi các mảnh gói tin không thể tái tạo:** Các mảnh gói tin được tạo ra sao cho không thể tập hợp lại thành một gói tin hoàn chỉnh, buộc hệ thống phải giữ chúng trong bộ nhớ tạm thời trong một khoảng thời gian dài, cuối cùng dẫn đến cạn kiệt bộ nhớ.
- **DoS:** Kẻ tấn công có thể lợi dụng phân mảnh để gửi các gói tin IP vượt quá 65535 byte (kích thước tối đa của gói tin IP). Các hệ thống cũ có thể bị treo hoặc gặp sự cố khi cố gắng tập hợp lại các gói tin này, dẫn đến từ chối dịch vụ. Một ví dụ cụ thể là cuộc tấn công Teardrop.
### **Defend**
- Để phòng thủ chống lại các cuộc tấn công này, **IDS/IPS/Tường lửa** nên thực hiện "Tập hợp lại trì hoãn" (Delayed Reassembly). Điều này có nghĩa là chúng nên đợi tất cả các mảnh gói tin đến, tập hợp lại toàn bộ dữ liệu, và sau đó kiểm tra gói tin trước khi chuyển tiếp lưu lượng.
## **Finding Irregularities in Fragment Offsets**
- For starters, we might notice several ICMP requests going to one host from another, this is indicative of the starting requests from a traditional Nmap scan.
- Emulation: `TAH19@htb[/htb]``**$**` `nmap <host ip>`

![[image 10.png|image 10.png](../../../../../Image/image%2010.png)

- Fragment Attack Emulation : `TAH19@htb[/htb]``**$**` `nmap -f 10 <host ip>`

⇒ Generate IP packet Fragments with a maximum size of 10.

![[image 1 8.png|image 1 8.png](../../../../../Image/image%201%208.png)

- Reply with RST (closed port) ⇒ Scan to verify Port Active.

![[image 2 6.png|image 2 6.png](../../../../../Image/image%202%206.png)