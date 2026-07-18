# Phishing Email Investigations

Investigations into phishing emails sourced from [phishing_pot](https://github.com/rf-peixoto/phishing_pot), a public dataset of real-world phishing samples. Each case includes header analysis, sender/IP reputation, URL detonation, and a verdict.

## Repo structure

```
phishing-investigations/
├── README.md
└── cases/
    └── YYYY-MM-DD_short-case-name/
        ├── analysis.md
        ├── iocs.md
        └── raw_sample.txt
```

## Case format (`analysis.md`)

1. **Title** — `Analysis — [Case Name] ([Impersonation Target])`
2. **What happened** — plain-language summary of the email and its goal
3. **Header analysis** — SPF/DKIM/DMARC results (via MXToolbox) + notes
4. **Sender / IP reputation** — IP/domain lookup (via AbuseIPDB) + notes
5. **URL / attachment behavior** — link/file scan results (via VirusTotal) + notes
6. **Social engineering** — table of tactics used (urgency, authority, fear, etc.)
7. **Verdict** — True Positive / False Positive / Inconclusive, with justification
8. **IOCs** — table of indicators, mirrored in `iocs.md`

## Tools

| Purpose | Tool |
|---|---|
| Header/authentication analysis | [MXToolbox](https://mxtoolbox.com/) |
| IP reputation | [AbuseIPDB](https://www.abuseipdb.com/) |
| URL/file detonation | [VirusTotal](https://www.virustotal.com/) |
| Sample source | [phishing_pot](https://github.com/rf-peixoto/phishing_pot) |

## Disclaimer

Samples come from the public [phishing_pot](https://github.com/rf-peixoto/phishing_pot) dataset, used strictly for research and detection-engineering purposes.
