# Tools

This document lists tools that may be useful for CyberDefenders labs.

The tools are grouped by investigation type.

## Threat Intelligence

### VirusTotal

Useful for:

- hash lookup
- file reputation
- metadata
- detection names
- relations
- community notes
- behavior summaries

Website:

```text
https://www.virustotal.com/
```

### ANY.RUN

Useful for:

- dynamic malware analysis
- process tree review
- HTTP requests
- DNS requests
- connections
- malware configuration
- MITRE ATT&CK mapping
- IOC extraction

Website:

```text
https://app.any.run/
```

### Hybrid Analysis

Useful for:

- sandbox reports
- behavioral indicators
- dropped files
- network indicators
- MITRE ATT&CK data

Website:

```text
https://www.hybrid-analysis.com/
```

### MalwareBazaar

Useful for:

- malware hash lookup
- malware family search
- sample metadata
- threat intelligence pivoting

Website:

```text
https://bazaar.abuse.ch/
```

### URLhaus

Useful for:

- malicious URL lookup
- payload hosting indicators
- URL-based threat intelligence

Website:

```text
https://urlhaus.abuse.ch/
```

## Network Analysis

### Wireshark

Useful for:

- PCAP analysis
- DNS traffic
- HTTP traffic
- TLS indicators
- suspicious connections
- file extraction from captures

### tshark

Command-line version of Wireshark.

Useful for:

- filtering packets
- extracting fields
- automation
- quick summaries

Example:

```bash
tshark -r capture.pcap
```

## Malware Triage

### file

Identifies file type.

```bash
file suspicious_file
```

### strings

Extracts printable strings.

```bash
strings suspicious_file | less
```

### exiftool

Extracts metadata.

```bash
exiftool suspicious_file
```

### sha256sum

Calculates SHA256 hash.

```bash
sha256sum suspicious_file
```

### md5sum

Calculates MD5 hash.

```bash
md5sum suspicious_file
```

## Memory Forensics

### Volatility

Useful for:

- process listing
- command line extraction
- network connections
- DLL listing
- registry analysis
- malware hunting in memory

Example:

```bash
volatility3 -f memory.raw windows.pslist
```

## Disk and File Forensics

### Autopsy

Useful for:

- filesystem analysis
- deleted file recovery
- timeline analysis
- artifact extraction

### FTK Imager

Useful for:

- disk image preview
- file extraction
- evidence mounting

## Windows Log Analysis

### Event Viewer

Useful for:

- Windows event logs
- Security logs
- Sysmon logs
- PowerShell logs

### PowerShell

Useful for:

- parsing logs
- filtering events
- exporting results
- incident triage

Example:

```powershell
Get-WinEvent -LogName Security -MaxEvents 20
```

## Detection Engineering

### Sigma

Useful for:

- generic detection rules
- SIEM-friendly logic
- detection portability

### YARA

Useful for:

- malware pattern matching
- file scanning
- string-based detection

## Text Processing

### grep

Useful for searching text.

```bash
grep -i "keyword" file.txt
```

### jq

Useful for JSON parsing.

```bash
cat file.json | jq .
```

### awk

Useful for extracting fields.

```bash
awk '{print $1}' file.txt
```

### sed

Useful for text replacement and filtering.

```bash
sed 's/old/new/g' file.txt
```

## Encoding and Decoding

### CyberChef

Useful for:

- base64
- hex
- URL decoding
- XOR
- compression
- data transformation

Website:

```text
https://gchq.github.io/CyberChef/
```

## Notes

The tool choice depends on the lab type.

I try to document:

- which tool was used
- why it was used
- what evidence it provided
- what limitations it had
