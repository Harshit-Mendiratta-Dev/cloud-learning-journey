# Day 48 - Troubleshooting Scenario Practice (169.254 / DHCP Failure)
**Date:** 2026-10-03
**Focus Area:** Phase 1 - Networking
**Time Spent:** 0.5 Hours 

## 1. Key Concepts Learned (0.5h Theory)
* **Scenario:** my Mac has IP `169.254.x.x` and the gateway ping fails. What does the IP tell me, and what do I check next?
* **`169.254.x.x` is link-local (APIPA), not a normal private address.** Private ranges are `192.168.x.x`, `10.x.x.x`, and `172.16-31.x.x`. A machine assigns itself a `169.254` address when it gets **no DHCP reply**. It means "no lease".
* **My reasoning (correct):** a `169.254` address means DHCP didn't happen, so the fault is in getting an address, not in the internet.
* **"Router is down" is one possible cause, not the only one:**
  * Router up, but its DHCP service not answering
  * Wi-Fi joined badly (wrong password, weak signal), so the request never reaches the router
  * Router or line actually down
  * My Mac's DHCP client misbehaving
* **Why my router-ping plan wouldn't work:**
  * `ipconfig getifaddr` shows *my* IP, not the router's.
  * With no DHCP lease, my Mac learned no gateway.
  * From a `169.254` address, pinging the router's usual IP will most likely give "No route to host", because I'm not on its subnet. A failed ping there proves nothing.

**Decision table for this scenario:**

| Step | Check | What the result tells me |
|---|---|---|
| 1 | Do other devices (phone) get online on the same Wi-Fi? | Yes = problem is my Mac. No = router or line. |
| 2 | Is the Mac joined to Wi-Fi? (`ifconfig en1 \| grep status`) | `active` = joined. Otherwise fix the Wi-Fi connection first. |
| 3 | Force a fresh lease (Wi-Fi off/on, or `sudo ipconfig set en1 DHCP`) | New IP = was a one-off DHCP failure. Still `169.254` = DHCP not answering. |
| 4 | Inspect the lease (`ipconfig getpacket en1`) | Shows whether a DHCP offer arrived at all. |
| 5 | If other devices also fail: reboot router, wait, retry | Fixes router-side DHCP or hardware problems. |

**Rule of thumb:** test with a second device *before* touching hardware. One check separates "my Mac" from "the router".

## 2. Hands-on Lab & Commands 
```bash
# Light theory/scenario day, nothing run. Commands to use in this scenario:
ifconfig en1 | grep status       # am I joined to Wi-Fi? (expect "active")
ipconfig getifaddr en1           # what IP do I have? (169.254.x.x = no lease)
sudo ipconfig set en1 DHCP       # force a fresh DHCP request (from Day 12)
ipconfig getpacket en1           # inspect the DHCP lease/offer details
```

## 3. Key Takeaway / Blocker Solved
* **Takeaway:** `169.254.x.x` = DHCP failed. Before acting, test with a second device to tell "my Mac" from "the router".
* **Correction learned:** a router ping can't diagnose this, because without a lease I'm not on the router's subnet.
* **Mistake to remember:** treating the first plausible cause (router down) as the only one. List possible causes, then test the cheapest first.
* **Next:** run this scenario for real if the chance comes up, or try another one (e.g. gateway pings fine but `1.1.1.1` fails).