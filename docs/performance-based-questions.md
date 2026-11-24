<!-- nav:top -->[Contents](../README.md#contents) · Previous: [Domain 5: Network Troubleshooting](domains/5-network-troubleshooting.md) · Next: [How Network+ scoring works](scoring.md)<!-- /nav:top -->

# Performance-based questions and pacing

Network+ mixes multiple-choice questions with performance-based questions (PBQs). PBQs are where many candidates lose time, not because they are impossible but because people often meet them at the start of the exam and spend too long on them.

## What PBQs are

A PBQ asks you to do something rather than pick a letter. In our own words, the common shapes on a networking exam are:

- **Subnetting tables:** given an address and prefix, fill in the network ID, broadcast address, usable host range or subnet mask, or pick a subnet that fits a number of hosts.
- **Drag and drop or match:** pair ports with protocols, cable types with connectors, or DNS record types with their purpose.
- **Order the steps:** put the troubleshooting methodology in sequence, or the steps of a change request.
- **Configure:** set switch ports to the right VLAN or trunk, choose wireless settings for a site, or pick firewall and ACL rules.
- **Analyze:** read command output such as `ipconfig`, `traceroute`, `show interface` or a routing table and decide what is wrong.
- **Place on a diagram:** decide where an access point, firewall, load balancer or patch panel belongs.

CompTIA lists Network+ as a mix of multiple-choice and performance-based questions. It does not publish how many PBQs appear on a given exam or how they are weighted in the score, so treat any number you read online as hearsay.

## A pacing plan for 90 minutes

| Time on the clock | Where you should be |
|---|---|
| 0 to 2 minutes | Read the instructions. If the first questions are PBQs, read each one, make a quick attempt only if it is obviously short, and flag it |
| By 30 minutes | Roughly a third of the multiple-choice questions done |
| By 60 minutes | Roughly two thirds of the multiple-choice questions done |
| By 75 minutes | All multiple-choice questions answered, including guesses on the hard ones |
| 75 to 88 minutes | Return to the flagged PBQs and finish them |
| Last 2 minutes | Make sure nothing is left blank |

The idea is simple. A PBQ might be worth more than one multiple-choice item, but nobody outside CompTIA knows how much more. Fifteen quick multiple-choice questions you never reached because you were stuck on one PBQ are a certain loss. So bank the quick points first, then give PBQs the time that is left. Some candidates prefer to do PBQs first while fresh; that works if you set yourself a hard limit, such as 4 to 5 minutes each, and move on when it is up.

On average across the exam, you have about one minute per question. Short recall items, such as a port number or a connector name, take 30 to 45 seconds, which buys time for PBQs.

## How to approach a PBQ

1. **Read the whole task first,** including every tab, device or panel, before you click anything.
2. **Write down the goal in one line** on your notepad: "Find the subnet for 50 hosts" or "Order the troubleshooting steps".
3. **Do the parts you are sure of first.** Partial progress on a table or a matching task is better than a blank screen.
4. **For subnetting, write the block-size table once** (for example /25 = 128, /26 = 64, /27 = 32, /28 = 16, /29 = 8, /30 = 4) and use it for every row instead of working each one out from scratch.
5. **Check constraints** the task gives you, such as "fewest wasted addresses" or "the printer must keep its address", against each choice.
6. **Look for a reset button** before experimenting, and note what the original state was.
7. **Stop at your time limit,** make your best choices, and move on.

## Practice task 1: complete the subnetting table

For each host address, fill in the subnet mask, network ID, broadcast address, usable host range and number of usable hosts.

| Host address | Mask | Network ID | Broadcast | Usable range | Usable hosts |
|---|---|---|---|---|---|
| 172.16.45.130/26 | ? | ? | ? | ? | ? |
| 10.20.30.77/29 | ? | ? | ? | ? | ? |
| 192.168.5.200/27 | ? | ? | ? | ? | ? |

<details>
<summary>Answer</summary>

| Host address | Mask | Network ID | Broadcast | Usable range | Usable hosts |
|---|---|---|---|---|---|
| 172.16.45.130/26 | 255.255.255.192 | 172.16.45.128 | 172.16.45.191 | .129 to .190 | 62 |
| 10.20.30.77/29 | 255.255.255.248 | 10.20.30.72 | 10.20.30.79 | .73 to .78 | 6 |
| 192.168.5.200/27 | 255.255.255.224 | 192.168.5.192 | 192.168.5.223 | .193 to .222 | 30 |

The method: find the block size in the last octet (256 minus the mask value: 64 for /26, 8 for /29, 32 for /27). The network ID is the largest multiple of the block size that is not above the host's value (128, 72, 192). The broadcast is the next block's start minus one. Usable hosts are 2 to the power of the host bits, minus 2 for the network ID and broadcast. See [Domain 1](domains/1-networking-concepts.md#key-concepts-explained).

</details>

## Practice task 2: order the troubleshooting steps

Users on one floor report they cannot reach the file server. Put these actions in the order CompTIA's troubleshooting methodology uses:

- Replace the faulty patch cable you suspect and see if the link comes up
- Write up what you found, what you changed and what you learned
- Ask the users when it started and check whether anything changed on that floor
- Decide that a damaged patch cable to the floor's switch is the most likely cause
- Confirm every user on the floor can reach the server, and label the cable run to prevent a repeat
- Plan the cable swap for a quiet moment and note which users will briefly lose connection
- Make the change: install the new cable permanently in the patch panel

<details>
<summary>Answer</summary>

1. Ask the users when it started and check whether anything changed on that floor (identify the problem)
2. Decide that a damaged patch cable to the floor's switch is the most likely cause (establish a theory of probable cause)
3. Replace the faulty patch cable you suspect and see if the link comes up (test the theory to determine the cause)
4. Plan the cable swap for a quiet moment and note which users will briefly lose connection (establish a plan of action and identify potential effects)
5. Make the change: install the new cable permanently in the patch panel (implement the solution or escalate as necessary)
6. Confirm every user on the floor can reach the server, and label the cable run to prevent a repeat (verify full system functionality and implement preventive measures)
7. Write up what you found, what you changed and what you learned (document findings, actions, outcomes and lessons learned)

The objectives say documentation happens throughout the process, but in an ordering task it goes last. If the test in step 3 had failed, the method sends you back to a new theory or to escalation. See [Domain 5](domains/5-network-troubleshooting.md#key-concepts-explained).

</details>

## Practice under time

PBQ skill improves fastest with timed practice. Work through the domain items in this repo, then take the [free Network+ (N10-009) practice test on PrepClubs](https://prepclubs.com/free-practice-tests/network-plus?utm_source=github&utm_medium=referral&utm_campaign=study-notes&utm_content=network-plus) and check your pace against the plan above.

---

<!-- nav:bottom -->← Previous: [Domain 5: Network Troubleshooting](domains/5-network-troubleshooting.md) | Next: [How Network+ scoring works](scoring.md) → | [Back to contents](../README.md#contents)<!-- /nav:bottom -->
