<!-- nav:top -->[Contents](../../README.md#contents) · Previous: [Domain 1: Networking Concepts](1-networking-concepts.md) · Next: [Domain 3: Network Operations](3-network-operations.md)<!-- /nav:top -->

# Domain 2: Network Implementation

**Weight:** 20% of the exam (CompTIA's figure). By our estimate that is roughly 18 of up to 90 questions. **Objectives:** 2.1 to 2.4.

This domain is about building the network: how routers choose a path, how switches are configured with VLANs and trunks, how wireless is planned and secured, and how the equipment is racked, powered and cooled. Two of its four objectives are "given a scenario" objectives, so expect to read a routing table, a switch port setting or a wireless plan and decide what is wrong or what fits. Candidates lose marks when they know the terms but cannot apply the selection rules, such as longest prefix before administrative distance.

## What this domain measures

| Objective | What it asks you to do | Topics you must know |
|---|---|---|
| 2.1 | Explain routing technologies | Static and dynamic routing, BGP, EIGRP, OSPF, route selection by prefix length, administrative distance and metric, NAT, PAT, FHRP, virtual IP, subinterfaces |
| 2.2 | Configure switching technologies and features (scenario) | VLANs and the VLAN database, SVIs, native and voice VLANs, 802.1Q tagging, link aggregation, speed and duplex, spanning tree, MTU and jumbo frames |
| 2.3 | Select and configure wireless devices and technologies (scenario) | Channels, channel width, non-overlapping channels, 802.11h, 2.4, 5 and 6 GHz, band steering, SSID, BSSID, ESSID, network types, WPA2, WPA3, guest networks and captive portals, PSK vs Enterprise, antennas, autonomous vs lightweight APs |
| 2.4 | Explain factors of physical installations | MDF and IDF, rack size, port-side exhaust and intake, patch panels, fiber distribution panels, lockable racks, UPS, PDU, power load, voltage, humidity, fire suppression, temperature |

## Key concepts explained

### 2.1 Routing technologies

**Static vs dynamic routing.**

| | Static | Dynamic |
|---|---|---|
| How routes appear | An administrator types them in | Routers learn them from each other with a routing protocol |
| Reacts to failures | No, unless a backup route is configured | Yes, routes converge around failed links |
| Overhead | None on the network | Protocol traffic and CPU |
| Best for | Small networks, stub sites with one exit, default routes | Networks with many routers or redundant paths |

A **default route** (0.0.0.0/0) is the route of last resort: it matches anything when nothing more specific does.

**The three dynamic protocols on the objectives.**

| Protocol | Type | Metric | Where it is used |
|---|---|---|---|
| OSPF (Open Shortest Path First) | Link state, open standard | Cost, based on interface bandwidth | Inside an organization. Uses areas; area 0 is the backbone |
| EIGRP (Enhanced Interior Gateway Routing Protocol) | Advanced distance vector, originally Cisco | Composite, by default bandwidth and delay | Inside an organization, mostly Cisco networks |
| BGP (Border Gateway Protocol) | Path vector | Path attributes, such as AS path length | Between autonomous systems: the protocol of the internet, and for organizations with links to several ISPs. Runs over TCP 179 |

OSPF and EIGRP are **interior** gateway protocols (inside one autonomous system). BGP is the **exterior** gateway protocol.

**Route selection.** When a router has several routes that could reach a destination, it decides in this order:

1. **Prefix length (longest match wins).** The most specific route is used. A /24 beats a /16 beats the default /0, regardless of protocol.
2. **Administrative distance** breaks ties between different sources offering the **same** prefix. Lower is more trusted.
3. **Metric** breaks ties within one protocol. Lower is better.

Cisco default administrative distances, the values usually taught:

| Route source | Administrative distance |
|---|---|
| Directly connected | 0 |
| Static | 1 |
| External BGP | 20 |
| EIGRP (internal) | 90 |
| OSPF | 110 |
| RIP | 120 |
| Internal BGP | 200 |

A **floating static route** is a static route given a higher administrative distance than the dynamic route, so it is used only when the dynamic route disappears.

**Address translation.**

- **NAT** translates private addresses to public ones. Static NAT maps one inside address to one public address, often for a server that must be reachable from outside.
- **PAT** (port address translation, also called NAT overload) lets many inside hosts share one public address by tracking each connection with a different source port. It is what nearly every home and office router does.

**FHRP and VIP.** Hosts have a single default gateway. If that router fails, they are cut off. A **first hop redundancy protocol** makes two or more routers share a **virtual IP (VIP)** address (and a virtual MAC). Hosts point to the VIP; one router is active, and if it fails another takes over the VIP within seconds. Examples: VRRP (open standard), HSRP (Cisco), GLBP (Cisco, also load balances).

**Subinterfaces.** One physical router interface split into logical interfaces, one per VLAN, each with its own IP address and 802.1Q tag. This is "router on a stick": the router routes between VLANs that reach it over a single trunk link.

```
interface GigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
interface GigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

### 2.2 Switching technologies

**VLANs.** A VLAN splits one physical switch into separate layer 2 networks, each its own broadcast domain and usually its own IP subnet. Traffic between VLANs must be routed. The **VLAN database** is the list of VLANs defined on a switch; a port assigned to a VLAN that does not exist in the database will not pass traffic.

**SVI (switch virtual interface).** A virtual layer 3 interface on a multilayer switch, one per VLAN (for example "interface vlan 10" with 192.168.10.1). SVIs act as the default gateway for each VLAN and route between them inside the switch, faster than router on a stick. A management SVI also gives a layer 2 switch an address you can SSH to.

**Interface configuration.**

| Setting | What it means |
|---|---|
| Access port | Carries one VLAN, untagged. For PCs, printers, APs on a single VLAN |
| Trunk port | Carries many VLANs between switches, routers or hypervisors, using **802.1Q tags** |
| 802.1Q tagging | Inserts a 4-byte tag into the Ethernet frame. The tag has a 12-bit VLAN ID, so VLANs 1 to 4094 are usable |
| Native VLAN | The one VLAN on a trunk that is sent untagged. Default is VLAN 1. Both ends of a trunk must agree, or traffic leaks between VLANs and switches log mismatch errors. Changing it from VLAN 1 to an unused VLAN is a hardening step |
| Voice VLAN | A separate VLAN for IP phones on the same access port as a PC. The phone tags its voice traffic; the PC plugged into the phone stays on the data VLAN untagged. Lets QoS and security treat voice separately |
| Link aggregation | Bundles several physical links into one logical link for more bandwidth and redundancy. LACP (IEEE 802.3ad, now 802.1AX) negotiates the bundle. Speed, duplex and VLAN settings must match on all members |
| Speed | 10, 100, 1000 Mbps and up. Both ends must agree |
| Duplex | Full duplex sends and receives at once; half duplex takes turns. Auto-negotiation should be on both ends or set identically on both |

**Duplex mismatch.** If one end is hard set to full and the other auto-negotiates, the auto side usually falls back to half duplex. The link works but is slow. The half-duplex side counts **late collisions**; the full-duplex side counts **CRC or FCS errors** and runts. Fix: make both ends match.

**Spanning tree (STP, IEEE 802.1D).** Redundant switch links create layer 2 loops. Ethernet frames have no TTL, so a loop causes a **broadcast storm** and an unstable MAC address table within seconds. STP prevents this by blocking redundant ports.

- Switches elect a **root bridge**: the lowest bridge ID (priority, then MAC address). Set the priority on purpose so a core switch wins.
- Each other switch picks a **root port** (best path to the root); each segment has one **designated port**; remaining redundant ports are **blocked**.
- Classic port states: blocking, listening, learning, forwarding (plus disabled). Classic STP takes 30 to 50 seconds to converge. **Rapid STP** (802.1w) converges in a few seconds.
- Edge features: **PortFast** puts a port connected to an end device straight into forwarding. **BPDU guard** shuts a PortFast port down if a switch is plugged into it.

**MTU and jumbo frames.** The standard Ethernet MTU is 1500 bytes of payload. **Jumbo frames** raise it, commonly to about 9000 bytes, to cut overhead for storage traffic such as iSCSI and for backups. Every device in the path must support the same MTU; one switch left at 1500 causes drops or fragmentation that look like random slowness.

### 2.3 Wireless devices and technologies

**Frequency bands.**

| Band | Range and penetration | Channels | Notes |
|---|---|---|---|
| 2.4 GHz | Longest range, best through walls | Only three non-overlapping 20 MHz channels: 1, 6 and 11 (in North America) | Crowded; shares space with Bluetooth and microwave ovens |
| 5 GHz | Shorter range | Many non-overlapping channels | Some channels require DFS (see 802.11h below) |
| 6 GHz | Shortest range | The most clean spectrum, many wide channels | Wi-Fi 6E and later; WPA3 is required |

**Channel width.** Wider channels (20, 40, 80, 160 MHz) give more throughput per client but leave fewer non-overlapping channels, so neighboring APs interfere more. In a dense office, 20 or 40 MHz on 5 GHz often performs better overall than 80 MHz. On 2.4 GHz, stick to 20 MHz.

**Regulatory impacts and 802.11h.** Countries set which channels and power levels are legal. **802.11h** adds **DFS** (dynamic frequency selection: an AP using certain 5 GHz channels must listen for radar and move off the channel if it detects it) and **TPC** (transmit power control: limit power to what is needed). If clients drop briefly and the AP logs show radar detection, DFS is doing its job.

**Band steering** nudges dual-band clients toward 5 GHz (or 6 GHz), leaving 2.4 GHz for older devices.

**SSID, BSSID, ESSID.**

- **SSID:** the network name users see.
- **BSSID:** the MAC address of one AP radio. It identifies one basic service set (one AP's cell).
- **ESSID:** the same SSID served by many APs, forming an extended service set so clients roam between APs without reconnecting to a different network.

**Network types.**

| Type | Description |
|---|---|
| Infrastructure | Clients connect through APs. The normal enterprise and home setup |
| Ad hoc | Devices connect directly to each other, no AP |
| Mesh | APs connect to each other wirelessly, and only some need a wired uplink. Good where running cable is hard |
| Point to point | A wireless bridge linking two sites, usually with directional antennas |

**Encryption and authentication.**

| | WPA2 | WPA3 |
|---|---|---|
| Encryption | AES with CCMP | AES with GCMP and CCMP; 192-bit mode for Enterprise |
| Personal mode | PSK, a shared passphrase; the captured handshake can be attacked offline | SAE (Simultaneous Authentication of Equals), which resists offline guessing |
| Management frames | Protection optional | Protected management frames required |

**PSK vs Enterprise.** **PSK** (Personal) uses one shared passphrase for everyone; when someone leaves, the passphrase must change on every device. **Enterprise** uses 802.1X: each user or device authenticates with its own credentials or certificate through EAP to a **RADIUS** server. Access can be revoked per user, and traffic is keyed per session. Choose Enterprise for any business network with staff accounts.

**Guest networks.** Give guests a separate SSID mapped to an isolated VLAN that reaches only the internet. **Client isolation** stops guests talking to each other. A **captive portal** is the web page that appears before access, used to accept terms, enter a voucher code or register.

**Antennas.**

- **Omnidirectional:** radiates in all directions horizontally. Ceiling APs covering a room or floor.
- **Directional** (Yagi, patch, parabolic dish): focuses energy in one direction for long range. Point to point links between buildings, long corridors, warehouse aisles.

**Autonomous vs lightweight APs.** An **autonomous** AP is configured and managed on its own, which is fine for a few APs. A **lightweight** AP is managed by a **wireless LAN controller** (on premises or in the cloud), which pushes configuration, picks channels and power, and coordinates roaming. Choose lightweight for anything beyond a handful of APs.

### 2.4 Physical installations

**Locations.**

- **MDF (main distribution frame):** the main wiring room or closet, where the building's core equipment and the connection from the service provider usually terminate.
- **IDF (intermediate distribution frame):** a smaller closet on each floor or wing, linked back to the MDF (often by fiber), with the access switches for nearby users. IDFs keep horizontal copper runs within the 100 m limit.

**Racks.**

- **Rack size** is measured in rack units: 1U = 1.75 inches (44.45 mm) of height. A full-height rack is commonly 42U. Check depth too, since long servers need deep racks.
- **Port-side exhaust vs port-side intake:** switches move air either front to back or back to front. Data centers use hot and cold aisles. If a switch's ports face the hot aisle (common for top-of-rack switches mounted to match the server ports), it needs **port-side exhaust** so it pulls cold air from the cold aisle and blows hot air out the port side. The wrong airflow model makes the switch breathe its own hot air.
- **Lockable** racks and cabinets restrict physical access to equipment.

**Cabling.**

- **Patch panel:** the permanent horizontal cables from wall jacks terminate on the back; short patch cables connect the front to switch ports. Changes happen at the panel, not by re-running cable.
- **Fiber distribution panel:** the same idea for fiber, terminating and protecting fiber strands and providing connectors for fiber patch cords.

**Power.**

| Item | Purpose |
|---|---|
| UPS (uninterruptible power supply) | Battery that keeps equipment running through outages and smooths spikes and sags. Gives time to shut down cleanly or for a generator to start. Sized in VA and watts, with runtime at a given load |
| PDU (power distribution unit) | The power strip of a rack. Distributes power to devices; metered PDUs show load, switched PDUs let you power-cycle outlets remotely |
| Power load | The total draw of the devices on a circuit, UPS or PDU. Add up the devices before adding a new one. A common rule of thumb is to keep continuous load at no more than 80% of the circuit rating |
| Voltage | Match device power supplies to the supply. North American offices use 120 V; data centers often use 208 V or 230 V circuits. Most IT power supplies accept 100 to 240 V, but check |

**Environmental factors.**

- **Temperature:** too hot shortens equipment life and causes shutdowns. Use hot and cold aisles and monitor with sensors.
- **Humidity:** too dry raises static discharge risk; too humid causes condensation and corrosion. Keep it in the middle range and monitor it.
- **Fire suppression:** water sprinklers damage electronics. Server rooms often use **clean agent** gas systems that put out fire without residue, or **pre-action** sprinklers whose pipes stay dry until a detector and a sprinkler head both trigger.

## How questions look

Expect "Given a scenario" stems such as: users on one VLAN cannot reach another, a new phone and PC must share one port, a link is slow and the interface shows errors, a router has several routes to a destination, a warehouse needs Wi-Fi coverage, guests need internet access only, or a new switch is going into a rack. Exhibits include routing tables, `show interface` style output, switch port configuration and wireless survey summaries. Performance-based items commonly ask you to configure VLANs and trunks on switch ports, assign wireless channels and security settings to APs, or pick the route a router will use.

## A method for these questions

1. Name the layer: routing (layer 3), switching (layer 2), wireless or physical. It rules out half the options.
2. For routing tables, find every route that contains the destination, keep the longest prefix, then use administrative distance, then metric.
3. For switching problems, check in order: is the port in the right VLAN, is the VLAN in the database, is the trunk allowing it with matching native VLANs, are speed and duplex the same on both ends.
4. For wireless planning, pick the band from range and density needs, then non-overlapping channels, then width, then security (Enterprise for staff, isolated guest network with portal for visitors).
5. For physical items, think airflow, power budget and access control before anything else.
6. When two options both work, choose the one that is standard practice and the least effort to manage at scale (controller-based APs, 802.1X, LACP, SVIs).

## Traps that cost points

- **Administrative distance before prefix length.** The longest prefix wins first. AD only compares routes to the same prefix.
- **Higher metric looks better.** For both AD and metric, lower wins.
- **NAT vs PAT.** Many hosts sharing one public address is PAT. One-to-one mapping for a published server is static NAT.
- **Native VLAN mismatch dismissed.** The link stays up, which is why it is missed. Untagged traffic ends up in the wrong VLAN on the other side.
- **Fixing duplex by replacing the cable.** Late collisions and CRC errors with a good cable point to a duplex mismatch. Set both ends the same.
- **Disabling STP to stop blocked ports.** The blocked port is protecting you from a loop. Tune the root bridge or use RSTP instead.
- **Jumbo frames on one hop only.** The MTU must match along the whole path.
- **Channels 1, 2 and 3 on 2.4 GHz.** These overlap. Use 1, 6 and 11.
- **WPA2-PSK for staff.** A shared key cannot be revoked for one person. Use Enterprise with RADIUS.

## Timing

Routing table and interface output items take longer than recall items: give them about 90 seconds and work the steps on scratch paper. Wireless and physical installation items are usually quick if you know the tables. VLAN and wireless configuration PBQs can take several minutes; if one looks long, flag it and come back after your first pass.

## Practice items

These 10 items are original, written for these notes. They are not taken from CompTIA's exam or any prep provider.

### Item 1

A router has these routes. A packet arrives for 10.20.30.200. Which next hop does the router use?

```
Destination       Next hop       Source   [AD/metric]
0.0.0.0/0         203.0.113.1    static   [1/0]
10.20.0.0/16      10.0.0.2       OSPF     [110/20]
10.20.30.0/24     10.0.0.6       OSPF     [110/50]
10.20.30.0/25     10.0.0.10      static   [1/0]
```

A. 203.0.113.1

B. 10.0.0.2

C. 10.0.0.6

D. 10.0.0.10

<details>
<summary>Answer and explanation</summary>

**Answer: C.** 10.20.30.0/25 covers only 10.20.30.0 to 10.20.30.127, so it does not contain .200. Of the routes that do match, 10.20.30.0/24 is the longest prefix, so the router uses next hop 10.0.0.6 even though its metric is higher. (Objective 2.1)

- **A** is wrong because the default route is used only when nothing more specific matches.
- **B** is wrong because the /16 is less specific than the matching /24.
- **D** is wrong because the /25 does not include the destination, despite its low administrative distance.

</details>

### Item 2

A router learns 172.18.4.0/24 from both OSPF and EIGRP (internal), with default settings. Which route goes into the routing table?

A. The EIGRP route, because its administrative distance is lower

B. The OSPF route, because link-state protocols are always preferred

C. Both, load balanced equally, because the prefixes match

D. The OSPF route, because its metric number is smaller

<details>
<summary>Answer and explanation</summary>

**Answer: A.** With the same prefix length, the router compares administrative distance. Internal EIGRP is 90 and OSPF is 110, so EIGRP wins. (Objective 2.1)

- **B** is wrong because there is no rule preferring link-state protocols; administrative distance decides.
- **C** is wrong because routers do not load balance across different protocols for one prefix by default.
- **D** is wrong because metrics from different protocols are not comparable; metric only breaks ties within one protocol.

</details>

### Item 3

A branch has two routers connected to the user VLAN. Users must keep working if either router fails, without anyone changing the default gateway on their PCs. What should the administrator configure?

A. PAT on both routers

B. Subinterfaces on one router

C. BGP between the two routers

D. An FHRP such as VRRP, with hosts using the virtual IP as their gateway

<details>
<summary>Answer and explanation</summary>

**Answer: D.** A first hop redundancy protocol lets the routers share a virtual IP. If the active router fails, the standby takes over the VIP and hosts keep the same gateway. (Objective 2.1)

- **A** is wrong because PAT translates addresses; it gives no gateway failover.
- **B** is wrong because subinterfaces on one router still leave that router as a single point of failure.
- **C** is wrong because BGP exchanges routes between autonomous systems; it does not give hosts a redundant gateway address.

</details>

### Item 4

An office is installing IP phones. Each desk has one network jack. The PC will plug into the phone, and the phone into the jack. Voice must be prioritized and kept separate from data. What should the switch port be configured with?

A. A trunk carrying all VLANs with VLAN 1 as native

B. An access VLAN for data and a voice VLAN for the phone

C. Jumbo frames and half duplex

D. Link aggregation with a second port

<details>
<summary>Answer and explanation</summary>

**Answer: B.** The voice VLAN feature lets the phone tag its traffic into a separate VLAN while the PC's traffic stays untagged on the data VLAN, on one port. (Objective 2.2)

- **A** is wrong because carrying every VLAN to a desk port is unnecessary and insecure.
- **C** is wrong because neither setting separates or prioritizes voice, and half duplex would slow the port.
- **D** is wrong because there is only one jack, and aggregation adds bandwidth, not voice separation.

</details>

### Item 5

Users on a file server complain that transfers are slow. The cable tested good. The switch port facing the server shows:

```
GigabitEthernet1/0/14 is up, line protocol is up
  Half-duplex, 100Mb/s, media type is 10/100/1000BaseTX
  Auto-duplex, Auto-speed
  0 input errors, 0 CRC
  1874 late collision, 2210 collisions
```

The server NIC is set manually to 100 Mbps full duplex. What is the BEST fix?

A. Replace the cable with Cat 6a

B. Move the port to a different VLAN

C. Enable jumbo frames on the port

D. Set both the NIC and the switch port to matching settings, ideally auto-negotiation on both

<details>
<summary>Answer and explanation</summary>

**Answer: D.** The NIC is hard set to full duplex, so it does not negotiate, and the switch falls back to half duplex. Late collisions on the half-duplex side are the classic sign of a duplex mismatch. Matching settings on both ends fixes it. (Objective 2.2)

- **A** is wrong because the cable tested good, and a new cable does not change the duplex settings.
- **B** is wrong because the VLAN does not affect duplex or collisions.
- **C** is wrong because jumbo frames change the MTU, not the duplex, and would not fix collisions.

</details>

### Item 6

A small warehouse has three APs on 2.4 GHz using 20 MHz channels, and their coverage areas overlap. Which channel plan avoids co-channel and adjacent-channel interference in North America?

A. 1, 6 and 11

B. 1, 2 and 3

C. 6 on all three APs

D. 3, 7 and 9

<details>
<summary>Answer and explanation</summary>

**Answer: A.** Channels 1, 6 and 11 are the only three 20 MHz channels in the North American 2.4 GHz band that do not overlap. (Objective 2.3)

- **B** is wrong because adjacent channels overlap heavily.
- **C** is wrong because putting every AP on one channel makes them share airtime and interfere.
- **D** is wrong because 3 and 7 overlap, and so do 7 and 9. Non-overlapping 20 MHz channels need at least five channels of separation.

</details>

### Item 7

A hotel wants Wi-Fi for visitors that shows a terms-of-use page, gives internet access only, and keeps guests away from the hotel's booking systems. Which two settings meet these goals? Choose two.

A. Put guests on the staff SSID with a shared passphrase printed at reception

B. A captive portal that guests must accept before access

C. WEP encryption for compatibility with older devices

D. Disable client isolation so guests can share files

E. A separate guest SSID mapped to an isolated VLAN with internet-only access

<details>
<summary>Answer and explanation</summary>

**Answer: B and E.** The captive portal presents the terms page. A separate SSID on an isolated VLAN with internet-only rules keeps guests off internal systems. (Objective 2.3)

- **A** is wrong because the staff network would put guests next to the booking systems.
- **C** is wrong because WEP is broken and has no place in a new deployment.
- **D** is wrong because guests should not reach each other, so client isolation should stay on.

</details>

### Item 8

A company needs a wireless link between two buildings 400 m apart with clear line of sight. Running fiber is not allowed. Which antenna choice fits BEST?

A. Omnidirectional antennas on both roofs

B. Higher transmit power on the indoor ceiling APs

C. Directional antennas on each building, aimed at each other

D. Band steering on the existing APs

<details>
<summary>Answer and explanation</summary>

**Answer: C.** A point to point link needs directional antennas (such as Yagi or dish) that focus the signal toward the other end, giving range and less interference. (Objective 2.3)

- **A** is wrong because omnidirectional antennas spread energy in all directions, wasting it and picking up more interference.
- **B** is wrong because indoor APs are not designed or aimed for a 400 m outdoor link, and more power also raises interference.
- **D** is wrong because band steering moves clients between bands; it does not extend range.

</details>

### Item 9

A data center uses hot and cold aisles, with server fronts facing the cold aisle. New top-of-rack switches are mounted so their ports face the rear, toward the servers' network ports and the hot aisle. The switches start logging high temperature alarms. What is the MOST likely cause and fix?

A. The PDU is overloaded; add a second UPS

B. The switches are port-side intake models; replace them with port-side exhaust models

C. The humidity is too low; add a humidifier

D. The rack is not lockable; replace it with a lockable cabinet

<details>
<summary>Answer and explanation</summary>

**Answer: B.** With ports facing the hot aisle, a port-side intake switch draws in hot exhaust air. A port-side exhaust model pulls cold air from the cold aisle side and blows it out toward the hot aisle. (Objective 2.4)

- **A** is wrong because an overloaded PDU trips or alarms on power, not on switch temperature.
- **C** is wrong because low humidity raises static risk; it does not overheat switches.
- **D** is wrong because locking controls access and has no effect on airflow.

</details>

### Item 10

A technician must add a new switch to a rack that already holds several servers on a metered PDU fed by one circuit. What should the technician check FIRST?

A. Whether the total power load with the new switch stays within the PDU and circuit ratings

B. Whether the switch supports jumbo frames

C. Which VLANs the switch will trunk

D. Whether the server room uses a clean agent fire suppression system

<details>
<summary>Answer and explanation</summary>

**Answer: A.** Before plugging anything in, confirm the added draw will not overload the PDU or circuit. An overload trips the breaker and takes down every device in the rack. (Objective 2.4)

- **B** is wrong because jumbo frame support is a configuration detail that can be checked later.
- **C** is wrong because VLAN planning comes after the switch is safely installed and powered.
- **D** is wrong because fire suppression is a room design factor that does not change with one switch.

</details>

## Score yourself

| Item | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| Answer | C | A | D | B | D | A | B, E | C | B | A |
| Objective | 2.1 | 2.1 | 2.1 | 2.2 | 2.2 | 2.3 | 2.3 | 2.3 | 2.4 | 2.4 |

- **9 to 10:** strong. Keep the route selection order and the wireless tables fresh with mixed practice.
- **7 to 8:** list the objectives you missed. Route selection and switch port problems are the usual weak spots; reread [2.1 Routing technologies](#21-routing-technologies) and [2.2 Switching technologies](#22-switching-technologies).
- **6 or fewer:** spend two study sessions here. Write the route selection order and the administrative distance table from memory, draw a trunk with native and voice VLANs on paper, then reread [2.3 Wireless devices and technologies](#23-wireless-devices-and-technologies) and [2.4 Physical installations](#24-physical-installations) before redoing the items.

## Next step

1. Redo any item you missed without opening the explanation, and write the trap you fell for in one line.
2. Run `route print` on Windows (or `ip route` on Linux), find the default route and the most specific route for your own subnet, and explain which one a packet to a local printer and a packet to a website would use. If you have a free network simulator, build two VLANs with a trunk and route between them with an SVI.
3. Take a timed run with the [free Network+ (N10-009) practice test on PrepClubs](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus) to practise pacing across the whole exam.

---

<!-- nav:bottom -->← Previous: [Domain 1: Networking Concepts](1-networking-concepts.md) | Next: [Domain 3: Network Operations](3-network-operations.md) → | [Back to contents](../../README.md#contents)<!-- /nav:bottom -->
