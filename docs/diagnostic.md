<!-- nav:top -->[Contents](../README.md#contents) · Previous: [The Network+ N10-009 exam explained](exam-overview.md) · Next: [Domain 1: Networking Concepts](domains/1-networking-concepts.md)<!-- /nav:top -->

# 5-minute diagnostic

Five original items, one from each N10-009 domain, to show you where to start. Set a timer for 5 minutes, answer everything without looking anything up (a pen and paper for subnetting is fine, as on the real exam), then check each answer and use the table at the bottom.

These items are original, written for these notes. They are not taken from CompTIA's exam or any prep provider. They are different from the practice items on the domain pages, so you can still use those afterwards.

If you would rather start with a full timed run in the real format, take the [free Network+ (N10-009) practice test on PrepClubs](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus) instead, then come back here.

## Part A: Domain 1, Networking Concepts

### D1

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

## Part B: Domain 2, Network Implementation

### D2

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

## Part C: Domain 3, Network Operations

### D3

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

### D4

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

### D5

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
| Part A | D1 | A |
| Part B | D2 | D |
| Part C | D3 | C |
| Part D | D4 | B |
| Part E | D5 | D |

## What your result means

Check whether you answered each part's item correctly. A choose-two item counts only if both choices are right.

| Part | Domain weight | Missed | Correct |
|---|---|---|---|
| A. [Networking Concepts](domains/1-networking-concepts.md) | 23% | Top priority: ports, addressing and subnetting feed every other domain | Still do the subnetting item for speed |
| B. [Network Implementation](domains/2-network-implementation.md) | 20% | Study routing, switching and wireless tables | Light review only |
| C. [Network Operations](domains/3-network-operations.md) | 19% | Study the full guide | Light review only |
| D. [Network Security](domains/4-network-security.md) | 14% | Study the attack and defense tables | Light review only |
| E. [Network Troubleshooting](domains/5-network-troubleshooting.md) | 24% | Top priority: the heaviest domain | Still do the items: it is nearly a quarter of the exam |

If two domains tie, start with the one with the higher weight: a gap in a 24% domain costs more than the same gap in a 14% domain.

Then pick a plan: the [7-day plan](study-plans.md#7-day-plan) if you have a week, or the [2-day plan](study-plans.md#2-day-plan) if your exam is close.

---

<!-- nav:bottom -->← Previous: [The Network+ N10-009 exam explained](exam-overview.md) | Next: [Domain 1: Networking Concepts](domains/1-networking-concepts.md) → | [Back to contents](../README.md#contents)<!-- /nav:bottom -->
