# Day 81 - Review: Re-reading Days 51-55
**Date:** 2026-11-07
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 51-55, line by line.**

**Revisited:**
* **Day 51 (degraded path):** a connection can work and still be bad. Measure the packet loss percentage and the min, avg, and max latency. Traceroute shows where the delay starts jumping.
* **Day 51 (your Mac dropping while others work):** the other devices rule out the router and ISP, so the cause sits on the Mac or its Wi-Fi link. An Ethernet test is the decisive check.
* **Day 52 (private network):** DNS resolving a name doesn't mean the address is reachable. A private IP needs a route, which a VPN provides.
* **Day 53 (consolidation):** the decision tree for the first five scenarios. The first failing test decides the branch.
* **Day 54 (revision of Days 41-44):** the foundations held. The gaps were usable addresses, the NAT and PAT distinction, firewall states, and RIR registration.
* **Day 55 (port blocked):** ping works, but `curl` to port 443 times out. ICMP is allowed and TCP to that port is blocked, so this is a firewall rule, not a dead connection.

**Still to firm up:**
* Stating the difference between degraded and dead in one sentence.
* Saying why ping working while `curl` fails points at a port block.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ping -c 20 1.1.1.1              # loss % and min/avg/max (Day 51)
traceroute -n 1.1.1.1           # where the delay starts (Day 51)
curl -v https://google.com      # where the connection hangs (Day 55)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 51-55 cover the scenarios that aren't a simple on-or-off failure: slow paths, missing routes, and blocked ports. Each one is told apart by a specific test.
* **Blocker:** degraded versus dead, and what a ping-works, curl-fails result means, are the two points that blur.