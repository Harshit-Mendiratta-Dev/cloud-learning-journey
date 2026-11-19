# Day 92 - Review: AAA and Hardening (Days 37-38)
**Date:** 2026-11-19
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Revisited the AAA and hardening material from Days 37-38.**

**AAA, the three functions:**
* **Authentication:** who you are (verify identity, for example with a password, certificate, or MFA).
* **Authorization:** what you're allowed to do once identified (which resources and commands).
* **Accounting:** what you did and when (a log of sessions and actions).

**Central control:** a network with many devices doesn't keep a separate login on each one. A central server handles AAA for all of them.

**RADIUS versus TACACS+:**
* **RADIUS:** common for network access, such as Wi-Fi and VPN. Combines authentication and authorization in one step.
* **TACACS+:** common for administrative access to network devices. Keeps authentication, authorization, and accounting separate, so you can control which commands an admin may run.

**802.1X (Day 38):** a port-based access control. A device must authenticate before a switch port or access point lets it onto the network. The three roles are the supplicant (the device), the authenticator (the switch or access point), and the authentication server.

**MFA:** a second proof of identity beyond a password.

**Port security:** limits which devices can connect to a physical port, for example by MAC address.

**VLAN segmentation as hardening (Day 26 link):** even after login, a user reaches only the segment they belong to.

**Basic hardening principles:**
* Turn off what you don't use.
* Give the least access needed.
* Change default settings.

**Still to firm up:**
* Stating the difference between RADIUS and TACACS+ in one line each.
* Naming the three 802.1X roles without a pause.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
sudo socketfilterfw --getglobalstate    # the Mac's host firewall state (Day 22)
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** AAA is three separate questions (who, what, and what did they do), and a central server answers them for every device. 802.1X applies the same idea at the port, before any traffic is allowed in.
* **Blocker:** RADIUS and TACACS+ blur, since both do AAA. The useful split is where each is used: RADIUS for network access, TACACS+ for admin access.