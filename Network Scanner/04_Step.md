## Dictionaries
##### similar to lists but use key instead oof index.
---
```
#!/usr/bin/env python

import  scapy.all as scapy

def scan(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast/arp_request
    answered_List = scapy.srp(arp_request_broadcast, timeout = 1 , verbose=False)[0]

    print("IP\t\t\tMAC Address\n--------------------------------------------------------------")
    clients_List = []
    for elemetn in answered_List:
        clients_dict = {"ip": elemetn[1].psrc, "mac": elemetn[1].hwsrc}
        clients_List.append(clients_dict)
        print(elemetn[1].psrc + "\t\t" + elemetn[1].hwsrc)
    print(clients_List)

scan("10.0.2.1/24")

```
## output :
```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
IP			MAC Address
--------------------------------------------------------------
10.0.2.1		52:54:00:12:35:00
10.0.2.2		52:54:00:12:35:00
10.0.2.3		08:00:27:88:93:38
[{'ip': '10.0.2.1', 'mac': '52:54:00:12:35:00'}, {'ip': '10.0.2.2', 'mac': '52:54:00:12:35:00'}, {'ip': '10.0.2.3', 'mac': '08:00:27:88:93:38'}]

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
    answered_List = scapy.srp(arp_request_broadcast, timeout = 1 , verbose=False)[0]

    clients_List = []
    for elemetn in answered_List:
        clients_dict = {"ip": elemetn[1].psrc, "mac": elemetn[1].hwsrc}
        clients_List.append(clients_dict)

    return clients_List

def print_result(results_list):
    print("IP\t\t\tMAC Address\n--------------------------------------------------------------")
    for client in results_list:
        print(client["ip"] + "\t\t" + client["mac"])

scan_result = scan("10.0.2.1/24")
print_result(scan_result)

```
## output :
```
root@kali:~/PycharmProjects/network_scanner/venv# python network_scanner.py 
IP			MAC Address
--------------------------------------------------------------
10.0.2.1		52:54:00:12:35:00
10.0.2.2		52:54:00:12:35:00
10.0.2.3		08:00:27:88:93:38

```
---
---


