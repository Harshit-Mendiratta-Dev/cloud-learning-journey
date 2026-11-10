# Day 83 - Review: Re-reading Days 56-60
**Date:** 2026-11-10
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 56-60, line by line.**

**Revisited:**
* **Day 56 (selective block):** one IP times out while other IPs on the same path work, and a phone on mobile data reaches the site. That means selective filtering by the ISP, not a dead site or a dead path.
* **Day 57 (consolidation):** the decision points across Days 48 to 56. The first test that separates "working" from "broken" decides each case, and escalation needs numbers.
* **Day 58 (server down):** nearby IPs on the same `/24` answer, the target doesn't, and traceroute reaches the destination before it goes silent. The network is fine, and the service is down.
* **Day 59 (intermittent):** ping stays steady while `curl` comes and goes, and one hop's latency and loss vary from run to run. That points to an unstable path or congestion, and sampling over time is the evidence.
* **Day 60 (revision):** the same decision points, recalled from memory. Each case is decided by checking the first layer that answers the question.

**Still to firm up:**
* Naming the three selective-failure cases (selective block, server down, intermittent) without a pause.
* Saying why steady ping with flaky `curl` points at the path, not the destination.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ping -c 20 <target-ip>              # steady or lossy? (Day 59)
mtr -c 100 <target-host>            # loss per hop over time (Day 59)
traceroute -n <target-ip>           # where the path goes silent (Day 58)
curl -v https://<target-host>       # where the connection hangs (Day 56)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 56-60 cover failures that are selective: one destination, one service, or one flaky hop. Each is separated from the others by comparing it with a working reference, such as a nearby IP or a second network.
* **Blocker:** telling "server down" from "path blocked" depends on whether nearby addresses also fail. That comparison is what decides it.
