# Day 74 - Review: Re-reading Days 36-40
**Date:** 2026-10-29
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1 Hours 

## 1. Key Concepts Learned 

**Re-read the GitHub notes for Days 36-40, line by line.**

**Revisited:**
* **ASN and BGP (Day 36):** an ASN identifies an autonomous system, a network under one administration. BGP exchanges routes between ASNs. The ASN lookup via `whois.cymru.com` showed Google's ASN and the `/24` prefix it announces.
* **IGP versus EGP:** interior gateway protocols (such as OSPF) route within an organisation. Exterior gateway protocols (BGP) route between organisations. I worked this distinction out before being given the terms.
* **AAA (Days 37-38):** Authentication (who you are), Authorization (what you may do), Accounting (what you did). RADIUS is common for network access, and TACACS+ for controlling admin access to devices.
* **802.1X, MFA, and port security:** a device must authenticate before a switch port admits it, and MFA adds a second proof of identity.
* **SNMP and syslog (Day 40):** SNMP polls devices for data or receives traps. A MIB is a catalogue of available data, and OIDs address individual items in it. Syslog is devices pushing logs to a server, not the server pulling them.

**Still to firm up:**
* Stating IGP versus EGP in one sentence without a pause.
* Keeping SNMP polling and traps apart: polling is the manager asking, a trap is the device telling.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Review day, nothing run.
whois -h whois.cymru.com " -v 8.8.8.8"    # ASN and prefix lookup, from Day 36
log show --last 5m | head -50             # recent system log entries, from Day 40
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** Days 36-40 cover how routing crosses organisations (BGP and ASNs), how access is controlled (AAA and 802.1X), and how devices are watched (SNMP and syslog). These answer the "who, where, and what happened" questions behind real support tickets.
* **Blocker:** SNMP polling versus traps is the detail that blurs, so the note now pins it to who starts the conversation.
* **Next:** Day 75, the re-read of Days 41-44 (the four consolidation blocks), then the remaining Phase 1 review before Phase 2 starts around Day 91.