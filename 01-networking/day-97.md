# Day 97 - Review: Scenario Practice, Days 55-56
**Date:** 2026-11-25
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**Revisited the two port-and-route scenarios from Days 55 and 56.**

**Day 55: one public site blocked, everything else works**
* `ping` to the destination's IP works, but `curl` to port 443 times out.
* ICMP is allowed, while TCP to that port is blocked, so this is a port-level filter, not a dead destination.
* A phone on mobile data reaching the site means the destination is up, and the block sits on the home network or in the path.
* Test: `curl -v` shows where the connection hangs. `telnet <ip> 443` checks the port directly.

**Day 56: a working site times out, and the ISP is selectively blocking one IP**
* `ping 8.8.8.8` works from the same connection, but the site's IP times out.
* One IP failing while others pass means selective filtering, not a dead link.
* A phone on mobile data reaching the site proves the site is up.
* Escalation to the ISP: give the IP, the traceroute showing where it stops, and proof from another network.

**The shared rule:** a second network or a working reference (another IP, another carrier) decides whether the site is down or the path is blocked.

**Still to firm up:**
* Stating the difference between a port block (Day 55) and a selective IP block (Day 56) in one line each.
* Naming which test separates "site down" from "path blocked" without a pause.

## 2. Hands-on Lab & Commands 
```bash
# Review day, nothing run.
ping -c 2 8.8.8.8                   # a working reference IP (Day 56)
curl -v https://<site>              # where the connection hangs (Day 55)
telnet <ip> 443                     # does the port accept a connection? (Day 55)
traceroute -n <ip>                  # where the path stops (Day 56)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** both scenarios are decided by a comparison. A port block is decided by pinging the IP and curling the port. A selective IP block is decided by a working reference IP and a second network.
* **Blocker:** a port block and a selective IP block can both look like "it doesn't load." The difference is whether ping to that IP works.