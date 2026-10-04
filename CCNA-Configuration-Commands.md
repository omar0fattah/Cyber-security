# CCNA Configuration Commands

A practical **Cisco IOS / Cisco Packet Tracer command reference** focused on CCNA-level networking, configuration, verification, and troubleshooting.

> **Scope:** CCNA 200-301 v1.1 and closely related Cisco IOS commands useful for labs and networking practice.  
> **Platform:** Cisco IOS / Cisco Packet Tracer  
> **Note:** Command availability may vary between Cisco IOS versions, device models, and Packet Tracer.

---

## Table of Contents

1. [Cisco IOS CLI Basics](#1-cisco-ios-cli-basics)
2. [Basic Device Configuration](#2-basic-device-configuration)
3. [Interface Configuration](#3-interface-configuration)
4. [IPv4 Configuration](#4-ipv4-configuration)
5. [IPv6 Configuration](#5-ipv6-configuration)
6. [VLANs](#6-vlans)
7. [802.1Q Trunking](#7-8021q-trunking)
8. [Inter-VLAN Routing](#8-inter-vlan-routing)
9. [CDP and LLDP](#9-cdp-and-lldp)
10. [EtherChannel and LACP](#10-etherchannel-and-lacp)
11. [STP and Rapid PVST+](#11-stp-and-rapid-pvst)
12. [Static Routing](#12-static-routing)
13. [OSPFv2](#13-ospfv2)
14. [DHCP](#14-dhcp)
15. [NAT and PAT](#15-nat-and-pat)
16. [Access Control Lists](#16-access-control-lists)
17. [Port Security](#17-port-security)
18. [DHCP Snooping](#18-dhcp-snooping)
19. [Dynamic ARP Inspection](#19-dynamic-arp-inspection)
20. [IPv6 RA Guard](#20-ipv6-ra-guard)
21. [SSH](#21-ssh)
22. [AAA and Authentication](#22-aaa-and-authentication)
23. [NTP](#23-ntp)
24. [DNS](#24-dns)
25. [Syslog](#25-syslog)
26. [SNMP](#26-snmp)
27. [TFTP and Configuration Transfer](#27-tftp-and-configuration-transfer)
28. [Configuration Management](#28-configuration-management)
29. [Verification Commands](#29-verification-commands)
30. [Troubleshooting](#30-troubleshooting)
31. [Common Configuration Workflows](#31-common-configuration-workflows)

---

# 1. Cisco IOS CLI Basics

## Enter Privileged EXEC Mode

```text
enable
```

## Enter Global Configuration Mode

```text
configure terminal
```

## Exit the Current Configuration Mode

```text
exit
```

## Return Directly to Privileged EXEC Mode

```text
end
```

or:

```text
Ctrl+Z
```

## Save the Running Configuration

```text
copy running-config startup-config
```

Alternative:

```text
write memory
```

## Display the Running Configuration

```text
show running-config
```

## Display the Startup Configuration

```text
show startup-config
```

## Display IOS and Hardware Information

```text
show version
```

## Display Available Commands

```text
?
```

## Display Available Commands at a Specific Level

```text
show ?
```

## Command Completion

Press:

```text
TAB
```

## Repeat Previous Commands

Use:

```text
Up Arrow
```

## Display Command History

```text
show history
```

## Cancel a Command

```text
Ctrl+C
```

---

# 2. Basic Device Configuration

## Set the Hostname

```text
configure terminal
hostname R1
```

Example:

```text
R1(config)#
```

## Configure the Privileged EXEC Password

```text
enable secret MyPassword
```

## Configure the Console Password

```text
line console 0
password MyPassword
login
exit
```

## Configure VTY Password Authentication

```text
line vty 0 4
password MyPassword
login
exit
```

## Configure a Local User

```text
username admin privilege 15 secret MyPassword
```

## Disable DNS Lookup

Useful when an incorrectly typed command should not be interpreted as a hostname.

```text
no ip domain lookup
```

## Configure a Domain Name

```text
ip domain name example.local
```

## Configure a Message-of-the-Day Banner

```text
banner motd #Unauthorized access prohibited#
```

## Configure an Interface Description

```text
interface gigabitEthernet 0/0
description Connection_to_R2
```

---

# 3. Interface Configuration

## Enter an Interface

```text
interface gigabitEthernet 0/0
```

Other examples:

```text
interface fastEthernet 0/1
```

```text
interface serial 0/0/0
```

## Enable an Interface

```text
no shutdown
```

## Disable an Interface

```text
shutdown
```

## Configure a Description

```text
description Connection_to_Switch1
```

## Configure Multiple Interfaces

```text
interface range gigabitEthernet 0/1-4
```

or:

```text
interface range fastEthernet 0/1-10
```

## Return an Interface to Its Default Configuration

```text
default interface gigabitEthernet 0/1
```

## Verify Interfaces

```text
show ip interface brief
```

```text
show interfaces
```

```text
show interfaces gigabitEthernet 0/0
```

## Display an Interface's Configuration

```text
show running-config interface gigabitEthernet 0/0
```

---

# 4. IPv4 Configuration

## Configure an IPv4 Address

```text
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```

## Configure a Secondary IPv4 Address

```text
ip address 192.168.2.1 255.255.255.0 secondary
```

## Remove an IPv4 Address

```text
no ip address
```

## Configure a Default Gateway on a Layer 2 Switch

```text
ip default-gateway 192.168.1.1
```

## Verify IPv4 Addressing

```text
show ip interface brief
```

```text
show ip interface
```

## Test IPv4 Connectivity

```text
ping 192.168.1.1
```

## Specify a Source Interface for a Ping

```text
ping 192.168.1.1 source gigabitEthernet 0/0
```

---

# 5. IPv6 Configuration

## Enable IPv6 Unicast Routing

```text
ipv6 unicast-routing
```

## Configure a Global Unicast Address

```text
interface gigabitEthernet 0/0
ipv6 address 2001:DB8:1::1/64
no shutdown
```

## Configure a Link-Local Address

```text
ipv6 address FE80::1 link-local
```

## Configure an Address Using EUI-64

```text
ipv6 address 2001:DB8:1::/64 eui-64
```

## Remove an IPv6 Address

```text
no ipv6 address 2001:DB8:1::1/64
```

## Verify IPv6 Interfaces

```text
show ipv6 interface brief
```

```text
show ipv6 interface
```

## Test IPv6 Connectivity

```text
ping 2001:DB8:1::2
```

## Display the IPv6 Neighbor Table

```text
show ipv6 neighbors
```

---

# 6. VLANs

## Create a VLAN

```text
vlan 10
name SALES
exit
```

## Configure an Access Port

```text
interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 10
```

## Configure Multiple Access Ports

```text
interface range gigabitEthernet 0/1-10
switchport mode access
switchport access vlan 10
```

## Return a Port to VLAN 1

```text
interface gigabitEthernet 0/1
switchport access vlan 1
```

## Delete a VLAN

```text
no vlan 10
```

## Verify VLANs

```text
show vlan brief
```

```text
show vlan
```

## Display a Specific VLAN

```text
show vlan id 10
```

## Display Switchport Information

```text
show interfaces gigabitEthernet 0/1 switchport
```

---

# 7. 802.1Q Trunking

## Configure a Trunk

```text
interface gigabitEthernet 0/1
switchport mode trunk
```

## Configure the Native VLAN

```text
switchport trunk native vlan 99
```

## Configure Allowed VLANs

```text
switchport trunk allowed vlan 10,20,30
```

## Add VLANs to the Allowed List

```text
switchport trunk allowed vlan add 40
```

## Remove VLANs from the Allowed List

```text
switchport trunk allowed vlan remove 40
```

## Allow All VLANs

```text
switchport trunk allowed vlan all
```

## Verify Trunks

```text
show interfaces trunk
```

```text
show interfaces gigabitEthernet 0/1 trunk
```

```text
show interfaces gigabitEthernet 0/1 switchport
```

---

# 8. Inter-VLAN Routing

## Router-on-a-Stick

Configure the physical interface:

```text
interface gigabitEthernet 0/0
no shutdown
```

Configure a VLAN subinterface:

```text
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

Another VLAN:

```text
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

## Native VLAN Subinterface

```text
interface gigabitEthernet 0/0.99
encapsulation dot1Q 99 native
ip address 192.168.99.1 255.255.255.0
```

## Configure an SVI

```text
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown
```

## Enable Layer 3 Routing on a Multilayer Switch

```text
ip routing
```

## Configure a Layer 3 Physical Interface

```text
interface gigabitEthernet 0/1
no switchport
ip address 10.0.0.1 255.255.255.252
no shutdown
```

## Configure IPv6 on a Layer 3 Interface

```text
interface gigabitEthernet 0/1
no switchport
ipv6 address 2001:DB8:1::1/64
no shutdown
```

---

# 9. CDP and LLDP

## Enable CDP Globally

```text
cdp run
```

## Disable CDP Globally

```text
no cdp run
```

## Enable CDP on an Interface

```text
interface gigabitEthernet 0/1
cdp enable
```

## Disable CDP on an Interface

```text
interface gigabitEthernet 0/1
no cdp enable
```

## Verify CDP

```text
show cdp neighbors
```

```text
show cdp neighbors detail
```

```text
show cdp interface
```

## Enable LLDP

```text
lldp run
```

## Configure LLDP Transmit and Receive

```text
interface gigabitEthernet 0/1
lldp transmit
lldp receive
```

## Verify LLDP

```text
show lldp
```

```text
show lldp neighbors
```

```text
show lldp neighbors detail
```

---

# 10. EtherChannel and LACP

## Configure LACP

```text
interface range gigabitEthernet 0/1-2
channel-group 1 mode active
```

## Configure a Port-Channel as a Trunk

```text
interface port-channel 1
switchport mode trunk
```

## Configure an Access EtherChannel

```text
interface range gigabitEthernet 0/1-2
switchport mode access
switchport access vlan 10
channel-group 1 mode active
```

## Configure Allowed VLANs on a Port-Channel

```text
interface port-channel 1
switchport trunk allowed vlan 10,20,30
```

## Verify EtherChannel

```text
show etherchannel summary
```

```text
show etherchannel detail
```

```text
show etherchannel port-channel
```

```text
show interfaces port-channel 1
```

## Verify LACP Neighbors

```text
show lacp neighbor
```

---

# 11. STP and Rapid PVST+

## Enable Rapid PVST+

```text
spanning-tree mode rapid-pvst
```

## Configure a Switch as Root Primary

```text
spanning-tree vlan 10 root primary
```

## Configure a Switch as Root Secondary

```text
spanning-tree vlan 10 root secondary
```

## Manually Configure STP Priority

Lower values have a higher chance of becoming the root bridge.

```text
spanning-tree vlan 10 priority 4096
```

## Configure Multiple VLAN Priorities

```text
spanning-tree vlan 10,20 priority 4096
```

## Configure PortFast

```text
interface gigabitEthernet 0/1
spanning-tree portfast
```

## Configure BPDU Guard

```text
interface gigabitEthernet 0/1
spanning-tree bpduguard enable
```

## Configure Root Guard

```text
interface gigabitEthernet 0/1
spanning-tree guard root
```

## Configure Loop Guard

```text
interface gigabitEthernet 0/1
spanning-tree guard loop
```

## Verify STP

```text
show spanning-tree
```

```text
show spanning-tree vlan 10
```

```text
show spanning-tree root
```

```text
show spanning-tree summary
```

---

# 12. Static Routing

## IPv4 Network Route

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## IPv4 Route Using an Exit Interface

```text
ip route 192.168.20.0 255.255.255.0 gigabitEthernet 0/1
```

## IPv4 Route Using Next Hop and Exit Interface

```text
ip route 192.168.20.0 255.255.255.0 gigabitEthernet 0/1 10.0.0.2
```

## IPv4 Default Route

```text
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

## IPv4 Host Route

```text
ip route 192.168.20.10 255.255.255.255 10.0.0.2
```

## Floating Static Route

The final value is the administrative distance.

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2 200
```

## Remove a Static Route

```text
no ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## IPv6 Static Route

```text
ipv6 route 2001:DB8:20::/64 2001:DB8:1::2
```

## IPv6 Route Using an Exit Interface

```text
ipv6 route 2001:DB8:20::/64 gigabitEthernet 0/0
```

## IPv6 Default Route

```text
ipv6 route ::/0 2001:DB8:1::2
```

## IPv6 Host Route

```text
ipv6 route 2001:DB8:20::10/128 2001:DB8:1::2
```

## IPv6 Floating Static Route

```text
ipv6 route 2001:DB8:20::/64 2001:DB8:1::2 200
```

## Verify IPv4 Routing

```text
show ip route
```

```text
show ip route static
```

## Verify IPv6 Routing

```text
show ipv6 route
```

```text
show ipv6 route static
```

---

# 13. OSPFv2

## Start OSPF

```text
router ospf 1
```

## Configure a Router ID

```text
router ospf 1
router-id 1.1.1.1
```

## Advertise a Network

```text
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
```

## Configure OSPF Directly on an Interface

```text
interface gigabitEthernet 0/0
ip ospf 1 area 0
```

## Configure a Passive Interface

```text
router ospf 1
passive-interface gigabitEthernet 0/1
```

## Make All Interfaces Passive

```text
router ospf 1
passive-interface default
```

Then allow OSPF on a specific interface:

```text
no passive-interface gigabitEthernet 0/0
```

## Configure OSPF Interface Cost

```text
interface gigabitEthernet 0/0
ip ospf 1 area 0
ip ospf cost 10
```

## Configure OSPF Interface Priority

```text
interface gigabitEthernet 0/0
ip ospf priority 100
```

## Advertise a Default Route

```text
router ospf 1
default-information originate
```

## Restart the OSPF Process

```text
clear ip ospf process
```

## Verify OSPF

```text
show ip protocols
```

```text
show ip ospf
```

```text
show ip ospf interface
```

```text
show ip ospf interface gigabitEthernet 0/0
```

```text
show ip ospf neighbor
```

```text
show ip route ospf
```

```text
show ip route
```

---

# 14. DHCP

## Configure an Interface as a DHCP Client

```text
interface gigabitEthernet 0/0
ip address dhcp
no shutdown
```

## Configure a DHCP Relay

```text
interface gigabitEthernet 0/0
ip helper-address 192.168.1.10
```

## Remove a DHCP Relay

```text
no ip helper-address 192.168.1.10
```

## Configure a Cisco IOS DHCP Server

Exclude addresses that should not be assigned:

```text
ip dhcp excluded-address 192.168.10.1 192.168.10.20
```

Create a DHCP pool:

```text
ip dhcp pool LAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
domain-name example.local
```

## Configure a DHCP Lease

```text
ip dhcp pool LAN10
lease 7
```

## Verify DHCP

```text
show ip dhcp pool
```

```text
show ip dhcp binding
```

```text
show ip dhcp conflict
```

```text
show ip dhcp server statistics
```

---

# 15. NAT and PAT

## Configure an Inside Interface

```text
interface gigabitEthernet 0/0
ip nat inside
```

## Configure an Outside Interface

```text
interface gigabitEthernet 0/1
ip nat outside
```

## Static NAT

```text
ip nat inside source static 192.168.1.10 203.0.113.10
```

## Dynamic NAT

Create an ACL identifying inside addresses:

```text
access-list 1 permit 192.168.1.0 0.0.0.255
```

Create a public address pool:

```text
ip nat pool PUBLIC 203.0.113.10 203.0.113.20 netmask 255.255.255.0
```

Connect the ACL to the pool:

```text
ip nat inside source list 1 pool PUBLIC
```

## PAT Using an Address Pool

```text
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat pool PUBLIC 203.0.113.10 203.0.113.20 netmask 255.255.255.0
ip nat inside source list 1 pool PUBLIC overload
```

## PAT Using the Outside Interface

```text
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```

## Verify NAT

```text
show ip nat translations
```

```text
show ip nat statistics
```

## Clear NAT Translations

```text
clear ip nat translation *
```

---

# 16. Access Control Lists

## Standard Numbered ACL

```text
access-list 10 permit 192.168.1.0 0.0.0.255
```

## Permit a Specific Host

```text
access-list 10 permit host 192.168.1.10
```

Equivalent wildcard syntax:

```text
access-list 10 permit 192.168.1.10 0.0.0.0
```

## Deny a Network

```text
access-list 10 deny 192.168.1.0 0.0.0.255
```

## Permit Everything

```text
access-list 10 permit any
```

## Deny Everything

```text
access-list 10 deny any
```

> IPv4 ACLs have an implicit deny at the end when no matching permit statement is reached.

## Apply a Standard ACL

```text
interface gigabitEthernet 0/0
ip access-group 10 in
```

## Remove an ACL from an Interface

```text
interface gigabitEthernet 0/0
no ip access-group 10 in
```

## Named Standard ACL

```text
ip access-list standard BLOCK-LAN
deny 192.168.1.0 0.0.0.255
permit any
exit
```

## Extended Numbered ACL

Permit all IP traffic from a network:

```text
access-list 100 permit ip 192.168.1.0 0.0.0.255 any
```

Permit TCP:

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any
```

Permit HTTP:

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
```

Permit HTTPS:

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
```

Permit SSH:

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 22
```

Permit DNS:

```text
access-list 100 permit udp 192.168.1.0 0.0.0.255 any eq 53
```

Permit ICMP:

```text
access-list 100 permit icmp 192.168.1.0 0.0.0.255 any
```

Deny TCP:

```text
access-list 100 deny tcp 192.168.1.0 0.0.0.255 any
```

Permit remaining traffic:

```text
access-list 100 permit ip any any
```

## Apply an Extended ACL

```text
interface gigabitEthernet 0/0
ip access-group 100 in
```

## Named Extended ACL

```text
ip access-list extended WEB-ACCESS
permit tcp 192.168.1.0 0.0.0.255 any eq 80
permit tcp 192.168.1.0 0.0.0.255 any eq 443
deny ip 192.168.1.0 0.0.0.255 any
permit ip any any
exit
```

## Apply a Named Extended ACL

```text
interface gigabitEthernet 0/0
ip access-group WEB-ACCESS in
```

## Delete a Numbered ACL

```text
no access-list 10
```

## Delete a Named ACL

```text
no ip access-list extended WEB-ACCESS
```

## IPv6 ACL

Create the ACL:

```text
ipv6 access-list V6-FILTER
```

Permit IPv6 traffic:

```text
permit ipv6 2001:DB8:1::/64 any
```

Permit ICMPv6:

```text
permit icmp any any
```

Deny IPv6 traffic:

```text
deny ipv6 any any
```

Apply the ACL:

```text
interface gigabitEthernet 0/0
ipv6 traffic-filter V6-FILTER in
```

Remove the ACL:

```text
interface gigabitEthernet 0/0
no ipv6 traffic-filter V6-FILTER in
```

## Verify ACLs

```text
show access-lists
```

```text
show ip access-lists
```

```text
show ipv6 access-list
```

---

# 17. Port Security

## Enable Port Security

```text
interface gigabitEthernet 0/1
switchport mode access
switchport port-security
```

## Configure Maximum MAC Addresses

```text
switchport port-security maximum 2
```

## Configure a Static Secure MAC Address

```text
switchport port-security mac-address 0011.2233.4455
```

## Enable Sticky MAC Addresses

```text
switchport port-security mac-address sticky
```

## Configure Violation Mode

Shutdown:

```text
switchport port-security violation shutdown
```

Restrict:

```text
switchport port-security violation restrict
```

Protect:

```text
switchport port-security violation protect
```

## Verify Port Security

```text
show port-security
```

```text
show port-security interface gigabitEthernet 0/1
```

```text
show port-security address
```

## Recover a Shutdown Port

```text
interface gigabitEthernet 0/1
shutdown
no shutdown
```

---

# 18. DHCP Snooping

## Enable DHCP Snooping

```text
ip dhcp snooping
```

## Enable DHCP Snooping for a VLAN

```text
ip dhcp snooping vlan 10
```

## Trust a DHCP Server-Facing Interface

```text
interface gigabitEthernet 0/24
ip dhcp snooping trust
```

## Rate-Limit DHCP Packets

```text
interface gigabitEthernet 0/1
ip dhcp snooping limit rate 10
```

## Verify DHCP Snooping

```text
show ip dhcp snooping
```

```text
show ip dhcp snooping binding
```

---

# 19. Dynamic ARP Inspection

## Enable DAI for a VLAN

```text
ip arp inspection vlan 10
```

## Trust an Interface

```text
interface gigabitEthernet 0/24
ip arp inspection trust
```

## Rate-Limit ARP Packets

```text
interface gigabitEthernet 0/1
ip arp inspection limit rate 15
```

## Verify DAI

```text
show ip arp inspection
```

```text
show ip arp inspection vlan 10
```

```text
show ip arp inspection interfaces
```

---

# 20. IPv6 RA Guard

## Enable RA Guard

```text
interface gigabitEthernet 0/1
ipv6 nd raguard
```

## Remove RA Guard

```text
interface gigabitEthernet 0/1
no ipv6 nd raguard
```

---

# 21. SSH

## Configure a Domain Name

```text
ip domain name example.local
```

## Create a Local User

```text
username admin privilege 15 secret MyPassword
```

## Generate RSA Keys

```text
crypto key generate rsa
```

When prompted for the modulus size, a typical lab configuration is:

```text
2048
```

## Enable SSH Version 2

```text
ip ssh version 2
```

## Configure VTY Lines for Local Authentication

```text
line vty 0 4
login local
transport input ssh
exit
```

## Configure SSH Timeout

```text
ip ssh time-out 60
```

## Configure SSH Authentication Retries

```text
ip ssh authentication-retries 3
```

## Verify SSH

```text
show ip ssh
```

```text
show users
```

```text
show running-config
```

## Connect to an SSH Device

From a Cisco IOS device:

```text
ssh -l admin 192.168.1.1
```

---

# 22. AAA and Authentication

## Enable AAA

```text
aaa new-model
```

## Configure Local AAA Authentication

```text
aaa authentication login default local
```

## Create a Local User

```text
username admin privilege 15 secret MyPassword
```

## Apply AAA Authentication to VTY Lines

```text
line vty 0 4
login authentication default
```

> AAA is used for **Authentication, Authorization, and Accounting**. External AAA servers such as RADIUS and TACACS+ are commonly used in larger networks.

---

# 23. NTP

## Configure an NTP Client

```text
ntp server 192.168.1.10
```

## Configure the Device as an NTP Master

```text
ntp master 3
```

## Display the Clock

```text
show clock
```

## Verify NTP Status

```text
show ntp status
```

## Verify NTP Associations

```text
show ntp associations
```

---

# 24. DNS

## Enable DNS Lookup

```text
ip domain lookup
```

## Disable DNS Lookup

```text
no ip domain lookup
```

## Configure a DNS Server

```text
ip name-server 8.8.8.8
```

## Configure Multiple DNS Servers

```text
ip name-server 8.8.8.8 1.1.1.1
```

## Verify DNS Information

```text
show hosts
```

---

# 25. Syslog

## Configure a Syslog Server

```text
logging host 192.168.1.100
```

## Configure the Logging Severity

```text
logging trap warnings
```

Common severity levels:

```text
0 Emergencies
1 Alerts
2 Critical
3 Errors
4 Warnings
5 Notifications
6 Informational
7 Debugging
```

## Enable Console Logging

```text
logging console
```

## Configure Buffered Logging

```text
logging buffered
```

## Verify Logging

```text
show logging
```

---

# 26. SNMP

## Configure a Read-Only Community

```text
snmp-server community public ro
```

## Configure a Read-Write Community

```text
snmp-server community private rw
```

## Verify SNMP

```text
show snmp
```

> SNMP is commonly used for network monitoring and management.

---

# 27. TFTP and Configuration Transfer

## Copy Running Configuration to TFTP

```text
copy running-config tftp:
```

## Copy Startup Configuration to TFTP

```text
copy startup-config tftp:
```

## Restore Configuration from TFTP

```text
copy tftp: running-config
```

or:

```text
copy tftp: startup-config
```

## Copy a File from Flash to TFTP

```text
copy flash: tftp:
```

## Copy a File from TFTP to Flash

```text
copy tftp: flash:
```

---

# 28. Configuration Management

## Save Running Configuration

```text
copy running-config startup-config
```

## Copy Startup Configuration to Running Configuration

```text
copy startup-config running-config
```

## Display Running Configuration

```text
show running-config
```

## Display Startup Configuration

```text
show startup-config
```

## Display Flash Contents

```text
show flash:
```

or:

```text
dir flash:
```

## Display File System Contents

```text
dir
```

---

# 29. Verification Commands

## General Device Information

```text
show version
```

## Running Configuration

```text
show running-config
```

## Startup Configuration

```text
show startup-config
```

## Interface Summary

```text
show ip interface brief
```

```text
show ipv6 interface brief
```

## Detailed Interface Information

```text
show interfaces
```

```text
show interfaces gigabitEthernet 0/0
```

## Interface Configuration

```text
show running-config interface gigabitEthernet 0/0
```

## VLAN Information

```text
show vlan brief
```

```text
show vlan
```

## Trunk Information

```text
show interfaces trunk
```

## Switchport Information

```text
show interfaces gigabitEthernet 0/1 switchport
```

## MAC Address Table

```text
show mac address-table
```

## Display Dynamic MAC Addresses

```text
show mac address-table dynamic
```

## Display MAC Addresses for a VLAN

```text
show mac address-table vlan 10
```

## Display MAC Addresses on an Interface

```text
show mac address-table interface gigabitEthernet 0/1
```

## IPv4 Routing Table

```text
show ip route
```

## IPv6 Routing Table

```text
show ipv6 route
```

## ARP Table

```text
show ip arp
```

## IPv6 Neighbor Table

```text
show ipv6 neighbors
```

## CDP

```text
show cdp neighbors
```

```text
show cdp neighbors detail
```

## LLDP

```text
show lldp neighbors
```

```text
show lldp neighbors detail
```

## EtherChannel

```text
show etherchannel summary
```

## STP

```text
show spanning-tree
```

## OSPF

```text
show ip ospf neighbor
```

```text
show ip route ospf
```

## DHCP

```text
show ip dhcp pool
```

```text
show ip dhcp binding
```

## NAT

```text
show ip nat translations
```

```text
show ip nat statistics
```

## ACLs

```text
show access-lists
```

```text
show ip access-lists
```

```text
show ipv6 access-list
```

## Port Security

```text
show port-security
```

```text
show port-security address
```

## DHCP Snooping

```text
show ip dhcp snooping
```

```text
show ip dhcp snooping binding
```

## Dynamic ARP Inspection

```text
show ip arp inspection
```

## SSH

```text
show ip ssh
```

## NTP

```text
show clock
```

```text
show ntp status
```

```text
show ntp associations
```

## Syslog

```text
show logging
```

## SNMP

```text
show snmp
```

---

# 30. Troubleshooting

## Test Connectivity

```text
ping 192.168.1.1
```

## Trace a Path

```text
traceroute 192.168.1.1
```

## Verify Interface Status

```text
show ip interface brief
```

Look for:

```text
up/up
```

A common problem state:

```text
administratively down/down
```

usually indicates that the interface is shut down.

## Detailed Interface Troubleshooting

```text
show interfaces
```

```text
show interfaces gigabitEthernet 0/0
```

## Check Interface Configuration

```text
show running-config interface gigabitEthernet 0/0
```

## Check Switchport Configuration

```text
show interfaces gigabitEthernet 0/1 switchport
```

## Check VLAN Membership

```text
show vlan brief
```

## Check Trunk Status

```text
show interfaces trunk
```

## Check MAC Learning

```text
show mac address-table
```

## Check ARP

```text
show ip arp
```

## Check IPv6 Neighbors

```text
show ipv6 neighbors
```

## Check Routing

```text
show ip route
```

```text
show ipv6 route
```

## Check OSPF Neighbors

```text
show ip ospf neighbor
```

## Check EtherChannel

```text
show etherchannel summary
```

## Check STP

```text
show spanning-tree
```

## Check ACL Counters

```text
show access-lists
```

## Clear ARP Cache

```text
clear arp-cache
```

## Clear Interface Counters

```text
clear counters
```

## Reset an Interface

```text
interface gigabitEthernet 0/1
shutdown
no shutdown
```

## Reset an Interface to Defaults

```text
default interface gigabitEthernet 0/1
```

---

# 31. Common Configuration Workflows

## Basic Router Configuration

```text
enable
configure terminal

hostname R1

enable secret MyPassword

no ip domain lookup

interface gigabitEthernet 0/0
description LAN
ip address 192.168.1.1 255.255.255.0
no shutdown

end

copy running-config startup-config
```

## Basic Layer 2 Switch Configuration

```text
enable
configure terminal

hostname SW1

enable secret MyPassword

vlan 10
name USERS
exit

interface range gigabitEthernet 0/1-10
switchport mode access
switchport access vlan 10
no shutdown

end

copy running-config startup-config
```

## Basic Trunk Configuration

```text
enable
configure terminal

interface gigabitEthernet 0/24
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown

end

copy running-config startup-config
```

## Basic SSH Configuration

```text
enable
configure terminal

hostname R1

ip domain name example.local

username admin privilege 15 secret MyPassword

crypto key generate rsa

ip ssh version 2

line vty 0 4
login local
transport input ssh
exit

end

copy running-config startup-config
```

## Basic OSPF Configuration

```text
enable
configure terminal

router ospf 1
router-id 1.1.1.1
network 192.168.1.0 0.0.0.255 area 0
network 10.0.0.0 0.0.0.3 area 0

end
```

Verify:

```text
show ip ospf neighbor
show ip route ospf
```

## Basic NAT/PAT Configuration

```text
enable
configure terminal

access-list 1 permit 192.168.1.0 0.0.0.255

interface gigabitEthernet 0/0
ip nat inside
exit

interface gigabitEthernet 0/1
ip nat outside
exit

ip nat inside source list 1 interface gigabitEthernet 0/1 overload

end
```

Verify:

```text
show ip nat translations
show ip nat statistics
```

## Basic Port Security Configuration

```text
enable
configure terminal

interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict

end
```

Verify:

```text
show port-security interface gigabitEthernet 0/1
```

---

# Quick Reference

## Enter Configuration Mode

```text
enable
configure terminal
```

## Save Configuration

```text
copy running-config startup-config
```

## Interface

```text
interface gigabitEthernet 0/0
no shutdown
```

## IPv4

```text
ip address 192.168.1.1 255.255.255.0
```

## IPv6

```text
ipv6 address 2001:DB8:1::1/64
```

## VLAN

```text
vlan 10
name USERS
```

## Access Port

```text
switchport mode access
switchport access vlan 10
```

## Trunk

```text
switchport mode trunk
```

## Static Route

```text
ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

## IPv6 Default Route

```text
ipv6 route ::/0 2001:DB8:1::1
```

## OSPF

```text
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
```

## DHCP Relay

```text
ip helper-address 192.168.1.10
```

## NAT/PAT

```text
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```

## ACL

```text
access-list 10 permit 192.168.1.0 0.0.0.255
```

## SSH

```text
ip ssh version 2
```

## Port Security

```text
switchport port-security
```

## DHCP Snooping

```text
ip dhcp snooping
```

## DAI

```text
ip arp inspection vlan 10
```

## NTP

```text
ntp server 192.168.1.10
```

## Most Useful Verification Commands

```text
show running-config
show ip interface brief
show ipv6 interface brief
show vlan brief
show interfaces trunk
show mac address-table
show ip route
show ipv6 route
show ip ospf neighbor
show etherchannel summary
show spanning-tree
show access-lists
show port-security
show ip dhcp snooping
show ip arp inspection
show ip ssh
```

---

## Notes

- Replace example IP addresses, VLAN IDs, interfaces, usernames, passwords, and hostnames with values appropriate for your topology.
- Cisco IOS syntax can vary between device models and IOS versions.
- Packet Tracer does not implement every feature available on physical Cisco equipment.
- Some commands may require a specific device model or IOS feature set.
- This reference is intended for **CCNA study, Cisco Packet Tracer labs, networking practice, and practical Cisco IOS reference**.

---

**CCNA 200-301 v1.1 — Cisco Configuration Commands**
