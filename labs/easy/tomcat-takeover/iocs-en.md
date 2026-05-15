# Tomcat Takeover — IOCs

## Sources

| Source | Usage |
|---|---|
| CyberDefenders | Scenario and questions |
| PCAP | Network evidence |
| Wireshark | Stream analysis |
| NetworkMiner | Additional network view |

## Artifact Hashes

| Type | Value | Comment |
|---|---|---|
| MD5 | `9ab62c5a2a1f8d0030f8240355305e7` | PCAP file hash |
| SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` | PCAP file hash |

## Network

| Type | Value | Role | Comment |
|---|---|---|---|
| Attacker IP | To complete | Attack source | Responsible for scanning and suspicious requests |
| Server IP | To complete | Target | Tomcat web server |
| Admin port | To complete | Administration | Administration panel port |

## HTTP

| Type | Value | Comment |
|---|---|---|
| Admin directory | To complete | Directory discovered after enumeration |
| Tool User-Agent | To complete | Identified enumeration tool |
| Credentials | To mask | Credentials successfully used |
| Uploaded file | To complete | Uploaded malicious file |

## Provisional MITRE ATT&CK

| Tactic | Technique | ID | Comment |
|---|---|---|---|
| Reconnaissance | Active Scanning | T1595 | Port scanning observed |
| Discovery | File and Directory Discovery | T1083 | Directory enumeration |
| Credential Access | Brute Force | T1110 | Login attempts |
| Initial Access | Exploit Public-Facing Application | T1190 | Compromise of exposed web service |
| Persistence | Scheduled Task/Job | T1053 | Scheduled persistence to confirm |
