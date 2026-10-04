# CCNA Configuration Commands

Cisco IOS / Cisco Packet Tracer command reference focused on CCNA-level networking, practical configuration, verification, and troubleshooting.

> **Scope:** CCNA-focused Cisco IOS commands with useful real-world and Packet Tracer configuration examples.  
> **Platform:** Cisco IOS / Cisco Packet Tracer  
> **Style:** Configuration + verification + troubleshooting

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
30. [Common Configuration Workflows](#30-common-configuration-workflows)
31. [Quick Reference](#31-quick-reference)

---

# 1. Cisco IOS CLI Basics

## Enter privileged EXEC mode

```text
enable
```

## Enter global configuration mode

```text
configure terminal
```

Short form:

```text
conf t
```

## Exit the current configuration mode

```text
exit
```

## Return directly to privileged EXEC mode

```text
end
```

or:

```text
Ctrl+Z
```

## Save the running configuration

```text
copy running-config startup-config
```

Short form:

```text
copy run start
```

Alternative:

```text
write memory
```

## Display the running configuration

```text
show running-config
```

Short form:

```text
show run
```

## Display the startup configuration

```text
show startup-config
```

Short form:

```text
show start
```

## Display IOS and device information

```text
show version
```

## Display available commands

```text
?
```

## Display commands beginning with a specific word

```text
show ?
```

## Display previously entered commands

```text
show history
```

## Useful CLI shortcuts

```text
Tab
```

Auto-completes a command.

```text
?
```

Displays available commands or parameters.

```text
Ctrl+C
```

Cancels the current command or operation.

```text
Up Arrow
```

Recalls the previous command.

---

# 2. Basic Device Configuration

## Set the hostname

```text
configure terminal
hostname R1
```

Example prompt:

```text
R1(config)#
```

## Configure the privileged EXEC password

Use `enable secret` rather than the older `enable password`.

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

Some devices support additional VTY lines:

```text
line vty 0 15
```

## Configure a local username

```text
username admin secret MyPassword
```

## Disable DNS lookup

Useful in labs so IOS does not interpret mistyped commands as hostnames.

```text
no ip domain lookup
```

## Configure a domain name

Required for RSA key generation when configuring SSH.

```text
ip domain name example.local
```

## Configure a login banner

```text
banner motd #Unauthorized access prohibited#
```

## Configure an encrypted enable password

```text
enable secret MySecretPassword
```

## Encrypt plaintext line passwords

```text
service password-encryption
```

---

# 3. Interface Configuration

## Enter an interface

```text
interface gigabitEthernet 0/0
```

Short form:

```text
interface g0/0
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

Example:

```text
interface range gigabitEthernet 0/1-10
shutdown
```

## Reset an interface to default settings

```text
default interface gigabitEthernet 0/1
```

## Verify interface status

```text
show ip interface brief
```

## Display detailed interface information

```text
show interfaces
```

## Display one specific interface

```text
show interfaces gigabitEthernet 0/0
```

## Display the configured interface section

```text
show running-config interface gigabitEthernet 0/0
```

## Check Layer 3 interface status

```text
show ip interface gigabitEthernet 0/0
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

## Configure a default gateway on a Layer 2 switch

```text
ip default-gateway 192.168.1.1
```

> On a Layer 3 switch using `ip routing`, routing is handled with routes instead of `ip default-gateway`.

## Verify IPv4 addressing

```text
show ip interface brief
```

```text
show ip interface
```

## Test connectivity

```text
ping 192.168.1.1
```

## Ping from a specific source

```text
ping
```

Then specify the source when prompted.

Or, on platforms supporting extended ping:

```text
ping 192.168.1.1 source gigabitEthernet 0/0
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

## Configure an IPv6 address using EUI-64

```text
ipv6 address 2001:DB8:1::/64 eui-64
```

## Remove an IPv6 address

```text
no ipv6 address 2001:DB8:1::1/64
```

## Verify IPv6 interfaces

```text
show ipv6 interface brief
```

```text
show ipv6 interface
```

## Display the IPv6 routing table

```text
show ipv6 route
```

## Test IPv6 connectivity

```text
ping 2001:DB8:1::2
```

## Display IPv6 neighbors

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
switchport mode access
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

## Display switchport information

```text
show interfaces switchport
```

## Display a specific port's switchport information

```text
show interfaces gigabitEthernet 0/1 switchport
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

## Remove the allowed VLAN restriction

```text
no switchport trunk allowed vlan
```

## Verify trunks

```text
show interfaces trunk
```

```text
show interfaces switchport
```

---

# 8. Inter-VLAN Routing

## Router-on-a-Stick

Configure the physical router interface:

```text
interface gigabitEthernet 0/0
no shutdown
```

Configure VLAN 10:

```text
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

Configure VLAN 20:

```text
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

## Configure IPv6 on a Layer 3 interface

```text
interface gigabitEthernet 0/1
no switchport
ipv6 address 2001:DB8:1::1/64
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

## Verify CDP neighbors

```text
show cdp neighbors
```

## Display detailed CDP information

```text
show cdp neighbors detail
```

## Display CDP interface information

```text
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

`active` actively negotiates LACP.

A passive LACP side:

```text
channel-group 1 mode passive
```

> At least one side must actively initiate LACP.

## Configure a Port-Channel as a trunk

```text
interface port-channel 1
switchport mode trunk
```

## Configure allowed VLANs on the Port-Channel

```text
interface port-channel 1
switchport trunk allowed vlan 10,20,30
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

## Display LACP neighbors

```text
show lacp neighbor
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

## Explicitly configure STP priority

```text
spanning-tree vlan 10 priority 24576
```

Lower priority wins root bridge election.

## Configure PortFast

```text
interface gigabitEthernet 0/1
spanning-tree portfast
```

## Enable BPDU Guard

```text
interface gigabitEthernet 0/1
spanning-tree bpduguard enable
```

## Configure root guard

```text
interface gigabitEthernet 0/1
spanning-tree guard root
```

## Configure loop guard

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

## IPv4 network route

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## Static route using an exit interface

```text
ip route 192.168.20.0 255.255.255.0 gigabitEthernet 0/1
```

## Static route using next-hop and exit interface

```text
ip route 192.168.20.0 255.255.255.0 gigabitEthernet 0/1 10.0.0.2
```

## IPv4 default route

```text
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

## IPv4 host route

```text
ip route 192.168.20.10 255.255.255.255 10.0.0.2
```

## Floating static route

A higher administrative distance makes it a backup route.

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

## IPv6 host route

```text
ipv6 route 2001:DB8:20::10/128 2001:DB8:1::2
```

## IPv6 floating static route

```text
ipv6 route 2001:DB8:20::/64 2001:DB8:1::2 200
```

## Verify routing

```text
show ip route
```

```text
show ip route static
```

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

## Make all interfaces passive

```text
passive-interface default
```

## Make a specific interface active again

```text
no passive-interface gigabitEthernet 0/0
```

## Configure OSPF cost

```text
interface gigabitEthernet 0/0
ip ospf cost 10
```

## Configure OSPF interface priority

```text
interface gigabitEthernet 0/0
ip ospf priority 100
```

## Advertise a default route through OSPF

```text
default-information originate
```

## Restart the OSPF process

```text
clear ip ospf process
```

## Verify OSPF configuration

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
show ip ospf neighbor
```

```text
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

## Configure a DHCP server

Exclude addresses:

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

## Configure a DHCP lease

```text
ip dhcp pool LAN10
lease 7
```

## Verify DHCP pools

```text
show ip dhcp pool
```

## Display DHCP bindings

```text
show ip dhcp binding
```

## Display DHCP conflicts

```text
show ip dhcp conflict
```

## Display DHCP-related interface information

```text
show ip interface
```

---

# 15. NAT and PAT

## Configure the inside interface

```text
interface gigabitEthernet 0/0
ip nat inside
```

## Configure the outside interface

```text
interface gigabitEthernet 0/1
ip nat outside
```

## Static NAT

```text
ip nat inside source static 192.168.1.10 203.0.113.10
```

## Dynamic NAT pool

Create the matching ACL:

```text
access-list 1 permit 192.168.1.0 0.0.0.255
```

Create the public pool:

```text
ip nat pool PUBLIC 203.0.113.10 203.0.113.20 netmask 255.255.255.0
```

Connect the ACL to the pool:

```text
ip nat inside source list 1 pool PUBLIC
```

## PAT using the outside interface

```text
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```

## Verify NAT translations

```text
show ip nat translations
```

## Verify NAT statistics

```text
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

## Permit everything

```text
access-list 10 permit any
```

## Deny everything

```text
access-list 10 deny any
```

## Apply a standard ACL

```text
interface gigabitEthernet 0/0
ip access-group 10 in
```

## Remove an ACL from an interface

```text
interface gigabitEthernet 0/0
no ip access-group 10 in
```

## Named standard ACL

```text
ip access-list standard BLOCK-LAN
deny 192.168.1.0 0.0.0.255
permit any
exit
```

## Extended ACL

Permit all IP traffic:

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

## Named extended ACL

```text
ip access-list extended WEB-ONLY
permit tcp 192.168.1.0 0.0.0.255 any eq 80
permit tcp 192.168.1.0 0.0.0.255 any eq 443
deny ip any any
exit
```

## Apply an extended ACL

```text
interface gigabitEthernet 0/0
ip access-group 100 in
```

## Remove an ACL

```text
no access-list 100
```

## IPv6 ACL

Create the ACL:

```text
ipv6 access-list V6-FILTER
permit ipv6 2001:DB8:1::/64 any
permit icmp any any
deny ipv6 any any
exit
```

Apply it:

```text
interface gigabitEthernet 0/0
ipv6 traffic-filter V6-FILTER in
```

Remove it:

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

```text
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

Protect:

```text
switchport port-security violation protect
```

Restrict:

```text
switchport port-security violation restrict
```

Shutdown:

```text
switchport port-security violation shutdown
```

## Verify port security

```text
show port-security
```

```text
show port-security interface gigabitEthernet 0/1
```

## Recover a shutdown port

```text
interface gigabitEthernet 0/1
shutdown
no shutdown
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

## Trust the DHCP server/uplink interface

```text
interface gigabitEthernet 0/1
ip dhcp snooping trust
```

## Limit DHCP messages on an untrusted port

```text
interface gigabitEthernet 0/2
ip dhcp snooping limit rate 10
```

## Verify DHCP snooping

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

## Trust an uplink

```text
interface gigabitEthernet 0/1
ip arp inspection trust
```

## Limit ARP traffic

```text
interface gigabitEthernet 0/2
ip arp inspection limit rate 15
```

## Verify DAI

```text
show ip arp inspection
```

```text
show ip arp inspection interfaces
```

```text
show ip arp inspection statistics
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
interface gigabitEthernet 0/1
no ipv6 nd raguard
```

> Exact IPv6 RA Guard support and syntax can vary by IOS release and Packet Tracer version.

---

# 21. SSH

## Configure the domain name

```text
ip domain name example.local
```

## Create a local user

```text
username admin privilege 15 secret MyPassword
```

## Generate RSA keys

```text
crypto key generate rsa modulus 2048
```

## Enable SSH version 2

```text
ip ssh version 2
```

## Configure VTY lines to use the local database

```text
line vty 0 4
login local
transport input ssh
exit
```

## Configure SSH timeout

```text
ip ssh time-out 60
```

## Configure SSH authentication retries

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

## Connect to another device using SSH

```text
ssh -l admin 192.168.1.1
```

---

# 22. AAA and Authentication

## Enable AAA

```text
aaa new-model
```

## Create a local administrative account

```text
username admin privilege 15 secret MyPassword
```

## Use the local database for login authentication

```text
aaa authentication login default local
```

## Apply the AAA method list to VTY lines

```text
line vty 0 4
login authentication default
```

## Local authentication without AAA

```text
line vty 0 4
login local
```

> `login local` uses the local username database. AAA provides a more general authentication, authorization, and accounting framework.

---

# 23. NTP

## Configure an NTP client

```text
ntp server 192.168.1.10
```

## Configure multiple NTP servers

```text
ntp server 192.168.1.10
ntp server 192.168.1.11
```

## Configure a device as an NTP master

```text
ntp master 3
```

## Verify the clock

```text
show clock
```

## Verify NTP status

```text
show ntp status
```

## Display NTP associations

```text
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

Multiple servers:

```text
ip name-server 8.8.8.8 1.1.1.1
```

## Display cached host information

```text
show hosts
```

## Test DNS resolution

```text
ping example.com
```

---

# 25. Syslog

## Configure a remote syslog server

```text
logging host 192.168.1.10
```

## Configure the logging severity level

```text
logging trap warnings
```

Common severity levels:

```text
0 emergencies
1 alerts
2 critical
3 errors
4 warnings
5 notifications
6 informational
7 debugging
```

## Configure console logging

```text
logging console warnings
```

## Configure buffered logging

```text
logging buffered 16384
```

## Verify logging

```text
show logging
```

---

# 26. SNMP

## Configure a read-only SNMP community

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

> SNMP community strings are credentials. Avoid using simple/default community strings in real production networks.

---

# 27. TFTP and Configuration Transfer

## Copy the running configuration to a TFTP server

```text
copy running-config tftp:
```

IOS will prompt for:

```text
Address or name of remote host
Destination filename
```

## Copy startup configuration to a TFTP server

```text
copy startup-config tftp:
```

## Restore a configuration from TFTP

```text
copy tftp: running-config
```

or:

```text
copy tftp: startup-config
```

## Copy an IOS image from flash to TFTP

```text
copy flash: tftp:
```

## Copy an IOS image from TFTP to flash

```text
copy tftp: flash:
```

## Display flash contents

```text
show flash:
```

or:

```text
dir flash:
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
```

## Detailed interfaces

```text
show interfaces
```

## VLANs

```text
show vlan brief
```

## Trunks

```text
show interfaces trunk
```

## MAC address table

```text
show mac address-table
```

## Specific MAC address

```text
show mac address-table address 0011.2233.4455
```

## ARP table

```text
show ip arp
```

## IPv4 routing table

```text
show ip route
```

## IPv6 routing table

```text
show ipv6 route
```

## OSPF neighbors

```text
show ip ospf neighbor
```

## OSPF routes

```text
show ip route ospf
```

## EtherChannel

```text
show etherchannel summary
```

## STP

```text
show spanning-tree
```

## CDP neighbors

```text
show cdp neighbors
```

## LLDP neighbors

```text
show lldp neighbors
```

## Port security

```text
show port-security
```

## DHCP snooping

```text
show ip dhcp snooping
```

## DAI

```text
show ip arp inspection
```

## ACLs

```text
show access-lists
```

## NAT

```text
show ip nat translations
```

```text
show ip nat statistics
```

## SSH

```text
show ip ssh
```

## Users currently connected

```text
show users
```

## Logging

```text
show logging
```

## NTP

```text
show ntp status
```

```text
show ntp associations
```

---

# 29. Troubleshooting

## Test basic connectivity

```text
ping 192.168.1.1
```

## Trace the path to a destination

```text
traceroute 192.168.1.1
```

## Check interface status

```text
show ip interface brief
```

Look for:

```text
up    up
```

If an interface is:

```text
administratively down
```

enable it:

```text
interface gigabitEthernet 0/0
no shutdown
```

## Check detailed interface errors

```text
show interfaces gigabitEthernet 0/0
```

Useful counters include:

- input errors
- CRC
- collisions
- drops
- output errors

## Check VLAN assignment

```text
show vlan brief
```

## Check trunking

```text
show interfaces trunk
```

## Check switchport configuration

```text
show interfaces gigabitEthernet 0/1 switchport
```

## Check MAC addresses

```text
show mac address-table
```

## Check ARP

```text
show ip arp
```

## Check routing

```text
show ip route
```

## Check IPv6 routing

```text
show ipv6 route
```

## Check OSPF neighbors

```text
show ip ospf neighbor
```

## Check OSPF configuration

```text
show ip protocols
```

## Check EtherChannel

```text
show etherchannel summary
```

Look for correctly bundled ports.

## Check STP

```text
show spanning-tree
```

## Check ACLs

```text
show access-lists
```

## Check ACL application

```text
show running-config interface gigabitEthernet 0/0
```

## Check NAT

```text
show ip nat translations
```

```text
show ip nat statistics
```

## Clear ARP cache

```text
clear arp-cache
```

## Clear interface counters

```text
clear counters
```

## Reset an interface

```text
interface gigabitEthernet 0/0
shutdown
no shutdown
```

## Reset an interface configuration

```text
default interface gigabitEthernet 0/0
```

> Be careful with `default interface` because it removes the interface's current configuration.

---

# 30. Common Configuration Workflows

## Basic Router Configuration

```text
enable
configure terminal

hostname R1
no ip domain lookup
enable secret MySecretPassword

interface gigabitEthernet 0/0
description LAN
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
description WAN
ip address 10.0.0.1 255.255.255.252
no shutdown
exit

end
copy running-config startup-config
```

---

## Basic Layer 2 Switch Configuration

```text
enable
configure terminal

hostname SW1
no ip domain lookup
enable secret MySecretPassword

vlan 10
name USERS
exit

interface range gigabitEthernet 0/1-10
switchport mode access
switchport access vlan 10
spanning-tree portfast
spanning-tree bpduguard enable
exit

end
copy running-config startup-config
```

---

## Trunk Configuration

Switch 1:

```text
enable
configure terminal

interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit

end
copy running-config startup-config
```

Switch 2:

```text
enable
configure terminal

interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit

end
copy running-config startup-config
```

Verify:

```text
show interfaces trunk
```

---

## SSH Configuration

```text
enable
configure terminal

hostname R1
ip domain name example.local

username admin privilege 15 secret MyPassword

crypto key generate rsa modulus 2048
ip ssh version 2

line vty 0 4
login local
transport input ssh
exit

end
copy running-config startup-config
```

Test:

```text
ssh -l admin 192.168.1.1
```

---

## OSPF Configuration

Router 1:

```text
enable
configure terminal

router ospf 1
router-id 1.1.1.1
network 192.168.1.0 0.0.0.255 area 0
network 10.0.0.0 0.0.0.3 area 0
exit

end
```

Router 2:

```text
enable
configure terminal

router ospf 1
router-id 2.2.2.2
network 192.168.2.0 0.0.0.255 area 0
network 10.0.0.0 0.0.0.3 area 0
exit

end
```

Verify:

```text
show ip ospf neighbor
show ip route ospf
```

---

## NAT/PAT Configuration

Inside interface:

```text
interface gigabitEthernet 0/0
ip nat inside
```

Outside interface:

```text
interface gigabitEthernet 0/1
ip nat outside
```

Create the matching ACL:

```text
access-list 1 permit 192.168.1.0 0.0.0.255
```

Enable PAT:

```text
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```

Verify:

```text
show ip nat translations
show ip nat statistics
```

---

## Port Security Configuration

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
exit

end
copy running-config startup-config
```

Verify:

```text
show port-security interface gigabitEthernet 0/1
```

---

# 31. Quick Reference

## Modes

```text
enable
configure terminal
interface g0/0
exit
end
```

## Save

```text
copy running-config startup-config
```

## Interface

```text
interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```

## VLAN

```text
vlan 10
name USERS
```

## Access port

```text
interface g0/1
switchport mode access
switchport access vlan 10
```

## Trunk

```text
interface g0/1
switchport mode trunk
```

## Router-on-a-Stick

```text
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

## Static route

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## Default route

```text
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

## OSPF

```text
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
```

## DHCP relay

```text
interface g0/0
ip helper-address 192.168.1.10
```

## NAT/PAT

```text
ip nat inside
ip nat outside
ip nat inside source list 1 interface g0/1 overload
```

## Standard ACL

```text
access-list 10 permit 192.168.1.0 0.0.0.255
```

## Extended ACL

```text
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
```

## Port Security

```text
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
```

## DHCP Snooping

```text
ip dhcp snooping
ip dhcp snooping vlan 10
```

## DAI

```text
ip arp inspection vlan 10
```

## SSH

```text
ip domain name example.local
username admin privilege 15 secret MyPassword
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
login local
transport input ssh
```

## Verification

```text
show ip interface brief
show running-config
show vlan brief
show interfaces trunk
show mac address-table
show ip arp
show ip route
show ipv6 route
show ip ospf neighbor
show etherchannel summary
show spanning-tree
show access-lists
show ip nat translations
show port-security
show ip dhcp snooping
show ip arp inspection
show ip ssh
show logging
show ntp status
```

---

# Notes

- Cisco IOS syntax can vary between IOS, IOS XE, switch/router models, and Packet Tracer versions.
- Interface names such as `g0/0`, `g0/1`, `fa0/1`, and `s0/0/0` depend on the device model.
- Always verify the available syntax with `?` on the actual device.
- Commands that modify routing, security, NAT, ACLs, STP, or interface configuration should be tested carefully before using them on production equipment.
- This file is intended as a practical CCNA-level Cisco IOS / Packet Tracer reference, not an exhaustive Cisco IOS command encyclopedia.
