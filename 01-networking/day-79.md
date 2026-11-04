# Day 79 - Review: Re-reading Days 46-50
**Date:** 2026-11-04
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 46-50, line by line.**

**Revisited:**
* **Day 46 (layered lab):** the ladder from `127.0.0.1` through the gateway, `1.1.1.1`, `dig`, and `curl`. Each command answers one yes or no question, from nearest to farthest.
* **Day 46 (break tests):** Wi-Fi off gave "No route to host" (nothing sent). A dead DNS server gave a timeout (sent, no reply). The two errors point at different places.
* **Day 47 (mental model):** troubleshooting has two phases. Layer-finding asks where the fault is. Cause-finding asks why it's there. A failed gateway ping means stop climbing and switch to cause-finding.
* **Day 48 (169.254 address):** a link-local address means DHCP failed. Test a second device before touching hardware.
* **Day 49 (Wi-Fi works, internet doesn't):** if the gateway passes and `1.1.1.1` fails, the fault is beyond the router. Look outward at the router-to-ISP link, not at DNS.
* **Day 50 (ping works, names fail):** a number that works with a name that fails means DNS. `dig @1.1.1.1` proves the configured server is the problem.

**Still to firm up:**
* Stating the two-phase model (layer-finding, then cause-finding) without a pause.
* Explaining why a timeout and "No route to host" point at different places.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ping -c 2 127.0.0.1                          # Day 46 baseline: own network stack
netstat -nr | grep default                   # Day 46 baseline: gateway
dig +time=2 +tries=1 google.com @10.255.255.1   # Day 46 break test: dead DNS server
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 46-50 turned the layer ladder into a working method. Each scenario starts at the first failing test and asks where to look next.
* **Blocker:** the timeout versus "No route to host" distinction is easy to blur, so it's worth restating in your own words.