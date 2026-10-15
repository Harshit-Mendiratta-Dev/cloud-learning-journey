# Day 60 - Revision: Scenario Practice (Days 48-59)
**Date:** 2026-10-15
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**Revisiting the scenario practice from Days 48-59 from memory, checking each decision point:**

| Day | Scenario | First test that decides it | Diagnosis |
|---|---|---|---|
| 48 | `169.254` address | Own IP, then second device | DHCP failed |
| 49 | `1.1.1.1` times out | Gateway ping passes, `1.1.1.1` fails | Router-to-ISP link |
| 50 | `dig` fails, `1.1.1.1` works | `dig @1.1.1.1` | Configured DNS server |
| 51 | Slow, packet loss | Ping stats, traceroute jump | Degraded ISP path |
| 52 | Name resolves, IP unreachable | `ping` private IP | No route, needs VPN |
| 55 | Ping works, `curl` fails | Ping vs `curl` to same IP | Port/firewall block |
| 56 | One IP blocked, another works | `ping 8.8.8.8` vs blocked IP | Selective ISP block |
| 58 | Nearby IPs work, one IP dead | Nearby `/24` IPs vs target | Server down |
| 59 | Works, then fails, repeatedly | ICMP stable, TCP intermittent | Unstable path, packet loss |

**Decision points to retain:**
* The first test that answers "is my device, the network, or the destination?" is the one that matters most.
* A second device on a different network (mobile data) separates home problems from destination problems.
* Ping working while TCP fails points to a filter or port block, or to intermittent path quality.
* Name vs number: a number that works and a name that fails means DNS.

**Things to say out loud before checking:**
* Day 48: what does a `169.254` address tell me, and what would I check first?
* Day 49: if the gateway passes and `1.1.1.1` fails, which direction do I look?
* Day 58: if nearby IPs work but one doesn't, is it the path or the destination?

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Revision day, no commands run.
# Reference commands for the scenarios above:
ifconfig en1 | grep status                 # Day 48: Wi-Fi joined?
ping -c 2 <gateway-ip>                     # Day 49: router reachable?
dig @1.1.1.1 google.com                    # Day 50: does a different DNS answer?
traceroute -n <ip>                         # Days 49, 51, 56, 58: where the path ends
curl -v https://<site>                     # Days 55, 59: where the connection hangs
```

## 3. Key Takeaway / Blocker Solved

* Revised the full scenario set from Days 48-59 with the decision points in view.
* The sequence holds: check the first layer that answers the question, then move up or outward based on the result.
* Next: repeat the revision without notes, and note only the decisions you got wrong.