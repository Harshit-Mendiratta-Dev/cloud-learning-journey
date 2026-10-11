# Day 56 - Troubleshooting Scenario: "A Working Site Now Times Out"
**Date:** 2026-10-11
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The scenario:** the user's API endpoint used to work, now times out. Is it down?

**The measurements:**
* `ping 1.1.1.1`: Pass, ~8 ms, 0% loss
* `dig [company-api].com`: Pass, returns `198.51.100.50`
* `ping 198.51.100.50`: **Timeout** (100% loss)
* `ping 8.8.8.8`: **Pass**, ~9 ms, 0% loss
* `curl -I https://[company-api].com`: **Timeout**
* `traceroute -n 198.51.100.50`: Hops 1-5 reply (~6-30 ms), hop 6+ timeout
* `telnet 198.51.100.50 80`: Timeout
* Phone on home Wi-Fi: Can't reach [company-api].com
* Phone on mobile data: **Can reach [company-api].com fine**

**What the data tells me:**

| Result | What it means |
|---|---|
| Site's IP exists (dig works) | The domain is registered and DNS works. |
| Ping to `8.8.8.8` works | The ISP path to the internet is open. |
| Ping to the site's IP fails | A specific IP is unreachable. |
| Traceroute 1-5 reply, 6+ timeout | The packet gets partway out, then hits a barrier. |
| Phone on mobile data reaches it | The site is **alive and working**. |

**The key insight: phone on mobile data works = site is NOT down.**

If the site were down, the phone couldn't reach it on any network. Since it works on mobile data (a different carrier), the site is fine. The problem is the **ISP blocking that specific IP**.

**Selective blocking vs outage:**
* `ping 8.8.8.8` passes, but `ping 198.51.100.50` fails. Same ISP, same path, but one IP works and one doesn't.
* This is **not a dead ISP link** (Day 49). It's **selective filtering**: the ISP has a rule that blocks traffic to this IP.
* The barrier at hop 6 is likely an ISP firewall or route policy.

**Where is the block?**
* Not the destination (phone on mobile data reaches it).
* Not the path itself (other IPs work fine).
* **The ISP has filtered this specific IP**, probably by accident, a configuration error, or a policy against this API.

**Possible causes:**
* ISP firewall rule blocking the IP by mistake
* ISP policy blocking this API (rate limiting, DDoS protection, or content filtering)
* BGP misconfiguration at the ISP (the route is being filtered)
* The IP was recently added to a blocklist

**Diagnostic commands:**

| Command | What it shows |
|---|---|
| `curl -v https://198.51.100.50` | Where the connection hangs (TCP, TLS, or timeout) |
| `telnet 198.51.100.50 443` | Can you connect to port 443 directly? |
| `mtr -c 100 198.51.100.50` | More detailed traceroute with statistics over many packets |

**Is this a fix or escalation?**
* **Not the user's fault.** They didn't cause the ISP to block this IP.
* **Escalate to the ISP.** Provide the IP, the domain, and the evidence: "My phone reaches this site on mobile data, but not on home Wi-Fi, which uses your ISP. The IP appears to be blocked."

**Message to the user:**

> The site is reachable from outside your ISP (confirmed via mobile data), but your ISP has blocked access to that IP. This is not a problem with the site or your network setup. Contact your ISP with this information: the IP `198.51.100.50` is blocked, but it's reachable from other carriers. Ask them to unblock it.

## 2. Hands-on Lab & Commands 
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
ping 1.1.1.1                           # public internet reachable?
dig [company-api].com                  # DNS resolves?
ping 198.51.100.50                     # specific IP reachable?
ping 8.8.8.8                           # can you reach other IPs?
curl -I https://[company-api].com      # application works?
curl -v https://198.51.100.50          # verbose to see where it hangs
traceroute -n 198.51.100.50            # where does the path end?
telnet 198.51.100.50 443               # can you connect to port 443?
mtr -c 100 198.51.100.50               # detailed traceroute with stats
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** if a second device on a different network (mobile data) can reach the site, the site is up. What looks like a "site down" is actually selective ISP blocking.
* **Blocker solved:** I initially said "it's probably not an ISP issue." The evidence (mobile data works, Wi-Fi doesn't) proves it *is* an ISP issue. The phone's Wi-Fi uses your ISP; mobile data uses a different carrier.
* **Pattern learned:** selective blocking (one IP blocked, others work) is different from a dead path (everything times out) or a port block (ping works, port fails).
* **Mistake to remember:** when the second device reaches a site via a different network, the site is definitely up. Look at what's between the user and the site, not the site itself.
* **Escalation evidence:** IP address, traceroute showing where it stops, and proof from another network that the site works.
* **Next:** a scenario where the destination is truly down, or where packet corruption appears along the path.