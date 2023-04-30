# Cisco IOS Automation with Python
I have continued exploring network automation with Python, and I decided to start automating Cisco IOS configs using the `netmiko` library.

# Python Libraries
I am using the following python libraries to write my automation scripts:
* netmiko

# Demo Topology
For the lab, I constructed a collapsed core topology and configured an edge router, and several switches, all using python. Below is the topology I designed
and used for the exercise. 
<img src="https://github.com/kmcgaugh/kmohr-tech-projects/blob/main/images/Untitled.drawio.png"></img>

# Script Examples
### Switch "floor*" Configuration
I wrote the below script to automate configuration of the "floor*" access switches.
The script sets up VLANs, attaches switch ports to said VLANs, and configures trunk ports.
At the end, the script prompts to choose whether or not to save the configuration (I did this for error checking purposes).

"floor1" script:
```
# Compiled by Kaleb McGaugh Mohr
# Sun 30 Apr 2023
# For demo purposes only; not used in production/not meant for ridistribution

from netmiko import ConnectHandler
import getpass

password = getpass()
secret = getpass()

floor1 = {
        'device_type': 'cisco_ios',
        'host': '172.28.10.36',
        'username': 'kaleb',
        'password': password,
        'secret': secret,
        }
# connection handler
net_connect = ConnectHandler(**floor1)
# enable mode
net_connect.enable()
# configuration
config_commands = ['vlan 10', #vlan config
        'name accounting',
        'vlan 20',
        'name buisdev',
        'vlan 30',
        'name marketing',
        'vlan 40',
        'name manuf',
        'exit',
        'int g0/2', #trunk configuration
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'no shut',
        'int g0/3',
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'no shut',
        'int g1/0',
        'switchport mode access',
        'switchport access vlan 10',
        'switchport nonegotiate',
        'no shut',
        'int g1/1',
        'switchport mode access',
        'switchport access vlan 20',
        'switchport nonegotiate',
        'no shut',
        'int g1/2',
        'switchport mode access',
        'switchport access vlan 30',
        'switchport nonegotiate',
        'no shut',
        'int g1/3',
        'switchport mode access',
        'switchport access vlan 40',
        'switchport nonegotiate',
        'no shut',
        ]
output = net_connect.send_config_set(config_commands)
print(output)

# verification
# vlans
vlans = net_connect.send_command('show vlan brief')
print(vlans)
# trunk
trunks = net_connect.send_command('show int trunk')
print(trunks)
# save prompt
save_prompt = input('Save the running configuration? (y/n): ')
if save_prompt == 'y':
    net_connect.save_config()
elif save_prompt == 'n':
    print('exiting without saving...')
else:
    print('sorry, you need to make a decision.')

```
"floor2" script:
```
# Compiled by Kaleb McGaugh Mohr
# Sun 30 Apr 2023
# For demo purposes only; not used in production/not meant for ridistribution

from netmiko import ConnectHandler
import getpass

password = getpass()
secret = getpass()

floor2 = {
        'device_type': 'cisco_ios',
        'host': '172.28.10.165',
        'username': 'kaleb',
        'password': password,
        'secret': secret,
        }
# connection handler
net_connect = ConnectHandler(**floor1)
# enable mode
net_connect.enable()
# configuration
config_commands = ['vlan 10', #vlan config
        'name accounting',
        'vlan 20',
        'name buisdev',
        'vlan 30',
        'name marketing',
        'vlan 40',
        'name manuf',
        'exit',
        'int g0/2', #trunk configuration
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'no shut',
        'int g0/3',
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'no shut',
        'int g1/0',
        'switchport mode access',
        'switchport access vlan 10',
        'switchport nonegotiate',
        'no shut',
        'int g1/1',
        'switchport mode access',
        'switchport access vlan 20',
        'switchport nonegotiate',
        'no shut',
        'int g1/2',
        'switchport mode access',
        'switchport access vlan 30',
        'switchport nonegotiate',
        'no shut',
        'int g1/3',
        'switchport mode access',
        'switchport access vlan 40',
        'switchport nonegotiate',
        'no shut',
        ]
output = net_connect.send_config_set(config_commands)
print(output)

# verification
# vlans
vlans = net_connect.send_command('show vlan brief')
print(vlans)
# trunk
trunks = net_connect.send_command('show int trunk')
print(trunks)
# save prompt
save_prompt = input('Save the running configuration? (y/n): ')
if save_prompt == 'y':
    net_connect.save_config()
elif save_prompt == 'n':
    print('exiting without saving...')
else:
    print('sorry, you need to make a decision.')
```
"floor3" script:
```
from netmiko import ConnectHandler
import getpass

password = getpass()
secret = getpass()

floor3 = {
        'device_type': 'cisco_ios',
        'host': '172.28.10.146',
        'username': 'kaleb',
        'password': password,
        'secret': secret,
        }
# connection handler
net_connect = ConnectHandler(**floor1)
# enable mode
net_connect.enable()
# configuration
config_commands = ['vlan 10', #vlan config
        'name accounting',
        'vlan 20',
        'name buisdev',
        'vlan 30',
        'name marketing',
        'vlan 40',
        'name manuf',
        'exit',
        'int g0/2', #trunk configuration
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'no shut',
        'int g0/3',
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'no shut',
        'int g1/0',
        'switchport mode access',
        'switchport access vlan 10',
        'switchport nonegotiate',
        'no shut',
        'int g1/1',
        'switchport mode access',
        'switchport access vlan 20',
        'switchport nonegotiate',
        'no shut',
        'int g1/2',
        'switchport mode access',
        'switchport access vlan 30',
        'switchport nonegotiate',
        'no shut',
        'int g1/3',
        'switchport mode access',
        'switchport access vlan 40',
        'switchport nonegotiate',
        'no shut',
        ]
output = net_connect.send_config_set(config_commands)
print(output)

# verification
# vlans
vlans = net_connect.send_command('show vlan brief')
print(vlans)
# trunk
trunks = net_connect.send_command('show int trunk')
print(trunks)
# save prompt
save_prompt = input('Save the running configuration? (y/n): ')
if save_prompt == 'y':
    net_connect.save_config()
elif save_prompt == 'n':
    print('exiting without saving...')
else:
    print('sorry, you need to make a decision.')
```

### core-sw* configurations
The following scripts configured core-sw1 and core-sw2.
I set up layer 3 VLANs/inter-vlan routing, layer 3 link aggregation, HSRP, and OSPF routing.
```
from netmiko import ConnectHandler
import getpass

password = getpass()
secret = getpass()

core_sw1 = {
        'device_type': 'cisco_ios',
        'host': '172.28.10.26',
        'username': 'kaleb',
        'password': password,
        'secret': secret,
        }
# connection handler
net_connect = ConnectHandler(**core_sw1)
# enable mode
net_connect.enable()
# configure
config_commands = ['ip routing',# enable ip routing
        'vlan 10', #vlan config
        'name accounting',
        'exit',
        'int vlan 10',
        'ip address 192.168.10.2 255.255.255.248',
        'desc default gateway for accounting vlan',
        'no shut',
        'exit',
        'vlan 20',
        'name buisdev',
        'exit',
        'int vlan 20',
        'ip address 192.168.20.2 255.255.255.224',
        'desc default gateway for business-dev vlan',
        'no shut',
        'exit',
        'vlan 30',
        'name marketing',
        'exit',
        'int vlan 30',
        'ip address 192.168.30.2 255.255.255.224',
        'desc default gateway for marketing vlan',
        'no shut',
        'exit',
        'vlan 40',
        'name manuf',
        'exit',
        'int vlan 40',
        'ip address 192.168.40.2 255.255.255.128',
        'desc default gateway for manufacturing vlan',
        'no shut',
        'exit',
        'int g0/2', # trunk port configs
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'no shut',
        'int g0/3',
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'int g1/0',
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'int range g1/1-2', #etherchannel configuration
        'no switchport',
        'channel-group 1 mode desirable',
        'exit',
        'int po1',
        'ip address 10.0.0.9 255.255.255.252',
        'desc aggregation link to core-sw2',
        'no shut',
        'exit',
        'int g0/1', # uplink to r1 config
        'no switchport',
        'ip address 10.0.0.2 255.255.255.252',
        'desc uplink to r1',
        'no shut',
        'router ospf 10', # ospf config
        'router-id 2.2.2.2',
        'network 10.0.0.0 0.0.0.3 area 0',
        'network 10.0.0.4 0.0.0.3 area 0',
        'network 10.0.0.8 0.0.0.3 area 0',
        'network 192.168.10.0 0.0.0.7 area 0',
        'network 192.168.20.0 0.0.0.31 area 0',
        'network 192.168.30.0 0.0.0.31 area 0',
        'network 192.168.40.0 0.0.0.127 area 0',
        'exit',
        'int po1',
        'ip ospf 10 area 0',
        'exit',
        'int g0/1',
        'ip ospf 10 area 0',
        'exit',
        'int vlan 10',
        'ip ospf 10 area 0',
        'int vlan 20',
        'ip ospf 10 area 0',
        'int vlan 30',
        'ip ospf 10 area 0',
        'int vlan 40',
        'ip ospf 10 area 0',
        'exit',
        'int vlan 10', #hsrp configuration
        'standby 1 ip 192.168.10.1',
        'standby 1 priority 105',
        'standby 1 preempt',
        'exit',
        'int vlan 20',
        'standby 1 ip 192.168.20.1',
        'standby 1 priority 105',
        'standby 1 preempt',
        'exit',
        'int vlan 30',
        'standby 1 ip 192.168.30.1',
        'standby 1 priority 105',
        'standby 1 preempt',
        'exit',
        'int vlan 40',
        'standby 1 ip 192.168.40.1',
        'standby 1 priority 105',
        'standby 1 preempt',
]
output = net_connect.send_config_set(config_commands)
print(output)

# verify
# ip link verification
ip_links = net_connect.send_command('show ip int b')
print(ip_links)
print()
# route verification
route_table = net_connect.send_command('show ip route')
print(route_table)
print()
# etherchannel verification
pagp_summary = net_connect.send_command('show etherchannel summary')
print(pagp_summary)
print()
port_channel = net_connect.send_command('show etherchannel port-channel')
print(port_channel)
print()
# ospf verification
ospf_interface = net_connect.send_command('show ip ospf interface')
print(ospf_interface)
print()
ospf_neighbor = net_connect.send_command('show ip ospf neighbor')
print(ospf_neighbor)
print()

# option to save config
save_prompt = input('Save the running configuration? (y/n): ')
if save_prompt == 'y':
    net_connect.save_config()
elif save_prompt == 'n':
    print('exiting without saving...')
else:
    print('sorry, you need to make a decision.')
```
```
from netmiko import ConnectHandler
import getpass

password = getpass()
secret = getpass()

core_sw2 = {
        'device_type': 'cisco_ios',
        'host': '172.28.10.28',
        'username': 'kaleb',
        'password': password,
        'secret': secret,
        }
# connection handler
net_connect = ConnectHandler(**core_sw1)
# enable mode
net_connect.enable()
# configure
config_commands = ['ip routing',# enable ip routing
        'vlan 10', #vlan config
        'name accounting',
        'exit',
        'int vlan 10',
        'ip address 192.168.10.3 255.255.255.248',
        'desc default gateway for accounting vlan',
        'no shut',
        'exit',
        'vlan 20',
        'name buisdev',
        'exit',
        'int vlan 20',
        'ip address 192.168.20.3 255.255.255.224',
        'desc default gateway for business-dev vlan',
        'no shut',
        'exit',
        'vlan 30',
        'name marketing',
        'exit',
        'int vlan 30',
        'ip address 192.168.30.3 255.255.255.224',
        'desc default gateway for marketing vlan',
        'no shut',
        'exit',
        'vlan 40',
        'name manuf',
        'exit',
        'int vlan 40',
        'ip address 192.168.40.3 255.255.255.128',
        'desc default gateway for manufacturing vlan',
        'no shut',
        'exit',
        'int g0/2', # trunk port configs
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'no shut',
        'int g0/3',
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'int g1/0',
        'switchport trunk encapsulation dot1q',
        'switchport mode trunk',
        'switchport trunk allowed vlan 10,20,30,40',
        'switchport trunk native vlan 99',
        'int range g1/1-2', #etherchannel configuration
        'no switchport',
        'channel-group 1 mode desirable',
        'exit',
        'int po1',
        'ip address 10.0.0.10 255.255.255.252',
        'desc aggregation link to core-sw2',
        'no shut',
        'exit',
        'int g0/1', # uplink to r1 config
        'no switchport',
        'ip address 10.0.0.6 255.255.255.252',
        'desc uplink to r1',
        'no shut',
        'router ospf 10', # ospf config
        'router-id 2.2.2.2',
        'network 10.0.0.0 0.0.0.3 area 0',
        'network 10.0.0.4 0.0.0.3 area 0',
        'network 10.0.0.8 0.0.0.3 area 0',
        'network 192.168.10.0 0.0.0.7 area 0',
        'network 192.168.20.0 0.0.0.31 area 0',
        'network 192.168.30.0 0.0.0.31 area 0',
        'network 192.168.40.0 0.0.0.127 area 0',
        'exit',
        'int po1',
        'ip ospf 10 area 0',
        'exit',
        'int g0/1',
        'ip ospf 10 area 0',
        'exit',
        'int vlan 10',
        'ip ospf 10 area 0',
        'int vlan 20',
        'ip ospf 10 area 0',
        'int vlan 30',
        'ip ospf 10 area 0',
        'int vlan 40',
        'ip ospf 10 area 0',
        'exit',
        'int vlan 10', #hsrp configuration
        'standby 1 ip 192.168.10.1',
        'standby 1 preempt',
        'exit',
        'int vlan 20',
        'standby 1 ip 192.168.20.1',
        'standby 1 preempt',
        'exit',
        'int vlan 30',
        'standby 1 ip 192.168.30.1',
        'standby 1 preempt',
        'exit',
        'int vlan 40',
        'standby 1 ip 192.168.40.1',
        'standby 1 preempt',
]
output = net_connect.send_config_set(config_commands)
print(output)

# verify
# ip link verification
ip_links = net_connect.send_command('show ip int b')
print(ip_links)
print()
# route verification
route_table = net_connect.send_command('show ip route')
print(route_table)
print()
# etherchannel verification
pagp_summary = net_connect.send_command('show etherchannel summary')
print(pagp_summary)
print()
port_channel = net_connect.send_command('show etherchannel port-channel')
print(port_channel)
print()
# ospf verification
ospf_interface = net_connect.send_command('show ip ospf interface')
print(ospf_interface)
print()
ospf_neighbor = net_connect.send_command('show ip ospf neighbor')
print(ospf_neighbor)
print()

# option to save config
save_prompt = input('Save the running configuration? (y/n): ')
if save_prompt == 'y':
    net_connect.save_config()
elif save_prompt == 'n':
    print('exiting without saving...')
else:
    print('sorry, you need to make a decision.')
```

### Edge router configuration
The following script configured the edge router.
I configured OSPF and interfaces with this script. 
(Note: mind the joke at the end, I was trying to be funny.)
```
from netmiko import ConnectHandler
import getpass

password = getpass()
secret = getpass()

r1 = {
        'device_type': 'cisco_ios',
        'host': '172.28.10.8',
        'username': 'kaleb',
        'password': password,
        'secret': secret,
        }
# connection handler
net_connect = ConnectHandler(**r1)
# enable mode
net_connect.enable()

# configuration
config_commands = [ 'int g0/1', # configuring int g0/1
        'ip address 10.0.0.1 255.255.255.252',
        'no shut',
        'desc link to core-sw1',
        'exit',
        'int g0/2', # configuring int g0/2
        'ip address 10.0.0.6 255.255.255.252',
        'no shut',
        'desc link to core-sw2',
        'exit',
        'router ospf 10', # ospf configuration setup
        'router-id 1.1.1.1',
        'network 10.0.0.0 0.0.0.3 area 0',
        'network 10.0.0.4 0.0.0.3 area 0',
        'network 10.0.0.8 0.0.0.3 area 0',
        'exit',
        'int g0/1', # apply ospf to interfaces
        'ip ospf 10 area 0',
        'int g0/2',
        'ip ospf 10 area 0',
        'exit', # applying default route and gateway config for internet access
        'ip default-gateway 172.28.10.1',
        'ip route 0.0.0.0 0.0.0.0 172.28.10.1',
        ]
output = net_connect.send_config_set(config_commands)
print(output)

# verify
ospf_config = net_connect.send_command('show ip ospf interface')
routes = net_connect.send_command('show ip route')

print(ospf_config)
print(routes)

save_config_opt = input('Save the configuration? (y/n)')
if save_config_opt == 'y':
    net_connect.save_config()
elif save_config_opt == 'n':
    print('ok')
else:
    print('you idiot, you didnt ever choose an option')
```

Thanks for checking out my project!
