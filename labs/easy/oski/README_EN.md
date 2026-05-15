# Oski Lab

## Platform

CyberDefenders

Official link: https://cyberdefenders.org/blueteam-ctf-challenges/oski/

## Lab Information

| Field | Value |
|---|---|
| Lab name | Oski |
| Track | SOC Analyst Tier 1 |
| Level | Level 1 |
| Difficulty | Easy |
| Estimated time | 30 minutes |
| Category | Threat Intel |
| Suggested tools | VirusTotal, ANY.RUN |

## Lab Objective

The objective of this lab is to analyze an ANY.RUN sandbox report in order to identify the behavior of a stealer-type malware.

The scenario requires extracting important information such as the malware creation date, the C2 server, the libraries contacted after infection, the RC4 key used, the observed MITRE ATT&CK techniques, the directory targeted for DLL deletion, and the delay before self-deletion.

## Scenario

An employee receives an email titled **Urgent New Order** from a client.

When attempting to access the attached invoice, the employee discovers that the document contains false order information.

Later, a SIEM alert is generated regarding the download of a potentially malicious file.

An initial investigation indicates that the PPT file may be responsible for this download.

## Lab Questions

| Number | Topic |
|---|---|
| Q1 | Malware creation time |
| Q2 | C2 server contacted by the PPT file |
| Q3 | First library requested after infection |
| Q4 | RC4 key used to decrypt a base64-encoded string |
| Q5 | Main MITRE ATT&CK technique used to steal passwords |
| Q6 | Directory targeted for DLL deletion |
| Q7 | Time before self-deletion after exfiltration |

## Folder Files

| File | Description |
|---|---|
| `README.md` | Lab overview |
| `writeup-fr.md` | Full writeup in French |
| `writeup-en.md` | Full writeup in English |
| `notes-fr.md` | Investigation notes in French |
| `notes-en.md` | Investigation notes in English |
| `iocs.md` | Indicators of compromise |
| `screenshots/` | Screenshots used to document the lab |

## Status

In progress

## Disclaimer

This lab is documented only for educational, defensive, and controlled cybersecurity training purposes.

No real-world offensive or unauthorized activity is performed.
