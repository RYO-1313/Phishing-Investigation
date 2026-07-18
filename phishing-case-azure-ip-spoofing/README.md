# Phishing Case — Spoofed Identity: Gibberish Domain Infrastructure (PHI-003)

A phishing email sent from randomly-generated gibberish domains, routed through a trusted
Microsoft cloud IP to dodge reputation-based blocking, with an attached link flagged malicious
by threat intel.

**Verdict:** True Positive — Escalated to L2 SOC

## Files

* `samples.txt` — raw sample
* `analysis.md` — investigation and findings
* `iocs.md` — indicators of compromise
* `screenshots/` — evidence

## Email Overview
Sender used a from-address and sender-address built from random alphanumeric strings
(`8DDW299YD3LSARPE5KWZZCHBHC2B.com`, `A8XINU8B9JL9H1.com`), sent from a Microsoft cloud IP
(`51.104.208.151`), with no SPF/DKIM/DMARC. Reply-To pointed to `getdrip.com` and Complaint-To
routed through an unrelated Italian server (`we16.morona.it`). The attached link,
`kyempapu.org`, was flagged Phishing by 2 VirusTotal engines and Suspicious/Spam by 3 more.
