## creat arp request directed to broadcast MAC asking for ip 
---
### how to see all the devices ip  connecting is same network:
``` 
 root@kali:~# netdiscover -r 10.0.2.8/24^C
```
## output:
```
 Currently scanning: Finished!   |   Screen View: Unique Hosts                               
                                                                                             
 4 Captured ARP Req/Rep packets, from 3 hosts.   Total size: 240                             
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 10.0.2.1        52:54:00:12:35:00      2     120  Unknown vendor                            
 10.0.2.2        52:54:00:12:35:00      1      60  Unknown vendor                            
 10.0.2.3        08:00:27:88:93:38      1      60  PCS Systemtechnik GmbH   
```

---


# 00_Step
# Address Resolution Protocol - ARP Request

## The Address Resolution Protocol (ARP) is a communication protocol used for discovering the link layer address, such as a MAC address, associated with a given internet layer address, typically an IPv4 address. This mapping is a critical function in the Internet protocol suite. ARP was defined in 1982 by RFC 826,[1] which is Internet Standard STD 37.

## ARP has been implemented with many combinations of network and data link layer technologies, such as IPv4, Chaosnet, DECnet and Xerox PARC Universal Packet (PUP) using IEEE 802 standards, FDDI, X.25, Frame Relay and Asynchronous Transfer Mode (ATM). IPv4 over IEEE 802.3 and IEEE 802.11 is the most common usage.

## In Internet Protocol Version 6 (IPv6) networks, the functionality of ARP is provided by the Neighbor Discovery Protocol (NDP). 

## how ARP works:
```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    scapy.arping(ip)

scan("10.0.2.1/24")
```
## output:
```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
Begin emission:
***Finished sending 256 packets.

Received 3 packets, got 3 answers, remaining 253 packets
  52:54:00:12:35:00 10.0.2.1
  52:54:00:12:35:00 10.0.2.2
  08:00:27:88:93:38 10.0.2.3

```
---

```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    print(arp_request.summary()) 


scan("10.0.2.1/24") ARP who has Net('10.0.2.1/24') says 10.0.2.8

```
---
```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    print(broadcast.summary())


scan("10.0.2.1/24")
```

## output

```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
08:00:27:d8:f8:e3 > ff:ff:ff:ff:ff:ff (0x9000)

```
---
---
---
```
#!/usr/bin/env python

import  scapy.all as scapy


def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    arp_request.show()
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    broadcast.show()
    arp_request_broadcast = broadcast/arp_request
    print(arp_request_broadcast.summary()) # Ether / ARP who has Net('10.0.2.1/24') says 10.0.2.8
    arp_request_broadcast.show()



scan("10.0.2.1/24")
```
## output 
```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
###[ ARP ]### 
  hwtype    = 0x1
  ptype     = 0x800
  hwlen     = 6
  plen      = 4
  op        = who-has
  hwsrc     = 08:00:27:d8:f8:e3
  psrc      = 10.0.2.8
  hwdst     = 00:00:00:00:00:00
  pdst      = Net('10.0.2.1/24')

###[ Ethernet ]### 
  dst       = ff:ff:ff:ff:ff:ff
  src       = 08:00:27:d8:f8:e3
  type      = 0x9000

Ether / ARP who has Net('10.0.2.1/24') says 10.0.2.8
###[ Ethernet ]### 
  dst       = ff:ff:ff:ff:ff:ff
  src       = 08:00:27:d8:f8:e3
  type      = 0x806
###[ ARP ]### 
     hwtype    = 0x1
     ptype     = 0x800
     hwlen     = 6
     plen      = 4
     op        = who-has
     hwsrc     = 08:00:27:d8:f8:e3
     psrc      = 10.0.2.8
     hwdst     = 00:00:00:00:00:00
     pdst      = Net('10.0.2.1/24')


```