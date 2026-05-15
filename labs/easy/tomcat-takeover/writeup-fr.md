# CyberDefenders — Tomcat Takeover Lab

## 1. Informations générales

| Champ | Valeur |
|---|---|
| Plateforme | CyberDefenders |
| Parcours | SOC Analyst Tier 1 |
| Niveau | Level 3 |
| Lab | Tomcat Takeover |
| Catégorie | Network Forensics |
| Difficulté | Easy |
| Durée estimée | 30 minutes |
| Outils principaux | Wireshark, NetworkMiner, tshark |
| Statut | En cours |

## 2. Liens utiles

| Source | Lien |
|---|---|
| CyberDefenders | https://cyberdefenders.org/blueteam-ctf-challenges/tomcat-takeover/ |

## 3. Contexte du scénario

L’équipe SOC a identifié une activité suspecte sur un serveur web interne. Une capture réseau est fournie pour comprendre le périmètre de l’attaque et déterminer si le serveur Apache Tomcat a été compromis.

## 4. Objectif de l’analyse

L’objectif est d’analyser le fichier PCAP afin d’identifier l’adresse IP attaquante, les ports ciblés, le panneau d’administration, les outils d’énumération, les identifiants valides, le fichier malveillant uploadé et la commande utilisée pour maintenir une persistance.

## 5. Préparation de l’environnement

```bash
mkdir -p ~/Documents/Tomcat_Takeover_Lab
cd ~/Documents/Tomcat_Takeover_Lab
unzip -P cyberdefenders.org 135-TomcatTakeover.zip -d temp_extract_dir
cd temp_extract_dir
```

## Artefact fourni

| Fichier | Type | Commentaire |
|---|---|---|
| `web server.pcap` | PCAP | Capture réseau à analyser |

## Hashes de l’artefact

| Type | Valeur |
|---|---|
| MD5 | `9ab62c5a2a1f8d0030f8240355305e7` |
| SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` |


## 6. Vérification de l’artefact

```bash
file web\ server.pcap
sha256sum web\ server.pcap
md5sum web\ server.pcap
```

Résultat observé :

```text
web server.pcap: pcap capture file, microsecond ts (little-endian) - version 2.4
```

## 7. Méthodologie

1. Identifier les hôtes présents dans le PCAP
2. Repérer l’activité de scan réseau
3. Identifier l’adresse IP source la plus suspecte
4. Rechercher le pays associé à l’IP attaquante
5. Analyser les ports ciblés
6. Identifier le port du panneau d’administration Tomcat
7. Examiner les requêtes HTTP
8. Rechercher les User-Agents et outils d’énumération
9. Identifier les accès au panneau d’administration
10. Analyser les tentatives de connexion
11. Retrouver les identifiants valides
12. Rechercher les uploads de fichiers
13. Identifier les traces de reverse shell
14. Rechercher une commande de persistance
15. Documenter les IOCs et la timeline

## 8. Commandes et filtres utiles

### Conversations IP

```bash
tshark -r web\ server.pcap -q -z conv,ip
```

### Requêtes HTTP

```bash
tshark -r web\ server.pcap -Y "http.request" -T fields -e frame.time_relative -e ip.src -e ip.dst -e tcp.dstport -e http.request.method -e http.host -e http.request.uri -e http.user_agent
```

### Requêtes POST

```bash
tshark -r web\ server.pcap -Y "http.request.method == POST" -T fields -e frame.time_relative -e ip.src -e ip.dst -e tcp.dstport -e http.request.uri -e http.user_agent
```

### Authentification HTTP

```bash
tshark -r web\ server.pcap -Y "http.authorization" -T fields -e ip.src -e ip.dst -e http.authorization
```

### Filtres Wireshark

```text
http
http.request
http.request.method == "GET"
http.request.method == "POST"
http.authorization
tcp.port == 8080
ip.addr == <IP_A_COMPLETER>
tcp.stream eq <NUMERO>
```

## 9. Réponses aux questions du lab

Les réponses seront complétées pendant l’analyse puis partiellement masquées avant publication.

### Q1 — Adresse IP source responsable du scan

**Réponse masquée :**

```text
À compléter
```

**Méthode :** analyser les conversations IP/TCP et repérer l’hôte qui initie de nombreuses connexions vers plusieurs ports.

### Q2 — Pays associé à l’adresse IP attaquante

**Réponse masquée :**

```text
À compléter
```

**Méthode :** utiliser une source GeoIP à partir de l’adresse IP identifiée en Q1.

### Q3 — Port du panneau d’administration web

**Réponse masquée :**

```text
À compléter
```

**Méthode :** repérer les ports ouverts et identifier celui qui expose l’interface d’administration Tomcat.

### Q4 — Outil utilisé pendant l’énumération

**Réponse masquée :**

```text
À compléter
```

**Méthode :** examiner les User-Agents HTTP et les patterns de requêtes.

### Q5 — Répertoire d’administration découvert

**Réponse masquée :**

```text
À compléter
```

**Méthode :** filtrer les requêtes HTTP et rechercher les chemins liés à l’administration Tomcat.

### Q6 — Identifiants utilisés avec succès

**Réponse masquée :**

```text
À compléter
```

**Méthode :** analyser les en-têtes Authorization et les réponses HTTP indiquant une connexion réussie.

### Q7 — Fichier malveillant uploadé

**Réponse masquée :**

```text
À compléter
```

**Méthode :** chercher les requêtes POST liées à l’upload ou au déploiement d’un fichier.

### Q8 — Commande de persistance planifiée

**Réponse masquée :**

```text
À compléter
```

**Méthode :** suivre les flux post-compromission pour identifier une commande récurrente ou planifiée.

## 10. IOCs à compléter

| Type | Valeur | Description |
|---|---|---|
| PCAP MD5 | `9ab62c5a2a1f8d0030f8240355305e7` | Hash de l’artefact |
| PCAP SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` | Hash de l’artefact |
| IP attaquant | À compléter | Source du scan et de l’attaque |
| IP serveur | À compléter | Serveur web ciblé |
| Port admin | À compléter | Port du panneau d’administration |
| Répertoire admin | À compléter | Chemin découvert |
| Fichier uploadé | À compléter | Fichier malveillant |
| Commande de persistance | À compléter | Commande planifiée |

## 11. Timeline provisoire

| Temps | Événement | Source |
|---|---|---|
| À compléter | Début du scan | PCAP |
| À compléter | Découverte du panneau admin | HTTP |
| À compléter | Brute-force ou tentative de connexion | HTTP |
| À compléter | Authentification réussie | HTTP |
| À compléter | Upload du fichier malveillant | HTTP |
| À compléter | Reverse shell | Flux réseau |
| À compléter | Mise en place de persistance | Commande observée |

## 12. Conclusion provisoire

Ce lab permet de travailler l’analyse réseau à partir d’un PCAP et de reconstruire une compromission web Apache Tomcat, de la reconnaissance jusqu’à la persistance.
