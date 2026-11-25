# Phishing Case — "Fake Product Report Email (The Lions Spray)"

**Verdict:** True Positive

**Severity:** 4 [TODO: confirm scale]

**Date Received:** 2026/11/15

**Date Analyzed:** 2026/11/15

**Analyst:** Youssef Touhami

## Summary

A user reported an email from "The Lions Spray Verbraucherreport" with the German subject "The Lions Spray analysiert den Erfolg des Produkts". The body is a single image; clicking it redirects. The email was re-sent through Gmail and fails authentication. VirusTotal flags the link (11 vendors, phishing to malicious). When clicked, the link lands on a "buy this domain" page.

## Detection Source

User report.

## Timeline

| Time | Event |
|-|-|
| [TODO: confirm] | Email delivered |
| 2026/11/15 09:30 | Alert fired |
| 2026/11/15 09:32 | Triage started |
| 2026/11/15 09:42 | Verdict reached |
| 2026/11/15 09:46 | Containment action taken |
| 2026/11/15 09:46 | Case closed |

> **Note:** The `Date:` header in the .eml reads 21 Jun 2026 18:37 UTC. The case dates above are the ones I gave for this scenario. [TODO: confirm timezone for the times above]

## Scope

* **Recipients affected:** 1 mailbox (carl)
* **Other reports of same campaign:** [TODO: confirm with Youssef]

## Email Overview

* **From (header):** `The Lions Spray Verbraucherreport <rain.baker@vfggamerisons999.onmicrosoft.com>`
* **Return-Path:** `rain.baker@vfggamerisons999.onmicrosoft.com`
* **Originating host (HELO):** `mail-yw1-f193.google.com` (209.85.128.193)
* **Subject:** The Lions Spray analysiert den Erfolg des Produkts (German: "The Lions Spray analyzes the product's success")
* **Delivered to:** phishing@pot
* **Attachments/Links:** No file attachment; the body is one image that links out
* **IP address:** 209.85.128.193
* **SCL (Spam Confidence Level):** 5 (medium), BCL 0

## Header Analysis

**Authentication:** SPF = fail, DKIM = fail (signature did not verify), DMARC = none (action = none), compauth = fail. The only green result is SPF domain alignment: From and Return-Path share the same domain, `vfggamerisons999.onmicrosoft.com`.

**Microsoft headers:** `X-SID-Result: FAIL`, `X-MS-Exchange-Organization-SCL: 5`, `X-Microsoft-Antispam: BCL:0`.

**Gmail relay:** The Message-ID `<CAG++pZ3H2iiWQyozbv0hg2rJVopG2F0QMP1gnyV=a6_RGnH2eg@mail.gmail.com>` was created by the Gmail system. The email wasn't sent directly; it was re-sent through Gmail.

**Received chain:**
1. `mail-yw1-f193.google.com` (209.85.128.193) → `CH2PEPF0000009D.mail.protection.outlook.com`
2. → `CH0PR08CA0020.namprd08.prod.outlook.com`
3. → `SN7PR10MB7104.namprd10.prod.outlook.com`
4. → `MW4PR10MB5749.namprd10.prod.outlook.com` *(final delivery to mailbox)*

## Sender / IP Reputation

209.85.128.193: Google LLC, AS15169, hostname `mail-yw1-f193.google.com`, domain google.com, Data Center/Web Hosting/Transit, Atlanta, Georgia, US.

* **VirusTotal:** clean
* **AbuseIPDB:** 0% (Low Risk). Community reports mostly spam; 3 under Email Spam, latest 2026-11-18 22:42:26.

## URL / Attachment Analysis

No file attachment. The body is a photo; clicking it redirects.

| Purpose | URL | Notes |
|-|-|-|
| Click-through (image) | `hxxps://cricktoon[.]org/4HHYIe21654jocx1466dqlqhdcujw114YBDUUXFJCJJWGUF23463PHCS646923k1` | 11 VirusTotal vendors flag it (phishing to malicious) |

* **Redirect chain:** [TODO: confirm with Youssef]
* **Final payload/page behavior:** Clicked it, the page just shows "buy this domain".
* **Scan:** URL scanner couldn't scan it.
* **Sandbox detonation notes:** [TODO: confirm with Youssef]
* **Credential submission tested:** [TODO: confirm with Youssef]
* **Body language:** The body is an image. Language not identified.

## Indicators of Compromise (IOCs)

| Type | Value | Notes |
|-|-|-|
| IP | 209.85.128.193 | Google LLC (AS15169); clean on VirusTotal, 0% on AbuseIPDB |
| Domain (From / Return-Path) | vfggamerisons999[.]onmicrosoft[.]com | Same domain in both; SPF alignment passes, everything else fails |
| Email address | rain.baker@vfggamerisons999[.]onmicrosoft[.]com | Display name "The Lions Spray Verbraucherreport" |
| Domain | cricktoon[.]org | Click-through link domain |
| URL | hxxps://cricktoon[.]org/4HHYIe21654jocx1466dqlqhdcujw114YBDUUXFJCJJWGUF23463PHCS646923k1 | 11 VirusTotal vendors flag it |
| Subject line | The Lions Spray analysiert den Erfolg des Produkts | |

## MITRE ATT&CK Mapping

| Technique | ID |
|-|-|
| Phishing: Spearphishing Link | T1566.002 |

## Containment / Response Actions

* [ ] Email quarantined/deleted org-wide
* [X] Sender domain/IP blocked
* [X] Malicious URL blocked at proxy/firewall
* [X] User(s) contacted
* [X] Team informed

## Conclusion

True Positive, Severity 4. The email fails SPF, DKIM, DMARC and compauth, with only SPF domain alignment passing. It was re-sent through Gmail, and the sender IP is Google's (clean on VirusTotal, 0% on AbuseIPDB). The link is flagged by 11 VirusTotal vendors, and clicking it lands on a "buy this domain" page. Sender and URL blocked, user contacted, team informed.
