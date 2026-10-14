# Day 59 - Troubleshooting Scenario: "Intermittent Timeouts—Works Then Doesn't"
**Date:** 2026-10-14
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The scenario:** a site works fine, then suddenly times out. Refresh and it works again. The pattern repeats: works, fails, works, fails.

**The measurements (inconsistent across time):**

| Check | Attempt 1 | Attempt 2 (5 min later) |
|---|---|---|
| `ping -c 20 1.1.1.1` | 0% loss, avg 8 ms | 0% loss, avg 8 ms |
| `curl -I https://example.com` | **Pass, 200 OK** | **Timeout** |
| `curl -I https://example.com` (retry) | **Timeout** | **Pass, 200 OK** |
| `traceroute -n example.com` | 10 hops, normal latency | **Hop 8: 1000+ ms** |
| `mtr -c 100 example.com` | 0% loss, avg 20 ms | **60% loss on hops 7-8** |
| Phone on home Wi-Fi | Works | **Timeout** |
| Phone on mobile data | Works | Works |

**What the data tells me:**

| Result | What it means |
|---|---|
| Destination not at fault (mobile data works) | The server is fine. |
| Home Wi-Fi is intermittent | The problem is between home and destination. |
| ICMP (ping) is consistent, TCP (curl) is not | Small packets get through, but TCP connections fail intermittently. |
| Hop 8 shows variable latency and loss | An unstable link on the path. |

**The key insight: ICMP stable, TCP intermittent = packet loss affecting larger packets or path quality degradation.**

Ping is small, simple packets. TCP requires a connection and sends larger packets. If ping always works but TCP sometimes fails, it suggests:

- **Packet loss that discriminates by size:** small ICMP packets get through, TCP packets (or their fragments) get dropped
- **Path MTU issues:** TCP packets are fragmented, and fragments are being dropped selectively
- **QoS or buffer overflow:** some router prioritizes small packets and drops larger ones under load
- **Unstable link quality:** hop 8 shows variable latency and loss, suggesting congestion or a flaky line

**Where is the problem?**
* Not the destination (mobile data works consistently).
* Not the home network exclusively (phone on Wi-Fi also fails intermittently, but reachable at other times).
* **The path between home and the destination, likely inside the ISP's network** (hop 8 is unstable).

**Possible causes:**
* ISP backbone congestion (rush hour, peak traffic)
* Peering point instability (ISP-to-upstream link dropping packets)
* Packet loss at a specific hop that's being overloaded
* Home router buffer exhaustion under certain conditions
* Wi-Fi interference causing packet retransmissions (check signal/noise ratio)

**Diagnostic commands:**

| Command | What it shows |
|---|---|
| `curl -v https://example.com` | Verbose output shows where the connection hangs (TCP, TLS, or timeout) |
| `sudo tcpdump -n -i en1 -c 50 -X host example.com` | Capture traffic; see if packets are sent but not received, or connection never established |
| `netstat -s \| grep -i "dropped"` | System-level packet drops |
| `airport -I` | (Mac) Wi-Fi signal strength, noise ratio, channel (check for interference) |
| `mtr -c 100 example.com` | Extended traceroute with statistics over 100 packets; shows which hop is flaky |

**Before escalating:**

1. **Reboot the router** and rerun `mtr -c 100` for a full minute. If loss clears, it was congestion or a transient hiccup.
2. **Run `airport -I` multiple times** during success and failure periods to see if signal/noise ratio correlates with timeouts.
3. **Collect concrete data:** which hop, what % loss, what latency, which times it happened.

**Message to the user (before escalation):**

> The destination is reachable and working (confirmed on mobile data). Your home Wi-Fi connection is intermittently unstable, likely due to congestion or packet loss at hop 8 of your ISP's network. Try rebooting your router. If the problem persists, contact your ISP with this information: [hop number], [packet loss %], [latency], and the time the issue occurred.

**Intermittent vs Dead—the difference:**
* **Dead (Day 58):** consistent timeouts, destination doesn't respond at all.
* **Intermittent (Day 59):** sometimes works, sometimes fails. Same path, same destination, but variable quality.

Intermittent almost always means congestion, packet loss, or instability—not a binary broken/working condition. It's harder to diagnose because the problem is fleeting.

## 2. Hands-on Lab & Commands 
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
ping -c 20 1.1.1.1                           # ICMP stable?
curl -I https://example.com                  # TCP works?
curl -v https://example.com                  # verbose to see where it hangs
traceroute -n example.com                    # which hop is unstable?
mtr -c 100 example.com                       # extended stats on packet loss
sudo tcpdump -n -i en1 -c 50 -X host example.com   # capture the traffic
netstat -s | grep -i "dropped"               # packet drops on your Mac
airport -I                                   # (Mac) Wi-Fi signal/noise ratio
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** ICMP (ping) stable but TCP (curl) intermittent = packet loss or path quality degradation, not a binary fault.
* **Blocker solved:** I didn't initially know to check packet loss discrimination (why ICMP works but TCP fails). The size/type difference is the clue.
* **Pattern learned:** intermittency is harder to diagnose than a clean failure because you need to catch it in action. Collect data during both success and failure periods.
* **Mistake to remember:** don't jump to "reboot the router" without first checking whether the problem is the router, the ISP, or Wi-Fi interference. Run diagnostics first.
* **Escalation data:** hop number, packet loss %, min/avg/max latency, and the time(s) it happened. Without this, the ISP can't act.
* **Next:** path MTU issues (fragmentation), or move into HA/redundancy and IPv6 depth for remaining Phase 1 days.