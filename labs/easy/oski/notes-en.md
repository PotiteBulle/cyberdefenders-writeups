# Oski Lab — Professional Investigation Notes

## 1. Purpose of These Notes

These notes document the reasoning process used during the Oski CyberDefenders lab.

They are not designed to provide direct answers.  
Their purpose is to explain how to approach the investigation, how to use the available evidence, and how to understand the malware behavior from a SOC and Blue Team perspective.

The main goal is to build a clear methodology that can be reused in future labs involving malware triage, sandbox analysis, and threat intelligence.

## 2. Lab Context

The Oski lab is part of the CyberDefenders **SOC Analyst Tier 1** practice track.

The scenario starts with an employee receiving an email titled **Urgent New Order**. The email contains an invoice-like attachment. After the user interacts with the attachment, the SIEM generates an alert related to a potentially malicious download.

The initial hypothesis is that the suspicious PPT file triggered the download or execution of a malware sample.

From a SOC perspective, this scenario represents a common infection chain:

```text
Phishing email
→ Suspicious attachment
→ Payload download or execution
→ Malware communication
→ Credential theft or data collection
→ Exfiltration
→ Cleanup or self-deletion
```

## 3. Investigation Scope

The investigation focuses on the following areas:

- identifying the provided artifact
- using hashes as threat intelligence pivots
- retrieving metadata from VirusTotal
- reviewing the ANY.RUN sandbox report
- understanding the malware family behavior
- identifying suspicious network activity
- reading the extracted malware configuration
- mapping observed behavior to MITRE ATT&CK
- understanding child process activity
- documenting useful IOCs
- avoiding unsafe local execution

## 4. Initial Artifact Handling

The lab archive contains a file named:

```text
hash.txt
```

This file does not contain the malware sample itself. It contains a hash that must be used to search online threat intelligence platforms.

This is important because the workflow is different from a case where the sample is directly provided. Here, the analyst must use a pivot-based approach.

Recommended initial commands:

```bash
ls -lah
file hash.txt
cat hash.txt
cat -A hash.txt
grep -Eo '[a-fA-F0-9]{32}' hash.txt
```

### Why `cat -A` Matters

The `cat -A` command helps display hidden characters, line endings, and spacing.

It is useful when:

- a hash does not return results on threat intelligence platforms
- a value may have been copied incorrectly
- invisible characters may be present
- the file uses Windows-style line endings

In this lab, carefully verifying the hash is an important early step.

## 5. Hash-Based Pivoting

A hash is used as a pivot to identify a file across different threat intelligence platforms.

Common hash types:

| Hash type | Length | Usage |
|---|---:|---|
| MD5 | 32 hexadecimal characters | Quick pivot, still widely used |
| SHA1 | 40 hexadecimal characters | Stronger than MD5, still commonly displayed |
| SHA256 | 64 hexadecimal characters | Preferred identifier for malware analysis |

Recommended workflow:

1. Extract the MD5 from `hash.txt`
2. Search the MD5 on VirusTotal
3. Collect the associated SHA1 and SHA256
4. Use the SHA256 as the main pivot
5. Search the SHA256 on ANY.RUN and Hybrid Analysis
6. Compare results across platforms

Once available, the SHA256 should be preferred because it identifies the file more reliably.

## 6. VirusTotal Analysis Notes

VirusTotal is useful for collecting static metadata and reputation information.

In this lab, VirusTotal can help identify:

- file type
- file size
- MD5, SHA1, and SHA256
- PE metadata
- creation or compilation timestamp
- first submission date
- first seen in the wild
- detection names
- malware family labels
- relationships with other files or URLs

Important sections to review:

```text
Details
Relations
Behavior
Community
Detection
```

### Static Metadata to Record

| Field | Why it matters |
|---|---|
| File type | Confirms whether the file is an executable, document, archive, or script |
| Magic | Gives a low-level file format description |
| File size | Helps compare the sample with other submissions |
| Creation Time | Can help answer timeline-related questions |
| Compilation Timestamp | Useful for PE malware triage |
| Names | Shows filenames used in submissions |
| Imports | Gives clues about Windows APIs or capabilities |
| Sections | May indicate packing, resources, or unusual structure |

### Caution About Timestamps

Timestamps should be interpreted carefully.

A malware timestamp can be:

- real
- modified by the attacker
- inherited from a builder
- generated automatically
- intentionally forged

The timestamp is useful for the lab, but it should not be treated as absolute proof of the malware's real origin.

## 7. ANY.RUN Analysis Notes

ANY.RUN provides dynamic sandbox analysis. It allows the analyst to observe how the sample behaves when executed in a controlled Windows environment.

Important areas to review:

```text
Processes
HTTP Requests
Connections
DNS Requests
Threats
Malware configuration
MITRE ATT&CK
Text report
IOC
Graph
```

The goal is not only to collect values, but to understand how the malware behaves during execution.

## 8. Process Tree Analysis

The process tree helps identify the main malware process and its child processes.

Useful questions:

- What is the initial process?
- What child processes are created?
- Are command-line interpreters used?
- Is `cmd.exe` or PowerShell involved?
- Are files created, deleted, or moved?
- Is there evidence of self-deletion?
- Are commands used for delay or cleanup?

In this lab, the process tree is especially important for understanding post-exfiltration behavior and cleanup actions.

### Command Line Review

Patterns to look for in command lines:

```cmd
timeout /t
ping 127.0.0.1
choice /t
del /f /q
rmdir
copy
move
attrib
```

These commands may indicate:

- execution delay
- file deletion
- self-cleanup
- evasion
- artifact removal
- temporary payload staging

## 9. Network Analysis Notes

Network analysis is used to identify communications with external infrastructure.

Useful ANY.RUN tabs:

```text
HTTP Requests
Connections
DNS Requests
Threats
```

### What to Look For

Focus on:

- non-Microsoft domains
- non-Google domains
- raw IP-based URLs
- unusual countries or hosting providers
- HTTP POST requests
- downloads of DLLs or executables
- repeated communication to the same endpoint
- C2-like paths ending in `.php`
- responses containing binary content
- traffic associated with the malware PID

### Filtering Noise

Sandbox environments often generate legitimate background traffic.

Examples of normal traffic:

```text
microsoft.com
windows.com
digicert.com
ocsp.*
crl.*
bing.com
login.live.com
settings-win.data.microsoft.com
watson.events.data.microsoft.com
```

This traffic can be documented, but it should not automatically be treated as malicious.

The most important entries are those associated with the malware process and containing suspicious URLs, IPs, paths, or content.

## 10. HTTP Request Analysis

HTTP requests are central in this lab.

Useful fields:

| Field | Purpose |
|---|---|
| Timestamp | Reconstructs the event order |
| Method | GET may indicate a download, POST may indicate communication or exfiltration |
| PID | Links the request to a process |
| Process name | Identifies whether the request came from malware or the system |
| URL | Identifies C2, payloads, or libraries |
| Content size | Helps distinguish beacons, text, or binaries |
| Content type | Identifies text, binary, or executable data |

### POST Requests

Repeated `POST` requests to a suspicious endpoint may indicate:

- beaconing
- check-in
- command exchange
- exfiltration
- configuration retrieval

### GET Requests

`GET` requests to DLL files may indicate:

- payload retrieval
- dependency download
- modular malware behavior
- execution of additional components

## 11. Malware Configuration Analysis

The `Malware configuration` panel is one of the most important parts of the ANY.RUN report.

It may contain:

- malware family
- C2 server
- encryption keys
- RC4 key
- targeted paths
- targeted extensions
- strings used by the malware
- directories used during execution

In this lab, the configuration is important for understanding the malware communication and how encoded data is processed.

### Why RC4 Matters

RC4 is a stream cipher sometimes used by malware to hide strings, configuration data, or traffic.

A visible RC4 key in a sandbox report often means that ANY.RUN automatically extracted the malware configuration.

Method:

1. Open the `Malware configuration` panel
2. Locate the `Keys` section
3. Check whether an RC4 key is displayed
4. Document the method without publishing every sensitive value directly
5. Explain how the information was located

## 12. MITRE ATT&CK Mapping

MITRE ATT&CK provides a standardized way to describe adversary behavior.

In this lab, the important tactic is **Credential Access**.

Look for:

- browser credential theft
- access to password stores
- user profile data collection
- credential extraction

Useful questions:

- Which tactic is displayed in ANY.RUN?
- Which technique is marked as relevant?
- Is it a main technique or a sub-technique?
- What behavior supports this mapping?
- Is this behavior consistent with the malware family?

The writeup should explain the behavior behind the ATT&CK ID rather than only listing an identifier.

## 13. Stealc Context

ANY.RUN associates the observed behavior with **Stealc**, a stealer family.

In general, stealers attempt to collect:

- browser passwords
- cookies
- autofill data
- cryptocurrency wallets
- files from user directories
- system information
- application credentials

In this lab, the observed behavior follows a stealer-like workflow:

```text
Execution
→ C2 communication
→ Configuration retrieval or validation
→ Data collection
→ Credential access
→ Exfiltration
→ Cleanup
```

## 14. Self-Deletion Logic

Self-deletion is a common malware cleanup technique.

The sandbox shows a child process performing delayed deletion logic.

The command line should be broken down into blocks:

```cmd
cmd.exe /c
timeout /t <delay>
del /f /q "<temporary malware path>"
del "<target directory>\*.dll"
exit
```

Component roles:

| Component | Role |
|---|---|
| `cmd.exe /c` | Runs a command and closes the shell |
| `timeout /t` | Waits a number of seconds |
| `del /f /q` | Deletes a file forcefully and quietly |
| temporary malware path | Location of the running sample |
| `*.dll` | Targets all DLL files in a directory |
| `exit` | Closes the command interpreter |

This behavior suggests cleanup after execution or exfiltration.

## 15. IOC Documentation

IOCs should be documented cleanly and consistently.

Recommended categories:

```text
Hashes
Network
Files
Directories
MITRE ATT&CK
Sandbox links
```

Recommended structure:

| Type | Value | Context |
|---|---|---|
| MD5 | value | Hash provided by the lab |
| SHA1 | value | Identified through VirusTotal |
| SHA256 | value | Main pivot |
| URL | partially masked | C2 endpoint |
| File | partially masked | Requested library |
| Directory | partially masked | DLL cleanup target |
| MITRE ID | partially masked | Credential Access technique |

For a public GitHub writeup, some direct answers can be partially masked to avoid copy-pasting.

## 16. Answer Masking Strategy

The writeup intentionally masks direct answers.

This approach preserves the educational value of the lab while still documenting the method.

Examples:

| Data type | Masking example |
|---|---|
| Timestamp | `2022-*** 17:**` |
| URL | `http://******.28.***/5c0******86.php` |
| DLL | `*******.dll` |
| RC4 key | `5*********************72*******74****` |
| MITRE ID | `T*******5*` |
| Directory | `C:\***********` |
| Seconds | `*` |

The goal is to provide enough context to understand the approach without publishing a direct answer sheet.

## 17. Investigation Checklist

### Artifact Handling

- [ ] Download the lab archive
- [ ] Extract the archive safely
- [ ] List extracted files
- [ ] Identify file types
- [ ] Read `hash.txt`
- [ ] Verify the hash with `cat -A`
- [ ] Extract the MD5 cleanly with regex

### Threat Intelligence

- [ ] Search the MD5 on VirusTotal
- [ ] Collect SHA1 and SHA256
- [ ] Use SHA256 as the main pivot
- [ ] Review file metadata
- [ ] Review PE information
- [ ] Review names and relations
- [ ] Search the hash on ANY.RUN
- [ ] Search the hash on Hybrid Analysis if needed

### Sandbox Analysis

- [ ] Open the ANY.RUN report
- [ ] Identify the main process
- [ ] Review HTTP requests
- [ ] Review DNS requests
- [ ] Review connections
- [ ] Distinguish system traffic from suspicious traffic
- [ ] Open malware configuration
- [ ] Review extracted keys
- [ ] Read the MITRE ATT&CK mapping
- [ ] Inspect child processes
- [ ] Identify deletion or cleanup commands

### Documentation

- [ ] Document the methodology
- [ ] Document important metadata
- [ ] Add an IOC table
- [ ] Add a timeline
- [ ] Add a MITRE ATT&CK mapping
- [ ] Mask direct answers in the public writeup
- [ ] Write lessons learned

## 18. Suggested Writeup Structure

Clean structure for this lab:

```text
1. General information
2. Useful links
3. Scenario context
4. Analysis objective
5. Environment preparation
6. hash.txt content
7. Hash correction
8. Identified hashes
9. VirusTotal metadata
10. PE information
11. ANY.RUN analysis
12. Network analysis
13. Malware configuration
14. MITRE ATT&CK analysis
15. Child process analysis
16. Questions with masked answers
17. IOCs
18. Timeline
19. Conclusion
20. Lessons learned
```

## 19. Skills Practiced

This lab helps practice:

- safe handling of suspicious artifacts
- hash-based investigation
- threat intelligence pivoting
- static metadata review
- sandbox report analysis
- network behavior interpretation
- malware configuration extraction
- MITRE ATT&CK mapping
- IOC documentation
- responsible public writeup writing

## 20. Personal Analyst Notes

The main lesson from this lab is that the first provided artifact is not always the malware.

Here, the investigation starts with a text file containing a hash. This requires a pivot-based approach rather than direct sample analysis.

Another important point is that a sandbox report contains both malicious activity and normal system activity. Windows, Microsoft, or certificate-related traffic should not be confused with C2 communication.

The analyst must correlate:

```text
process name
PID
URL
timestamp
content type
malware configuration
MITRE mapping
child process command line
```

This correlation is what makes it possible to correctly understand the observed behavior.
