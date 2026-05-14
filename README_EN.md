# CyberDefenders Writeups

Personal repository dedicated to my CyberDefenders writeups.

CyberDefenders is a Blue Team and SOC training platform offering labs, cyber ranges, investigation scenarios, training paths and certifications.

Official website: https://cyberdefenders.org/

## Repository Goal

- SOC analysis
- Blue Team operations
- Digital Forensics
- Incident Response
- Threat Hunting
- Malware triage
- Network analysis
- Windows log analysis
- SIEM investigation
- Sigma and YARA detection
- Technical report writing

## Recommended Structure

```text
cyberdefenders-writeups/
├── README.md
├── labs/
│   ├── easy/
│   ├── medium/
│   └── hard/
├── templates/
│   ├── writeup-template-fr.md
│   └── writeup-template-en.md
├── notes/
│   ├── methodology.md
│   ├── tools.md
│   ├── commands-cheatsheet.md
│   └── soc-glossary.md
└── assets/
    └── screenshots/
```

## Writeup Format

Each writeup follows a clear investigation structure:

1. Lab context
2. Analysis objectives
3. Tools used
4. Methodology
5. Investigation steps
6. Observed findings
7. Timeline
8. Indicators of Compromise
9. Conclusion
10. Lessons learned

## Example Lab Structure

```text
labs/easy/lab-name/
├── README.md
├── writeup-fr.md
├── writeup-en.md
└── screenshots/
```

## Personal Methodology

Each investigation should remain readable and reproducible.

I document:

- Initial hypotheses
- Commands used
- Analyzed artifacts
- Suspicious elements
- Important evidence
- Errors or abandoned leads
- Defensive conclusions

## Possible Tools

Depending on the lab, the tools may include:

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
- SIEM or lab-provided interface

## Note

The writeups published here are written for educational and defensive purposes.

They should not be used to bypass learning or simply copy answers.

The goal is to understand the methodology, tools and reasoning behind each investigation.
