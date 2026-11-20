# Day 93 - Review: ICMP, TTL, and Routing (Days 8-9)
**Date:** 2026-11-20
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Revisited the ICMP, TTL, and routing material from Days 8 and 9.**

**ICMP:**
* A Layer 3 control protocol. It carries error and diagnostic messages, not application data.
* Echo request and echo reply are what `ping` uses.
* Errors such as "time exceeded" and "destination unreachable" are how the network reports problems back to the sender.

**TTL (time to live):**
* A field in the IP header, set by the sender, and decremented by one at every router.
* When TTL reaches zero, the router drops the packet and sends an ICMP "time exceeded" back to the source.
* Stops packets from looping forever on a misconfigured network.
* Local pings showed TTL 64 (no routers crossed). Pings to `1.1.1.1` showed TTL 51, so the packet crossed many routers.

**Traceroute and TTL:**
* Traceroute sends packets with TTL = 1, then 2, then 3, and so on.
* Each router that drops a packet replies with "time exceeded," which reveals that hop's address.
* The hops are read in order, so the path shows itself one router at a time.
* A silent hop (`* * *`) means that router didn't send an error reply. The path can still continue past it.

**Routing (Day 9):**
* A router forwards a packet by looking up the destination IP in its routing table and choosing the best matching route.
* The default route (`0.0.0.0/0`) matches everything, and it sends traffic to the gateway when no more specific route exists.
* The hand-built traceroute with `ping -m` (Day 9) showed the same mechanism: TTL increases one hop at a time.

**Still to firm up:**
* Explaining in one line why traceroute uses increasing TTL values.
* Stating what a router does when TTL reaches zero, without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
traceroute -n 1.1.1.1        # increasing TTL, one hop per line (Day 9)
ping -c 2 1.1.1.1            # check the TTL in the reply (51 means many hops)
netstat -nr | grep default   # the default route (0.0.0.0/0) to the gateway
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** TTL is what makes traceroute work. Each router decrements it and reports back when it expires, so the path is revealed one hop at a time.
* **Blocker:** a silent hop in traceroute is not the same as a failed hop. The path can continue past it, which is why traceroute alone doesn't prove a break.
