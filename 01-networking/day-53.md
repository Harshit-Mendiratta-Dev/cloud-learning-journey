# Day 53 - Scenario Consolidation: Patterns Across Days 48-52
**Date:** 2026-10-08
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The five scenarios and their faults:**

| Day | Scenario | Key Result | Where the fault is | Fix or escalate? |
|---|---|---|---|---|
| 48 | 169.254 address (DHCP failed) | No IP from router | Layer 1/2 or DHCP on router | Check: Wi-Fi active, test second device |
| 49 | `1.1.1.1` times out (internet dead) | Gateway fine, internet fails | Layer 3, router-to-ISP link | Escalate to ISP |
| 50 | `dig` fails, but `1.1.1.1` works | DNS times out, ping works | Layer 7, the Mac's configured DNS | Fix: change to `1.1.1.1` or `8.8.8.8` |
| 51 | High latency + packet loss | Traceroute jumps at hop 4 | Layer 3, inside the ISP | Escalate to ISP with traceroute data |
| 52 | Private network unreachable | `dig` works, `ping` times out | No route to private IP (expected) | User education: use VPN |

**The decision tree across scenarios:**

1. **Start with the ladder (Days 45-47):** ping layers 1-3, then 7 (DNS), then app.
2. **If everything passes except one thing:** look at that one thing.
   - Only `dig` fails? → DNS (Day 50)
   - Only a private IP fails? → No route / needs VPN (Day 52)
   - Latency jumps? → Identify where with traceroute, escalate (Day 51)
3. **If the network is partially broken:**
   - Test a second device to separate "my device" from "the network" (Days 48, 49, 51)
   - Reboot the router before escalating (Day 51)
4. **Gather evidence, then escalate:** traceroute output, packet loss %, min/avg/max latency, what devices work and what don't (Day 51)

**Patterns to notice while rereading:**

* **Which tests were the decision points?** (The first "no" told the story.)
* **Where did I jump to the wrong cause?** (Day 50: I suspected DNS when it was Layer 4-6. Day 49: I said DNS first when the number-based test ruled it out.)
* **When did a second device test change the answer?** (Days 48, 49, 51: all revealed the problem wasn't device-specific.)
* **Which scenarios needed escalation, and which didn't?** (Fix: Days 48, 50. Educate: Day 52. Escalate: Days 49, 51.)

**What each scenario taught:**

| Day | The lesson |
|---|---|
| 48 | A `169.254` address is a red flag. Test a second device first. |
| 49 | `1.1.1.1` fails + gateway passes = look outward (router-to-ISP), not upward (DNS/app). |
| 50 | Number works, name fails = DNS. `dig @server` proves it and finds the working server. |
| 51 | Degraded connection has numbers: measure loss %, latency (min/avg/max), and where traceroute jumps. |
| 52 | DNS resolves ≠ reachable. Private networks need a route (VPN). |

**Common mistakes across scenarios (and how to avoid them):**

* Treating the first plausible cause as the only one. List several, test cheapest first.
* Mixing up "DNS works" (dig succeeds) with "DNS is the problem" (dig is slow because the path is slow, Day 51).
* Confusing "my Mac" with "the network". Other devices answer that question in one test.
* Forgetting to mention numbers in escalations. A support team can't act on "it's slow."
* Escalating too early. Reboot the router and run the tests again (Day 51).

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# No new commands today. Reference from all five scenarios:
# Day 48: ipconfig getifaddr, ifconfig status, sudo ipconfig set DHCP
# Day 49: ping gateway, ping 1.1.1.1, traceroute, router admin page
# Day 50: dig google.com, dig google.com @1.1.1.1, curl -I
# Day 51: ping with statistics (min/avg/max), traceroute for delay jumps
# Day 52: dig company.local, traceroute to private IP
```

## 3. Key Takeaway / Blocker Solved

**Patterns to carry forward:**
* Dead connection (Day 49) vs degraded (Day 51) are different tests and different escalations.
* "Some sites work, some don't" can mean DNS (Day 50), a firewall, or no route (Day 52). The ladder decides.
* Always compare: your device + second device + your Mac + other Mac (if available). Cuts through device-specific issues.
* Evidence before escalation: traceroute, loss %, latency numbers, which devices work and which don't.

**Mistakes to drill:**
* Day 49: I treated DNS as the first suspect when a number-based ping already ruled it out.
* Day 50: I placed the fault in Layers 4-6 when Layers 4-6 were proven working.
* Day 51: I thought 6 ms latency proved the internet was fine (it only proved the router was fine).
* Day 52: I considered ARP and VPN config when the real answer was "no route by design."

**Questions to ask yourself while reading:**
1. Which test first ruled out the most possibilities?
2. When did I guess wrong, and what was the actual evidence?
3. Would I escalate this one, or fix it myself?
4. If I escalate, what numbers would I send?

**Next steps:** choose one or two scenarios to rework from memory (no notes) before moving to new patterns, or start a new scenario type (firewall blocking, destination down, packet corruption).