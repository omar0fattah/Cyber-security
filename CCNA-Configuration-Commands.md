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

```
enable
```

## Enter global configuration mode

```
configure terminal
```

Short form:

```
conf t
```

## Exit the current configuration mode

```
exit
```

## Return directly to privileged EXEC mode

```
end
```

or:

```
Ctrl+Z
```

## Save the running configuration

```
copy running-config startup-config
```

Short form:

```
copy run start
```

Alternative:

```
write memory
```

## Display the running configuration

```
show running-config
```

Short form:

```
show run
```

## Display the startup configuration

```
show startup-config
```

Short form:

```
show start
```

## Display IOS and device information

```
show version
```

## Display available commands in the current mode

```
?
```

## Display the arguments of a command

```
show ?
```

Lists everything that can follow `show`.

## Display commands that start with specific letters

```
s?
```

No space before the `?`. Lists every command beginning with "s" (for example `show`, `shutdown`, `spanning-tree`).

## Display previously entered commands

```
show history
```

## Useful CLI shortcuts

```
Tab
```

Auto-completes a command.

```
?
```

Displays available commands or parameters.

```
Ctrl+C
```

Aborts the current command and exits configuration mode.

```
Ctrl+Shift+6
```

Interrupts a running process such as `ping` or `traceroute`.

```
Up Arrow
```

Recalls the previous command.

---

# 2. Basic Device Configuration

## Set the hostname

```
configure terminal
hostname R1
```

Example prompt:

```
R1(config)#
```

## Configure the privileged EXEC password

Use `enable secret` rather than the older `enable password`. `enable secret` is stored hashed.

```
enable secret MyPassword
```

## Configure the console password

```
line console 0
password MyPassword
login
exit
```

## Configure VTY access

```
line vty 0 4
password MyPassword
login
exit
```

Some devices support additional VTY lines:

```
line vty 0 15
```

## Configure a local username

```
username admin secret MyPassword
```

## Disable DNS lookup

Useful in labs so IOS does not interpret mistyped commands as hostnames.

```
no ip domain lookup
```

> On older IOS releases and some Packet Tracer versions the hyphenated form is used instead: `no ip domain-lookup`. If one is rejected, try the other.

## Configure a domain name

Required (together with a non-default hostname) for RSA key generation when configuring SSH.

```
ip domain name example.local
```

## Configure a login banner

Shown at the login prompt:

```
banner login #Unauthorized access prohibited#
```

## Configure a message-of-the-day (MOTD) banner

Shown when someone connects, before the login prompt:

```
banner motd #Authorized users only#
```

## Encrypt plaintext line passwords

```
service password-encryption
```

> This only applies weak (type 7) obfuscation to line passwords. It is not strong protection, which is why `enable secret` and `username ... secret` are preferred.

---

# 3. Interface Configuration

## Enter an interface

```
interface gigabitEthernet 0/0
```

Short form:

```
interface g0/0
```

## Add a description

```
description Connection_to_R2
```

## Enable an interface

```
no shutdown
```

## Disable an interface

```
shutdown
```

## Configure multiple interfaces at once

```
interface range gigabitEthernet 0/1-4
```

Example:

```
interface range gigabitEthernet 0/1-10
shutdown
```

## Reset an interface to default settings

```
default interface gigabitEthernet 0/1
```

## Verify interface status

```
show ip interface brief
```

## Display detailed interface information

```
show interfaces
```

## Display one specific interface

```
show interfaces gigabitEthernet 0/0
```

## Display the configured interface section

```
show running-config interface gigabitEthernet 0/0
```

## Check Layer 3 interface status

```
show ip interface gigabitEthernet 0/0
```

---

# 4. IPv4 Configuration

## Configure an IPv4 address

```
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```

## Configure a secondary IPv4 address

```
ip address 192.168.2.1 255.255.255.0 secondary
```

## Remove an IPv4 address

```
no ip address
```

## Configure a default gateway on a Layer 2 switch

```
ip default-gateway 192.168.1.1
```

> On a Layer 3 switch using `ip routing`, routing is handled with routes instead of `ip default-gateway`.

## Verify IPv4 addressing

```
show ip interface brief
```

```
show ip interface
```

## Test connectivity

```
ping 192.168.1.1
```

## Ping from a specific source

```
ping
```

Then specify the source when prompted (extended ping).

Or, on platforms supporting it:

```
ping 192.168.1.1 source gigabitEthernet 0/0
```

---

# 5. IPv6 Configuration

## Enable IPv6 routing

Required on routers before they will route IPv6 or send Router Advertisements.

```
ipv6 unicast-routing
```

## Configure a global unicast address

```
interface gigabitEthernet 0/0
ipv6 address 2001:DB8:1::1/64
no shutdown
```

## Configure a link-local address

```
ipv6 address FE80::1 link-local
```

## Configure an IPv6 address using EUI-64

```
ipv6 address 2001:DB8:1::/64 eui-64
```

## Remove an IPv6 address

```
no ipv6 address 2001:DB8:1::1/64
```

## Verify IPv6 interfaces

```
show ipv6 interface brief
```

```
show ipv6 interface
```

## Display the IPv6 routing table

```
show ipv6 route
```

## Test IPv6 connectivity

```
ping 2001:DB8:1::2
```

## Display IPv6 neighbors

```
show ipv6 neighbors
```

---

# 6. VLANs

## Create a VLAN

```
vlan 10
name SALES
exit
```

## Configure an access port

```
interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 10
```

## Configure multiple access ports

```
interface range gigabitEthernet 0/1-10
switchport mode access
switchport access vlan 10
```

## Return a port to VLAN 1

```
interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 1
```

## Delete a VLAN

```
no vlan 10
```

> Ports assigned to a deleted VLAN become inactive until you reassign them to another VLAN.

## Verify VLANs

```
show vlan brief
```

```
show vlan
```

## Display switchport information

```
show interfaces switchport
```

## Display a specific port's switchport information

```
show interfaces gigabitEthernet 0/1 switchport
```

---

# 7. Trunking

## Configure a trunk

```
interface gigabitEthernet 0/1
switchport mode trunk
```

## Configure a trunk on a switch that supports multiple encapsulations

Some Layer 3-capable switches (for example the 3560 in Packet Tracer) require the encapsulation to be set first, otherwise `switchport mode trunk` is rejected. 2960-series switches do not need this.

```
interface gigabitEthernet 0/1
switchport trunk encapsulation dot1q
switchport mode trunk
```

## Configure the native VLAN

```
switchport trunk native vlan 99
```

## Configure allowed VLANs

```
switchport trunk allowed vlan 10,20,30
```

## Add a VLAN to the allowed list

```
switchport trunk allowed vlan add 40
```

## Remove a VLAN from the allowed list

```
switchport trunk allowed vlan remove 40
```

## Allow all VLANs

```
switchport trunk allowed vlan all
```

## Remove the allowed VLAN restriction

```
no switchport trunk allowed vlan
```

## Verify trunks

```
show interfaces trunk
```

```
show interfaces switchport
```

---

# 8. Inter-VLAN Routing

## Router-on-a-Stick

Configure the physical router interface (no IP address on the parent interface):

```
interface gigabitEthernet 0/0
no shutdown
```

Configure VLAN 10:

```
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

Configure VLAN 20:

```
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

## Native VLAN subinterface

```
interface gigabitEthernet 0/0.99
encapsulation dot1Q 99 native
ip address 192.168.99.1 255.255.255.0
```

## SVI on a Layer 3 switch

```
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown
```

> An SVI only comes up if the VLAN exists and at least one port in that VLAN is up.

## Enable Layer 3 routing on a switch

```
ip routing
```

## Configure a Layer 3 physical interface

```
interface gigabitEthernet 0/1
no switchport
ip address 10.0.0.1 255.255.255.252
no shutdown
```

## Configure IPv6 on a Layer 3 interface

```
interface gigabitEthernet 0/1
no switchport
ipv6 address 2001:DB8:1::1/64
no shutdown
```

> Remember `ipv6 unicast-routing` in global configuration if the device should route IPv6.

---

# 9. CDP and LLDP

## Enable CDP globally

```
cdp run
```

## Disable CDP globally

```
no cdp run
```

## Enable CDP on an interface

```
interface gigabitEthernet 0/1
cdp enable
```

## Disable CDP on an interface

```
no cdp enable
```

## Verify CDP neighbors

```
show cdp neighbors
```

## Display detailed CDP information

```
show cdp neighbors detail
```

## Display CDP interface information

```
show cdp interface
```

## Enable LLDP

```
lldp run
```

## Configure LLDP transmit/receive

```
interface gigabitEthernet 0/1
lldp transmit
lldp receive
```

## Verify LLDP

```
show lldp
```

```
show lldp neighbors
```

```
show lldp neighbors detail
```

> LLDP support in Packet Tracer is limited or absent depending on the version. Test on your version.

---

# 10. EtherChannel and LACP

## Configure LACP

```
interface range gigabitEthernet 0/1-2
channel-group 1 mode active
```

`active` actively negotiates LACP.

A passive LACP side:

```
channel-group 1 mode passive
```

> At least one side must actively initiate LACP (active-active and active-passive work; passive-passive does not form a channel). Member ports must have matching speed, duplex, and VLAN/trunk settings.

## Configure a Port-Channel as a trunk

```
interface port-channel 1
switchport mode trunk
```

## Configure allowed VLANs on the Port-Channel

```
interface port-channel 1
switchport trunk allowed vlan 10,20,30
```

## Access EtherChannel

```
interface range gigabitEthernet 0/1-2
switchport mode access
switchport access vlan 10
channel-group 1 mode active
```

## Verify EtherChannel

```
show etherchannel summary
```

```
show etherchannel detail
```

```
show etherchannel port-channel
```

```
show interfaces port-channel 1
```

## Display LACP neighbors

```
show lacp neighbor
```

---

# 11. STP and Rapid PVST+

## Enable Rapid PVST+

```
spanning-tree mode rapid-pvst
```

## Configure a switch as root primary

```
spanning-tree vlan 10 root primary
```

## Configure a switch as root secondary

```
spanning-tree vlan 10 root secondary
```

## Explicitly configure STP priority

```
spanning-tree vlan 10 priority 24576
```

Lower priority wins the root bridge election. Priority must be a multiple of 4096.

## Configure PortFast

```
interface gigabitEthernet 0/1
spanning-tree portfast
```

## Enable BPDU Guard

```
interface gigabitEthernet 0/1
spanning-tree bpduguard enable
```

## Configure root guard

```
interface gigabitEthernet 0/1
spanning-tree guard root
```

## Configure loop guard

```
interface gigabitEthernet 0/1
spanning-tree guard loop
```

## Verify STP

```
show spanning-tree
```

```
show spanning-tree vlan 10
```

```
show spanning-tree root
```

```
show spanning-tree summary
```

---

# 12. Static Routing

## IPv4 network route (next-hop)

```
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## Static route using an exit interface

```
ip route 192.168.20.0 255.255.255.0 gigabitEthernet 0/1
```

> Fine on point-to-point links (serial). On Ethernet (multi-access) links this makes the router ARP for every destination, so prefer a next-hop or next-hop plus exit interface.

## Static route using next-hop and exit interface

```
ip route 192.168.20.0 255.255.255.0 gigabitEthernet 0/1 10.0.0.2
```

## IPv4 default route

```
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

## IPv4 host route

```
ip route 192.168.20.10 255.255.255.255 10.0.0.2
```

## Floating static route

A higher administrative distance makes it a backup route.

```
ip route 192.168.20.0 255.255.255.0 10.0.0.2 200
```

## Remove a static route

```
no ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## IPv6 static route

Requires `ipv6 unicast-routing`.

```
ipv6 route 2001:DB8:20::/64 2001:DB8:1::2
```

## IPv6 default route

```
ipv6 route ::/0 2001:DB8:1::2
```

## IPv6 host route

```
ipv6 route 2001:DB8:20::10/128 2001:DB8:1::2
```

## IPv6 floating static route

```
ipv6 route 2001:DB8:20::/64 2001:DB8:1::2 200
```

## Verify routing

```
show ip route
```

```
show ip route static
```

```
show ipv6 route
```

```
show ipv6 route static
```

---

# 13. OSPFv2

## Start OSPF

```
router ospf 1
```

## Configure a router ID

```
router-id 1.1.1.1
```

> If you change the router ID on a running process, restart it with `clear ip ospf process` for it to take effect.

## Advertise a network

```
network 192.168.1.0 0.0.0.255 area 0
```

## Configure OSPF directly on an interface

```
interface gigabitEthernet 0/0
ip ospf 1 area 0
```

## Configure a passive interface

```
passive-interface gigabitEthernet 0/1
```

## Make all interfaces passive

```
passive-interface default
```

## Make a specific interface active again

```
no passive-interface gigabitEthernet 0/0
```

## Configure OSPF cost

```
interface gigabitEthernet 0/0
ip ospf cost 10
```

## Configure OSPF interface priority

```
interface gigabitEthernet 0/0
ip ospf priority 100
```

## Advertise a default route through OSPF

```
default-information originate
```

## Restart the OSPF process

```
clear ip ospf process
```

## Verify OSPF configuration

```
show ip protocols
```

```
show ip ospf
```

```
show ip ospf interface
```

```
show ip ospf neighbor
```

```
show ip route ospf
```

---

# 14. DHCP

## Configure an interface as a DHCP client

```
interface gigabitEthernet 0/0
ip address dhcp
no shutdown
```

## Configure a DHCP relay

```
interface gigabitEthernet 0/0
ip helper-address 192.168.1.10
```

## Remove a DHCP relay

```
no ip helper-address 192.168.1.10
```

## Configure a DHCP server

Exclude addresses:

```
ip dhcp excluded-address 192.168.10.1 192.168.10.20
```

Create a DHCP pool:

```
ip dhcp pool LAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
domain-name example.local
```

## Configure a DHCP lease

The lease time is in days.

```
ip dhcp pool LAN10
lease 7
```

## Verify DHCP pools

```
show ip dhcp pool
```

## Display DHCP bindings

```
show ip dhcp binding
```

## Display DHCP conflicts

```
show ip dhcp conflict
```

## Display DHCP-related interface information

```
show ip interface
```

---

# 15. NAT and PAT

## Configure the inside interface

```
interface gigabitEthernet 0/0
ip nat inside
```

## Configure the outside interface

```
interface gigabitEthernet 0/1
ip nat outside
```

## Static NAT

```
ip nat inside source static 192.168.1.10 203.0.113.10
```

## Dynamic NAT pool

Create the matching ACL:

```
access-list 1 permit 192.168.1.0 0.0.0.255
```

Create the public pool:

```
ip nat pool PUBLIC 203.0.113.10 203.0.113.20 netmask 255.255.255.0
```

Connect the ACL to the pool:

```
ip nat inside source list 1 pool PUBLIC
```

## PAT using the outside interface

```
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```

## Verify NAT translations

```
show ip nat translations
```

## Verify NAT statistics

```
show ip nat statistics
```

## Clear NAT translations

```
clear ip nat translation *
```

---

# 16. Access Control Lists

## Standard numbered ACL

```
access-list 10 permit 192.168.1.0 0.0.0.255
```

## Permit a specific host

```
access-list 10 permit host 192.168.1.10
```

## Deny a network

```
access-list 10 deny 192.168.1.0 0.0.0.255
```

## Permit everything

```
access-list 10 permit any
```

## Deny everything

```
access-list 10 deny any
```

> Every ACL ends with an implicit `deny any`, so an ACL that only has deny statements blocks everything. Add `permit any` where needed.

## Apply a standard ACL

```
interface gigabitEthernet 0/0
ip access-group 10 in
```

> Standard ACLs filter by source only, so they are normally placed as close to the destination as possible.

## Remove an ACL from an interface

```
interface gigabitEthernet 0/0
no ip access-group 10 in
```

## Named standard ACL

```
ip access-list standard BLOCK-LAN
deny 192.168.1.0 0.0.0.255
permit any
exit
```

## Extended ACL

Permit all IP traffic:

```
access-list 100 permit ip 192.168.1.0 0.0.0.255 any
```

Permit TCP:

```
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any
```

Permit HTTP:

```
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
```

Permit HTTPS:

```
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
```

Permit SSH:

```
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 22
```

Permit DNS:

```
access-list 100 permit udp 192.168.1.0 0.0.0.255 any eq 53
```

Permit ICMP:

```
access-list 100 permit icmp 192.168.1.0 0.0.0.255 any
```

## Named extended ACL

```
ip access-list extended WEB-ONLY
permit tcp 192.168.1.0 0.0.0.255 any eq 80
permit tcp 192.168.1.0 0.0.0.255 any eq 443
deny ip any any
exit
```

## Apply an extended ACL

```
interface gigabitEthernet 0/0
ip access-group 100 in
```

> Extended ACLs filter by source, destination, protocol, and port, so they are normally placed as close to the source as possible.

## Remove an ACL

```
no access-list 100
```

## IPv6 ACL

Create the ACL:

```
ipv6 access-list V6-FILTER
permit ipv6 2001:DB8:1::/64 any
permit icmp any any
deny ipv6 any any
exit
```

> IPv6 ACLs have implicit permits for Neighbor Discovery before the implicit deny. An explicit `deny ipv6 any any` removes that protection, so keep `permit icmp any any` (or explicit ND permits) or neighbor discovery will break.

Apply it:

```
interface gigabitEthernet 0/0
ipv6 traffic-filter V6-FILTER in
```

Remove it:

```
interface gigabitEthernet 0/0
no ipv6 traffic-filter V6-FILTER in
```

## Verify ACLs

```
show access-lists
```

```
show ip access-lists
```

```
show ipv6 access-list
```

```
show running-config
```

---

# 17. Port Security

## Enable port security

```
interface gigabitEthernet 0/1
switchport mode access
switchport port-security
```

## Limit the number of MAC addresses

```
switchport port-security maximum 2
```

## Configure a secure MAC address

```
switchport port-security mac-address 0011.2233.4455
```

## Enable sticky MAC addresses

```
switchport port-security mac-address sticky
```

## Configure violation mode

Protect:

```
switchport port-security violation protect
```

Restrict:

```
switchport port-security violation restrict
```

Shutdown:

```
switchport port-security violation shutdown
```

## Verify port security

```
show port-security
```

```
show port-security interface gigabitEthernet 0/1
```

## Recover a shutdown (err-disabled) port

```
interface gigabitEthernet 0/1
shutdown
no shutdown
```

---

# 18. DHCP Snooping

## Enable DHCP snooping

```
ip dhcp snooping
```

## Enable it for a VLAN

```
ip dhcp snooping vlan 10
```

## Trust the DHCP server/uplink interface

```
interface gigabitEthernet 0/1
ip dhcp snooping trust
```

## Limit DHCP messages on an untrusted port

```
interface gigabitEthernet 0/2
ip dhcp snooping limit rate 10
```

## Verify DHCP snooping

```
show ip dhcp snooping
```

```
show ip dhcp snooping binding
```

---

# 19. Dynamic ARP Inspection

> DAI validates ARP packets against the DHCP snooping binding table. Enable DHCP snooping (section 18) first. Hosts with static IP addresses have no binding and will have their ARP dropped on untrusted ports unless you add an ARP ACL for them.

## Enable DAI for a VLAN

```
ip arp inspection vlan 10
```

## Trust an uplink

```
interface gigabitEthernet 0/1
ip arp inspection trust
```

## Limit ARP traffic

```
interface gigabitEthernet 0/2
ip arp inspection limit rate 15
```

## Verify DAI

```
show ip arp inspection
```

```
show ip arp inspection interfaces
```

```
show ip arp inspection statistics
```

---

# 20. IPv6 RA Guard

## Enable RA Guard on an interface

```
interface gigabitEthernet 0/1
ipv6 nd raguard
```

## Remove RA Guard

```
interface gigabitEthernet 0/1
no ipv6 nd raguard
```

> Exact IPv6 RA Guard support and syntax vary by IOS release and platform. Many Catalyst releases use a policy-based form instead: define a policy with `ipv6 nd raguard policy NAME`, then apply it on the interface with `ipv6 nd raguard attach-policy NAME`. Packet Tracer support is limited. Always check with `?` on your actual device.

---

# 21. SSH

> Before generating RSA keys the device needs **both** a non-default hostname and a domain name, otherwise key generation is refused.

## Configure the hostname

```
hostname R1
```

## Configure the domain name

```
ip domain name example.local
```

## Create a local user

```
username admin privilege 15 secret MyPassword
```

## Generate RSA keys

```
crypto key generate rsa modulus 2048
```

## Enable SSH version 2

```
ip ssh version 2
```

## Configure VTY lines to use the local database

```
line vty 0 4
login local
transport input ssh
exit
```

## Configure SSH timeout

```
ip ssh time-out 60
```

## Configure SSH authentication retries

```
ip ssh authentication-retries 3
```

## Verify SSH

```
show ip ssh
```

```
show users
```

```
show running-config
```

## Connect to another device using SSH

```
ssh -l admin 192.168.1.1
```

---

# 22. AAA and Authentication

> **Lockout warning:** create a local user *before* enabling `aaa new-model`, and keep a console session open while you test. Enabling AAA without a working method list or user can lock you out of the device.

## Create a local administrative account

```
username admin privilege 15 secret MyPassword
```

## Enable AAA

```
aaa new-model
```

## Use the local database for login authentication

```
aaa authentication login default local
```

## Apply the AAA method list to VTY lines

```
line vty 0 4
login authentication default
```

> The `default` method list is applied to all lines automatically; naming it explicitly is optional. Custom-named lists must be applied with `login authentication LISTNAME`.

## Local authentication without AAA

```
line vty 0 4
login local
```

> `login local` uses the local username database. AAA provides a more general authentication, authorization, and accounting framework.

---

# 23. NTP

## Configure an NTP client

```
ntp server 192.168.1.10
```

## Configure multiple NTP servers

```
ntp server 192.168.1.10
ntp server 192.168.1.11
```

## Configure a device as an NTP master

```
ntp master 3
```

## Verify the clock

```
show clock
```

## Verify NTP status

```
show ntp status
```

## Display NTP associations

```
show ntp associations
```

---

# 24. DNS

## Enable DNS lookup

```
ip domain lookup
```

## Disable DNS lookup

```
no ip domain lookup
```

> Older IOS releases and some Packet Tracer versions use the hyphenated form: `ip domain-lookup` / `no ip domain-lookup`.

## Configure a DNS server

```
ip name-server 8.8.8.8
```

Multiple servers:

```
ip name-server 8.8.8.8 1.1.1.1
```

## Display cached host information

```
show hosts
```

## Test DNS resolution

```
ping example.com
```

---

# 25. Syslog

## Configure a remote syslog server

```
logging host 192.168.1.10
```

## Configure the logging severity level

```
logging trap warnings
```

Common severity levels:

```
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

```
logging console warnings
```

## Configure buffered logging

```
logging buffered 16384
```

## Add timestamps to log messages

```
service timestamps log datetime msec
```

## Verify logging

```
show logging
```

---

# 26. SNMP

## Configure a read-only SNMP community

```
snmp-server community public ro
```

## Configure a read-write community

```
snmp-server community private rw
```

## Verify SNMP

```
show snmp
```

> SNMP community strings are credentials. Avoid using simple/default community strings (and avoid `rw`) in real production networks.

---

# 27. TFTP and Configuration Transfer

## Copy the running configuration to a TFTP server

```
copy running-config tftp:
```

IOS will prompt for:

```
Address or name of remote host
Destination filename
```

## Copy startup configuration to a TFTP server

```
copy startup-config tftp:
```

## Restore a configuration from TFTP

```
copy tftp: running-config
```

or:

```
copy tftp: startup-config
```

## Copy an IOS image from flash to TFTP

```
copy flash: tftp:
```

## Copy an IOS image from TFTP to flash

```
copy tftp: flash:
```

## Display flash contents

```
show flash:
```

or:

```
dir flash:
```

---

# 28. Device Verification

## General device information

```
show version
```

## Running configuration

```
show running-config
```

## Startup configuration

```
show startup-config
```

## Interface summary

```
show ip interface brief
```

## Detailed interfaces

```
show interfaces
```

## VLANs

```
show vlan brief
```

## Trunks

```
show interfaces trunk
```

## MAC address table

```
show mac address-table
```

## Specific MAC address

```
show mac address-table address 0011.2233.4455
```

## ARP table

```
show ip arp
```

## IPv4 routing table

```
show ip route
```

## IPv6 routing table

```
show ipv6 route
```

## OSPF neighbors

```
show ip ospf neighbor
```

## OSPF routes

```
show ip route ospf
```

## EtherChannel

```
show etherchannel summary
```

## STP

```
show spanning-tree
```

## CDP neighbors

```
show cdp neighbors
```

## LLDP neighbors

```
show lldp neighbors
```

## Port security

```
show port-security
```

## DHCP snooping

```
show ip dhcp snooping
```

## DAI

```
show ip arp inspection
```

## ACLs

```
show access-lists
```

## NAT

```
show ip nat translations
```

```
show ip nat statistics
```

## SSH

```
show ip ssh
```

## Users currently connected

```
show users
```

## Logging

```
show logging
```

## NTP

```
show ntp status
```

```
show ntp associations
```

---

# 29. Troubleshooting

## Test basic connectivity

```
ping 192.168.1.1
```

## Trace the path to a destination

```
traceroute 192.168.1.1
```

> On a Windows PC the equivalent command is `tracert`. Use `Ctrl+Shift+6` to interrupt a traceroute on a Cisco device.

## Check interface status

```
show ip interface brief
```

Look for:

```
up    up
```

If an interface is:

```
administratively down
```

enable it:

```
interface gigabitEthernet 0/0
no shutdown
```

## Check detailed interface errors

```
show interfaces gigabitEthernet 0/0
```

Useful counters include:

- input errors
- CRC
- collisions
- drops
- output errors

## Check VLAN assignment

```
show vlan brief
```

## Check trunking

```
show interfaces trunk
```

## Check switchport configuration

```
show interfaces gigabitEthernet 0/1 switchport
```

## Check MAC addresses

```
show mac address-table
```

## Check ARP

```
show ip arp
```

## Check routing

```
show ip route
```

## Check IPv6 routing

```
show ipv6 route
```

## Check OSPF neighbors

```
show ip ospf neighbor
```

## Check OSPF configuration

```
show ip protocols
```

## Check EtherChannel

```
show etherchannel summary
```

Look for correctly bundled ports.

## Check STP

```
show spanning-tree
```

## Check ACLs

```
show access-lists
```

## Check ACL application

```
show running-config interface gigabitEthernet 0/0
```

or:

```
show ip interface gigabitEthernet 0/0
```

## Check NAT

```
show ip nat translations
```

```
show ip nat statistics
```

## Clear ARP cache

```
clear arp-cache
```

## Clear interface counters

```
clear counters
```

## Reset an interface

```
interface gigabitEthernet 0/0
shutdown
no shutdown
```

## Reset an interface configuration

```
default interface gigabitEthernet 0/0
```

> Be careful with `default interface` because it removes the interface's current configuration.

---

# 30. Common Configuration Workflows

## Basic Router Configuration

```
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

```
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

```
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

```
enable
configure terminal

interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit

end
copy running-config startup-config
```

> On switches that need it (for example the 3560), add `switchport trunk encapsulation dot1q` before `switchport mode trunk` on both sides.

Verify:

```
show interfaces trunk
```

---

## SSH Configuration

```
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

```
ssh -l admin 192.168.1.1
```

---

## OSPF Configuration

Router 1:

```
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

```
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

```
show ip ospf neighbor
show ip route ospf
```

---

## NAT/PAT Configuration

Inside interface:

```
interface gigabitEthernet 0/0
ip nat inside
```

Outside interface:

```
interface gigabitEthernet 0/1
ip nat outside
```

Create the matching ACL:

```
access-list 1 permit 192.168.1.0 0.0.0.255
```

Enable PAT:

```
ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```

Verify:

```
show ip nat translations
show ip nat statistics
```

---

## Port Security Configuration

```
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

```
show port-security interface gigabitEthernet 0/1
```

---

# 31. Quick Reference

## Modes

```
enable
configure terminal
interface g0/0
exit
end
```

## Save

```
copy running-config startup-config
```

## Interface

```
interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```

## VLAN

```
vlan 10
name USERS
```

## Access port

```
interface g0/1
switchport mode access
switchport access vlan 10
```

## Trunk

```
interface g0/1
switchport mode trunk
```

(Add `switchport trunk encapsulation dot1q` first on switches that require it.)

## Router-on-a-Stick

```
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

## Static route

```
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

## Default route

```
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

## OSPF

```
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
```

## DHCP relay

```
interface g0/0
ip helper-address 192.168.1.10
```

## NAT/PAT

```
ip nat inside
ip nat outside
ip nat inside source list 1 interface g0/1 overload
```

## Standard ACL

```
access-list 10 permit 192.168.1.0 0.0.0.255
```

## Extended ACL

```
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
```

## Port Security

```
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
```

## DHCP Snooping

```
ip dhcp snooping
ip dhcp snooping vlan 10
```

## DAI

```
ip arp inspection vlan 10
```

## SSH

```
hostname R1
ip domain name example.local
username admin privilege 15 secret MyPassword
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
login local
transport input ssh
```

## Verification

```
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
- Not every feature here is supported in Packet Tracer (for example LLDP and IPv6 RA Guard are limited or absent depending on the version).
- Commands that modify routing, security, NAT, ACLs, STP, AAA, or interface configuration should be tested carefully before using them on production equipment.
- This file is intended as a practical CCNA-level Cisco IOS / Packet Tracer reference, not an exhaustive Cisco IOS command encyclopedia.
