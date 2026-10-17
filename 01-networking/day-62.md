# Day 62 - Layers Refined: What Each Does, and Where Things Fail
**Date:** 2026-10-17
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The layers, what each one answers, and how to test it:**

| Layer | Question it answers | Test | Failure looks like |
|---|---|---|---|
| 1 Physical | Is anything connected? | Link status, cable, `ifconfig en1 \| grep status` | Wi-Fi off, cable unplugged |
| 2 Data link | Can I reach devices on my segment? | `arp -a`, `ifconfig` | Joined Wi-Fi but no local reachability |
| 3 Network | Is there a path to that IP? | `ping`, `traceroute`, `netstat -nr` | "No route to host", timeouts, `169.254` |
| 4 Transport | Can a connection to this port form? | `telnet host port`, `nc -vz` | Ping works, port fails |
| 5-6 Session/Presentation | Did the secure handshake succeed? | `curl -v` | Certificate warnings, TLS failure |
| 7 Application | Does the service respond correctly? | `dig`, `curl -I` | Name won't resolve, HTTP errors |

**Corrections from today's questions:**

* **ARP is Layer 2.** It resolves an IP to a MAC on the same segment (broadcast domain). Layer 3 relies on it for local delivery. Across networks, the Mac ARPs for the **gateway's** MAC, not the remote IP.
* **DHCP is Layer 7** (an application protocol over UDP 67/68), not Layer 3. It doesn't use ICMP. It runs DORA as broadcasts on the local segment, so it happens before the Mac has a usable IP.
* **DHCP failure and ISP failure look different:**
  * DHCP fails: the Mac can't reach the router's normal address. Gateway ping fails.
  * ISP lapses: the Mac reaches the router, gateway ping passes, `1.1.1.1` fails. This is the Day 49 pattern, on the WAN side.
* **A static IP on the router's subnet** can reach the router without DHCP or the ISP. Same subnet means Layer 2 delivery, and no public IP is needed.

**How ARP works, step by step:**
1. The Mac wants to reach `[router-ip]` but has no MAC for it in the cache.
2. It broadcasts an ARP request to `ff:ff:ff:ff:ff:ff`: "who has `[router-ip]`?"
3. Only the owner of that IP replies, unicast, with its MAC.
4. The Mac caches the IP-to-MAC pair for a short time.

**Reading `arp -a` on my Mac:**
* The router entry is cached from ARP.
* Another device on the Wi-Fi is cached the same way (`?` means no hostname was resolved).
* `224.0.0.251` (mDNS) is a **multicast** entry marked `permanent`. Multicast addresses map to multicast MACs by a fixed rule, so they aren't learned by ARP.
* Redact MACs and `192.168.x.x` addresses before posting to the repo.

**Open question for Day 63:** the router is replaced with a new device keeping the same IP. The Mac's ARP cache still holds the old MAC. What does `ping` do, and how do you clear the stale entry?

## 2. Hands-on Lab & Commands (0.5h Lab)
```bash
# Theory day, no new commands run. Commands used today:
netstat -nr | grep default     # find the router IP (first "default" line)
ifconfig en1 | grep ether      # my Mac's MAC address
arp -a                         # cached IP-to-MAC entries
ifconfig en1                   # check subnet mask and gateway on this host
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** each layer answers one question, and a fault lives at the lowest layer that fails. DHCP is Layer 7 but has to succeed before the Mac can route at Layer 3.
* **Blocker solved:** I'd placed DHCP at Layer 3 because it "goes over Wi-Fi." It actually runs as local broadcasts, before any routing exists.
* **Mistake to remember:** "same subnet" means Layer 2 delivery, with ARP as the mechanism. "Different network" means Layer 3 routing through the gateway.
* **Next:** Day 63 (the four-question loop and the drill), then the stale-ARP question.