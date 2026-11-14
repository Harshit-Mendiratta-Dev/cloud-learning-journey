# Day 87 - Review: Re-reading Days 61-63
**Date:** 2026-11-14
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 61-63, line by line.**

**Revisited:**
* **Day 61 (certificate warning on home Wi-Fi):** ping and TCP both work, but the TLS handshake fails. A certificate issuer that changes between networks for the same site means something on the path is re-signing HTTPS traffic. The bank isn't at fault, and DHCP renewal or an ISP call won't fix it. Check the router's security features and the device's installed profiles first.
* **Day 61 (layer lesson):** a TLS failure isn't a timeout. The layers below it are working, and the failure sits in the handshake.
* **Day 62 (layers refined):** ARP is Layer 2, resolving an IP to a MAC on the same segment. DHCP is a Layer 7 protocol over UDP, which runs as local broadcasts before the Mac has a usable IP.
* **Day 62 (DHCP versus ISP failure):** with DHCP broken, the gateway can't be reached. With the ISP link down, the gateway answers but `1.1.1.1` doesn't.
* **Day 63 (FIXIT video):** five steps, iterated: find the problem, inspect the symptoms, exclude possibilities, implement a fix hypothesis, and track the result. The three approaches (top-down, bottom-up, hybrid) are where you start in the OSI model.

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

* **Takeaway:** Days 61-63 added two things to the troubleshooting method: a way to read TLS failures as a layer-level fault, and a structured framework (FIXIT) to organise the work.
* **Blocker:** a TLS failure can look like a connection problem from the outside. Checking whether TCP connects first is what separates the two.