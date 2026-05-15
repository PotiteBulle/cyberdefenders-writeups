# CyberDefenders — Oski Lab

## 1. General Information

| Field | Value |
|---|---|
| Platform | CyberDefenders |
| Track | SOC Analyst Tier 1 |
| Level | Level 1 |
| Lab | Oski |
| Category | Threat Intel |
| Difficulty | Easy |
| Estimated time | 30 minutes |
| Main tools | ANY.RUN, VirusTotal, Hybrid Analysis |
| Status | Completed |

## 2. Useful Links

| Source | Link |
|---|---|
| CyberDefenders | https://cyberdefenders.org/blueteam-ctf-challenges/oski/ |
| VirusTotal | `https://www.virustotal.com/gui/file/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` |
| ANY.RUN | `https://app.any.run` |

## 3. Scenario Context

An employee received an email titled **Urgent New Order** from a client.

After trying to open the attached invoice, the employee noticed that the document contained false order information.

Later, the SIEM generated an alert related to the download of a potentially malicious file.

The initial investigation indicates that the PPT file may be responsible for the download.

## 4. Analysis Objective

The goal of this lab is to analyze a sandbox report and threat intelligence data in order to identify the malware behavior, its network communications, configuration, MITRE ATT&CK techniques, and evasion mechanisms.

This lab focuses on **Threat Intel**, **malware triage**, **sandbox analysis**, and **SOC investigation**.

## 5. Environment Preparation

The analysis was started from a Kali Linux machine in the following directory:

```bash
~/Documents/Oski_Lab/
```

The file downloaded from CyberDefenders is a password-protected ZIP archive:

```text
140-Oski.zip
```

The password provided by the platform is:

```text
cyberdefenders.org
```

Archive extraction:

```bash
unzip -P cyberdefenders.org 140-Oski.zip -d temp_extract_dir
```

After extraction, the archive contains a single file:

```text
hash.txt
```

File type verification:

```bash
file hash.txt
```

Result:

```text
hash.txt: ASCII text
```

## 6. hash.txt Content

Command used:

```bash
cat hash.txt
```

Observed content:

```text
MD5 Hash: 12c1842c3ccafe7408c23ebf292ee3d9

Use this hash on online threat intel platforms (e.g., VirusTotal, Hybrid Analysis) to complete the lab analysis.
```

The `hash.txt` file does not contain the malware directly. It provides an MD5 hash used as a pivot for threat intelligence research.

## 7. Hash Correction

The hash was initially copied incorrectly.

Incorrect hash tested at first:

```text
12c1842c3ccfe7408c23ebf292ee3d9
```

After verifying the file content with `cat -A`, the correct hash is:

```text
12c1842c3ccafe7408c23ebf292ee3d9
```

Command used:

```bash
cat -A hash.txt
```

The difference is located in the `3ccafe` sequence.

## 8. Identified Hashes

Searching the correct MD5 on VirusTotal made it possible to identify the hashes associated with the sample.

| Type | Value |
|---|---|
| MD5 | `12c1842c3ccafe7408c23ebf292ee3d9` |
| SHA1 | `4b1af84cc11a8b1e290a18a4222a49526eadd10` |
| SHA256 | `a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` |

The SHA256 becomes the main pivot for the rest of the analysis, as it identifies the file more reliably than the MD5 hash.

## 9. VirusTotal Metadata

The basic properties observed in VirusTotal indicate that the file is a 32-bit Windows executable.

| Field | Value |
|---|---|
| Type | Win32 EXE |
| Magic | PE32 executable GUI Intel 80386, for MS Windows |
| Size | 311.50 KB |
| Creation Time | 2022-************** 17:******* UTC |
| First Seen In The Wild | 2023-09-23 22:33:33 UTC |
| First Submission | 2023-09-23 22:02:55 UTC |
| Last Submission | 2025-09-29 02:35:54 UTC |
| Last Analysis | 2026-05-13 19:04:16 UTC |

## 10. Observed PE Information

The Portable Executable information shows the following elements:

| Field | Value |
|---|---|
| Target architecture | Intel 386 or compatible processors |
| Compilation Timestamp | 2022-************** 17:******* UTC |
| Contained sections | 3 |
| Visible sections | `.text`, `.data`, `.rsrc` |
| Visible imports | `KERNEL32.dll`, `USER32.dll`, `GDI32.dll`, `ADVAPI32.dll` |

These elements confirm that the sample is a Windows executable and not a simple document.

## 11. ANY.RUN Analysis

The ANY.RUN report associated with the sample allows the malware behavior to be observed in a sandbox environment.

Link:

```text
https://app.any.run/
```

The sample is identified under the following name:

```text
VPN.exe
```

The ANY.RUN report associates the sample with the following tags:

```text
stealc, stealer, loader, oski
```

The tracker indicates an association with:

```text
Loader, Stealc, Stealer
```

## 12. Network Analysis

The `HTTP Requests` tab shows several HTTP requests made by the `VPN.exe` process.

The C2 server is visible in the malware configuration and in the HTTP requests.

Answer intentionally partially masked:

```text
http://*******.28.***/5c06***86.php
```

Several requests are sent to this server, including `POST` requests to a PHP file.

A `GET` request to a DLL library is also observed after infection.

Answer intentionally partially masked:

```text
*******.dll
```

## 13. Malware Configuration

The `Malware configuration` tab in ANY.RUN identifies the malware as **Stealc**.

The configuration contains several important elements:

- the C2 server
- an RC4 key
- strings related to user directories
- targeted extensions
- paths used during execution

The RC4 key used to decrypt a base64-encoded string is visible in the configuration.

Answer intentionally partially masked:

```text
5***462144***074****
```

## 14. MITRE ATT&CK Analysis

The MITRE ATT&CK tab in the ANY.RUN report shows several observed tactics and techniques.

For password theft, the main technique displayed is related to credentials stored in web browsers.

Answer intentionally partially masked:

```text
T***5*
```

The observed technique corresponds to:

```text
Credentials from Web Browsers
```

## 15. Child Process Analysis

The main observed process is:

```text
VPN.exe
```

A child `cmd.exe` process is launched with a deletion and self-deletion command.

Command observed in the report:

```cmd
cmd.exe /c timeout /t 5 & del /f /q "C:\Users\admin\AppData\Local\Temp\VPN.exe" & del "C:\ProgramData\*.dll" & exit
```

This command line shows two important behaviors:

- deletion of the temporary `VPN.exe` binary
- deletion of DLL files in a specific directory

Directory targeted for DLL deletion, intentionally partially masked:

```text
C:\*******
```

Time before self-deletion, intentionally masked:

```text
*
```

## 16. Lab Question Answers

The answers are intentionally partially masked to avoid direct copy-pasting and to encourage understanding the analysis method.

### Q1 — Malware Creation Time

**Question:**  
Determining the creation time of the malware can provide insights into its origin. What was the time of malware creation?

**Masked answer:**

```text
2022-*** 17:**
```

**Method:**  
The answer was found in the VirusTotal basic properties of the file. The `Creation Time` field shows `2022-*** 17:****** UTC`. The expected CyberDefenders format is `YYYY-MM-DD HH:MM`.

---

### Q2 — C2 Server

**Question:**  
Identifying the command and control server that the malware communicates with can help trace back to the attacker. Which C2 server does the malware in the PPT file communicate with?

**Masked answer:**

```text
http://******.28.***/5c0******86.php
```

**Method:**  
The answer is visible in `Malware configuration`, in the `C2` field, and also in the HTTP requests made by the `VPN.exe` process.

---

### Q3 — First Library Requested After Infection

**Question:**  
Identifying the initial actions of the malware post-infection can provide insights into its primary objectives. What is the first library that the malware requests post-infection?

**Masked answer:**

```text
*******.dll
```

**Method:**  
The answer is found in the chronological HTTP requests. The first library downloaded after infection is a DLL file requested through a `GET` request.

---

### Q4 — RC4 Key

**Question:**  
By examining the provided ANY.RUN report, what RC4 key is used by the malware to decrypt its base64-encoded string?

**Masked answer:**

```text
5*********************72*******74****
```

**Method:**  
The key is visible in `Malware configuration`, under the `Keys` section, in the `RC4` field.

---

### Q5 — MITRE ATT&CK Technique Related to Password Theft

**Question:**  
By examining the MITRE ATT&CK techniques displayed in the ANY.RUN sandbox report, identify the main MITRE technique the malware uses to steal the user's password.

**Masked answer:**

```text
T*******5*
```

**Method:**  
The answer is found in the MITRE ATT&CK tab of the ANY.RUN report. The displayed technique relates to credentials stored in web browsers.

---

### Q6 — Directory Targeted for DLL Deletion

**Question:**  
By examining the child processes displayed in the ANY.RUN sandbox report, which directory does the malware target for the deletion of all DLL files?

**Masked answer:**

```text
C:\***********
```

**Method:**  
The answer is visible in the command line of the child `cmd.exe` process. The command contains a deletion of `*.dll` files in a specific directory.

---

### Q7 — Time Before Self-Deletion

**Question:**  
Understanding the malware's behavior post-data exfiltration can give insights into evasion techniques. By analyzing the child process, after successfully exfiltrating the user's data, how many seconds does it take for the malware to self-delete?

**Masked answer:**

```text
*
```

**Method:**  
The answer is visible in the command line of the child `cmd.exe` process, through the `timeout /t` instruction.

## 17. Indicators of Compromise

### Hashes

| Type | Value | Description |
|---|---|---|
| MD5 | `12c1842c3ccafe7408c23ebf292ee3d9` | Sample hash provided by the lab |
| SHA1 | `4b1af84cc11a8b1e290a18a4222a49526eadd10` | Hash identified through VirusTotal |
| SHA256 | `a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` | Hash identified through VirusTotal |

### Network

| Type | Value | Description |
|---|---|---|
| C2 | `http://*******.28.***/5c0*******86.php` | Command and control server, partially masked |
| Requested file | `sqlite*.dll` | First requested library, partially masked |

### Files and Paths

| Type | Value | Description |
|---|---|---|
| Observed name | `VPN.exe` | Sample name in ANY.RUN |
| Targeted directory | `C:\***********` | Directory targeted for DLL deletion, partially masked |
| Temporary file | `C:\Users\admin\AppData\Local\Temp\VPN.exe` | Temporary binary path observed in the command line |

### MITRE ATT&CK

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Credentials from Web Browsers | `T*******5*` |

## 18. Timeline

| Time | Event | Source |
|---|---|---|
| 2022-****** 17:******* UTC | Malware creation or compilation | VirusTotal |
| Not specified | Receipt of the **Urgent New Order** email | CyberDefenders scenario |
| Not specified | Opening of the suspicious PPT file | CyberDefenders scenario |
| During sandbox execution | Communication with the C2 server | ANY.RUN |
| During sandbox execution | Download of DLL libraries | ANY.RUN |
| After exfiltration | Delayed deletion of the binary | Child `cmd.exe` process |

## 19. Conclusion

The Oski lab highlights an infection scenario involving a file associated with **Stealc stealer** malware.

The analysis begins with an MD5 hash provided in `hash.txt`, then continues with VirusTotal to identify the full hashes, PE metadata, and malware creation time.

The ANY.RUN report then makes it possible to observe the sample's dynamic behavior, including:

- communications with the C2 server
- malware configuration
- the RC4 key used
- downloaded libraries
- MITRE ATT&CK techniques associated with credential theft
- deletion and self-deletion commands

This lab is a good introduction to sandbox analysis, threat intelligence, and SOC investigation documentation.

## 20. Lessons Learned

- Always inventory the provided files before starting the analysis
- Verify a hash when threat intelligence platforms return no result
- Use VirusTotal to retrieve the full hashes associated with a sample
- Use ANY.RUN to analyze the dynamic behavior of malware
- Distinguish legitimate system traffic from malware-related traffic
- Read the extracted configuration of a stealer
- Map observed behavior to MITRE ATT&CK
- Document answers with a method rather than only providing raw values
