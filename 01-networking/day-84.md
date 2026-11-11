# Day 84 - Reconsolidation: Re-reading Days 56-60 (Second Pass)
**Date:** 2026-11-11
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Second pass through the GitHub notes for Days 56-60, consolidating the concepts after the first re-read.**

**Reconsolidated:**
* **Selective block (Day 56):** one IP fails while other IPs on the same path work, and a second network reaches the site. That's ISP-side filtering of that address. The fix is to show the ISP the IP and the evidence.
* **Consolidation (Day 57):** the decision points across Days 48 to 56. The first test that separates working from broken decides each case, and an escalation needs numbers: loss percentage, latency, and the hop where it stops.
* **Server down (Day 58):** nearby IPs answer, the target doesn't, and traceroute reaches the destination before it goes silent. The path is fine, and the service on that address is down. The right response is to wait for the maintenance window and retry.
* **Intermittent failure (Day 59):** ping stays steady while `curl` comes and goes, and one hop's latency and loss vary between runs. That's an unstable path or congestion, so the evidence is samples taken over time, not one test.
* **Revision (Day 60):** the decision points, recalled from memory. Each case is decided by the first layer that answers the question.

**Still to firm up:**
* Saying in one line why steady ping with flaky `curl` points at the path, not the destination.
* Telling server down from a path block in one sentence: check whether nearby addresses also fail.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
ping -c 20 <target-ip>              # steady or lossy? (Day 59)
mtr -c 100 <target-host>            # loss per hop over time (Day 59)
traceroute -n <target-ip>           # where the path goes silent (Day 58)
curl -v https://<target-host>       # where the connection hangs (Day 56)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 56-60 cover failures that are selective: one destination, one service, or one flaky hop. A working reference, such as a nearby IP or a second network, is what separates them.
* **Blocker:** server down versus path block depends on whether nearby addresses also fail. That comparison decides it.
* **Next:** Day 85, the re-read of Days 61-63 (certificate interception and the framework video).