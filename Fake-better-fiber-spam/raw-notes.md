# Phishing Case — "Fake ISP fiber advertisement"

**Verdict:** True Positive — Low Risk (Recon)

**Severity:** Low 

**Date Received:** 2026/04/18

**Date Analyzed:** 2026/07/26

**Analyst:** Youssef Touhami

## Summary

The Email claimed to be an ISP provider with better fiber pricing, the email happened to be phishing spam with no redirecting links , but a tracking one

## Detection Source

EDR/mail gateway alert.

## Timeline

|Time|Event|
|-|-|
|2026/07/26  18:03|Email delivered|
|2026/07/26  18:05|User reported / alert fired|
|2026/07/26  18:10|Triage started|
|2026/07/26  18:25|Verdict reached|
|2026/07/26  18:30|Containment action taken|
|2026/07/26  18:33|Case closed|

## Scope

* **Recipients affected:** 1 mailbox
* **Other reports of same campaign:** no

## Email Overview

* **From:** SNLCTC48@F15OHZNAWS03XM546WOITWUPXS0D.com
* **Reply-To :** areply-\[nu\_24]@in2.getdrip.com
* **Subject :** Votre adresse est éligible : -300€/an sur la fibre ? 🌐
* **Delivered to :** phishing@pot
* **Attachments/Links :** yes, Tracker Html link
* **IP address :** 108.143.19.9

## Header Analysis

No SPF/DKIM/DMARC , Received chain, No spoofing signs, sender IP claimed to be ISP , but the domain is random letters

## Sender / IP Reputation

108.143.19.9 resolves to Microsoft Corporation which suggests that the sender used cloud hosted infrastructure rather than dedicated malicious host, that's common for bulk spam/Marketing campaigns . 

## URL / Attachment Analysis

* **Redirect chain:** final landing page
* **Final payload/page behavior:** Tracking.
* **Sandbox detonation notes:** not applicable
* **Credential submission tested:** no 

## Indicators of Compromise (IOCs)

|Type|Value|Notes|
|-|-|-|
|IP|108.143.19.9|resolves to Microsoft Corporation, with no reports on VT or AbuseIPDB|
|Domain|F15OHZNAWS03XM546WOITWUPXS0D.com|Not found in AbuseIPDB VT, the random letters are red flag|
|URL|https://kyempapu.org/track/?act=op\&amp;pid=1330\_rd\&amp;uid=20\&amp;cmpid=0\&amp;ofid=4865\&amp;lid=466\&amp;cid=6983baaef125cc1528f7e2f6|flagged by Gridinsoft as suspicious|
|File hash|no |no|

## MITRE ATT\&CK Mapping

|Technique|ID|
|-|-|
|Phishing: Spearphishing Link|T1566.002|

## Containment / Response Actions

* \[ ] Email quarantined/deleted org-wide
* \[X] Sender domain/IP blocked
* \[ ] Malicious URL blocked at proxy/firewall
* \[ ] User(s) contacted / password reset if credentials entered
* \[X] Escalated to IR

## Conclusion

True Positive, low-risk phishing/spam. No credential-harvesting or malicious payload present. The embedded tracking link may indicate a recon attempt to validate the target's email as active, worth escalation to IR for monitoring 

