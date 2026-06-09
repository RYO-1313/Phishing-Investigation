# 🎣 Phishing Investigations

![Investigations](https://img.shields.io/badge/Investigations-3-red?style=for-the-badge&logo=mail&logoColor=white)
![Tools](https://img.shields.io/badge/Tools-MXToolbox%20%7C%20VT%20%7C%20AbuseIPDB-blue?style=for-the-badge)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-Mapped-red?style=for-the-badge&logo=target&logoColor=white)
![Focus](https://img.shields.io/badge/Focus-SOC%20%7C%20Email%20Analysis-darkblue?style=for-the-badge&logo=shield&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

A hands-on phishing analysis lab where I investigate real-world phishing email samples end-to-end — extracting headers, profiling sender infrastructure, analyzing attached URLs, and delivering a triage verdict with escalation recommendation.

Every email investigated in this repository is sourced from [**Phishing Pot**](https://github.com/rf-peixoto/phishing_pot/tree/main) — an open collection of real phishing samples captured via honeypots, maintained for researchers and detection developers.

---

## 🎯 What This Lab Demonstrates

Each investigation follows the same workflow a SOC analyst applies when a suspicious email hits the queue: header analysis, sender IP and domain reputation checks, URL detonation, authentication verification, and a final verdict with documented rationale.

The goal is not just to find the red flags — it's to build a complete, reproducible picture of how the attack was constructed and why it should be escalated.

---

## 🔬 Investigation Methodology

Every case in this repo follows this triage process:

| Step | Action | Tool |
|------|--------|------|
| 1 | Extract and parse email headers | MXToolbox |
| 2 | Check sender IP reputation and geolocation | AbuseIPDB |
| 3 | Verify domain registration age and ownership | WHOIS.com |
| 4 | Analyze URLs, domains, and attachments | VirusTotal |
| 5 | Document red flags, MITRE mapping, and escalation decision | — |

---

## 📂 Investigations

| # | Case | Type | Verdict |
|---|------|------|---------|
| PHI-001 | [Brazilian Toll Road Scam — DETRAN Impersonation](PHI-001.md) | Credential Harvesting | ✅ True Positive |
| PHI-002 | [Real Estate BEC — Reply-To Hijack](PHI-002.md) | Business Email Compromise | ✅ True Positive |
| PHI-003 | [Spoofed Identity — Gibberish Domain Infrastructure](PHI-003.md) | Suspicious Link Delivery | ✅ True Positive |

> More investigations are actively in progress and will be added to this repo.

---

## 🛠️ Tools & Technologies

`MXToolbox` · `VirusTotal` · `AbuseIPDB` · `WHOIS` · `MITRE ATT&CK`

---

## 📌 Sample Source

All email samples are sourced from [**rf-peixoto/phishing_pot**](https://github.com/rf-peixoto/phishing_pot/tree/main) — a public honeypot-based collection of real phishing emails maintained for security researchers and detection developers.

---

*by [ryo](https://github.com/RYO-1313)*
