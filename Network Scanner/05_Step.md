## all Steps
```
#!/usr/bin/env python

import  scapy.all as scapy
import argparse

def get_arguments():
    parser = argparse.ArgumentParser()
    parser.add_argument("-t", "--target", dest="target", help="Target IP / IP range.")
    option = parser.parse_args()
    return  option

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

option = get_arguments()
scan_result = scan(option.target)
print_result(scan_result)

```
## output
```
IP			MAC Address
--------------------------------------------------------------
10.0.2.1		52:54:00:12:35:00
10.0.2.2		52:54:00:12:35:00
10.0.2.3		08:00:27:88:93:38

```