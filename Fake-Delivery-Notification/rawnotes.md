# Phishing Case — "Fake Delivery Notification (Chronopost Impersonation)"

**Verdict:** True Positive — Low Risk (Recon / Bulk Spam Campaign)

**Severity:** Low

**Date Received:** 2026/05/18

**Date Analyzed:** 2026/08/11

**Analyst:** Youssef Touhami

## Summary

An EDR/mail gateway alert fired on an inbound email impersonating **Chronopost** (a French parcel carrier), designed to look like a "Global Tracking System / Logistics Express" delivery notice. The email claims an incorrect address or unpaid customs fee (€0.99) is blocking delivery, with a hold period expiring in 14 hours, pressuring the recipient to click through. The message contains no credential-harvesting form or malicious attachment — it uses tracked links (open/click/unsubscribe) consistent with a bulk phishing/spam kit, most likely a reconnaissance attempt to confirm the target mailbox is active.

## Detection Source

EDR/mail gateway alert.

## Timeline

| Time | Event |
|-|-|
| 2026/05/18 11:05 UTC | Email delivered (per header timestamps) |
| 2026/05/18 — | User reported / alert fired |
| 2026/05/18 — | Triage started |
| 2026/05/18 — | Verdict reached |
| 2026/05/18 — | Containment action taken |
| 2026/05/18 — | Case closed |

> **Note:** Original draft listed three different dates (2026/04/18, 2026/07/26, and no header cross-check). The `Date:` header and `X-MS-Exchange-Organization-ExpirationStartTime` header both confirm **18 May 2026 11:05 UTC** — use this consistently. Fill in real alert-system timestamps for the blank rows if available.

## Scope

* **Recipients affected:** 1 mailbox
* **Note:** delivered to `phishing@pot`, a test/honeypot address — not a live production mailbox. Worth stating explicitly so scope isn't misread as real user impact.
* **Other reports of same campaign:** no

## Email Overview

* **From (header):** `Chronopost 📦 <no-reply@odzahquwvpiypw>` — impersonates Chronopost; From-domain has no valid TLD (malformed/spoofed)
* **Sender (envelope):** `CNPG496KBEXEY9TS4LF28AAI@SNDQJO8F3TKXD4.com`
* **Originating host (HELO):** `momf.fioridigusto.it` (20.24.45.102)
* **Reply-To:** `reply-CNPG496KBEXEY9TS4LF28AAI@in2.getdrip.com` (Drip — legitimate email marketing platform being abused for reply routing)
* **Subject:** Votre colis est bloqué au centre
* **Delivered to:** phishing@pot
* **Attachments/Links:** No attachments; 3 tracked HTML links (open pixel, click-through, unsubscribe)
* **IP address:** 20.24.45.102
* **SCL (Spam Confidence Level):** 9/9 — Microsoft Exchange's own filter already scored this at max confidence

## Header Analysis

**Authentication:** SPF = none, DKIM = none, DMARC = none, action = none. No authentication passes — consistent with a spoofed/unauthenticated sender.

**Received chain:**
1. `momf.fioridigusto.it` (20.24.45.102) → `DB1PEPF000509FE.eurprd03.prod.outlook.com` *(entry into Microsoft's front-end/EOP)*
2. → `DB9PR02CA0019.outlook.office365.com`
3. → `DM3PPFA09EE1970.namprd10.prod.outlook.com`
4. → `MW4PR10MB5749` *(final delivery to mailbox)*

**Spoofing indicators:**
- From-header display name ("Chronopost 📦") impersonates a real carrier, but the actual From-domain (`odzahquwvpiypw`) is malformed — no TLD, clearly not Chronopost's real domain.
- Envelope Sender domain (`SNDQJO8F3TKXD4.com`) is a randomly generated string, not tied to any real brand.
- Fake urgency: tracking reference `#FR-9822-TK`, 14-hour hold period, small "customs fee" (€0.99) — classic low-friction phishing lure amount.

## Sender / IP Reputation

20.24.45.102 resolves to Microsoft Corporation infrastructure (cloud-hosted), suggesting the actual delivery hop used Azure-adjacent infrastructure rather than a dedicated malicious host — common for bulk spam/marketing campaigns riding on legitimate cloud providers. No prior abuse reports on this IP.

## URL / Attachment Analysis

All three links share identical campaign parameters (`pid=1338_rd`, `uid=20`, `cmpid=0`, `ofid=5114`, `lid=466`, `cid=6983baaef125cc1528f7e2f6`) — a pattern consistent with a templated bulk phishing/spam-tracking kit rather than a one-off page.

| Purpose | URL | Notes |
|-|-|-|
| Open tracking (pixel) | `http://kyempapu.org/track/?act=op&...` | Invisible 1x1 image, fires when email is opened |
| Click-through | `http://kyempapu.org/?act=cl&...` | Main redirect, used by all body buttons/links |
| Unsubscribe | `http://kyempapu.org/?act=un&...` | Standard unsubscribe-style link |

* **Redirect chain:** Not fully traced — recommend following `act=cl` to confirm final landing page behavior.
* **Final payload/page behavior:** Tracking / lead confirmation (recon), based on kit structure — no confirmed credential form.
* **Sandbox detonation notes:** Not performed.
* **Credential submission tested:** No.

## Indicators of Compromise (IOCs)

| Type | Value | Notes |
|-|-|-|
| IP | 20.24.45.102 | Resolves to Microsoft Corp infrastructure; no VT/AbuseIPDB hits |
| Domain (HELO/origin) | momf.fioridigusto.it | Actual sending host per Received chain — not previously logged, needs separate reputation check |
| Domain (envelope sender) | SNDQJO8F3TKXD4.com | Randomly generated string; not in VT/AbuseIPDB |
| Domain (From header) | odzahquwvpiypw | Malformed/no TLD — spoofed identity |
| Domain (Reply-To) | in2.getdrip.com | Legitimate Drip marketing platform, abused for reply routing |
| URL | http://kyempapu.org/?act=cl | Flagged by G-Data and BitDefender as phishing; 3 other vendors flag as spam/suspicious |
| URL | http://kyempapu.org/track/?act=op | Open-tracking pixel, same domain/kit |
| File hash | — | No attachment present |

## MITRE ATT&CK Mapping

| Technique | ID |
|-|-|
| Phishing: Spearphishing Link | T1566.002 |

## Containment / Response Actions

* [ ] Email quarantined/deleted org-wide
* [X] Sender domain/IP blocked
* [ ] Malicious URL blocked at proxy/firewall
* [ ] User(s) contacted / password reset if credentials entered
* [X] Escalated to IR

## Conclusion

True Positive, low-risk phishing/spam impersonating Chronopost via a bulk delivery-notification lure. No credential-harvesting form or malicious payload present. Authentication fully fails (SPF/DKIM/DMARC none) and Microsoft's own filter already scored this at maximum spam confidence (SCL 9). The tracked open/click/unsubscribe link structure indicates a templated spam/phishing kit, and the embedded tracking pixel/link likely serves as a recon mechanism to confirm the target mailbox is active. Recommend escalating the origin domain (`momf.fioridigusto.it`) and envelope-sender domain (`SNDQJO8F3TKXD4.com`) for independent reputation review, in addition to the already-blocked IP.
