# Phishing Case — Fake Delivery Notification (Chronopost Impersonation)

An email impersonating Chronopost claims a blocked parcel and a small customs fee to pressure a click. Authentication fails completely, and the link structure matches a bulk phishing/spam kit built for recon, not credential theft.

**Verdict:** True Positive — Low Risk (Recon / Bulk Spam Campaign)

## Files

* `samples.txt` — raw sample
* `analysis.md` — investigation and findings
* `iocs.md` — indicators of compromise
* `screenshots/` — evidence
* `raw-notes.md` — my raw notes (its a template i fill with informations about the email while investigating)

## Email Overview

Spoofs Chronopost with a "Global Tracking System / Logistics Express" delivery notice, claiming an incorrect address or an unpaid €0.99 customs fee with a 14-hour hold. SPF/DKIM/DMARC all fail, and Microsoft's own filter scored it at max spam confidence (SCL 9/9). No attachments, three tracked links (open, click, unsubscribe) tied to a templated spam kit.
