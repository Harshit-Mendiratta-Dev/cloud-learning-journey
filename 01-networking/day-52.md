# Day 52 - Troubleshooting Scenario: "Some Sites Work, Some Don't"
**Date:** 2026-10-07
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The scenario:** the user can browse Google and YouTube fine, but the company intranet times out.

**The measurements:**
* `ping 1.1.1.1`: Pass, ~8 ms, 0% loss
* `dig google.com`: Pass, ~50 ms
* `dig company.local`: Pass, returns `10.10.1.5` (internal IP)
* `curl -I https://google.com`: **Pass**, 200 OK, ~800 ms
* `curl -I https://company.local:8443`: **Timeout**
* `curl -I https://10.10.1.5:8443`: **Timeout**
* `ping 10.10.1.5`: **Timeout** (100% loss)
* `traceroute -n 10.10.1.5`: Hop 1 replies (~6 ms), hop 2 times out

**What the data tells me:**

| Result | What it means |
|---|---|
| Public internet (Google) works fine | Home Wi-Fi, router, and ISP are fine. |
| `dig company.local` succeeds | DNS works and the name resolves to an IP. |
| `curl` to IP and to name both timeout | It's not a DNS problem; the address isn't reachable. |
| Traceroute: hop 1 replies, hop 2 times out | The packet leaves the router but hits a barrier immediately. There's no path. |
| Other devices (phone) can't reach intranet either | It's a network-level issue, not device-specific. |

**The key learning: DNS success ≠ reachable.**
* `dig company.local` turns the name into an IP. That's all DNS does.
* It doesn't mean you can *reach* that IP. The address is private and on a separate network.

**What's actually happening:**
* `company.local` and `10.10.1.5` are on the company's private network, which is isolated from the public internet.
* From home, there is **no route** to private company IP space (`10.x.x.x`).
* Hop 2 times out because the router has no path to forward the packet to `10.10.1.5`.
* This is expected behavior, not a fault.

**The missing piece: a VPN.**
* A VPN creates a tunnel from your home machine into the company network.
* Once tunneled, your machine appears to be on the company network, with access to `company.local`.
* After the VPN is established, AAA (Days 37-38) authenticates you, checks your permissions, and logs the connection.

**Is this a problem to fix or escalate?**
* **Neither.** This is expected behavior. There's nothing broken.
* The user needs to connect to the company VPN to reach internal resources.

**Message to the user:**

> The company intranet is on a private internal network that's only reachable from inside the company or via a company VPN. Your home internet connection has no path to it—that's by design and secure. Connect to the company VPN, and you'll be able to access the intranet. If you don't have VPN credentials or access, contact your IT or network team.

**Layers and routing:**
* Layers 1-3 (Wi-Fi, IP, routing) are fine for public destinations.
* But routing tables don't have a path to private company IP space from home.
* A VPN adds that path by tunneling traffic into the company network.

## 2. Hands-on Lab & Commands 
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
ping 1.1.1.1                           # public internet reachable?
dig company.local                      # DNS resolves internal names?
dig google.com                         # public DNS works?
curl -I https://google.com             # public sites load?
curl -I https://company.local:8443     # internal site reachable?
curl -I https://10.10.1.5:8443         # by IP address?
ping 10.10.1.5                         # can we reach the IP at all?
traceroute -n 10.10.1.5                # where does the path stop?
```

## 3. Key Takeaway / Blocker Solved
* **Takeaway:** DNS resolves names to IPs. Routing gets packets to IPs. DNS working doesn't mean the address is reachable—the route has to exist.
* **Blocker solved:** I thought ARP or a VPN config issue was the cause. The real cause is the absence of a route, which is correct and secure.
* **Pattern learned:** selective failures (some sites work, some don't) can mean (a) the network is fine but a firewall is blocking one destination, (b) the destination is unreachable by design (like a private intranet from home), or (c) the destination itself is down.
* **Not a support ticket:** this is user education. The user needs to connect to the VPN, not for you to troubleshoot.
* **Next:** a scenario where a firewall is actually blocking a public destination, or where the destination is genuinely down.