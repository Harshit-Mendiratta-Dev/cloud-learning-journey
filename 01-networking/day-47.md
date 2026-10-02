# Day 47 - Command Theory & Troubleshooting Mental Model
**Date:** 2026-10-02
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2.5 Hours 

## 1. Key Concepts Learned (1.5h Theory)

**Core idea:** each command asks one yes/no question, and each depends on the ones before it. Go from nearest and cheapest to farthest, and the first "no" shows where the fault is.

**The ladder (what each command asks, and why in this order):**

| # | Command | Question it answers | Why here | Layer |
|---|---|---|---|---|
| 1 | `ping 127.0.0.1` | Is my machine's network software alive? | Packet never leaves the Mac. If this fails, nothing else can work. | 3 (software) |
| 2 | `ipconfig getifaddr en1` | Do I have an IP address? | Every later test needs one. Blank = no DHCP lease. | 1/2 + 3 |
| 3 | `ping <own IP>` | Is my interface configured and up? | Handled inside the Mac; does NOT prove the Wi-Fi radio works. | 3 |
| 4 | `netstat -nr \| grep default` | Where does non-local traffic go? (gateway IP) | Need the router's IP before pinging it. | 3 |
| 5 | `ping <gateway>` | Can I reach my router? | First test that leaves the machine. Needs Wi-Fi + IP. | 1/2/3 |
| 6 | `ping 1.1.1.1` | Can I reach the internet by number? | Skips DNS on purpose. Best single test. | 3 |
| 7 | `dig +short google.com` | Can I turn a name into an IP? | Only meaningful once step 6 passes. | 7 (DNS) |
| 8 | `curl -I https://google.com` | Can I complete a full web request? | Uses everything below it. 2xx OK, 3xx redirect, 4xx client error, 5xx server error. | 4-7 |

**Two phases of troubleshooting:**
* **Layer-finding** (the ladder): tells me *where* the fault is. Maps to 7-step method, step 2 (theory).
* **Cause-finding** (checks inside that area): tells me *why*. Maps to step 3 (test the theory).

**Where to start (pick by symptom):**

| Symptom | Start with | Approach |
|---|---|---|
| Nothing loads at all | `ping 1.1.1.1` (middle) | Divide-and-conquer |
| Only one site/app fails | `curl -I <site>` | Top-down |
| Suspect cable/Wi-Fi/hardware | Check link, `ipconfig` | Bottom-up |
| Internet works by IP, not by name | `dig` | DNS focus |

**Reading the result (decision table):**

| Result | Meaning | Next move |
|---|---|---|
| `1.1.1.1` passes | Network and internet OK | Go UP: test DNS, then app |
| `1.1.1.1` fails | Fault at Layer 3 or below | Go DOWN: gateway, then IP, then link |
| Gateway ping fails | Problem is local (Wi-Fi, IP config, or router) | STOP going up. Switch to cause-finding (below). |
| "No route to host" | My machine has no path out; nothing was sent | Check Wi-Fi, IP, default route |
| "Request timeout" | Packet sent, no reply | Check path or far end |
| `dig` times out but `1.1.1.1` works | DNS server problem only | Try another DNS server (`dig google.com @1.1.1.1`) |

**Cause-finding checks when the gateway ping fails:**
* Is the Mac on Wi-Fi? (`ifconfig en1 | grep status` should say `active`)
* Do I have a valid IP? Blank = no DHCP lease. `169.254.x.x` = Mac gave itself an address because DHCP failed.
* Is the gateway in the same subnet as my IP? (e.g. both `192.168.29.x`)
* Can I ping another device on the same Wi-Fi?
* Do other devices (phone) work on this Wi-Fi? Yes = problem is my Mac. No = router or ISP.
* Caution: some routers ignore ping. Confirm with a second signal.

**Other facts:**
* TTL: local pings show 64; internet pings show lower (e.g. 51) because they crossed routers.
* Latency: ~0.2 ms own IP, ~6-7 ms router over Wi-Fi, ~8-13 ms internet.
* `netstat -nr`: `-n` = numbers only (no DNS lookup), `-r` = routing table.
* en0 = Ethernet, en1 = Wi-Fi on this Mac.

## 2. Hands-on Lab & Commands (1h Lab)
```bash
# Theory day. Reference sequence from Day 46:
ping -c 2 127.0.0.1
ipconfig getifaddr en1
ping -c 2 [own-ip]
netstat -nr | grep default
ping -c 2 [gateway-ip]
ping -c 2 1.1.1.1
dig +short google.com
curl -I https://google.com
```

## 3. Key Takeaway / Blocker Solved
* **Takeaway:** a failed gateway ping stops the climb up the stack. Switch from "which layer?" to "why?", and use cause-finding checks, not more ladder steps.
* **Correction learned:** a ping to my own IP never crosses the Wi-Fi radio, so it can't prove Wi-Fi works. The gateway ping does.
* **Practice scenario 1:** gateway ping fails, own-IP ping passes. Where is the fault? What do I run next, and why?
* **Practice scenario 2:** my IP is `169.254.x.x` and the gateway ping fails. What does the IP tell me, and which step do I focus on?
* **Revisit method:** work scenarios with these tables beside me, answer out loud first, then check against the decision table. Repeat until the order feels automatic.