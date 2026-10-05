# Day 50 - Troubleshooting Scenario: "Ping Works, But Websites Won't Load"
**Date:** 2026-10-05
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.0 Hours 

## 1. Key Concepts Learned 

**The scenario:** the user is connected but the browser can't open anything.

| Command | Result |
|---|---|
| `ping 127.0.0.1` | Pass |
| `ipconfig getifaddr en1` | Normal address (`192.168.x.x`) |
| `ping <own IP>` | Pass |
| `ping <gateway>` | Pass, ~6 ms |
| `ping 1.1.1.1` / `ping 8.8.8.8` | Pass |
| `dig +short google.com` | **Fails**: timed out, no servers reached |
| `dig +short google.com @1.1.1.1` | **Pass**: returns IPs |
| `curl -I https://google.com` | **Fails**: could not resolve host |
| `curl -I https://1.1.1.1` | **Pass**: response received |

**Where the fault is:**
* Layers 1-3 ruled out (IP, gateway, internet pings all pass).
* **Layers 4-6 also ruled out:** `curl` to `1.1.1.1` succeeded, which needs a TCP connection (L4) and a TLS handshake (L5-6).
* **The fault is in Layer 7, in the DNS part.** DNS is an application-layer service. HTTP itself works, but name resolution doesn't.
* **Mistake to remember:** I first placed the fault in Layers 4-6 and said Layer 7 was ruled out. Layer 7 is exactly where it is.

**What `dig @server` tells me that plain `dig` can't:**
* Plain `dig` asks the DNS server my Mac is *configured* to use. `@1.1.1.1` asks one I choose.
* `dig` talks to the server directly; it doesn't use the Mac's own cache the way a browser does.
* A different server answering means: the internet is fine, my Mac's connection is fine, and the **DNS server my Mac is configured to use is the broken part**.

**Where my DNS server setting comes from (Day 12 tie-in):**
* DHCP hands out an IP, a gateway, **and a DNS server** in the same lease. It's usually the router itself, which forwards to the ISP's DNS.
* It can also be overridden manually, and each interface (Wi-Fi/Ethernet) can have its own DNS.

**Possible causes (list several, test the cheapest first):**
* The router's DNS forwarding stopped working
* The ISP's DNS server is down or slow
* A manually set DNS on the Mac that's wrong or dead
* DHCP handed out the wrong DNS server
* A VPN or profile overriding DNS
* Something blocking DNS traffic (port 53)

**Test order:**

| Step | Check | What it tells me |
|---|---|---|
| 1 | `dig @1.1.1.1 google.com` (already done) | Internet fine, my configured DNS is the problem |
| 2 | See which DNS the Mac uses (`scutil --dns`) | Is it the router, the ISP, or a manual entry? |
| 3 | `dig +time=2 +tries=1 google.com @<gateway-ip>` | Is the router's DNS answering? |
| 4 | Phone on the same Wi-Fi: do sites load? | Phone also fails = router/ISP DNS. Only Mac = local setting. |
| 5 | Check for VPN or custom DNS settings | Local override as the cause |

**Quick workaround (separate from the real fix):**
* Set the Mac's DNS manually to `1.1.1.1` or `8.8.8.8` (System Settings > Wi-Fi > Details > DNS). The user can browse immediately.
* It's only a workaround: the router/ISP DNS is still broken, and the root cause needs fixing separately.

**Direction rule, Day 49 vs Day 50:**
* **Day 49:** `1.1.1.1` **failed** → fault is *past the router* → look **outward** (router-to-ISP link).
* **Day 50:** `1.1.1.1` **passed** → the whole path is fine → look **up**, at name resolution (DNS).
* **Rule:** a number works but a name fails = DNS. A number fails = network path.

## 2. Hands-on Lab & Commands 
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
dig +short google.com                              # uses my configured DNS
dig +short google.com @1.1.1.1                     # uses a DNS server I choose
dig +time=2 +tries=1 google.com @<gateway-ip>      # is the router's DNS answering?
curl -I https://google.com                         # by name (needs DNS)
curl -I https://1.1.1.1                            # by IP (skips DNS)
scutil --dns | head -20                            # DNS servers the Mac uses
ipconfig getpacket en1 | grep domain_name_server   # DNS handed out by DHCP
```

## 3. Key Takeaway / Blocker Solved
* **Takeaway:** number works, name fails = DNS. Compare plain `dig` with `dig @1.1.1.1` to prove it's my configured DNS server, not the internet.
* **Blocker solved:** I'd placed the fault in Layers 4-6. `curl` to an IP proves L4-6 work, so the fault is the DNS part of L7.
* **Contrast with Day 49:** the same first suspect (DNS) was *wrong* then and *right* now. The evidence decides, not the familiarity.
* **Mistake to remember:** I didn't know a workaround. Setting DNS manually gets the user browsing while the real fix is pursued.
* **Next:** a new scenario type, e.g. "some sites load but others don't", or "everything is slow" (latency and packet loss, `ping` statistics, `traceroute`).