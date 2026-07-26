# Analysis — Fake ISP fiber advertisement

## What happened
The email claimed to be an ISP provider offering better fiber pricing. It turned out to be phishing spam with no redirecting links, just a tracking one.

## Header analysis
No SPF/DKIM/DMARC. Checked the received chain, no spoofing signs. The sender IP claimed to be an ISP, but the domain is random letters.

## Sender / IP reputation
108.143.19.9 resolves to Microsoft Corporation, which suggests the sender used cloud-hosted infrastructure rather than a dedicated malicious host. That's common for bulk spam/marketing campaigns.

## URL / attachment behavior
Redirect chain led to a final landing page. The behavior was tracking only. Credential submission wasn't tested.

## Verdict
True Positive, low-risk phishing/spam. No credential-harvesting or malicious payload present. The embedded tracking link may indicate a recon attempt to validate the target's email as active, worth escalating to IR for monitoring.
