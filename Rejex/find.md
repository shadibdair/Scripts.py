## find the eth0 mac address
```
#!/usr/bin/env python

import subprocess
import optparse
import re # Rejex

def get_arguments():
    parser = optparse.OptionParser()
    # add option to user
    parser.add_option("-i", "--interface", dest="interface", help="Interface to change its MAC address")
    parser.add_option("-m", "--mac", dest="new_mac", help="New MAC address")

    # allow the object to understand what the user entered & handle-it
    (options, arguments) =  parser.parse_args()
    if not options.interface: # first condition
        # code to handle error
        parser.error("[-] Please specify an interface, use --help for more info.")
    elif not options.new_mac: # second condition if the first condition was ret False
        # code to handle error
        parser.error("[-] Please specify an interface, use --help for more info.")
    return options


def change_mac(interface, new_mac):
    # another way more secret
    print("[+] Changing MAC address for " + interface + " to " + new_mac)
    subprocess.call(["ifconfig", interface, "down"])
    subprocess.call(["ifconfig", interface, "hw", "ether", new_mac])
    subprocess.call(["ifconfig", interface, "up"])

options = get_arguments()
#change_mac(options.interface, options.new_mac)

ifconfig_result = subprocess.check_output(["ifconfig", options.interface])
print(ifconfig_result)

mac_address_search_result = re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w", ifconfig_result)
print(mac_address_search_result.group(0))
```

## output
```
root@kali:~/PycharmProjects/mac_changer# python mac_changer.py --interface eth0 --mac 22:54:22:66:35:99
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
        inet6 fe80::a00:27ff:fef8:42a7  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:f8:42:a7  txqueuelen 1000  (Ethernet)
        RX packets 17767  bytes 23297037 (22.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 6144  bytes 640645 (625.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


08:00:27:f8:42:a7 // this is what we wanted

```

---
```
#!/usr/bin/env python

import subprocess
import optparse
import re # Rejex

def get_arguments():
    parser = optparse.OptionParser()
    # add option to user
    parser.add_option("-i", "--interface", dest="interface", help="Interface to change its MAC address")
    parser.add_option("-m", "--mac", dest="new_mac", help="New MAC address")

    # allow the object to understand what the user entered & handle-it
    (options, arguments) =  parser.parse_args()
    if not options.interface: # first condition
        # code to handle error
        parser.error("[-] Please specify an interface, use --help for more info.")
    elif not options.new_mac: # second condition if the first condition was ret False
        # code to handle error
        parser.error("[-] Please specify an interface, use --help for more info.")
    return options


def change_mac(interface, new_mac):
    # another way more secret
    print("[+] Changing MAC address for " + interface + " to " + new_mac)
    subprocess.call(["ifconfig", interface, "down"])
    subprocess.call(["ifconfig", interface, "hw", "ether", new_mac])
    subprocess.call(["ifconfig", interface, "up"])

def get_current_mac(interface):
    ifconfig_result = subprocess.check_output(["ifconfig", interface])
    mac_address_search_result = re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w", ifconfig_result)

    if mac_address_search_result:
        return mac_address_search_result.group(0)
    else:
        print("[-] Could not read MAC address.")


options = get_arguments() # is going to get me thearguments that the user enters
current_mac = get_current_mac(options.interface) # get me the current MAC
print("Current  MAC = " + str(current_mac)) # pinting the current MAC

change_mac(options.interface, options.new_mac) # Changing the MAC address

current_mac = get_current_mac(options.interface)
if current_mac == options.new_mac:
    print("[+] MAC address was successfully changed to " + current_mac)
else:
    print("[-] MAC address did not get changed.")


```
## output
```
root@kali:~/PycharmProjects/mac_changer# python mac_changer.py --interface eth0 --mac 22:54:22:66:35:13
Current  MAC = 22:54:22:66:35:13
[+] Changing MAC address for eth0 to 22:54:22:66:35:13
[+] MAC address was successfully changed to 22:54:22:66:35:13

```