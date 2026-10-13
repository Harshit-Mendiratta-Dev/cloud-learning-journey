# Day 58 - Troubleshooting Scenario: "One Service Down, Others Nearby Up"
**Date:** 2026-10-13
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The scenario:** an API that worked yesterday is completely dead.

**The measurements:**
* `ping 1.1.1.1`: Pass, ~8 ms, 0% loss
* `dig [api.example].com`: Pass, returns `203.0.113.200`
* `ping 203.0.113.200`: **Timeout** (100% loss)
* `ping 203.0.113.1` (nearby service): **Pass**, ~30 ms, 0% loss
* `ping 203.0.113.2` (another nearby service): **Pass**, ~30 ms, 0% loss
* `curl -I https://[api.example].com`: **Timeout**
* `traceroute -n 203.0.113.200`: All hops reply until the destination itself times out
* Phone on home Wi-Fi: Can't reach it
* Phone on mobile data: **Can't reach it either**
* Status check: Server maintenance scheduled 2-4 AM UTC

**What the data tells me:**

| Result | What it means |
|---|---|
| Nearby IPs work, this IP doesn't | The path to that `/24` block is fine. This specific IP is the problem. |
| Both networks time out | Rules out ISP, home network, and carrier. The destination is at fault. |
| Traceroute reaches the destination | The route exists. But the server isn't responding. |
| Maintenance scheduled | The service is down by design, not by accident. |

**Network is working. Server is not.**

This is a **service problem**, not a **network problem**. The network diagnosis proved the path is perfect. Multiple devices on multiple networks all fail to reach *this specific IP* while reaching nearby IPs fine. The server itself is offline.

**Where is the problem?**
* Not your home network (multiple networks fail).
* Not the ISP (nearby IPs reachable, mobile data also fails).
* Not a firewall or block (nearby IPs on the same `/24` are reachable).
* **The server at that IP is offline or unresponsive.**

**Possible causes:**
* Server maintenance (confirmed by status page)
* Server crashed or restarted
* Power loss to that machine
* Service stopped but server still running (misconfiguration)
* Hardware failure

**How to tell this scenario from earlier ones:**

| Scenario | Test | Result | Diagnosis |
|---|---|---|---|
| ISP down (Day 49) | `ping 1.1.1.1` | Timeout | Network path broken |
| ISP blocks (Day 56) | `ping other-IP` | Works | Selective ISP block |
| Server down (Day 58) | `ping nearby-IP` | Works | This server offline |

**Message to the user:**

> The network path to the server is working fine. Other services on the same network block are reachable. The server at `203.0.113.200` itself is not responding. According to the status page, it's down for scheduled maintenance until 4:00 AM UTC. Try again after 4:30 AM to allow for startup time.

## 2. Hands-on Lab & Commands 
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
ping 1.1.1.1                           # internet reachable?
dig [api.example].com                  # DNS resolves?
ping 203.0.113.200                     # this IP reachable?
ping 203.0.113.1                       # other IPs on same /24 reachable?
ping 203.0.113.2                       # another one nearby?
curl -v https://[api.example].com      # verbose to see where it hangs
traceroute -n 203.0.113.200            # does the route exist?
telnet 203.0.113.200 443               # can you reach port 443?
# Check status page or Twitter for maintenance announcements
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** if nearby IPs work but this one doesn't, and multiple networks all fail, the server is offline. The network is not the problem.
* **Blocker solved:** this is the first scenario where the answer is "it's not the network." Days 48-56 were all network problems. Day 58 is a service problem.
* **Pattern:** run the ladder to prove the network works. If it does but the app still fails, the server is the answer.
* **Message to user:** not technical troubleshooting, just factual status and when to retry.
* **Next:** more scenarios (packet corruption, path MTU issues) or move into HA/redundancy and IPv6 depth for the remaining Phase 1 days.