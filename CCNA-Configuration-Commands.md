# CCNA Configuration Commands

Cisco IOS / Cisco Packet Tracer command reference focused on CCNA-level networking.

---

## Table of Contents

1. [Cisco IOS CLI Basics](#1-cisco-ios-cli-basics)
2. [Basic Device Configuration](#2-basic-device-configuration)
3. [Interface Configuration](#3-interface-configuration)
4. [IPv4 Configuration](#4-ipv4-configuration)
5. [IPv6 Configuration](#5-ipv6-configuration)
6. [VLANs](#6-vlans)
7. [Trunking](#7-trunking)
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
28. [Device Verification](#28-device-verification)
29. [Troubleshooting](#29-troubleshooting)

---

# 1. Cisco IOS CLI Basics

### Enter privileged EXEC mode

```text
enable
```

### Enter global configuration mode

```text
configure terminal
```

### Exit the current mode

```text
exit
```

### Return directly to privileged EXEC mode

```text
end
```

### Save the running configuration

```text
copy running-config startup-config
```

### Display the running configuration

```text
show running-config
```

### Display the startup configuration

```text
show startup-config
```

### Display IOS and device information

```text
show version
```

### Display available commands

```text
?
```

### Display previously entered commands

```text
show history
```

---

# 2. Basic Device Configuration

## Set the hostname

```text
configure terminal
hostname R1
```

Example:

```text
R1(config)#
```

## Configure the privileged EXEC password

```text
enable secret MyPassword
```

## Configure the console password

```text
line console 0
password MyPassword
login
exit
```

## Configure VTY access

```text
line vty 0 4
password MyPassword
login
exit
```

## Configure a login banner

```text
banner motd #Unauthorized access prohibited#
```

## Disable DNS lookup

```text
no ip domain lookup
```

## Configure a domain name

```text
ip domain name example.local
```

---

# 3. Interface Configuration

## Enter an interface

```text
interface gigabitEthernet 0/0
```

## Add a description

```text
description Connection_to_R2
```

## Enable an interface

```text
no shutdown
```

## Disable an interface

```text
shutdown
```

## Configure multiple interfaces at once

```text
interface range gigabitEthernet 0/1-4
```

## Verify interfaces

```text
show ip interface brief
show interfaces
show interfaces gigabitEthernet 0/0
```

---

# 4. IPv4 Configuration

## Configure an IPv4 address

```text
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```

## Configure a secondary IPv4 address

```text
ip address 192.168.2.1 255.255.255.0 secondary
```

## Remove an IPv4 address

```text
no ip address
```

## Verify IPv4 addressing

```text
show ip interface brief
show ip interface
```

## Test connectivity

```text
ping 192.168.1.1
```

---

# 5. IPv6 Configuration

## Enable IPv6 routing

```text
ipv6 unicast-routing
```

## Configure a global unicast address

```text
interface gigabitEthernet 0/0
ipv6 address 2001:DB8:1::1/64
no shutdown
```

## Configure a link-local address

```text
ipv6 address FE80::1 link-local
```

## Configure an address using EUI-64

```text
ipv6 address 2001:DB8:1::/64 eui-64
```

## Remove an IPv6 address

```text
no ipv6 address 2001:DB8:1::1/64
```

## Verify IPv6

```text
show ipv6 interface brief
show ipv6 interface
```

## Test IPv6 connectivity

```text
ping 2001:DB8:1::2
```

---

# 6. VLANs

## Create a VLAN

```text
vlan 10
name SALES
exit
```

## Configure an access port

```text
interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 10
```

## Configure multiple access ports

```text
interface range gigabitEthernet 0/1-10
switchport mode access
switchport access vlan 10
```

## Return a port to VLAN 1

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
show vlan
show interfaces switchport
```

---

# 7. Trunking

## Configure a trunk

```text
interface gigabitEthernet 0/1
switchport mode trunk
```

## Configure the native VLAN

```text
switchport trunk native vlan 99
```

## Configure allowed VLANs

```text
switchport trunk allowed vlan 10,20,30
```

## Add a VLAN to the allowed list

```text
switchport trunk allowed vlan add 40
```

## Remove a VLAN from the allowed list

```text
switchport trunk allowed vlan remove 40
```

## Allow all VLANs

```text
switchport trunk allowed vlan all
```

## Verify trunks

```text
show interfaces trunk
show interfaces switchport
```

---

# 8. Inter-VLAN Routing

## Router-on-a-Stick

```text
interface gigabitEthernet 0/0
no shutdown

interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

## Native VLAN subinterface

```text
interface gigabitEthernet 0/0.99
encapsulation dot1Q 99 native
ip address 192.168.99.1 255.255.255.0
```

## SVI on a Layer 3 switch

```text
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown
```

## Enable Layer 3 routing on a switch

```text
ip routing
```

## Configure a Layer 3 physical interface

```text
interface gigabitEthernet 0/1
no switchport
ip address 10.0.0.1 255.255.255.252
no shutdown
```

---

# 9. CDP and LLDP

## Enable CDP globally

```text
cdp run
```

## Disable CDP globally

```text
no cdp run
```

## Enable CDP on an interface

```text
interface gigabitEthernet 0/1
cdp enable
```

## Disable CDP on an interface

```text
no cdp enable
```

## Verify CDP

```text
show cdp neighbors
show cdp neighbors detail
show cdp interface
```

## Enable LLDP

```text
lldp run
```

## Configure LLDP transmit/receive

```text
interface gigabitEthernet 0/1
lldp transmit
lldp receive
```

## Verify LLDP

```text
show lldp
show lldp neighbors
show lldp neighbors detail
```

---

# 10. EtherChannel and LACP

## Configure LACP

```text
interface range gigabitEthernet 0/1-2
channel-group 1 mode active
```

## Configure the Port-Channel

```text
interface port-channel 1
switchport mode trunk
```

## Access EtherChannel

```text
interface range gigabitEthernet 0/1-2
switchport mode access
switchport access vlan 10
channel-group 1 mode active
```

## Verify EtherChannel

```text
show etherchannel summary
show etherchannel port-channel
show interfaces port-channel 1
```

---

# 11. STP and Rapid PVST+

## Enable Rapid PVST+

```text
spanning-tree mode rapid-pvst
```

## Configure a switch as root primary

```text
spanning-tree vlan 10 root primary
```

## Configure a switch as root secondary

```text
spanning-tree vlan 10 root secondary
```

## Configure PortFast

```text
interface gigabitEthernet 0/1
spanning-tree portfast
```

## Configure BPDU Guard

```text
spanning-tree bpduguard enable
```

## Configure root guard

```text
spanning-tree guard root
```

## Configure loop guard

```text
spanning-tree guard loop
```

## Verify STP

```text
show spanning-tree
show spanning-tree vlan 10
show spanning-tree root
show spanning-tree summary
```

---

# 12. Static Routing

## IPv4 network route

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## Static route using an exit interface

```text
ip route 192.168.20.0 255.255.255.0 gigabitEthernet 0/1
```

## Default route

```text
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

## IPv4 host route

```text
ip route 192.168.20.10 255.255.255.255 10.0.0.2
```

## Floating static route

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2 200
```

## Remove a static route

```text
no ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## IPv6 static route

```text
ipv6 route 2001:DB8:20::/64 2001:DB8:1::2
```

## IPv6 default route

```text
ipv6 route ::/0 2001:DB8:1::2
```

## Verify routing

```text
show ip route
show ip route static
show ipv6 route
show ipv6 route static
```

---

# 13. OSPFv2

## Start OSPF

```text
router ospf 1
```

## Configure a router ID

```text
router-id 1.1.1.1
```

## Advertise a network

```text
network 192.168.1.0 0.0.0.255 area 0
```

## Configure OSPF directly on an interface

```text
interface gigabitEthernet 0/0
ip ospf 1 area 0
```

## Configure a passive interface

```text
passive-interface gigabitEthernet 0/1
```

## Verify OSPF

```text
show ip protocols
show ip ospf
show ip ospf interface
show ip ospf neighbor
show ip route ospf
```

---

# 14. DHCP

## Configure an interface as a DHCP client

```text
interface gigabitEthernet 0/0
ip address dhcp
no shutdown
```

## Configure a DHCP relay

```text
interface gigabitEthernet 0/0
ip helper-address 192.168.1.10
```

## Remove a DHCP relay

```text
no ip helper-address 192.168.1.10
```

## DHCP server configuration

```text
ip dhcp excluded-address 192.168.10.1 192.168.10.20

ip dhcp pool LAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
domain-name example.local
```

## Verify DHCP

```text
show ip dhcp pool
show ip dhcp binding
show ip dhcp conflict
show ip interface
```

---

# 15. NAT and PAT

## Configure inside interface

```text
interface gigabitEthernet 0/0
ip nat inside
```

## Configure outside interface

```text
interface gigabitEthernet 0/1
ip nat outside
```

## Static NAT

```text
ip nat inside source static 192.168.1.10 203.0.113.10
```

## Dynamic NAT pool

```text
access-list 1 permit 192.168.1.0 0.0.0.255

ip nat pool PUBLIC 203.0.113.10 203.0.113.20 netmask 255.255.255.0

ip nat inside source list 1 pool PUBLIC
```

## PAT using the outside interface

```text
access-list 1 permit 192.168.1.0 0.0.0.255

ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```

## Verify NAT

```text
show ip nat translations
show ip nat statistics
```

## Clear NAT translations

```text
clear ip nat translation *
```

---

# 16. Access Control Lists

## Standard numbered ACL

```text
access-list 10 permit 192.168.1.0 0.0.0.255
```

## Permit a specific host

```text
access-list 10 permit host 192.168.1.10
```

## Deny a network

```text
access-list 10 deny 192.168.1.0 0.0.0.255
```

## Apply a standard ACL

```text
interface gigabitEthernet 0/0
ip access-group 10 in
```

## Named standard ACL

```text
ip access-list standard BLOCK-LAN
deny 192.168.1.0 0.0.0.255
permit any
exit
```

## Extended ACL

```text
access-list 100 permit ip 192.168.1.0 0.0.0.255 any
```

## Permit TCP

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any
```

## Permit HTTP

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
```

## Permit HTTPS

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
```

## Permit SSH

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 22
```

## Permit ICMP

```text
access-list 100 permit icmp 192.168.1.0 0.0.0.255 any
```

## Apply an extended ACL

```text
interface gigabitEthernet 0/0
ip access-group 100 in
```

## Verify ACLs

```text
show access-lists
show ip access-lists
show running-config
```

---

# 17. Port Security

## Enable port security

```text
interface gigabitEthernet 0/1
switchport mode access
switchport port-security
```

## Limit the number of MAC addresses

```text
switchport port-security maximum 2
```

## Configure a secure MAC address

```text
switchport port-security mac-address 0011.2233.4455
```

## Enable sticky MAC addresses

```text
switchport port-security mac-address sticky
```

## Configure violation mode

```text
switchport port-security violation shutdown
```

Other violation modes:

```text
switchport port-security violation restrict
switchport port-security violation protect
```

## Verify port security

```text
show port-security
show port-security interface gigabitEthernet 0/1
show port-security address
```

---

# 18. DHCP Snooping

## Enable DHCP snooping

```text
ip dhcp snooping
```

## Enable it for a VLAN

```text
ip dhcp snooping vlan 10
```

## Trust a DHCP server-facing port

```text
interface gigabitEthernet 0/24
ip dhcp snooping trust
```

## Rate-limit DHCP packets

```text
interface gigabitEthernet 0/1
ip dhcp snooping limit rate 10
```

## Verify DHCP snooping

```text
show ip dhcp snooping
show ip dhcp snooping binding
```

---

# 19. Dynamic ARP Inspection

## Enable DAI for a VLAN

```text
ip arp inspection vlan 10
```

## Trust an interface

```text
interface gigabitEthernet 0/24
ip arp inspection trust
```

## Rate-limit ARP packets

```text
ip arp inspection limit rate 15
```

## Verify DAI

```text
show ip arp inspection
show ip arp inspection vlan 10
show ip arp inspection interfaces
```

---

# 20. IPv6 RA Guard

## Enable RA Guard on an interface

```text
interface gigabitEthernet 0/1
ipv6 nd raguard
```

## Remove RA Guard

```text
no ipv6 nd raguard
```

---

# 21. SSH

## Configure a domain name

```text
ip domain name example.local
```

## Create a local user

```text
username admin privilege 15 secret MyPassword
```

## Generate RSA keys

```text
crypto key generate rsa
```

Example:

```text
How many bits in the modulus [512]: 2048
```

## Enable SSH version 2

```text
ip ssh version 2
```

## Configure VTY lines for local authentication

```text
line vty 0 4
login local
transport input ssh
exit
```

## Verify SSH

```text
show ip ssh
show users
show running-config
```

---

# 22. AAA and Authentication

## Enable AAA

```text
aaa new-model
```

## Configure local AAA authentication

```text
aaa authentication login default local
```

## Create a local user

```text
username admin privilege 15 secret MyPassword
```

## Configure VTY authentication

```text
line vty 0 4
login authentication default
```

---

# 23. NTP

## Configure an NTP client

```text
ntp server 192.168.1.10
```

## Configure the device as an NTP master

```text
ntp master 3
```

## Verify NTP

```text
show clock
show ntp status
show ntp associations
```

---

# 24. DNS

## Enable DNS lookup

```text
ip domain lookup
```

## Disable DNS lookup

```text
no ip domain lookup
```

## Configure a DNS server

```text
ip name-server 8.8.8.8
```

## Verify DNS information

```text
show hosts
```

---

# 25. Syslog

## Send logs to a Syslog server

```text
logging host 192.168.1.100
```

## Set the logging level

```text
logging trap warnings
```

## Enable console logging

```text
logging console
```

## Configure buffered logging

```text
logging buffered
```

## Verify logging

```text
show logging
```

---

# 26. SNMP

## Configure a read-only community

```text
snmp-server community public ro
```

## Configure a read-write community

```text
snmp-server community private rw
```

## Verify SNMP

```text
show snmp
```

---

# 27. TFTP and Configuration Transfer

## Copy running configuration to a TFTP server

```text
copy running-config tftp:
```

## Copy startup configuration to a TFTP server

```text
copy startup-config tftp:
```

## Restore configuration from TFTP

```text
copy tftp: running-config
```

or:

```text
copy tftp: startup-config
```

## Copy IOS/image files

```text
copy flash: tftp:
```

```text
copy tftp: flash:
```

---

# 28. Device Verification

## General device information

```text
show version
```

## Running configuration

```text
show running-config
```

## Startup configuration

```text
show startup-config
```

## Interface summary

```text
show ip interface brief
show ipv6 interface brief
```

## VLAN information

```text
show vlan brief
```

## Trunk information

```text
show interfaces trunk
```

## MAC address table

```text
show mac address-table
```

## Routing table

```text
show ip route
show ipv6 route
```

## ARP table

```text
show ip arp
```

---

# 29. Troubleshooting

## Test connectivity

```text
ping <destination>
```

## Trace the path

```text
traceroute <destination>
```

## View interface status

```text
show ip interface brief
show interfaces
```

## View a specific interface

```text
show interfaces gigabitEthernet 0/0
```

## View interface configuration

```text
show running-config interface gigabitEthernet 0/0
```

## View switchport information

```text
show interfaces gigabitEthernet 0/1 switchport
```

## View MAC addresses

```text
show mac address-table
```

## View ARP entries

```text
show ip arp
```

## Clear ARP cache

```text
clear arp-cache
```

## Clear interface counters

```text
clear counters
```

## Reset an interface to its default configuration

```text
default interface gigabitEthernet 0/1
```

## Remove a VLAN

```text
no vlan 10
```

## Display available commands

```text
?
```

## Use command completion

```text
<TAB>
```

## Repeat previous commands

```text
↑
```

## Return to privileged EXEC mode

```text
end
```

or:

```text
Ctrl+Z
```

---

# Quick Save

Always save important configurations:

```text
copy running-config startup-config
```

or:

```text
write memory
```

---

## Notes

- Replace example IP addresses, VLAN IDs, interfaces, usernames, passwords, and hostnames with values appropriate for your topology.
- Command availability can vary depending on the Cisco IOS version and device model.
- Packet Tracer may not support every command available on physical Cisco hardware.
- This reference is intended primarily for **CCNA-level study, Cisco Packet Tracer labs, and general Cisco IOS practice**.
