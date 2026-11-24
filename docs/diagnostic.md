<!-- nav:top -->[Contents](../README.md#contents) · Previous: [The Network+ N10-009 exam explained](exam-overview.md) · Next: [Domain 1: Networking Concepts](domains/1-networking-concepts.md)<!-- /nav:top -->

# 15-minute diagnostic

Fifteen original items, three from each N10-009 domain, to show you where to start. Set a timer for 15 minutes, answer everything without looking anything up (a pen and paper for subnetting is fine, as on the real exam), then check each answer and use the table at the bottom.

These items are original, written for these notes. They are not taken from CompTIA's exam or any prep provider. They are different from the practice items on the domain pages, so you can still use those afterwards.

If you would rather start with a full timed run in the real format, take the [free Network+ (N10-009) practice test on PrepClubs](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus) instead, then come back here.

## Part A: Domain 1, Networking Concepts

### D1

A technician runs traceroute to a remote server. The tool discovers each router along the path by sending packets that expire one hop further each time. Which IP header field makes this work?

A. DSCP

B. Source port

C. TTL

D. Checksum

<details>
<summary>Answer and explanation</summary>

**Answer: C.** Each router lowers the TTL by 1 and, when it reaches 0, drops the packet and sends back an ICMP time exceeded message. Traceroute sends probes with TTL 1, 2, 3 and so on to reveal each hop. (Objective 1.2)

- **A** is wrong because DSCP marks traffic for QoS priority.
- **B** is wrong because ports belong to the TCP or UDP header, not the IP header, and do not expire.
- **D** is wrong because the checksum detects header errors; it does not count hops.

</details>

### D2

A user cannot reach any network resources. The output of `ipconfig` shows:

```
Ethernet adapter Ethernet:
   IPv4 Address. . . . . . . . . . . : 169.254.38.112
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . :
```

What is the MOST likely cause?

A. The PC could not reach a DHCP server

B. The PC has a duplicate static address

C. The DNS server is down

D. The default gateway router has failed

<details>
<summary>Answer and explanation</summary>

**Answer: A.** 169.254.0.0/16 is the APIPA range. Windows assigns it to itself when no DHCP server answers, and leaves the gateway blank. (Objective 1.7)

- **B** is wrong because APIPA is self-assigned after DHCP fails; it is not a static address.
- **C** is wrong because a DNS failure would leave a valid address and gateway in place.
- **D** is wrong because a failed router would not stop DHCP from assigning a normal address and gateway on the local subnet.

</details>

### D3

A company has moved its internal clients to IPv6 only. Some vendor websites they need are still reachable only over IPv4. Which solution lets the IPv6-only clients reach the IPv4-only sites?

A. Dual stack on the vendor websites

B. VXLAN between the two networks

C. APIPA on the clients

D. NAT64, with DNS64 providing IPv6 answers for IPv4-only names

<details>
<summary>Answer and explanation</summary>

**Answer: D.** NAT64 translates between IPv6 and IPv4, and DNS64 synthesizes IPv6 addresses for names that have only IPv4 records, so IPv6-only clients can reach IPv4-only servers. (Objective 1.8)

- **A** is wrong because the company does not control the vendors' servers, and its clients have no IPv4 to use anyway.
- **B** is wrong because VXLAN stretches layer 2 segments; it does not translate between IP versions.
- **C** is wrong because APIPA is an IPv4 fallback for failed DHCP.

</details>

## Part B: Domain 2, Network Implementation

### D4

A small office has 25 PCs with private addresses and a single public IP address from its ISP. All PCs browse the internet at the same time. Which feature on the edge router makes this possible?

A. Static NAT

B. PAT

C. FHRP

D. Subinterfaces

<details>
<summary>Answer and explanation</summary>

**Answer: B.** Port address translation lets many inside hosts share one public address by giving each connection a unique source port. (Objective 2.1)

- **A** is wrong because static NAT maps one inside address to one public address, so 25 PCs would need 25 public addresses.
- **C** is wrong because an FHRP provides gateway redundancy, not address translation.
- **D** is wrong because subinterfaces split a router port by VLAN; they do not translate addresses.

</details>

### D5

A technician connects a second cable between two switches for redundancy. Within seconds, the network slows to a crawl, switch CPU usage hits 100% and the MAC address tables keep changing. What protocol should have prevented this?

A. LACP

B. 802.1Q

C. VRRP

D. Spanning tree

<details>
<summary>Answer and explanation</summary>

**Answer: D.** The extra link created a layer 2 loop and a broadcast storm. Spanning tree detects redundant paths and blocks one, which prevents loops. (Objective 2.2)

- **A** is wrong because LACP bundles links that are deliberately configured as an aggregate; the technician did not configure one.
- **B** is wrong because 802.1Q tags VLANs on trunks; it does not stop loops.
- **C** is wrong because VRRP gives gateway redundancy at layer 3.

</details>

### D6

An AP running on a 5 GHz channel in the 100s suddenly switches channels, and clients disconnect for a moment. The AP log shows "radar detected". Which standard requires this behavior?

A. 802.11h

B. 802.1X

C. 802.3af

D. 802.11b

<details>
<summary>Answer and explanation</summary>

**Answer: A.** 802.11h adds dynamic frequency selection (DFS) and transmit power control. On DFS channels an AP must leave the channel when it detects radar. (Objective 2.3)

- **B** is wrong because 802.1X is port-based network access control, used for Enterprise wireless authentication.
- **C** is wrong because 802.3af is Power over Ethernet.
- **D** is wrong because 802.11b is an older 2.4 GHz standard.

</details>

## Part C: Domain 3, Network Operations

### D7

Which metric describes the average time a device runs before it fails?

A. MTTR

B. MTBF

C. RPO

D. RTO

<details>
<summary>Answer and explanation</summary>

**Answer: B.** Mean time between failures is the average running time between one failure and the next. Higher is better. (Objective 3.3)

- **A** is wrong because MTTR is the average time taken to repair a failure.
- **C** is wrong because RPO is the maximum acceptable data loss, measured in time.
- **D** is wrong because RTO is the maximum acceptable downtime.

</details>

### D8

A network manager wants a weekly report of which hosts sent the most traffic to the internet and which applications they used. Storage is limited, and the report does not need to show packet contents. Which monitoring method fits BEST?

A. Full packet capture on the internet link

B. SNMP traps from the edge router

C. Syslog from every workstation

D. Flow data such as NetFlow or IPFIX from the edge router

<details>
<summary>Answer and explanation</summary>

**Answer: D.** Flow data records source, destination, ports, protocol and byte counts for each conversation, which is exactly what a top-talkers report needs, without storing payloads. (Objective 3.2)

- **A** is wrong because full captures store every payload, which uses far more storage than needed.
- **B** is wrong because traps report events such as a link going down, not traffic volumes per host.
- **C** is wrong because workstation logs record local events, not network-wide traffic statistics.

</details>

### D9

After a web server moved to a new address, one user's browser still opens the old site. The technician sees this:

```
C:\>nslookup intranet.corp.example.com
Server:  dns1.corp.example.com
Address:  10.0.0.53

Name:    intranet.corp.example.com
Address:  10.10.5.20

C:\>ping intranet.corp.example.com

Pinging intranet.corp.example.com [10.10.1.99] with 32 bytes of data:
```

DNS holds the new address 10.10.5.20, and other users reach the new site. What is the MOST likely cause?

A. The DNS server has a stale A record

B. The user's PC is missing a PTR record

C. A static entry in the PC's hosts file points to the old address

D. The DNS zone is not authoritative

<details>
<summary>Answer and explanation</summary>

**Answer: C.** nslookup asks the DNS server directly and gets the new address, but ping uses the operating system's resolver, which checks the hosts file first. An old hosts file entry explains the difference. (Objective 3.4)

- **A** is wrong because nslookup shows the DNS server already returns the new address.
- **B** is wrong because PTR records serve reverse lookups and do not change which address a name resolves to.
- **D** is wrong because other users resolve correctly, and the authoritative status of the answer would not make ping use a different address.

</details>

## Part D: Domain 4, Network Security

### D10

A company places a decoy server on its network that looks like a file server full of sensitive data. No real user ever needs it, so any connection to it triggers an alert. What is this?

A. A honeypot

B. A screened subnet

C. A jump box

D. A SIEM

<details>
<summary>Answer and explanation</summary>

**Answer: A.** A honeypot is a deception system with no legitimate use, so any activity on it is suspicious and worth investigating. (Objective 4.1)

- **B** is wrong because a screened subnet is a zone for real public-facing servers.
- **C** is wrong because a jump box is a real, hardened server admins use to reach management interfaces.
- **D** is wrong because a SIEM collects and correlates logs; it is not a decoy.

</details>

### D11

Several users on the third floor suddenly cannot reach the internet. Their PCs show addresses in 192.168.0.0/24 with a gateway of 192.168.0.1, while the company uses 10.30.0.0/16 for that floor. Users on other floors are fine. What is the MOST likely cause?

A. MAC flooding on the floor switch

B. DNS poisoning of the internal DNS server

C. A VLAN hopping attack

D. A rogue DHCP server, such as a consumer router plugged into the floor network

<details>
<summary>Answer and explanation</summary>

**Answer: D.** The clients received a different subnet and gateway than the company's DHCP server hands out. A home router's 192.168.0.x defaults are a classic sign of a rogue DHCP server answering first. (Objective 4.2)

- **A** is wrong because MAC flooding makes the switch flood frames; it does not give clients different IP settings.
- **B** is wrong because DNS poisoning gives wrong name answers; it does not change the client's address and gateway.
- **C** is wrong because VLAN hopping lets an attacker reach another VLAN; it does not hand out addresses.

</details>

### D12

A router interface facing the user subnet 10.5.5.0/24 has this inbound ACL:

```
10 permit tcp 10.5.5.0 0.0.0.255 any eq 443
20 permit udp 10.5.5.0 0.0.0.255 host 10.1.1.53 eq 53
30 deny   tcp 10.5.5.0 0.0.0.255 any eq 22
```

A user at 10.5.5.40 reports that a website using plain HTTP on TCP 80 will not load, though HTTPS sites work. Why?

A. Line 30 blocks TCP 80

B. No line permits TCP 80, so the implicit deny at the end drops it

C. Line 20 blocks web traffic because DNS is limited to one server

D. ACLs are processed bottom to top, so line 30 is applied first

<details>
<summary>Answer and explanation</summary>

**Answer: B.** Traffic that matches no permit line hits the implicit deny at the end of the ACL. This ACL permits only HTTPS and DNS to one server, so HTTP is dropped. (Objective 4.3)

- **A** is wrong because line 30 matches only TCP 22 (SSH).
- **C** is wrong because line 20 permits DNS; it does not match or block TCP 80.
- **D** is wrong because ACLs are processed top to bottom, first match wins.

</details>

## Part E: Domain 5, Network Troubleshooting

### D13

A technician must find which patch panel port connects to an unlabeled wall jack in a conference room. Which tool should she use?

A. Visual fault locator

B. Wi-Fi analyzer

C. Toner and probe

D. Protocol analyzer

<details>
<summary>Answer and explanation</summary>

**Answer: C.** The tone generator connects at the wall jack and the probe finds the matching cable at the patch panel by its tone. (Objective 5.5)

- **A** is wrong because a visual fault locator is for fiber, and wall jacks are normally copper.
- **B** is wrong because a Wi-Fi analyzer examines wireless signals, not cables.
- **D** is wrong because a protocol analyzer captures traffic and cannot trace a physical cable.

</details>

### D14

Two workstations in accounting lose network access at random, a few minutes at a time. Windows on one of them shows an IP address conflict warning. Running `arp -a` from a third PC shows the MAC address for 10.8.2.44 changing between two values over several minutes. What is the MOST likely cause?

A. Duplicate IP address

B. Incorrect default gateway

C. Address pool exhaustion

D. An STP loop

<details>
<summary>Answer and explanation</summary>

**Answer: A.** Two devices using 10.8.2.44 take turns owning the address, so the ARP entry flips between their MACs and each loses connectivity at times. The conflict warning confirms it. (Objective 5.3)

- **B** is wrong because a wrong gateway causes a steady failure to reach other subnets, not intermittent loss and an address conflict.
- **C** is wrong because exhaustion stops new clients getting addresses; these hosts already have one.
- **D** is wrong because a loop affects the whole switched network with a broadcast storm, not two hosts.

</details>

### D15

A user plugs a small unmanaged switch into his desk jack so he can connect a second PC. Both PCs immediately lose connectivity. The access switch shows:

```
Switch#show interfaces status

Port      Name        Status       Vlan  Duplex  Speed  Type
Gi1/0/14  Desk-3-14   err-disabled 20    auto    auto   10/100/1000BaseTX
Gi1/0/15  Desk-3-15   connected    20    a-full  a-1000 10/100/1000BaseTX

Switch#show port-security interface Gi1/0/14
Port Security              : Enabled
Port Status                : Secure-shutdown
Violation Mode             : Shutdown
Maximum MAC Addresses      : 1
Total MAC Addresses        : 1
Security Violation Count   : 1
```

What should the technician do to restore service while following policy?

A. Configure `no shutdown` on the port, then leave the small switch in place

B. Replace the cable, since err-disabled means a physical fault

C. Move the port to a different VLAN

D. Remove the unmanaged switch, then shut and re-enable the port

<details>
<summary>Answer and explanation</summary>

**Answer: D.** Port security allows one MAC address, and the second PC caused a violation that shut the port. Remove the cause first, then bounce the port, or it will err-disable again. (Objective 5.2)

- **A** is wrong because with the small switch still connected, the second MAC address triggers the violation again.
- **B** is wrong because the port security output shows the shutdown was a security violation, not a cable fault.
- **C** is wrong because changing the VLAN does not clear the err-disabled state or the reason for it.

</details>

## Answer key

| Part | Items | Answers |
|---|---|---|
| Part A | D1 to D3 | C, A, D |
| Part B | D4 to D6 | B, D, A |
| Part C | D7 to D9 | B, D, C |
| Part D | D10 to D12 | A, D, B |
| Part E | D13 to D15 | C, A, D |

## What your result means

Count your correct answers in each part. A choose-two item counts only if both choices are right.

| Part | Domain weight | Score 0 or 1 | Score 2 | Score 3 |
|---|---|---|---|---|
| A. [Networking Concepts](domains/1-networking-concepts.md) | 23% | Top priority: ports, addressing and subnetting feed every other domain | Do its 12 items and redo the subnetting examples | Still do the subnetting items for speed |
| B. [Network Implementation](domains/2-network-implementation.md) | 20% | Study routing, switching and wireless tables | Do its practice items | Light review only |
| C. [Network Operations](domains/3-network-operations.md) | 19% | Study the full guide | Do its practice items | Light review only |
| D. [Network Security](domains/4-network-security.md) | 14% | Study the attack and defense tables | Do its practice items | Light review only |
| E. [Network Troubleshooting](domains/5-network-troubleshooting.md) | 24% | Top priority: the heaviest domain | Learn the methodology order, then do all 12 items | Still do the items: it is nearly a quarter of the exam |

If two domains tie, start with the one with the higher weight: a gap in a 24% domain costs more than the same gap in a 14% domain.

Then pick a plan: the [7-day plan](study-plans.md#7-day-plan) if you have a week, or the [2-day plan](study-plans.md#2-day-plan) if your exam is close.

---

<!-- nav:bottom -->← Previous: [The Network+ N10-009 exam explained](exam-overview.md) | Next: [Domain 1: Networking Concepts](domains/1-networking-concepts.md) → | [Back to contents](../README.md#contents)<!-- /nav:bottom -->
