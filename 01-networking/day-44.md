# Day 44 - Retention Check: Days 31-40 Review
**Date:** 2026-09-28
**Focus Area:** Phase 1 - Networking (Consolidation)
**Time Spent:** ~1.5 Hour

## 1. Key Concepts Learned (Review Format, No New Theory)

Self-directed re-read of Days 31-40 notes, followed by an 8-question no-notes quiz covering WAN technologies, dynamic routing, BGP/ASN, AAA, and monitoring.

**Score: ~7.2/8**

* **Leased line / MPLS / SD-WAN** — fully correct. MPLS's "label as a ticket pinning a packet to a pre-determined route" was accurate as stated.
* **RIP vs. OSPF** — correct, with one wording fix: OSPF's cost is derived from a link's **bandwidth capacity**, not current usage or measured travel time.
* **AS and BGP's role, including why business relationships matter** — fully correct.
* **IGP vs. EGP** — fully correct (RIP/OSPF as IGPs, BGP as the EGP).
* **ASN registry vs. live BGP configuration — the one real gap:** BGP configuration living on border routers (typically alongside an IGP like OSPF) was correct, but ASN registration data was attributed to "root servers." It actually lives in the **Regional Internet Registries (RIRs)**: ARIN, APNIC, RIPE NCC, LACNIC, AFRINIC, delegated from IANA. Root servers are the separate DNS concept from Day 10.
* **802.1X vs. firewall/ACL** — fully correct ("before entry" gate vs. "after entry" traffic rules).
* **MFA's three categories and why layering different ones works** — fully correct.
* **SNMP / syslog** — correct on polling vs. traps and syslog's pattern-spotting purpose, with two wording fixes: SNMP is Simple Network **Management** Protocol (not "Monitoring"), and syslog is **push-based** (devices send logs, the server doesn't pull them).

## 2. Hands-on Lab & Commands
None today, since this was a pure review and recall session.

## 3. Key Takeaway / Blocker Solved

No blocker. One real gap (ASN data lives in the RIRs, not DNS root servers) and a few wording-precision fixes, all resolved immediately. This completes the three-part consolidation pass across Days 1-40: 7.5/8, 7/8, 7.5/8, and ~7.2/8 for the four blocks. Recurring items to keep drilling: NAT/PAT means Network/Port Address **Translation** (not Protocol), and SNMP means Simple Network **Management** Protocol. Foundation confirmed solid before starting the deferred formal troubleshooting methodology week.