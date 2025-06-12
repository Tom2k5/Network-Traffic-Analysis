## **Excessive SYN Flags**

![](../../Image/image%2013.png)

- It is worth noting that there are two primary scan types we might detect that use the SYN flag. These are:

1. `SYN Scans` - In these scans the behavior will be as we see, however the attacker will pre-emptively end the handshake with the RST flag.
2. `SYN Stealth Scans` - In this case the attacker will attempt to evade detection by only partially completing the TCP handshake.

## **No Flags**

- In a NULL scan an attacker sends TCP packets with no flags. TCP connections behave like the following when a NULL packet is received.

1. `If the port is open` - The system will not respond at all since there is no flags.
2. `If the port is closed` - The system will respond with an RST packet.

![](../../Image/image%201%2011.png)

## **Too Many ACKs**

In this case the attacker might be employing the usage of an ACK scan. In the case of an ACK scan TCP connections will behave like the following.

1. `If the port is open` - The affected machine will either not respond, or will respond with an RST packet.
2. `If the port is closed` - The affected machine will respond with an RST packet.

![](../../Image/image%202%209.png)

## **Excessive FINs**

In this case, all TCP packets will be marked with the FIN flag. We might notice the following behavior from our affected machine.

1. `If the port is open` - Our affected machine simply will not respond.
2. `If the port is closed` - Our affected machine will respond with an RST packet.

![](../../Image/image%203%206.png)

## **Just too many flags**

- Utilize a Xmas tree scan, which is when they put all TCP flags on their transmissions. Similarly, our affected host might respond like the following when all flags are set.

1. `If the port is open` - The affected machine will not respond, or at least it will with an RST packet.
2. `If the port is closed` - The affected machine will respond with an RST packet.

![](../../Image/image%204%205.png)