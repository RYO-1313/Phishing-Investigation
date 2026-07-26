# Analysis — Fake ISP fiber advertisement

## What happened
The email claimed to be an ISP provider offering better fiber pricing. It turned out to be phishing spam with no redirecting links, just a tracking one.

## Header analysis
No SPF/DKIM/DMARC. Checked the received chain, no spoofing signs. The sender IP claimed to be an ISP, but the domain is random letters.
<img width="1919" height="931" alt="Screenshot 2026-07-26 143256" src="https://github.com/user-attachments/assets/0bd51d55-835a-4e61-89cc-f3c5228b6ea3" />

## Sender / IP reputation
108.143.19.9 resolves to Microsoft Corporation, which suggests the sender used cloud-hosted infrastructure rather than a dedicated malicious host. That's common for bulk spam/marketing campaigns.
<img width="1919" height="959" alt="Screenshot 2026-07-26 143345" src="https://github.com/user-attachments/assets/cffd351b-17b5-4603-9f9f-5efc5dbee763" />

## URL / attachment behavior
Redirect chain led to a final landing page. The behavior was tracking only. Credential submission wasn't tested.
<img width="1916" height="969" alt="Screenshot 2026-07-26 143417" src="https://github.com/user-attachments/assets/13388c8b-ff4a-4cf7-b0e4-78a94b9389f0" />

## Verdict
True Positive, low-risk phishing/spam. No credential-harvesting or malicious payload present. The embedded tracking link may indicate a recon attempt to validate the target's email as active, worth escalating to IR for monitoring.
