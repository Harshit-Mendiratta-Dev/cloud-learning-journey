# Day 51 - Troubleshooting Scenario: "Internet Works But Is Painfully Slow"
**Date:** 2026-10-06
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The scenario:** the user says pages load slowly and video buffers.

**The measurements:**
* `ping <gateway>` (20 pings): 0% loss, avg ~6 ms, max ~12 ms
* `ping 1.1.1.1` (20 pings): **15% loss**, min 9 ms, **avg ~140 ms, max ~620 ms**
* `dig google.com`: Works but **Query time: ~1800 ms**
* `curl -I https://google.com`: Works but takes ~6 seconds
* Phone on home Wi-Fi: Also slow
* Phone on mobile data (Wi-Fi off): Fast

**Traceroute hops 1-7:**
```
1: ~6 ms
2: ~10 ms
3: ~12 ms
4: ~180 ms  ← JUMP
5-7: ~190-205 ms (stay high)
```

**What the data tells me:**

| Result | What it means |
|---|---|
| 0% loss to gateway, ~6 ms | Mac, Wi-Fi link, home network are fine. **Stop suspecting home Wi-Fi.** |
| 15% loss to internet, 140 ms average | The connection works but is lossy and slow. **Degraded, not dead.** |
| Delay jumps at hop 4, stays high | The problem starts at the hop 3→4 link, likely inside the ISP's network. |
| Phone on Wi-Fi also slow | The problem isn't specific to the Mac. |
| Phone on mobile data is fast | The internet and destination are fine. **The home connection (router to ISP) is the fault.** |

**Degraded vs dead (compare to Day 49):**
* **Day 49:** pings timed out completely, 0% received. The connection was dead.
* **Day 51:** pings succeed but with loss and high latency. The connection works badly.
* Name the numbers in your escalation, because that's what makes it checkable.

**DNS as a symptom, not a cause:**
* The 1800 ms `dig` time isn't a DNS failure (Day 50 style). It's a DNS query crossing a slow, lossy path.
* DNS is affected, but it's not the cause. The path itself is the problem.

**Possible causes (test cheapest first):**
* ISP congestion or a fault on the hop 3-4 link
* A damaged or degraded line between your home and the ISP
* The router's WAN side misbehaving
* A device on your network saturating the connection
* Interference (if Wi-Fi; ruled out here because phone and iPad are also slow)

**Troubleshooting order:**
1. Reboot the router, wait for it to come back, re-run the tests
2. Run `traceroute -n 1.1.1.1` again to see if the hop 4 jump returns or changes
3. If nothing changes, escalate to the ISP with the evidence

**Escalation message (step 5 of the 7-step method: implement or escalate):**

> Since [date], all devices on my home Wi-Fi are slow while mobile data is fast. Pings to the router are clean (0% loss, ~6 ms), but pings to the internet show 15% loss and ~140 ms average, with spikes to 620 ms. Traceroute shows latency jumping from ~12 ms to ~180 ms at hop 4 and staying high thereafter. I've rebooted the router with no change. Please check the line and the upstream link.

---

## A Real Problem: Your Mac Drops Connection While Wi-Fi Shows Connected

You mentioned: *"My Mac keeps dropping the internet connection while Wi-Fi still shows connected. Phone, iPad, and TV all work fine. Speed test shows ~100 Mbps."*

This is a **different pattern** from the scenario above. When other devices work, the router and ISP are fine, so the cause is on the Mac or its Wi-Fi link.

**Quick tests to run the next time it drops:**

```bash
ping -c 100 <gateway-ip>      # do pings to router fail or time out?
ipconfig getifaddr en1        # still have an IP? or a 169.254 one?
system_profiler SPAirPortDataType | grep -E "Channel|Signal|Noise"   # link quality
```

**Likely cause:** From Day 25, you found three networks on 2.4GHz Channel 1 and a neighbor on your 5GHz Channel 149. **Wi-Fi interference is a strong suspect**, especially since the Mac can drop where a phone doesn't (different radios, positions, driver behavior).

**Decisive test:**
* **Plug the Mac into Ethernet** (use your Day 35 Cat6 cable to the router). Run it over a day or two without Wi-Fi.
  * If drops stop → the problem is the Mac's Wi-Fi link or driver. Consider: channel switch (5GHz to 6GHz if your router supports it), changing channels, moving the router, or updating drivers.
  * If drops continue → the problem is the Mac-to-ISP connection itself, and it's rare enough that it needs escalation.

**Other suspects to check:**
* Is the Mac going to sleep and failing to wake the network cleanly? Check System Settings > Lock Screen & Screen Saver.
* Are background apps (iCloud sync, Time Machine, updates) downloading when the drops happen? Check Activity Monitor > Network.
* Does the router show the Mac's MAC address consistently, or does it disappear and reappear? Check the router's connected devices list.

## 2. Hands-on Lab & Commands 
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
ping -c 20 <gateway-ip>            # loss and latency to router
ping -c 20 1.1.1.1                 # loss and latency to internet
traceroute -n 1.1.1.1              # where does delay start?
dig google.com                     # DNS query time
curl -I https://google.com         # real page load time

# For your real Mac drops:
ping -c 100 <gateway-ip>           # sustained test during a drop
ipconfig getifaddr en1             # IP check
system_profiler SPAirPortDataType | grep -E "Channel|Signal|Noise"   # link quality
```

## 3. Key Takeaway / Blocker Solved
* **Takeaway:** a degraded connection (packet loss + high latency) differs from a dead one. Measure the specific numbers and identify where the delay jumps (traceroute), not just that it exists.
* **Escalation:** write it in steps 1-4 of the 7-step method, because step 5 needs evidence. Include the traceroute hop where the jump happens, the packet loss %, and the min/avg/max latency.
* **DNS pattern:** if DNS is slow *because* the path is slow, fixing the path fixes DNS. If DNS is broken *while* the path is fine (Day 50), DNS itself is the target.
* **Your real problem:** degraded connection for all devices = ISP/router. Connection drops only on the Mac while others work = Mac's Wi-Fi link or driver. Ethernet test is decisive.
* **Next:** more scenarios (packet loss, timeout vs no-route patterns) or move into a hands-on troubleshooting week with a real test network if one is available.