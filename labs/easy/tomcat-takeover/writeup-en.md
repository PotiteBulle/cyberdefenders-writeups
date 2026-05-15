# CyberDefenders — Tomcat Takeover Lab

## 1. General Information

| Field | Value |
|---|---|
| Platform | CyberDefenders |
| Track | SOC Analyst Tier 1 |
| Level | Level 3 |
| Lab | Tomcat Takeover |
| Category | Network Forensics |
| Difficulty | Easy |
| Estimated time | 30 minutes |
| Main tools | Wireshark, NetworkMiner, tshark |
| Status | In progress |

## 2. Useful Links

| Source | Link |
|---|---|
| CyberDefenders | https://cyberdefenders.org/blueteam-ctf-challenges/tomcat-takeover/ |

## 3. Scenario Context

The SOC team identified suspicious activity on an internal web server. A network capture is provided to understand the scope of the attack and determine whether the Apache Tomcat server was compromised.

## 4. Analysis Objective

The goal is to analyze the PCAP file to identify the attacker IP, targeted ports, administration panel, enumeration tools, valid credentials, malicious uploaded file, and persistence command.

## 5. Environment Preparation

```bash
mkdir -p ~/Documents/Tomcat_Takeover_Lab
cd ~/Documents/Tomcat_Takeover_Lab
unzip -P cyberdefenders.org 135-TomcatTakeover.zip -d temp_extract_dir
cd temp_extract_dir
```

## Provided Artifact

| File | Type | Comment |
|---|---|---|
| `web server.pcap` | PCAP | Network capture to analyze |

## Artifact Hashes

| Type | Value |
|---|---|
| MD5 | `9ab62c5a2a1f8d0030f8240355305e7` |
| SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` |


## 6. Artifact Verification

```bash
file web\ server.pcap
sha256sum web\ server.pcap
md5sum web\ server.pcap
```

Observed result:

```text
web server.pcap: pcap capture file, microsecond ts (little-endian) - version 2.4
```

## 7. Methodology

1. Identify hosts present in the PCAP
2. Detect network scanning activity
3. Identify the most suspicious source IP address
4. Determine the country associated with the attacker IP
5. Analyze targeted ports
6. Identify the Tomcat administration panel port
7. Review HTTP requests
8. Inspect User-Agents and enumeration tools
9. Identify access to the administration panel
10. Analyze login attempts
11. Identify valid credentials
12. Search for file uploads
13. Identify reverse shell traces
14. Search for a persistence command
15. Document IOCs and timeline

## 8. Useful Commands and Filters

### IP conversations

```bash
tshark -r web\ server.pcap -q -z conv,ip
```

### HTTP requests

```bash
tshark -r web\ server.pcap -Y "http.request" -T fields -e frame.time_relative -e ip.src -e ip.dst -e tcp.dstport -e http.request.method -e http.host -e http.request.uri -e http.user_agent
```

### POST requests

```bash
tshark -r web\ server.pcap -Y "http.request.method == POST" -T fields -e frame.time_relative -e ip.src -e ip.dst -e tcp.dstport -e http.request.uri -e http.user_agent
```

### HTTP authentication

```bash
tshark -r web\ server.pcap -Y "http.authorization" -T fields -e ip.src -e ip.dst -e http.authorization
```

### Wireshark filters

```text
http
http.request
http.request.method == "GET"
http.request.method == "POST"
http.authorization
tcp.port == 8080
ip.addr == <IP_TO_COMPLETE>
tcp.stream eq <NUMBER>
```

## 9. Lab Question Answers

Answers will be completed during analysis and partially masked before public release.

### Q1 — Source IP Responsible for the Scan

**Masked answer:**

```text
To complete
```

**Method:** analyze IP/TCP conversations and identify the host initiating many connections toward multiple ports.

### Q2 — Country Associated With the Attacker IP

**Masked answer:**

```text
To complete
```

**Method:** use a GeoIP source based on the IP identified in Q1.

### Q3 — Web Admin Panel Port

**Masked answer:**

```text
To complete
```

**Method:** identify open ports and determine which one exposes the Tomcat administration interface.

### Q4 — Tool Used During Enumeration

**Masked answer:**

```text
To complete
```

**Method:** review HTTP User-Agents and request patterns.

### Q5 — Discovered Administration Directory

**Masked answer:**

```text
To complete
```

**Method:** filter HTTP requests and search for Tomcat administration-related paths.

### Q6 — Successfully Used Credentials

**Masked answer:**

```text
To complete
```

**Method:** analyze Authorization headers and HTTP responses indicating successful login.

### Q7 — Uploaded Malicious File

**Masked answer:**

```text
To complete
```

**Method:** search POST requests related to file upload or deployment.

### Q8 — Scheduled Persistence Command

**Masked answer:**

```text
To complete
```

**Method:** follow post-compromise streams to identify a recurring or scheduled command.

## 10. IOCs To Complete

| Type | Value | Description |
|---|---|---|
| PCAP MD5 | `9ab62c5a2a1f8d0030f8240355305e7` | Artifact hash |
| PCAP SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` | Artifact hash |
| Attacker IP | To complete | Scan and attack source |
| Server IP | To complete | Targeted web server |
| Admin port | To complete | Administration panel port |
| Admin directory | To complete | Discovered path |
| Uploaded file | To complete | Malicious file |
| Persistence command | To complete | Scheduled command |

## 11. Provisional Timeline

| Time | Event | Source |
|---|---|---|
| To complete | Scan starts | PCAP |
| To complete | Admin panel discovered | HTTP |
| To complete | Brute-force or login attempt | HTTP |
| To complete | Successful authentication | HTTP |
| To complete | Malicious file upload | HTTP |
| To complete | Reverse shell | Network stream |
| To complete | Persistence setup | Observed command |

## 12. Provisional Conclusion

This lab helps practice PCAP-based network analysis and reconstruct an Apache Tomcat web compromise from reconnaissance to persistence.
