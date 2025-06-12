### **Giao thức ARP**

- ARP là giao thức cho phép các thiết bị nhận diện nhau trên network.
- ARP hoạt động trên mạng cục bộ, cho phép giao tiếp giữa các địa chỉ MAC.
- Các gói tin ARP phổ biến bao gồm yêu cầu & phản hồi, thông báo và gói tin không cần thiết.

### **Tấn công ARP Poisoning**

- **Kẻ tấn công (hacker) giả mạo router:** Gửi thông báo ARP giả mạo, tự nhận là router với địa chỉ MAC của hacker.

⇒ Sửa lại bảng ARP.

- **Mục tiêu:** Đầu độc ARP cache của nạn nhân, khiến nạn nhân liên kết địa chỉ MAC của hacker với địa chỉ IP của router.
- **Hậu quả:**
    - Dữ liệu từ nạn nhân sẽ được gửi đến hacker thay vì router.
    - Hacker có thể theo dõi, đánh cắp thông tin, thậm chí thay đổi dữ liệu trước khi gửi đến router.

⇒ Thực hiện **MITM (Man-In-The-Middle) Attack**.

### Tấn công ARP Flooding (MAC Flooding)

- **Gửi hàng loạt gói ARP giả mạo:** Kẻ tấn công gửi một lượng lớn các gói ARP với một hoặc nhiều địa chỉ MAC giả mạo đến switch.
- **Hậu quả:**
    - **Bảng CAM bị quá tải:** Switch cố gắng cập nhật bảng CAM với các thông tin MAC mới này, dẫn đến bảng CAM bị đầy.
    - **Switch hoạt động như một hub:** Khi bảng CAM bị đầy, switch không còn đủ khả năng để xác định cổng đích cho các gói tin và sẽ chuyển tất cả các gói tin đến tất cả các cổng.

### ARP Analysis in a nutshell

| Note                                           | Wireshark filter                                                         |
| ---------------------------------------------- | ------------------------------------------------------------------------ |
| Global search                                  | `arp`                                                                    |
| Opcode 1: ARP requests.                        | `arp.opcode == 1`                                                        |
| Opcode 2: ARP responses.                       | `arp.opcode == 2`                                                        |
| **Hunt:** Arp scanning                         | `arp.dst.hw_mac==00:00:00:00:00:00`                                      |
| **Hunt:** Possible ARP poisoning detection     | `arp.duplicate-address-detected or arp.duplicate-address-frame`          |
| **Hunt:** Possible ARP flooding from detection | `((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address)` |

- **ARP request && ARP reply:**

![](../../../../Image/image%2029.png)

- **ARP Spoofing:**

⇒ **SUS situation:** Hai **ARP response** va chạm với nhau vì một địa chỉ IP.

⇒ MAC address với đuôi `b4` cho rằng mình là `192.168.1.1` với ARP request từ `192.168.1.25` .

![](../../../../Image/image%201%2024.png)

|Notes|Detection Notes|Findings|
|---|---|---|
|Possible IP address match|1 IP address được thông báo từ 1 MAC address|• MAC: 00:0c:29:e2:18:b4  <br>• IP: 192.168.1.25|
|Possible ARP spoofing attempt|2 MAC address cho rằng mình là `192.168.1.1` Có thể là IP default gateway|• MAC 1: 50:78:b3:f3:cd:f4  <br>• MAC 2: 00:0c:29:e2:18:b4|
|Possible ARP flooding attempt|MAC address đuôi `b4` muốn có một địa chỉ IP mới.|• MAC: 00:0c:29:e2:18:b4  <br>• IP: 192.168.1.1|

- **ARP Flooding:**

⇒ **SUS situation:** MAC address đuôi `b4` tạo ra một đống ARP request với `192.168.1.25` .

![](../../../../Image/image%202%2018.png)

|Notes|Detection Notes|Findings|
|---|---|---|
|Possible IP address match|1 IP address được thông báo từ 1 MAC address|• MAC: 00:0c:29:e2:18:b4 • IP: 192.168.1.25|
|Possible ARP spoofing attempt|2 MAC address cho rằng mình là `192.168.1.1` Có thể là IP default gateway|• MAC 1: 50:78:b3:f3:cd:f4 • MAC 2: 00:0c:29:e2:18:b4|
|Possible ARP flooding attempt|MAC address đuôi `b4` muốn có một địa chỉ IP mới.|• MAC: 00:0c:29:e2:18:b4 • IP: 192.168.1.1|
|Possible ARP flooding attempt|MAC address đuôi `b4` tạo ra một đống ARP request với `192.168.1.25`|• MAC: 00:0c:29:e2:18:b4  <br>• IP: 192.168.1.xxx|

⇒ Đây là bằng chứng cho rằng MAC address đã sỡ hữu IP address `192.168.1.25` và tạo ARP request với địa chỉ đó.

⇒ Người tấn công cũng sỡ hữu cả `192.168.1.1` , có thể là default gateway address.

- **HTTP Traffic:**

![](../../../../Image/image%203%2012.png)

- **Phân tích HTTP Traffic:**

![](../../../../Image/image%204%2010.png)

⇒ The MAC address that ends with `b4` is the destination of all HTTP packets.

⇒ It is evident that there is a **MITM attack.**

- **Summary:**

|Notes|Detection Notes|Findings|
|---|---|---|
|IP to MAC matches|3 IP to MAC address matches|• MAC: 00:0c:29:e2:18:b4 = IP: 192.168.1.25  <br>• MAC: 50:78:b3:f3:cd:f4 = IP: 192.1681.1  <br>• MAC: 00:0c:29:98:c7:a8 = IP: 192.168.1.12|
|Attacker|The attacker created noise with ARP packets|• MAC: 00:0c:29:e2:18:b4 = IP: 192.168.1.25|
|Router/gateway|Gateway address|• MAC: 50:78:b3:f3:cd:f4 = IP: 192.168.1.1|
|Victim|The attacker sniffed all traffic of the victim|• MAC: 50:78:b3:f3:cd:f4 = IP: 192.1681.12|

---

- Answer the questions below:

1. What is the number of ARP requests crafted by the attacker?
    
    ⇒ `arp.opcode==1 && arp.src.hw_mac==00:0c:29:e2:18:b4` ⇒ 284.
    
    ⇒ opcode = 1 (request).
    
    ⇒ Request từ attacker.
    
2. What is the number of HTTP packets received by the attacker?
    
    ⇒ `http && eth.dst==00:0c:29:e2:18:b4` ⇒ 90.
    
    ⇒ Vì tấn công MITM luôn có destination là attacker (`00:0c:29:e2:18:b4`).
    
3. What is the number of sniffed username&password entries?
    
    ⇒ `(urlencoded-form.key == "uname") && (urlencoded-form.key == "pass")`
    
4. What is the password of the "Client986"?
    
    ⇒ `(urlencoded-form.key == "uname") && (urlencoded-form.value == "client986")`
    
    ⇒ `
    
5. What is the comment provided by the "Client354"?
    
    ![](../../../../Image/image%205%208.png)
    
    ⇒ `http.file_data contains "client354"`