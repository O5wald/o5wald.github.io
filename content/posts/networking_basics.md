---
layout: post
title: "Networking Basics"
author: Aryan Kapse
date: 2021-11-01
draf: false
---

## Topics Coverd:
1. Netwrok Programming and C
2. OSI and TCP/IP
3. The Internet Protocol
4. IPv4 and IPv6
5. Domain Names
6. Internet Protocol routing
7. Network Address translation
8. some more Topics

---
## The internet and C
millions of desktops, laptops,routers and servers conneted to the internet
and have been for decades. the new **Internet of Things(IoT)** trend atracting peoples to use and learn Networking. Almost every network stack is programmed in C. This is true for Windows, Linux, and macOS. If your mobile phone uses Android or iOS, then even though the apps for these were programmed in a different language (Java and Objective C), the kernel and networking code was written in C. It is very likely that the network routers that your internet data goes through are programmed in C. Even if the user interface and higher-level functions of your modem or router are programmed in another language, the networking drivers are still probably implemented in C.

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSY2TIdddP8CpLBqo2rpql7iApvbDvWkTbNTw&usqp=CAU"/>


---
# OSI model (Open Systems Interconnection)

The most popular layer system for networking is called the Open Systems Interconnection
model (**OSI model**). It was standardized in 1977 and is published as ISO 7498. It has seven
layers

<img src="https://images.slideplayer.com/34/10179711/slides/slide_4.jpg"/>

| Layer number | Layer |
|:----:		| :----:|
| 7			| Application|
|6			| presentation|
| 5			| Session|
| 4			| Transport|
| 3			| Network|
| 2			| Data Link|
| 1			| Physical|


- **1.Physical** : voltage levels on an ithernet cables,the readio frequency of Wifi etc....
- **2.Data link** : this level build on physical layer, It deals with protocols for directly communicating between two nodes.It defines how a direct message between nodes starts and ends (framing), error detection and correction, and flow control.
- **3.Network layer** : the netwrok layer provides the methods to transmit data sequences (called packets). This is the layer that the internet protocol is defined on.
-  **4.Transport Layer** : at this layer we hace methods to reliably deliver variable length data between hosts. the **The transmission control protocol(TCP)** and **User datagram protocol(UDP)** are commonly said to exist on this layer
- **5.Session Layer**: this layer build on the transport later by adding methods to establish, checkpoint, suspend,resume, and terminate dialogs.
- **6.Presentation Layer**: this is the lowest layer at which data structure  and presentation for an appliation are defined. Concerns such as data encoding , serialization, and encryption are handled here.
- **7.Application layer**: The application that the user interfaces with(Ex. web browser and email clients etc....).


# TCP/IP (Transmission Control Protocol/Internet Protocol)

- OSI model has 7 layers were TCP/IP has 4 layers

| Layer number | Layer |
| --- | --- |
| 4   | Process/Application |
| 3   | Host-to-Host |
| 2   | internet |
| 1   | Network Access |

- In both models , same function are performed;they are just divided diffrently.

- **1.Network Access** : physical connection and data framing happen.(for Example, sending Ethernet and Wi-Fi packet).

- **2.Internet** : This layer deals with the concerns of addressing packets and routing them over multiple interconnection networks. IP address is defined at this layer.

- **3.Host-to-Host** : this provides two protocols TCP/IP and UDP.These protocols address concerns such as data order,data segmentation,network congestion, and error correction.

- **4.Process/Application layer** : protocols such as HTTP, SMTP, and FTP are implemented.

# Internet Protocol

there are two Versions

1.  IPv4
2.  IPv6

**IPv4** : IPv4 uses 32-bit addresses, which limits it to addressing no more than 232 or 4,294,967,296 systems. However, these 4.3 billion addresses were not initially assigned efficiently, and now many Internet Service Providers (ISPs) are forced to ration IPv4 addresses

**IPv6** : IPv6 was designed to replace IPv4 and has been standardized by the Internet Engineering Task Force (IETF) since 1998. It uses a 128-bit address, which allows it to address a theoretical 2128 = 340,282,366,920,938,463,463,374,607,431,768,211,456, or about a 3.4 x 1038 addresses
how it would be

* * *

# What is address?

- All internet protocol traffic routes to and address.
- IPv4 addresses are 32 bits long. They are commonly divided into four 8-bit sections. Each section is displayed as a decimal number between 0 and 255 inclusive and is delineated by a period.
- Example
    - 0.0.0.0
    - 127.0.0.1
    - 10.0.0.0
    - 176.15.0.1
    - 192.168.1.1
    - 255.255.255.255
- A special address, called the loopback address, is reserved at `127.0.0.1`. this means `establish a connection to myself`.
- Operating systems short-circuit thisaddress so that packets to it never enter the network but instead stay local on theoriginating system.

* * *

### IPv4 reserves some address ranges for private use.

- if you're using **IPv4** through a router/NAT, then you are likely using an IP address in one of the ranges.

    - 10.0.0.0 **to** 10.255.255.255
    - 172.16.0.0 **to** 172.31.255.255
    - 192.168.0.0 **to** 192.168.255.255
- There is shorthand notation for writing them. Using **Classless Inter-Domain Routing (CIDR)** notion, we can write the three previous address range as follows:

    - 10.0.0.0/18
    - 172.16.0.0/12
    - 192.168.0.0/16
- CIDR notation works by specifying the number of bits that are fixed. For example, 10.0.0.0/8 specifies that the first eight bits of the 10.0.0.0 address are fixed,the first eight bits being just the first 10. part; the remaining 0.0.0 part of the address canbe anything and still be on the 10.0.0.0/8 block. Therefore, 10.0.0.0/8 encompasses 10.0.0.0 through 10.255.255.255

---
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSWv6-rJekczOJx3547PIEEQ1cC5Z2p9mTVTQ&usqp=CAU"/>

# IPv6
-  IPv6 addresses are 128 bits long. They are written as eight groups of four hexadecimalcharacters delineated by colons. A hexadecimal character can be from 0-9 or from a-f. Hereare some examples of IPv6 addresses:
	-  0000:0000:0000:0000:0000:0000:0000:0001
	-  2001:0db8:0000:0000:0000:ff00:0042:8329
	-  fe80:0000:0000:0000:75f4:ac69:5fa7:67f9

- ### Rules for shortening IPv6
	- Leading zeros in each section to be omitted(ex. 0db7= db7)
	- consecutive sections of zeros to be replaced with the double colon `::`
		- example
		- : : 1
		- 2001:db7:ab5:ff0
		- fe80:75d6:abb7:65af
		- ffff:ffff:ffff:ffff
- Like IPv4, IPv6 also has a loopback address ::1

- Dual-stack implementations also recognize a special class of IPv6 address that map directly to an IPv4 address. These reserved addresses start with 80 zero bits, and then by 16 one bits, followed by the 32-bit IPv4 address. Using CIDR notation, this block of address is ::ffff:0:0/96

| IPv6 Address | Mapped IPv4 Address |
|--------------|---------------------|
|::ffff:10.0.0.0|10.0.0.0|
| ::ffff:172.16.0.5|172.16.0.5|
|etc....

- Another address type that you should be familiar with are **link-local addresses**. Link-local addresses are usable only on the local link. Routers never forward packets from these addresses. They are useful for a system to accesses auto-configuration functions before having an assigned IP address. Link-local addresses are in the **IPv4 169.254.0.0/16** address block or the **IPv6 fe80::/10** address block

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQOZKZw6r9TdaeepSMPiSSxEjNDb6_QUz8zgg&usqp=CAU"/>

---

- **If you think that IPv4 addresses are difficult to memorize, and IPv6 addresses impossible, then you are not alone. Luckily, we have a system to assign names to specific addresses.**

# Domain names

- The Internet Protocol can only route packets to an IP address, not a name. So, if you try to connect to a website, such as `example.com`, your system must first resolve that domain name, `example.com`, into an IP address for the server that hosts that website.
- This is done by connecting to a **Domain Name System (DNS)** server. You connect to a domain name server by knowing in advance its IP address. The IP address for a domain name server is usually assigned by your ISP.
- we all serve through public DNS servers like Google, Cloudflare etc....
- To resolve a hostname, your computer sends a UDP message to your domain name serverand asks it for an AAAA-type record for the domain you're trying to resolve. If this record exists, an IPv6 address is returned. You can then connect to a server at that address to load the website.
- if no AAAA record exist, then your computer queries server for A type record. if this record will exist then you receive IPv4 address.

---

# Internet routing

- the internet today has an estimated 20 billion devices connected.
- When you make a connection over the internet, your data first transmits to your local router, from there it transmited to another router which is connected to another router and that is connected to receiving device, at which point, the data reached its destination.


![](https://static.packt-cdn.com/products/9781789349863/graphics/893c8ea4-7385-4bbd-8af8-33a425fb412c.png)
- imagine each router in the preceding diagram is connected to tens,hundreds, or even thousands of other router and systems.
	- you can see this devices using tools like
	- `tracert` on windows

        ![](https://www.colocationamerica.com/wp-content/uploads/2015/08/tracert-speed-test.jpg)
	- `traceroute` on Linux

		![](https://www.hostdime.com/kb/hd/files/7374064/7374066/1/1541179921000/Traceroute+Results+-+Mac.png)
	- in this images you can see the IP address, each one of them are devices.

- later we can see how proxy chains and Tor networks works (in Hacking section).

---

# Local networks and address translation

- as we know previously, there are IPv4 addresses ranges reserved for use in the small local networks.
- private ranges:
	- 10.0.0.0 **to** 10.255.255.255
	- 172.16.0.0 **to** 172.31.255.255
	- 192.168.0.0 **to** 192.168.255.255

- when a packet originates from a device on an IPv4 local network, it must undergo **Netwok Address Translation(NAT)** before being routed on the internet.
- **A router that implements NAT remembers which local address a connection is established from.**
	- The router does this by modifying the source IP address from the original private LAN IP address to its public internet IP address.
	- ![](https://techdifferences.com/wp-content/uploads/2018/02/NAT-example.jpg)
	- Likewise, when the router receives the return communication, it must modify the destination address from its public IP to the private IP of the original sender

---

# Subnetting and CIDR

- IP addresses can be split into parts. The most significant bits are used to identify the network or subnetwork, and the least significant bits are used to identify the specific device on the network. (same as your home address split into parts )

- IPv4 use mask notation to identify the IP address parts.
  - consider a router on the 10.0.0.0 network with subnet mask of 255.255.255.0. This router can take any incoming packet and perform bitwise `AND` operation with the `subnet mask` to determine whether the packet belongs on the local subnet or needs to be forwarded on
    - router receives a packet to be delivered to `10.0.0.105`. It does a
bitwise AND operation on this address with the subnet mask of `255.255.255.0`, which
produces `10.0.0.0`. That matches the subnet of the router, so the traffic is local. If, instead,
we consider a packet destined for `10.0.15.22`, the result of the bitwise AND with the
subnet mask is `10.0.15.0`. This address doesn't match the subnet the router is on, and so it
must be forwarded

- IPv6 uses CIDR. Networks and subnetworks are specified using the **CIDR** notation we
described earlier
  - if the IPv6 subnet is /112`, then the router knows that any
address that matches on the first 112 bits is on the local subnet
---

<img src="https://i.kym-cdn.com/photos/images/original/001/779/384/c9c.jpg"/>


# What is your IP address?

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR9vyf_BjVnfXcB_q7jrTU6n2_02bLgmW7vzQ&usqp=CAU" />

#### You can find your IP address using:
- `ipconfig` on windows

  - <img src="https://www.lifewire.com/thmb/oEfzSEqLR3eqLCotabNd0qSbAzw=/754x424/smart/filters:no_upscale()/2019-03-19_16h09_50-5c914cb946e0fb0001770166.png"/>
  - `Ethernet adapter` means you are using ethernet not wireless
  - `IPv4 Address` is your LAN address
  - `Default Gateway` is your Router address
  - `Subnet Mask ` is subnet mask which your network belongs to.


- `ifconfig` or `ip addr` on linux

  - <img src="https://4.bp.blogspot.com/-dUXPutX9hjE/XNHOEFF11JI/AAAAAAAAEB0/339t49Z50hgow8uZNKf5sPeJRXUJmkqoQCLcBGAs/s1600/ifconfig-command.png" />
  - `eth0` means you are using Ethernet
  - `inet` is your LAN address in IPv4
  - `inet6` is your LAN address in IPv6
- few public API for finding your `Public IP address`

  - `api.ipfy.org`

    <img src="https://user-images.githubusercontent.com/19902259/63141012-0dd39500-c01f-11e9-8c11-c0b0bed218b4.png"/>

  - `Google`

    <img src="https://cdn.ourcodeworld.com/public-media/gallery/gallery-5e948b0844465.png"/>
---

***
