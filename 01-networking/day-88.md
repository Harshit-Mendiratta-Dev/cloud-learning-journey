# Day 88 - Reconsolidation: Re-reading Days 61-63 (Second Pass)
**Date:** 2026-11-15
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Second pass through the GitHub notes for Days 61-63, consolidating the concepts after the first re-read.**

**Reconsolidated:**
* **TLS failure is not a timeout (Day 61):** ping and TCP both succeed, so the layers below are working. The handshake is what fails. A changed issuer for the same site means something on the path is re-signing HTTPS.
* **Check the path before the site (Day 61):** if the certificate warning appears only on one network, the device on that network is the place to look: the router's security features, then installed profiles.
* **ARP is Layer 2 (Day 62):** it resolves an IP to a MAC on the same segment. Across networks, the Mac ARPs for the gateway's MAC, not the remote IP.
* **DHCP is Layer 7 over UDP (Day 62):** it runs as local broadcasts before the Mac has a usable IP, so it isn't Layer 3.
* **DHCP failure versus ISP failure (Day 62):** with DHCP broken, the gateway can't be reached. With the ISP link down, the gateway answers but `1.1.1.1` doesn't.
* **FIXIT (Day 63):** five steps, iterated: find the problem, inspect the symptoms, exclude possibilities, implement a fix hypothesis, and track the result. The three approaches (top-down, bottom-up, hybrid) set where you start in the OSI model.

**Still to firm up:**
* Saying in one line why a TLS failure is not a timeout.
* Naming the five FIXIT steps in order without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
curl -v https://<site> 2>&1 | grep -i issuer    # which CA signed the certificate (Day 61)
arp -a                                          # cached IP-to-MAC entries (Day 62)
ping -c 2 127.0.0.1                             # the layer 3 sanity check (Day 62)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 61-63 hold up on a second pass. The layer distinctions (TLS versus timeout, ARP versus DHCP) are the parts that stay useful.
* **Blocker:** a TLS failure can look like a connection problem from the outside. Checking whether TCP connects first separates the two.