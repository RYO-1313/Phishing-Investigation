# Phishing Email Investigations

This repo contains my personal investigations into phishing emails sourced from [phishing_pot](https://github.com/rf-peixoto/phishing_pot), a public dataset of real-world phishing samples. Each case is analyzed and documented the way I'd write it up for a SOC ticket: what the email claims, what the technical evidence actually shows, and whether it's a confirmed threat.

The goal is to build a searchable, consistent archive of phishing patterns, indicators, and analyst workflow — useful for practice, reference, and pattern-matching against future samples.

## Why this exists

`phishing_pot` gives raw samples but no analysis. This repo is where I do the actual investigative work: header forensics, sender/IP reputation, URL detonation, and social engineering breakdown, then document the findings in a repeatable format so cases stay comparable to one another.

## Repo structure

```
phishing-investigations/
├── README.md
├── cases/
│   ├── 2026-06-09_detran-toll-phishing/
│   │   ├── analysis.md
│   │   ├── iocs.md
│   │   └── raw_sample.txt
│   ├── YYYY-MM-DD_short-case-name/
│   │   ├── analysis.md
│   │   ├── iocs.md
│   │   └── raw_sample.txt
│   └── ...
└── templates/
    └── case_template.md
```

Each case gets its own folder, named `YYYY-MM-DD_short-descriptive-name`, using the date the sample was collected/analyzed. Inside every case folder:

- **`analysis.md`** — the full writeup (see format below)
- **`iocs.md`** — a clean, copy-pasteable table of indicators of compromise
- **`raw_sample.txt`** — the original headers/body/URLs pulled from `phishing_pot`, unmodified

## Case documentation format

Every `analysis.md` follows the same skeleton so cases are easy to compare and skim:

### 1. Title
`Analysis — [Short Case Name] ([Impersonation Target, if any])`

### 2. What happened
A short plain-language summary: who/what the email impersonates, what it threatens or offers, and what it's trying to get the recipient to do. This is the "explain it to a non-analyst" paragraph.

### 3. Header analysis
Headers run through a tool like [MXToolbox](https://mxtoolbox.com/) to pull SPF, DKIM, and DMARC results, plus routing/originating info. Presented as a small results table:

| Check | Result |
|---|---|
| SPF | |
| DKIM | |
| DMARC | |

Followed by a short note on what the results mean (e.g., all-empty auth results = sender identity can't be verified).

### 4. Sender / IP reputation
Originating IP checked against [AbuseIPDB](https://www.abuseipdb.com/) (or similar). Notes on:
- Geolocation and ISP/hosting provider
- Abuse report history
- Any mismatch between claimed origin (e.g., a government agency) and actual infrastructure (e.g., generic cloud hosting in another country)
- Sender domain validity (real TLD vs. unregistrable string, age, registrar, etc.)

### 5. URL / attachment behavior
Links and attachments submitted to [VirusTotal](https://www.virustotal.com/) for multi-engine scanning. Notes on:
- Detection count/ratio
- Techniques used to disguise the URL (lookalike domains, embedded trusted brand names, abuse of legitimate cloud platforms like Cloud Run, Azure, AWS, etc.)
- Actual landing destination vs. what it appears to be at a glance

### 6. Social engineering
A table breaking down the manipulation techniques used in the body copy:

| Technique | Example from email | Effect |
|---|---|---|
| Urgency | | |
| Authority | | |
| Fear/penalty | | |

### 7. Verdict
One of:
- **True Positive** — confirmed phishing, with confidence level (low/medium/high) and next step (e.g., escalated to L2 SOC, blocked, reported)
- **False Positive** — legitimate email, with reasoning
- **Inconclusive** — insufficient evidence, with what's missing

Always backed by a one-line justification tying back to the strongest indicators found.

### 8. IOCs
Mirrored into `iocs.md` as a standalone table:

| Type | Value | Notes |
|---|---|---|
| IP | | |
| Domain | | |
| URL | | |

### 9. Raw sample
The original sample preserved as-is in `raw_sample.txt`, including:
- Email headers/sender info (display name, from address, return-path, sending IP, language)
- Body themes, key phrases, and any URLs/attachments as extracted from the original

## Tools used across investigations

| Purpose | Tool |
|---|---|
| Header/authentication analysis | [MXToolbox](https://mxtoolbox.com/) |
| IP reputation | [AbuseIPDB](https://www.abuseipdb.com/) |
| URL/file detonation | [VirusTotal](https://www.virustotal.com/) |
| Sample source | [phishing_pot](https://github.com/rf-peixoto/phishing_pot) |

## Adding a new case

1. Copy `templates/case_template.md` into a new folder under `cases/` following the naming convention.
2. Pull the raw sample from `phishing_pot` into `raw_sample.txt`.
3. Run header, IP, and URL checks; fill in `analysis.md` section by section.
4. Populate `iocs.md` from the IOCs table in the analysis.
5. Commit.

## Disclaimer

All samples originate from the public [phishing_pot](https://github.com/rf-peixoto/phishing_pot) dataset and are used strictly for research, training, and detection-engineering purposes. No live phishing infrastructure is interacted with beyond passive lookups and sandboxed detonation via third-party tools (VirusTotal, etc.). IOCs are shared for defensive/blocklist use only.
