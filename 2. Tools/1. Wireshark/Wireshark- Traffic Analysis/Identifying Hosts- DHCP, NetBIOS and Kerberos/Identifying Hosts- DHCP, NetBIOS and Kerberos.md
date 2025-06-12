Protocols that can be used in **Host and User identification**:

- **DHCP traffic.**
- **NetBIOS (NBNS) traffic.**
- **Kerberos traffic.**

⇒ **Know how to identifying the hosts and users on the network to decide the investigation's starting point and list the hosts and users associated with the malicious traffic/activity.**

- Enterprise networks use a predefined pattern to name users and hosts.
    - **Good Sides**: Easy to identify a user or host by looking at the name.
    - **Bad Sides:** Easy to clone that pattern and live in the enterprise network for adversaries.

## **DHCP Analysis**

- **Giao thức DHCP:**
    - Quản lý việc gán địa chỉ IP tự động và các thông số giao tiếp cần thiết.
    - Phân tích DHCP có thể cung cấp thông tin quan trọng về máy chủ và người dùng trên mạng.

|Notes|Wireshark Filter|
|---|---|
|Global search|`dhcp` hoặc `bootp`|
|• “**DHCP Request"** packet chứa thông tin máy chủ.  <br>• **"DHCP ACK"** packet đại diện Accepted Requests  <br>• **"DHCP NAK"** packet đại diện Denied Requests.|• Request: `dhcp.option.dhcp == 3`  <br>• ACK:  <br>`dhcp.option.dhcp == 5`  <br>• NAK:  <br>`dhcp.option.dhcp == 6`|
|**"DHCP Request"**  <br>• **Option 12:** Hostname.  <br>• **Option 50:** Requested IP address.  <br>• **Option 51:** Requested IP lease time.  <br>• **Option 61:** Client's MAC address.|`dhcp.option.hostname contains "keyword"`|
|**"DHCP ACK"**  <br>• **Option 15:** Domain name.  <br>• **Option 51:** Assigned IP lease time.|`dhcp.option.domain_name contains "keyword"`|
|**"DHCP NAK"**  <br>• **Option 56:** Message (rejection details/reason).|As the message could be unique according to the case/situation, It is suggested to read the message instead of filtering it.|

⇒ **Notes:** Except for **Request** type, you should filter **the packet type** first, and then you can filter **the rest of the options** by **"applying as column"** or use the advanced filters like **"contains" and "matches"**.

![[image 30.png|image 30.png](../../../../Image/image%2030.png)

## NetBIOS (NBNS) Analysis

- **NetBIOS** or **Net**work **B**asic **I**nput/**O**utput **S**ystem is the technology responsible for allowing applications on different hosts to communicate with each other. 

![[image 1 25.png|image 1 25.png](../../../../Image/image%201%2025.png)

![[image 2 19.png|image 2 19.png](../../../../Image/image%202%2019.png)

## **Kerberos Analysis**

- **Kerberos** is the default authentication service for Microsoft Windows domains. It is responsible for authenticating service requests between two or more computers over the untrusted network. The ultimate aim is to prove identity securely.

|Notes|Wireshark filter|
|---|---|
|Global search|• `**kerberos**`|
|**User account search:**  <br>• **CNameString:** The username.  <br>**Note:** Some packets could provide hostname information in this field. To avoid this confusion, filter the **"$"** value. The values end with **"$"** are hostnames, and the ones without it are usernames.|• `**kerberos.CNameString contains "keyword"**`  <br>  <br>  <br>•  <br>`**kerberos.CNameString and !(kerberos.CNameString contains "$" )**`|
|**"Kerberos"** options for grabbing the low-hanging fruits:  <br>• **pvno:** Protocol version.  <br>• **realm:** Domain name for the generated ticket.  <br>• **sname:** Service and domain name for the generated ticket.  <br>• **addresses:** Client IP address and NetBIOS name.  <br>**Note:** the "addresses" information is only available in request packets.|•  <br>`**kerberos.pvno == 5**`  <br>  <br>  <br>•  <br>`**kerberos.realm contains ".org"**`  <br>  <br>  <br>•  <br>`**kerberos.SNameString == "krbtg"**`|

![[image 3 13.png|image 3 13.png](../../../../Image/image%203%2013.png)

![[image 4 11.png|image 4 11.png](../../../../Image/image%204%2011.png)

![[image 5 9.png|image 5 9.png](../../../../Image/image%205%209.png)

![[image 6 5.png|image 6 5.png](../../../../Image/image%206%205.png)

![[image 7 3.png|image 7 3.png](../../../../Image/image%207%203.png)

![[image 8 2.png|image 8 2.png](../../../../Image/image%208%202.png)