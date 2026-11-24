<!-- nav:top -->[Contents](../README.md#contents) · Previous: [FAQ](faq.md) · Next: [Sources](sources.md)<!-- /nav:top -->

# Glossary

Short definitions of terms that come up on N10-009. Most acronyms here appear in CompTIA's official objectives and acronym list, which CompTIA encourages candidates to know. Many link back to the domain where they matter most.

| Term | Meaning |
|---|---|
| 802.1Q | The IEEE standard for VLAN tagging: a 4-byte tag in the Ethernet frame carries the VLAN ID across a trunk ([Domain 2](domains/2-network-implementation.md)) |
| ACL | Access control list: ordered permit and deny rules, processed top down, applied to traffic or resources |
| Anycast | One address shared by several hosts; traffic goes to the nearest one by routing distance |
| APIPA | Automatic Private IP Addressing: a Windows host gives itself 169.254.x.x when no DHCP server answers ([Domain 5](domains/5-network-troubleshooting.md)) |
| ARP | Address Resolution Protocol: finds the MAC address that belongs to an IPv4 address on the local network |
| BGP | Border Gateway Protocol: the path-vector routing protocol used between autonomous systems on the internet |
| BPDU | Bridge Protocol Data Unit: the frames switches exchange to run spanning tree |
| Broadcast domain | The set of devices that receive a layer 2 broadcast; routers and VLANs separate broadcast domains |
| BSSID | Basic Service Set Identifier: the MAC address of one access point radio |
| CDN | Content delivery network: servers spread across regions that cache content close to users |
| CIDR | Classless Inter-domain Routing: writing a network as address plus prefix length, such as 10.0.0.0/22 ([Domain 1](domains/1-networking-concepts.md)) |
| CNAME | Canonical name record: a DNS alias that points one name at another name |
| CRC | Cyclic redundancy check: a frame checksum; rising CRC errors usually point to cabling or interference |
| DHCP | Dynamic Host Configuration Protocol: leases IP address, mask, gateway and DNS settings to clients (UDP 67 and 68) ([Domain 3](domains/3-network-operations.md)) |
| DNS | Domain Name System: resolves names to IP addresses and back (port 53) |
| DNSSEC | DNS Security Extensions: digital signatures that let resolvers verify DNS answers were not altered |
| DoH and DoT | DNS over HTTPS and DNS over TLS: encrypt DNS queries in transit |
| Duplex mismatch | One side of a link at full duplex and the other at half, causing collisions and slow transfers |
| EAP | Extensible Authentication Protocol: the framework used by 802.1X to carry authentication methods |
| EIGRP | Enhanced Interior Gateway Routing Protocol: an advanced distance-vector interior routing protocol |
| FHRP | First hop redundancy protocol: lets two or more routers share a virtual gateway address, such as HSRP or VRRP |
| FQDN | Fully qualified domain name: a complete host name including all domains, such as www.example.com |
| GRE | Generic Routing Encapsulation: a tunneling protocol that wraps packets inside IP; it does not encrypt |
| IaaS | Infrastructure as a service: the provider runs the hardware; you manage the operating systems and up ([Domain 1](domains/1-networking-concepts.md)) |
| IaC | Infrastructure as code: building and changing infrastructure from version-controlled configuration files |
| ICMP | Internet Control Message Protocol: error and diagnostic messages, used by ping and traceroute |
| IDF and MDF | Intermediate and main distribution frames: the wiring closets that connect floors (IDF) back to the building's core (MDF) |
| IDS and IPS | Intrusion detection and prevention systems: an IDS alerts on suspicious traffic; an IPS sits inline and can block it |
| IPAM | IP address management: tools for planning, tracking and assigning address space |
| IPSec | Internet Protocol Security: authenticates (AH) and encrypts (ESP) IP traffic, with keys set up by IKE |
| Jitter | Variation in packet delay over time; it hurts voice and video ([Domain 5](domains/5-network-troubleshooting.md)) |
| LACP | Link Aggregation Control Protocol: bundles several physical links into one logical link |
| LDAP and LDAPS | Lightweight Directory Access Protocol (389) and its TLS-protected form (636) for directory queries |
| LLDP and CDP | Link Layer Discovery Protocol (open standard) and Cisco Discovery Protocol: devices advertise their identity to neighbors |
| MAC address | Media Access Control address: the 48-bit layer 2 address of a network interface |
| MDIX | Medium Dependent Interface Crossover: a port that automatically adjusts so a straight-through cable works between like devices |
| MIB | Management Information Base: the structured set of values an SNMP agent exposes |
| MPLS | Multiprotocol Label Switching: forwards packets by short labels instead of full route lookups, common in provider WANs |
| MTU | Maximum transmission unit: the largest packet a link carries without fragmenting (1500 bytes on standard Ethernet) |
| NAC | Network access control: checks a device's identity and posture before letting it join the network ([Domain 4](domains/4-network-security.md)) |
| NAT and PAT | Network and port address translation: rewrite private addresses to public ones; PAT lets many hosts share one public address by port |
| NFV | Network functions virtualization: running firewalls, routers and similar functions as software on standard servers |
| NTP | Network Time Protocol: synchronizes clocks over UDP 123; NTS adds authentication |
| OSI model | Open Systems Interconnection model: seven layers from physical (1) to application (7) used to describe and troubleshoot networks ([Domain 1](domains/1-networking-concepts.md)) |
| OSPF | Open Shortest Path First: a link-state interior routing protocol that uses cost as its metric |
| Out-of-band management | Managing devices over a separate path, such as a console server, so you can reach them when the main network is down |
| PoE | Power over Ethernet: delivers power to devices like phones, cameras and access points over the data cable |
| PTR | Pointer record: a DNS record that maps an IP address back to a name (reverse lookup) |
| QoS | Quality of service: marking and queuing traffic so delay-sensitive flows such as voice go first |
| RADIUS | Remote Authentication Dial-in User Service: centralized AAA, often behind 802.1X and VPN logins |
| RPO and RTO | Recovery point objective (how much data loss is acceptable) and recovery time objective (how fast service must return) ([Domain 3](domains/3-network-operations.md)) |
| RSSI | Received Signal Strength Indicator: how strong a wireless signal is at the receiver |
| SAN and NAS | Storage area network (block storage over a dedicated network) and network-attached storage (file shares over the LAN) |
| SASE and SSE | Secure access service edge (networking and security delivered from the cloud) and security service edge (its security part) |
| Screened subnet | A network segment between the internet and the internal network for public-facing servers, also known as a DMZ |
| SD-WAN | Software-defined WAN: central policy picks the best path across several WAN links |
| SDN | Software-defined networking: separates the control plane from the data plane so the network is managed centrally |
| SFP and QSFP | Small form-factor pluggable and quad SFP: hot-swappable transceiver modules; QSFP carries four lanes for higher speeds ([Domain 1](domains/1-networking-concepts.md)) |
| SIEM | Security information and event management: collects and correlates logs and raises alerts |
| SLAAC | Stateless Address Autoconfiguration: an IPv6 host builds its own address from the router's advertised prefix |
| SNMP | Simple Network Management Protocol: polls and receives traps from devices (UDP 161 and 162); version 3 adds authentication and encryption |
| SPAN | Switched Port Analyzer: a port mirror that copies traffic from one port or VLAN to another for capture |
| SSID | Service Set Identifier: the name of a wireless network |
| STP | Spanning Tree Protocol: blocks redundant switch paths to prevent loops; also shielded twisted pair in cabling |
| Subnet mask | The 32-bit value that marks which part of an IPv4 address is the network, such as 255.255.255.192 for /26 |
| SVI | Switch virtual interface: a layer 3 interface on a switch for a VLAN, used for inter-VLAN routing |
| Syslog | A standard for sending log messages to a central server (UDP 514) |
| TACACS+ | Terminal Access Controller Access Control System Plus: AAA protocol often used for device administration; encrypts the whole payload |
| TTL | Time to live: a hop count in IP packets, or how long a DNS record may be cached |
| VLAN | Virtual LAN: a logical layer 2 network that separates traffic on shared switches ([Domain 2](domains/2-network-implementation.md)) |
| VLSM | Variable Length Subnet Mask: using different prefix lengths inside one address block to fit subnets to their size |
| VPN | Virtual private network: an encrypted tunnel; site-to-site joins networks, client-to-site joins a user |
| VRRP and HSRP | Virtual Router Redundancy Protocol (open) and Hot Standby Router Protocol (Cisco): FHRPs that share a virtual gateway |
| VXLAN | Virtual Extensible LAN: carries layer 2 frames over a layer 3 network, used in data centers |
| WPA3 | Wi-Fi Protected Access 3: the current wireless security standard; personal mode uses SAE instead of WPA2's pre-shared key exchange |
| Zero trust | Security model with no implicit trust based on network location; every request is verified ([Domain 4](domains/4-network-security.md)) |

---

<!-- nav:bottom -->← Previous: [FAQ](faq.md) | Next: [Sources](sources.md) → | [Back to contents](../README.md#contents)<!-- /nav:bottom -->
