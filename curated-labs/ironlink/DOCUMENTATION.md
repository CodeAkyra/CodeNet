# Topologys
This repository contains my personal documentation and step-by-step explanations for the pre-configured labs provided by **Ironlink Computer Learning Center**. 

The goal of this project is to document my own approach to solving these technical challenges, focusing on the most efficient methods and providing clear reasoning for every configuration choice.

> [!IMPORTANT]  
> **Note on Access:** These Packet Tracer labs are exclusive to Ironlink alumni. While I cannot distribute the original lab files, my specific **device configurations and solution methodologies** are fully documented here for reference.

___
# NAT DHCP SSH Sim
![1](imgs/NAT_DHCP_SSH_Sim.png)

___
# Named Access List DHCP Snooping Sim 3
![2](imgs/Named_Access_list_DHCP_Snooping_Sim_3.png)

<details>
<summary>Click to view configuration and explanation</summary>
  
### I added a server to act as an HTTP host so I could verify the ACL rules.
![2.1](imgs/NALDHCPSS3_HTTP_Server.png)

### Task 1: Configure R1
```
R1>en
R1#conf t
R1(config)#ip access-list extended WWW_ACL
R1(config-ext-nacl)#10 deny tcp host 10.101.1.2 any eq telnet
R1(config-ext-nacl)#20 permit tcp 10.101.1.0 0.0.0.255 any eq www
R1(config-ext-nacl)#25 deny tcp any any eq www
R1(config-ext-nacl)#30 permit ip any any
```
To meet the requirements, I configured the WWW_ACL using a top-down logical approach. I first placed a specific deny for PC1's Telnet traffic at the top to ensure it is dropped immediately, followed by an explicit permit for the VLAN 202 subnet to access HTTP (port 80). I then added a general deny for all other HTTP traffic to strictly enforce that "only" VLAN 202 can use that port, and finally, I included a permit ip any any statement to act as a catch-all, ensuring all other non-restricted traffic—like Pings and DNS—is allowed to pass for the rest of the network.
![2.2](imgs/NALDHCPSS3_result.png)

### Task 2: Configure SW2
```
SW2>en
SW2#conf t
SW2(config)#username AdminGroup privilege 1 algorithm-type scrypt secret BumBL3d
SW2(config)#line vty 0 4
SW2(config)#login local
SW2(config)#transport input telnet
```
On SW2, I created a local user named AdminGroup with Privilege Level 1 to provide standard Exec mode access, using the Scrypt algorithm (Type 9) to securely hash the password BumBL3d. I then applied this to virtual ports 0–4 by enabling local authentication and restricting the lines to Telnet access only via the ```transport input telnet``` command. This ensures that only the local database is used for logins and that more secure or alternative protocols like SSH are explicitly disabled on these ports.

### Task 3: Configure SW3
```
SW3>en
SW3#conf t
SW3(config)#ip dhcp snooping vlan 102,202
SW3(config)#ip dhcp snooping verify mac-address
```
To enhance Layer 2 security on SW3, I enabled DHCP Snooping for VLANs 102 and 202 and activated MAC address verification to ensure the source MAC address in DHCP requests matches the client's hardware address. This configuration builds a DHCP binding database that prevents DHCP spoofing attacks by ensuring users only receive IP addresses from legitimate sources while dropping packets from unauthorized or rogue DHCP servers.

</details>

___
# PAT NTP DHCP Relay
![3](imgs/PAT_NTP_DHCP_Relay.png)

___
# Static Route Sim 1
![4](imgs/Static_Route_Sim_1.png)

___
# Static Route Sim 2
![5](imgs/Static_Route_Sim_2.png)

___
# Static Route Sim 3
![6](imgs/Static_Route_Sim_3.png)

___
# Static Route Sim 4
![7](imgs/Static_Route_Sim_4.png)

___
# VLAN CDP LLDP Sim
![8](imgs/VLAN_CDP_LLDP_Sim.png)

___
# VLAN CDP Sim
![9](imgs/VLAN_CDP_Sim.png)

___
# VLAN CDP Sim 2
![10](imgs/VLAN_CDP_Sim_2.png)

___
# VLAN LLDP Sim
![11](imgs/VLAN_LLDP_Sim.png)

___
# VLAN and Trunking Sim
![12](imgs/VLAN_and_Trunking_Sim.png)

