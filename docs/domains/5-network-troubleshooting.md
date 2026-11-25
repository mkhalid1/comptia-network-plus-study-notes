<!-- nav:top -->
[Contents](../../README.md#contents) · Previous: [Domain 4: Network Security](4-network-security.md) · Next: [Performance-based questions and pacing](../performance-based-questions.md)
<!-- /nav:top -->

# Domain 5: Network Troubleshooting

**Weight:** 24% of the exam (CompTIA's figure), the heaviest domain. By our estimate that is roughly 21 of up to 90 questions. **Objectives:** 5.1 to 5.5.

This is where the exam checks whether you can fix things, not just name them. You get symptoms, command output and interface counters, and you have to find the cause, pick the right tool, or say what to do next in CompTIA's troubleshooting order. Candidates lose marks here by jumping to a fix before reading the evidence, and by guessing the methodology order instead of knowing it.

## What this domain measures

Objective 5.1 is the method. Objectives 5.2 to 5.4 are symptom-to-cause scenarios at the physical, network service and performance levels. Objective 5.5 is choosing and reading tools.

| Objective | What it asks you to do | Topics you must know |
|---|---|---|
| 5.1 | Explain the troubleshooting methodology | The seven steps in order, with their sub-steps |
| 5.2 | Troubleshoot cabling and physical interface issues | Wrong cable type (single mode, multimode, Cat 5 to 8, STP and UTP), crosstalk, interference, attenuation, bad termination, TX and RX swapped, CRC errors, runts, giants, drops, err-disabled, administratively down, suspended, PoE budget and standard, transceiver mismatch and signal strength |
| 5.3 | Troubleshoot network service issues | STP loops, root bridge, port roles and states, wrong VLAN, ACLs, routing table, default routes, address pool exhaustion, wrong gateway, wrong or duplicate IP, wrong subnet mask |
| 5.4 | Troubleshoot performance issues | Congestion, bottlenecks, bandwidth and throughput, latency, packet loss, jitter, wireless interference and channel overlap, signal loss, coverage, client disassociation, roaming |
| 5.5 | Use the right tool or protocol | Protocol analyzer, ping, traceroute, nslookup, tcpdump, dig, netstat, ip, ifconfig, ipconfig, arp, Nmap, LLDP and CDP, speed tester, toner, cable tester, taps, Wi-Fi analyzer, visual fault locator, show commands |

## Key concepts explained

### 5.1 Troubleshooting methodology

CompTIA's methodology has seven steps. Learn them in this order, because "What should the technician do NEXT?" items test the order directly.

1. **Identify the problem.**
   - Gather information.
   - Question users.
   - Identify symptoms.
   - Determine if anything has changed.
   - Duplicate the problem, if possible.
   - Approach multiple problems individually.
2. **Establish a theory of probable cause.**
   - Question the obvious (is it plugged in, is the port enabled).
   - Consider multiple approaches: top-to-bottom or bottom-to-top through the OSI model, or divide and conquer (start in the middle, such as with a ping, and work toward the half that fails).
3. **Test the theory to determine the cause.**
   - If the theory is confirmed, determine the next steps to resolve the problem.
   - If the theory is not confirmed, establish a new theory or escalate.
4. **Establish a plan of action to resolve the problem and identify potential effects.** Think about what the fix might break, and whether it needs a change request or a maintenance window.
5. **Implement the solution or escalate as necessary.**
6. **Verify full system functionality and implement preventive measures if applicable.** Confirm the user can work, not just that a light is green, and stop it happening again.
7. **Document findings, actions, outcomes, and lessons learned throughout the process.** Documentation is listed last but happens all the way through.

**Which OSI approach when.** Bottom-to-top (start at the cable) suits "no link at all" problems. Top-to-bottom (start at the application) suits "one app fails but the network seems fine". Divide and conquer suits "it worked yesterday and I have no idea where it breaks": a ping to the gateway quickly tells you which half of the stack to look at.

**Common NEXT patterns.**

| Where the technician is | What comes next |
|---|---|
| Has questioned the user and reproduced the fault | Establish a theory of probable cause |
| Tested a theory and it was wrong | Establish a new theory or escalate |
| Confirmed the cause | Establish a plan of action and identify potential effects |
| Applied the fix | Verify full system functionality and add preventive measures |
| Everything works again | Document findings, actions, outcomes and lessons learned |

### 5.2 Cabling and physical interface issues

**Choosing the right cable.**

| Cable | Notes |
|---|---|
| Single-mode fiber (SMF) | Small core (about 9 microns), laser light, long distances measured in kilometers |
| Multimode fiber (MMF) | Larger core (50 or 62.5 microns), shorter distances measured in hundreds of meters |
| Cat 5 | 100 Mbps. Cat 5e supports 1 Gbps to 100 m |
| Cat 6 | 1 Gbps to 100 m, 10 Gbps over shorter runs (about 55 m). Cat 6a supports 10 Gbps to 100 m |
| Cat 7 | 10 Gbps to 100 m, shielded pairs |
| Cat 8 | 25 or 40 Gbps over short runs (up to 30 m), shielded, for data center links |
| UTP vs STP | Unshielded twisted pair is cheaper and common. Shielded twisted pair resists electromagnetic interference near motors, lights and power cables |

**Signal problems.**

- **Crosstalk:** signal from one pair bleeds into another, often from untwisting too much wire at the connector.
- **Interference (EMI):** outside electrical noise from motors, fluorescent lights or power lines.
- **Attenuation:** the signal weakens with distance. Copper Ethernet runs over 100 m and fiber runs beyond the optic's rating suffer it.
- **Improper termination:** wrong pinout, loose crimp, or bad punch-down.
- **TX/RX transposed:** on fiber, the transmit strand must reach the far end's receive. Swapped strands give no link. Fix by swapping the pair at one end.

**Interface counters** (seen with `show interface`). Rising counters matter more than old totals, so clear them and watch.

| Counter | Meaning | Likely causes |
|---|---|---|
| CRC errors | Frames arrived with a bad checksum | Damaged cable, EMI, bad connector, duplex mismatch |
| Runts | Frames smaller than 64 bytes | Collisions, usually from a duplex mismatch, or a faulty NIC |
| Giants | Frames larger than the maximum (1518 bytes standard) | MTU or jumbo frame mismatch between devices |
| Drops | Frames discarded, usually because buffers were full | Congestion or an oversubscribed link |

**Port status.**

| Status | Meaning | Fix |
|---|---|---|
| Error disabled (err-disabled) | The switch shut the port because a protection feature tripped, such as a port security violation or BPDU guard | Remove the cause, then bounce the port (shutdown, then no shutdown) or let errdisable recovery do it |
| Administratively down | Someone configured `shutdown` on the port | Enable it with `no shutdown`, if it should be in use |
| Suspended | Commonly a link aggregation (port channel) member whose settings do not match the bundle or the far end | Make speed, duplex, VLAN and LACP settings match |

**Power over Ethernet.** 802.3af (PoE) supplies up to 15.4 W per port, 802.3at (PoE+) up to 30 W, and 802.3bt (PoE++) more again for devices such as PTZ cameras and high-power APs. Two classic faults:

- **Power budget exceeded:** the switch has a total PoE budget. When it runs out, new devices are denied power even though each port could supply it.
- **Incorrect standard:** a device needs PoE+ or PoE++ but the switch or injector only supplies 802.3af, so it does not boot or runs with features disabled.

**Transceivers.** A **mismatch** means the optics at each end do not match (single mode at one end and multimode at the other, or different wavelengths or speeds), so there is no link. **Signal strength** is read in dBm from the optic's diagnostics. Too low suggests a dirty or damaged connector, a bad splice, or a run that is too long. Too high on a short run with a long-range optic can overload the receiver, which an attenuator fixes.

**Symptom to cause, physical layer.**

| Symptom | Most likely cause |
|---|---|
| No link light on a new copper run | Bad termination, broken cable, or port administratively down |
| No link on a new fiber run, both ends lit | TX and RX strands transposed, or transceiver mismatch |
| Link up, CRC errors climbing steadily | Damaged cable, bad connector or EMI |
| Late collisions and runts, one side half duplex | Duplex mismatch |
| Giants counter climbing after enabling jumbo frames on one switch | MTU mismatch |
| Link works at 100 Mbps but not 1 Gbps | Cable below spec (Cat 5) or a broken pair |
| Errors only when the factory machines run | EMI; use STP or fiber, reroute the cable |
| Port shows err-disabled after a user plugged in a small switch | Port security violation or BPDU guard |
| New PoE phones fail to boot, older ones fine | PoE budget exceeded |
| PTZ camera powers on but tilt and heaters fail | Incorrect PoE standard (needs more power) |
| Optic reports very low receive power | Dirty connector, bad splice or excessive distance |

### 5.3 Network service issues

**Spanning Tree Protocol (STP).** STP stops Layer 2 loops by blocking redundant paths.

- **Network loops:** without STP, a loop causes a broadcast storm, MAC address flapping and a network that grinds to a halt.
- **Root bridge selection:** the switch with the lowest bridge ID wins. The bridge ID is the priority (default 32768) plus the MAC address, so if priorities are equal, the oldest switch with the lowest MAC can become root by accident. Set the priority on the switch you want as root.
- **Port roles:** **root port** (each non-root switch's best path to the root), **designated port** (the forwarding port on each segment), and **blocking** or **alternate** ports (backups that do not forward). RSTP adds a **backup** role.
- **Port states:** classic STP uses blocking, listening, learning, forwarding and disabled. RSTP simplifies to discarding, learning and forwarding. A new port that waits around 30 seconds before passing traffic is moving through listening and learning.

**Other switching issues.**

- **Incorrect VLAN assignment:** the port is in the wrong VLAN, so the host gets an address in the wrong subnet or none at all, and cannot reach its resources.
- **ACLs:** a rule blocks traffic that should be allowed. Remember the top-down order and the implicit deny.

**Route selection.** A router chooses the most specific match in its **routing table** (longest prefix wins). If nothing matches, it uses the **default route** (0.0.0.0/0). No default route means traffic to unknown networks is dropped.

**Addressing problems.**

| Problem | Symptom |
|---|---|
| Address pool exhaustion | New clients get APIPA addresses (169.254.x.x) while existing clients work |
| Incorrect default gateway | Host reaches local hosts but nothing on other subnets or the internet |
| Incorrect IP address | Host in the wrong subnet cannot reach the gateway |
| Duplicate IP address | Intermittent connectivity for two hosts, and an address conflict warning |
| Incorrect subnet mask | Host can reach some local or remote hosts but not others, because it misjudges which addresses are local |

**Worked example: wrong subnet mask.** A host has 192.168.1.50 with mask 255.255.0.0, while the real network is 192.168.1.0/24 with gateway 192.168.1.1. The host thinks everything in 192.168.0.0/16 is local. When it sends to 192.168.2.10 (a different subnet), it ARPs for it directly instead of using the gateway, and the traffic fails. Internet traffic still works because it goes to the gateway.

**Symptom to cause, network services.**

| Symptom | Most likely cause |
|---|---|
| Whole network slows to a crawl after a cable is added between two switches, switch LEDs flash constantly | Switching loop, STP disabled or misconfigured |
| Traffic takes a slow path through an access switch after it was added | The new switch became root bridge (lowest bridge ID) |
| One PC gets an address from the wrong subnet | Incorrect VLAN assignment on its port |
| Users reach the web server by ping but not on HTTPS | ACL blocks TCP 443 |
| Users can reach internal subnets but not the internet | Missing default route on the router, or wrong gateway on hosts |
| New guest devices get 169.254 addresses in the afternoon | DHCP address pool exhausted |
| One host reaches local devices only | Incorrect default gateway |
| Two users drop off the network at random | Duplicate IP address |

### 5.4 Performance issues

**Terms.**

| Term | Meaning |
|---|---|
| Congestion or contention | More traffic wants a link or medium than it can carry, so frames queue and drop |
| Bottlenecking | One slow point limits the whole path, such as a 1 Gbps uplink serving many 1 Gbps clients |
| Bandwidth | The theoretical capacity of a link |
| Throughput capacity | What you actually get after overhead, errors and contention |
| Latency | Delay from one end to the other, measured in milliseconds |
| Packet loss | Packets that never arrive |
| Jitter | Variation in latency between packets. Voice and video suffer most |

**Wireless performance.**

- **Interference:** from microwaves, Bluetooth, cordless phones and neighboring networks. **Channel overlap** is the common one: in 2.4 GHz, only channels 1, 6 and 11 do not overlap in North America. Neighboring APs on channels 1 and 3 interfere with each other.
- **Signal degradation or loss:** walls, metal, glass, water and distance weaken the signal.
- **Insufficient wireless coverage:** dead spots where clients cannot connect, found with a site survey.
- **Client disassociation issues:** clients drop off the AP, from weak signal, interference, too many clients, or deauthentication attacks.
- **Roaming misconfiguration:** APs in one area must share the same SSID and security settings, with suitable overlap, or clients lose their session moving between them. Clients that cling to a distant AP (sticky clients) also cause poor performance.

**Symptom to cause, performance.**

| Symptom | Most likely cause |
|---|---|
| Voice calls choppy, ping shows no loss but times vary widely | Jitter |
| Speed test shows far less than the link speed during the backup window | Congestion |
| Every remote site is slow, and the head office internet link is at 100% | Bottleneck at the shared link |
| Satellite link: everything works but feels slow, ping times around 600 ms | Latency |
| Ping shows several timeouts out of every 100 | Packet loss |
| Wi-Fi slow in a building with APs on channels 1, 2, 6 and 7 | Channel overlap |
| Laptops drop Wi-Fi in the warehouse aisles with metal shelving | Signal degradation, insufficient coverage |
| Calls drop when users walk between floors | Roaming misconfiguration |

### 5.5 Tools and protocols

**Software tools.**

| Tool | What it does | Use it when |
|---|---|---|
| Protocol analyzer (for example Wireshark) | Captures and decodes packets with a GUI | You need to see exactly what is on the wire |
| `ping` | Sends ICMP echo requests and shows replies and times | You test basic reachability, loss and latency |
| `traceroute` (Linux, macOS) / `tracert` (Windows) | Lists each router hop to a destination | You find where traffic stops or slows |
| `nslookup` | Queries DNS servers | You check name resolution, on Windows or elsewhere |
| `dig` | Queries DNS with detailed output | You check records, TTLs and authoritative answers, mostly on Linux |
| `tcpdump` | Command-line packet capture | You capture on a Linux server or device without a GUI |
| `netstat` | Shows connections, listening ports and statistics | You check what a host is connected to or listening on |
| `ip` / `ifconfig` / `ipconfig` | Show and set interface settings. `ip` is current on Linux, `ifconfig` older Linux and macOS, `ipconfig` on Windows (`/all`, `/release`, `/renew`, `/flushdns`) | You check a host's address, mask, gateway and DNS |
| `arp` | Shows and edits the ARP cache (`arp -a`) | You check IP to MAC mappings, duplicates or spoofing |
| Nmap | Scans hosts and ports, and can detect services and operating systems | You find what is on a subnet or which ports are open |
| LLDP / CDP | Neighbor discovery protocols. LLDP is the open standard, CDP is Cisco's | You find which switch and port a device is plugged into |
| Speed tester | Measures real throughput to a test server | You compare real throughput to the paid bandwidth |

**Hardware tools.**

| Tool | What it does | Use it when |
|---|---|---|
| Toner (tone generator and probe) | Puts a tone on a cable that the probe hears at the other end | You trace an unlabeled cable to a patch panel port |
| Cable tester | Checks continuity, pinout and pairs, and some certify performance | You check a new or suspect copper run |
| Tap | Hardware device inline on a link that copies all traffic to a monitor port | You need a complete, reliable copy of traffic, including errors, without relying on the switch |
| Wi-Fi analyzer | Shows SSIDs, channels, signal strength and noise | You plan channels or find interference and dead spots |
| Visual fault locator | Shines visible red laser light down a fiber; breaks and tight bends glow | You find a break or check fiber continuity |

**Basic networking device commands.** The objectives list short names. The Cisco IOS form is shown next to each.

| Objective name | Cisco form | Shows |
|---|---|---|
| show mac-address-table | `show mac address-table` | Which MAC addresses were learned on which port and VLAN |
| show route | `show ip route` | The routing table, including the default route |
| show interface | `show interfaces` | Status, speed, duplex and error counters |
| show config | `show running-config` (current) and `show startup-config` (saved) | The device configuration |
| show arp | `show arp` or `show ip arp` | IP to MAC mappings the device knows |
| show vlan | `show vlan brief` | VLANs and which ports belong to each |
| show power | `show power inline` | PoE budget, power used per port and port power state |

**Reading `tracert` output.**

```
C:\>tracert 198.51.100.25

Tracing route to 198.51.100.25 over a maximum of 30 hops

  1     1 ms     1 ms     1 ms  10.0.0.1
  2     9 ms     8 ms     9 ms  203.0.113.1
  3    11 ms    10 ms    12 ms  203.0.113.65
  4     *        *        *     Request timed out.
  5     *        *        *     Request timed out.
```

The path works as far as hop 3 in the provider network, then stops. The problem lies at or beyond hop 3's next hop, not on the local LAN. (Some routers do not answer traceroute, so one silent hop in the middle of a working path is normal. A path that never completes is not.)

## How questions look

This domain is heavy on exhibits. Expect `ipconfig`, `ping`, `tracert`, `arp -a`, `show interface` and `show power inline` output, then "What is the MOST likely cause?" Methodology items ask what to do NEXT or FIRST at a given point. Tool items describe a task (trace an unlabeled cable, find a fiber break, find which switch port a device is on) and ask for the tool or command. Performance-based items may give you a small network with several faults to find, a set of commands to run, or the seven methodology steps to put in order.

## A method for these questions

1. If the stem says FIRST or NEXT, place the technician on the seven-step methodology before reading the options.
2. Read every number in an exhibit: addresses, masks, gateways, counters, times. The fault is almost always one value that does not fit.
3. Decide the layer: no link or errors (physical), wrong address or path (network services), slow or choppy (performance).
4. Ask what still works. Local but not remote points to the gateway or routing. Some local hosts but not others points to the mask. New clients only points to DHCP.
5. For counters, match the counter to its cause: CRC to cabling or EMI, runts to collisions, giants to MTU, drops to congestion.
6. For tool items, match the tool to the medium and task: copper (toner, cable tester), fiber (visual fault locator), wireless (Wi-Fi analyzer), packets (protocol analyzer, tcpdump, tap).
7. Choose the least drastic fix that addresses the cause, and remember to verify afterwards.

## Traps that cost points

- **Fixing before testing.** Jumping from a theory straight to a fix skips "test the theory". If the item has a test option, that is usually the answer.
- **Documentation only at the end.** It is step 7, but you document throughout. If an option says "document" right after the user calls, it is not the NEXT step.
- **Escalating too early.** Escalation is an option when a theory fails or a fix is out of your scope, not the first move.
- **APIPA blamed on DNS.** A 169.254 address means no DHCP lease. DNS settings cannot help a host with no valid address.
- **Bandwidth vs throughput.** Bandwidth is the rated capacity; throughput is what you measure. A speed test measures throughput.
- **Jitter vs latency.** Consistently high times are latency. Times that swing up and down are jitter.
- **Administratively down vs err-disabled.** Admin down was configured by a person. Err-disabled was triggered by the switch, so fix the cause first or it will trip again.
- **Toner for fiber.** Toners work on copper. Fiber breaks are found with a visual fault locator (or an OTDR for long runs).
- **Overlapping 2.4 GHz channels.** Only 1, 6 and 11 are non-overlapping in North America. Channels 1 and 3 interfere.

## Timing

The exam is one 90-minute block, so aim for about a minute per multiple-choice item on average and allow extra for PBQs. Exhibit items in this domain take longer to read, so do not burn time on recall questions elsewhere. If a troubleshooting PBQ has several devices, flag it, answer the quick items, then come back with a fresh head: these PBQs carry a lot of this domain's content.

## Practice items

These 4 items are original, written for these notes. They are not taken from CompTIA's exam or any prep provider.

### Item 1

A technician suspects a faulty patch cable and swaps it, but the workstation still has no connectivity. According to the troubleshooting methodology, what should the technician do now?

A. Establish a new theory or escalate

B. Verify full system functionality

C. Document lessons learned and close the ticket

D. Identify potential effects of the original fix

<details>
<summary>Answer and explanation</summary>

**Answer: A.** Testing the theory did not confirm it, so the methodology says to establish a new theory or escalate. (Objective 5.1)

- **B** is wrong because there is no working fix to verify.
- **C** is wrong because the problem is not solved.
- **D** is wrong because identifying potential effects belongs to planning a fix for a confirmed cause.

</details>

### Item 2

Users on one floor report slow file transfers. The technician checks the uplink port twice, five minutes apart:

```
GigabitEthernet1/0/48 is up, line protocol is up
  Hardware is Gigabit Ethernet, address is 00a1.b2c3.d430
  Full-duplex, 1000Mb/s, media type is 10/100/1000BaseTX
  5 minute input rate 41200000 bits/sec, 5120 packets/sec
  5 minute output rate 39800000 bits/sec, 4980 packets/sec
     0 runts, 0 giants, 0 throttles
     1876 input errors, 1876 CRC, 0 frame, 0 overrun, 0 ignored
     0 output errors, 0 collisions, 0 interface resets
     0 output drops

(five minutes later)
     0 runts, 0 giants, 0 throttles
     3412 input errors, 3412 CRC, 0 frame, 0 overrun, 0 ignored
     0 output errors, 0 collisions, 0 interface resets
     0 output drops
```

The far end is also set to full duplex at 1000 Mbps. What is the MOST likely cause?

A. The link is congested

B. An MTU mismatch between the switches

C. The switch's PoE budget is exceeded

D. A damaged cable or a source of interference along the run

<details>
<summary>Answer and explanation</summary>

**Answer: D.** CRC errors are climbing while there are no runts, collisions, giants or drops, and both ends agree on full duplex. That points to the physical medium: a damaged cable, bad connector or EMI. (Objective 5.2)

- **A** is wrong because congestion shows as drops, and the load is about 40 Mbps on a 1 Gbps link with zero drops.
- **B** is wrong because an MTU mismatch shows as giants, which are zero.
- **C** is wrong because PoE budget problems deny power to devices; they do not cause CRC errors on an uplink.

</details>

### Item 3

A user can print to a printer on the same subnet but cannot reach any internal websites on other subnets or the internet. The technician runs this:

```
C:\>ipconfig

Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . : corp.example.com
   IPv4 Address. . . . . . . . . . . : 192.168.20.45
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.2.1
```

The router's interface for this subnet is 192.168.20.1. What is the problem?

A. Incorrect default gateway

B. Duplicate IP address

C. DHCP address pool exhaustion

D. Incorrect subnet mask

<details>
<summary>Answer and explanation</summary>

**Answer: A.** The gateway 192.168.2.1 is not in the host's subnet (192.168.20.0/24), so off-subnet traffic has nowhere to go. Local printing still works. (Objective 5.3)

- **B** is wrong because a duplicate address causes intermittent failures for local traffic too, and the exhibit shows no conflict.
- **C** is wrong because the host has a valid address, not a 169.254 APIPA address.
- **D** is wrong because 255.255.255.0 is correct for this /24 subnet.

</details>

### Item 4

Users complain that VoIP calls to the head office break up, although web browsing feels fine. A technician pings the head office voice gateway:

```
C:\>ping -n 8 10.20.0.5

Reply from 10.20.0.5: bytes=32 time=14ms TTL=62
Reply from 10.20.0.5: bytes=32 time=96ms TTL=62
Reply from 10.20.0.5: bytes=32 time=12ms TTL=62
Reply from 10.20.0.5: bytes=32 time=141ms TTL=62
Reply from 10.20.0.5: bytes=32 time=19ms TTL=62
Reply from 10.20.0.5: bytes=32 time=88ms TTL=62
Reply from 10.20.0.5: bytes=32 time=15ms TTL=62
Reply from 10.20.0.5: bytes=32 time=122ms TTL=62

Ping statistics for 10.20.0.5:
    Packets: Sent = 8, Received = 8, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 12ms, Maximum = 141ms, Average = 63ms
```

Which issue does the output show?

A. Packet loss

B. Attenuation

C. A routing loop

D. Jitter

<details>
<summary>Answer and explanation</summary>

**Answer: D.** No packets are lost, but round-trip times swing between 12 ms and 141 ms. That variation is jitter, which damages real-time voice while barely affecting browsing. (Objective 5.4)

- **A** is wrong because all 8 replies arrived.
- **B** is wrong because attenuation is signal loss on a cable and would show as errors or loss, not variable delay alone.
- **C** is wrong because a routing loop makes packets expire (TTL exceeded) and fail; here every reply arrives with a steady TTL.

</details>

Want more? The free timed Network+ (N10-009) practice test on PrepClubs has 25 questions, with a domain-by-domain breakdown: [take the free test](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus).

## Score yourself

| Item | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| Answer | A | D | A | D |
| Objective | 5.1 | 5.2 | 5.3 | 5.4 |

- **4:** strong. Keep practicing with fresh command output so you read exhibits quickly under time pressure.
- **3:** list the objectives you missed. Counter meanings and the methodology order are the usual gaps; reread [5.1 Troubleshooting methodology](#51-troubleshooting-methodology) and [5.2 Cabling and physical interface issues](#52-cabling-and-physical-interface-issues).
- **2 or fewer:** this domain carries the most weight, so give it two study sessions. Write the seven steps from memory, rebuild the three symptom-to-cause tables without looking, then reread [5.3 Network service issues](#53-network-service-issues) and [5.5 Tools and protocols](#55-tools-and-protocols) and redo the items.

## Next step

1. Redo any item you missed without opening the explanation, and write the trap you fell for in one line.
2. On your own computer, run `ipconfig /all` (or `ip addr` and `ip route` on Linux), then `ping` your gateway, `tracert` (or `traceroute`) to a public site, and `arp -a`. Explain every line of output, and say which methodology step each command would support.
3. Take a timed run with the [free Network+ (N10-009) practice test on PrepClubs](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus) to practice pacing across the whole exam.

---

<!-- nav:bottom -->
← Previous: [Domain 4: Network Security](4-network-security.md) | Next: [Performance-based questions and pacing](../performance-based-questions.md) → | [Back to contents](../../README.md#contents)
<!-- /nav:bottom -->
