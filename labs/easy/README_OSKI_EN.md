# Oski Lab

## Platform

CyberDefenders

Official link: https://cyberdefenders.org/blueteam-ctf-challenges/oski/

## Information

| Field | Value |
|---|---|
| Lab | Oski |
| Track | SOC Analyst Tier 1 |
| Level | Level 1 |
| Difficulty | Easy |
| Category | Threat Intel |
| Status | Completed |

## Sources

| Source | Link |
|---|---|
| CyberDefenders | `https://cyberdefenders.org/blueteam-ctf-challenges/oski/` |
| VirusTotal | `https://www.virustotal.com/gui/file/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` |
| ANY.RUN | `https://app.any.run/` |

## Summary

This lab consists of analyzing a hash provided by CyberDefenders in order to identify a malicious sample, review its VirusTotal metadata, and then analyze its behavior in ANY.RUN.

The sample is associated with **Stealc stealer** malware.

The analysis helps practice several important SOC and Blue Team skills:

- hash-based investigation
- threat intelligence pivoting
- VirusTotal metadata review
- ANY.RUN sandbox analysis
- C2 communication identification
- malware configuration review
- child process analysis
- MITRE ATT&CK mapping
- IOC documentation
- writeup writing with partially masked answers

## Learning Objectives

The main objectives of the lab are:

- understand how to use a hash as an investigation starting point
- identify a sample from an MD5 hash
- enrich the analysis with SHA1 and SHA256
- identify the file type and PE metadata
- analyze HTTP requests and network communications
- understand stealer malware behavior
- identify configuration elements extracted by ANY.RUN
- document observations in a clean and reusable format

## Folder Structure

| File | Description |
|---|---|
| `README.md` | Lab overview in French |
| `README_EN.md` | Lab overview in English |
| `writeup-fr.md` | Full French writeup with partially masked answers |
| `writeup-en.md` | Full English writeup with partially masked answers |
| `notes-fr.md` | Professional investigation notes in French |
| `notes-en.md` | Professional investigation notes in English |
| `iocs-fr.md` | Indicators of compromise in French |
| `iocs-en.md` | Indicators of compromise in English |

## Documentation Choice

No screenshots are included in this folder.

The writeup intentionally relies on text-based documentation: commands used, observations, methodology, IOCs, MITRE ATT&CK mapping, and references to analysis platforms.

This keeps the repository lighter, easier to read, and easier to maintain.

## Lab Status

| Item | Status |
|---|---|
| Hash extraction | Completed |
| VirusTotal pivot | Completed |
| ANY.RUN analysis | Completed |
| MITRE ATT&CK mapping | Completed |
| French writeup | Completed |
| English writeup | Completed |
| French notes | Completed |
| English notes | Completed |
| French IOCs | Completed |
| English IOCs | Completed |
| Screenshots | Not used |

## Disclaimer

Answers are intentionally partially masked to avoid direct copy-pasting and to encourage understanding the analysis methodology.

This lab is documented only for educational, defensive, and controlled cybersecurity training purposes.

No real-world offensive or unauthorized activity is performed.
