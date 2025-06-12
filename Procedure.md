1. `What is the issue?`
    - Suspected breach?
    - Networking issue?
2. `Define our scope and the goal (what are we looking for? which time period?)`
    - Target: multiple hosts potentially downloading a malicious file from bad.example.com
    - When: within the last 48 hours + 2 hours from now.
    - Supporting info:
        - Filenames/Types ⇒ 'superbad.exe' 'new-crypto-miner.exe'
3. `Define our target(s) (net / host(s) / protocol)`
    - Scope: 192.168.100.0/24 network protocols used were HTTP and FTP.
4. `Capture network traffic`
    - Plug into a link with access to the 192.168.100.0/24 network to capture live traffic to try and grab one of the executables in transfer. See if an admin can pull PCAP and/or netflow data from our SIEM for the historical data.
5. `Identification of required network traffic components (filtering)`
    - Once we have traffic, filter out any traffic not needed for this investigation to include; any traffic that matches our common baseline and keep anything relevant to the scope. `HTTP and FTP from the subnet, anything transferring or containing a GET request for the suspected executable files.
6. `An understanding of captured network traffic`
    - Once we have filtered out the noise, it's time to dig for our targets—filter on things like `ftp-data` to find any files transferred and reconstruct them. For HTTP, we can filter on `http.request.method == "GET"` to see any GET requests that match the filenames we are searching for. This can show us who has acquired the files and potential other transfers internal to the network on the same protocols.
7. `Note-taking and mind mapping of the found results.`
    - Annotating everything we do, see, or find throughout the investigation is crucial. Ensure we are taking ample notes, including:
        - Timeframes we captured traffic during.
        - Suspicious hosts within the network.
        - Conversations containing the files in question. ( to include timestamps and packet numbers)
8. `Summary of the analysis (what did we find?)`
    - Finally, summarize what has been found, explaining the relevant details so that superiors can make an informed decision to quarantine the affected hosts or perform more significant incident response.
    - Our analysis will affect decisions made, so it is essential to be as clear and concise as possible.

## **Key Components of an Effective Analysis**

### **1. Know your environment**

Để phân tích lưu lượng mạng hiệu quả, bạn cần biết rõ môi trường mạng của mình. Nếu không biết một thiết bị (host) có thuộc về mạng của bạn hay không, bạn sẽ không thể xác định được liệu nó có phải là thiết bị lạ (rogue) hay không.

- Cách thực hiện:
    - Duy trì một **asset inventory** liệt kê tất cả các thiết bị, ứng dụng và dịch vụ trong mạng.
    - Có **network map** để hiểu rõ cách các thiết bị kết nối và tương tác với nhau.

### **2. Placement is Key**

Vị trí đặt công cụ thu thập lưu lượng (capture tool) quyết định khả năng thu thập thông tin chính xác và đầy đủ về sự cố.

- **Cách thực hiện:**
    - Nếu sự cố xuất phát từ **internet**, hãy đặt công cụ thu thập lưu lượng ở các liên kết đầu vào (inbound links) để có cái nhìn toàn diện về lưu lượng.
    - Nếu sự cố chỉ xảy ra trên **một thiết bị cụ thể trong mạng nội bộ**, hãy đặt công cụ thu thập trong cùng phân đoạn mạng (segment) với thiết bị đó để theo dõi lưu lượng cục bộ.

### **3. Persistence**

Không phải lúc nào sự cố cũng dễ dàng phát hiện ngay lập tức. Một số hoạt động độc hại có thể xảy ra không thường xuyên, chẳng hạn như máy chủ Command and Control (C&C) của kẻ tấn công chỉ liên lạc với máy tính bị nhiễm malware vài giờ một lần, thậm chí một lần mỗi ngày.

- **Cách thực hiện:**
    - Kiên trì theo dõi và phân tích lưu lượng mạng trong thời gian dài.
    - Không bỏ cuộc nếu không phát hiện ra vấn đề ngay lần đầu tiên.

## **Analysis Approach**

### Standard Protocols

- Phân tích protocol đến từ internet: HTTP/S, FTP, SMTP, TCP/UDP.
- Phân tích protocol cho phép giao tiếp giữa các network: SSH, RDP, Telnet.
- Khi quan sát các anomaly, quan tâm đến chính sách bảo mật của network. Công ty có cho phép kết nối RDP, Telnet từ nơi khác không ?

### Patterns

- Phát hiện các hoạt động đáng ngờ như một hoặc nhiều máy tính giao tiếp với một địa chỉ internet cụ thể vào cùng một thời điểm mỗi ngày, có thể là dấu hiệu của Command and Control (C&C).

### Host to host

- Trong một mạng thông thường, các máy tính của người dùng (user hosts) **ít khi giao tiếp trực tiếp với nhau**. Thay vào đó, chúng thường giao tiếp với các **hạ tầng mạng** hoặc **dịch vụ doanh nghiệp** để thực hiện các chức năng cần thiết.
- **VD về các giao tiếp thông thường:**
    - **Cấp phát địa chỉ IP**: Máy tính giao tiếp với DHCP server để nhận địa chỉ IP.
    - **Yêu cầu DNS**: Máy tính giao tiếp với DNS server để phân giải tên miền.
    - **Dịch vụ doanh nghiệp**: Máy tính kết nối với các dịch vụ như file server, web server nội bộ, hoặc các ứng dụng doanh nghiệp, file sharing.
    - **Tìm đường ra internet**: Máy tính giao tiếp với gateway hoặc router để truy cập internet.
- Nếu bạn thấy hai máy tính trong mạng nội bộ giao tiếp trực tiếp với nhau, đây có thể là dấu hiệu bất thường.
- **Nguy cơ tiềm ẩn**: Lưu lượng host-to-host có thể liên quan đến các hoạt động độc hại, chẳng hạn như:
    - **Lây lan malware**: Malware có thể lây lan từ máy này sang máy khác thông qua giao tiếp trực tiếp.
    - **Lateral movement**: Kẻ tấn công có thể di chuyển ngang trong mạng (lateral movement) để leo thang đặc quyền hoặc truy cập dữ liệu nhạy cảm.
    - **C2 (Command and Control) traffic**: Máy tính bị nhiễm malware có thể giao tiếp với nhau để nhận lệnh từ kẻ tấn công.

### Unique Events

- Chú ý đến các thay đổi bất thường, chẳng hạn như một máy tính thường truy cập một trang web 10 lần/ngày nhưng đột nhiên giảm xuống còn 1 lần.
- Cảnh giác với các chuỗi User-Agent không khớp với ứng dụng hoặc máy chủ trong mạng, hoặc các cổng (port) ngẫu nhiên chỉ được mở một vài lần, vì đây có thể là dấu hiệu của C2 callbacks hoặc hành vi bất thường.