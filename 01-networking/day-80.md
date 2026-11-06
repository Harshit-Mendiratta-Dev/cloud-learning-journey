# Day 80 - Reconsolidation: Re-reading Days 46-50
**Date:** 2026-11-06
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Second pass through the GitHub notes for Days 46-50, fixing the concepts after the first re-read.**

**Reconsolidated:**
* **Layered ladder (Day 46):** each command answers one yes-or-no question, from nearest to farthest: `127.0.0.1`, own IP, gateway, `1.1.1.1`, `dig`, `curl`.
* **Two error messages (Day 46):** "No route to host" means nothing was sent, because my machine has no path out. A timeout means the packet was sent and no reply came back. They point at different places.
* **Two-phase model (Day 47):** layer-finding asks *where* the fault is. Cause-finding asks *why*. A failed gateway ping means stop climbing and switch to cause-finding.
* **Link-local address (Day 48):** `169.254.x.x` means DHCP failed. Test a second device before touching hardware.
* **Gateway passes, `1.1.1.1` fails (Day 49):** the fault is beyond the router. Look at the router-to-ISP link, not at DNS.
* **Number works, name fails (Day 50):** DNS. `dig @1.1.1.1` proves the configured server is the problem.

**Still to firm up:**
* Stating the two-phase model without a pause.
* Explaining timeout versus "No route to host" in my own words.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ping -c 2 127.0.0.1                             # Day 46 baseline: own network stack
netstat -nr | grep default                      # Day 46 baseline: gateway
dig +time=2 +tries=1 google.com @10.255.255.1   # Day 46 break test: dead DNS server
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 46-50 turned the layer ladder into a working method. Each scenario starts at the first failing test and asks where to look next.
* **Blocker:** timeout versus "No route to host" is still the distinction that blurs, so it gets a restatement in your own words.