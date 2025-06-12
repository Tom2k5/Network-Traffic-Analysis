- Không phải lúc nào cũng có một đội ngũ lớn tham gia vào việc điều tra. Là một nhà phân tích bảo mật, có những trường hợp bạn cần tự mình phát hiện bất thường, xác định nguồn gốc và thực hiện hành động.
- Tạo quy tắc tường lửa bằng cách sử dụng menu **"Tools --> Firewall ACL Rules"**.
- **Tính năng tạo quy tắc tường lửa**: Khi bạn sử dụng tính năng này, một cửa sổ mới sẽ mở ra và cung cấp một tập hợp các quy tắc dựa trên IP, cổng và địa chỉ MAC cho các mục đích khác nhau.
    
    ⇒ Lưu ý rằng những quy tắc này được tạo ra để áp dụng cho giao diện tường lửa **bên ngoài**.
    
- **Các loại quy tắc đặt cho hệ thống tường lửa**:
    
    - **Netfilter (iptables)**: Hệ thống quản lý tường lửa trên Linux.
    - **Cisco IOS (chuẩn/mở rộng)**: Hệ điều hành cho thiết bị mạng của Cisco.
    - **IP Filter (ipfilter)**: Một công cụ để kiểm soát lưu lượng mạng.
    - **IPFirewall (ipfw)**: Tường lửa cho hệ điều hành BSD.
    - **Packet filter (pf)**: Tường lửa cho OpenBSD.
    - **Windows Firewall (netsh định dạng mới/cũ)**: Tường lửa tích hợp trong Windows.
        
        ![[image 35.png|image 35.png](../../../../Image/image%2035.png)
        
    
    ## Answer the questions:
    
    1. **Select packet number 99. Create a rule for "IPFirewall (ipfw)". What is the rule for "denying source IPv4 address"?**
        
        ![[image 1 30.png|image 1 30.png](../../../../Image/image%201%2030.png)
        
    2. **Select packet number 231. Create "IPFirewall" rules. What is the rule for "allowing destination MAC address"?**

![[image 2 24.png|image 2 24.png](../../../../Image/image%202%2024.png)