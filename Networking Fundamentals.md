
Networking Fundamentals
How networking works, from a single cable to a web server, with the security basics that go with it. This guide assumes you know nothing about networks. It starts with stories and everyday examples, then gets more detailed section by section, so you never get dropped into the deep end.
> **Companion file:** [CCNA-Configuration-Commands.md](CCNA-Configuration-Commands.md) has the Cisco IOS commands that implement the concepts explained here.
How to read this guide
Read the sections in order. Each one builds on the one before.
Boxes marked 🔍 Going deeper hold extra detail. Skip them on your first read and come back later. Nothing in the main text depends on them.
Every section ends with a Self-check. Try to answer before opening the answer.
New words are explained the first time they appear and collected in a Key terms table at the end of each section.
Outline
[x] 1. The Big Picture: what a network is and how data travels
[x] 2. The Layers: OSI model, TCP/IP model, encapsulation
[x] 3. Addressing: MAC addresses, IPv4, subnetting, IPv6
[x] 4. The Local Network: switches, ARP, VLANs, STP, Wi-Fi
[x] 5. Between Networks: routing, gateways, NAT
[x] 6. Core Services: DHCP, DNS, HTTP/HTTPS, how servers work
[x] 7. Security Fundamentals: threats, firewalls, encryption, and the defenses against them
---
1. The Big Picture
What is a network?
A network is two or more devices connected together so they can share information. That's the whole definition.
Your phone and your home Wi-Fi router form a tiny network.
All the computers in a school form a bigger one.
Millions of networks connected to each other form the internet.
What gets shared? Messages, photos, web pages, video calls, files, even a printer. Every time you do something online, data is moving from one device to another. This guide is about how that happens.
A story: mailing a LEGO castle
Before any technical terms, imagine this.
You built a huge LEGO castle and want to mail it to your friend Sara, who lives in another country. It's far too big for one box, so you:
Take it apart and pack the pieces into many small boxes.
Number each box: "box 1 of 10", "box 2 of 10", and so on.
Write Sara's address on every box, plus your own return address.
Hand the boxes to the post office.
Now the boxes travel. At each post office or sorting hub, workers look only at the address and decide which way to send each box next. Some boxes may take different routes. Some arrive early, some late, so they may arrive out of order.
When the boxes reach Sara, she uses the numbers to put them in order and rebuilds the castle. If box 7 never shows up, she tells you "box 7 is missing", and you send it again.
That story is almost exactly how networks work. Here's the translation:
In the story	In networking	Technical name
The LEGO castle	The data you want to send (a photo, a web page)	Data / message
Small numbered boxes	Small pieces of data	Packets
The labels on each box	Information added to each piece: who it's from, who it's for, which piece it is	Headers
Sara's street address	The unique address of the destination device	IP address
Post offices and sorting hubs	Devices that look at the address and pick the next direction	Routers
Roads, planes, trucks	The actual connection carrying the data	Cables, fiber, Wi-Fi
Sara putting boxes in order	The receiving device reassembling the pieces	Reassembly (using sequence numbers)
"Box 7 is missing" → resend	Detecting lost data and sending it again	Retransmission
Both of you agreeing how it works	A shared set of rules	Protocols
Keep this story in mind. Every concept in this guide is a more detailed version of one part of it.
Core idea 1: everything is bits
Computers don't understand letters, photos, or music. They only understand bits: tiny switches that are either 0 or 1. Eight bits make a byte.
Everything you send is converted into bits first:
The letter "A" becomes `01000001`.
A photo becomes millions of 0s and 1s describing the color of each dot.
A video is the same thing, many times per second.
A 3 MB photo is roughly 24 million bits.
Then the bits have to physically travel. How a 0 or 1 becomes something that can move depends on the connection:
Connection	How bits are carried
Copper cable (Ethernet)	Changes in electrical voltage
Fiber-optic cable	Flashes of light
Wi-Fi / mobile data	Changes in radio waves
The receiving device reads those changes and turns them back into 0s and 1s.
Core idea 2: data is split into packets
Instead of sending a whole photo as one giant block, networks chop it into small pieces called packets. Just like the LEGO boxes, this has real benefits:
Fair sharing. A big file doesn't hog the line. Packets from many people take turns, so everyone gets a turn.
Cheap fixes. If one packet is lost, only that small piece is resent, not the whole photo.
Flexible routes. If one path is broken or busy, packets can take another.
Each packet carries a header: extra information stuck on the front, like the label on the box. The header says who sent it, who it's for, and which piece it is.
> **🔍 Going deeper:** Most networks limit a packet to about 1,500 bytes. So a 3 MB photo becomes roughly 2,000 packets. That sounds like a lot, but your device handles it in a fraction of a second.
Core idea 3: everything needs an address
For data to reach the right place, every device needs an address, just like a house. You'll meet several kinds, and each does a different job:
IP address: identifies a device on a network, anywhere in the world. Looks like `192.168.1.10`. This is the "street address" from our story.
MAC address: identifies a device's network hardware. Used for delivery inside a single local network. Looks like `00:1A:2B:3C:4D:5E`.
Port number: identifies which program on a device should receive the data. If the IP address is the street address of an apartment building, the port is the apartment number.
For now, just remember: addresses tell the network where data should go. Section 3 explains them properly.
Core idea 4: protocols are the rules
Two devices can only communicate if they follow the same rules. Those rules are called protocols. A protocol defines things like: what a message must look like, who speaks first, how to say "I got it", and what to do if something goes wrong.
Think of a phone call. There's a protocol: you say "hello", the other person says "hello", you talk, you say "bye". If one of you spoke a different language, the call would fail.
Some protocols you'll meet in this guide:
Protocol	What it does
IP (Internet Protocol)	Addresses packets and gets them across networks
TCP	Makes sure data arrives complete and in order
HTTP / HTTPS	How web browsers and web servers talk
DNS	Turns names like `example.com` into IP addresses
DHCP	Automatically gives devices an IP address when they join a network
ARP	Finds a device's MAC address when you know its IP address
You don't need to understand them yet. Just know that each one is a set of rules for one specific job.
Speed: bandwidth and latency
When people say a connection is "fast", they could mean two different things:
Bandwidth: how much data can be sent per second (measured in bits per second, like Mbps). Think of the width of a pipe: wider pipe, more water at once.
Latency: how long it takes for data to get there (measured in milliseconds, ms). Think of the length of the pipe: shorter pipe, water arrives sooner.
You can have high bandwidth and still high latency. Satellite internet can download large files quickly, yet still feels laggy in a video game, because each packet has a long way to travel.
Who's who: the devices in a network
Device	What it is	Everyday example
End device (host)	Any device that sends or receives data for a person or program	Phone, laptop, game console, smart TV, printer
Client	A device (or program) that asks for something	Your browser asking for a web page
Server	A computer running software that waits for requests and answers them	The machine that stores a website
Switch	Connects devices inside one network and forwards data to the right one	The box that the office computers all plug into
Router	Connects different networks and chooses the path between them	The device that links your home to the internet
Access point	Lets wireless devices join a wired network	The Wi-Fi part of your router
Modem	Converts between your internet provider's signal and one your router understands	The box from your internet provider
Firewall	Allows or blocks traffic based on rules	Protects a network from unwanted traffic
A couple of important notes:
"Server" doesn't mean a special, powerful machine. It's a role. Any computer running server software is a server. Your laptop can be a server too. The big ones in data centers are just built to handle many requests at once.
Your home "router" is usually several devices in one box. It's typically a modem, a router, a switch (the Ethernet ports), and a Wi-Fi access point all in one. They're separate jobs, which is why we learn them separately.
How big is a network? LAN, WAN, and the internet
Term	Meaning	Example
LAN (Local Area Network)	A network in one limited area, usually owned by one person or organization	Your home, a school, an office
WAN (Wide Area Network)	A network that connects LANs across long distances	A company linking offices in different cities
Internet	The biggest network: millions of LANs and WANs connected together	Everything
ISP (Internet Service Provider)	A company that connects you to the internet	Your home internet company
The internet isn't one giant network. It's a network of networks, joined by routers.
The whole story: what happens when you open a website
Here's the full journey in plain words, from typing a website name to seeing the page. Don't worry about the terms. Every step gets its own section later.
You type `example.com`. Your browser knows the name but not where the server is. It asks a DNS server, a kind of internet phone book, and gets back an IP address. (Section 6)
The browser creates a request that basically says "please send me this page". (Section 6)
The request is split into packets, and each gets a header with your IP address (the sender) and the server's IP address (the destination). (Section 2)
The packets leave your device over Wi-Fi or a cable and arrive at your router, your gateway to the outside world. (Sections 4 and 5)
The router passes them on to your ISP, and from there they hop through many more routers. Each router looks only at the destination address and decides the next hop. (Section 5)
The packets reach the server. Server software reads the request, builds a response (the web page), and sends it back as packets, the same way. (Section 6)
The packets arrive back at your device, possibly by a different route and in a different order. Your device puts them back in order. (Section 2)
Your browser draws the page.
All of this takes a fraction of a second.
Key terms
Term	Meaning
Network	Two or more devices connected to share data
Bit / byte	A single 0 or 1 / eight bits
Packet	A small piece of data with a header attached
Header	Extra information on the front of a packet (sender, destination, etc.)
IP address	The address of a device on a network
Protocol	A set of rules two devices follow to communicate
Client / server	The one asking for data / the one answering
Switch	Connects devices inside one network
Router	Connects different networks
LAN / WAN	Local area network / wide area network
Bandwidth / latency	How much data per second / how long data takes to arrive
Self-check
Try to answer first, then open the answer.
1. Why are big files split into small packets instead of being sent as one piece?
<details>
<summary>Answer</summary>
Splitting lets many users share the line fairly, means only a lost packet has to be resent (not the whole file), and lets packets take different routes if one path is busy or broken.
</details>
2. In the LEGO story, what does the post office represent, and what does it look at to decide where a box goes next?
<details>
<summary>Answer</summary>
It represents a router. It looks only at the destination address on the label (the IP address in a real network).
</details>
3. What's the difference between a switch and a router?
<details>
<summary>Answer</summary>
A switch connects devices inside one network. A router connects different networks together and chooses the path between them.
</details>
4. Satellite internet downloads big files quickly but feels laggy in games. Is the problem bandwidth or latency?
<details>
<summary>Answer</summary>
Latency. Bandwidth (how much data per second) is fine, but each packet takes a long time to travel, which hurts real-time things like games.
</details>
5. Is a "server" a special kind of hardware?
<details>
<summary>Answer</summary>
No. A server is a role: any computer running software that waits for requests and answers them.
</details>
---
2. The Layers
In Section 1 you saw that sending data involves many different jobs: turning bits into signals, finding the right device, crossing many networks, making sure nothing is lost, and giving the data to the right program. Networking handles this by splitting the work into layers.
Why layers? The postal system again
Think about how a letter gets delivered:
You write the letter. You only care about what you want to say.
You put it in an envelope and write the address. You only care about where it's going.
The post office sorts it and decides which hub to send it to. It only cares about the address, not what's inside.
A truck or plane physically carries it. The driver only cares about getting the bag to the next stop.
Each person does one job and doesn't need to understand the others. You don't need to know how planes work to mail a letter, and the pilot never reads your letter.
Networking layers work the same way. Each layer:
does one job,
uses the services of the layer below it,
provides a service to the layer above it.
This design has big benefits:
Simpler. Each layer is a smaller problem.
Swappable. You can switch from Wi-Fi to a cable without changing your browser, because the layers are independent.
Compatible. Different companies can build different layers that still work together, since they all follow the same standards.
Easier to fix. When something breaks, you can ask "which layer is the problem?"
Four jobs: the TCP/IP model
The internet runs on a model with four layers. Here they are in plain words, from top to bottom:
Layer	Its job	Our story
4. Application	Create and understand the actual message	Writing the letter
3. Transport	Make sure the message reaches the right program, and decide how reliably	Choosing "registered mail" or "regular mail"
2. Internet	Get the packet across networks to the right device	The postal system routing between cities
1. Network Access	Move the data over the physical link to the next device	The truck driving to the next stop
Now let's look at each one more closely.
Application layer: the message itself
This is where programs live: your browser, email app, messaging app. The application layer defines how programs talk to each other using protocols like:
HTTP / HTTPS: web pages
DNS: turning names into IP addresses
SMTP / IMAP: sending and receiving email
SSH: securely controlling another computer remotely
FTP: transferring files
Transport layer: the right program, delivered the right way
An IP address gets data to the right device, but a device runs many programs at once: a browser, a music app, a game. How does the data know which program it's for? With port numbers.
Think of the device as an apartment building. The IP address is the building's address, and the port is the apartment number. A web server listens on port 443; an SSH server listens on port 22.
Some common ports worth memorizing:
Port	Protocol	Used for
20 / 21	FTP	File transfer
22	SSH	Secure remote login
23	Telnet	Remote login (insecure, sends everything in plain text)
25	SMTP	Sending email
53	DNS	Name lookups
67 / 68	DHCP	Automatic IP addressing
80	HTTP	Web (unencrypted)
443	HTTPS	Web (encrypted)
The transport layer also decides how carefully to deliver, with two main protocols:
	TCP	UDP
Style	Like registered mail with tracking	Like dropping postcards in a mailbox
Delivery	Guaranteed: lost data is resent	Not guaranteed
Order	Pieces are put back in order	No ordering
Speed	Slower (more checking)	Faster (less overhead)
Used for	Web pages, email, file transfers	Video calls, online games, live streams, DNS lookups
If one frame of a live video call is lost, there's no point resending it, because the moment has passed. So UDP is a better fit. If one piece of a downloaded file is lost, the file is broken, so TCP is the right choice.
> **🔍 Going deeper: the TCP three-way handshake.** Before sending data, TCP sets up a connection with a quick three-step "hello":
> 1. Client → server: **SYN** ("I'd like to talk")
> 2. Server → client: **SYN-ACK** ("OK, I'm ready")
> 3. Client → server: **ACK** ("Great, starting now")
>
> Only then does the real data flow. Attackers abuse this on purpose: a **SYN flood** sends thousands of step-1 messages without ever finishing, which keeps the server busy waiting.
Internet layer: across networks to the right device
This layer's job is getting a packet from one device to another even when they're on different networks, possibly on opposite sides of the planet. It uses:
IP addresses to identify the source and destination.
Routers, which read the destination IP address and choose the next hop.
The main protocol is IP. A helper protocol, ICMP, carries error and test messages: it's what `ping` uses.
IP is "best effort": it tries to deliver packets but makes no guarantees by itself. Making delivery reliable is the transport layer's job (TCP).
> **🔍 Going deeper: TTL.** Every IP packet has a **Time To Live** number. Each router that handles the packet reduces it by 1. If it reaches 0, the packet is thrown away. This stops packets from circling the internet forever if there's a routing mistake. The `traceroute` tool cleverly uses this to discover every router on the path.
Network Access layer: the next hop
Everything above is about logical addressing. This layer handles the physical reality: moving bits across one link to the next device. It covers:
MAC addresses for delivery inside the local network
Ethernet (cables) and Wi-Fi (radio)
Frames, the packaging used on a single link
The actual signals: voltage, light, or radio
Notice the keyword: next device. This layer only gets data across one hop. The Internet layer above it thinks about the whole journey.
The OSI model: the same idea, in seven slices
You'll often hear about the OSI model with 7 layers. It describes the same ideas but slices them more finely. Real networks run on TCP/IP, but the OSI numbers are the common language of networking ("that's a Layer 2 problem", "a Layer 7 firewall").
#	OSI layer	Job	Examples	Data unit
7	Application	The interface programs use to talk to the network	HTTP, DNS, SSH, SMTP	Data
6	Presentation	Data format, encryption, compression	TLS (loosely), JPEG	Data
5	Session	Starting, maintaining, and ending conversations	Session setup	Data
4	Transport	Delivery between programs, using ports	TCP, UDP	Segment (TCP) / Datagram (UDP)
3	Network	Logical addressing and routing between networks	IP, ICMP	Packet
2	Data Link	Delivery on the local network, using MAC addresses	Ethernet, Wi-Fi	Frame
1	Physical	Sending raw bits over a medium	Cables, radio, fiber	Bits
How the two models line up:
TCP/IP layer	OSI layers it covers
Application	7, 6, 5
Transport	4
Internet	3
Network Access	2, 1
Memory trick for layers 1 to 7: Please Do Not Throw Sausage Pizza Away (Physical, Data Link, Network, Transport, Session, Presentation, Application).
> TLS (the encryption behind HTTPS) doesn't fit neatly into one OSI layer. It's often placed at layer 6, but it really sits between the transport and application layers. Real protocols don't always follow the models perfectly, so treat the models as a *map*, not a law.
Encapsulation: boxes inside boxes
Now the key mechanism. When your device sends data, each layer wraps the data with its own header, like putting a letter in an envelope, then in a bigger envelope, then in a mailbag. The data link layer also adds a trailer at the end. This wrapping is called encapsulation.
Say your browser sends a request to a web server. Here's what happens, top to bottom:
Application: the browser creates the HTTP request. This is the data.
Transport: TCP adds a header with the source and destination ports. The result is a segment.
Internet: IP adds a header with the source and destination IP addresses. The result is a packet.
Network Access: Ethernet adds a header with the source and destination MAC addresses, and a trailer for error checking. The result is a frame.
Physical: the frame is sent as bits.
```
Application    [ DATA ]
Transport      [ TCP header | DATA ]                        = segment
Internet       [ IP header | TCP header | DATA ]            = packet
Network Access [ Eth header | IP | TCP | DATA | Eth trailer ] = frame
Physical       1011001010101...                             = bits
```
The receiving device does the reverse, called de-encapsulation: it removes the frame header, then the IP header, then the TCP header, and finally hands the data to the application.
What's actually inside the headers
Each header is just a set of labeled fields. A simplified look:
Header	Important fields	Example values
TCP (Layer 4)	Source port, destination port, sequence number, acknowledgment number, flags (SYN, ACK, FIN...), window size	Source port `51234`, destination port `443`
IP (Layer 3)	Source IP, destination IP, TTL, protocol (6 = TCP, 17 = UDP)	`192.168.1.10` → `203.0.113.50`, TTL `64`
Ethernet (Layer 2)	Destination MAC, source MAC, type (`0x0800` = IPv4), and a trailer with an error-check value (FCS)	`00:50:56:AA:BB:CC` ← `00:1A:2B:00:00:01`
Two things to notice:
The client's source port is a random high number (like `51234`) chosen by the operating system. The destination port is the well-known one for the service (like `443`).
The sequence numbers in the TCP header are the "box 3 of 10" from our LEGO story. They let the receiver put pieces back in order and spot missing ones.
Which address does which job?
Address	Layer	Question it answers	Does it change along the way?
Port	4 (Transport)	Which program?	No
IP address	3 (Internet)	Which device, anywhere in the world?	No (end to end)*
MAC address	2 (Data Link)	Which device on this local link?	Yes, at every hop
* Home routers do rewrite the source IP using NAT. That's covered in Section 5.
A key detail: MAC addresses change at every hop, IP addresses don't
This surprises almost everyone, and it's one of the most important ideas in networking. Follow one packet.
```
 PC                     Router                    Web server
 IP  192.168.1.10       LAN side: 192.168.1.1     IP  203.0.113.50
 MAC 00:1A:2B:00:00:01  MAC 00:1A:2B:00:00:99     MAC 00:50:56:AA:BB:CC
                        Other side MAC: 00:1A:2B:00:00:98
```
To keep it simple, pretend the router connects directly to the server's network.
Hop 1: PC → Router
Field	Value
Source MAC	`00:1A:2B:00:00:01` (PC)
Destination MAC	`00:1A:2B:00:00:99` (router, not the server!)
Source IP	`192.168.1.10`
Destination IP	`203.0.113.50` (server)
The PC notices the server is not on its local network, so it can't deliver the frame directly. It sends the frame to the router, so the destination MAC is the router's. The IP still says the final destination is the server.
Hop 2: Router → Server
The router removes the old frame, reads the destination IP, decides where to send it, and builds a brand-new frame:
Field	Value
Source MAC	`00:1A:2B:00:00:98` (router)
Destination MAC	`00:50:56:AA:BB:CC` (server)
Source IP	`192.168.1.10` (unchanged*)
Destination IP	`203.0.113.50` (unchanged)
The rule: the frame is rebuilt at every hop (new MAC addresses for each link), but the IP addresses stay the same from start to finish. MAC addresses answer "who's next?", while IP addresses answer "who's the final destination?"
> **🔍 Going deeper:** How does the PC know the router's MAC address? It asks the local network using **ARP**: "Who has 192.168.1.1? Tell me your MAC." Section 4 covers this, and it's also where an important attack (ARP spoofing) comes from.
Which device works at which layer
A device is defined by the highest layer it looks at when handling data:
Device	Layer	What it looks at	In the story
Hub (old)	1	Nothing: repeats bits out of every port	A loudspeaker
Switch	2	MAC addresses	Sorting inside one building
Router	3	IP addresses	Post office choosing the next city
Firewall	3 to 7	IPs, ports, and (on newer firewalls) application data	A guard inspecting the boxes
Why layers matter
For troubleshooting: work through the layers, usually from the bottom up.
Layer 1: Is the cable plugged in? Is the link light on? Is Wi-Fi connected?
Layer 2: Is the switch learning the device's MAC address? Is the port in the right VLAN?
Layer 3: Does the device have a valid IP address? Can you `ping` it?
Layer 4: Is the port open, or is a firewall blocking it?
Layer 7: Is the application or service itself running correctly?
For security: attacks and defenses are both tied to layers.
Layer	Example attack	Example defense
2	ARP spoofing, MAC flooding	Dynamic ARP Inspection, port security
3	IP spoofing	ACLs, anti-spoofing filters
4	SYN flood, port scanning	Firewalls, rate limiting
7	SQL injection, XSS	Input validation, web application firewalls
> The Layer 2 defenses above are covered with commands in the CCNA file (see Port Security, DHCP Snooping, and Dynamic ARP Inspection).
Key terms
Term	Meaning
Layer	One step in the networking model, with one specific job
TCP/IP model	The 4-layer model the internet actually uses
OSI model	A 7-layer reference model used as common vocabulary
Encapsulation	Each layer wrapping the data with its own header (and trailer)
De-encapsulation	The receiver unwrapping the layers in reverse order
Segment / packet / frame	The data unit at Layer 4 / Layer 3 / Layer 2
Port	A number identifying which program should receive the data
TCP / UDP	Reliable delivery with checking / fast delivery without guarantees
MAC address	Hardware address used for delivery on the local link
Hop	One step from one device to the next along the path
TTL	A counter that stops packets from circling forever
Self-check
Try to answer first, then open the answer.
1. Why is the network split into layers? Give two benefits.
<details>
<summary>Answer</summary>
Any two of: each layer is a smaller, simpler problem; layers can be swapped without affecting the others (Wi-Fi to cable without changing the browser); different vendors' equipment works together through shared standards; and problems are easier to locate by layer.
</details>
2. An IP address gets a packet to the right device. What gets the data to the right program on that device?
<details>
<summary>Answer</summary>
The port number, handled at the Transport layer.
</details>
3. For each, would you choose TCP or UDP: (a) downloading a file, (b) a live video call?
<details>
<summary>Answer</summary>
(a) TCP, because every piece must arrive and in order or the file is corrupted. (b) UDP, because speed matters more than perfection and a lost frame isn't worth resending.
</details>
4. What is the data unit called at the Transport layer (TCP), the Internet/Network layer, and the Data Link layer?
<details>
<summary>Answer</summary>
Segment, packet, and frame.
</details>
5. A PC sends data to a server on another network. At the first hop, whose MAC address is in the destination field, and why?
<details>
<summary>Answer</summary>
The router's. The server isn't on the PC's local network, so the PC delivers the frame to the router (the next hop). The destination IP address still points to the server.
</details>
6. As a packet crosses several routers, which addresses stay the same and which change?
<details>
<summary>Answer</summary>
The source and destination IP addresses stay the same end to end (apart from NAT on home routers), while the MAC addresses are replaced at every hop because a new frame is built for each link.
</details>
7. A web server answers ping but the website won't load. Which layers are probably fine, and what should you check next?
<details>
<summary>Answer</summary>
Ping working means Layers 1 to 3 are fine. Check Layer 4 (is port 80 or 443 open, or blocked by a firewall?), then Layer 7 (is the web service itself running?).
</details>
---
3. Addressing
In Section 1 you met three kinds of addresses: MAC addresses, IP addresses, and ports. This section explains the first two properly. It includes subnetting, the topic that scares most beginners but is really just careful counting. We'll go slowly and use lots of examples.
Why so many addresses?
Each address answers a different question, at a different layer:
Address	Layer	Question	Looks like	Scope
MAC	2	Which network card on this link?	`00:1A:2B:3C:4D:5E`	Local network only
IP	3	Which device, and which network is it on?	`192.168.1.10`	Worldwide
Port	4	Which program on the device?	`443`	One device
First, a quick number-systems primer
Addresses are written in different number systems. You need two of them.
Binary (base 2)
Computers use only 0 and 1. In binary, each position is worth double the one to its right. For one octet (8 bits), the place values are:
```
128   64   32   16    8    4    2    1
```
Binary to decimal: add the place values wherever there's a 1.
```
11000000  =  128 + 64            = 192
10101000  =  128 + 32 + 8        = 168
00001010  =  8 + 2               = 10
```
Decimal to binary: go left to right through the place values. If the number is at least as big as the place value, write `1` and subtract it. Otherwise write `0`.
Example: convert 168.
Place value	168 ≥ value?	Bit	Left over
128	yes	1	40
64	no	0	40
32	yes	1	8
16	no	0	8
8	yes	1	0
4	no	0	0
2	no	0	0
1	no	0	0
So 168 = `10101000`.
Hexadecimal (base 16)
Hexadecimal ("hex") uses 16 symbols: `0-9` and then `A-F`, where A = 10, B = 11, C = 12, D = 13, E = 14, F = 15. One hex digit equals exactly 4 bits, which makes it a short way to write binary.
```
1A  =  0001 1010  =  (1 × 16) + 10  =  26
FF  =  1111 1111  =  255
```
MAC addresses
A MAC (Media Access Control) address identifies a network interface, such as the Wi-Fi chip or Ethernet port in your device.
It is 48 bits long, written as 12 hex digits: `00:1A:2B:3C:4D:5E`.
Different systems use different punctuation: `00-1A-2B-3C-4D-5E` (Windows) or `001a.2b3c.4d5e` (Cisco).
The first half (first 24 bits, 6 hex digits) is the OUI, which identifies the manufacturer. The second half is a number the manufacturer assigns to that specific card.
It's assigned at manufacturing, which is why it's often called the "burned-in address". In practice it can be changed in software ("MAC spoofing"), so never treat a MAC address as proof of identity.
Special MAC addresses:
Address	Meaning
`FF:FF:FF:FF:FF:FF`	Broadcast: every device on the local network
Starts with `01:00:5E`	IPv4 multicast: a chosen group of devices
Anything else	Unicast: one specific device
MAC addresses only matter on the local link. As you saw in Section 2, they are replaced at every hop.
IPv4 addresses
An IPv4 address is 32 bits long. To make it readable, it's split into four 8-bit chunks called octets, each written as a decimal number from 0 to 255 and separated by dots. This is dotted decimal notation.
```
192.168.1.10
= 11000000.10101000.00000001.00001010
```
With 32 bits there are 2³² = 4,294,967,296 possible addresses. That sounds like a lot, but it's fewer than the number of devices in the world, which is why the internet is moving to IPv6 (later in this section).
> **🔍 Going deeper: classful addressing (history).** Early IPv4 divided addresses into fixed classes by their first octet: **Class A** (1 to 126, mask /8), **Class B** (128 to 191, /16), **Class C** (192 to 223, /24), **Class D** (224 to 239, multicast), and **Class E** (240 to 255, experimental). This wasted a huge number of addresses, so it was replaced by flexible masks (CIDR, below). You'll still see the class names in exams and old documentation, but modern networks ignore them.
The network part and the host part
An IP address has two parts, like a postal address:
The network part says which network (like the street name).
The host part says which device on that network (like the house number).
Everyone on the same street shares the street name but has a different house number. Everyone on the same network shares the network part but has a different host part.
How do we know where the network part ends? That's the job of the subnet mask.
The subnet mask
A subnet mask is a 32-bit number that marks which bits of the address are the network part. In the mask, 1s mean network and 0s mean host. The 1s always come first.
```
IP address   192.168.1.10   =  11000000.10101000.00000001.00001010
Subnet mask  255.255.255.0  =  11111111.11111111.11111111.00000000
                               |------ network part ------| host part
```
So here the network is `192.168.1` and the host is `.10`.
CIDR notation (/24)
Writing the full mask is long, so we usually just count the 1s and write that after a slash. This is CIDR notation (or prefix length).
`255.255.255.0` has 24 ones, so it's /24.
`192.168.1.10/24` means "address 192.168.1.10 with a 24-bit network part".
Common masks to memorize:
Prefix	Subnet mask	Host bits	Usable hosts
/8	255.0.0.0	24	16,777,214
/16	255.255.0.0	16	65,534
/24	255.255.255.0	8	254
/25	255.255.255.128	7	126
/26	255.255.255.192	6	62
/27	255.255.255.224	5	30
/28	255.255.255.240	4	14
/29	255.255.255.248	3	6
/30	255.255.255.252	2	2
Three special addresses in every network
Inside each network, two addresses can't be given to a normal device:
Network address: the address where all host bits are 0. It names the network itself (for example `192.168.1.0`).
Broadcast address: the address where all host bits are 1. It means "everyone on this network" (for example `192.168.1.255`).
Everything in between is available for devices. So:
> **Usable hosts = 2^h − 2**, where *h* is the number of host bits. (The −2 removes the network and broadcast addresses.)
Example, `192.168.1.0/24`:
Item	Value
Network address	`192.168.1.0`
First usable host	`192.168.1.1`
Last usable host	`192.168.1.254`
Broadcast address	`192.168.1.255`
Usable hosts	2⁸ − 2 = 254
Is the destination on my network?
Before sending anything, a device asks: "Is the destination on my network or somewhere else?" It compares network parts.
My PC is `192.168.1.10/24`, so my network part is `192.168.1`.
Destination `192.168.1.20`: network part `192.168.1`. Same network, so send it directly to that device.
Destination `8.8.8.8`: network part `8.8.8`. Different network, so send it to the router (the default gateway), exactly like the MAC example in Section 2.
> **🔍 Going deeper: the binary AND.** Devices do this check by performing a bitwise **AND** between the address and the mask. Where the mask has a 1, the address bit is kept; where it has a 0, the result is 0. Example with `192.168.1.70/26` (mask `255.255.255.192`): the last octet is `70 = 01000110` and `192 = 11000000`. `01000110 AND 11000000 = 01000000 = 64`. So the network address is `192.168.1.64`.
Public, private, and special addresses
Not every IP address can be used on the internet.
Private addresses (defined by RFC 1918) are reserved for use inside homes, schools, and companies. They are not routed on the public internet, so thousands of networks can reuse them.
Range	CIDR	Typical use
10.0.0.0 to 10.255.255.255	10.0.0.0/8	Large organizations
172.16.0.0 to 172.31.255.255	172.16.0.0/12	Medium networks
192.168.0.0 to 192.168.255.255	192.168.0.0/16	Homes, small offices
Everything else (apart from the special ranges below) is a public address, unique worldwide and assigned through ISPs. How private devices still reach the internet is NAT, covered in Section 5.
Special addresses you'll run into:
Address	Meaning
`127.0.0.1` (all of `127.0.0.0/8`)	Loopback: "this device itself". `ping 127.0.0.1` tests your own network software.
`169.254.x.x` (`169.254.0.0/16`)	Link-local (APIPA): a device gives itself this address when it asked for an IP automatically and nobody answered. If you see it, DHCP failed.
`0.0.0.0`	"Unspecified" or "any address". Also used for the default route.
`255.255.255.255`	Limited broadcast: everyone on the local network
Subnetting: splitting a network into smaller networks
Subnetting means taking one big network and dividing it into several smaller ones, called subnets.
Why would you?
Organization: a separate subnet per department, floor, or purpose.
Smaller broadcast domains: broadcasts stay inside their subnet, which reduces noise and improves performance.
Security: traffic between subnets must pass through a router (or firewall), where you can filter it.
Efficiency: you give each group only the number of addresses it needs, instead of wasting a whole big network.
How it works: you borrow bits from the host part and use them as extra network bits. The prefix gets longer, so there are more (but smaller) networks.
The method
Decide what you need: a number of subnets or a number of hosts per subnet.
Find the new prefix length:
Need subnets? Borrow n bits so that 2ⁿ ≥ subnets needed.
Need hosts? Keep h host bits so that 2ʰ − 2 ≥ hosts needed.
Work out the block size (the "jump" between subnets). It is `256 − (the interesting octet of the mask)`.
List the subnets by counting up in steps of the block size. For each subnet:
Network address = the start of the block
Broadcast address = the last address of the block (next network − 1)
Usable hosts = everything in between
Example 1: split into 4 subnets
You have `192.168.1.0/24` and need 4 subnets.
2ⁿ ≥ 4 means n = 2, so borrow 2 bits: /24 becomes /26 (mask `255.255.255.192`).
Block size = 256 − 192 = 64.
Hosts per subnet = 2⁶ − 2 = 62.
Subnet	Network address	Usable range	Broadcast
1	192.168.1.0/26	.1 to .62	.63
2	192.168.1.64/26	.65 to .126	.127
3	192.168.1.128/26	.129 to .190	.191
4	192.168.1.192/26	.193 to .254	.255
Example 2: how many hosts do I need?
A department needs 50 hosts, and you're working inside `192.168.10.0/24`.
2ʰ − 2 ≥ 50 means h = 6 (2⁶ − 2 = 62; with h = 5 you'd only get 30).
6 host bits means the prefix is 32 − 6 = /26.
Example 3: which subnet is this address in?
Which subnet does `192.168.5.77/27` belong to?
/27 mask is `255.255.255.224`, so block size = 256 − 224 = 32.
The subnets start at 0, 32, 64, 96...
77 falls between 64 and 96, so the subnet starts at 64.
Network `192.168.5.64`, broadcast `192.168.5.95`, usable `.65` to `.94`.
Quick reference for the last octet
Prefix	Mask (last octet)	Block size	Usable hosts	Subnets from a /24
/25	128	128	126	2
/26	192	64	62	4
/27	224	32	30	8
/28	240	16	14	16
/29	248	8	6	32
/30	252	4	2	64
A /30 gives exactly 2 usable addresses, which is perfect for a link between two routers.
> **🔍 Going deeper: VLSM (variable-length subnet masks).** Real networks need subnets of different sizes. With **VLSM** you give each subnet the prefix it needs. The golden rule: **allocate the largest subnet first**, then fit smaller ones after it. Example from `192.168.1.0/24`:
>
> | Need | Prefix | Subnet |
> | --- | --- | --- |
> | 100 hosts | /25 (126 hosts) | `192.168.1.0/25` (.0 to .127) |
> | 50 hosts | /26 (62 hosts) | `192.168.1.128/26` (.128 to .191) |
> | Router-to-router link (2 hosts) | /30 (2 hosts) | `192.168.1.192/30` (.192 to .195) |
>
> The subnets don't overlap, and plenty of space (from `.196` on) is left for future use.
> **🔍 Going deeper: /31 and /32.** A **/32** identifies a single host (used in host routes). A **/31** has 2 addresses and no network/broadcast, and is used on point-to-point links (RFC 3021).
IPv6
Why IPv6? IPv4 has about 4.3 billion addresses, which ran out. IPv6 uses 128 bits, giving about 3.4 × 10³⁸ addresses, enough that running out isn't a realistic concern.
Writing IPv6 addresses
An IPv6 address is written as eight groups of four hex digits, separated by colons:
```
2001:0db8:0000:0000:0000:0000:0000:0001
```
That's long, so there are two shortening rules:
Drop leading zeros in each group: `0db8` becomes `db8`, `0000` becomes `0`.
Replace one run of consecutive all-zero groups with `::`. You can only do this once per address, otherwise it would be unclear how many zeros each `::` stands for.
```
2001:0db8:0000:0000:0000:0000:0000:0001
-> 2001:db8:0:0:0:0:0:1        (rule 1)
-> 2001:db8::1                  (rule 2)
```
Prefix and interface ID
IPv6 also uses prefix lengths. On a typical LAN, the address splits into a 64-bit network prefix and a 64-bit interface ID (the host part), which is why LANs normally use /64:
```
2001:db8:1:1 : 0000:0000:0000:0001
|--- /64 prefix ---| |-- interface ID --|
```
IPv6 address types
Type	Range	Purpose
Global unicast	`2000::/3` (starts with 2 or 3)	Public, routable addresses
Link-local	`FE80::/10`	Works only on the local link. Every IPv6 interface has one.
Unique local	`FC00::/7` (in practice `FD00::/8`)	Private addresses, similar in spirit to RFC 1918
Multicast	`FF00::/8`	One-to-many. Replaces broadcast.
Loopback	`::1`	The device itself
Important differences from IPv4:
There is no broadcast in IPv6. Multicast does that job (for example `FF02::1` means "all nodes on this link").
ARP is replaced by NDP (Neighbor Discovery Protocol), which uses ICMPv6 messages.
Devices can build their own address automatically with SLAAC: the router announces the prefix, and the device creates the interface ID itself.
> **🔍 Going deeper: EUI-64.** One way to create the interface ID is from the MAC address: split the MAC in half, insert `FFFE` in the middle, and flip the 7th bit of the first byte. Example: MAC `00:1A:2B:3C:4D:5E` becomes interface ID `021A:2BFF:FE3C:4D5E`. Because this exposes the MAC address, modern systems usually use random interface IDs instead.
The commands for configuring IPv4 and IPv6 addresses on Cisco devices are in the IPv4 Configuration and IPv6 Configuration sections of the CCNA file.
Static vs dynamic assignment
An IP address can be set two ways:
Static: you type it in by hand. Used for servers, routers, and printers that must always be at the same address.
Dynamic: a DHCP server hands out addresses automatically. Used for phones, laptops, and everyday devices. DHCP is covered in Section 6.
Key terms
Term	Meaning
Binary / hex	Base-2 and base-16 number systems
Octet	A group of 8 bits, one "chunk" of an IPv4 address
MAC address	48-bit hardware address used on the local link
IPv4 address	32-bit address, written in dotted decimal
Subnet mask	Marks which bits of an address are the network part
CIDR / prefix length	The number of network bits, written like `/24`
Network address	First address in a subnet (host bits all 0)
Broadcast address	Last address in a subnet (host bits all 1)
Subnetting	Dividing a network into smaller networks
Block size	The step between subnet addresses (256 minus the mask value)
Private address	Address reserved for internal use (RFC 1918)
Loopback	`127.0.0.1`, meaning "this device"
IPv6	128-bit addressing, written in hex
Link-local	An address valid only on the local link (`FE80::/10`)
Self-check
Try to answer first, then open the answer.
1. Convert `172.16.5.1` to binary.
<details>
<summary>Answer</summary>
`10101100.00010000.00000101.00000001`
(172 = 128 + 32 + 8 + 4, 16 = 16, 5 = 4 + 1, 1 = 1.)
</details>
2. What is the subnet mask for /26?
<details>
<summary>Answer</summary>
`255.255.255.192`. A /26 has 26 ones: three full octets (24) plus 2 more bits, so the last octet is `11000000` = 192.
</details>
3. How many usable hosts are in a /27?
<details>
<summary>Answer</summary>
A /27 leaves 5 host bits, and 2⁵ − 2 = 30.
</details>
4. For `192.168.1.130/26`, find the network address, broadcast address, and usable range.
<details>
<summary>Answer</summary>
Block size is 64, so the subnets start at 0, 64, 128, 192. 130 is in the block starting at 128. Network `192.168.1.128`, broadcast `192.168.1.191`, usable `192.168.1.129` to `192.168.1.190`.
</details>
5. Are `10.1.1.5/24` and `10.1.2.5/24` on the same network?
<details>
<summary>Answer</summary>
No. With a /24 the first three octets are the network part. One is on `10.1.1.0` and the other on `10.1.2.0`, so they need a router to communicate.
</details>
6. You need subnets with at least 20 hosts each. Which prefix length do you use?
<details>
<summary>Answer</summary>
/27. You need 5 host bits (2⁵ − 2 = 30 ≥ 20). With only 4 host bits you'd get 14, which is too few.
</details>
7. Which of these is a private address: `172.32.0.1` or `172.20.0.1`?
<details>
<summary>Answer</summary>
`172.20.0.1`. The private range is 172.16.0.0 to 172.31.255.255, so 172.32.x.x is public.
</details>
8. Shorten `2001:0db8:0000:0000:0000:0000:0000:0001`. Why can `::` only be used once in an address?
<details>
<summary>Answer</summary>
`2001:db8::1`. If `::` appeared twice, you couldn't tell how many zero groups each one replaced, so the address would be ambiguous.
</details>
---
4. The Local Network
Now we zoom into a single LAN: the devices in one home, classroom, or office, and the equipment that connects them. This is the Network Access layer from Section 2 (Layers 1 and 2) in action.
What a LAN looks like
In a typical wired LAN, every device plugs into a switch. Wireless devices connect through an access point, which in turn plugs into the switch. A router connects the whole LAN to the outside world.
```
  PC A ─────┐
  PC B ─────┤
  PC C ─────┼──── Switch ──── Router ──── Internet
  Printer ──┤
  Access point ─┘   (Wi-Fi devices connect here)
```
Hubs vs switches
The old way to connect devices was a hub. A hub is a Layer 1 device with no intelligence: whatever comes in one port is repeated out of every other port.
Every device hears every conversation (bad for privacy and performance).
If two devices talk at once, their signals collide and both must retry.
A switch is smarter. It learns which device is on which port and sends each frame only where it needs to go. Two conversations can happen at the same time without interfering.
Two terms you'll see often:
Term	Meaning
Collision domain	A group of devices whose signals can collide. A hub puts everything in one. A switch gives each port its own, which is why modern switched networks (running full-duplex) basically don't have collisions.
Broadcast domain	The group of devices that receive each other's broadcasts. All ports on a switch share one broadcast domain by default. Routers split broadcast domains.
How a switch learns: the MAC address table
A switch keeps a MAC address table (also called the CAM table) that maps MAC addresses to ports. It builds it automatically by watching traffic. The rules are simple:
Learn: when a frame arrives, record the source MAC address and the port it came in on.
Forward: look up the destination MAC. If it's in the table, send the frame out only that port.
Flood: if the destination is unknown (or it's a broadcast), send the frame out all ports except the one it came from.
Filter: if the destination is on the same port the frame arrived on, don't forward it.
Worked example
Four PCs, A to D, are plugged into ports 1 to 4. The table starts empty.
Step 1: A sends a frame to C.
The switch learns: A is on port 1.
C is unknown, so the switch floods the frame out ports 2, 3, and 4. Only C accepts it; B and D ignore it.
Step 2: C replies to A.
The switch learns: C is on port 3.
A is known (port 1), so the frame goes out port 1 only.
Step 3: A sends to C again.
Both are known, so the frame goes only out port 3. B and D never see it.
The table now looks like this:
MAC address	Port
A	1
C	3
Entries expire after a period of inactivity (300 seconds by default on Cisco switches), so the table stays accurate when devices move. On a Cisco switch, view the table with `show mac address-table`.
The three kinds of frames
Type	Destination MAC	Switch behavior
Unicast	One specific device	Forward to one port (or flood if unknown)
Broadcast	`FF:FF:FF:FF:FF:FF`	Flood to everyone in the broadcast domain
Multicast	A group address	Sent to interested devices (or flooded, depending on the switch)
ARP: finding a MAC address from an IP address
Here's a puzzle. A PC wants to send data to `192.168.1.20`. It knows the IP address, but to build the Ethernet frame it needs the MAC address. How does it find it?
It uses ARP (Address Resolution Protocol):
The PC first checks its ARP cache (a small table of IP-to-MAC mappings it already knows).
If it's not there, the PC sends an ARP request as a broadcast: "Who has 192.168.1.20? Tell 192.168.1.10." (Destination MAC = `FF:FF:FF:FF:FF:FF`.)
Every device on the LAN receives it, but only the owner of `192.168.1.20` answers.
That device sends an ARP reply straight back (unicast): "192.168.1.20 is at 00:50:56:AA:BB:CC."
The PC stores the answer in its ARP cache and builds the frame.
What if the destination is on another network? The PC doesn't ARP for the far-away server. It ARPs for the default gateway's IP address, and sends the frame to the router's MAC (exactly the "Hop 1" example in Section 2).
Useful commands:
Where	Command to view the ARP table
Windows / Linux / macOS	`arp -a`
Cisco router/switch	`show ip arp`
> **🔍 Going deeper:** ARP has **no authentication**: devices simply believe any reply. An attacker on the LAN can send fake replies to trick devices into sending traffic to the wrong place (**ARP spoofing**). Section 7 covers it, and the Dynamic ARP Inspection section of the [CCNA file](CCNA-Configuration-Commands.md) shows the defense. Cisco routers keep ARP entries for 4 hours by default.
Ethernet basics
Ethernet is the standard for wired LANs. A few details worth knowing:
Speeds: Fast Ethernet (100 Mbps), Gigabit (1 Gbps), 10 Gigabit, and higher. Cables are typically copper twisted-pair (Cat5e/Cat6) for short distances, and fiber for long distances or high speeds.
Duplex: Half duplex means a device can send or receive but not both at once (collisions possible). Full duplex means both at the same time with no collisions. Modern switched links run full duplex. A duplex mismatch (one side full, the other half) causes slow, error-filled links.
MTU: the largest payload one frame carries, normally 1,500 bytes. This is why large data gets split into many packets.
Auto-negotiation: the two ends of a link agree on speed and duplex automatically.
VLANs: splitting one switch into several networks
By default, every port on a switch is in the same broadcast domain. As the network grows, that causes problems: more broadcast noise, and no separation between, say, the finance department and guests.
A VLAN (Virtual LAN) divides one physical switch into several separate logical networks.
Analogy: one office building (the switch), divided into separate floors with locked doors (VLANs). People on the same floor can talk freely, but to reach another floor, they must go through reception (a router).
Benefits:
Smaller broadcast domains, so less noise and better performance
Security: isolate sensitive systems (servers, guests, cameras) from each other
Flexibility: group devices by function, not by where they're plugged in
Every VLAN has a number (VLAN ID). VLAN 1 is the default.
Access ports and trunk ports
Port type	Carries	Connects to
Access port	Traffic of one VLAN	End devices (PCs, printers)
Trunk port	Traffic of many VLANs at once	Other switches and routers
Because a trunk carries several VLANs over one cable, each frame needs a label saying which VLAN it belongs to. That's the 802.1Q tag: a 4-byte field inserted into the Ethernet frame, containing a 12-bit VLAN ID.
```
  Sales PCs (VLAN 10)             Switch 1 ═══ trunk ═══ Switch 2        Sales PCs (VLAN 10)
  Finance PCs (VLAN 20)    ───>   (frames tagged with VLAN ID)    ───>   Finance PCs (VLAN 20)
```
Key facts:
Usable VLAN IDs run from 1 to 4094 (the "normal range" is 1 to 1005).
The native VLAN is the one VLAN whose frames cross a trunk untagged. By default it's VLAN 1.
Devices in different VLANs can't talk to each other without a router, even if they're plugged into the same switch. Connecting VLANs through a router is inter-VLAN routing, covered in Section 5.
> **🔍 Going deeper:** Good practice is to *not* use VLAN 1 for user traffic, and to change the native VLAN to an unused one. Those are security measures, explained in Section 7.
The commands to create VLANs and configure access and trunk ports are in the VLANs and Trunking sections of the CCNA file.
Spanning Tree Protocol (STP): preventing loops
Networks often have redundant links, so if one cable or switch fails, another path keeps things working. But redundancy creates a danger: a loop.
Remember that switches flood broadcasts, and that Ethernet frames have no TTL (unlike IP packets). In a loop, a broadcast frame circles forever, getting copied at every switch. Within seconds the copies saturate every link. This is a broadcast storm, and it can bring down a whole network.
STP (Spanning Tree Protocol) solves this. Think of traffic officers closing one road in a circular route so cars can't go round and round, but ready to reopen it if another road is blocked.
How it works, in plain steps:
The switches elect one root bridge: the switch with the lowest Bridge ID (a priority number, default 32768, with the MAC address as the tie-breaker).
Every other switch finds its best path to the root and uses that port as its root port.
On each link, one side becomes the designated port (forwards traffic).
Any port that would create a loop is put in a blocking state. It sits idle, ready to take over if an active link fails.
The result is a loop-free "tree" shaped network, with the backup links held in reserve.
Version	Notes
Classic STP (802.1D)	Slow to react: up to about 50 seconds to recover after a change
Rapid STP / Rapid PVST+	Recovers in a few seconds. This is what you should use on Cisco networks.
> **🔍 Going deeper:** Because STP decides who becomes the root bridge, attackers sometimes try to become the root by sending special messages (BPDUs). Features such as **PortFast**, **BPDU Guard**, and **Root Guard** prevent this. See the **STP and Rapid PVST+** section of the [CCNA file](CCNA-Configuration-Commands.md).
EtherChannel: combining links
If you connect two switches with two cables, STP would block one. EtherChannel bundles several physical links into one logical link, giving more bandwidth and redundancy without STP blocking them. It's negotiated with LACP (an open standard) or PAgP (Cisco-only). See the EtherChannel and LACP section of the CCNA file.
Wi-Fi basics
Wi-Fi is Ethernet's wireless cousin (the IEEE 802.11 family). Wireless devices connect to an access point (AP), which bridges them onto the wired LAN.
SSID: the network's name.
Bands: 2.4 GHz reaches farther but is slower and crowded; 5 GHz (and 6 GHz) is faster with shorter range.
Shared medium: radio is like a hub, because everyone in range can hear the signal. Wi-Fi therefore relies on encryption to protect data.
Security standards:
Standard	Verdict
Open (no password)	Traffic isn't protected
WEP	Broken. Never use it.
WPA2 (AES/CCMP)	Good if you use a strong password

WPA3	Best available, with stronger protection against password guessing
Turn off WPS (push-button setup) if you don't need it, and use a separate guest network for visitors.
Layer 2 security preview
The LAN is where many attacks begin, because devices on a LAN tend to trust each other. Common Layer 2 attacks include MAC flooding, ARP spoofing, rogue DHCP servers, VLAN hopping, and STP manipulation. Each has a matching defense, and Section 7 ties them together.
Key terms
Term	Meaning
Hub	Old Layer 1 device that repeats everything to every port
Switch	Layer 2 device that forwards frames by MAC address
MAC address table (CAM table)	The switch's map of MAC addresses to ports
Flooding	Sending a frame out all ports except the one it arrived on
Collision domain	Devices whose transmissions can collide
Broadcast domain	Devices that receive each other's broadcasts
ARP	Protocol that finds a MAC address from an IP address
ARP cache	Table of recently learned IP-to-MAC mappings
MTU	Largest payload a frame can carry (normally 1,500 bytes)
VLAN	A logical network created by dividing a switch
Access port / trunk port	Carries one VLAN / carries many VLANs
802.1Q	The VLAN tagging standard
Native VLAN	The VLAN whose frames cross a trunk untagged
STP	Protocol that prevents switching loops
Root bridge	The switch at the center of the STP tree
Broadcast storm	Broadcast frames circling in a loop and flooding the network
EtherChannel	Several links bundled into one logical link
SSID	The name of a Wi-Fi network
Self-check
Try to answer first, then open the answer.
1. A switch receives a frame whose destination MAC isn't in its table. What does it do?
<details>
<summary>Answer</summary>
It floods the frame out every port except the one it arrived on. When the real destination replies, the switch learns its port.
</details>
2. How does a switch learn which MAC address is on which port?
<details>
<summary>Answer</summary>
By reading the source MAC address of every frame that arrives and recording the port it came in on.
</details>
3. Why is an ARP request sent as a broadcast, while the ARP reply is a unicast?
<details>
<summary>Answer</summary>
The sender doesn't know the target's MAC address yet, so it must ask everyone (broadcast). The target already knows the asker's MAC (it's in the request), so it can reply directly.
</details>
4. A PC wants to reach a server on a different network. Which IP address does it ARP for?
<details>
<summary>Answer</summary>
The default gateway's. The frame goes to the router's MAC address, and the router carries it onward.
</details>
5. Two PCs are plugged into the same switch but are in different VLANs. Can they communicate directly?
<details>
<summary>Answer</summary>
No. Different VLANs are separate broadcast domains and networks, so traffic must go through a router (inter-VLAN routing).
</details>
6. What's the difference between an access port and a trunk port?
<details>
<summary>Answer</summary>
An access port belongs to one VLAN and connects an end device. A trunk port carries multiple VLANs (tagged with 802.1Q) between switches or to a router.
</details>
7. Why do loops cause broadcast storms in switched networks but not in routed networks?
<details>
<summary>Answer</summary>
Ethernet frames have no TTL, so a looping broadcast frame is never discarded and keeps multiplying. IP packets have a TTL that decreases at every router, so looping packets eventually die.
</details>
8. What does STP do with a redundant link?
<details>
<summary>Answer</summary>
It puts one port in a blocking state so the redundant link carries no traffic (preventing a loop), and brings it back into use if an active link fails.
</details>
---
5. Between Networks
A switch connects devices within one network. To reach a device on a different network, whether another subnet in your building or a server across the world, you need a router. This section follows a packet as it leaves your LAN and crosses the internet.
What a router does
A router has interfaces in two or more networks and forwards packets between them. Whenever a packet arrives, the router:
Receives the frame and removes the Layer 2 header (checks the destination MAC is its own).
Reads the destination IP address in the IP header.
Reduces the TTL by 1 (and drops the packet if it reaches 0).
Looks up the destination in its routing table to find the next hop and the outgoing interface.
Builds a new frame (new MAC addresses for the next link) and sends the packet on.
Step 5 is the "MAC addresses change at every hop" rule from Section 2.
Routers also separate broadcast domains: broadcasts don't cross a router. That's one reason large networks are divided into subnets.
The default gateway
When a host wants to reach a device on another network, it sends the packet to its default gateway: the IP address of the router on its own LAN. The host's decision looks like this:
```
Is the destination in MY subnet?
   ├── YES: ARP for the destination's MAC, send directly.
   └── NO:  ARP for the DEFAULT GATEWAY's MAC, send the frame to the router.
```
If a device has no default gateway configured (or a wrong one), it can talk to its own LAN but not beyond. That's a classic troubleshooting clue.
The routing table
The routing table is the router's map: a list of destination networks and how to reach each. A simplified example from a Cisco router (`show ip route`):
```
C    192.168.1.0/24 is directly connected, GigabitEthernet0/0
L    192.168.1.1/32 is directly connected, GigabitEthernet0/0
C    10.0.0.0/30 is directly connected, GigabitEthernet0/1
S    192.168.20.0/24 [1/0] via 10.0.0.2
O    192.168.30.0/24 [110/2] via 10.0.0.2, 00:05:12, GigabitEthernet0/1
S*   0.0.0.0/0 [1/0] via 10.0.0.2
```
How to read a line:
Part	Meaning
`C` / `L` / `S` / `O`	How the router learned the route: Connected, Local, Static, OSPF
`192.168.20.0/24`	The destination network
`[1/0]`	`[administrative distance / metric]`: explained below
`via 10.0.0.2`	The next hop: where to send the packet
`GigabitEthernet0/1`	The outgoing interface
`*`	This is the default route (candidate)
Longest prefix match
What if several routes match one destination? The router picks the most specific one: the match with the longest prefix.
Suppose the table has:
Route	Next hop
`10.0.0.0/8`	A
`10.1.0.0/16`	B
`10.1.1.0/24`	C
`0.0.0.0/0` (default)	D
A packet for `10.1.1.5` matches all four, but `/24` is the most specific, so it goes to C. A packet for `10.2.0.1` matches `/8` and the default, so it goes to A. A packet for `8.8.8.8` matches only the default, so it goes to D.
The default route (`0.0.0.0/0`) means "if nothing else matches, send it here". It's typically the path to the internet.
If there's no matching route and no default route, the router drops the packet and usually sends back an ICMP "destination unreachable" message.
How the routing table gets filled
There are three sources:
Connected routes: automatically added for each network the router's own interfaces are on.
Static routes: typed in manually by an administrator.
Dynamic routes: learned automatically by running a routing protocol, where routers share information with each other.
	Static routing	Dynamic routing
Setup	Manual, per route	Configure the protocol once
Adapts to failures	No (unless you add backup routes)	Yes, automatically
Overhead	None	Uses CPU, memory, bandwidth
Best for	Small networks, stub networks, default routes	Larger networks
A floating static route is a static route with a high administrative distance (see below) that acts as a backup. It only enters the table if the main route disappears.
The commands for static routes and OSPF are in the Static Routing and OSPFv2 sections of the CCNA file.
Routing protocols
Routing protocols fall into two families by how they think:
Distance-vector (for example RIP): each router tells its neighbors "here's what I know and how far it is". Simple, but slow to react and limited in size. RIP counts hops.
Link-state (for example OSPF): every router builds a complete map of the network, then calculates the shortest path itself. Faster and scales better.
And by where they're used:
Type	Protocols	Used for
IGP (inside one organization)	RIP, OSPF, EIGRP	Routing within a company or campus
EGP (between organizations)	BGP	The internet itself: how ISPs and large networks exchange routes
OSPF in plain words
OSPF (Open Shortest Path First) is the main protocol to learn for the CCNA.
Routers on the same link find each other by sending Hello messages and become neighbors.
Neighbors exchange information about their links (called LSAs), until every router in the area has the same map (the link-state database).
Each router runs the SPF algorithm to find the lowest-cost path to every network and puts the results in its routing table.
If a link fails, routers flood the update, and everyone recalculates.
Key ideas:
Cost is the metric. It is based on link bandwidth: faster links have lower cost. Lower total cost wins.
Areas divide a large network into smaller groups so updates stay local. Area 0 is the backbone that all other areas connect to.
Each router needs a unique router ID (written like an IPv4 address).
> **🔍 Going deeper:** On a shared (multi-access) segment like an Ethernet LAN with several routers, OSPF elects a **DR (Designated Router)** and **BDR (Backup DR)** so that routers don't all have to talk to each other. OSPF's default cost formula uses a 100 Mbps reference bandwidth, which means Fast Ethernet and Gigabit both end up with cost 1. Real networks adjust the reference bandwidth so faster links get a lower cost.
Administrative distance vs metric
Two different "scores" help a router decide:
Administrative distance (AD): how much the router trusts the source of a route. Used when different sources offer a route to the same network. Lower is more trusted.
Metric: how good a route is within one protocol (hops, cost, and so on). Used to choose between routes from the same source.
Route source	Default AD
Directly connected	0
Static route	1
EIGRP	90
OSPF	110
RIP	120
So a static route (AD 1) beats an OSPF route (AD 110) for the same network. That's how a floating static works: give it an AD of 200, and it only wins when the OSPF route is gone.
NAT: how private addresses reach the internet
Remember from Section 3: private addresses like `192.168.1.10` aren't routable on the public internet. And there aren't enough public IPv4 addresses to give every device one. NAT (Network Address Translation) solves both problems. A router at the edge of your network rewrites the IP addresses in packets as they pass through.
There are three flavors:
Type	What it does
Static NAT	One private address is permanently mapped to one public address (one-to-one). Used to make an internal server reachable.
Dynamic NAT	Private addresses are mapped to a pool of public addresses as needed.
PAT (Port Address Translation / "NAT overload")	Many private addresses share one public address, distinguished by port numbers. This is what your home router does.
How PAT works
Two PCs on a home network both browse the web. The router has one public address, `203.0.113.5`.
Inside (private)	Translated to (public)
`192.168.1.10:51234`	`203.0.113.5:40001`
`192.168.1.11:51234`	`203.0.113.5:40002`
PC1 sends a packet from `192.168.1.10:51234` to the server.
The router rewrites the source to `203.0.113.5:40001`, and records the mapping in its NAT table.
The server sees only `203.0.113.5:40001` and replies to that.
The router looks up port `40001` in the table, finds that it belongs to PC1, rewrites the destination back to `192.168.1.10:51234`, and forwards it.
Both PCs share one public IP, and the port number tells the router which conversation is whose.
Cisco uses specific terms for the addresses involved:
Term	Meaning
Inside local	The private address of an inside host (`192.168.1.10`)
Inside global	The public address that represents that host (`203.0.113.5`)
Outside global	The address of the external server
NAT has side effects worth understanding:
Incoming connections are blocked by default, because the router has no table entry for unsolicited traffic. This feels like protection, but NAT is not a firewall. To host a server at home you need port forwarding (a static mapping).
Some protocols embed IP addresses inside their data and need special handling.
IPv6 has enough addresses that NAT isn't needed.
The NAT commands are in the NAT and PAT section of the CCNA file.
Following a packet with traceroute
The traceroute tool (`tracert` on Windows) shows each router between you and a destination. It exploits TTL from Section 2: it sends packets with TTL = 1, then 2, then 3, and so on. Each router that reduces the TTL to 0 sends back a "time exceeded" message, revealing itself.
```
1   192.168.1.1     1 ms     (your home router)
2   198.51.100.1    8 ms     (your ISP)
3   198.51.100.9   12 ms     (a router inside the ISP)
4   203.0.113.1    30 ms     (another network)
5   203.0.113.50   31 ms     (the destination server)
```
Each line is one hop, and the time shows where delay builds up.
Putting Sections 2 to 5 together
Your PC (`192.168.1.10`) sends a request to a server (`203.0.113.50`):
The PC sees the server is on another network, so it ARPs for the default gateway and sends the frame to the router.
The router removes the frame, reads the destination IP, decreases TTL, and finds a route.
If this is a home router, PAT rewrites the source address to the public one.
The router builds a new frame for the next hop and sends it.
Each router along the way repeats steps 2 and 4.
The last router ARPs for the server's MAC (it's on its local network) and delivers the frame.
The server answers, and the reply travels back the same way (possibly by a different route), with PAT translating the destination back at your router.
Key terms
Term	Meaning
Router	Layer 3 device that forwards packets between networks
Default gateway	The router address a host uses to reach other networks
Routing table	The router's list of networks and how to reach them
Next hop	The next router to send the packet to
Longest prefix match	Choosing the most specific matching route
Default route	`0.0.0.0/0`: used when nothing more specific matches
Static / dynamic routing	Routes set by hand / learned from a routing protocol
Floating static route	A backup static route with a higher administrative distance
Administrative distance	How much the router trusts a route's source (lower is better)
Metric	A route's quality within one protocol (lower is better)
OSPF	A link-state routing protocol
BGP	The routing protocol that connects the internet's networks
NAT / PAT	Rewriting IP addresses / many hosts sharing one public IP via ports
Hop	One router on the path
Traceroute	Tool that lists the routers on the path to a destination
Self-check
Try to answer first, then open the answer.
1. A host can reach devices on its own LAN but nothing beyond. What's the most likely configuration problem?
<details>
<summary>Answer</summary>
A missing or incorrect default gateway (or the gateway router is down). The host has no way to send packets to other networks.
</details>
2. A router has routes for `10.0.0.0/8`, `10.1.0.0/16`, and `10.1.1.0/24`. Which does it use for a packet to `10.1.1.77`? And for `10.1.200.1`?
<details>
<summary>Answer</summary>
`10.1.1.77` uses the `/24` route (longest match). `10.1.200.1` doesn't fit `10.1.1.0/24`, so it uses the `/16` route.
</details>
3. A static route (AD 1) and an OSPF route (AD 110) both lead to the same network. Which wins, and why?
<details>
<summary>Answer</summary>
The static route wins. The lower administrative distance means the router trusts it more.
</details>
4. What's the difference between OSPF and BGP in terms of where they're used?
<details>
<summary>Answer</summary>
OSPF is an interior protocol, used to route within one organization. BGP is the exterior protocol that exchanges routes between organizations and ISPs, and holds the internet together.
</details>
5. Two home PCs use the same source port number at the same time through one public IP. How does the router tell their replies apart?
<details>
<summary>Answer</summary>
PAT gives each conversation a different translated source port on the public address, and records the mapping in the NAT table. Replies are matched by that port number.
</details>
6. Is NAT a firewall?
<details>
<summary>Answer</summary>
No. NAT blocks unsolicited incoming connections as a side effect of having no matching table entry, but it doesn't inspect or filter traffic by policy. You still need a real firewall.
</details>
7. How does traceroute discover each router on the path?
<details>
<summary>Answer</summary>
It sends packets with increasing TTL values (1, 2, 3...). Each router that reduces the TTL to zero sends back a "time exceeded" message, which reveals the router's address.
</details>
---
6. Core Services
Networks exist to deliver services: getting an address, finding a website, loading a page, sending an email. This section covers the services you use every time you go online, and ends with the full story from Section 1, now with every step explained.
DHCP: getting an address automatically
When your phone joins Wi-Fi, who tells it its IP address, subnet mask, gateway, and DNS server? DHCP (Dynamic Host Configuration Protocol). Typing these into every device by hand would be slow and error-prone (and two devices with the same IP cause conflicts).
A DHCP server holds a pool of addresses and leases them out to devices for a limited time.
The DORA process
A new device has no IP address and doesn't know where the DHCP server is, so it starts by broadcasting. DHCP uses UDP ports 67 (server) and 68 (client).
Step	Name	Direction	What it says
1	Discover	Client → everyone (broadcast)	"Is there a DHCP server? I need an address."
2	Offer	Server → client	"You can have `192.168.1.50`, mask `/24`, gateway `192.168.1.1`, DNS `8.8.8.8`, for 24 hours."
3	Request	Client → everyone (broadcast)	"I accept that offer."
4	Acknowledge	Server → client	"Confirmed. It's yours."
What the server hands out:
IP address and subnet mask
Default gateway
DNS server(s)
Lease time
Leases
The address is only leased. The client tries to renew halfway through the lease (at 50%). If the server doesn't answer, it keeps trying to rebind later (around 87.5%). When a device leaves, its address eventually returns to the pool.
DHCP across networks: relay
DHCP Discover is a broadcast, and routers don't forward broadcasts. If the DHCP server is on a different network from the clients, the router needs to be told to pass the requests along. This is a DHCP relay (the command `ip helper-address` on Cisco), and the router forwards the request as a unicast to the server.
When DHCP fails
If a device gets an address starting with `169.254` (APIPA from Section 3), it asked for an address and nobody answered. Check the DHCP server, the cable, and the VLAN.
> **🔍 Going deeper:** A **rogue DHCP server** (a device someone plugged in that also answers) can give clients a malicious gateway or DNS server. The defense, **DHCP snooping**, tells the switch which ports may send DHCP offers. See Section 7 and the DHCP section in the [CCNA file](CCNA-Configuration-Commands.md).
DNS: the internet's phone book
Humans remember names like `example.com`. Computers need IP addresses. DNS (Domain Name System) translates names into addresses. Without it you'd have to memorize numbers for every site.
The DNS hierarchy
DNS is a distributed, tree-shaped system, not one giant list:
```
                  . (root)
         ┌─────────┼─────────┐
        .com      .org      .eg   (top-level domains, TLDs)
         │
    example.com                   (the domain owner's server)
         │
  www.example.com                 (a host in that domain)
```
Reading a name from right to left goes from the most general to the most specific: `www` . `example` . `com` . (root).
What happens when you look up `www.example.com`
Your device checks its own cache (and the local hosts file). If it already knows, done.
If not, it asks its configured recursive resolver (usually your ISP's, or a public one like `8.8.8.8` or `1.1.1.1`). The resolver does the hard work for you.
The resolver checks its cache. If not found, it asks a root server: "Who handles `.com`?" The root points it to the `.com` servers.
The resolver asks the `.com` server: "Who handles `example.com`?" It points to the domain's own servers.
The resolver asks the authoritative server for `example.com`: "What's the IP of `www.example.com`?" and gets the answer.
The resolver returns the answer to your device and caches it for the duration of its TTL (time to live), so the next lookup is instant.
DNS normally uses UDP port 53 (and TCP for large responses and zone transfers).
Common record types
Record	Purpose	Example
A	Name → IPv4 address	`www.example.com → 203.0.113.50`
AAAA	Name → IPv6 address	`www.example.com → 2001:db8::50`
CNAME	Alias to another name	`blog.example.com → www.example.com`
MX	Mail servers for the domain	`example.com → mail.example.com`
NS	Which servers are authoritative for the domain	`example.com → ns1.example.com`
TXT	Free text (used for email authentication and verification)	`"v=spf1 ..."`
PTR	Reverse lookup: IP → name	`203.0.113.50 → www.example.com`
Tools to try: `nslookup example.com` (Windows, Linux, macOS) or `dig example.com` (Linux/macOS).
> **🔍 Going deeper: DNS security.** Classic DNS is **unencrypted and unauthenticated**. Attackers can forge replies (**DNS spoofing / cache poisoning**) to send users to fake sites, and DNS can be abused to smuggle data out of a network (**DNS tunneling**). Defenses include **DNSSEC** (signing records), and encrypted DNS such as **DNS over HTTPS (DoH)** and **DNS over TLS (DoT)**.
HTTP and HTTPS: how the web works
HTTP (HyperText Transfer Protocol) is how browsers and web servers talk. It's a simple request-response protocol: the client asks, the server answers.
Anatomy of a URL
```
https://www.example.com:443/shop/shoes?color=red#reviews
└─┬──┘  └───────┬───────┘└┬┘└───┬────┘└───┬────┘└──┬───┘
scheme        host      port  path     query   fragment
```
Part	Meaning
Scheme	The protocol (`http`, `https`)
Host	The server's name (resolved with DNS)
Port	Optional; defaults to 80 for HTTP and 443 for HTTPS
Path	Which resource on the server
Query	Extra parameters sent to the server
Fragment	A spot within the page (handled by the browser only)
A real request and response
Request (browser to server):
```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
```
Response (server to browser):
```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1256

<html> ...the page... </html>
```
A request has a method, a path, headers, and sometimes a body. A response has a status code, headers, and a body.
Common methods:
Method	Typical use
`GET`	Fetch a resource
`POST`	Send data (submit a form, log in)
`PUT`	Replace/upload a resource
`DELETE`	Remove a resource
Status codes come in families:
Range	Meaning	Examples
1xx	Informational	`100 Continue`
2xx	Success	`200 OK`
3xx	Redirect	`301 Moved Permanently`, `302 Found`
4xx	Client error	`400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`
5xx	Server error	`500 Internal Server Error`, `503 Service Unavailable`
HTTP is stateless: each request is independent, and the server doesn't remember you. Websites add memory using cookies: small pieces of data the server asks your browser to store and send back with each request, so the site can recognize your session (for example, that you're logged in).
HTTPS: HTTP with encryption
Plain HTTP sends everything in readable text. Anyone between you and the server (on the same Wi-Fi, or along the path) could read or change it. HTTPS is HTTP wrapped in TLS (Transport Layer Security), which provides:
Encryption: outsiders can't read the data
Integrity: the data can't be altered without detection
Authentication: you're really talking to the server you think you are, proven by a certificate
How the connection starts (simplified):
Your browser connects and says which TLS versions and ciphers it supports.
The server sends its certificate, which is signed by a trusted Certificate Authority (CA).
Your browser verifies the certificate (is it for this domain, is it valid, is it signed by a CA I trust?).
Both sides agree on a secret session key.
All further traffic is encrypted with that key (fast symmetric encryption).
The padlock in your browser means that the connection is encrypted and the certificate matched the site. It does not mean the site itself is trustworthy: phishing sites can have valid certificates too.
How servers work
Earlier we said a server is just "a computer running software that waits for requests". Let's make that concrete.
Listening on a port
A server program (a web server such as nginx or Apache, an SSH server, a database) binds to a port and listens. It sits waiting until a client connects. One machine can run many services at once, each on its own port:
Port	Service running
22	SSH server
80 / 443	Web server
3306	Database (MySQL)
When data arrives for IP `203.0.113.50` on port `443`, the operating system hands it to the program listening on 443.
Serving many clients at once
A web server handles thousands of clients simultaneously. How does it keep them apart? Every connection is uniquely identified by a set of five values: protocol, source IP, source port, destination IP, destination port.
```
TCP  198.51.100.7:51234  →  203.0.113.50:443    (client 1)
TCP  198.51.100.7:51235  →  203.0.113.50:443    (same client, second tab)
TCP  192.0.2.44:60001    →  203.0.113.50:443    (client 2)
```
All three go to the same destination port, but each is a different connection because the source differs.
Behind the scenes
A busy website is usually several layers of servers:
Web server / reverse proxy: receives the client's request (and often handles HTTPS).
Application server: runs the site's logic.
Database server: stores the data.
Load balancer: spreads requests across several identical servers so no single one is overwhelmed, and keeps the site up if one fails.
Servers live in data centers and increasingly run as virtual machines or containers in the cloud, but the networking is exactly what you've learned.
Seeing which ports are open
Every listening port is a way in, so you should know what's listening on your own machines.
Where	Command
Linux	`ss -tuln` (listening TCP/UDP ports, numeric)
Windows	`netstat -an`
Remote check (only on systems you own or have permission to test)	`nmap`
The fewer services listening, the smaller the attack surface.
Other services you'll meet
Service	Port	What it does
SMTP	25 (587)	Sending email between servers and from clients
IMAP / POP3	143 / 110	Reading email from a mail server
SSH	22	Secure remote command line and file transfer (SFTP)
FTP	21	File transfer (unencrypted; prefer SFTP)
NTP	123	Keeps device clocks accurate, which matters for logs and certificates
Syslog	514	Collects log messages from devices
SNMP	161	Monitoring and managing network devices
The Cisco configuration for SSH, NTP, Syslog, SNMP, and DNS is in the CCNA file.
The whole story, revisited
In Section 1 we followed a website request in plain words. Now we can show every protocol involved, in order. You open your laptop, join Wi-Fi, and type `www.example.com`:
#	Protocol	What happens
1	DHCP	Your laptop broadcasts a Discover and receives an IP address, mask, gateway, and DNS server.
2	ARP	It asks "who has the gateway's IP?" and learns the router's MAC address.
3	DNS	It asks the DNS resolver for the IP of `www.example.com` and receives `203.0.113.50`.
4	TCP	It opens a connection to `203.0.113.50:443` with the three-way handshake (SYN, SYN-ACK, ACK).
5	TLS	The server's certificate is verified and an encrypted session is set up.
6	HTTP	Inside the encrypted session, the browser sends `GET /` and the server replies `200 OK` with the page.
7	Routing / NAT	Along the way, each router forwards the packets by IP address; your home router uses PAT to share one public address.
8	Ethernet / Wi-Fi	On every link, the data is wrapped in a frame with new MAC addresses.
Everything from Section 1's "story" is now something you can name and explain.
Key terms
Term	Meaning
DHCP	Protocol that assigns IP settings automatically
DORA	Discover, Offer, Request, Acknowledge: the DHCP steps
Lease	The time a DHCP address is loaned to a device
DHCP relay	A router forwarding DHCP broadcasts to a server on another network
DNS	System that turns names into IP addresses
Resolver	The server that does DNS lookups on a client's behalf
TTL (DNS)	How long an answer can be cached
A / AAAA / CNAME / MX	DNS record types (IPv4 / IPv6 / alias / mail)
HTTP / HTTPS	The web protocol / the same, encrypted with TLS
Status code	A number in the response saying how the request went
Cookie	Small data stored by the browser, used to track sessions
TLS / certificate	Encryption protocol / proof of a server's identity
CA	A trusted authority that signs certificates
Port binding / listening	A program reserving a port and waiting for connections
Load balancer	Spreads requests across multiple servers
Attack surface	The total ways someone could try to get in
Self-check
Try to answer first, then open the answer.
1. Put the four DHCP steps in order and say which ones are broadcasts.
<details>
<summary>Answer</summary>
Discover, Offer, Request, Acknowledge (DORA). Discover and Request are broadcast by the client, because it doesn't have an address yet and doesn't know the server's address.
</details>
2. A device has the address `169.254.12.8`. What does that tell you?
<details>
<summary>Answer</summary>
It was set to get its address automatically but no DHCP server answered, so it gave itself a link-local (APIPA) address. Check DHCP, the cable, and VLAN settings.
</details>
3. Why does a DHCP relay exist?
<details>
<summary>Answer</summary>
DHCP Discover is a broadcast, and routers don't forward broadcasts. A relay (such as `ip helper-address`) forwards the request to a DHCP server located on a different network.
</details>
4. Briefly describe the steps of a DNS lookup that isn't cached.
<details>
<summary>Answer</summary>
The client asks its recursive resolver, which asks a root server (who handles `.com`?), then the `.com` server (who handles `example.com`?), then the authoritative server for the domain, which gives the IP address. The resolver returns it to the client and caches it.
</details>
5. What's the difference between a 404 and a 500 response?
<details>
<summary>Answer</summary>
404 is a client-side error family code: the requested resource wasn't found. 500 is a server error: the server failed while handling a valid request.
</details>
6. Does the HTTPS padlock mean a website is safe?
<details>
<summary>Answer</summary>
No. It means the connection is encrypted and the certificate matches the domain. A malicious or phishing site can have a valid certificate too.
</details>
7. A server has one IP address and one port 443. How does it tell thousands of clients apart?
<details>
<summary>Answer</summary>
Each connection is identified by the combination of protocol, source IP, source port, destination IP, and destination port. Every client uses a different source IP and/or source port, so each connection is unique.
</details>
8. In what order do DHCP, DNS, TCP, TLS, and HTTP happen when you open a site for the first time?
<details>
<summary>Answer</summary>
DHCP (get an address), then DNS (find the server's IP), then TCP (open the connection), then TLS (secure it), then HTTP (request the page). ARP also happens early to find the gateway's MAC.
</details>
<!--NEXT-->
