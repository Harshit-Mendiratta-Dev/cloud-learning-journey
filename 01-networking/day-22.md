# Day 22 - Firewalls: Hands-On with macOS Application Firewall
**Date:** 2026-09-06
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2 Hours

## 1. Key Concepts Learned (1.0h Theory)

* **Stateful vs. stateless firewalls (not covered in yesterday's beginner video, added today):**
  - **Stateless** — judges every packet in isolation, no memory of prior traffic
  - **Stateful** — remembers active connections; if an outgoing SYN was already approved, the matching reply is automatically let back in without re-checking every rule from scratch
  - Almost every modern firewall (macOS's included, and AWS Security Groups later in Phase 2) is stateful — this is *why* Day 13's TCP handshake and Day 18's Nmap scans worked normally without manually opening return-traffic rules.

* **macOS's Application Firewall works at the process/application level, not just by raw port number** — a rule is tied to a specific program (e.g., `sshd-keygen-wrapper`), deciding whether *that program* may accept incoming connections. This differs from a traditional network firewall or AWS Security Groups, which filter purely by IP/port/protocol regardless of which process owns the connection — both are "firewalls," just operating at different layers of decision-making.

* **ACLs made concrete:** Each entry in `--listapps` is a real, live Access Control List rule — a specific program, paired with an Allow/Block decision. Directly connects yesterday's abstract "ACL" concept from the PowerCert video to an actual, editable rule table on this machine.

* **Defense-in-depth reasoning:** On a home network, the router's NAT (Day 14) already blocks most unsolicited incoming internet traffic — so the Application Firewall acts as a *second* layer, not the only one. On an untrusted network (public Wi-Fi, etc.), where other devices can reach your Mac directly at Layer 2/3 (same mechanism as Day 14's `rapportd` link-local capture), the Application Firewall becomes the *primary* defense.

* **Important discovery:** macOS ships with the Application Firewall **disabled by default** — confirmed directly (`State = 0`) before making any changes today.

## 2. Hands-on Lab & Commands (1.0h Lab)
```bash
# Check current firewall state
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate

# Turn firewall on
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on

# List every app with a firewall rule (the actual ACL)
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --listapps

# Block a specific service (SSH), watch the rule flip live
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --blockapp /usr/libexec/sshd-keygen-wrapper
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --listapps

# Clean up — restore original state exactly as found
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --unblockapp /usr/libexec/sshd-keygen-wrapper
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate off
```

## 3. Key Takeaway / Blocker Solved

No blocker — clean session start to finish. Discovered the Mac's built-in Application Firewall was off by default, found a genuinely readable 8-entry ACL (`remoted`, `python3`, `ruby`, `cupsd`, `sharingd`, `sshd-keygen-wrapper`, `smbd`, `com.apple.universalcontrol`), and watched a rule flip live from Allow to Block by targeting SSH specifically — directly tying yesterday's abstract firewall/ACL theory to a real, editable rule table.

Several entries connected straight back to earlier days: `smbd` (port 445, Day 6's ports table, and one of the "filtered" ports seen on remote hosts during Day 18's Nmap scans), `sshd-keygen-wrapper` (port 22, same port seen open on `scanme.nmap.org`), and `sharingd` (likely related to the Continuity/AirDrop traffic directly observed in Day 14's `lsof` capture). Cleaned up fully at the end — firewall and SSH rule restored to their exact original state.

This closes out the last major gap in Phase 1's "how does traffic flow, and how does it get allowed or blocked" story — addressing, transport, DNS, DHCP, TCP, NAT/PAT, port scanning, and now firewalls/ACLs are all covered with real, hands-on evidence.