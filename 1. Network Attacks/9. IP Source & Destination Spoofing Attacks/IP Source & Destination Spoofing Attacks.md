## Attack Techniques

An attacker might conduct these packet crafting attacks towards the source and destination IP addresses for many different reasons or desired outcomes.

- **Decoy Scanning:** 
    - Bằng cách thay đổi địa chỉ IP nguồn của gói tin, kẻ tấn công có thể vượt qua các hạn chế của tường lửa để thu thập thêm thông tin về một máy chủ trong một phân đoạn mạng khác.
    - Thay đổi IP nguồn thành một địa chỉ trong cùng một mạng con với máy chủ mục tiêu có thể giúp kẻ tấn công vượt qua tường lửa.
- **Random Source Attack DDoS:** 
    - Kẻ tấn công có thể làm ngập một máy chủ nạn nhân bằng lưu lượng truy cập đến một cổng cụ thể bằng cách sử dụng tạo nguồn ngẫu nhiên, có khả năng làm quá tải các kiểm soát mạng hoặc tài nguyên của máy chủ đích.
- **LAND Attacks:**
    - Tương tự như các cuộc tấn công từ chối dịch vụ nguồn ngẫu nhiên, các cuộc tấn công LAND đặt địa chỉ nguồn giống với máy chủ đích. Điều này có thể làm cạn kiệt tài nguyên mạng hoặc gây ra sự cố trên máy chủ mục tiêu.
- **SMURF Attacks:** 
    - Kẻ tấn công gửi một lượng lớn các gói ICMP đến nhiều máy chủ khác nhau, nhưng địa chỉ nguồn được đặt thành máy nạn nhân. Các máy chủ nhận được phản hồi bằng các trả lời ICMP cho nạn nhân, gây ra sự cạn kiệt tài nguyên.
- **Initialization Vector Generation:**
    - Trong các mạng không dây cũ (ví dụ: bảo mật tương đương có dây), kẻ tấn công có thể chụp, giải mã, chế tạo và chèn lại các gói tin với các địa chỉ IP nguồn và đích đã sửa đổi.
    - Điều này được thực hiện để tạo ra các vector khởi tạo, được sử dụng để xây dựng bảng giải mã cho các cuộc tấn công thống kê. Sự hiện diện của các gói tin lặp đi lặp lại quá mức giữa các máy chủ có thể chỉ ra loại hoạt động này.

## **Finding Decoy Scanning Attempts**

- Simply put, when an attacker wants to gather information, they might change their source address to be the same as another legitimate host, or in some cases entirely different from any real host. This is to attempt to evade IDS/Firewall controls, and it can be easily observed.
- In the case of decoy scanning, we will notice some strange behavior.

1. `Initial Fragmentation from a fake address`
2. `Some TCP traffic from the legitimate source address`

![[image 11.png|image 11.png](../../Image/image%2011.png)

![[image 1 9.png|image 1 9.png](../../Image/image%201%209.png)

⇒ Ta có thể phát hiện Port Scanning.

## **Finding Random Source Attacks**

- Random Source Attack có thể thực hiện theo nhiều cách.
- **Scenario 1:**
    
    - Cuộc tấn công thực hiện ngược lại so với SMURF Attack, các client sẽ ping đến một thiết bị không tồn tại, thiết bị ấy sẽ reply toàn bộ.
    - Source IP bị spoof random lúc ban đầu để ping tới IP Destination được dựng lên.
    
    ![[image 2 7.png|image 2 7.png](../../Image/image%202%207.png)
    
    - Các Reply có thể được fragmentation để tấn công, làm resource exhaustion.
- Scenario 2:
    - Cuộc tấn công làm cạn kiệt tài nguyên của một cổng dịch vụ chỉ định.
    - Source IP bị spoof random.

![[image 3 5.png|image 3 5.png](../../Image/image%203%205.png)

## **Finding Smurf Attacks**

[What is Smurf Attack? | What is The Denial Of Service Attack?](https://techofide.com/blogs/what-is-smurf-attack-what-is-the-denial-of-service-attack-practical-ddos-attack-step-by-step-guide/)

- Simply put, an attacker conducts these like the following:
    1. `The attacker will send an ICMP request to live hosts with a spoofed address of the victim host`
    2. `The live hosts will respond to the legitimate victim host with an ICMP reply`
    3. `This may cause resource exhaustion on the victim host`

![[image 4 4.png|image 4 4.png](../../Image/image%204%204.png)

- We might notice many different hosts pinging our single host, and in this case it represents the basic nature of SMURF attacks.

![[image 5 3.png|image 5 3.png](../../Image/image%205%203.png)

## LAND Attacks

- Tấn công LAND là một loại tấn công từ chối dịch vụ (DoS), trong đó kẻ tấn công đặt địa chỉ IP nguồn giống với địa chỉ IP đích. Gói tin độc hại này được gửi đến hệ thống mục tiêu, khiến hệ thống cố gắng giao tiếp với chính nó, dẫn đến sự nhầm lẫn và cạn kiệt tài nguyên hệ thống.
- Cuộc tấn công hoạt động thông qua lưu lượng truy cập lớn và việc tái sử dụng cổng, gây khó khăn cho việc thiết lập các kết nối hợp lệ đến máy chủ bị ảnh hưởng. Khi máy mục tiêu cố gắng trả lời, nó sẽ đi vào một vòng lặp, liên tục gửi các phản hồi cho chính nó, điều này cuối cùng có thể khiến máy nạn nhân bị sập.

![[image 6 2.png|image 6 2.png](../../Image/image%206%202.png)