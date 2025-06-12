## **Finding Traditional Telnet Traffic Port 23**

- Notice some telnet communications originating from Port 23

![[image 21.png|image 21.png](../../Image/image%2021.png)

## **Unrecognized TCP Telnet**

- Keeping an eye on these strange port communications can allow us to find potentially malicious actions.

![[image 1 17.png|image 1 17.png](../../Image/image%201%2017.png)

- See a ton of communications from one client on port 9999. We can dive into this a little further by looking at the contents of these communications.

![[image 2 12.png|image 2 12.png](../../Image/image%202%2012.png)

## **Telnet Protocol through IPv6**

- Observing IPv6 traffic can be an indicator of bad actions within our environment. We might notice the usage of IPv6 addresses for telnet like the following.

![[image 3 8.png|image 3 8.png](../../Image/image%203%208.png)

- `((ipv6.src_host == fe80::c9c8:ed3:1b10:f10b) or (ipv6.dst_host == fe80::c9c8:ed3:1b10:f10b)) and telnet`

![[image 4 6.png|image 4 6.png](../../Image/image%204%206.png)

## UDP Communication

- Attackers might opt to use UDP connections over TCP in their exfiltration efforts.

![[image 5 4.png|image 5 4.png](../../Image/image%205%204.png)

- Like TCP, we can follow UDP traffic in Wireshark, and inspect its contents.

![[image 6 3.png|image 6 3.png](../../Image/image%206%203.png)