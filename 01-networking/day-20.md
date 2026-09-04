# Day 20 - WHOIS: A Typo That Taught Domain Security
**Date:** 2026-09-04
**Focus Area:** Phase 1 - Networking (Light Day)
**Time Spent:** ~1 Hour

## 1. Key Concepts Learned

* **`whois` — domain registration lookup, different from DNS:** Where `dig`/`host`/`nslookup` (Day 10) tell you how a name resolves to an IP, `whois` tells you *who registered* the domain, when, and through which registrar. Different question entirely — registration vs. resolution.

* **WHOIS follows the same hierarchy pattern as DNS resolution (Day 10):**
  1. **IANA** — the very top, doesn't know domain details, just refers you to the right registry (`refer: whois.verisign-grs.com` for any `.com` domain)
  2. **Verisign** (the `.com` registry operator — same `gtld-servers.net` servers seen in Day 10's `dig +trace`) — knows creation/expiry dates, refers to the actual registrar
  3. **The registrar** (e.g., MarkMonitor) — has the full registrant details: organization, address, contact info, name servers

* **A typo (`linkdin.com` instead of `linkedin.com`) accidentally surfaced a real security practice — defensive domain registration / typosquatting protection:** Companies deliberately register common misspellings of their own domain specifically so attackers can't grab them for phishing. Confirmed directly: `linkdin.com` is registered by LinkedIn Corporation itself, same registrar (MarkMonitor) and same DNS providers as the real domain.

* **Creation date comparison told a clear story:** `linkedin.com` was registered in **2002** (around the company's actual founding), while `linkdin.com` wasn't registered until **2010** — confirming the misspelling was grabbed defensively, years later, once the company had grown into a real target, not proactively on day one.

* **`client...Prohibited` vs. `server...Prohibited` domain status locks:**
  - `client*Prohibited` = a protection lock requested by the *registrant* — they can remove it themselves
  - `server*Prohibited` = a stronger lock set by the *registry* itself — harder to remove, typically reserved for higher-value/primary domains
  - The real `linkedin.com` had *both* sets of locks (6 total); the defensive typo domain `linkdin.com` only had the client-side locks (3) — makes sense, since the primary domain actually serving traffic gets the maximum protection tier.

* **DNS provider redundancy:** LinkedIn uses two separate DNS providers simultaneously — NS1 (`p09.nsone.net`) and Azure DNS (`azure-dns.com/net/org/info`) — on both domains, so if one provider has an outage, resolution still works via the other.

## 2. Hands-on Lab & Commands
```bash
whois linkdin.com     # typo — turned out to be a real, separately-registered domain
whois linkedin.com    # correct spelling — direct comparison
```

## 3. Key Takeaway / Blocker Solved
No blocker — a deliberately light day that turned into a genuinely useful discovery. An accidental typo led directly into real-world domain security concepts (typosquatting defense, registry-level vs. registrant-level protection locks, DNS provider redundancy) without any of it being planned in advance. Good example of curiosity-driven exploration surfacing real concepts organically, same pattern as Day 6's MX record discovery via `host`. Firewalls remain on track for Saturday.