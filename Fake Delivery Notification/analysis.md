# Analysis — Fake Delivery Notification (Chronopost Impersonation)

## What happened
An EDR/mail gateway alert fired on an email impersonating Chronopost, styled as a "Global Tracking System / Logistics Express" delivery notice. It claims an incorrect address or an unpaid €0.99 customs fee is blocking delivery, with a 14-hour hold period, pushing the recipient to click through. There's no credential form or malicious attachment. The tracked open/click/unsubscribe links match a bulk phishing/spam kit, most likely recon to confirm the mailbox is active.

## Header analysis
SPF, DKIM, and DMARC all came back none, with no authentication action taken. No authentication passes at all, consistent with a spoofed, unauthenticated sender.

Received chain: momf.fioridigusto.it (20.24.45.102) into Microsoft's front-end/EOP at DB1PEPF000509FE.eurprd03.prod.outlook.com, then DB9PR02CA0019.outlook.office365.com, then DM3PPFA09EE1970.namprd10.prod.outlook.com, then final delivery at MW4PR10MB5749.

Spoofing indicators: the From-header display name "Chronopost 📦" impersonates the real carrier, but the actual From-domain (odzahquwvpiypw) is malformed with no TLD. The envelope sender domain (SNDQJO8F3TKXD4.com) is a randomly generated string with no tie to the brand. The fake urgency (tracking reference #FR-9822-TK, 14-hour hold, a small €0.99 "customs fee") is a classic low-friction phishing lure amount.

Microsoft's own filter already scored this at SCL 9/9, max spam confidence.

## Sender / IP reputation
20.24.45.102 resolves to Microsoft Corporation infrastructure, cloud-hosted. The actual delivery hop rode on Azure-adjacent infrastructure rather than a dedicated malicious host, which is common for bulk spam/marketing campaigns using legitimate cloud providers. No prior abuse reports on this IP.

## URL / attachment behavior
No attachments. Three tracked HTML links, all sharing identical campaign parameters (pid=1338_rd, uid=20, cmpid=0, ofid=5114, lid=466, cid=6983baaef125cc1528f7e2f6), which points to a templated bulk phishing/spam-tracking kit rather than a one-off page:

- Open tracking (pixel): fires when the email is opened
- Click-through: the main redirect used by all body buttons/links
- Unsubscribe: standard unsubscribe-style link

The redirect chain wasn't fully traced — following `act=cl` would confirm the final landing page behavior. Sandbox detonation wasn't performed, and credential submission wasn't tested. Based on the kit structure, the final behavior looks like tracking/lead confirmation (recon), with no confirmed credential form.

## Verdict
True Positive, low-risk phishing/spam impersonating Chronopost via a bulk delivery-notification lure. No credential-harvesting form or malicious payload present. Authentication fully fails (SPF/DKIM/DMARC none) and Microsoft's filter already flagged it at max spam confidence (SCL 9). The tracked open/click/unsubscribe structure points to a templated spam/phishing kit, and the tracking pixel/link likely serves as recon to confirm the mailbox is active. Recommend escalating the origin domain (momf.fioridigusto.it) and envelope-sender domain (SNDQJO8F3TKXD4.com) for independent reputation review, alongside the already-blocked IP.
