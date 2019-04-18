##### 00_Step
## mac changer
```
#!/usr/bin/env python

# The subprocess module allows you to spawn new processes,
# connect to their input/output/error pipes,
# and obtain their return codes.
import subprocess

# Run the command described by args. Wait for command to complete,
# then return the returncode attribute.
subprocess.call("ifconfig", shell=True)
```
## output:
```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
        inet6 fe80::a00:27ff:fef8:42a7  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:f8:42:a7  txqueuelen 1000  (Ethernet)
        RX packets 3816  bytes 3851623 (3.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2649  bytes 264551 (258.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 20  bytes 1116 (1.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 20  bytes 1116 (1.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
---

## Code in Python that changed the mac address:
```
#!/usr/bin/env python

# The subprocess module allows you to spawn new processes,
# connect to their input/output/error pipes,
# and obtain their return codes.
import subprocess

# Run the command described by args. Wait for command to complete,
# then return the returncode attribute.
subprocess.call("ifconfig eth0 down", shell=True)
subprocess.call("ifconfig eth0 hw ether 00:33:11:55:32:88", shell=True)
subprocess.call("ifconfig eth0 up", shell=True)
```
## run it in Terminator:
```
root@kali:~/PycharmProjects/mac_changer# python mac_changer.py

root@kali:~/PycharmProjects/mac_changer# ifconfig eth0
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.5  netmask 255.255.255.0  broadcast 10.0.2.255
        inet6 fe80::233:11ff:fe55:3288  prefixlen 64  scopeid 0x20<link>
        ether 00:33:11:55:32:88  txqueuelen 1000  (Ethernet)
        RX packets 3883  bytes 3891194 (3.7 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2781  bytes 278605 (272.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```
