# Analysis — Spoofed Identity: Gibberish Domain Infrastructure (PHI-003)

## What happened
A suspicious email came in from a sender address built from a random alphanumeric string —
not the kind of domain any real organization uses. The sending IP traced back to Microsoft's
cloud infrastructure, a deliberate move to borrow legitimacy from a trusted provider. The
complaint path routed through an Italian server with no clear connection to any of the other
domains involved. The attached link, `kyempapu.org`, carried an active suspicious reputation.

## Header analysis
Pulled the full header through MXToolbox to check authentication and routing.

| Check | Result |
|-------|--------|
| SPF   | None |
| DKIM  | None |
| DMARC | None |

Complete absence of all three. No legitimate sender — individual, business, or service —
skips SPF, DKIM, and DMARC at the same time. That alone puts the email in the suspicious
category before anything else is checked.

## Sender / IP reputation
Sender IP `51.104.208.151` — ISP is Microsoft Limited. AbuseIPDB: no reports. VirusTotal:
unrated.

Microsoft Azure IPs are used by millions of legitimate services every day, which is exactly
why attackers provision sending infrastructure there — the IP carries built-in trust that
reputation-based filters are reluctant to block. An unrated, clean Microsoft IP combined with
zero email authentication is the classic setup for a campaign riding trusted infrastructure
through the front door.

Both sender domains — `8DDW299YD3LSARPE5KWZZCHBHC2B.com` and `A8XINU8B9JL9H1.com` — are
random alphanumeric strings with no connection to any registered entity, business, or
service. Domains like this exist for one purpose: single-use disposable sending
infrastructure, registered cheap, used once, abandoned before they pick up reputation flags.

The header's complaint-to address routes to `we16.morona.it`, an Italian server unconnected
to any other domain in the email. Legitimate bulk senders route abuse complaints to their own
domain. An Italian complaint server showing up alongside US cloud infrastructure and nonsense
sender domains points to infrastructure stitched together from separate disposable pieces —
common in spam-as-a-service setups.

## URL / attachment behavior
Submitted `https://kyempapu.org` to VirusTotal. Result: 2/92 flagged it outright as
**Phishing** (BitDefender, G-Data). Beyond the main score, ESET and Gridinsoft flagged it
**Suspicious**, and alphaMountain.ai flagged it **Spam**. The rest of the engines returned
clean.

A low overall score doesn't mean the link is safe — two engines calling it phishing outright,
plus three more flagging it suspicious/spam, is enough signal on its own, and it lines up with
every other indicator in this email. Fresh or recently rotated malicious infrastructure often
hasn't been picked up by the full engine set yet. Treating this as malicious pending sandbox
confirmation.

## Verdict
**True Positive — Escalated to L2 SOC.**

Fabricated sender domains, complete absence of email authentication, a flagged attached link,
and anomalous routing infrastructure. No legitimate explanation covers all of these signals
together. Escalated to L2 for sandbox detonation of `kyempapu.org` and full infrastructure
mapping.
