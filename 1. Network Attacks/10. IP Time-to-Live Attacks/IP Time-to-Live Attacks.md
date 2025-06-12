![](../../Image/image%2012.png)

- Time-to-Live attacks are primarily utilized as a means of evasion by attackers. Basically speaking the attacker will intentionally set a very low TTL on their IP packets in order to attempt to evade firewalls, IDS, and IPS systems. These work like the following.

1. The attacker will craft an IP packet with an intentionally low TTL value (1, 2, 3 and so on).
2. Through each host that this packet passes through this TTL value will be decremented by one until it reaches zero.
3. Upon reaching zero this packet will be discarded. The attacker will try to get this packet discarded before it reaches a firewall or filtering system to avoid detection/controls.
4. When the packets expire, the routers along the path generate ICMP Time Exceeded messages and send them back to the source IP address.

## **Finding Irregularities in IP TTL**

- A returned SYN, ACK message from one of our legitimate service ports on our affected host. In doing so, the attacker might have successfully evaded one of our firewall controls.

![](../../Image/image%201%2010.png)

- So, if we were to open one of these packets, we could realistically see why this is. Suppose we opened the IPv4 tab in Wireshark for any of these packets. We might notice a very low TTL like the following.

![](../../Image/image%202%208.png)

## Solution

- Implement a control which discards or filters packets that do not have a high enough TTL. In doing so, we can prevent these forms of IP packet crafting attacks.