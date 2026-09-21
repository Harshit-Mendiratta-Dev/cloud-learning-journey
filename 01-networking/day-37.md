# Day 37 - AAA Framework & Network Hardening Basics (Part 1)
**Date:** 2026-09-21
**Focus Area:** Phase 1 - Networking
**Time Spent:** ~45 Minutes

## 1. Key Concepts Learned
* **AAA:** Authentication (who you are), Authorization (what you're allowed to do), Accounting (logging what actually happened) — the standard framework network admins use to safeguard access and maintain a trail for tracing back issues or malicious activity.
* **RADIUS** (combines Authorization + Accounting, common for Wi-Fi login) vs. **TACACS+** (keeps all three separate, common for network device admin access).
* **Hardening basics:** disable unused ports/services (ties to Day 22's firewall/ACL work), never leave default credentials unchanged, apply least privilege.
* Anchored to own Mac: password/Touch ID (Authentication), admin vs. standard account privileges (Authorization), background system logs (Accounting).

## 2. Hands-on Lab & Commands
None — theory only, compressed session.

## 3. Key Takeaway / Blocker Solved
No blocker. Correctly summarized AAA as the standard procedural framework admins enforce for access control and audit trails — good, accurate grasp despite the short session. Flagged as **Part 1** — deeper follow-up (802.1X, MFA, port security, certificate-based authentication, VLAN-based segmentation) planned for the next session, since the full topic is genuinely larger than a 25-minute pass could cover.