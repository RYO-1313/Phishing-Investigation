# Phishing Case — Fake ISP fiber advertisement

An email posing as an ISP offering cheaper fiber pricing turned out to be phishing spam with a tracking link, no credential harvesting or malicious payload.

**Verdict:** True Positive — Low Risk (Recon)

## Files

* `samples.txt` — raw sample
* `analysis.md` — investigation and findings
* `iocs.md` — indicators of compromise
* `screenshots/` — evidence

## Email Overview

Claimed to be from an ISP provider advertising -300€/year off fiber pricing. Sent from a random-letter domain with no SPF/DKIM/DMARC, hosted on Microsoft cloud infrastructure. Contained one tracker link, no redirecting or credential-harvesting links.
