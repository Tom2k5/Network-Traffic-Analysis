## **Snort ở chế độ IDS/IPS**

- **Khả năng của Snort**:
    - **IDS**: Giám sát lưu lượng và cảnh báo khi phát hiện hành vi đáng ngờ.
    - **IPS**: Ngoài cảnh báo, nó còn chặn (drop) các gói tin nguy hiểm.
- **Quản lý lưu lượng**: Snort dựa trên **quy tắc (rules)** để quyết định cách xử lý lưu lượng mạng.
- **Phụ thuộc vào cấu hình**:
    - **File cấu hình** `/etc/snort/snort.conf` chứa các thiết lập tổng quát.
    - **Rule** được lưu trong `/etc/snort/rules/local.rules` để chỉ định hành động cụ thể.

---

## **Chạy Snort ở chế độ IDS/IPS**

### **Các tham số Network IDS**

|Tham số|Mô tả|
|---|---|
|**-c**|Chỉ định file cấu hình (VD: /etc/snort/snort.conf). Đây là nơi Snort đọc thiết lập.|
|**-T**|Kiểm tra file cấu hình xem có lỗi cú pháp hay không.|
|**-N**|Tắt chế độ ghi log, chỉ giữ các chức năng khác (như cảnh báo).|
|**-D**|Chạy Snort ở chế độ nền (background), không hiển thị đầu ra trực tiếp trên terminal.|
|**-A**|Chế độ cảnh báo, quyết định cách Snort thông báo khi phát hiện vấn đề:|
||`- full`: Cảnh báo đầy đủ, chứa mọi thông tin (mặc định nếu không chỉ định chế độ).|
||`- fast`: Cảnh báo nhanh, chỉ hiển thị tin nhắn, thời gian, IP nguồn/đích, cổng.|
||`- console`: Hiển thị cảnh báo nhanh trên terminal.|
||`- cmg`: Hiển thị chi tiết header và payload (dạng hex + text).|
||`- none`: Tắt cảnh báo hoàn toàn.|

### Preparation

- **Sample Rule:**
    
    ```Bash
    alert icmp any any <> any any (msg: "ICMP Packet Found"; sid: 100001; rev:1;)
    ```
    
    - **Ý nghĩa:** Phát cảnh báo khi có gói tin ICMP (ping) đi bất kỳ hướng nào.
- **Ghi chú**:
    - Khi có cảnh báo, Snort tạo file alert trong thư mục log `/var/log/snort`.

---

## **Chế Độ IDS**

### **Tham số -c và -T**

- **Mục đích**: Kiểm tra file cấu hình trước khi chạy thật.
- **Lệnh**: `sudo snort -c /etc/snort/snort.conf -T`
- **Giải thích**:
    - c: Chỉ định file cấu hình cần dùng.
    - T: Kiểm tra cú pháp của file snort.conf. Nếu có lỗi (misconfiguration), Snort sẽ thông báo.
- **Ứng dụng**: Dùng để đảm bảo cấu hình ổn định trước khi triển khai.

### **Tham số -N**

- **Mục đích**: Tắt ghi log để giảm tải hoặc tập trung vào cảnh báo.
- **Lệnh**: `sudo snort -c /etc/snort/snort.conf -N`
- **Kết quả**:
    - Log không được ghi vào thư mục log (VD: `/var/log/snort`).

### **Tham số -D**

- **Mục đích**: Chạy Snort ở chế độ nền (background).
- **Lệnh**: `sudo snort -c /etc/snort/snort.conf -D`
- **Kết quả mẫu**:

```Bash
Spawning daemon child...
My daemon child 2898 lives...
Daemon parent exiting (0)
```

- **Giải thích**:
    - Snort chạy như một tiến trình nền (daemon), không hiển thị đầu ra trực tiếp.
    - Kiểm tra tiến trình: `ps -ef | grep snort` → Thấy **PID** (VD: **2898**).
    - Dừng tiến trình: `sudo kill -9 2898`.

### **Tham số -A (Chế độ cảnh báo)**

- **Mục đích**: Quyết định cách Snort thông báo khi phát hiện vấn đề.
- **Các tùy chọn**:
    - `console`: Hiển thị nhanh trên terminal.
    - `cmg`: Hiển thị trên terminal với header và payload.
    - `full`: Ghi chi tiết vào file log (mặc định).
    - `fast`: Ghi thông tin cơ bản vào file log.
    - `none`: Tắt cảnh báo, chỉ ghi log nhị phân.
- `**-A console**`
    - **Lệnh**: `sudo snort -c /etc/snort/snort.conf -A console`
    - **Kết quả mẫu**:
        
        ```Bash
        12/12-02:08:27.577495  [**] [1:366:7] ICMP PING *NIX [**] [Classification: Misc activity] [Priority: 3] {ICMP} 192.168.175.129 -> 142.250.187.110
        12/12-02:08:27.577495  [**] [1:10000001:0] ICMP Packet found [**] [Priority: 0] {ICMP} 192.168.175.129 -> 142.250.187.110
        12/12-02:08:27.577495  [**] [1:384:5] ICMP PING [**] [Classification: Misc activity] [Priority: 3] {ICMP} 192.168.175.129 -> 142.250.187.110
        12/12-02:08:27.609719  [**] [1:10000001:0] ICMP Packet found [**] [Priority: 0] {ICMP} 142.250.187.110 -> 192.168.175.129
        ^C*** Caught Int-Signal
        12/12-02:08:29.595898  [**] [1:366:7] ICMP PING *NIX [**] [Classification: Misc activity] [Priority: 3] {ICMP} 192.168.175.129 -> 142.250.187.110
        12/12-02:08:29.595898  [**] [1:10000001:0] ICMP Packet found [**] [Priority: 0] {ICMP} 192.168.175.129 -> 142.250.187.110
        12/12-02:08:29.595898  [**] [1:384:5] ICMP PING [**] [Classification: Misc activity] [Priority: 3] {ICMP} 192.168.175.129 -> 142.250.187.110
        ```
        
    - **Giải thích**:
        - Hiển thị cảnh báo nhanh trên terminal: thời gian, thông điệp, IP nguồn/đích.
        - Dừng bằng Ctrl+C.
- `**-A cmg**`
    
    - **Lệnh**: `sudo snort -c /etc/snort/snort.conf -A cmg`
    
    ```Bash
    12/12-02:23:56.944351  [**] [1:366:7] ICMP PING *NIX [**] [Classification: Misc activity] [Priority: 3] {ICMP} 192.168.175.129 -> 142.250.187.110
    12/12-02:23:56.944351 00:0C:29:A5:B7:A2 -> 00:50:56:E1:9B:9D type:0x800 len:0x62
    192.168.175.129 -> 142.250.187.110 ICMP TTL:64 TOS:0x0 ID:10393 IpLen:20 DgmLen:84 DF
    Type:8  Code:0  ID:4   Seq:1  ECHO
    BC CD B5 61 00 00 00 00 CE 68 0E 00 00 00 00 00  ...a.....h......
    10 11 12 13 14 15 16 17 18 19 1A 1B 1C 1D 1E 1F  ................
    20 21 22 23 24 25 26 27 28 29 2A 2B 2C 2D 2E 2F   !"#$%&'()*+,-./
    30 31 32 33 34 35 36 37                          01234567
    ```
    
    - **Giải thích**:
        - Ngoài thông tin cơ bản, còn hiển thị **header** (TTL, ID, v.v.) và **payload** (nội dung gói tin ở dạng hex và text).
- `**-A fast**`
    
    - **Lệnh:** `sudo snort -c /etc/snort/snort.conf -A fast`
    
    ![[image 24.png|image 24.png](../../../Image/image%2024.png)
    
    - **Kết quả**: Không hiển thị trên terminal, ghi vào file alert với thông tin cơ bản (thời gian, IP, cổng).
- `**-A full**`
    
    - **Lệnh**: `sudo snort -c /etc/snort/snort.conf -A full`
    
    ![[image 1 19.png|image 1 19.png](../../../Image/image%201%2019.png)
    
    - **Kết quả**: Ghi mọi chi tiết vào file alert (header, payload, v.v.), không hiển thị trên terminal.
- `**-A none**`
    
    - **Lệnh**:`sudo snort -c /etc/snort/snort.conf -A none`
    
    ![[image 2 14.png|image 2 14.png](../../../Image/image%202%2014.png)
    
    - **Kết quả**: Không tạo file alert, chỉ ghi log nhị phân.

## **Chế độ IPS**

- **Mục đích**: Chặn gói tin thay vì chỉ cảnh báo.
- **Lệnh**: `sudo snort -c /etc/snort/snort.conf -q -Q --daq afpacket -i eth0:eth1 -A console`
- **Kết quả mẫu**

```Bash
Running in IPS mode

12/18-07:40:01.527100  [Drop] [**] [1:1000001:0] ICMP Packet found [**] [Priority: 0] {ICMP} 192.168.175.131 -> 192.168.175.2
12/18-07:40:01.552811  [Drop] [**] [1:1000001:0] ICMP Packet found [**] [Priority: 0] {ICMP} 172.217.169.142 -> 192.168.1.18
12/18-07:40:01.566232  [Drop] [**] [1:1000001:0] ICMP Packet found [**] [Priority: 0] {ICMP} 192.168.175.131 -> 192.168.175.2
12/18-07:40:02.517903  [Drop] [**] [1:1000001:0] ICMP Packet found [**] [Priority: 0] {ICMP} 192.168.1.18 -> 172.217.169.142
12/18-07:40:02.550844  [Drop] [**] [1:1000001:0] ICMP Packet found [**] [Priority: 0] {ICMP} 172.217.169.142 -> 192.168.1.18
^C*** Caught Int-Signal
```

- **Giải thích**:
    - `Q --daq afpacket`: Kích hoạt chế độ IPS với module DAQ (Data Acquisition).
    - `i eth0:eth1`: Chỉ định 2 giao diện mạng (yêu cầu của IPS).
    - `[Drop]` : Gói tin bị chặn, không chỉ là cảnh báo.

---