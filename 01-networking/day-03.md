# Day 03 - Network Diagnostics & Port Auditing
**Date:** 2026-08-18
**Focus Area:** Phase 1 - Networking
**Time Spent:** 2.5 Hours

## 1. Key Concepts Learned (1.5h Theory)
## Core OS Differences
* **macOS vs. Linux `ping`:** On TryHackMe (Linux), you can use the `-4` flag to force IPv4 (`ping -c 3 -4 google.com`). On macOS, `ping` is strictly IPv4 by default, so the `-4` flag causes an error. 

## CLI Diagnostics Executed
* **Domain Reconnaissance (`whois`):** Queried top-level registrars to pull official registration data for domains like `facebook.com` and `microsoft.com`.
* **Targeted DNS Resolution (`dig`):** Ran `dig google.com @1.1.1.1` to bypass the local router's DNS and directly query Cloudflare's public resolver.
* **TLS/HTTP Inspection (`curl`):** Ran `curl -vI https://youtube.com` to inspect the raw Layer 7 headers, witnessing the TLS 1.3 handshake and a `301 Moved Permanently` redirect.

## Process Auditing (`lsof`)
Ran `lsof -i -P -n | grep LISTEN` to audit local Mac ports:
* **Case Sensitivity:** Used uppercase `-P` to show raw port numbers (e.g., `5000`) instead of service names.
* **Wildcard (`*`) Bindings:** Processes bound to `*` (like macOS Control Center) are listening on all network interfaces and can accept traffic from the local Wi-Fi network.
* **Loopback (`127.0.0.1`) Bindings:** Processes bound to localhost (like VS Code Helper) reject external traffic and only talk to the internal machine.

## 2. Hands-on Lab & Commands (1.0h Lab)
```bash
# Domain reconnaissance to pull official registrar records
whois facebook.com
whois microsoft.com

# Bypassed local router DNS to query Cloudflare's public resolver directly
dig google.com @1.1.1.1

# Inspected raw Layer 7 headers, TLS 1.3 handshakes, and 301 redirects
curl -vI [https://youtube.com](https://youtube.com)

# Audited local Mac processes bound to listening network ports
lsof -i -P -n | grep LISTEN
```

## 3. Key Takeaway / Blocker Solved

    macOS vs. Linux OS Differences: Solved a blocker where ping -4 crashed. Discovered macOS ping is hardcoded to IPv4 by default (requiring ping6 for IPv6), while TryHackMe's Linux environment uses the flags.

    CLI Case Sensitivity: Fixed an lsof syntax error by learning -p (lowercase) filters by Process ID, while -P (uppercase) reveals raw Port numbers.

    Security Bindings: Uncovered the difference between wildcard bindings (*, which are exposed to the local network) and loopback bindings (127.0.0.1, which securely restrict access to the local machine only).
