# Campus OSPF Routing Lab - March 5, 2023
### Summary
I did an OSPF routing exercise using `eve-ng`. I built a very basic, 2-layer, campus network topology, consisting of 2 multilayer collapsed core switches, and 4 access layer switches. I implemented 4 VLANs, to simulate access networks for each floor of a building, and configured inter-VLAN routing via OSPF between each VLANs, through a layer 3 etherchannel aggregation link.
#### Equipment Used
* 2x Cisco Catalyst Multilayer vSwitches
* 4x Cisco Catalyst Layer 2 Switches
* 4x VPCs provided by EVE-NG

### Network Topology
<img src="https://github.com/kmcgaugh/kmcguagh-network-projects/blob/main/images/Screenshot%202023-03-05%20132014.png" alt="network topology"></img>

#### Subnets
* 10.15.100.0/30, backbone aggregation link
* 192.168.10.0/24, VLAN 10/1st floor
* 192.168.20.0/24, VLAN 20/2nd floor
* 192.168.30.0/24, VLAN 30/3rd floor
* 192.168.40.0/24, VLAN 40/4th floor

#### OSPF Areas
* Area 0 for backbone (industry standard)
* Area 1 for VLAN 10
* Area 2 for VLAN 20
* Area 3 for VLAN 30
* Area 4 for VLAN 40


### Device Configurations
#### core-SW1
```
!
hostname core-sw1
!
ip dhcp excluded-address 192.168.10.1
ip dhcp excluded-address 192.168.20.1
!
ip dhcp pool 1stfloor
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 1.1.1.1
 domain-name kmcgaugh.lab
!
ip dhcp pool 2ndfloor
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 domain-name kmcgaugh.lab
 dns-server 1.1.1.1
!
!
no ip domain-lookup
!
interface Port-channel1
 no switchport
 ip address 10.15.100.1 255.255.255.252
!
interface GigabitEthernet0/0
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 20
 switchport mode access
 switchport nonegotiate
 negotiation auto
!
interface GigabitEthernet0/2
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet0/3
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet1/0
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface Vlan10
 ip address 192.168.10.1 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.1 255.255.255.0
!
router ospf 10
 network 10.15.100.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 1
 network 192.168.20.0 0.0.0.255 area 2
!
```
#### core-SW2
```
!
hostname core-sw2
!
!
!
ip dhcp excluded-address 192.168.30.1
ip dhcp excluded-address 192.168.40.1
!
ip dhcp pool 3rdfloor
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 domain-name kmcgaugh.lab
 dns-server 1.1.1.1
!
ip dhcp pool 4thfloor
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.1
 domain-name kmcgaugh.lab
 dns-server 1.1.1.1
!
!
interface Port-channel1
 no switchport
 ip address 10.15.100.2 255.255.255.252
!
interface GigabitEthernet0/0
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 40
 switchport mode access
 switchport nonegotiate
 negotiation auto
!
interface GigabitEthernet0/2
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet0/3
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet1/0
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet1/1
 negotiation auto
!
interface GigabitEthernet1/2
 negotiation auto
!
interface GigabitEthernet1/3
 negotiation auto
!
interface Vlan30
 ip address 192.168.30.1 255.255.255.0
!
interface Vlan40
 ip address 192.168.40.1 255.255.255.0
!
router ospf 10
 network 10.15.100.0 0.0.0.3 area 0
 network 192.168.30.0 0.0.0.255 area 3
 network 192.168.40.0 0.0.0.255 area 4
!
```
#### access-sw1
```
!
hostname access-sw1
!
interface GigabitEthernet0/0
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
!
interface GigabitEthernet0/1
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
!
```
#### access-sw2
```
!
hostname access-sw2
!
!
!
interface GigabitEthernet0/0
 switchport access vlan 20
 switchport mode access
 switchport nonegotiate
!
interface GigabitEthernet0/1
 switchport access vlan 20
 switchport mode access
 switchport nonegotiate
!
```
#### access-sw3
```
!
hostname access-sw3
!
interface GigabitEthernet0/0
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 negotiation auto
! 
```
#### access-sw4
```
hostname access-sw4
!
interface GigabitEthernet0/0
 switchport access vlan 40
 switchport mode access
 switchport nonegotiate
 negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 40
 switchport mode access
 switchport nonegotiate
 negotiation auto
!
```
### Routing Tables
#### core-sw1
```
core-sw1#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.15.100.0/30 is directly connected, Port-channel1
L        10.15.100.1/32 is directly connected, Port-channel1
      192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.10.0/24 is directly connected, Vlan10
L        192.168.10.1/32 is directly connected, Vlan10
      192.168.20.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.20.0/24 is directly connected, Vlan20
L        192.168.20.1/32 is directly connected, Vlan20
O IA  192.168.30.0/24 [110/2] via 10.15.100.2, 00:40:34, Port-channel1
O IA  192.168.40.0/24 [110/2] via 10.15.100.2, 00:40:34, Port-channel1
```
#### core-sw2
```
core-sw2#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.15.100.0/30 is directly connected, Port-channel1
L        10.15.100.2/32 is directly connected, Port-channel1
O IA  192.168.10.0/24 [110/2] via 10.15.100.1, 00:41:09, Port-channel1
O IA  192.168.20.0/24 [110/2] via 10.15.100.1, 00:41:09, Port-channel1
      192.168.30.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.30.0/24 is directly connected, Vlan30
L        192.168.30.1/32 is directly connected, Vlan30
      192.168.40.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.40.0/24 is directly connected, Vlan40
L        192.168.40.1/32 is directly connected, Vlan40
```
### Connectivity Demo
VPC1 able to ping all other VPCs in the network.
<img src="https://github.com/kmcgaugh/kmcguagh-network-projects/blob/main/images/Screenshot%202023-03-05%20134319.png" alt="vpc1 ping"></img>
Traceroute capture from core-SW2 to VPC1.
<img src="https://github.com/kmcgaugh/kmcguagh-network-projects/blob/main/images/Screenshot%202023-03-05%20134541.png" alt="core-sw2 traceroute"></img>
