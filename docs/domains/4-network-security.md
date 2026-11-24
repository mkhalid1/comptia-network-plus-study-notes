<!-- nav:top -->[Contents](../../README.md#contents) · Previous: [Domain 3: Network Operations](3-network-operations.md) · Next: [Domain 5: Network Troubleshooting](5-network-troubleshooting.md)<!-- /nav:top -->

# Domain 4: Network Security

**Weight:** 14% of the exam (CompTIA's figure), the lightest domain. By our estimate that is roughly 12 of up to 90 questions. **Objectives:** 4.1 to 4.3.

This domain covers the security a network technician has to understand and apply: the basic concepts, the attacks that target switches, Wi-Fi and name resolution, and the features that stop them. It has only three objectives, but each one is broad, and the attack names are close enough to each other that careless reading costs points. Match every attack to the layer and device it abuses, and to the control that stops it.

## What this domain measures

Objective 4.1 is vocabulary and concepts, 4.2 is recognizing attacks from their symptoms, and 4.3 is choosing and applying defenses in a scenario.

| Objective | What it asks you to do | Topics you must know |
|---|---|---|
| 4.1 | Explain basic network security concepts | Encryption in transit and at rest, PKI and self-signed certificates, IAM, MFA, SSO, RADIUS, LDAP, SAML, TACACS+, time-based authentication, least privilege, RBAC, geofencing, cameras and locks, honeypots and honeynets, risk, vulnerability, exploit, threat, CIA triad, data locality, PCI DSS, GDPR, segmentation for IoT, IIoT, SCADA, ICS, OT, guest and BYOD |
| 4.2 | Summarize attacks and their impact | DoS and DDoS, VLAN hopping, MAC flooding, ARP poisoning and spoofing, DNS poisoning and spoofing, rogue DHCP and rogue APs, evil twin, on-path, phishing, dumpster diving, shoulder surfing, tailgating, malware |
| 4.3 | Apply security features, defenses and solutions | Disabling unused ports and services, changing default passwords, NAC, port security, 802.1X, MAC filtering, key management, ACLs, URL and content filtering, trusted and untrusted zones, screened subnet |

## Key concepts explained

### 4.1 Basic network security concepts

**Common security terms.** These four are easy to mix up, and questions test them directly.

| Term | Meaning | Example |
|---|---|---|
| Vulnerability | A weakness | A switch still running firmware with a known flaw |
| Threat | Something that could exploit a weakness | An attacker, malware, or even a flood |
| Exploit | The method or code that takes advantage of a vulnerability | A script that uses the firmware flaw to get admin access |
| Risk | The likelihood that a threat exploits a vulnerability, combined with the impact | High if the switch is reachable from the internet and runs the core network |

**The CIA triad.**

- **Confidentiality:** only authorized people can read the data. Encryption and access control protect it.
- **Integrity:** data is not changed without detection. Hashes, digital signatures and DNSSEC protect it.
- **Availability:** systems and data are there when needed. Redundancy, backups and DDoS protection protect it. A DoS attack is an attack on availability.

**Encryption.** **Data in transit** is protected with protocols such as TLS, IPsec, SSH and WPA3. **Data at rest** is protected with disk, volume, database or file encryption, usually AES.

**Certificates.** In a **public key infrastructure (PKI)**, a certificate authority (CA) signs certificates, and clients trust them because they trust the CA. A **self-signed certificate** is signed by its own key, not a CA. It still encrypts traffic, but clients cannot verify who issued it, so browsers warn unless the certificate is installed as trusted. Self-signed certificates are fine for a lab or an internal device you control, not for public services.

**Identity and access management (IAM).** Authentication proves who you are. Authorization decides what you may do.

| Authentication item | What it is |
|---|---|
| Multifactor authentication (MFA) | Two or more factors from different categories: something you know, have, or are, plus location |
| Single sign-on (SSO) | Log in once, reach many applications |
| Time-based authentication | One-time codes that change every 30 seconds or so (TOTP), from an app or token; also logins allowed only during set hours |
| RADIUS | AAA server for network access: VPN, Wi-Fi with 802.1X, switch ports. UDP 1812 (authentication) and 1813 (accounting). Encrypts only the password |
| TACACS+ | AAA protocol mostly used for administrator logins to network devices. TCP 49. Encrypts the whole payload and separates authentication, authorization and accounting, so it can authorize each command |
| LDAP | Directory protocol to look up and authenticate users, TCP 389. LDAPS (LDAP over TLS) uses TCP 636 |
| SAML | XML-based standard that passes authentication assertions from an identity provider to a service, common for web SSO and federation |

How to tell RADIUS from TACACS+: if the question is about users joining Wi-Fi or VPN, think RADIUS. If it is about controlling which commands each admin can run on routers and switches, think TACACS+.

**Authorization.**

- **Least privilege:** give each account only the access it needs for its job, and nothing more.
- **Role-based access control (RBAC):** permissions are attached to roles such as "Help desk" or "Network admin", and users get the role.

**Geofencing** uses location (GPS, Wi-Fi or IP) to allow or block access, or to trigger an action, when a device enters or leaves an area. For example, a company app may only work on company premises.

**Physical security.** **Cameras** deter and record. **Locks** (keys, badge readers, biometric locks, locked racks and cabinets) prevent physical access. Network security fails if anyone can walk up to the switch.

**Deception technologies.** A **honeypot** is a decoy system that looks valuable and has no real users, so anything touching it is suspicious. A **honeynet** is a network of honeypots. They detect attackers early and show their methods.

**Audits and regulatory compliance.**

| Item | What it requires |
|---|---|
| Data locality | Data must be stored and processed in a particular country or region, often because of law |
| PCI DSS | Security standard for organizations that store, process or transmit payment card data: segmentation, encryption, access control, logging, testing |
| GDPR | EU regulation protecting personal data of people in the EU: lawful processing, breach notification, rights such as access and erasure, and limits on transfers outside the EU |

**Network segmentation enforcement.** Put devices with different risk levels on separate segments (VLANs or subnets) and control traffic between them with ACLs or firewalls.

| Segment | Why it is separated |
|---|---|
| IoT and IIoT | Cameras, sensors and smart devices are often hard to patch and use weak defaults |
| SCADA, ICS, OT | Industrial control systems run physical processes and old protocols; an outage can stop a plant or harm people |
| Guest | Visitors need internet only, never internal resources |
| BYOD | Personal devices are not managed by the company, so they get limited access |

### 4.2 Attacks and their impact

**Layer 2 attacks.**

| Attack | How it works | Impact | Main defense |
|---|---|---|---|
| VLAN hopping | **Switch spoofing:** the attacker negotiates a trunk with the switch (for example using DTP). **Double tagging:** the attacker sends frames with two 802.1Q tags; the first is the native VLAN and is stripped, the second delivers the frame to another VLAN | Reaches a VLAN the attacker should not reach | Disable trunk negotiation on access ports, set access ports to access mode, use an unused VLAN as the native VLAN |
| MAC flooding | Floods the switch with frames from thousands of fake source MAC addresses until its MAC address table is full | The switch floods frames out all ports like a hub, so the attacker can capture traffic | Port security limiting MAC addresses per port |
| ARP spoofing | The attacker sends forged ARP replies claiming another host's IP, usually the default gateway | Traffic meant for that IP goes to the attacker | Dynamic ARP inspection, static ARP for critical hosts, segmentation |
| ARP poisoning | The result of spoofed replies: other hosts' ARP caches now hold the wrong MAC for an IP | On-path interception or denial of service | Same as ARP spoofing |

**Name resolution attacks.**

- **DNS poisoning:** false records are inserted into a DNS server's cache (or a hosts file), so everyone using it is sent to the wrong address.
- **DNS spoofing:** the attacker answers DNS queries with forged responses, often from an on-path position, before the real answer arrives.
- Defenses: DNSSEC validation, patched resolvers, encrypted DNS (DoT, DoH), and protecting the hosts file.

**Rogue devices and services.**

- **Rogue DHCP server:** an unauthorized server hands out addresses, often with the attacker's address as gateway or DNS server. Symptoms: clients get the wrong subnet, gateway or DNS. Defense: DHCP snooping on switches, which only trusts DHCP offers from designated ports.
- **Rogue AP:** an unauthorized access point plugged into the wired network, giving a back door around security. Defense: wireless scanning, 802.1X and port security on wired ports.
- **Evil twin:** a fake access point that copies a legitimate SSID so users connect to it. It does not need to touch your wired network. Defense: WPA3-Enterprise with certificate checks, user training, wireless intrusion detection.

**On-path attack** (formerly man-in-the-middle). The attacker sits between two parties, reading or changing traffic. ARP spoofing, DNS spoofing and evil twins are all ways to get on-path. Encryption with proper certificate checks limits the damage.

**Denial of service.** A **DoS** comes from one source. A **DDoS** comes from many sources at once, usually a botnet, so blocking one address does not stop it. Both attack availability. Defenses include upstream filtering, rate limiting and DDoS mitigation services.

**Social engineering.**

| Attack | What happens |
|---|---|
| Phishing | Fake messages trick users into clicking links, opening attachments or giving credentials |
| Dumpster diving | Searching trash for documents, notes or devices with useful information |
| Shoulder surfing | Watching someone type a password or read a screen |
| Tailgating | Following an authorized person through a secure door without badging |

**Malware** is any malicious software: viruses, worms (spread over the network by themselves), trojans, ransomware, spyware and others. On a network, worms and ransomware cause the widest damage.

### 4.3 Network security features, defenses and solutions

**Device hardening.**

- **Disable unused ports and services.** Shut down switch ports nobody uses and turn off services such as Telnet, HTTP management or unused routing protocols. Fewer open doors, fewer attacks.
- **Change default passwords.** Default credentials for devices are published online. Change them before a device goes on the production network.

**Network access control (NAC).** NAC decides whether a device may connect, and to what. It can check identity and posture (patches, antivirus) and place devices in a production, guest or quarantine VLAN.

| Control | What it does | Limits |
|---|---|---|
| Port security | Limits which and how many MAC addresses can use a switch port. Can learn ("sticky") addresses. On violation it can shut down the port, restrict or drop frames | Works on MAC addresses, which can be spoofed |
| 802.1X | Port-based authentication. The **supplicant** (client) sends credentials through the **authenticator** (switch or AP) to the **authentication server** (usually RADIUS). The port passes no normal traffic until authentication succeeds | Needs a supplicant and a RADIUS server |
| MAC filtering | An allow or deny list of MAC addresses, often on wireless | Easy to bypass: MAC addresses are visible and can be spoofed |

**Key management.** Keys need a life cycle: generate them securely, store them protected (for example in a hardware security module), rotate them, revoke them when compromised, and destroy them when retired. A strong algorithm with a leaked key is no protection.

**Security rules.**

- **Access control list (ACL):** an ordered list of permit and deny rules on a router, switch or firewall, matched by addresses, protocols and ports. Rules are read top to bottom, the first match wins, and anything unmatched hits the implicit deny at the end.
- **URL filtering:** allows or blocks web access by address or by category of site.
- **Content filtering:** inspects what is in the traffic (file types, keywords, malware, scripts) and blocks unwanted content even from allowed sites.

Reading an ACL:

```
10 permit tcp 192.168.10.0 0.0.0.255 host 10.1.1.20 eq 443
20 permit udp 192.168.10.0 0.0.0.255 host 10.1.1.53 eq 53
30 deny   ip  192.168.10.0 0.0.0.255 10.1.1.0 0.0.0.255
40 permit ip  any any
```

Hosts in 192.168.10.0/24 can reach the web server 10.1.1.20 on HTTPS and the DNS server 10.1.1.53 on DNS, and nothing else in 10.1.1.0/24. Everything else is allowed by line 40. The 0.0.0.255 is a wildcard mask, the inverse of a /24 subnet mask.

**Zones.**

- **Trusted zone:** the internal network you control.
- **Untrusted zone:** the internet or any network you do not control.
- **Screened subnet** (formerly called a DMZ): a separate zone for servers that the public must reach, such as web and mail servers. The firewall allows the internet in to those specific services only, and limits what the screened subnet can reach inside. If a public server is compromised, the internal network is still protected.

**Matching attacks to defenses.**

| Attack | Defense |
|---|---|
| MAC flooding | Port security |
| VLAN hopping | Disable trunk negotiation, unused native VLAN |
| Rogue DHCP | DHCP snooping |
| ARP spoofing | Dynamic ARP inspection |
| Rogue device on a wall jack | 802.1X, port security, disable unused ports |
| Default credentials | Change default passwords during setup |
| Users browsing malicious sites | URL and content filtering |
| Public server compromise spreading inward | Screened subnet |

## How questions look

Expect scenarios that describe symptoms and ask which attack is MOST likely happening: users on a public SSID see a login page asking for corporate credentials, a switch starts sending everyone's traffic to every port, or the ARP table shows two IPs with the same MAC. Others ask which control BEST stops an attack, where to place a server, or which AAA protocol fits. Exhibits are usually ARP tables, short ACLs, switch port configurations or a list of devices to place in zones. PBQs may ask you to place devices into segments, match attacks to mitigations, or apply hardening steps to a switch.

## A method for these questions

1. Decide whether the item is asking for a concept (4.1), an attack name (4.2) or a defense (4.3).
2. For attack items, find the layer and the device being abused: switch table (MAC flooding), trunks and tags (VLAN hopping), ARP caches (ARP spoofing), DNS answers (DNS spoofing or poisoning), Wi-Fi (evil twin, rogue AP), people (social engineering).
3. Look for the detail that separates close cousins: a fake SSID versus a device on your wired LAN, a forged reply versus a corrupted cache.
4. For defense items, choose the control that works at the same layer as the attack.
5. Prefer controls that cannot be trivially bypassed: 802.1X over MAC filtering, SNMPv3 over v2c, TACACS+ for per-command admin control.
6. For placement items, put public services in the screened subnet and keep IoT, OT and guest devices in their own segments.

## Traps that cost points

- **MAC filtering as strong security.** MAC addresses are sent in the clear and can be copied. Use 802.1X for real authentication.
- **Rogue AP vs evil twin.** A rogue AP is connected to your network without permission. An evil twin imitates your SSID to lure users and may have no link to your network at all.
- **DoS vs DDoS.** Many sources means DDoS, and blocking one IP address will not end it.
- **RADIUS for per-command authorization.** RADIUS bundles authentication and authorization. TACACS+ separates them and authorizes individual commands on network devices.
- **Self-signed means unencrypted.** A self-signed certificate still encrypts. Its problem is trust, not encryption.
- **Threat vs vulnerability.** The weakness is the vulnerability. The attacker or event is the threat.
- **Implicit deny forgotten.** An ACL with only permit lines still blocks everything not listed.
- **Screened subnet for internal servers.** Only services the public must reach belong there. File servers and databases stay inside.
- **Data locality vs GDPR.** Data locality is about where data physically lives. GDPR is the wider EU rulebook for personal data.

## Timing

The exam is one 90-minute block, so aim for about a minute per multiple-choice item on average and allow more for PBQs. Attack identification items here are usually quick once you know the symptoms, so answer them fast. Give ACL and placement PBQs the extra time: read every line of an ACL before you decide.

## Practice items

These 4 items are original, written for these notes. They are not taken from CompTIA's exam or any prep provider.

### Item 1

A network manager wants router and switch administrators to authenticate against a central server. Junior admins must be allowed to run show commands only, while senior admins may change configuration, and every command must be logged. Which protocol fits BEST?

A. RADIUS

B. TACACS+

C. LDAP

D. SAML

<details>
<summary>Answer and explanation</summary>

**Answer: B.** TACACS+ separates authentication, authorization and accounting, so it can authorize and log individual commands on network devices. (Objective 4.1)

- **A** is wrong because RADIUS combines authentication and authorization and is mainly used for network access such as Wi-Fi and VPN, not per-command control.
- **C** is wrong because LDAP is a directory lookup protocol and does not authorize device commands.
- **D** is wrong because SAML is for web single sign-on between an identity provider and a service.

</details>

### Item 2

Users on one subnet report slow and odd web sessions. A technician runs this on an affected PC, where 10.0.4.1 is the default gateway:

```
C:\>arp -a

Interface: 10.0.4.23 --- 0x7
  Internet Address      Physical Address      Type
  10.0.4.1              3c-52-82-1a-9f-04     dynamic
  10.0.4.17             3c-52-82-1a-9f-04     dynamic
  10.0.4.30             f0-1f-af-62-0b-11     dynamic
  10.0.4.255            ff-ff-ff-ff-ff-ff     static
```

The gateway's real MAC address ends in 77-a2. Which attack is MOST likely?

A. MAC flooding

B. ARP poisoning to put 10.0.4.17 on-path

C. DNS poisoning

D. VLAN hopping

<details>
<summary>Answer and explanation</summary>

**Answer: B.** The PC's ARP cache maps the gateway's IP to the MAC of host 10.0.4.17, not the gateway's real MAC. Traffic for the internet now passes through 10.0.4.17, an on-path position. (Objective 4.2)

- **A** is wrong because MAC flooding fills the switch's MAC table; it does not change entries in a PC's ARP cache.
- **C** is wrong because DNS poisoning changes name-to-address answers, and the exhibit shows a wrong address-to-MAC mapping.
- **D** is wrong because VLAN hopping reaches another VLAN; everything here is on the same subnet.

</details>

### Item 3

A penetration tester shows that she can send double-tagged frames from an access port and reach the server VLAN. Which two changes would BEST stop this kind of attack? Choose two.

A. Change the trunk native VLAN to an unused VLAN with no hosts

B. Lower the DHCP lease time

C. Enable URL filtering on the proxy

D. Set user ports to access mode and disable trunk negotiation

E. Add a PTR record for each server

<details>
<summary>Answer and explanation</summary>

**Answer: A and D.** Double tagging relies on the attacker's access VLAN being the trunk's native VLAN, so moving the native VLAN to an unused one breaks it. Forcing access mode and disabling negotiation also stops switch spoofing, the other VLAN hopping method. (Objective 4.2)

- **B** is wrong because lease time affects DHCP addressing, not VLAN tags.
- **C** is wrong because URL filtering controls web access, not Layer 2 frames.
- **E** is wrong because PTR records are for reverse DNS and have nothing to do with VLANs.

</details>

### Item 4

A company is launching a public web store. The web server must be reachable from the internet, but if it is ever compromised the attacker must not be able to reach the internal file and database servers directly. Where should the web server go?

A. In a screened subnet between the internet firewall and the internal network

B. On the internal LAN with a host firewall

C. On the guest Wi-Fi network

D. On the same VLAN as the database server for speed

<details>
<summary>Answer and explanation</summary>

**Answer: A.** A screened subnet holds public-facing servers, with firewall rules limiting what reaches them from the internet and what they can reach inside. (Objective 4.3)

- **B** is wrong because a compromised server on the internal LAN would sit next to the systems you are protecting.
- **C** is wrong because the guest network is for visitors' internet access, not hosting public services.
- **D** is wrong because sharing a VLAN with the database gives an attacker direct access to it.

</details>

Want more? The free timed Network+ (N10-009) practice test on PrepClubs has 25 questions, with a domain-by-domain breakdown: [take the free test](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus).

## Score yourself

| Item | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| Answer | B | B | A, D | A |
| Objective | 4.1 | 4.2 | 4.2 | 4.3 |

- **4:** strong. Keep the attack and defense pairs in a flashcard deck and review them weekly.
- **3:** list the objectives you missed. Layer 2 attacks and the RADIUS versus TACACS+ split are the usual gaps; reread [4.2 Attacks and their impact](#42-attacks-and-their-impact) and the IAM table in [4.1 Basic network security concepts](#41-basic-network-security-concepts).
- **2 or fewer:** spend a study session here. Write the "Matching attacks to defenses" table from memory, then reread [4.3 Network security features, defenses and solutions](#43-network-security-features-defenses-and-solutions) and redo the items.

## Next step

1. Redo any item you missed without opening the explanation, and write the trap you fell for in one line.
2. Run `arp -a` on your own computer and find your default gateway's entry. Note its MAC address, then check it again on another day: if it ever matches another host's MAC, you know what that means.
3. Take a timed run with the [free Network+ (N10-009) practice test on PrepClubs](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus) to practice pacing across the whole exam.

---

<!-- nav:bottom -->← Previous: [Domain 3: Network Operations](3-network-operations.md) | Next: [Domain 5: Network Troubleshooting](5-network-troubleshooting.md) → | [Back to contents](../../README.md#contents)<!-- /nav:bottom -->
