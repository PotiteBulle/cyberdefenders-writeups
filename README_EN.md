# CyberDefenders Writeups

Personal repository dedicated to my CyberDefenders writeups.

CyberDefenders is a Blue Team and SOC training platform offering labs, cyber ranges, investigation scenarios, training paths, and certifications.

Official website: https://cyberdefenders.org/

## Repository Goal

This repository documents my progress in defensive cybersecurity through CyberDefenders labs.

Main focus areas include:

- SOC analysis
- Blue Team
- Digital Forensics
- Incident Response
- Threat Hunting
- Malware triage
- Network analysis
- Windows log analysis
- SIEM investigation
- Detection with Sigma and YARA
- Technical report writing

## Repository Structure

```text
cyberdefenders-writeups/
├── README.md
├── README_EN.md
├── labs/
│   ├── README.md
│   ├── easy/
│   ├── medium/
│   └── hard/
├── templates/
│   ├── README.md
│   ├── README_EN.md
│   ├── writeup-template-fr.md
│   ├── writeup-template-en.md
│   ├── notes-template-fr.md
│   ├── notes-template-en.md
│   ├── iocs-template-fr.md
│   └── iocs-template-en.md
└── notes/
    ├── methodology.md
    ├── tools.md
    ├── commands-cheatsheet.md
    └── soc-glossary.md
```

## Writeup Format

Each writeup follows a clear investigation structure:

1. General information
2. Useful links
3. Lab context
4. Analysis objective
5. Environment preparation
6. Provided artifacts
7. Methodology
8. Technical analysis
9. Lab questions with partially masked answers
10. IOCs
11. Timeline
12. Conclusion
13. Lessons learned

## Example Lab Structure

```text
labs/easy/lab-name/
├── readme_LABNAME.md
├── Readme_LABNAME_EN.md
├── writeup-fr.md
├── writeup-en.md
├── notes-fr.md
├── notes-en.md
├── iocs-fr.md
└── iocs-en.md
```

## Personal Methodology

Each investigation should remain readable, structured, and reproducible.

I document:

- initial hypotheses
- commands used
- analyzed artifacts
- suspicious elements
- important evidence
- mistakes or abandoned leads
- defensive conclusions
- analysis limitations
- lessons learned

## Possible Tools

Depending on the lab, tools may include:

- Wireshark
- CyberChef
- Volatility
- Autopsy
- FTK Imager
- Windows Event Viewer
- PowerShell
- Python
- YARA
- Sigma
- jq
- grep
- strings
- VirusTotal
- ANY.RUN
- Hybrid Analysis
- MITRE ATT&CK
- SIEM or lab-provided interface

## Answer Handling

Direct answers to lab questions may be partially masked in public writeups.

The goal is to avoid simple copy-pasting while still showing the investigation method, tools used, and technical reasoning.

Example:

```text
Full answer: not published directly
Masked answer: http://******.example/***.php
Method: clearly documented
```

## Disclaimer

The writeups published here are written for educational, defensive, and ethical purposes.

They should not be used to bypass learning or simply copy answers.

The goal is to understand the methodology, tools, and reasoning behind each investigation.

No real-world offensive or unauthorized activity is performed as part of this repository.
