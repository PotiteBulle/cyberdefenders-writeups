# Tomcat Takeover — Investigation Notes

## Purpose

These notes guide the analysis of the Tomcat Takeover lab without directly providing answers.

## Artifact

| File | Type |
|---|---|
| `web server.pcap` | PCAP |

## Hashes

| Type | Value |
|---|---|
| MD5 | `9ab62c5a2a1f8d0030f8240355305e7` |
| SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` |

## Analysis Areas

- IP conversations
- port scans
- HTTP requests
- User-Agents
- HTTP authentication
- Tomcat directories
- file upload
- reverse shell
- persistence

## Useful Wireshark Filters

```text
http
http.request
http.request.method == "GET"
http.request.method == "POST"
http.authorization
tcp
tcp.stream eq <number>
ip.addr == <IP>
tcp.port == <port>
```

## Recommended Methodology

1. Identify the main IP addresses
2. Identify the web server
3. Identify the IP scanning multiple ports
4. Review HTTP requests
5. Identify sensitive paths
6. Search for authentication attempts
7. Follow suspicious TCP streams
8. Look for uploads
9. Search for commands executed after compromise
10. Build a timeline
