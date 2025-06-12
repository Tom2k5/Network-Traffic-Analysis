## **ARP Scanning Signs**

Some typical red flags indicative of ARP scanning are:

1. `Broadcast ARP requests sent to sequential IP addresses (.1,.2,.3,...)`
2. `Broadcast ARP requests sent to non-existent hosts`
3. `Potentially, an unusual volume of ARP traffic originating from a malicious or compromised host`

## **Finding ARP Scanning**

![](../../Image/image%207.png)

- Nếu thấy một máy tính duy nhất gửi yêu cầu ARP đến tất cả các địa chỉ IP trong mạng, đây là dấu hiệu của ARP Scanning.

## **Identifying Denial-of-Service**

- An attacker can exploit ARP scanning to compile a list of live hosts.
- Upon acquiring this list, the attacker might alter their strategy to deny service to all these machines. Essentially, they will strive to contaminate an entire subnet and manipulate as many ARP caches as possible.

![](../../Image/image%201%205.png)

- `53:0c:ba` là attacker, `e2:d5:c3` là router, còn lại là client.
- Corrupt the router's ARP cache bằng cách thay thế toàn bộ IP của client trong router.
- Duplicate allocation of `192.168.10.1` to client devices, từ đó tự đóng vai trò là rogue router.