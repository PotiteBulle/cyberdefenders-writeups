# CyberDefenders — Oski Lab

## 1. Informations générales

| Champ | Valeur |
|---|---|
| Plateforme | CyberDefenders |
| Parcours | SOC Analyst Tier 1 |
| Niveau | Level 1 |
| Lab | Oski |
| Catégorie | Threat Intel |
| Difficulté | Easy |
| Durée estimée | 30 minutes |
| Outils principaux | ANY.RUN, VirusTotal, Hybrid Analysis |
| Statut | Terminé |

## 2. Liens utiles

| Source | Lien |
|---|---|
| CyberDefenders | https://cyberdefenders.org/blueteam-ctf-challenges/oski/ |
| VirusTotal | `https://www.virustotal.com/gui/file/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` |
| ANY.RUN | `https://app.any.run` |

## 3. Contexte du scénario

Un employé reçoit un email intitulé **Urgent New Order** provenant d’un client.

Après avoir tenté d’ouvrir la facture jointe, il constate que le document contient de fausses informations de commande.

Par la suite, une alerte SIEM est générée concernant le téléchargement d’un fichier potentiellement malveillant.

L’investigation initiale indique que le fichier PPT pourrait être responsable du téléchargement.

## 4. Objectif de l’analyse

L’objectif du lab est d’analyser un rapport sandbox et des données de threat intelligence afin d’identifier le comportement du malware, ses communications réseau, sa configuration, ses techniques MITRE ATT&CK et ses mécanismes d’évasion.

Ce lab est orienté **Threat Intel**, **malware triage**, **analyse sandbox** et **SOC investigation**.

## 5. Préparation de l’environnement

L’analyse a été commencée depuis une machine Kali Linux dans le dossier suivant :

```bash
~/Documents/Oski_Lab/
```

Le fichier téléchargé depuis CyberDefenders est une archive ZIP protégée par mot de passe :

```text
140-Oski.zip
```

Le mot de passe fourni par la plateforme est :

```text
cyberdefenders.org
```

Extraction de l’archive :

```bash
unzip -P cyberdefenders.org 140-Oski.zip -d temp_extract_dir
```

Après extraction, l’archive contient un seul fichier :

```text
hash.txt
```

Vérification du type de fichier :

```bash
file hash.txt
```

Résultat :

```text
hash.txt: ASCII text
```

## 6. Contenu du fichier hash.txt

Commande utilisée :

```bash
cat hash.txt
```

Contenu observé :

```text
MD5 Hash: 12c1842c3ccafe7408c23ebf292ee3d9

Use this hash on online threat intel platforms (e.g., VirusTotal, Hybrid Analysis) to complete the lab analysis.
```

Le fichier `hash.txt` ne contient pas directement le malware. Il fournit un hash MD5 servant de pivot pour la recherche threat intelligence.

## 7. Correction du hash

Une première recopie du hash a été faite de manière incorrecte.

Hash incorrect testé au départ :

```text
12c1842c3ccfe7408c23ebf292ee3d9
```

Après vérification avec `cat -A`, le hash correct est :

```text
12c1842c3ccafe7408c23ebf292ee3d9
```

Commande utilisée :

```bash
cat -A hash.txt
```

La différence se situe dans la séquence `3ccafe`.

## 8. Hashes identifiés

La recherche du MD5 correct sur VirusTotal a permis d’identifier les hashes associés au sample.

| Type | Valeur |
|---|---|
| MD5 | `12c1842c3ccafe7408c23ebf292ee3d9` |
| SHA1 | `4b1af84cc11a8b1e290a18a4222a49526eadd10` |
| SHA256 | `a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` |

Le SHA256 devient le pivot principal pour la suite de l’analyse, car il identifie le fichier de manière plus robuste que le MD5.

## 9. Métadonnées VirusTotal

Les propriétés de base observées dans VirusTotal indiquent que le fichier est un exécutable Windows 32 bits.

| Champ | Valeur |
|---|---|
| Type | Win32 EXE |
| Magic | PE32 executable GUI Intel 80386, for MS Windows |
| Taille | 311.50 KB |
| Creation Time | 2022-************** 17:******* UTC |
| First Seen In The Wild | 2023-09-23 22:33:33 UTC |
| First Submission | 2023-09-23 22:02:55 UTC |
| Last Submission | 2025-09-29 02:35:54 UTC |
| Last Analysis | 2026-05-13 19:04:16 UTC |

## 10. Informations PE observées

Les informations Portable Executable indiquent notamment :

| Champ | Valeur |
|---|---|
| Architecture cible | Intel 386 ou processeurs compatibles |
| Compilation Timestamp | 2022-************** 17:******* UTC |
| Sections contenues | 3 |
| Sections visibles | `.text`, `.data`, `.rsrc` |
| Imports visibles | `KERNEL32.dll`, `USER32.dll`, `GDI32.dll`, `ADVAPI32.dll` |

Ces éléments confirment que le sample est un exécutable Windows et non un simple document.

## 11. Analyse ANY.RUN

Le rapport ANY.RUN associé au sample permet d’observer le comportement du malware dans un environnement sandbox.

Lien :

```text
https://app.any.run/
```

Le sample est identifié sous le nom :

```text
VPN.exe
```

Le rapport ANY.RUN associe le sample aux tags suivants :

```text
stealc, stealer, loader, oski
```

Le tracker indique une association avec :

```text
Loader, Stealc, Stealer
```

## 12. Analyse réseau

L’onglet `HTTP Requests` montre plusieurs requêtes HTTP effectuées par le processus `VPN.exe`.

Le serveur C2 est visible dans la configuration du malware et dans les requêtes HTTP.

Réponse volontairement partiellement masquée :

```text
http://*******.28.***/5c06***86.php
```

Plusieurs requêtes sont effectuées vers ce serveur, notamment des requêtes `POST` vers un fichier PHP.

Une requête `GET` vers une bibliothèque DLL est également observée après infection.

Réponse volontairement partiellement masquée :

```text
*******.dll
```

## 13. Configuration du malware

L’onglet `Malware configuration` d’ANY.RUN identifie le malware comme **Stealc**.

La configuration contient notamment :

- le serveur C2
- une clé RC4
- des chaînes liées aux répertoires utilisateurs
- des extensions ciblées
- des chemins utilisés pendant l’exécution

La clé RC4 utilisée pour déchiffrer une chaîne encodée en base64 est visible dans la configuration.

Réponse volontairement partiellement masquée :

```text
5***462144***074****
```

## 14. Analyse MITRE ATT&CK

L’onglet MITRE ATT&CK du rapport ANY.RUN indique plusieurs tactiques et techniques observées.

Pour le vol de mots de passe, la technique principale affichée est liée aux identifiants stockés dans les navigateurs web.

Réponse volontairement partiellement masquée :

```text
T***5*
```

La technique observée correspond à :

```text
Credentials from Web Browsers
```

## 15. Analyse des processus enfants

Le processus principal observé est :

```text
VPN.exe
```

Un processus enfant `cmd.exe` est lancé avec une commande de suppression et d’auto-suppression.

Commande observée dans le rapport :

```cmd
cmd.exe /c timeout /t 5 & del /f /q "C:\Users\admin\AppData\Local\Temp\VPN.exe" & del "C:\ProgramData\*.dll" & exit
```

Cette ligne de commande montre deux comportements importants :

- suppression du binaire temporaire `VPN.exe`
- suppression de DLL dans un répertoire spécifique

Répertoire ciblé pour la suppression de DLL, volontairement partiellement masqué :

```text
C:\*******
```

Temps avant auto-suppression, volontairement masqué :

```text
*
```

## 16. Réponses aux questions du lab

Les réponses sont volontairement partiellement masquées afin d’éviter le simple copier-coller et de favoriser la compréhension de la méthode.

### Q1 — Heure de création du malware

**Question :**  
Determining the creation time of the malware can provide insights into its origin. What was the time of malware creation?

**Réponse masquée :**

```text
2022-*** 17:**
```

**Méthode :**  
La réponse a été trouvée dans les propriétés de base VirusTotal du fichier. Le champ `Creation Time` indique `2022-*** 17:****** UTC`. Le format attendu par CyberDefenders est `YYYY-MM-DD HH:MM`.

---

### Q2 — Serveur C2

**Question :**  
Identifying the command and control server that the malware communicates with can help trace back to the attacker. Which C2 server does the malware in the PPT file communicate with?

**Réponse masquée :**

```text
http://******.28.***/5c0******86.php
```

**Méthode :**  
La réponse est visible dans `Malware configuration`, champ `C2`, ainsi que dans les requêtes HTTP du processus `VPN.exe`.

---

### Q3 — Première bibliothèque demandée après infection

**Question :**  
Identifying the initial actions of the malware post-infection can provide insights into its primary objectives. What is the first library that the malware requests post-infection?

**Réponse masquée :**

```text
*******.dll
```

**Méthode :**  
La réponse est trouvée dans les requêtes HTTP chronologiques. La première bibliothèque téléchargée après infection est un fichier DLL demandé via une requête `GET`.

---

### Q4 — Clé RC4

**Question :**  
By examining the provided ANY.RUN report, what RC4 key is used by the malware to decrypt its base64-encoded string?

**Réponse masquée :**

```text
5*********************72*******74****
```

**Méthode :**  
La clé est visible dans `Malware configuration`, section `Keys`, champ `RC4`.

---

### Q5 — Technique MITRE ATT&CK liée au vol de mots de passe

**Question :**  
By examining the MITRE ATT&CK techniques displayed in the ANY.RUN sandbox report, identify the main MITRE technique the malware uses to steal the user's password.

**Réponse masquée :**

```text
T*******5*
```

**Méthode :**  
La réponse est trouvée dans l’onglet MITRE ATT&CK du rapport ANY.RUN. La technique affichée concerne les identifiants stockés dans les navigateurs web.

---

### Q6 — Répertoire ciblé pour la suppression des DLL

**Question :**  
By examining the child processes displayed in the ANY.RUN sandbox report, which directory does the malware target for the deletion of all DLL files?

**Réponse masquée :**

```text
C:\***********
```

**Méthode :**  
La réponse est visible dans la ligne de commande du processus enfant `cmd.exe`. La commande contient une suppression de fichiers `*.dll` dans un répertoire spécifique.

---

### Q7 — Temps avant auto-suppression

**Question :**  
Understanding the malware's behavior post-data exfiltration can give insights into evasion techniques. By analyzing the child process, after successfully exfiltrating the user's data, how many seconds does it take for the malware to self-delete?

**Réponse masquée :**

```text
*
```

**Méthode :**  
La réponse est visible dans la ligne de commande du processus enfant `cmd.exe`, avec l’instruction `timeout /t`.

## 17. Indicateurs de compromission

### Hashes

| Type | Valeur | Description |
|---|---|---|
| MD5 | `12c1842c3ccafe7408c23ebf292ee3d9` | Hash du sample fourni par le lab |
| SHA1 | `4b1af84cc11a8b1e290a18a4222a49526eadd10` | Hash identifié via VirusTotal |
| SHA256 | `a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` | Hash identifié via VirusTotal |

### Réseau

| Type | Valeur | Description |
|---|---|---|
| C2 | `http://*******.28.***/5c0*******86.php` | Serveur de commande et contrôle, partiellement masqué |
| Fichier demandé | `sqlite*.dll` | Première bibliothèque demandée, partiellement masquée |

### Fichiers et chemins

| Type | Valeur | Description |
|---|---|---|
| Nom observé | `VPN.exe` | Nom du sample dans ANY.RUN |
| Répertoire ciblé | `C:\***********` | Répertoire ciblé pour la suppression de DLL, partiellement masqué |
| Fichier temporaire | `C:\Users\admin\AppData\Local\Temp\VPN.exe` | Chemin du binaire temporaire observé |

### MITRE ATT&CK

| Tactique | Technique | ID |
|---|---|---|
| Credential Access | Credentials from Web Browsers | `T*******5*` |

## 18. Timeline

| Temps | Événement | Source |
|---|---|---|
| 2022-****** 17:******* UTC | Création ou compilation du malware | VirusTotal |
| Non précisé | Réception de l’email **Urgent New Order** | Scénario CyberDefenders |
| Non précisé | Ouverture du fichier PPT suspect | Scénario CyberDefenders |
| Pendant l’exécution sandbox | Communication avec le C2 | ANY.RUN |
| Pendant l’exécution sandbox | Téléchargement de bibliothèques DLL | ANY.RUN |
| Après exfiltration | Suppression différée du binaire | Processus enfant `cmd.exe` |

## 19. Conclusion

Le lab Oski met en évidence un scénario d’infection reposant sur un fichier associé à un malware de type **Stealc stealer**.

L’analyse commence avec un hash MD5 fourni dans `hash.txt`, puis se poursuit avec VirusTotal afin d’identifier les hashes complets, les métadonnées PE et l’heure de création du malware.

Le rapport ANY.RUN permet ensuite d’observer le comportement dynamique du sample, notamment :

- les communications avec le serveur C2
- la configuration du malware
- la clé RC4 utilisée
- les bibliothèques téléchargées
- les techniques MITRE ATT&CK associées au vol d’identifiants
- les commandes de suppression et d’auto-suppression

Ce lab est un bon exercice d’introduction à l’analyse sandbox, à la threat intelligence et à la documentation d’une investigation SOC.

## 20. Leçons apprises

- Toujours inventorier les fichiers fournis avant l’analyse
- Vérifier un hash en cas d’échec de recherche sur les plateformes de threat intelligence
- Utiliser VirusTotal pour retrouver les hashes complets d’un sample
- Utiliser ANY.RUN pour analyser le comportement dynamique d’un malware
- Distinguer le trafic système légitime du trafic réellement lié au malware
- Lire la configuration extraite d’un stealer
- Associer les comportements observés à MITRE ATT&CK
- Documenter les réponses avec une méthode plutôt qu’avec une simple valeur brute
