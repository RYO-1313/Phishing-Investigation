# Fake ExtremeSpins VIP casino bonus (no attachment, no login form)

**Verdict:** True positive, low severity
**Sample:** [sample-8888.eml](https://github.com/rf-peixoto/phishing_pot/blob/main/email/sample-8888.eml) (phishing_pot repo)
**Email date:** 5 Feb 2026 · **Analyzed:** 26 Nov 2026 · **Analyst:** Youssef

> This is a practice investigation on a public sample, not an incident from a live environment.

## The short version

An email pretending to be a Halloween gift from an online casino called ExtremeSpins says the recipient has won €1,000 and 100 free spins, with no deposit needed. It pushes a single "claim my bonus" button and a "limited number of members" line to get a click. There's no attachment and no login form in the email. It's phishing, and the main link is flagged as phishing by 10 security vendors.

## What the email looks like

Subject: `💎 Invitation VIP : obtenez 1 000 € et 100 tours sans dépôt !` — French for "VIP invitation: get €1,000 and 100 spins with no deposit!" The display name is `👻 Cadeau Halloween – ExtremeSpins` ("Halloween gift").

The body is black with green text. It opens with `🎉 Vous avez reçu 1 000 € et 100 tours gratuits — sans dépôt requis !`, greets the recipient, shows a fake account ID `#0023432`, and lists the gift as `1 000 € de bonus + 100 tours gratuits`. The main button is a green `RÉCLAMER MON BONUS »` ("claim my bonus"). The urgency line says the offer is "reserved for a limited number of members". The footer has unsubscribe and privacy links.

The HTML part greets `phishing@pot@hotmail.com`; the plain-text part still greets `rodrigo-fp@hotmail.com`, so the mail merge (the automatic name and address fill-in) is inconsistent.

## Who sent it?

The display name says ExtremeSpins, but the real From address is `noreply@houssaine-8fb71.firebaseapp.com`, a subdomain of Google's Firebase Hosting service. The envelope sender (the address the mail server actually used) is the same. Reply-To is `no-reply@example.com`, a placeholder domain that has nothing to do with ExtremeSpins. The only part that says ExtremeSpins is the display name.

On the authentication checks (SPF, DKIM and DMARC — the checks that prove a sender is who they say they are): SPF alignment and SPF authentication passed, and DKIM alignment passed. DKIM authentication and DMARC failed. The Delivery Information shows no DMARC record found. So the sending server was allowed to send for that Firebase subdomain, but the domain has no DMARC policy and the DKIM authentication check didn't pass. None of that ties the sender to the real ExtremeSpins.

## Where did it come from?

The sending IP is `209.85.219.69`, which the headers show as `mail-qv1-f69.google.com`. That's Google mail infrastructure, not a server registered to ExtremeSpins. VirusTotal has one vendor flagging the IP as suspicious. AbuseIPDB lists 661 reports from 63 reporters, mostly Email Spam (124), plus smaller numbers of Brute-Force, Spoofing, Phishing and Exploited Host reports. It also notes the IP is on a whitelisted Google subnet and gives it a 0% abuse confidence score.

On VirusTotal, `firebaseapp.com` has 0/92 vendor detections. The community tab has several comments about phishing subdomains. One comment explains why vendors don't block the domain: "Not easy to outright block firebaseapp.com because legitimate services uses it like Virustotal GitHub SSO." The specific subdomain `houssaine-8fb71.firebaseapp.com` also has 0 vendor detections, but every vendor is listed as "Unrated", so nobody has rated it rather than a clean verdict.

Microsoft's spam filter gave the email an SCL (spam confidence level) of 5 and a BCL (bulk complaint level) of 0, so it wasn't a confident spam verdict.

## What do the links do?

The email has one main link: `http://black01.click/t/4tlXwI6766MlAqY68bgQf0NLq0fJbn0Lwtk10`, used by the green `RÉCLAMER MON BONUS »` button. VirusTotal flags it as phishing by 10 vendors, with 1 malicious and 1 malware detection. urlscan returned HTTP 400 with "DNS Error - Could not resolve domain."

There are no attachments. The email is plain text plus HTML, and there's no login form or credential field in the email itself.

## What I didn't do

I didn't open the link, detonate it in a sandbox (run it in an isolated test environment), or follow the redirect. I only checked it on VirusTotal and urlscan. I also only looked at the main button link, not the unsubscribe link or the tracking pixel. That means the verdict is based on the email content and the link's reputation, not on what the landing page actually shows or whether it asks for credentials.

## Conclusion

True positive, low severity. The email impersonates ExtremeSpins with a fake Halloween bonus, uses a limited-members line to push a click, and the main link is flagged as phishing by 10 vendors. SPF passed, but DKIM authentication and DMARC failed, and there's no DMARC record. The email has no attachment and no credential-harvesting form. The link wasn't opened, so the landing page wasn't checked.

Actions: block the IP, escalate to L2, and inform the SOC and IT team.

## IOCs (clues you can search for or block)

| Type | Value | Note |
|---|---|---|
| Sender IP | `209.85.219.69` | Google mail server (`mail-qv1-f69.google.com`); shared infrastructure |
| From / Return-Path | `noreply@houssaine-8fb71.firebaseapp.com` | Firebase Hosting subdomain |
| Sender domain | `houssaine-8fb71.firebaseapp.com` | Firebase Hosting subdomain used as the sender |
| Reply-To | `no-reply@example.com` | Placeholder address, not ExtremeSpins |
| URL | `http://black01.click/t/4tlXwI6766MlAqY68bgQf0NLq0fJbn0Lwtk10` | Main button; 10 phishing / 1 malicious / 1 malware on VirusTotal |
| Domain | `firebaseapp.com` | Legitimate Google Firebase domain; don't block wholesale |
