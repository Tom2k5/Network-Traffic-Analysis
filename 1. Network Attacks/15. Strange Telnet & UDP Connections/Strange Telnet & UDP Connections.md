## **Finding Traditional Telnet Traffic Port 23**

- Notice some telnet communications originating from Port 23

![](../../Image/image%2021.png)

## **Unrecognized TCP Telnet**

- Keeping an eye on these strange port communications can allow us to find potentially malicious actions.

![](../../Image/image%201%2017.png)

- See a ton of communications from one client on port 9999. We can dive into this a little further by looking at the contents of these communications.

![](../../Image/image%202%2012.png)

## **Telnet Protocol through IPv6**

- Observing IPv6 traffic can be an indicator of bad actions within our environment. We might notice the usage of IPv6 addresses for telnet like the following.

![](../../Image/image%203%208.png)

- `((ipv6.src_host == fe80::c9c8:ed3:1b10:f10b) or (ipv6.dst_host == fe80::c9c8:ed3:1b10:f10b)) and telnet`

![](../../Image/image%204%206.png)

## UDP Communication

- Attackers might opt to use UDP connections over TCP in their exfiltration efforts.

![](../../Image/image%205%204.png)

- Like TCP, we can follow UDP traffic in Wireshark, and inspect its contents.

![](../../Image/image%206%203.png)