# IOCs — Fake Delivery Notification (Chronopost Impersonation)

| Type | Value | Notes |
|------|-------|-------|
| IP | 20.24.45.102 | Resolves to Microsoft Corp infrastructure; no VT/AbuseIPDB hits |
| Domain (HELO/origin) | momf.fioridigusto.it | Actual sending host per Received chain, not previously logged, needs separate reputation check |
| Domain (envelope sender) | SNDQJO8F3TKXD4.com | Randomly generated string, not in VT/AbuseIPDB |
| Domain (From header) | odzahquwvpiypw | Malformed/no TLD, spoofed identity |
| Domain (Reply-To) | in2.getdrip.com | Legitimate Drip marketing platform, abused for reply routing |
| URL | http://kyempapu.org/?act=cl | Flagged by G-Data and BitDefender as phishing; 3 other vendors flag as spam/suspicious |
| URL | http://kyempapu.org/track/?act=op | Open-tracking pixel, same domain/kit |
| Subject line | Votre colis est bloqué au centre | |
