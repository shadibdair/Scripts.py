## print result
---
```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast/arp_request
    answered_List = scapy.srp(arp_request_broadcast, timeout = 1 , verbose=False)[0]

    for elemetn in answered_List:
        print(elemetn[1].psrc)
        print(elemetn[1].hwsrc)
        print("-------------------------------------------------")


scan("10.0.2.1/24")

```
## output : the new is (verbose=False) it remove this
* Begin emission:

* ***Finished sending 256 packets.

* Received 3 packets, got 3 answers, remaining 253 packets

```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
10.0.2.1
52:54:00:12:35:00
-------------------------------------------------
10.0.2.2
52:54:00:12:35:00
-------------------------------------------------
10.0.2.3
08:00:27:88:93:38
-------------------------------------------------

```

---
## print Header
```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast/arp_request
    answered_List = scapy.srp(arp_request_broadcast, timeout = 1 , verbose=False)[0]

    print("IP\t\t\tMAC Address\n--------------------------------------------------------------")
    for elemetn in answered_List:
        print(elemetn[1].psrc + "\t\t" + elemetn[1].hwsrc)

        print("--------------------------------------------------------------------------------")


scan("10.0.2.1/24")

```
## output:
```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
IP			MAC Address
--------------------------------------------------------------
10.0.2.1		52:54:00:12:35:00
--------------------------------------------------------------
10.0.2.2		52:54:00:12:35:00
--------------------------------------------------------------
10.0.2.3		08:00:27:88:93:38
--------------------------------------------------------------

```
---
---