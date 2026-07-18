# Analysis — Brazilian Toll Road Phishing (DETRAN Impersonation)

## What happened
An email impersonating DETRAN, Brazil's national vehicle and traffic authority, showed up in
the queue warning the recipient of an unpaid toll charge and threatening fines, license point
deductions, and vehicle restrictions. The language was urgent and authoritative, built to
pressure the recipient into clicking a payment link without stopping to think. Header analysis,
sender reputation checks, and URL detonation confirmed it as a phishing attempt targeting
Portuguese-speaking victims to harvest payment credentials.

## Header analysis
Pulled the full header through MXToolbox to get the originating IP, authentication results,
and routing path.

Authentication results:

| Check | Result |
|-------|--------|
| SPF | None |
| DKIM | None |
| DMARC | None |

A legitimate government or institutional sender will always have SPF, DKIM, and DMARC
configured. All three checks coming back empty means there's no way to verify this email came
from who it claims. Any email failing all three should be treated as suspicious right away.

## Sender / IP reputation
Looked up the originating IP 162.243.92.6 in AbuseIPDB.

Findings:
- Location: United States
- ISP: DigitalOcean, LLC
- Abuse reports: none on record

An unrated IP on AbuseIPDB doesn't mean it's clean — it means the attacker is likely using
fresh or rotated infrastructure specifically to dodge reputation-based blocking. More telling
here: the IP traces to a US-based DigitalOcean server, while the email is written entirely in
Portuguese and impersonates a Brazilian government agency. That geographic and linguistic
mismatch is a strong sign the origin is spoofed.

Sender domain checked next: 616305pedagiodigital. It has no top-level domain — a legitimate
domain has to end in .com, .gov, .br, or similar. This one can't be registered, verified, or
traced, which looks like a deliberate move to block domain reputation lookups.

## URL / attachment behavior
Submitted the attached payment URL to VirusTotal for multi-engine analysis. 11 security engines
flagged it as phishing.

The URL is built to look legitimate at a glance — it opens with "office.com" to fool a
recipient reading quickly. The actual destination is a Google Cloud Run application
(southamerica-east1.run.app), meaning the attacker is abusing trusted cloud hosting to serve
the phishing page and slip past URL reputation filters.

## Social engineering
Beyond the technical indicators, the email leans on three pressure tactics:

| Technique | Example from email | Effect |
|-----------|--------------------|--------|
| Urgency | "URGENT NOTICE", "CHECK YOUR LICENSE PLATE NOW" | Forces a snap decision, no time to think |
| Authority | Impersonates DETRAN, a real government body | Makes the threat feel legitimate |
| Penalty/fear | Warnings of fines, license points, vehicle restrictions, debts forwarded to DETRAN | Creates fear of real consequences to override caution |

## Verdict
True Positive, high confidence. Escalated to L2 SOC. Multiple high-severity technical
indicators combined with confirmed phishing detections on VirusTotal and deliberate social
engineering language, with no legitimate explanation for any of the observed signals.
