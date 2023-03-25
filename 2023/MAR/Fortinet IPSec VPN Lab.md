# Fortigate IPSec VPN Lab - March 25, 2023
As part of studying for the Fortinet NSE4, I decided to configure IPSec on two Fortinet Fortigate firewalls in the network lab. I created a point-to-point IPSec link over a simulated WAN. Please note: Yes, I'm aware that I'm using a weak PSK for IPSec; this is NOT a production environment; I would otherwise use a VERY STRONG PSK, or opt for certificate-based authentication, when building IPSec tunnels.

# Network Topology
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20161718.png?raw=true"></img>
This is a simple point-to-point IPSec VPN topology. The IPSec tunnel connects two Fortinet FortiGate firewalls. The IPSec tunnel provides connectivity between subnets 192.168.1.0/24 (Site A) and 192.168.2.0/24 (Site B). I am using a bridge back to my home network to simulate the internet WAN connection. 

The IPSec connection provides a secure site-to-site network connection over encapsulation protocol ESP, which encrypts layers 4 to 8 of the OSI model. See RFC #4303 for more details on ESP.


## Connectivity Demonstration

The below screenshots resemble PC1 (192.168.1.3), connected in Site A, and PC2 (192.168.2.2), connected in Site B, are able to ping/communicate with each other. 

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20162617.png?raw=true"></img>
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20162839.png?raw=true"></img>

The below screenshots resemble FG1 (in Site A), and FG2 (in Site B), are able to ping/communicate with each other on their local/remote subnet interfaces.

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20163122.png?raw=true"></img>
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20163324.png?raw=true"></img>

The below screenshots show that the IPSec tunnel is up on both Fortigates.
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20170201.png?raw=true"></img>
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20170248.png?raw=true"></img>

# Device Configurations
This section describes the configurations for the devices in the network topology. You can see the configurations in both the GUI and in YAML format. Note: I am showing my configurations in YAML format as I feel that's the best way to display them - I did not configure any devices using YAML.

## IPSec Tunnel on FG1 (Fortigate in Site A)
### Graphical Configuration

Phase 1 Details

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20165038.png?raw=true"></img>
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20165440.png?raw=true"></img>

Phase 2 Details

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20165632.png?raw=true"></img>
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20165803.png?raw=true"></img> 
Address Object for RemoteToFG2_local

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20165826.png?raw=true"></img> 
Address Object for RemoteToFG2_remote

### Textual Configuration (YAML format)
```
IPSec Tunnel:
    Name: RemoteToFG2
    Phase 1:
        Remote IP: 172.28.10.39
        Outoing Interface: port2
        Dead Peer Detection (DPD): On Demand
        DPD retry count: 3
        DPD retry interval: 20
        Authentication:
            Method: pre-shared key
            PSK: "welcome625"
    Phase 2:
        Selectors:
            Local Address:
                Address object: RemoteToFG2_local
                Subnet: 192.168.1.0/24
            Remote Address:
                Address object: RemoteToFG2_remote
                Subnet: 192.168.2.0/24
```
## FG1 Firewall Traffic Rules
The following rules were created when the IPSec tunnel was created, to allow traffic between the local and remote subnets.

### Allow Traffic from Site A to Site B
`vpn_RemoteToFG2_local_0` allows traffic incoming from the LAN, via port1, to go to Site B (192.168.2.0/24), via the tunnel interface.
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20170733.png?raw=true"></img>

### Allow Traffic from Site B to Site A
`vpn_RemoteToFG2_remote_0` allows traffic from Site B, via RemoteToFG2 (the actual tunnel interface), into the LAN, via port1.
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20170758.png?raw=true"></img>

## IPSec Tunnel on FG2 (Fortigate in Site B)
### Graphical Configuration

Phase 1 Details
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20171430.png?raw=true"></img>
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20171457.png?raw=true"></img>

Phase 2 Details

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20171543.png?raw=true"></img>

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20171629.png?raw=true"></img> 
Address Object for RemoteToFG1_local

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20171651.png?raw=true"></img> 
Address Object for RemoteToFG1_remote

### Textual Configuration
```
IPSec Tunnel:
    Name: RemoteToFG1
    Phase 1:
        Remote IP: 172.28.10.9
        Outoing Interface: port2
        Dead Peer Detection (DPD): On Demand
        DPD retry count: 3
        DPD retry interval: 20
        Authentication:
            Method: pre-shared key
            PSK: "welcome625"
    Phase 2:
        Selectors:
            Local Address:
                Address object: RemoteToFG1_local
                Subnet: 192.168.2.0/24
            Remote Address:
                Address object: RemoteToFG1_remote
                Subnet: 192.168.1.0/24
```
## FG2 Firewall Traffic Rules
The following rules were created when the IPSec tunnel was created, to allow traffic between the local and remote subnets.

### Allow Traffic from Site B to Site A
`vpn_RemoteToFG1_local_0` allows traffic incoming from the LAN, via port1, to go to Site A (192.168.1.0/24), via the tunnel interface.
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20172229.png?raw=true"></img>

### Allow Traffic from Site A to Site B
`vpn_RemoteToFG1_remote_0` allows traffic from Site A, via RemoteToFG1 (the actual tunnel interface), into the LAN, via port1.
<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20172320.png?raw=true"></img>


# Wireshark Capture
Here's some final evidence that the IPSec tunnel is working. The below screenshot shows a Wireshark capture. As you can see, I have ESP packets traversing the WAN, from 172.28.10.9 (FG in Site A), and 172.28.10.39 (FG in Site B), and I cannot read layers 4 to 7 as it's encrypted in the ESP payload.

<img src="https://github.com/kmcgaugh/kmohr-network-projects/blob/main/images/Screenshot%202023-03-25%20172750.png?raw=true"></img>

# Questions/Comments/Inquiries
If you have any questions, comments, or inquiries, please contact me directly at kaleb.mohr1@gmail.com - please allow time for me to read/respond to your email. Thank you.
