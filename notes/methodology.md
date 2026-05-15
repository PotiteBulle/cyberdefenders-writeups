# Methodology

This document describes my general investigation methodology for CyberDefenders labs.

The goal is to keep my analysis structured, reproducible and useful for SOC, Blue Team, DFIR and threat hunting practice.

## 1. Read the Scenario

Before opening any file or tool, I start by reading the full lab scenario.

I try to identify:

- the suspected incident type
- the affected user or host
- the initial alert source
- the expected investigation objective
- the type of artifacts provided
- the tools likely required

Common scenarios include:

- phishing investigation
- malware triage
- network forensics
- memory forensics
- Windows event log analysis
- web attack investigation
- endpoint compromise
- data exfiltration

## 2. Inventory the Provided Artifacts

I always start by listing and identifying the provided files.

Useful commands:

```bash
ls -lah
file *
sha256sum *
md5sum *
```

For archives:

```bash
unzip -l archive.zip
7z l archive.7z
tar -tf archive.tar
```

The goal is to understand what was provided before performing any deeper analysis.

## 3. Avoid Unsafe Execution

Suspicious files should not be executed directly on the analysis machine.

Safe first steps include:

- checking file type
- calculating hashes
- reviewing metadata
- extracting strings
- using sandbox reports
- using threat intelligence platforms
- analyzing logs or captures offline

## 4. Use Hashes as Pivots

Hashes are useful for identifying files across multiple platforms.

Recommended order:

1. Extract MD5, SHA1 and SHA256 when available
2. Search the hash on VirusTotal
3. Search the hash on ANY.RUN
4. Search the hash on Hybrid Analysis
5. Search the hash on MalwareBazaar if relevant
6. Compare metadata and behavior across sources

SHA256 should be preferred once available.

## 5. Separate Evidence From Assumptions

During the investigation, I separate:

- confirmed evidence
- likely assumptions
- hypotheses
- false positives
- abandoned leads

Example:

| Type | Example |
|---|---|
| Evidence | A process executed a command shown in logs |
| Hypothesis | The command may be used for persistence |
| False positive | Microsoft telemetry traffic not related to malware |
| Abandoned lead | A domain looked suspicious but resolved to a known vendor |

## 6. Build a Timeline

A timeline helps understand the incident order.

Useful timeline fields:

| Time | Event | Source | Comment |
|---|---|---|---|
| Timestamp | Event description | Log, PCAP, sandbox, metadata | Analyst note |

The timeline should highlight:

- initial access
- execution
- network communication
- file creation
- credential access
- exfiltration
- cleanup
- detection

## 7. Extract IOCs

Indicators of compromise should be documented clearly.

Common IOC types:

- hashes
- IP addresses
- domains
- URLs
- filenames
- file paths
- registry keys
- mutexes
- email addresses
- user agents
- MITRE ATT&CK techniques

Each IOC should include context.

## 8. Map to MITRE ATT&CK

When possible, I map observed behavior to MITRE ATT&CK.

I try to document:

- tactic
- technique
- technique ID
- observed behavior
- supporting evidence

Example:

| Tactic | Technique | ID | Evidence |
|---|---|---|---|
| Credential Access | Credentials from Web Browsers | T1555.003 | Browser credential theft observed in sandbox |

## 9. Write the Report

Each writeup should explain the investigation method, not only the final answers.

Recommended sections:

1. General information
2. Scenario context
3. Analysis objective
4. Environment and tools
5. Artifacts
6. Methodology
7. Technical analysis
8. Questions and masked answers
9. IOCs
10. Timeline
11. Conclusion
12. Lessons learned

## 10. Mask Direct Answers When Needed

For public GitHub writeups, direct answers may be partially masked.

The goal is to preserve the educational value of the lab and avoid publishing a simple answer sheet.

Examples:

```text
Full answer: not published directly
Masked answer: http://******.example/***.php
Method: documented clearly
```

## 11. Final Review Checklist

Before publishing a writeup:

- [ ] The scenario is summarized clearly
- [ ] The tools are listed
- [ ] Commands are documented
- [ ] Evidence is separated from assumptions
- [ ] IOCs are cleanly formatted
- [ ] MITRE ATT&CK mapping is included when relevant
- [ ] Direct answers are masked if needed
- [ ] The conclusion is defensive and professional
- [ ] No sensitive or unnecessary screenshots are included
