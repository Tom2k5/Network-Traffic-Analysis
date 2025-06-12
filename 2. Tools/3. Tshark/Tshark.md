
| **capinfos** | A program that provides details of a specified capture file. It is suggested to view the summary of the capture file before starting an investigation. |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |

## Main Parameters

| **Parameter**    | **Purpose**                                                                                            |
| ---------------- | ------------------------------------------------------------------------------------------------------ |
| -h               | • Display the help page with the most common features.  <br>•  <br>`tshark -h`                         |
| -v               | • Show version info.  <br>•  <br>`tshark -v`                                                           |
| -D               | • List available sniffing interfaces.  <br>•  <br>`tshark -D`                                          |
| -i               | • Choose an interface to capture live traffic.  <br>•  <br>`tshark -i 1`  <br>•  <br>`tshark -i ens55` |
| **No Parameter** | • Sniff the traffic like tcpdump.  <br>•  <br>`tshark`                                                 |

## Extract Information

- **tshark -r demo.pcapng -Y 'http.request.method matches "(GET|POST)"' -T fields -e frame.time -E header=y**