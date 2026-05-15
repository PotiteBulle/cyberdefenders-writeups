# Commands Cheatsheet

This document contains useful commands for CyberDefenders labs.

## File Identification

```bash
file suspicious_file
ls -lah
stat suspicious_file
```

## Hashing

```bash
md5sum suspicious_file
sha1sum suspicious_file
sha256sum suspicious_file
```

Hash all files in a directory:

```bash
find . -type f -exec sha256sum {} \;
```

## Archive Handling

List ZIP content:

```bash
unzip -l archive.zip
```

Extract password-protected ZIP:

```bash
unzip -P password archive.zip -d output_dir
```

List 7z archive:

```bash
7z l archive.7z
```

Extract 7z archive:

```bash
7z x archive.7z -ooutput_dir
```

## Text Inspection

```bash
cat file.txt
cat -A file.txt
less file.txt
head file.txt
tail file.txt
```

Search for a keyword:

```bash
grep -i "keyword" file.txt
```

Search recursively:

```bash
grep -Rin "keyword" .
```

Extract MD5 hashes:

```bash
grep -Eo '[a-fA-F0-9]{32}' file.txt
```

Extract SHA1 hashes:

```bash
grep -Eo '[a-fA-F0-9]{40}' file.txt
```

Extract SHA256 hashes:

```bash
grep -Eo '[a-fA-F0-9]{64}' file.txt
```

## Strings

```bash
strings suspicious_file | less
strings suspicious_file | grep -i "http"
strings suspicious_file | grep -i "password"
```

## Metadata

```bash
exiftool suspicious_file
```

## JSON

Pretty print JSON:

```bash
cat file.json | jq .
```

Extract a field:

```bash
cat file.json | jq '.field'
```

## PCAP Analysis With tshark

Basic summary:

```bash
tshark -r capture.pcap
```

List protocols:

```bash
tshark -r capture.pcap -q -z io,phs
```

Extract DNS queries:

```bash
tshark -r capture.pcap -Y "dns.qry.name" -T fields -e dns.qry.name
```

Extract HTTP hosts:

```bash
tshark -r capture.pcap -Y "http.host" -T fields -e http.host
```

Extract HTTP requests:

```bash
tshark -r capture.pcap -Y "http.request" -T fields -e ip.src -e ip.dst -e http.host -e http.request.uri
```

## Windows Event Logs With PowerShell

List recent security events:

```powershell
Get-WinEvent -LogName Security -MaxEvents 20
```

Filter by Event ID:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624}
```

Export events:

```powershell
Get-WinEvent -LogName Security -MaxEvents 100 |
Export-Csv .\security_events.csv -NoTypeInformation
```

## Sysmon Logs

Process creation:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; Id=1}
```

Network connections:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; Id=3}
```

## Volatility 3

List processes:

```bash
volatility3 -f memory.raw windows.pslist
```

Process tree:

```bash
volatility3 -f memory.raw windows.pstree
```

Command lines:

```bash
volatility3 -f memory.raw windows.cmdline
```

Network connections:

```bash
volatility3 -f memory.raw windows.netstat
```

DLL list:

```bash
volatility3 -f memory.raw windows.dlllist
```

## YARA

Scan a file:

```bash
yara rule.yar suspicious_file
```

Scan a directory recursively:

```bash
yara -r rule.yar directory/
```

## Notes

Commands should be adapted to the lab context.

In writeups, I document only the commands that contributed to the investigation.
