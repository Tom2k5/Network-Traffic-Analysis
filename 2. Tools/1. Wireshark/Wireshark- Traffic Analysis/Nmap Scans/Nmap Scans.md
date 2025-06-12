- **Nmap** is an industry-standard tool for mapping networks, identifying live hosts and discovering the services.
- **Common Nmap scan types:**
    - TCP connect scans.
    - UDP scans.
    - SYN scans.

### **TCP flags in a nutshell**

| Notes                                                                                      | Wireshark Filters                                                                         |
| ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| Global search                                                                              | • `**tcp**`  <br>•  `udp`                                                                 |
| • Only SYN flag.  <br>• SYN flag is set. The rest of the bits are not important.           | • `**tcp.flags == 2**`  <br>•  <br>`**tcp.flags.syn == 1**`                               |
| • Only ACK flag.  <br>• ACK flag is set. The rest of the bits are not important.           | • `**tcp.flags == 16**`  <br>•  <br>`**tcp.flags.ack == 1**`                              |
| • Only SYN, ACK flags.  <br>• SYN and ACK are set. The rest of the bits are not important. | • `**tcp.flags == 18**`  <br>•  <br>`**(tcp.flags.syn == 1) and (tcp.flags.ack == 1)**`   |
| • Only RST flag.  <br>• RST flag is set. The rest of the bits are not important.           | • `**tcp.flags == 4**`  <br>•  <br>`**tcp.flags.reset == 1**`                             |
| • Only RST, ACK flags.  <br>• RST and ACK are set. The rest of the bits are not important. | • `**tcp.flags == 20**`  <br>•  <br>`**(tcp.flags.reset == 1) and (tcp.flags.ack == 1)**` |
| • Only FIN flag  <br>• FIN flag is set. The rest of the bits are not important.            | • `**tcp.flags == 1**`  <br>•  <br>`**tcp.flags.fin == 1**`                               |

### 1. TCP Connect Scans

- **TCP Connect Scan in a nutshell:**
    - Relies on the three-way handshake.
    - `**nmap -sT**` command.
    - Used by non-root users.
    - A windows size > 1024 bytes as the request expects some data due to the nature of the protocol.

![](../../../../Image/image%2028.png)

- **TCP Scan Pattern:** `**tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size > 1024**` 

![](../../../../Image/image%201%2023.png)

### 2. **SYN Scans**

- **TCP SYN Scan in a nutshell:**
    - Doesn't rely on the three-way handshake.
    -  `**nmap -sS**` command.
    - Used by privileged users.
    - A size ≤ 1024 bytes as the request is not finished and it doesn't expect to receive data.

![](../../../../Image/image%202%2017.png)

- **SYN scan pattern:** `**tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size <= 1024**`  

![](../../../../Image/image%203%2011.png)

### 3. **UDP Scans**

- **UDP Scan in a nutshell:**
    - Doesn't require a handshake process.
    - No prompt for open ports.
    - ICMP error message for close ports.
    -  `**nmap -sU**` command.

![](../../../../Image/image%204%209.png)

- **Closed (port no 69) and open (port no 68) UDP ports:**

![](../../../../Image/image%205%207.png)

- **UDP scan pattern:** `**icmp.type==3 and icmp.code==3**` 

![](../../../../Image/image%206%204.png)

---

- Answer the question:

1. What is the total number of the "TCP Connect" scans?

⇒ `tcp.flags.ack==0 and tcp.flags.syn==1 and tcp.window_size > 1024`

1. How many "UDP close port" messages are there?

⇒ `icmp.type==3 and icmp.code==3`

1. Which UDP port in the 55-70 port range is open?

![](../../../../Image/image%207%202.png)

⇒ `udp.port in {55 .. 70}`