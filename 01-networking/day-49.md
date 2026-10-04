# Day 49 - Troubleshooting Scenario: "Wi-Fi Works, Internet Doesn't"
**Date:** 2026-10-04
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned (1h Theory)

**The scenario:** the user is on Wi-Fi but no website loads.

| Command | Result |
|---|---|
| `ping 127.0.0.1` | Pass |
| `ipconfig getifaddr en1` | Normal address (`192.168.x.x`) |
| `ping <own IP>` | Pass |
| `netstat -nr \| grep default` | Gateway shown |
| `ping <gateway>` | Pass, ~6 ms |
| `ping 1.1.1.1` | **Timeout** |
| `dig +short google.com` | **Timeout** |

**Where the fault is:**
* `ping 1.1.1.1` is a **Layer 3** test and uses **no DNS**. Failing it means packets can't cross the internet-facing part of the network.
* Gateway passes + `1.1.1.1` fails = the fault is **beyond my router**, between the router and the internet.
* My first guess was Layers 4-7 and DNS first. That was wrong: the number-based ping already excludes DNS.

**"Timeout" vs "No route to host":**
* No route to host = my Mac never sent anything.
* Timeout = the packet was sent, reached the router (gateway ping works), and no reply came back from somewhere past it.

**Why `dig` failing adds no new information:**
* DNS servers live out on the internet too, so they're unreachable for the same reason.
* A failure is *expected*, but not proof. The router sometimes answers from its own cache, so `dig` can occasionally succeed for a recent site even when the internet is cut.
* Number-based ping stays the reliable test.

**Causes (list several, then test the cheapest first):**
* The router's connection to the ISP is down (cable/fiber fault, ISP outage)
* The router lost its upstream connection or login (no WAN IP, the same "DHCP didn't happen" problem one level up)
* The router is misconfigured or blocking outgoing traffic
* The destination (`1.1.1.1`) is down: unlikely, but easy to rule out
* NOT low bandwidth: the 6 ms is the time to my *router*, not the internet, and low bandwidth makes things slow, not dead

**What I got wrong and why:**
* Treated 6 ms as evidence about internet speed. It only measures the hop to the router.
* Jumped to DNS, the most familiar suspect. When the number ping fails, the result pushes **outward** (toward the router's link to the world), not **upward** (to the application layers).
* Suggested `curl`, which needs the same path and would just fail the same way.

**Test order (cheapest first):**

| Step | Check | What it tells me |
|---|---|---|
| 1 | `ping 8.8.8.8` | Is it one destination or the whole internet? |
| 2 | Phone on home Wi-Fi: internet? | Whole network down, or just my Mac? |
| 3 | `traceroute -n 1.1.1.1` | Hop 1 replies, then `* * *` = packets die at/just past the router. A few hops then silence = fault inside the ISP. |
| 4 | Router internet/WAN light | Quick yes/no on the ISP link |
| 5 | Router admin page: WAN/Internet status | Did the router get a public IP from the ISP? |
| 6 | Phone with Wi-Fi off, on mobile data | Internet fine = my home connection is the fault |
| 7 | ISP app or outage page | Area outage? |
| 8 | Reboot router, wait, retry | Fixes many upstream-link problems |

**If my IP were missing or `169.254.x.x`:** DHCP would be the first suspect (Day 48). Here the IP and the router ping both pass, so DHCP is ruled out.

## 2. Hands-on Lab & Commands 
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
ping -c 2 8.8.8.8            # is it just one public IP, or the whole internet?
traceroute -n 1.1.1.1        # where does the trail go cold? (hop 1 = router)
netstat -nr | grep default   # confirm the gateway
```

## 3. Key Takeaway / Blocker Solved
* **Takeaway:** if the gateway ping passes but `1.1.1.1` fails, the fault is beyond the router (Layer 3). Don't climb to DNS or the application. Go outward: router WAN link, then ISP.
* **Blocker solved:** "`ping 1.1.1.1` uses DNS" was a mistaken idea. It's a bare IP test, and that's exactly why it's the reliable one.
* **Mistake to remember:** reaching for the most familiar suspect (DNS) instead of what the test results actually point to.
* **Pattern across Days 48-49:** list several possible causes, then test the cheapest first.
* **Next:** another scenario, e.g. "ping 1.1.1.1 works, but `dig` fails" (the reverse case, where DNS really is the suspect).