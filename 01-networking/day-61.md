# Day 61 - Troubleshooting Scenario: "Works on Mobile, Certificate Warning on Home Wi-Fi"
**Date:** 2026-10-16
**Focus Area:** Phase 1 - Networking
**Time Spent:** 1.5 Hours 

## 1. Key Concepts Learned 

**The scenario:** a banking site loads on mobile data, but shows "Your connection is not private" on home Wi-Fi.

**The measurements:**

| Check | Result |
|---|---|
| `ping 1.1.1.1` | Pass, ~8 ms, 0% loss |
| `dig bank.example.com` | Pass, returns `203.0.113.80` |
| `ping 203.0.113.80` | Pass, ~35 ms, 0% loss |
| `curl -I https://bank.example.com` | Fails: certificate verification error |
| `curl -I http://bank.example.com` | Pass, 301 redirect to HTTPS |
| `curl -v https://bank.example.com` | TCP connects, TLS handshake fails at certificate verification |
| Phone on home Wi-Fi | Same warning |
| Phone on mobile data | Loads, valid certificate |
| Mac on mobile data (hotspot) | Loads, valid certificate |

**Certificate issuers:**
* Mobile data: `Example Trust CA`
* Home Wi-Fi: `Home-Router Local CA`

**The layers:**
* Layers 1-4 work. Ping and the TCP connection both succeed.
* The failure is at **TLS**, between the transport layer and the application (Layers 5-6).
* A timeout would mean the connection never formed. Here it forms, and the TLS check rejects the certificate.

**The key insight: a different issuer for the same site means something is re-signing HTTPS traffic.**
* The bank's certificate should always come from the same issuer.
* On home Wi-Fi, a device on the path replaces it with a certificate from `Home-Router Local CA`.
* Devices only trust that CA if it's installed on them, so the warning is correct behavior for an untrusted certificate.

**Possible causes (in the path, not the bank):**
* HTTPS inspection or security features on the home router
* Parental controls or security software on the router or a device
* A transparent proxy
* Malware on the network (less likely, but worth ruling out)

**What won't fix it:**
* Renewing the DHCP lease or rebooting: these change IP and link state, not certificates.
* Escalating to the ISP first: the evidence points to the router.

**Tests that separate the possibilities:**

| Test | What it shows |
|---|---|
| `curl -v https://bank.example.com 2>&1 \| grep -i issuer` | Which CA signed the certificate |
| Router admin page: HTTPS inspection, security, parental controls | Whether a feature is re-signing traffic |
| Mac: System Settings > Privacy & Security > Profiles | Installed profiles or certificates |
| Phone on home Wi-Fi with VPN off | Whether the warning persists outside the Mac |

**The response to the user:**
* Don't tell them to ignore the warning.
* Tell them a device on their home network is replacing the bank's certificate, which is a security concern to check before entering credentials.
* Check the router first. Escalate to the ISP only if the router is clean.

## 2. Hands-on Lab & Commands (0h Lab)
```bash
# Scenario/theory day, nothing run. Commands for this scenario:
ping -c 2 1.1.1.1                                  # internet reachable?
dig bank.example.com                               # DNS resolves?
curl -I http://bank.example.com                    # plain HTTP redirects?
curl -v https://bank.example.com                   # where the TLS handshake fails
curl -v https://bank.example.com 2>&1 | grep -i issuer   # which CA signed it?
```

## 3. Key Takeaway / Blocker Solved

* **Takeaway:** when TCP works but TLS fails, the connection is fine and the certificate is wrong. A changed issuer for the same site means interception.
* **Blocker solved:** I first attributed the certificate to the ISP. The issuer changing between networks points to a device on the home path, not the ISP.
* **Layer lesson:** a TLS failure isn't a timeout. The layers below it are working, and the failure is in the handshake.
* **Mistake to remember:** don't say "the application is fine" when the application never got a usable connection. Name the layer where it actually fails.
* **Next:** a scenario where the certificate is expired on the server side (a real destination fault), to contrast with interception.