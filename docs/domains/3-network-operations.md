<!-- nav:top -->[Contents](../../README.md#contents) · Previous: [Domain 2: Network Implementation](2-network-implementation.md) · Next: [Domain 4: Network Security](4-network-security.md)<!-- /nav:top -->

# Domain 3: Network Operations

**Weight:** 19% of the exam (CompTIA's figure). By our estimate that is roughly 17 of up to 90 questions. **Objectives:** 3.1 to 3.5.

This domain is about running a network well after it is built: documenting it, watching it, recovering it, handing out addresses and names, and reaching devices to manage them. The topics sound like paperwork, so candidates skim them and then lose marks on precise details, such as which SNMP version encrypts, which DNS record does reverse lookups, or what RPO means compared with RTO.

## What this domain measures

The five objectives mix "explain" items (processes, disaster recovery, access methods) with "given a scenario" items (monitoring and network services). Scenario items expect you to pick the right tool, record or setting for a described problem.

| Objective | What it asks you to do | Topics you must know |
|---|---|---|
| 3.1 | Explain organizational processes and procedures | Physical and logical diagrams, rack diagrams, cable maps, Layer 1, 2 and 3 diagrams, asset inventory, IPAM, SLAs, wireless surveys and heat maps, EOL and EOS, patching, decommissioning, change management, configuration management |
| 3.2 | Use network monitoring technologies | SNMP traps, MIBs, v2c and v3, community strings, flow data, packet capture, baselines and anomaly alerts, syslog, SIEM, API integration, port mirroring, discovery, traffic, performance, availability and configuration monitoring |
| 3.3 | Explain disaster recovery concepts | RPO, RTO, MTTR, MTBF, cold, warm and hot sites, active-active and active-passive, tabletop exercises, validation tests |
| 3.4 | Implement IPv4 and IPv6 network services | DHCP scopes, leases, reservations, options, relay, exclusions, SLAAC, DNS records, DNSSEC, DoH and DoT, zone types, recursion, hosts file, NTP, PTP, NTS |
| 3.5 | Compare network access and management methods | Site-to-site and client-to-site VPN, clientless VPN, split and full tunnel, SSH, GUI, API, console, jump box, in-band and out-of-band management |

## Key concepts explained

### 3.1 Organizational processes and procedures

**Diagrams.** A **physical diagram** shows where things really are: rooms, racks, cable runs, ports. A **logical diagram** shows how traffic flows: subnets, VLANs, routing, without caring where boxes sit.

| Document | Shows | Use it when |
|---|---|---|
| Rack diagram | Each device's position in a rack by rack unit (U), plus power and patch panels | You need to find or install hardware in a data center |
| Cable map or diagram | Which cable runs from which wall jack or patch panel port to which switch port | You trace a run or label a new drop |
| Layer 1 diagram | Physical connections: cables, media types, interfaces | You troubleshoot links or plan cabling |
| Layer 2 diagram | Switches, VLANs, trunks, spanning tree layout | You trace a VLAN or a loop |
| Layer 3 diagram | Subnets, router interfaces, IP addressing, routing | You plan addressing or follow a routing path |

**Asset inventory** records **hardware** (model, serial, location), **software** (versions installed), **licensing** (how many seats you own and use) and **warranty support** (when vendor repair coverage ends). An accurate inventory lets you spot unknown devices and plan replacements.

**IP address management (IPAM)** tracks which subnets and addresses are assigned, reserved or free, usually tied to DHCP and DNS. It prevents duplicate addresses and shows when a scope is filling up.

**Service-level agreement (SLA).** A contract stating the level of service a provider commits to, such as 99.9% uptime or a four-hour response time, and what happens if it is missed.

**Wireless survey and heat map.** A site survey measures signal and interference on site. The heat map is the colored floor plan it produces, showing strong and weak coverage.

**Life-cycle management.**

| Term | Meaning | What you do |
|---|---|---|
| End-of-life (EOL) | The vendor stops making or selling the product | Plan a replacement |
| End-of-support (EOS) | The vendor stops patches, fixes and technical support | Replace it, or isolate it and accept the risk formally |
| Software management | Applying patches and bug fixes, OS updates and firmware updates | Test first, schedule in a maintenance window, keep a rollback plan |
| Decommissioning | Retiring a device: remove it from inventory, monitoring, DNS and IPAM, wipe its configuration and data | Follow a checklist so nothing is left pointing at it |

**Change management.** Changes go through a formal process: a **request** (often a service request or ticket) that is tracked, reviewed for risk and impact, approved, scheduled, implemented with a rollback plan, and documented. Even a small VLAN change on a core switch needs a request first.

**Configuration management.**

- **Production configuration:** what is running on the device right now.
- **Backup configuration:** a saved copy you can restore if a change fails or a device dies.
- **Baseline or golden configuration:** the approved standard for that device type. You compare devices against it to find drift.

### 3.2 Network monitoring technologies

**SNMP (Simple Network Management Protocol).** A manager polls agents on devices over **UDP 161**. Agents send unsolicited **traps** to the manager on **UDP 162** when an event happens, such as an interface going down. The **MIB (management information base)** is the database of objects an agent can report, each identified by an OID.

| Version | Security | Notes |
|---|---|---|
| v2c | **Community strings** sent in plaintext | Read-only and read-write strings act like shared passwords. Change the defaults "public" and "private" |
| v3 | User-based **authentication** and optional **encryption** | Security levels: noAuthNoPriv, authNoPriv, authPriv (authentication and encryption) |

If a question says "encrypt" or "authenticate users" for monitoring, the answer is SNMPv3.

**Flow data** (NetFlow, IPFIX, sFlow) records conversations: source and destination addresses, ports, protocol, times and byte counts, but not the contents. It answers "who talked to whom, and how much".

**Packet capture** records full packets, payload included. It answers "what exactly was said". It is the most detailed and the most storage-hungry source.

**Baseline metrics.** Measure normal behavior (bandwidth, CPU, error rates at different times of day), then use **anomaly alerting and notification** to flag values that stray from it. Without a baseline, you cannot tell "busy" from "broken".

**Log aggregation.**

- A **syslog collector** gathers logs from many devices, traditionally on UDP 514. Syslog severity runs from 0 (Emergency) to 7 (Debug): 0 Emergency, 1 Alert, 2 Critical, 3 Error, 4 Warning, 5 Notice, 6 Informational, 7 Debug. Lower number means more severe.
- A **SIEM** collects logs, correlates events across sources and raises alerts.

**API integration** lets monitoring tools pull data from, or push actions to, devices and cloud services through their APIs rather than SNMP.

**Port mirroring** (SPAN on Cisco) copies traffic from one or more switch ports or VLANs to a monitoring port, where an analyzer or sensor listens.

**Monitoring solutions.**

| Solution | What it does |
|---|---|
| Network discovery | Finds devices on the network. **Ad hoc** runs on demand; **scheduled** runs regularly to catch new or missing devices |
| Traffic analysis | Shows top talkers, protocols and applications, usually from flow data |
| Performance monitoring | Tracks utilization, latency, errors and loss against the baseline |
| Availability monitoring | Checks whether devices and services are up, for example with ping or service probes |
| Configuration monitoring | Detects configuration changes and drift from the golden configuration |

### 3.3 Disaster recovery concepts

**DR metrics.**

| Metric | Question it answers | Example |
|---|---|---|
| Recovery point objective (RPO) | How much data can we afford to lose, measured in time? | RPO of 1 hour means backups or replication at least hourly |
| Recovery time objective (RTO) | How long can the service be down? | RTO of 4 hours means it must be running again within 4 hours |
| Mean time to repair (MTTR) | On average, how long does a fix take? | Total repair time divided by number of repairs |
| Mean time between failures (MTBF) | On average, how long does it run between failures? | Higher is better. Used for repairable devices |

A quick way to remember: RPO looks back at data, RTO looks forward at downtime.

**DR sites.**

| Site | What is there | Recovery speed | Cost |
|---|---|---|---|
| Cold | Space, power, cooling. No equipment or current data | Days to weeks | Lowest |
| Warm | Hardware and connectivity in place, data not fully current | Hours to days | Medium |
| Hot | Fully equipped, data replicated and current | Minutes to hours | Highest |

**High availability.** **Active-active** means two or more systems carry traffic at the same time and share the load; if one fails the others absorb its work. **Active-passive** means one system works while a standby waits and takes over on failure.

**Testing.** A **tabletop exercise** walks the team through a scenario in a meeting, with no systems touched. **Validation tests** actually exercise the plan, such as restoring a backup or failing over to the DR site, to prove it works.

### 3.4 IPv4 and IPv6 network services

**DHCP.** Clients get addresses through DORA: Discover, Offer, Request, Acknowledge. The server listens on UDP 67 and the client on UDP 68.

| Setting | Meaning |
|---|---|
| Scope | The range of addresses a server can hand out for one subnet |
| Exclusions | Addresses inside the scope that the server will never hand out, such as static server and printer addresses |
| Reservations | A specific address always given to a specific client, matched on its MAC address |
| Lease time | How long a client keeps the address before it must renew. Clients try to renew at half the lease |
| Options | Extra settings sent with the address: default gateway (option 3), DNS servers (option 6), domain name (option 15) |
| Relay or IP helper | A router forwards DHCP broadcasts to a DHCP server on another subnet. Broadcasts do not cross routers by themselves |

A Windows client that gets no DHCP answer gives itself an APIPA address in **169.254.0.0/16**. Seeing that address means "DHCP failed", not "DNS failed".

**SLAAC (stateless address autoconfiguration).** An IPv6 host builds its own address from the prefix in router advertisements, plus an interface ID it generates (EUI-64 from its MAC, or a random value). No DHCP server is needed. DHCPv6 can still supply settings such as DNS servers.

**DNS** uses UDP 53 for most queries and TCP 53 for zone transfers and large responses.

| Record | Purpose |
|---|---|
| A | Name to IPv4 address |
| AAAA | Name to IPv6 address |
| CNAME | Alias pointing one name to another name |
| MX | Mail server for a domain, with a preference number (lower is tried first) |
| TXT | Free text, commonly SPF, DKIM and domain verification |
| NS | Name servers that are authoritative for a zone |
| PTR | IP address to name, used for reverse lookups |

**Zones and servers.**

- **Forward zone:** names to addresses. **Reverse zone:** addresses to names, using in-addr.arpa for IPv4 and ip6.arpa for IPv6.
- **Authoritative** answers come from a server that holds the zone. **Non-authoritative** answers come from a resolver's cache.
- **Primary** servers hold the writable copy. **Secondary** servers hold read-only copies received by zone transfer.
- A **recursive** resolver does the whole lookup for the client, asking other servers until it has the answer.

**Securing DNS.**

| Technology | What it protects | How |
|---|---|---|
| DNSSEC | Integrity and origin of DNS data | Digital signatures on records, checked by resolvers. It does not encrypt |
| DNS over TLS (DoT) | Privacy of queries | Encrypts DNS in TLS on TCP 853 |
| DNS over HTTPS (DoH) | Privacy of queries | Encrypts DNS inside HTTPS on TCP 443, so it blends with web traffic |

**Hosts file.** A local text file mapping names to addresses: `C:\Windows\System32\drivers\etc\hosts` on Windows and `/etc/hosts` on Linux and macOS. The operating system checks it before asking DNS, so a stale entry overrides DNS. Note that `nslookup` and `dig` query DNS servers directly and skip the hosts file, so they can show a different answer from what a browser uses.

Reading `dig` output:

```
dig example.com MX +noall +answer

example.com.     3600   IN   MX   10 mail1.example.com.
example.com.     3600   IN   MX   20 mail2.example.com.
```

Mail goes to mail1 first (preference 10) and falls back to mail2. The 3600 is the TTL in seconds.

**Time protocols.** Accurate time matters for logs, certificates and authentication.

| Protocol | Use |
|---|---|
| NTP | General time sync over UDP 123, accurate to milliseconds. Stratum shows distance from the reference clock |
| PTP (Precision Time Protocol, IEEE 1588) | Sub-microsecond accuracy using hardware timestamps, for trading, telecom and industrial systems |
| NTS (Network Time Security) | Adds cryptographic authentication to NTP so clients can trust the time source |

### 3.5 Network access and management methods

**VPNs.**

| Type | Who connects | Typical tech |
|---|---|---|
| Site-to-site | Two networks, such as branch and head office, through their gateways | IPsec tunnel between routers or firewalls, always on |
| Client-to-site | One user's device to the company network | VPN client software, IPsec or SSL/TLS |
| Clientless | One user through a web browser, no client installed | SSL/TLS portal that publishes specific web apps |

**Split tunnel vs full tunnel.** In a **full tunnel**, all the user's traffic goes through the VPN, so the company can filter and inspect it all, at the cost of bandwidth. In a **split tunnel**, only traffic for company networks uses the VPN and everything else goes straight to the internet. Split tunnel saves bandwidth but the company cannot see or filter the direct traffic.

**Connection methods.**

| Method | What it is | Notes |
|---|---|---|
| SSH | Encrypted command-line access on TCP 22 | Replaces Telnet (TCP 23, plaintext) |
| GUI | Web or application interface, usually HTTPS | Easier for occasional tasks |
| API | Programmatic access, often REST over HTTPS | Used for automation and bulk changes |
| Console | Direct serial or USB connection to the device's console port | Works even with no network configuration |

**Jump box (jump host).** A hardened server that administrators log into first, and from which they reach management interfaces. Devices accept management connections only from it, which shrinks the attack surface and centralizes logging.

**In-band vs out-of-band management.** **In-band** management uses the same network that carries user traffic. If that network is down, you lose access. **Out-of-band** management uses a separate path, such as a console server with its own network or a cellular modem, so you can still reach devices during an outage.

## How questions look

Most items here are short scenarios. A manager wants to know which diagram shows where a switch sits in a rack. A monitoring team needs encrypted polling. A company states how much data it can lose and you name the metric. Clients on a new VLAN get 169.254 addresses and you pick the fix. Expect exhibits of `nslookup` or `dig` output, DHCP scope settings, or a short monitoring report. Performance-based items may ask you to match DNS record types to purposes, configure a DHCP scope, or match DR sites to requirements. Words like BEST and FIRST matter: change management items usually want the request before the change.

## A method for these questions

1. Name the objective: documentation, monitoring, DR, addressing and naming, or access.
2. Underline the requirement words: "encrypt", "without contents", "how much data", "how long", "from another subnet", "browser only".
3. For monitoring items, decide what data is needed: alerts (traps), counters (polling), conversations (flow), full content (capture), or log events (syslog, SIEM).
4. For DR items, translate the numbers: data loss is RPO, downtime is RTO, then pick the site whose recovery speed fits.
5. For DHCP and DNS items, follow the path: does the request reach the server, does the server have the right record or scope, and does the client get the right option?
6. For process items, pick the documented, approved route over the quick fix.

## Traps that cost points

- **RPO and RTO swapped.** RPO is data loss measured back from the failure. RTO is downtime measured forward. "We can lose at most 15 minutes of orders" is RPO.
- **SNMPv2c as secure.** Community strings travel in plaintext. Encryption and per-user authentication need SNMPv3.
- **Traps on the wrong port.** Polling uses UDP 161, traps go to UDP 162.
- **DNSSEC for privacy.** DNSSEC signs records to stop forgery. It does not hide queries. Privacy is DoT or DoH.
- **PTR mixed up with A.** Reverse lookups (address to name) use PTR records in a reverse zone.
- **Missing the relay.** DHCP broadcasts stop at the router. A new subnet with a remote DHCP server needs an IP helper on the router interface.
- **Exclusion vs reservation.** An exclusion keeps an address out of the pool. A reservation ties an address to a MAC.
- **EOL vs EOS.** EOL ends sales. EOS ends patches and support, which is the security problem.
- **In-band management in an outage.** If the network is down, only out-of-band access still works.

## Timing

The exam is one 90-minute block, so plan on about a minute per multiple-choice item on average and more for PBQs. Most items in this domain are recall wrapped in a short scenario, so you can often answer in under a minute and bank time. Spend the saved time on DNS exhibits and DHCP PBQs, where reading the output carefully is what earns the point.

## Practice items

These 10 items are original, written for these notes. They are not taken from CompTIA's exam or any prep provider.

### Item 1

A technician is sent to a colocation facility to replace a failed firewall. She needs to know which rack the firewall is in and its exact position in that rack. Which document should she check?

A. Layer 3 network diagram

B. IPAM report

C. Rack diagram

D. Logical diagram

<details>
<summary>Answer and explanation</summary>

**Answer: C.** A rack diagram shows each device's position by rack unit, which is exactly what she needs on site. (Objective 3.1)

- **A** is wrong because a Layer 3 diagram shows subnets and routing, not physical placement.
- **B** is wrong because IPAM tracks addresses, not where hardware sits.
- **D** is wrong because a logical diagram shows traffic flow and ignores physical location.

</details>

### Item 2

A network engineer wants to change the native VLAN on the trunk between two core switches to close a security finding. The change is planned but not yet made. What should the engineer do FIRST?

A. Submit a change request and get it approved

B. Make the change during lunch when traffic is low

C. Update the Layer 2 diagram to show the new native VLAN

D. Copy the new configuration to the golden configuration

<details>
<summary>Answer and explanation</summary>

**Answer: A.** Change management starts with a tracked request that is reviewed and approved before anything is touched. (Objective 3.1)

- **B** is wrong because making an unapproved change, even at a quiet time, skips the process that checks risk and schedules a rollback.
- **C** is wrong because documentation is updated after the change is made and verified.
- **D** is wrong because the golden configuration should only change once the change is approved and proven.

</details>

### Item 3

An auditor reports that the monitoring server polls routers with a community string that can be read off the wire. The team must keep SNMP but needs each poll authenticated and encrypted. What should they configure?

A. SNMPv2c with a longer community string

B. Syslog over UDP 514

C. SNMP traps on UDP 162

D. SNMPv3 with the authPriv security level

<details>
<summary>Answer and explanation</summary>

**Answer: D.** SNMPv3 adds user-based authentication, and authPriv adds encryption as well. (Objective 3.2)

- **A** is wrong because v2c sends any community string in plaintext, however long.
- **B** is wrong because syslog carries log messages, not polling, and plain syslog is unencrypted.
- **C** is wrong because traps are a message type, not a security setting, and v2c traps are still unencrypted.

</details>

### Item 4

A security analyst wants an intrusion detection sensor to see every frame entering and leaving the switch port that connects the internet router. The sensor plugs into a spare port on the same switch. What should be configured on the switch?

A. An SNMP trap destination

B. Port mirroring from the router port to the sensor port

C. A DHCP reservation for the sensor

D. Scheduled network discovery

<details>
<summary>Answer and explanation</summary>

**Answer: B.** Port mirroring copies the traffic from the router's port to the sensor's port, so the sensor sees everything without being inline. (Objective 3.2)

- **A** is wrong because traps are event alerts, not copies of traffic.
- **C** is wrong because a reservation only fixes the sensor's address.
- **D** is wrong because discovery finds devices; it does not deliver traffic to a sensor.

</details>

### Item 5

The finance team says the ordering database can lose no more than 15 minutes of transactions in a disaster. Nightly backups are taken at 01:00. Which DR metric is the current design failing to meet?

A. RPO

B. RTO

C. MTBF

D. MTTR

<details>
<summary>Answer and explanation</summary>

**Answer: A.** The tolerable amount of data loss, expressed as time, is the recovery point objective. Nightly backups can lose up to 24 hours of data. (Objective 3.3)

- **B** is wrong because RTO is about how long the service may be down, which the stem does not mention.
- **C** is wrong because MTBF measures reliability between failures.
- **D** is wrong because MTTR is the average time to repair a fault.

</details>

### Item 6

A company's recovery site already has servers, power and network links installed and ready, but holds no current data: the latest backups are restored onto it only when the site is activated, which takes several hours. Which type of site is this?

A. Cold site

B. Hot site

C. Warm site

D. Active-active site

<details>
<summary>Answer and explanation</summary>

**Answer: C.** A warm site has hardware and connectivity ready but needs current data loaded, giving recovery in hours to days. (Objective 3.3)

- **A** is wrong because a cold site has no installed equipment.
- **B** is wrong because a hot site already holds current, replicated data.
- **D** is wrong because active-active means both locations carry live traffic, so nothing needs restoring.

</details>

### Item 7

A new VLAN 40 is created for a training room. The router has an interface in VLAN 40 and a DHCP scope for that subnet exists on the server at 10.0.1.10, which sits in the server VLAN. Training room PCs get addresses in 169.254.0.0/16. What is the MOST likely fix?

A. Add an exclusion range to the VLAN 40 scope

B. Configure an IP helper on the router's VLAN 40 interface pointing to 10.0.1.10

C. Lower the lease time on the VLAN 40 scope

D. Add a PTR record for 10.0.1.10

<details>
<summary>Answer and explanation</summary>

**Answer: B.** DHCP discovers are broadcasts and do not cross the router. A relay (IP helper) forwards them to the server on the other subnet. (Objective 3.4)

- **A** is wrong because exclusions reduce the pool; they do not help requests reach the server.
- **C** is wrong because lease time matters only after clients get an address.
- **D** is wrong because a PTR record affects reverse DNS, not DHCP.

</details>

### Item 8

A help desk technician runs this command from a workstation:

```
C:\>nslookup intranet.example.com
Server:  dc1.corp.example.com
Address:  10.0.0.53

Non-authoritative answer:
Name:    intranet.example.com
Address:  203.0.113.40
```

What does "Non-authoritative answer" tell the technician?

A. The record is forged and DNSSEC validation failed

B. The name does not exist in DNS

C. The workstation used its hosts file instead of DNS

D. The answer came from the resolver's cache, not from a server that holds the zone

<details>
<summary>Answer and explanation</summary>

**Answer: D.** A non-authoritative answer is one the resolver (dc1) had cached from an earlier lookup, rather than one served by an authoritative server for the zone. It is normal. (Objective 3.4)

- **A** is wrong because a failed DNSSEC check gives an error, not a normal answer.
- **B** is wrong because an address was returned.
- **C** is wrong because nslookup queries DNS directly and does not read the hosts file.

</details>

### Item 9

A company is moving its email to a new provider. It needs inbound mail delivered to the provider's servers, and the provider's spam checks require a reverse lookup of the company's outbound mail relay to return its name. Which two record types are needed? Choose two.

A. CNAME

B. MX

C. AAAA

D. NS

E. PTR

<details>
<summary>Answer and explanation</summary>

**Answer: B and E.** MX records tell other servers where to deliver mail for the domain. A PTR record in the reverse zone maps the relay's IP address back to its name. (Objective 3.4)

- **A** is wrong because a CNAME creates an alias; MX records must not point to a CNAME, and it does not do reverse lookups.
- **C** is wrong because AAAA maps a name to an IPv6 address, a forward lookup.
- **D** is wrong because NS records name the zone's DNS servers, not its mail servers.

</details>

### Item 10

A remote branch's only WAN link fails, and engineers at head office lose all access to the branch router. They want to make sure they can still reach the router's command line the next time this happens. What should they add?

A. A console server at the branch with its own cellular connection

B. A clientless VPN portal at head office

C. A jump box at head office

D. SSH access on the router's LAN interface

<details>
<summary>Answer and explanation</summary>

**Answer: A.** Out-of-band management uses a separate path. A console server with cellular access reaches the router's console port even when the WAN is down. (Objective 3.5)

- **B** is wrong because a VPN portal still depends on reaching the branch over the failed link.
- **C** is wrong because a jump box controls how admins connect, but it still uses the same in-band path.
- **D** is wrong because SSH over the LAN interface is in-band and unreachable from head office during the outage.

</details>

## Score yourself

| Item | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| Answer | C | A | D | B | A | C | B | D | B, E | A |
| Objective | 3.1 | 3.1 | 3.2 | 3.2 | 3.3 | 3.3 | 3.4 | 3.4 | 3.4 | 3.5 |

- **9 to 10:** strong. Keep the port numbers and record types fresh with short daily recall.
- **7 to 8:** list the objectives you missed. SNMP versions, DHCP relay and DNS records are the usual gaps; reread [3.2 Network monitoring technologies](#32-network-monitoring-technologies) and [3.4 IPv4 and IPv6 network services](#34-ipv4-and-ipv6-network-services).
- **6 or fewer:** spend a study session here. Write the RPO, RTO, MTTR and MTBF definitions from memory, draw the three DR sites with their recovery speeds, and redo the items after rereading [Key concepts](#key-concepts-explained).

## Next step

1. Redo any item you missed without opening the explanation, and write the trap you fell for in one line.
2. On your own machine, run `nslookup -type=mx` and `nslookup -type=txt` against a domain you use, then `nslookup` with an IP address to see a PTR lookup. Write down which answers are authoritative and which are not.
3. Take a timed run with the [free Network+ (N10-009) practice test on PrepClubs](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus) to practice pacing across the whole exam.

---

<!-- nav:bottom -->← Previous: [Domain 2: Network Implementation](2-network-implementation.md) | Next: [Domain 4: Network Security](4-network-security.md) → | [Back to contents](../../README.md#contents)<!-- /nav:bottom -->
