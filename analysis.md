# Analysis — Fake Product Report Email (The Lions Spray)

## What happened
A user (carl) reported an email from "The Lions Spray Verbraucherreport" with the German subject "The Lions Spray analysiert den Erfolg des Produkts" (The Lions Spray analyzes the product's success). The body is a single image. Clicking it redirects. The sender passes through Gmail and fails authentication, and VirusTotal flags the link. I clicked it and landed on a "buy this domain" page.

## Header analysis
SPF failed, DKIM failed (signature did not verify), DMARC returned none with no action, and compauth failed. The only green result is SPF domain alignment: the From and Return-Path domains are the same, `vfggamerisons999.onmicrosoft.com`.

Microsoft's own headers: `X-SID-Result: FAIL`, SCL 5 (medium spam confidence), BCL 0.

The Message-ID (`...@mail.gmail.com`) is a Gmail-generated ID, so the email wasn't sent directly from the sender's domain. Gmail created it and re-sent it. The Received chain confirms it:

1. `mail-yw1-f193.google.com` (209.85.128.193) → `CH2PEPF0000009D.mail.protection.outlook.com`
2. → `CH0PR08CA0020.namprd08.prod.outlook.com`
3. → `SN7PR10MB7104.namprd10.prod.outlook.com`
4. → `MW4PR10MB5749.namprd10.prod.outlook.com` (final delivery)

## Sender / IP reputation
209.85.128.193 belongs to Google LLC (AS15169), hostname `mail-yw1-f193.google.com`, Data Center/Web Hosting/Transit, Atlanta, Georgia, US.

- **VirusTotal:** clean
- **AbuseIPDB:** 0% (Low Risk). Community reports are mostly spam, 3 under Email Spam, latest 2026-11-18 22:42:26.

## URL / attachment behavior
No file attachment. The body is an image with the link behind it. Clicking the image redirects to the link below.

- VirusTotal: 11 vendors flag the URL, with labels from phishing to malicious (vendor list in the screenshot).
- I clicked it. The page just says "buy this domain".
- The URL scanner couldn't scan it.

## Verdict
True Positive, Severity 4. The email fails SPF, DKIM, DMARC and compauth, it was relayed through Gmail, and the link is flagged by 11 VirusTotal vendors. The sender IP itself is Google's and comes back clean.

Actions taken: sender blocked, URL blocked, user contacted, team informed.
