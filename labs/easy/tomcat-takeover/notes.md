# Tomcat Takeover — Notes d’investigation

## Objectif

Ces notes guident l’analyse du lab Tomcat Takeover sans fournir directement les réponses.

## Artefact

| Fichier | Type |
|---|---|
| `web server.pcap` | PCAP |

## Hashes

| Type | Valeur |
|---|---|
| MD5 | `9ab62c5a2a1f8d0030f8240355305e7` |
| SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` |

## Axes d’analyse

- conversations IP
- scans de ports
- requêtes HTTP
- User-Agents
- authentification HTTP
- répertoires Tomcat
- upload de fichier
- reverse shell
- persistance

## Filtres Wireshark utiles

```text
http
http.request
http.request.method == "GET"
http.request.method == "POST"
http.authorization
tcp
tcp.stream eq <numéro>
ip.addr == <IP>
tcp.port == <port>
```

## Méthodologie conseillée

1. Identifier les IPs principales
2. Repérer le serveur web
3. Repérer l’IP qui scanne plusieurs ports
4. Examiner les requêtes HTTP
5. Identifier les chemins sensibles
6. Rechercher les tentatives d’authentification
7. Suivre les flux TCP suspects
8. Rechercher les uploads
9. Rechercher les commandes exécutées après compromission
10. Construire une timeline
