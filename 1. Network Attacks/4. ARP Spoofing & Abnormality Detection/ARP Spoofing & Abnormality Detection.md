## **How Address Resolution Protocol Works**

![](../../Image/image%206.png)

## **ARP Poisoning & Spoofing**

### ARP Poisoning

- **Định nghĩa**: ARP Poisoning là kỹ thuật tấn công trong đó kẻ tấn công gửi các thông điệp ARP giả mạo đến các thiết bị trong mạng, làm thay đổi bảng ARP cache của chúng.
- **Mục đích**:
    - Chuyển hướng lưu lượng mạng qua máy của kẻ tấn công (MITM).
    - Ngăn chặn liên lạc giữa các thiết bị (DoS).
    - Đánh cắp dữ liệu nhạy cảm (ví dụ: thông tin đăng nhập, cookie).
- **Cách hoạt động**:
    1. Kẻ tấn công gửi các thông điệp ARP giả mạo (spoofed ARP replies) đến các thiết bị trong mạng.
    2. Các thiết bị nhận được thông điệp ARP giả mạo sẽ cập nhật bảng ARP cache của chúng với thông tin sai lệch.
    3. Kết quả là lưu lượng mạng giữa các thiết bị bị chuyển hướng qua máy của kẻ tấn công.

### ARP Spoofing

- **Định nghĩa**: ARP Spoofing là một hình thức của ARP Poisoning, trong đó kẻ tấn công giả mạo địa chỉ MAC của mình để trở thành "người trung gian" giữa hai thiết bị.
- **Mục đích**:
    - Thực hiện tấn công MITM để nghe lén, thay đổi hoặc đánh cắp dữ liệu.
    - Gây rối loạn mạng bằng cách làm ngập lụt các thông điệp ARP.
- **Cách hoạt động**:
    1. Kẻ tấn công gửi thông điệp ARP giả mạo đến một hoặc nhiều thiết bị trong mạng.
    2. Các thiết bị nhận được thông điệp ARP giả mạo sẽ cập nhật bảng ARP cache của chúng với thông tin sai lệch.
    3. Lưu lượng mạng giữa các thiết bị bị chuyển hướng qua máy của kẻ tấn công.

### Solution

- **Switch and Router Port Security (DAI)**
- **Static ARP Entries (ARP Tĩnh)**

## **Finding ARP Spoofing**

![](../../Image/image%201%204.png)

- `08:00:27:53:0C:BA` is suspicious.
- The opcode functionality in ARP:
    - `Opcode == 1`: ARP Requests.
    - `Opcode == 2`: ARP Replies.
- Notice a red flag - an address duplication, accompanied by a warning message.

![](../../Image/image%202%204.png)

- To shift through more duplicate records:
    - `arp.duplicate-address-detected && arp.opcode == 2`

## **Identifying The Original IP Addresses**

![](../../Image/image%203%203.png)

![](../../Image/image%204%202.png)

- `08:00:27:53:0c:ba` was initially linked to the IP address `192.168.10.5`, but this was recently switched to `192.168.10.4`.

### Hành vi tấn công

![](../../Image/image%205%202.png)

- **Kẻ tấn công không chuyển tiếp lưu lượng**
    - **Dấu hiệu**: Các kết nối TCP liên tục bị ngắt (dropping).
    - **Nguyên nhân**:
        - Kẻ tấn công sử dụng ARP Spoofing để chuyển hướng lưu lượng qua máy của mình, nhưng **không chuyển tiếp lưu lượng** đến đích thực sự (ví dụ: router).
        - Kết quả là các gói tin từ máy nạn nhân (victim) không đến được router, và các kết nối TCP bị ngắt do không nhận được phản hồi.
    - **Ví dụ**:
        - Máy nạn nhân gửi gói tin đến router, nhưng gói tin bị chuyển hướng đến kẻ tấn công.
        - Kẻ tấn công không chuyển tiếp gói tin đến router, khiến máy nạn nhân không nhận được phản hồi.
        - Các kết nối TCP (ví dụ: truy cập web, kết nối ứng dụng) bị ngắt liên tục.
- **Kẻ tấn công chuyển tiếp lưu lượng và hoạt động như MITM**
    - **Dấu hiệu**: Lưu lượng mạng từ máy nạn nhân đến kẻ tấn công và từ kẻ tấn công đến router gần như **giống nhau hoặc đối xứng**.
    - **Nguyên nhân**:
        - Kẻ tấn công sử dụng ARP Spoofing để chuyển hướng lưu lượng qua máy của mình, đồng thời **chuyển tiếp lưu lượng** đến router.
        - Kẻ tấn công có thể nghe lén, thay đổi hoặc đánh cắp dữ liệu trong quá trình này.
    - **Ví dụ**:
        - Máy nạn nhân gửi gói tin đến router, nhưng gói tin bị chuyển hướng đến kẻ tấn công.
        - Kẻ tấn công nhận gói tin, sau đó chuyển tiếp nó đến router.
        - Router gửi phản hồi đến kẻ tấn công, và kẻ tấn công chuyển tiếp phản hồi này đến máy nạn nhân.
        - Kết quả là lưu lượng từ máy nạn nhân đến kẻ tấn công và từ kẻ tấn công đến router gần như giống nhau.