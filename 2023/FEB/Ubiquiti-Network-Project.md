# Ubiquiti Unifi Home Network Project - February 24-26, 2023

## Technical Specifications
### Main Equipment
* 1x Unifi UDM Pro
* 1x Unifi USW 24-Port PoE
* 4x Unifi U6 Lite LAPs
### Internet
* 1x Xfinity XB7 Modem w/Bridge Mode Enabled
### Miscellaneous
* Dell PowerEdge R610
* Cisco Catalyst 2960G

## Network Topology
<img src="https://github.com/kmcgaugh/kmcguagh-network-projects/blob/main/images/NetworkDiagram.png?raw=true"></img>

## Ubiquiti Network Configuration
### Networks
* 1 - Home Network (172.28.10.0/24)
Main home network. PCs/Macs, iPhones, Tablets, Smart TVs connect to this network.

* 2 - IoT Network (172.28.20.0/28)
Reserved for smart home devices. I do not want any IoT devices on my home network, to avoid the ability to pivot between the networks, and to keep broadcast domains separate between my home devices and my IoT devices. Amazon echo devices, security cameras, thermostats, and smoke alarms, connect to this network. It is limited to 14 devices.

* 3 - Guest Network (192.168.240.0/29)
Reserved for visitors. Complete isolation from the rest of the internetwork. Limited to 6 devices only.

* 4 - Lab Bridge Network (10.1.30.0/29)
Used to provide internet connectivity to my Proxmox VE server and pfSense VM so they can get firmware updates. I use a /29 block here to accomodate for the UDM Pro's gateway interface, the Catalyst 2960G switch, the Proxmox server, and the pfSense WAN interface.

* 5 - Lab Network (192.168.1.0/24)
Provide network connections for the LAB VMs via the pfSense firewall. All Proxmox VMs sit on this network.

### WiFi
* Radio Settings
Wireless Meshing is enabled. Transmit power control is globally set, 2.4GHz @ 14 dBm, 5GHz @ 11 dBm. Channel width is globally set, 2.4GHz @ 20 MHz, 5GHz @ 40 MHz. Nightly channel optimization is turned on.

* SSIDs (real ones are not shown for privacy reasons)
Home - broadcasts 172.28.10.0/24 network
IoT - broadcasts 172.28.20.0/28 network
Guest - broadcasts 192.168.240.0/29 network. Bandwidth profile set to 2.5 Mbps up/1 Mbps down, to prevent visitors from oversaturating the rest of the network. Using guest profile to isolate this network from the rest of the house network.

### Security
* Threat Management
This feature is set to "detect and block". Any threats run through the UDM's IPS engine are actively blocked. Dark web blocker enabled.

* DNS
Using CloudFlare's Malware and Adult Content blocking DNS servers. Below are the IPs to those DNS servers for anyone wondering:
Primary = 1.1.1.3
Secondary = 1.0.0.3
(Might look into using OpenDNS in the future so I can have more granular control over content filtering.)

### WAN Connection
* xFI Modem
Enabled Bridge Mode. It is a necessity to enable this setting to prevent double-NAT. Double-NAT causes poor network performance. Bridge mode strips xFI from its routing function and turns it into an L2 modem bridge. UniFI gets a direct DHCP public IP from the ISP.

## Gallery
### 1. Server Rack
<img src="https://media.licdn.com/dms/image/C4E22AQF0363ukHLRPw/feedshare-shrink_1280/0/1677452385634?e=1680739200&v=beta&t=qpaiEnkGPKgvVlanhR2k1VjctwS-c-EnYtvTvfqqO_o" alt="server_rack"></img>
### 2. Bedroom AP #1 (my bedroom)
<img src="https://media.licdn.com/dms/image/C4E22AQH-Ol-IYGeqbQ/feedshare-shrink_1280/0/1677452382369?e=1680739200&v=beta&t=cQtwmOcuiATpPRkxF-E0Ai9De0fM1tJ13qW-oznuS2M" alt="bedroom_ap1"></img>
### 3. Office AP
<img src="https://media.licdn.com/dms/image/C4E22AQG6tr57TziKYw/feedshare-shrink_1280/0/1677452385434?e=1680739200&v=beta&t=2yOUq2tC_ZKYGt1v4-ABctXumF-cM5zq5-G_Pb-w0Cc" alt="office_ap"></img>
### 4. Foyer AP
<img src="https://media.licdn.com/dms/image/C4E22AQE-pjTZZl-XMA/feedshare-shrink_1280/0/1677452385417?e=1680739200&v=beta&t=ZnqO20R4FawLaxCR1nVfQj4NWY_ySZz1nie9OFVGQhM" alt="foyer_ap"></img>
### 5. Bedroom AP #2 (master bedroom)
<img src="https://media.licdn.com/dms/image/C4E22AQEWwIu3OEqn4w/feedshare-shrink_1280/0/1677452385314?e=1680739200&v=beta&t=Ehgm7xdfA3q9x75TX8mzdV7AXfp1_ZdSJGsO9tCmfFk" alt="bedroom_ap2"></img>
### 6. Cable Runs in Attic
<img src="https://raw.githubusercontent.com/kmcgaugh/kmcguagh-network-projects/main/images/IMG_9822.jpg"></img>
