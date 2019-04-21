## Send packets and receive response
---
```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast/arp_request
    answered, unaswered = scapy.srp(arp_request_broadcast, timeout = 1)
    print(answered.summary())


scan("10.0.2.1/24")


'''
The output:

root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
Begin emission:
***Finished sending 256 packets.

Received 3 packets, got 3 answers, remaining 253 packets
Ether / ARP who has 10.0.2.1 says 10.0.2.8 ==> Ether / ARP is at 52:54:00:12:35:00 says 10.0.2.1 / Padding
Ether / ARP who has 10.0.2.2 says 10.0.2.8 ==> Ether / ARP is at 52:54:00:12:35:00 says 10.0.2.2 / Padding
Ether / ARP who has 10.0.2.3 says 10.0.2.8 ==> Ether / ARP is at 08:00:27:88:93:38 says 10.0.2.3 / Padding
None

'''
```