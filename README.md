# Phishing Investigations

This repo is where I document phishing emails I investigate — the raw sample, what I checked, what I found, and how I closed each case out.

## Structure

Each investigation gets its own folder, named after the case:

```
phishing-case-<slug>/
├── README.md       # quick summary and verdict
├── samples.txt     # raw sample: headers, body, URLs, attachments
├── analysis.md     # what I checked and found
├── iocs.md         # indicators of compromise
├── raw-notes.txt   # my raw notes from the investigation
└── screenshots/    # evidence
```

Browse into any case folder to see the full write-up.

## Cases

- [DETRAN Toll Phishing](./phishing-case-detran-toll) — fake toll payment SMS/email impersonating DETRAN
- [Fake Fiber advertisement](./Fake-better-fiber-spam) — fake ISP advertisement  spam/recon
- [Fake Delivery Notification](./Fake-Delivery-Notification) — fake Delivery Notification impersonating 
