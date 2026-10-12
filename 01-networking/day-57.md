# Day 57 - Consolidation Review: Days 48-56 Scenario Practice
**Date:** 2026-10-12
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.0 Hours 

## 1. Key Concepts Learned 

**Reread and reviewed all nine scenarios from Days 48-56:**

* Day 48: 169.254 address (DHCP failure, test second device)
* Day 49: Internet dead (`1.1.1.1` times out, traceroute to ISP link)
* Day 50: DNS fails (`dig` times out, `1.1.1.1` works, change DNS server)
* Day 51: Degraded path (latency jump, traceroute, packet loss %)
* Day 52: Private network (route doesn't exist, needs VPN)
* Day 53: Patterns across Days 48-52 (decision tree)
* Day 54: Revision of Days 41-44 blocks
* Day 55: Port blocked (ping works, `curl` times out, firewall rule)
* Day 56: ISP selective block (one IP blocked, another works, site is up)

**The decision tree across scenarios:**

1. **Start with the ladder:** ping gateway, then `1.1.1.1`, then `dig`, then `curl`.
2. **If the ladder breaks, which step failed first?**
   - Gateway fails → DHCP/Wi-Fi issue (Day 48)
   - `1.1.1.1` fails → ISP link (Day 49)
   - `dig` fails → DNS (Day 50)
   - All work but slow → degraded path (Day 51)
   - Name resolves, IP unreachable → no route (Day 52)
   - Ping works, `curl` times out → port blocked (Day 55)
   - One IP works, another times out → selective ISP block (Day 56)

**Patterns to notice while rereading:**

* **Which test answered the question?** (The first "no" told most of the story.)
* **When did a second device matter?** (Days 48, 49, 51, 56: "Is it my device or the network?" answered in one step.)
* **Which scenarios needed escalation?** (Days 49, 51, 56 → ISP. Days 50, 55 → user fix. Day 52 → user education.)
* **What numbers matter?** (Days 51, 56: packet loss %, latency min/avg/max, which hops reply in traceroute.)

**Common misreads from the scenarios (lessons learned):**

* Day 49: Treated DNS as the first suspect when `1.1.1.1` already ruled it out.
* Day 50: Placed the fault in Layers 4-6 when Layers 4-6 were proven working (curl to IP succeeded).
* Day 51: Thought 6 ms latency proved internet speed (it only proved the router was responsive).
* Day 55: Initially guessed the destination was the problem when the phone on mobile proved it was working.
* Day 56: Almost missed the ISP block by not comparing two different network paths.

**The decision framework (nine scenarios across Layers 1-7):**

| Layer | Symptom | Key test | Working result | Failed result | Fix |
|---|---|---|---|---|---|
| 1/2 | No address | `ipconfig` | Has IP | `169.254.x.x` | Check DHCP, Wi-Fi |
| 3 (local) | Can't reach router | `ping gateway` | ~6 ms | "No route to host" | Check Wi-Fi, IP |
| 3 (internet) | Can't reach internet | `ping 1.1.1.1` | ~8-30 ms | Timeout | Escalate (ISP) |
| 3 (ISP selective) | One IP blocked | `ping 8.8.8.8` vs blocked IP | One works, one doesn't | Both timeout | Escalate (ISP) |
| 3 (degraded) | Slow overall | `ping` with stats | 0% loss, <100ms | High loss, >100ms | Escalate (ISP) |
| 7 (DNS) | Names don't work | `dig` vs `dig @1.1.1.1` | One works, one doesn't | Both timeout | Change DNS |
| 7 (port) | App times out | `ping` IP vs `curl` | Ping works, curl fails | Both fail | Check firewall |
| 3 (route) | Private network | `ping private-IP` | Reachable | Unreachable | Use VPN |

**What to ask yourself while rereading:**

1. What was the first test that separated "working" from "broken"?
2. When did I guess wrong, and what was the actual evidence?
3. Which scenarios would I escalate, and which would I fix myself?
4. If I escalate, what numbers would I send?
5. How does the second device test change the diagnosis?

## 2. Hands-on Lab & Commands 
```bash
# Consolidation review day, no commands run.
# All commands from Days 48-56 reviewed as part of the scenario consolidation.
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** nine scenarios, nine different failure modes. The decision tree applies to all of them: run the ladder, identify what fails, then ask whether it's your device, the network, DNS, a route, or a filter.
* **Pattern across all nine:** test with a second device to separate "my machine" from "the network," then escalate with numbers (loss %, latency, which hops).
* **Mistakes to watch for:** jumping to DNS or "the destination is down" before the ladder rules them out; forgetting to compare two network paths (Wi-Fi vs mobile data); guessing about the ISP instead of sending traceroute data.
* **Ready to move forward:** the troubleshooting framework is locking in. Days 1-40 built the foundation. Days 45-56 built the diagnosis skill.
* **Next:** HA/redundancy, deeper IPv6, or start Phase 2 transition (ask which direction at the end of Day 57).