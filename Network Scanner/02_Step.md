## parse the response
---
```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast/arp_request
    answered_List = scapy.srp(arp_request_broadcast, timeout = 1)[0]

    for elemetn in answered_List:
        print(elemetn)
        print("-------------------------------------------------")


scan("10.0.2.1/24")
```


## output :
```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
Begin emission:
***Finished sending 256 packets.

Received 3 packets, got 3 answers, remaining 253 packets
(<Ether  dst=ff:ff:ff:ff:ff:ff type=0x806 |<ARP  pdst=10.0.2.1 |>>, <Ether  dst=08:00:27:d8:f8:e3 src=52:54:00:12:35:00 type=0x806 |<ARP  hwtype=0x1 ptype=0x800 hwlen=6 plen=4 op=is-at hwsrc=52:54:00:12:35:00 psrc=10.0.2.1 hwdst=08:00:27:d8:f8:e3 pdst=10.0.2.8 |<Padding  load='\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' |>>>)
-------------------------------------------------
(<Ether  dst=ff:ff:ff:ff:ff:ff type=0x806 |<ARP  pdst=10.0.2.2 |>>, <Ether  dst=08:00:27:d8:f8:e3 src=52:54:00:12:35:00 type=0x806 |<ARP  hwtype=0x1 ptype=0x800 hwlen=6 plen=4 op=is-at hwsrc=52:54:00:12:35:00 psrc=10.0.2.2 hwdst=08:00:27:d8:f8:e3 pdst=10.0.2.8 |<Padding  load='\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' |>>>)
-------------------------------------------------
(<Ether  dst=ff:ff:ff:ff:ff:ff type=0x806 |<ARP  pdst=10.0.2.3 |>>, <Ether  dst=08:00:27:d8:f8:e3 src=08:00:27:88:93:38 type=0x806 |<ARP  hwtype=0x1 ptype=0x800 hwlen=6 plen=4 op=is-at hwsrc=08:00:27:88:93:38 psrc=10.0.2.3 hwdst=08:00:27:d8:f8:e3 pdst=10.0.2.8 |<Padding  load='\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' |>>>)

```
---
---
```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast/arp_request
    answered_List = scapy.srp(arp_request_broadcast, timeout = 1)[0]

    for elemetn in answered_List:
        print(elemetn[1].psrc)
        print(elemetn[1].hwrc)
        print("-------------------------------------------------")


scan("10.0.2.1/24")

```
## output :
```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
Begin emission:
***Finished sending 256 packets.

Received 3 packets, got 3 answers, remaining 253 packets
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