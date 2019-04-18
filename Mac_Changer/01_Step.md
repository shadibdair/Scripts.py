##### 01_Step
### MAC-changer
```
#!/usr/bin/env python
import subprocess
interface = "eth0"
new_mac="00:22:11:66:33:88"
print("[+] Changing MAC address for " + interface+ " to " + new_mac)
```
Output:
```
root@kali:~/PycharmProjects/mac_changer# python mac_changer.py
[+] Changing MAC address for eth0 to 00:22:11:66:33:88
```

---
```
#!/usr/bin/env python
import subprocess
interface = "eth0"
new_mac="00:22:11:66:33:88"
print("[+] Changing MAC address for " + interface + " to " + new_mac)
subprocess.call("ifconfig" + interface + "down", shell=True)
subprocess.call("ifconfig" + interface + "hw ether" + new_mac, shell=True)
subprocess.call("ifconfig" + interface + "up", shell=True)
```
Output:
```
root@kali:~/PycharmProjects/mac_changer# python mac_changer.py
[+] Changing MAC address for eth0 to 00:22:11:66:33:88
```

---

```
#!/usr/bin/env python

import subprocess

interface = input("interface > ")
new_mac= input("new MAC > ")

print("[+] Changing MAC address for " + interface + " to " + new_mac)

subprocess.call("ifconfig" + interface + "down", shell=True)
subprocess.call("ifconfig" + interface + "hw ether" + new_mac, shell=True)
subprocess.call("ifconfig" + interface + "up", shell=True)

# another way more secret
subprocess.call(["ifconfig", interface, "down"])
subprocess.call(["ifconfig", interface, "hw","ether",new_mac])
subprocess.call(["ifconfig", interface, "up"])
```
 Output:
 ```
 interface > eth0
 new MAC > 00:22:11:66:33:88
 [+] Changing MAC address for eth0 to 00:22:11:66:33:88
 ```
 ---

 ```
 #!/usr/bin/env python

import subprocess
import optparse

parser = optparse.OptionParser()
# add option to user
parser.add_option("-i","--interface",dest="interface",help="Interface to change its MAC address")
parser.add_option("-m","--mac",dest="new_mac",help="New MAC address")

# allow the object to understand what the user entered & handle-it
(options, arguments) = parser.parse_args()

interface = options.interface
new_mac = options.new_mac

print("[+] Changing MAC address for " + interface + " to " + new_mac)

subprocess.call("ifconfig" + interface + "down", shell=True)
subprocess.call("ifconfig" + interface + "hw ether" + new_mac, shell=True)
subprocess.call("ifconfig" + interface + "up", shell=True)

# another way more secret
subprocess.call(["ifconfig", interface, "down"])
subprocess.call(["ifconfig", interface, "hw","ether",new_mac])
subprocess.call(["ifconfig", interface, "up"])
 ```
 Output:
 ```
 root@kali:~/PycharmProjects/mac_changer# python3 mac_changer.py -i eth0 -m 00:22:11:66:33:88
[+] Changing MAC address for eth0 to 00:22:11:66:33:88
 ```
 #### and also (option):
 ```
 root@kali:~/PycharmProjects/mac_changer# python3 mac_changer.py --help
Usage: mac_changer.py [options]

Options:
  -h, --help            show this help message and exit
  -i INTERFACE, --interface=INTERFACE
                        Interface to change its MAC address
  -m NEW_MAC, --mac=NEW_MAC
                        New MAC address

 ```
 ---
#### with functions
```
#!/usr/bin/env python

import subprocess
import optparse

def get_arguments():
    parser = optparse.OptionParser()
    # add option to user
    parser.add_option("-i", "--interface", dest="interface", help="Interface to change its MAC address")
    parser.add_option("-m", "--mac", dest="new_mac", help="New MAC address")

    # allow the object to understand what the user entered & handle-it
    return parser.parse_args()

def change_mac(interface, new_mac):
    # another way more secret
    print("[+] Changing MAC address for " + interface + " to " + new_mac)
    subprocess.call(["ifconfig", interface, "down"])
    subprocess.call(["ifconfig", interface, "hw", "ether", new_mac])
    subprocess.call(["ifconfig", interface, "up"])

(options, arguments) = get_arguments()
change_mac(options.interface, options.new_mac)

```
Output: 
```
root@kali:~/PycharmProjects/mac_changer# python3 mac_changer.py -i eth0 -m 00:22:11:66:33:88
[+] Changing MAC address for eth0 to 00:22:11:66:33:88
```
---
#### with using condition statment 
```
#!/usr/bin/env python

import subprocess
import optparse

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
change_mac(options.interface, options.new_mac)

```
Output:
```
root@kali:~/PycharmProjects/mac_changer# python3 mac_changer.py --interface eth0
Usage: mac_changer.py [options]

mac_changer.py: error: [-] Please specify an interface, use --help for more info.

```
---
