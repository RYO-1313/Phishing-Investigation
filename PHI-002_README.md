# 🎣 PHI-002 — Real Estate BEC: Reply-To Hijack

> **Path:** Phishing-Investigations / PHI-002

---

## 🧠 What This Investigation Is About

A suspicious email arrived appearing to come from a legitimate real estate business. The sender domains were old, clean, and registered through a reputable registrar — exactly the kind of email that slips past automated filters and reaches the inbox. The deception was hidden one layer deeper: the **reply-to address** pointed to a completely different domain, freshly registered in 2025 and flagged as phishing on VirusTotal.

The attached link directed victims to a file hosted on **Google Cloud Storage** — trusted infrastructure used deliberately to bypass URL reputation checks.

This investigation demonstrates how sophisticated phishing hides behind legitimate-looking sender chains, and why reply-to analysis is a non-negotiable step in any email triage.

**Verdict: True Positive — Further Investigation Required**

---

## 🖥️ Investigation Environment

**Tools Used:**

| Tool | Purpose |
|------|---------|
| [MXToolbox](https://mxtoolbox.com/) | Email header extraction and authentication checks |
| [AbuseIPDB](https://www.abuseipdb.com/) | Sender IP reputation |
| [DomainTools WHOIS](https://whois.domaintools.com/) | Domain registration and age lookup |
| [VirusTotal](https://www.virustotal.com/) | Domain and URL reputation across 70+ security engines |

---

## 🎯 SOC Relevance

**Reply-to hijacking** is one of the most effective phishing techniques against both users and automated filters. The email passes surface-level checks because the sending domain is legitimate — but any reply goes directly to the attacker. This is commonly used in **Business Email Compromise (BEC)** scenarios to intercept financial communications, redirect wire transfers, or harvest credentials under the cover of a trusted brand.

The use of **Google Cloud Storage** as a payload host is a deliberate evasion choice — many security tools trust Google infrastructure by default and will not flag a `storage.googleapis.com` URL without deeper inspection. Recognizing this evasion pattern is a high-value skill in modern SOC work.

---

## 📧 Email Profile

| Field | Value |
|-------|-------|
| From Domain | `jwgmedia.com` |
| Sender Domain | `ladelanoagency.com` |
| Reply-To | `brendamurphyrealestate.com` |
| Sender IP | `20.97.183.136` |
| Attached URL | `https://storage.googleapis.com/yxyltdgaoiqiztu/vbubnfvjsg.html#...` |

---

## 🔍 Investigation Walkthrough

### Step 1 — Header Extraction (MXToolbox)

Pulled the full email header through MXToolbox to assess authentication and routing.

**Authentication results:**

| Check | Result |
|-------|--------|
| SPF | ✅ Present |
| DKIM | ❌ None |
| DMARC | ❌ None |

> The presence of SPF is what makes this email dangerous — it passes the first automated filter most mail servers apply, which means it reaches the inbox instead of spam. But SPF alone only verifies that the sending server is authorized for that domain. Without DKIM (which signs the message content) and DMARC (which sets policy for failures), there is no integrity check on the message itself and no enforcement if something is wrong.

📸 `[SCREENSHOT — MXToolbox header showing SPF pass, DKIM and DMARC absent]`

---

### Step 2 — Sender IP Analysis (AbuseIPDB + VirusTotal)

Looked up the originating IP `20.97.183.136`.

**Findings:**
- No previous abuse reports on AbuseIPDB
- No flags on VirusTotal

📸 `[SCREENSHOT — AbuseIPDB and VT results for 20.97.183.136 showing clean status]`

> A clean IP with no history is consistent with legitimate infrastructure — but in this case it is also consistent with a compromised or freshly provisioned sending account. The IP alone clears, which is exactly why the investigation must go deeper.

---

### Step 3 — Sender Domain Analysis (WHOIS + VirusTotal)

Investigated both sending domains independently.

**`jwgmedia.com`:**
- Created: 2015
- Registrar: GoDaddy
- VirusTotal: Clean

**`ladelanoagency.com`:**
- Created: 2012
- Registrar: GoDaddy
- VirusTotal: No data

📸 `[SCREENSHOT — WHOIS results for both domains showing registration dates]`

> Both domains are old and registered with a major, reputable registrar. On the surface this looks completely legitimate — and that is the point. Attackers either compromise existing business accounts or craft emails that spoof the display name while routing replies elsewhere. The sender chain is designed to pass inspection.

---

### Step 4 — Reply-To Domain Analysis (VirusTotal + WHOIS)

**This is where the investigation breaks open.**

Investigated the reply-to domain: `brendamurphyrealestate.com`

| Field | Finding |
|-------|---------|
| Created | 2025-06-24 — **less than one year old** |
| VirusTotal | 🔴 **Flagged as phishing (Fortinet engine)** |

📸 `[SCREENSHOT — VirusTotal result for brendamurphyrealestate.com showing phishing flag]`

📸 `[SCREENSHOT — WHOIS showing domain creation date of 2025]`

> This is the attack. The sender domains are real and old — they establish trust. But the moment a victim replies, their response goes to `brendamurphyrealestate.com` — a domain registered weeks or months ago, already flagged for phishing. In a BEC scenario, that reply could contain payment details, contract information, or login credentials. The attacker reads it, the victim never knows.

---

### Step 5 — URL Analysis (VirusTotal)

Submitted the attached Google Cloud Storage URL to VirusTotal.

**Result:** Clean across all engines.

📸 `[SCREENSHOT — VirusTotal URL scan showing clean result for googleapis.com link]`

> The URL scanning clean is expected and is itself a red flag in context. Attackers deliberately host payloads on trusted platforms — Google Cloud Storage, OneDrive, Dropbox — because those domains have strong reputations and most security tools will not block them. The URL being clean does not mean the content is safe. In this case, the link leads to an HTML file with a randomized name on a freshly created storage bucket — a pattern consistent with credential harvesting pages. Full detonation in a sandbox environment is required before this can be ruled out.

---

## 🚩 Red Flags Summary

| # | Red Flag | Severity |
|---|----------|----------|
| 1 | Reply-to domain flagged as phishing on VirusTotal | 🔴 High |
| 2 | Reply-to domain created in 2025 — extremely new | 🔴 High |
| 3 | No DKIM or DMARC | 🟠 Medium |
| 4 | Reply-to domain completely different from sender domains | 🟠 Medium |
| 5 | Payload hosted on Google Cloud Storage to evade URL filters | 🟠 Medium |
| 6 | Sender domain `ladelanoagency.com` has no VT data — unverifiable | 🟡 Low |

---

## 🧩 MITRE ATT&CK Mapping

| Field | Value |
|-------|-------|
| Tactic | Initial Access |
| Technique | T1566.002 — Phishing: Spearphishing Link |
| Sub-technique | T1036 — Masquerading (legitimate sender chain) |
| Evasion | T1027 — Obfuscated Files or Information (trusted cloud hosting) |

---

## ✅ Verdict & Escalation

| Field | Value |
|-------|-------|
| Classification | **True Positive** |
| Confidence | High |
| Action Taken | Flagged for further investigation |
| Rationale | The sender chain passes surface-level checks but the reply-to domain is confirmed phishing. The Google Cloud Storage payload requires sandbox detonation before full scope can be determined. This case warrants deeper analysis — possible BEC campaign targeting the real estate sector. |

---

## 📋 Key Takeaways

- **Always check the reply-to. Always.** The entire deception in this email lives in one field that many analysts skip when the sender looks clean. Reply-to hijacking is low-effort for an attacker and high-impact — the sending infrastructure stays legitimate and unblocked while the attacker collects all the replies.
- **Domain age is a fast, high-value signal.** A reply-to domain created weeks before a phishing email arrives is not a coincidence. Building the habit of checking registration dates on any domain that appears in an email header takes seconds and catches this technique every time.
- **Trusted infrastructure does not mean trusted content.** Google Cloud Storage, OneDrive, and similar platforms are used by attackers precisely because they are trusted. A URL passing VirusTotal clean is not clearance — when the surrounding context is already suspicious, a clean URL scan means the payload needs a sandbox, not a sign-off.

---

*by [ryo](https://github.com/RYO-1313)*
