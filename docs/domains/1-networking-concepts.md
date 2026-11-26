<!-- nav:top -->
[Contents](../../README.md#contents) · Previous: [5-minute diagnostic](../diagnostic.md) · Next: [Domain 2: Network Implementation](2-network-implementation.md)
<!-- /nav:top -->

# Domain 1: Networking Concepts

**Weight:** 23% of the exam (CompTIA's figure), the second heaviest domain. By our estimate that is roughly 20 of up to 90 questions. **Objectives:** 1.1 to 1.8.

This is the vocabulary and arithmetic of networking: the OSI model, what each appliance does, cloud networking, ports and protocols, cables and transceivers, topologies, IPv4 addressing and the newer ideas such as SDN, zero trust and IPv6. Most marks here are lost on details that feel easy until a scenario twists them: a port number off by one, the wrong fiber type for a distance, or a subnet boundary worked out in a hurry. Learn the tables cold and practice the subnet method until it is automatic.

## What this domain measures

| Objective | What it asks you to do | Topics you must know |
|---|---|---|
| 1.1 | Explain the OSI reference model | The seven layers, what happens at each, PDUs, which devices and protocols sit where |
| 1.2 | Compare networking appliances, applications and functions | Routers, switches, firewalls, IDS and IPS, load balancers, proxies, NAS, SAN, APs and controllers, CDN, VPN, QoS, TTL |
| 1.3 | Summarize cloud concepts and connectivity | NFV, VPC, security groups and lists, internet and NAT gateways, VPN and Direct Connect, public, private and hybrid, SaaS, IaaS, PaaS, scalability, elasticity, multitenancy |
| 1.4 | Explain ports, protocols, services and traffic types | The 20 listed protocols and ports, TCP vs UDP, ICMP, GRE, IPSec (AH, ESP, IKE), unicast, multicast, anycast, broadcast |
| 1.5 | Compare transmission media and transceivers | 802.11, cellular, satellite, 802.3, single-mode vs multimode fiber, DAC and twinax, coax, cable speeds, plenum, SFP, QSFP, Ethernet vs Fibre Channel, connector types |
| 1.6 | Compare topologies, architectures and types | Mesh, hybrid, star and hub and spoke, spine and leaf, point to point, three-tier, collapsed core, north-south and east-west traffic |
| 1.7 | Use appropriate IPv4 addressing (scenario) | Public vs private, APIPA, RFC 1918, loopback, subnetting, VLSM, CIDR, classes A to E |
| 1.8 | Summarize evolving use cases for modern networks | SDN and SD-WAN, VXLAN, zero trust, SASE and SSE, infrastructure as code, source control, IPv6 and transition methods |

## Key concepts explained

### 1.1 OSI model

The OSI model splits networking into seven layers. Each layer serves the one above and uses the one below. When data is sent, each layer adds its own header (encapsulation); the receiver strips them off in reverse (de-encapsulation). A memory aid from the bottom up: "Please Do Not Throw Sausage Pizza Away".

| Layer | Name | What it does | PDU | Examples |
|---|---|---|---|---|
| 7 | Application | Network services the user's software talks to | Data | HTTP, DNS, SMTP, SSH, FTP |
| 6 | Presentation | Formatting, character encoding, encryption and compression | Data | TLS encryption, JPEG, ASCII |
| 5 | Session | Opens, manages and closes sessions between hosts | Data | RPC, NetBIOS sessions, SQL sessions |
| 4 | Transport | End-to-end delivery, ports, segmentation, reliability | Segment (TCP) or datagram (UDP) | TCP, UDP |
| 3 | Network | Logical addressing and routing between networks | Packet | IP, ICMP, routers, layer 3 switches |
| 2 | Data link | Framing, MAC addresses, delivery on the local segment | Frame | Ethernet, switches, bridges, Wi-Fi MAC, ARP |
| 1 | Physical | Bits as signals on copper, fiber or radio | Bits | Cables, connectors, hubs, repeaters, transceivers |

**How to place a device.** Ask what address it reads. A hub reads nothing and just repeats bits (layer 1). A switch reads MAC addresses (layer 2). A router reads IP addresses (layer 3). A firewall filtering on ports works at layer 4; one that inspects HTTP content works up to layer 7. A load balancer can work at layer 4 (IP and port) or layer 7 (URL, cookies).

**How to use it in troubleshooting.** "Bottom up" starts at layer 1: is there link light, is the cable good? A wrong VLAN is a layer 2 problem. A wrong gateway or IP address is layer 3. A blocked port is layer 4. A certificate error in the browser sits at the presentation and application layers.

### 1.2 Appliances and functions

| Appliance | What it does | How to recognize it in a question |
|---|---|---|
| Router | Forwards packets between networks using IP addresses and a routing table | "Connect two subnets", "default gateway", "forward to the internet" |
| Switch | Forwards frames inside a network using a MAC address table | "Connect hosts in the same office", VLANs, ports |
| Firewall | Allows or blocks traffic by rules (addresses, ports, applications) | "Block", "permit", rule order, DMZ |
| IDS | Watches a copy of traffic and alerts on attacks | "Alert only", connected to a SPAN port or tap |
| IPS | Sits inline and can drop attack traffic | "Block in real time", inline |
| Load balancer | Spreads requests across a pool of servers, checks their health | "Many web servers behind one address", "remove a failed server" |
| Proxy | Makes requests on behalf of clients; can cache and filter | "Filter web browsing", "cache content", forward or reverse proxy |
| NAS | A storage device on the LAN serving files | File-level access over SMB or NFS |
| SAN | A dedicated storage network giving servers raw disk volumes | Block-level access over Fibre Channel or iSCSI |
| Access point | Connects wireless clients to the wired network | "Wireless coverage in a room" |
| Wireless controller | Centrally manages many lightweight APs: config, channels, power, roaming | "Manage 200 APs from one place" |

**Physical vs virtual.** Every appliance above can be a box in a rack or a virtual machine. Virtual appliances are quick to deploy and scale, but share the host's resources.

**CDN (content delivery network).** A distributed set of servers that cache content close to users, so a user in Tokyo gets images from a nearby node rather than from the origin server in Virginia. It lowers latency and protects the origin from load.

**Functions.**

- **VPN:** an encrypted tunnel across an untrusted network. Site-to-site joins two networks; client-to-site (remote access) joins one user to the network.
- **QoS (quality of service):** classifies and prioritizes traffic so latency-sensitive traffic such as voice and video goes first when links are busy. Traffic is often marked with DSCP values in the IP header.
- **TTL (time to live):** in IP, a hop counter. Each router lowers it by 1, and a packet that reaches 0 is dropped and an ICMP "time exceeded" message is sent back. This stops packets looping forever, and traceroute uses it to discover each hop. In DNS, TTL means something different: how long a resolver may cache a record.

### 1.3 Cloud concepts

| Term | Meaning |
|---|---|
| NFV (network functions virtualization) | Running network functions such as routers, firewalls and load balancers as software on standard servers |
| VPC (virtual private cloud) | Your own isolated network inside a public cloud, with your own subnets and routing |
| Network security group | Firewall rules applied to instances or their interfaces, typically stateful |
| Network security list | Firewall rules applied at the subnet level; depending on the provider they can be stateless, so return traffic needs its own rule |
| Internet gateway | Gives a VPC's public subnets two-way access to and from the internet |
| NAT gateway | Lets instances in private subnets start outbound connections (for updates) while blocking unsolicited inbound connections |
| Cloud VPN | An encrypted tunnel over the internet between your site and the cloud |
| Direct Connect | A dedicated private link from your data center to the cloud provider, not over the public internet, with more predictable performance |

**Deployment models.** **Public** cloud is shared infrastructure run by a provider. **Private** cloud is dedicated to one organization, on premises or hosted. **Hybrid** combines them, for example a private data center that bursts to a public cloud.

**Service models.**

| Model | You manage | Provider manages | Example |
|---|---|---|---|
| SaaS | Your data and user accounts | The application and everything below it | Web email, online office suites |
| PaaS | Your application code and data | Runtime, OS, servers, network | A managed app hosting platform |
| IaaS | OS, applications, data | Physical servers, storage, network, virtualization | Renting virtual machines |

**Scalability vs elasticity.** Scalability is the ability to grow to meet demand (add capacity). Elasticity is doing it automatically in both directions, adding resources at peak and releasing them when demand drops. **Multitenancy** means many customers share the same physical infrastructure while being logically isolated from each other.

### 1.4 Ports protocols and traffic types

Know every row. CompTIA often gives the protocol and asks for the port, or describes a task and asks for the protocol.

| Protocol | Port | Transport | What it is for |
|---|---|---|---|
| FTP | 20, 21 | TCP | File transfer, unencrypted. 21 is control, 20 is data (active mode) |
| SFTP | 22 | TCP | File transfer over SSH, encrypted |
| SSH | 22 | TCP | Encrypted remote command line |
| Telnet | 23 | TCP | Unencrypted remote command line. Replace with SSH |
| SMTP | 25 | TCP | Sending mail between mail servers |
| DNS | 53 | UDP and TCP | Name resolution. UDP for most queries, TCP for zone transfers and large responses |
| DHCP | 67, 68 | UDP | Automatic IP configuration. Server listens on 67, client on 68 |
| TFTP | 69 | UDP | Simple file transfer with no login, used for device configs and network boot |
| HTTP | 80 | TCP | Web traffic, unencrypted |
| NTP | 123 | UDP | Time synchronization |
| SNMP | 161, 162 | UDP | Device monitoring. 161 for queries to agents, 162 for traps sent to the manager |
| LDAP | 389 | TCP | Directory queries, unencrypted |
| HTTPS | 443 | TCP | Web traffic over TLS |
| SMB | 445 | TCP | Windows file and printer sharing |
| Syslog | 514 | UDP | Sending log messages to a central log server |
| SMTPS | 587 | TCP | Mail submission from clients, secured with TLS (listed as SMTPS on the objectives) |
| LDAPS | 636 | TCP | LDAP over TLS |
| SQL Server | 1433 | TCP | Microsoft SQL Server database connections |
| RDP | 3389 | TCP | Remote desktop to Windows hosts |
| SIP | 5060, 5061 | 5060 UDP or TCP, 5061 TCP with TLS | Setting up and ending VoIP calls. 5061 is the encrypted version |

**Secure and insecure pairs to remember:** Telnet 23 to SSH 22, FTP 20 and 21 to SFTP 22, HTTP 80 to HTTPS 443, LDAP 389 to LDAPS 636, SMTP 25 to SMTPS 587, SIP 5060 to 5061.

**TCP vs UDP.**

| | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented, three-way handshake (SYN, SYN-ACK, ACK) | Connectionless, just sends |
| Reliability | Acknowledgments, retransmission, ordering, flow control | None built in |
| Overhead | Higher | Lower |
| Typical use | Web, email, file transfer, remote access | DNS queries, DHCP, VoIP media, streaming, TFTP, NTP, SNMP |

**Other IP protocol types.** These ride directly on IP, not on TCP or UDP, so they have IP protocol numbers instead of ports.

| Type | IP protocol number | Purpose |
|---|---|---|
| ICMP | 1 | Error and diagnostic messages: ping (echo request and reply), time exceeded, destination unreachable |
| TCP | 6 | Reliable transport |
| UDP | 17 | Lightweight transport |
| GRE | 47 | Generic tunneling. Wraps other packets, including multicast and routing traffic, but adds no encryption |
| IPSec ESP | 50 | Encrypts the payload and also provides integrity and authentication |
| IPSec AH | 51 | Integrity and authentication only, no encryption |
| IPSec IKE | UDP 500 (UDP 4500 through NAT) | Negotiates the keys and security associations for IPSec |

A common exam pattern: GRE over IPSec. GRE carries traffic that IPSec alone cannot (such as routing protocol multicasts), and IPSec encrypts the GRE tunnel.

**Traffic types.**

| Type | Sent to | Example |
|---|---|---|
| Unicast | One specific host | Loading a web page |
| Multicast | A group of hosts that joined (IPv4 224.0.0.0 to 239.255.255.255) | IPTV streams, OSPF updates |
| Anycast | The nearest of several hosts sharing one address, chosen by routing | Public DNS resolvers, CDN nodes |
| Broadcast | Every host on the local network segment (255.255.255.255 or the subnet broadcast) | DHCP discover, ARP request |

IPv6 has no broadcast; it uses multicast instead. Routers do not forward broadcasts, so a router marks the edge of a broadcast domain.

### 1.5 Media and transceivers

**Wireless.** The main 802.11 standards:

| Standard | Wi-Fi name | Band | Maximum theoretical rate |
|---|---|---|---|
| 802.11a | | 5 GHz | 54 Mbps |
| 802.11b | | 2.4 GHz | 11 Mbps |
| 802.11g | | 2.4 GHz | 54 Mbps |
| 802.11n | Wi-Fi 4 | 2.4 and 5 GHz | 600 Mbps |
| 802.11ac | Wi-Fi 5 | 5 GHz | About 6.9 Gbps |
| 802.11ax | Wi-Fi 6 and 6E | 2.4 and 5 GHz, plus 6 GHz for 6E | About 9.6 Gbps |

**Cellular** (4G LTE, 5G) gives mobile WAN access and backup links. **Satellite** reaches remote sites with no cable. Geostationary satellites are far away, so latency is high (hundreds of milliseconds), which hurts voice and interactive apps. Low earth orbit services have much lower latency.

**Copper (802.3).** Twisted-pair Ethernet has a 100 meter channel limit.

| Cable | Typical speed and distance |
|---|---|
| Cat 5e | 1 Gbps (1000BASE-T) to 100 m |
| Cat 6 | 1 Gbps to 100 m; 10 Gbps up to about 55 m |
| Cat 6a | 10 Gbps (10GBASE-T) to 100 m |
| Cat 8 | 25 or 40 Gbps up to 30 m, for data center links |

**Fiber: single-mode vs multimode.**

| | Multimode (MMF) | Single-mode (SMF) |
|---|---|---|
| Core | Larger (50 or 62.5 microns) | Small (about 9 microns) |
| Light source | LED or VCSEL, usually 850 nm | Laser, 1310 or 1550 nm |
| Distance | Short: hundreds of meters (for example 10GBASE-SR, up to about 300 to 400 m) | Long: kilometers (10GBASE-LR to 10 km, 10GBASE-ER to 40 km) |
| Typical use | Inside a building or data center | Between buildings, campuses, carrier links |
| Cost | Cheaper optics | More expensive optics |

The "S" and "L" in names help: 1000BASE-SX and 10GBASE-SR are short reach on multimode; 1000BASE-LX and 10GBASE-LR are long reach on single-mode.

**Other cable types.**

- **DAC (direct attach copper):** a twinaxial cable with transceivers fixed on both ends. Cheap and low latency for short runs, such as a server to a top-of-rack switch, a few meters.
- **Coaxial:** a center conductor with shielding. RG-6 carries cable TV and cable internet with F-type connectors.
- **Plenum vs non-plenum:** plenum spaces carry building air (above drop ceilings, under raised floors). Plenum-rated cable has a fire-retardant, low-smoke jacket and is required there by fire codes. Non-plenum (usually PVC) gives off toxic smoke when it burns.

**Transceivers.** A transceiver converts electrical signals to light or copper signals and plugs into a switch port. The protocol must match: **Ethernet** for LANs, **Fibre Channel** for SANs.

| Form factor | Common speeds |
|---|---|
| SFP | 1 Gbps |
| SFP+ | 10 Gbps (SFP28: 25 Gbps) |
| QSFP | Four lanes in one module: QSFP+ 40 Gbps, QSFP28 100 Gbps |

Both ends must match in wavelength, fiber type and speed. An LR optic on one side and an SR optic on the other will not link.

**Connectors.**

| Connector | Medium | How to recognize it |
|---|---|---|
| SC (subscriber connector) | Fiber | Square, push-pull |
| LC (local connector) | Fiber | Small, latching, the usual connector on SFP modules |
| ST (straight tip) | Fiber | Round, bayonet twist-lock, older installs |
| MPO (multi-fiber push on) | Fiber | One rectangular connector carrying 12 or more fibers, used for 40 and 100 Gbps |
| RJ11 | Copper | Small, telephone lines and DSL |
| RJ45 | Copper | 8-pin, twisted-pair Ethernet |
| F-type | Coax | Threaded screw-on, cable modems and TV |
| BNC | Coax | Bayonet twist-lock, video and legacy equipment |

### 1.6 Topologies and architectures

| Topology | Shape | Strength | Weakness |
|---|---|---|---|
| Mesh | Every node links to many or all others | Many redundant paths | Costly cabling and ports; full mesh needs n(n-1)/2 links |
| Star, hub and spoke | Everything connects to a central point | Simple, easy to manage | The center is a single point of failure; spoke-to-spoke traffic goes through the hub |
| Point to point | One direct link between two sites | Simple, dedicated | Only two endpoints |
| Hybrid | A mix, such as stars joined by a mesh core | Fits real networks | More complex to document |
| Spine and leaf | Every leaf switch connects to every spine switch, no leaf-to-leaf or spine-to-spine links | Every server is the same number of hops from every other, scales by adding switches | Needs many uplinks |

**Three-tier hierarchical model.**

- **Core:** the fast backbone. Moves traffic quickly between distribution blocks; avoid heavy filtering here.
- **Distribution:** aggregates access switches, applies policy, routing between VLANs and filtering.
- **Access:** where end devices plug in. Port security, VLAN assignment, PoE for phones and APs.

**Collapsed core** merges the core and distribution layers into one layer. It suits small and medium sites where a separate core would be wasted money.

**Traffic flows.** **North-south** traffic enters or leaves the data center (client to server, data center to internet). **East-west** traffic moves between servers inside it (app server to database). Modern applications create a lot of east-west traffic, which is why spine and leaf replaced the three-tier model in many data centers.

### 1.7 IPv4 addressing

**Classes.** Classful addressing is history, but the exam still asks about it.

| Class | First octet | Default mask | Use |
|---|---|---|---|
| A | 1 to 126 | 255.0.0.0 (/8) | Large networks |
| B | 128 to 191 | 255.255.0.0 (/16) | Medium networks |
| C | 192 to 223 | 255.255.255.0 (/24) | Small networks |
| D | 224 to 239 | None | Multicast |
| E | 240 to 255 | None | Experimental, reserved |

127 is missing from class A because 127.0.0.0/8 is reserved for **loopback**. 127.0.0.1 (localhost) always means "this host", so pinging it tests the local TCP/IP stack, not the network.

**Private and special ranges.**

| Range | CIDR | What it means |
|---|---|---|
| 10.0.0.0 to 10.255.255.255 | 10.0.0.0/8 | RFC 1918 private |
| 172.16.0.0 to 172.31.255.255 | 172.16.0.0/12 | RFC 1918 private |
| 192.168.0.0 to 192.168.255.255 | 192.168.0.0/16 | RFC 1918 private |
| 169.254.0.0 to 169.254.255.255 | 169.254.0.0/16 | APIPA (link-local): the host could not reach a DHCP server and gave itself an address |
| 127.0.0.0 to 127.255.255.255 | 127.0.0.0/8 | Loopback |

Private addresses are not routed on the internet. To reach it, they are translated to a public address by NAT. A host with a 169.254.x.x address can only talk to others on the same link, so the fix is to find out why DHCP failed (cable, VLAN, DHCP server, scope full, relay missing).

**CIDR and masks.** CIDR writes the mask as the number of network bits: /24 means 24 ones. The rest are host bits. Usable hosts = 2 to the power of host bits, minus 2 (the network address and the broadcast address).

| Prefix | Mask | Addresses | Usable hosts |
|---|---|---|---|
| /20 | 255.255.240.0 | 4,096 | 4,094 |
| /21 | 255.255.248.0 | 2,048 | 2,046 |
| /22 | 255.255.252.0 | 1,024 | 1,022 |
| /23 | 255.255.254.0 | 512 | 510 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

**A subnetting method that works under time pressure.**

1. Find the **interesting octet**: the one where the mask is not 255 or 0.
2. **Block size** = 256 minus the mask value in that octet.
3. List multiples of the block size in that octet: 0, block, 2 x block, and so on. The **network address** is the multiple at or below the host's value.
4. The **broadcast address** is the next network address minus 1.
5. Usable hosts run from network + 1 to broadcast - 1.

**Worked example 1: 192.168.10.77/27.**

- /27 is 24 + 3, so the fourth octet has 3 network bits: 128 + 64 + 32 = 224. Mask 255.255.255.224.
- Block size 256 - 224 = 32. Multiples: 0, 32, 64, 96. 77 falls between 64 and 96.
- Network 192.168.10.64. Broadcast 192.168.10.95.
- Usable hosts 192.168.10.65 to 192.168.10.94, which is 2^5 - 2 = 30 hosts.

**Worked example 2: 10.4.100.9/20.**

- /20 is 16 + 4, so the third octet has 4 network bits: 128 + 64 + 32 + 16 = 240. Mask 255.255.240.0.
- Block size 256 - 240 = 16, in the third octet. Multiples: 80, 96, 112. 100 falls between 96 and 112.
- Network 10.4.96.0. Broadcast is one below the next network (10.4.112.0), so 10.4.111.255.
- Usable hosts 10.4.96.1 to 10.4.111.254, which is 2^12 - 2 = 4,094 hosts.

**Working backwards from a host count.** Need 50 hosts? Find the smallest power of 2 whose value minus 2 is at least 50: 64 - 2 = 62. 64 addresses means 6 host bits, so 32 - 6 = /26.

**VLSM (variable length subnet masking)** gives each subnet a mask that fits its need instead of making every subnet the same size. Allocate the largest first so the blocks line up. Splitting 192.168.50.0/24 for 100 users, 50 users, 20 users and two router links:

| Need | Prefix | Subnet | Usable range | Broadcast |
|---|---|---|---|---|
| 100 hosts | /25 (126) | 192.168.50.0/25 | .1 to .126 | 192.168.50.127 |
| 50 hosts | /26 (62) | 192.168.50.128/26 | .129 to .190 | 192.168.50.191 |
| 20 hosts | /27 (30) | 192.168.50.192/27 | .193 to .222 | 192.168.50.223 |
| Link 1 | /30 (2) | 192.168.50.224/30 | .225 to .226 | 192.168.50.227 |
| Link 2 | /30 (2) | 192.168.50.228/30 | .229 to .230 | 192.168.50.231 |

**Same subnet or not?** Two hosts can talk directly only if they share a network address. Work out each host's network address with the method above and compare. If they differ, traffic must go through a router.

### 1.8 Modern network environments

**SDN and SD-WAN.** Software-defined networking separates the **control plane** (deciding where traffic goes) from the **data plane** (forwarding it), with a central controller programming the devices. SD-WAN applies this to branch WAN links:

- **Application aware:** recognizes applications and steers them, for example voice over the best-quality link.
- **Zero-touch provisioning:** a new branch device is shipped, plugged in, and pulls its configuration automatically.
- **Transport agnostic:** uses any mix of links (broadband, MPLS, cellular).
- **Central policy management:** one policy defined centrally and pushed to every site.

**VXLAN** wraps layer 2 Ethernet frames inside UDP packets (port 4789) so a layer 2 segment can stretch across a layer 3 network. It uses a 24-bit identifier, allowing about 16 million segments compared with 4,094 usable VLANs. A key use is **data center interconnect (DCI)**: extending the same layer 2 network across data centers so virtual machines can move between them.

**Zero trust architecture (ZTA).** Never trust based on network location; verify every request. Access depends on **policy-based authentication** (identity, device health, context), explicit **authorization** for each resource, and **least privilege access**.

**SASE and SSE.** Secure Access Service Edge combines networking (SD-WAN) and security services delivered from the cloud close to the user. Security Service Edge is the security part alone: secure web gateway, cloud access security broker, zero trust network access. The objectives write SASE as "Secure Access Secure Edge"; the usual industry expansion is Secure Access Service Edge.

**Infrastructure as code (IaC).** Network and server configuration described in files that tools apply automatically.

- **Automation:** playbooks and templates are reusable tasks; the same template builds 50 switches the same way.
- **Configuration drift:** devices slowly differ from the approved standard after manual changes. IaC tools detect drift and bring devices back into **compliance**.
- **Upgrades** can be rolled out the same way, and **dynamic inventories** build the list of target devices automatically rather than from a hand-kept list.
- **Source control:** configs live in a **central repository** with **version control** (every change recorded and reversible). **Branching** lets people work on changes separately, and **conflict identification** flags when two changes touch the same lines before they are merged.

**IPv6.** 128-bit addresses written as eight groups of four hex digits, for example 2001:0db8:0000:0000:0000:0000:0000:0001. Shorten by dropping leading zeros in a group and replacing one run of all-zero groups with "::", giving 2001:db8::1. "::" may appear only once.

| Address type | Prefix | Note |
|---|---|---|
| Global unicast | 2000::/3 | Publicly routable |
| Unique local | fc00::/7 | Private, similar in purpose to RFC 1918 |
| Link-local | fe80::/10 | Every IPv6 interface has one; not routed |
| Multicast | ff00::/8 | Replaces broadcast |
| Loopback | ::1 | Same role as 127.0.0.1 |

IPv6 exists mainly for **mitigating address exhaustion**: IPv4's roughly 4.3 billion addresses ran out. Because IPv4 and IPv6 cannot talk to each other directly, **compatibility** needs one of:

- **Dual stack:** devices run IPv4 and IPv6 side by side. The usual first choice.
- **Tunneling:** IPv6 packets carried inside IPv4 across parts of the network that only speak IPv4.
- **NAT64:** translates between IPv6-only clients and IPv4-only servers, usually with DNS64 creating IPv6 answers for IPv4-only names.

## How questions look

Expect many short recall items ("Which port does LDAPS use?") mixed with scenarios that hide the same fact: a firewall must allow encrypted directory lookups, so you need 636. Other common stems: which appliance or cloud component fits a requirement, which fiber and connector fit a distance, which topology suits a data center, which layer a fault sits at, and subnet questions ("Which address is the broadcast address?", "Which mask gives at least 50 hosts?", "Are these two hosts in the same subnet?"). Exhibits include ipconfig output, short routing or address tables and port lists. Performance-based items in this domain commonly ask you to match ports to protocols, drag devices to OSI layers, or fill in a subnetting table.

## A method for these questions

1. Name the objective first: OSI, appliance, cloud, port, media, topology, addressing or modern trends. It narrows what the answer can be.
2. Underline the deciding words: "encrypted", "inbound", "outbound only", "10 km", "between servers", "at least 50 hosts".
3. For port items, recall the secure and insecure pair, then check the transport (TCP or UDP) if the options differ on it.
4. For media items, match distance to fiber type, then fiber type to transceiver and connector.
5. For subnet items, write the block size before looking at the options, then compute network and broadcast addresses on scratch paper.
6. For cloud items, ask who starts the connection: inbound from the internet needs an internet gateway, outbound-only from private subnets needs a NAT gateway.
7. Eliminate options that are true statements but answer a different question.

## Traps that cost points

- **SMTP 25 vs SMTPS 587.** The objectives list 587 as the secure port for mail submission. 25 is server-to-server SMTP. Read whether the question wants encrypted or not.
- **SFTP vs FTPS.** SFTP runs over SSH on port 22. It is not FTP with TLS.
- **Paired ports reversed.** DHCP: the server listens on 67 and the client on 68. SNMP: queries go to agents on 161 and traps go to the manager on 162. All four are UDP.
- **Network and broadcast counted as hosts.** A /27 has 32 addresses but 30 usable hosts. Always subtract 2.
- **Wrong octet for block size.** In 10.4.100.9/20 the block size of 16 applies to the third octet, so the broadcast ends in .255, not .15.
- **APIPA mistaken for a static address problem.** 169.254.x.x means DHCP failed. Fix the path to the DHCP server, do not just type in an address.
- **Multimode for long runs.** Multimode reaches hundreds of meters, not kilometers. Between buildings far apart, choose single-mode.
- **NAS vs SAN.** NAS shares files (SMB, NFS); SAN presents block storage (Fibre Channel, iSCSI).
- **Scalability vs elasticity.** Elasticity is automatic growth and shrinkage. Adding servers by hand for next year is scalability.

## Timing

Recall items in this domain should take well under a minute, which buys time for the rest of the exam. Subnet questions take longer: aim for about 90 seconds and do the arithmetic on scratch paper rather than in your head. If a subnetting PBQ looks long, flag it and return after your first pass, but do return, since one table covers several facts at once.

## Practice items

These 4 items are original, written for these notes. They are not taken from CompTIA's exam or any prep provider.

### Item 1

Database servers in a cloud VPC sit in a private subnet. They must download security patches from the internet, but nothing on the internet should be able to start a connection to them. What should the administrator add?

A. A NAT gateway, with the private subnet's default route pointing to it

B. An internet gateway attached directly to the private subnet

C. A public IP address on each database server

D. A Direct Connect link to the patch vendor

<details>
<summary>Answer and explanation</summary>

**Answer: A.** A NAT gateway lets instances in a private subnet start outbound connections while unsolicited inbound connections are not allowed. (Objective 1.3)

- **B** is wrong because routing the subnet through an internet gateway with public addresses makes it a public subnet, reachable from outside.
- **C** is wrong because public addresses would expose each server to inbound connections.
- **D** is wrong because Direct Connect is a private link between your site and the cloud provider, not a path to arbitrary internet hosts.

</details>

### Item 2

A firewall team is writing rules for a monitoring network. Which two protocols use UDP as their standard transport? Choose two.

A. TFTP

B. SSH

C. HTTPS

D. SNMP

E. SMB

F. LDAPS

<details>
<summary>Answer and explanation</summary>

**Answer: A and D.** TFTP uses UDP 69, and SNMP uses UDP 161 for queries and 162 for traps. (Objective 1.4)

- **B** is wrong because SSH uses TCP 22.
- **C** is wrong because HTTPS uses TCP 443 in the classic setup the objectives describe.
- **E** is wrong because SMB uses TCP 445.
- **F** is wrong because LDAPS uses TCP 636.

</details>

### Item 3

Two campus buildings are 8 km apart. The company needs a 10 Gbps fiber link between their core switches, which have SFP+ ports. Which choice fits?

A. Multimode fiber with 10GBASE-SR transceivers

B. Cat 6a cable with 10GBASE-T

C. Single-mode fiber with 10GBASE-LR transceivers and LC connectors

D. Twinax DAC cable

<details>
<summary>Answer and explanation</summary>

**Answer: C.** 10GBASE-LR runs on single-mode fiber to 10 km, and SFP+ modules normally use LC connectors. (Objective 1.5)

- **A** is wrong because 10GBASE-SR on multimode reaches only a few hundred meters.
- **B** is wrong because twisted pair is limited to 100 m.
- **D** is wrong because DAC is for runs of a few meters inside a rack or row.

</details>

### Item 4

A server has the address 10.10.8.200/21. Which of these hosts is in the same subnet, so the two can communicate without a router?

A. 10.10.7.250/21

B. 10.10.16.1/21

C. 10.10.15.10/21

D. 10.10.0.200/21

<details>
<summary>Answer and explanation</summary>

**Answer: C.** /21 gives a mask of 255.255.248.0 and a block size of 8 in the third octet. 10.10.8.200 is in 10.10.8.0/21, which runs from 10.10.8.0 to 10.10.15.255. 10.10.15.10 falls inside that range. (Objective 1.7)

- **A** is wrong because 10.10.7.250 is in 10.10.0.0/21 (10.10.0.0 to 10.10.7.255).
- **B** is wrong because 10.10.16.1 is in the next subnet, 10.10.16.0/21.
- **D** is wrong because 10.10.0.200 is also in 10.10.0.0/21.

</details>

Want more? The free timed Network+ (N10-009) practice test on PrepClubs has 25 questions, with a domain-by-domain breakdown: [take the free test](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=fzshat&utm_content=network-plus).

## Score yourself

| Item | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| Answer | A | A, D | C | C |
| Objective | 1.3 | 1.4 | 1.5 | 1.7 |

- **4:** strong. Keep the port table and subnet method fresh with a few minutes of drills each day, since this domain carries close to a quarter of the exam.
- **3:** list the objectives you missed. Ports and subnetting are the usual weak spots; reread [1.4 Ports protocols and traffic types](#14-ports-protocols-and-traffic-types) and [1.7 IPv4 addressing](#17-ipv4-addressing).
- **2 or fewer:** spend two study sessions here. Write the port table and the CIDR table from memory until both are perfect, work ten subnets by hand with the five-step method, then reread [1.5 Media and transceivers](#15-media-and-transceivers) and [1.3 Cloud concepts](#13-cloud-concepts) before redoing the items.

## Next step

1. Redo any item you missed without opening the explanation, and write the trap you fell for in one line.
2. Run `ipconfig /all` on Windows (or `ip addr` on Linux), take your own address and prefix, and work out the network address, broadcast address and usable range by hand. Then check yourself with an online subnet calculator.
3. Take a timed run with the [free Network+ (N10-009) practice test on PrepClubs](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=fzshat&utm_content=network-plus) to practice pacing across the whole exam.

---

<!-- nav:bottom -->
← Previous: [5-minute diagnostic](../diagnostic.md) | Next: [Domain 2: Network Implementation](2-network-implementation.md) → | [Back to contents](../../README.md#contents)
<!-- /nav:bottom -->
