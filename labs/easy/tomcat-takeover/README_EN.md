# Tomcat Takeover Lab

## Platform

CyberDefenders

Official link: https://cyberdefenders.org/blueteam-ctf-challenges/tomcat-takeover/

## Information

| Field | Value |
|---|---|
| Lab | Tomcat Takeover |
| Track | SOC Analyst Tier 1 |
| Level | Level 3 |
| Difficulty | Easy |
| Category | Network Forensics |
| Estimated time | 30 minutes |
| Suggested tools | Wireshark, NetworkMiner |
| Status | In progress |

## Summary

This lab consists of analyzing a PCAP file to identify suspicious activity targeting an Apache Tomcat web server.

The scenario states that the SOC team detected suspicious activity on an internal web server. The PCAP should help understand the scope of the attack and the actions performed by the attacker.

The analysis focuses on **Network Forensics**, **Wireshark**, **HTTP analysis**, **Tomcat administration**, and **web server compromise**.

## Provided Artifact

| File | Type | Comment |
|---|---|---|
| `web server.pcap` | PCAP | Network capture to analyze |

## Artifact Hashes

| Type | Value |
|---|---|
| MD5 | `9ab62c5a2a1f8d0030f8240355305e7` |
| SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` |

## Learning Objectives

The main objectives of the lab are:

- analyze a network capture with Wireshark
- identify scanning activity
- identify the attacker source IP address
- identify open ports and the web administration panel
- recognize enumeration tools
- analyze an HTTP brute-force attempt
- identify a successful authentication
- find a malicious uploaded file
- understand reverse shell behavior
- identify a persistence command

## Lab Questions

| Number | Topic |
|---|---|
| Q1 | Source IP address responsible for scanning requests |
| Q2 | Country associated with the attacker IP address |
| Q3 | Port providing access to the web admin panel |
| Q4 | Tool used during enumeration |
| Q5 | Specific directory related to the admin panel |
| Q6 | Credentials successfully used for login |
| Q7 | Name of the uploaded malicious file |
| Q8 | Scheduled command used to maintain persistence |

## Folder Structure

| File | Description |
|---|---|
| `README.md` | Lab overview in French |
| `README_EN.md` | Lab overview in English |
| `README_TOMCAT_TAKEOVER.md` | Detailed lab overview in French |
| `README_TOMCAT_TAKEOVER_EN.md` | Detailed lab overview in English |
| `writeup-fr.md` | French writeup |
| `writeup-en.md` | English writeup |
| `notes.md` | French investigation notes |
| `notes-en.md` | English investigation notes |
| `iocs.md` | French indicators of compromise |
| `iocs-en.md` | English indicators of compromise |

## Documentation Choice

No screenshots are required in this folder.

The lab can be documented with a text-based approach: commands used, Wireshark observations, filters, HTTP streams, IOCs, timeline, and MITRE ATT&CK mapping.

## Disclaimer

Answers will be partially masked in public writeups to avoid direct copy-pasting and preserve the educational value of the lab.

This lab is documented only for educational, defensive, and controlled cybersecurity training purposes.
