### Answer the questions below

---

- **What is the name of the malicious/suspicious domain?**

⇒ jx2-bavuong[.]com

- **What is the total number of HTTP requests sent to the malicious domain?**

⇒ 14

- **What is the IP address associated with the malicious domain?**

⇒ 141[.]164[.]41[.]174

- **What is the server info of the suspicious domain?**

⇒ Apache/2.2.11 (Win32) DAV/2 mod_ssl/2.2.11 OpenSSL/0.9.8i PHP/5.2.9

- **Follow the "first TCP stream" in "ASCII".**
    
    **What is the number of listed files?**
    

⇒ 3

- **What is the filename of the first file?**

⇒ 123[.]php

- **Export all HTTP traffic objects.**
    
    **What is the name of the downloaded executable file?**
    

⇒ vlauto[.]exe

- **What is the SHA256 value of the malicious file?**

⇒ b4851333efaf399889456f78eac0fd532e9d8791b23a86a19402c1164aed20de

- **Search the SHA256 value of the file on VirtusTotal.**
    
    **What is the "PEiD packer" value?**
    

⇒ .NET executable

- **Search the SHA256 value of the file on VirtusTotal.**
    
    **What does the "Lastline Sandbox" flag this as?**
    

⇒ MALWARE TROJAN

### Solution

---

![](../../../../Image/image%2037.png)

- `tshark -r directory-curiosity.pcap -V | grep -50` `[jx2-bavuong.com](http://jx2-bavuong.com/)`

![](../../../../Image/image%201%2031.png)

- `tshark -r directory-curiosity.pcap -z follow,tcp,ascii,0 -q`

![](../../../../Image/image%202%2025.png)

⇒ 123.php , vlauto.exe, vlauto.php