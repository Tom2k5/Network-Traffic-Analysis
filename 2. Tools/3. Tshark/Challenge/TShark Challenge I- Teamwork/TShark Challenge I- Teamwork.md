### Answer the questions below

---

1. **What is the full URL of the malicious/suspicious domain address?**
    
    ⇒ hxxp[://]www[.]paypal[.]com4uswebappsresetaccountrecovery[.]timeseaways[.]com/
    
2. **When was the URL of the malicious/suspicious domain address first submitted to VirusTotal?**
    
    ⇒ 2017-04-17 22:52:53 UTC
    
3. **Which known service was the domain trying to impersonate?**
    
    ⇒ PayPal
    
4. **What is the IP address of the malicious domain?**
    
    ⇒ 184[.]154[.]127[.]226
    
5. **What is the email address that was used?**
    
    ⇒ johnny5alive[at]gmail[.]com
    

### Solution

---

- `tshark -r teamwork.pcap -Y "dns"`

```Bash
  39   8.965505  75.75.75.75 ? 192.168.1.100 DNS 136 Standard query response 0x6926 A www.paypal.com4uswebappsresetaccountrecovery.timeseaways.com A 184.154.127.226
```

- `tshark -r teamwork.pcap -Y "http"`

```Bash
 215  28.604818 192.168.1.100 ? 184.154.127.226 HTTP 551 GET /suspecious.php HTTP/1.1 
```

- `tshark -r teamwork.pcap -V -Y "http" | gre[ "gmail"`

![](../../../../Image/image%2036.png)