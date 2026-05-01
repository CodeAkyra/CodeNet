<!-- I'll update this file with the configuration and maybe explanation(not sure maybe not) as soon as possible. Thank you. -->

# Configuration
## PC0, PC1, PC2
Set the IP configuration to dynamic to automatically receive an IP address from the DHCP server.

## Switch0, Switch1
No configuration was needed for the switches. I just used them to connect my end devices to the routers.

## R1
```
Router>en
Router#conf t
Router(config)#hostname R1
R1(config)#ip dhcp pool BUILDING_A
R1(dhcp-config)#network 192.168.1.0 255.255.255.0
R1(dhcp-config)#default-router 192.168.1.1
R1(dhcp-config)#exit
R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.100
R1(config)#int l0
R1(config-if)#ip add 1.1.1.1 255.255.255.255
R1(config-if)#int g0/0
R1(config-if)#ip add 192.168.1.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#int g0/1
R1(config-if)#ip add 10.0.0.1 255.255.255.252
R1(config-if)#no shut
R1(config-if)#exit
R1(config)#router ospf 1
R1(config-router)#router-id 1.1.1.1
R1(config-router)#passive-interface l0
R1(config-router)#network 1.1.1.1 0.0.0.0 area 0
R1(config-router)#network 10.0.0.0 0.0.0.3 area 0
R1(config-router)#network 192.168.1.0 0.0.0.255 area 0
R1(config-router)#exit
R1(config)#do wr
```
## R2
```
Router>en
Router#conf t
Router(config)#hostname R2
R2(config)#int l0
R2(config-if)#ip add 2.2.2.2 255.255.255.255
R2(config-if)#int g0/0
R2(config-if)#ip add 10.0.0.2 255.255.255.252
R2(config-if)#no shut
R2(config-if)#int g0/1
R2(config-if)#ip add 20.0.0.1 255.255.255.252
R2(config-if)#no shut
R2(config-if)#exit
R2(config)#router ospf 1
R2(config-router)#router-id 2.2.2.2
R2(config-router)#passive-interface l0
R2(config-router)#network 2.2.2.2 0.0.0.0 area 0
R2(config-router)#network 10.0.0.0 0.0.0.3 area 0
R2(config-router)#network 20.0.0.0 0.0.0.3 area 0
R2(config-router)#exit
R2(config)#ip access-list extended GATE
R2(config-ext-nacl)#deny icmp host 192.168.1.101 any
R2(config-ext-nacl)#permit ip any any
R2(config-ext-nacl)#exit
R2(config)#int g0/0
R2(config-if)#ip access-group GATE in
R2(config-if)#exit
R2(config)#do wr
```
## R3
```
Router>en
Router#conf t
Router(config)#hostname R3
R3(config)#ip dhcp pool BUILDING_B
R3(dhcp-config)#network 192.168.2.0 255.255.255.0
R3(dhcp-config)#default-router 192.168.2.1
R3(dhcp-config)#exit
R3(config)#ip dhcp excluded-address 192.168.2.1 192.168.2.100
R3(config)#int l0
R3(config-if)#ip add 3.3.3.3 255.255.255.255
R3(config-if)#int g0/0
R3(config-if)#ip add 192.168.2.1 255.255.255.0
R3(config-if)#no shut
R3(config-if)#int g0/1
R3(config-if)#ip add 20.0.0.2 255.255.255.252
R3(config-if)#no shut
R3(config-if)#exit
R3(config)#router ospf 1
R3(config-router)#router-id 3.3.3.3
R3(config-router)#passive-interface l0
R3(config-router)#network 3.3.3.3 0.0.0.0 area 0
R3(config-router)#network 20.0.0.0 0.0.0.3 area 0
R3(config-router)#network 192.168.2.0 0.0.0.255 area 0
R3(config-router)#exit
R3(config)#do wr
```
