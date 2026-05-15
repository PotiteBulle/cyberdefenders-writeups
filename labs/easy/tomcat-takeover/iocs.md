# Tomcat Takeover — IOCs

## Sources

| Source | Usage |
|---|---|
| CyberDefenders | Scénario et questions |
| PCAP | Preuves réseau |
| Wireshark | Analyse des flux |
| NetworkMiner | Vue réseau complémentaire |

## Hashes de l’artefact

| Type | Valeur | Commentaire |
|---|---|---|
| MD5 | `9ab62c5a2a1f8d0030f8240355305e7` | Hash du fichier PCAP |
| SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` | Hash du fichier PCAP |

## Réseau

| Type | Valeur | Rôle | Commentaire |
|---|---|---|---|
| IP attaquant | À compléter | Source de l’attaque | Responsable du scan et des requêtes suspectes |
| IP serveur | À compléter | Cible | Serveur web Tomcat |
| Port admin | À compléter | Administration | Port du panneau d’administration |

## HTTP

| Type | Valeur | Commentaire |
|---|---|---|
| Répertoire admin | À compléter | Répertoire découvert après énumération |
| User-Agent outil | À compléter | Outil d’énumération identifié |
| Identifiants | À masquer | Identifiants utilisés avec succès |
| Fichier uploadé | À compléter | Fichier malveillant uploadé |

## MITRE ATT&CK provisoire

| Tactique | Technique | ID | Commentaire |
|---|---|---|---|
| Reconnaissance | Active Scanning | T1595 | Scan de ports observé |
| Discovery | File and Directory Discovery | T1083 | Énumération de répertoires |
| Credential Access | Brute Force | T1110 | Tentatives d’identification |
| Initial Access | Exploit Public-Facing Application | T1190 | Compromission d’un service web exposé |
| Persistence | Scheduled Task/Job | T1053 | Persistance planifiée à confirmer |
