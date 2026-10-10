# Day 55 - Troubleshooting Scenario: "One Public Site Blocked, Everything Else Works"
**Date:** 2026-10-10
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The scenario:** the user can browse Google and YouTube fine, but [streaming-site].com times out.

**The measurements:**
* `ping 1.1.1.1`: Pass, ~8 ms, 0% loss
* `dig [streaming-site].com`: Pass, returns public IP `203.0.113.45`
* `ping 203.0.113.45`: **Pass**, ~35 ms, 0% loss
* `curl -I https://[streaming-site].com`: **Timeout**
* `curl -I https://203.0.113.45`: **Timeout**
* `curl -I https://google.com`: Pass, 200 OK
* `traceroute -n 203.0.113.45`: Hops 1-3 reply (~6-12 ms), hop 4 times out
* Phone on home Wi-Fi: Can't access [streaming-site].com
* Phone on mobile data (Wi-Fi off): Can access [streaming-site].com fine

**What the data tells me:**

| Result | What it means |
|---|---|
| `ping` to destination works | The IP is reachable and responding to ICMP. |
| `curl` to destination times out | TCP port 443 (HTTPS) is blocked. |
| Hops 1-3 reply, hop 4 times out | A firewall rule is blocking the connection. |
| Phone on mobile data reaches the site | The destination is alive and the ISP doesn't block it. |
| Phone on Wi-Fi also can't reach it | The block is on the home network or router. |

**The key insight: ping works but application times out = port/protocol firewall block.**
* ICMP (used by ping) is allowed.
* TCP port 443 (HTTPS) is blocked by a firewall rule.
* This is different from a dead connection or destination problem.

**Where is the block?**
* Not the destination (phone on mobile data reaches it).
* Not the ISP (phone on mobile data reaches it).
* **The home network or home router:** something between the user's computer and the ISP is filtering port 443 for this specific domain.

**Possible causes:**
* Home router firewall rule blocking [streaming-site].com
* A corporate firewall appliance on the home connection (Cisco, Palo Alto, etc.)
* Parental control software blocking the site
* ISP-level filtering (but ruled out by the mobile data test)

**Diagnostic commands:**

| Command | What it shows |
|---|---|
| `curl -v https://203.0.113.45` | Verbose output shows where it hangs (TCP, TLS, or app layer) |
| `telnet 203.0.113.45 443` | Can you connect to port 443 at all? |
| `tcpdump -n -i en1 -c 5 -X host 203.0.113.45` | Capture the traffic; see if SYN packets are sent and what happens |

**Is this a fix or escalation?**
* **User education:** Tell the user the site is reachable from outside their network, but port 443 is blocked for it on their home network.
* **If user runs their own router:** Check the firewall rules and unblock it.
* **If it's the ISP:** Escalate with the evidence (phone on mobile data works, phone on Wi-Fi doesn't).

**Message to the user:**

> The streaming site is reachable from outside your network, but your home network or ISP has HTTPS traffic (port 443) blocked for that domain. If you manage your router, check the firewall rules. If this is an ISP block, contact them with this information: the site works on mobile data but not on your home Wi-Fi.

## 2. Hands-on Lab & Commands 
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
ping 1.1.1.1                          # public internet reachable?
dig [streaming-site].com              # DNS resolves?
ping 203.0.113.45                     # IP reachable?
curl -I https://[streaming-site].com  # application works?
curl -v https://203.0.113.45          # verbose to see where it hangs
telnet 203.0.113.45 443               # can you connect to port 443?
traceroute -n 203.0.113.45            # where does the path end?
tcpdump -n -i en1 -c 5 -X host 203.0.113.45   # capture traffic
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** ping works but the application times out = port/protocol firewall block, not a dead destination. The block is selective (ICMP allowed, TCP port 443 blocked).
* **Blocker solved:** I initially thought the destination was down or dropping connections. The phone on mobile data proved the destination is alive and the ISP isn't blocking it. The block is on the home network.
* **Pattern across scenarios:**
  - Day 49: number fails = network path broken.
  - Day 50: number works, name fails = DNS.
  - Day 51: number works slowly = degraded path.
  - Day 52: name resolves, number unreachable = no route (VPN needed).
  - Day 55: number works, port fails = firewall rule.
* **Mistake to remember:** don't assume the destination is the problem when phones on different networks can reach it. Use a second device as the deciding test.
* **Next:** a scenario where the destination is actually down, or packet loss/corruption in the path.