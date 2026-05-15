# Oski Lab — Notes professionnelles d’investigation

## 1. Objectif de ces notes

Ces notes servent à documenter le raisonnement suivi pendant le lab Oski de CyberDefenders.

Elles ne sont pas conçues pour donner directement les réponses.  
Leur objectif est d’expliquer comment aborder l’investigation, comment exploiter les preuves disponibles et comment comprendre le comportement du malware dans une logique SOC et Blue Team.

Le but principal est de construire une méthodologie claire, réutilisable dans d’autres labs liés au malware triage, à l’analyse sandbox et à la threat intelligence.

## 2. Contexte du lab

Le lab Oski fait partie du parcours CyberDefenders **SOC Analyst Tier 1**.

Le scénario commence par la réception d’un email intitulé **Urgent New Order** par un employé. L’email contient une pièce jointe ressemblant à une facture. Après interaction avec la pièce jointe, une alerte SIEM est générée concernant le téléchargement d’un fichier potentiellement malveillant.

L’hypothèse initiale est que le fichier PPT suspect a déclenché le téléchargement ou l’exécution d’un malware.

D’un point de vue SOC, ce scénario représente une chaîne d’infection classique :

```text
Email de phishing
→ Pièce jointe suspecte
→ Téléchargement ou exécution d’un payload
→ Communication malware
→ Vol d’identifiants ou collecte de données
→ Exfiltration
→ Nettoyage ou auto-suppression
```

## 3. Périmètre de l’investigation

L’investigation se concentre sur les éléments suivants :

- identifier l’artefact fourni
- utiliser les hashes comme pivots de threat intelligence
- récupérer les métadonnées depuis VirusTotal
- analyser le rapport sandbox ANY.RUN
- comprendre le comportement de la famille malware
- identifier l’activité réseau suspecte
- lire la configuration extraite du malware
- mapper les comportements observés avec MITRE ATT&CK
- comprendre l’activité des processus enfants
- documenter les IOCs utiles
- éviter toute exécution locale non contrôlée

## 4. Gestion initiale de l’artefact

L’archive du lab contient un fichier nommé :

```text
hash.txt
```

Ce fichier ne contient pas le malware. Il contient un hash qui doit être utilisé pour rechercher des informations sur des plateformes de threat intelligence.

Ce point est important, car la méthode d’analyse n’est pas celle d’un cas où le sample est directement fourni. Ici, l’analyste doit utiliser une approche par pivot.

Commandes initiales recommandées :

```bash
ls -lah
file hash.txt
cat hash.txt
cat -A hash.txt
grep -Eo '[a-fA-F0-9]{32}' hash.txt
```

### Pourquoi utiliser `cat -A`

La commande `cat -A` permet d’afficher les caractères invisibles, les fins de ligne et les espaces.

Elle est utile lorsque :

- un hash ne donne aucun résultat sur les plateformes de threat intelligence
- une valeur a peut-être été mal copiée
- des caractères invisibles peuvent être présents
- le fichier utilise des retours à la ligne de type Windows

Dans ce lab, vérifier précisément le hash est une étape importante.

## 5. Pivot par hash

Un hash sert de pivot pour identifier un fichier sur différentes plateformes de threat intelligence.

Types de hashes courants :

| Type | Longueur | Usage |
|---|---:|---|
| MD5 | 32 caractères hexadécimaux | Pivot rapide, encore très utilisé |
| SHA1 | 40 caractères hexadécimaux | Plus robuste que MD5, encore souvent affiché |
| SHA256 | 64 caractères hexadécimaux | Identifiant préféré pour l’analyse malware |

Méthodologie recommandée :

1. Extraire le MD5 depuis `hash.txt`
2. Rechercher le MD5 sur VirusTotal
3. Récupérer les valeurs SHA1 et SHA256 associées
4. Utiliser le SHA256 comme pivot principal
5. Rechercher le SHA256 sur ANY.RUN et Hybrid Analysis
6. Comparer les résultats entre les plateformes

Une fois disponible, le SHA256 doit être privilégié, car il identifie le fichier de manière plus fiable.

## 6. Notes d’analyse VirusTotal

VirusTotal est utile pour collecter des métadonnées statiques et des informations de réputation.

Dans ce lab, VirusTotal peut aider à identifier :

- le type de fichier
- la taille du fichier
- les hashes MD5, SHA1 et SHA256
- les métadonnées PE
- l’heure de création ou de compilation
- la première soumission
- la première apparition observée
- les noms de détection
- les labels de famille malware
- les relations avec d’autres fichiers ou URLs

Sections importantes à consulter :

```text
Details
Relations
Behavior
Community
Detection
```

### Métadonnées statiques à relever

| Champ | Pourquoi c’est utile |
|---|---|
| File type | Confirme si le fichier est un exécutable, document, archive ou script |
| Magic | Donne une description bas niveau du format |
| File size | Permet de comparer le sample avec d’autres soumissions |
| Creation Time | Peut aider pour les questions de timeline |
| Compilation Timestamp | Utile en triage de malware PE |
| Names | Montre les noms utilisés lors des soumissions |
| Imports | Donne des indices sur les API Windows utilisées |
| Sections | Peut indiquer un packing, des ressources ou une structure inhabituelle |

### Attention aux timestamps

Les timestamps doivent être interprétés avec prudence.

Un timestamp malware peut être :

- réel
- modifié par l’attaquant
- hérité d’un builder
- généré automatiquement
- volontairement falsifié

Le timestamp reste utile pour répondre au lab, mais il ne doit pas être interprété comme une preuve absolue de l’origine réelle du malware.

## 7. Notes d’analyse ANY.RUN

ANY.RUN fournit une analyse dynamique en sandbox. Il permet d’observer le comportement du sample lorsqu’il est exécuté dans un environnement Windows contrôlé.

Zones importantes à consulter :

```text
Processes
HTTP Requests
Connections
DNS Requests
Threats
Malware configuration
MITRE ATT&CK
Text report
IOC
Graph
```

L’objectif n’est pas seulement de collecter des valeurs, mais de comprendre le comportement du malware pendant son exécution.

## 8. Analyse de l’arbre des processus

L’arbre des processus permet d’identifier le processus principal et les processus enfants.

Questions utiles :

- Quel est le processus initial ?
- Quels processus enfants sont créés ?
- Des interpréteurs de commande sont-ils utilisés ?
- `cmd.exe` ou PowerShell sont-ils impliqués ?
- Des fichiers sont-ils créés, supprimés ou déplacés ?
- Y a-t-il une auto-suppression ?
- Des commandes de délai ou de nettoyage sont-elles utilisées ?

Dans ce lab, l’arbre des processus est particulièrement important pour comprendre le comportement après exfiltration et les actions de nettoyage.

### Analyse des lignes de commande

À rechercher dans les lignes de commande :

```cmd
timeout /t
ping 127.0.0.1
choice /t
del /f /q
rmdir
copy
move
attrib
```

Ces commandes peuvent indiquer :

- un délai d’exécution
- une suppression de fichiers
- une auto-suppression
- de l’évasion
- un nettoyage d’artefacts
- un staging temporaire de payload

## 9. Notes d’analyse réseau

L’analyse réseau sert à identifier les communications avec une infrastructure externe.

Onglets utiles dans ANY.RUN :

```text
HTTP Requests
Connections
DNS Requests
Threats
```

### Éléments à rechercher

Il faut se concentrer sur :

- les domaines non Microsoft
- les domaines non Google
- les URLs basées sur une IP brute
- les pays ou hébergeurs inhabituels
- les requêtes HTTP POST
- les téléchargements de DLL ou d’exécutables
- les communications répétées vers le même endpoint
- les chemins de type C2 se terminant par `.php`
- les réponses contenant du contenu binaire
- le trafic associé au PID du malware

### Filtrer le bruit

Les sandbox génèrent souvent du trafic de fond légitime.

Exemples de trafic normal :

```text
microsoft.com
windows.com
digicert.com
ocsp.*
crl.*
bing.com
login.live.com
settings-win.data.microsoft.com
watson.events.data.microsoft.com
```

Ce trafic peut être documenté, mais ne doit pas être considéré automatiquement comme malveillant.

Les entrées les plus importantes sont celles associées au processus malware et contenant des URLs, IPs, chemins ou contenus suspects.

## 10. Analyse des requêtes HTTP

Les requêtes HTTP sont centrales dans ce lab.

Champs utiles :

| Champ | Utilité |
|---|---|
| Timestamp | Reconstruire l’ordre des événements |
| Method | GET peut indiquer un téléchargement, POST peut indiquer une communication ou exfiltration |
| PID | Relier la requête à un processus |
| Process name | Identifier si la requête vient du malware ou du système |
| URL | Identifier C2, payloads ou bibliothèques |
| Content size | Distinguer beacon, texte ou binaire |
| Content type | Identifier texte, binaire ou exécutable |

### Requêtes POST

Des requêtes `POST` répétées vers un endpoint suspect peuvent indiquer :

- beaconing
- check-in
- échange de commandes
- exfiltration
- récupération de configuration

### Requêtes GET

Des requêtes `GET` vers des fichiers DLL peuvent indiquer :

- récupération de payload
- téléchargement de dépendances
- comportement modulaire
- exécution de composants supplémentaires

## 11. Analyse de la configuration malware

Le panneau `Malware configuration` est l’une des parties les plus importantes du rapport ANY.RUN.

Il peut contenir :

- la famille malware
- le serveur C2
- des clés de chiffrement
- une clé RC4
- des chemins ciblés
- des extensions ciblées
- des chaînes utilisées par le malware
- des répertoires utilisés pendant l’exécution

Dans ce lab, la configuration est importante pour comprendre la communication du malware et le traitement de données encodées.

### Pourquoi RC4 est important

RC4 est un chiffrement par flot parfois utilisé par les malwares pour masquer des chaînes, de la configuration ou du trafic.

Une clé RC4 visible dans un rapport sandbox indique souvent qu’ANY.RUN a extrait automatiquement une configuration du malware.

Méthode :

1. Ouvrir le panneau `Malware configuration`
2. Repérer la section `Keys`
3. Vérifier si une clé RC4 est affichée
4. Documenter la méthode sans publier directement toutes les valeurs sensibles
5. Expliquer comment l’information a été localisée

## 12. Mapping MITRE ATT&CK

MITRE ATT&CK fournit une manière standardisée de décrire les comportements adverses.

Dans ce lab, la tactique importante est **Credential Access**.

À rechercher :

- vol d’identifiants navigateur
- accès aux password stores
- collecte de données de profil utilisateur
- extraction d’identifiants

Questions utiles :

- Quelle tactique est affichée dans ANY.RUN ?
- Quelle technique est marquée comme pertinente ?
- Est-ce une technique principale ou une sous-technique ?
- Quel comportement justifie ce mapping ?
- Ce comportement est-il cohérent avec la famille malware ?

Le writeup doit expliquer le comportement derrière l’ID ATT&CK, pas seulement lister un identifiant.

## 13. Contexte Stealc

ANY.RUN associe le comportement observé à **Stealc**, une famille de stealer.

De manière générale, les stealers cherchent à collecter :

- mots de passe navigateur
- cookies
- données autofill
- wallets crypto
- fichiers dans les répertoires utilisateur
- informations système
- identifiants applicatifs

Dans ce lab, le comportement observé suit une logique de stealer :

```text
Exécution
→ Communication C2
→ Récupération ou validation de configuration
→ Collecte de données
→ Accès aux identifiants
→ Exfiltration
→ Nettoyage
```

## 14. Logique d’auto-suppression

L’auto-suppression est une technique fréquente de nettoyage malware.

La sandbox montre un processus enfant qui effectue une suppression différée.

La ligne de commande doit être découpée par blocs :

```cmd
cmd.exe /c
timeout /t <delay>
del /f /q "<temporary malware path>"
del "<target directory>\*.dll"
exit
```

Rôle des composants :

| Élément | Rôle |
|---|---|
| `cmd.exe /c` | Exécute une commande puis ferme le shell |
| `timeout /t` | Attend un nombre de secondes |
| `del /f /q` | Supprime un fichier de manière forcée et silencieuse |
| Chemin temporaire du malware | Emplacement du sample en cours d’exécution |
| `*.dll` | Cible toutes les DLL d’un répertoire |
| `exit` | Ferme l’interpréteur de commande |

Ce comportement suggère un nettoyage après exécution ou exfiltration.

## 15. Documentation des IOCs

Les IOCs doivent être documentés proprement et de manière cohérente.

Catégories recommandées :

```text
Hashes
Réseau
Fichiers
Répertoires
MITRE ATT&CK
Liens sandbox
```

Structure recommandée :

| Type | Valeur | Contexte |
|---|---|---|
| MD5 | valeur | Hash fourni par le lab |
| SHA1 | valeur | Identifié via VirusTotal |
| SHA256 | valeur | Pivot principal |
| URL | partiellement masquée | Endpoint C2 |
| Fichier | partiellement masqué | Bibliothèque demandée |
| Répertoire | partiellement masqué | Cible de nettoyage DLL |
| MITRE ID | partiellement masqué | Technique de Credential Access |

Pour un writeup public GitHub, certaines réponses directes peuvent être partiellement masquées afin d’éviter le copier-coller.

## 16. Stratégie de masquage des réponses

Le writeup masque volontairement les réponses directes.

Cette approche préserve la valeur pédagogique du lab tout en documentant la méthode.

Exemples :

| Type de donnée | Exemple de masquage |
|---|---|
| Timestamp | `2022-*** 17:**` |
| URL | `http://******.28.***/5c0******86.php` |
| DLL | `*******.dll` |
| Clé RC4 | `5*********************72*******74****` |
| ID MITRE | `T*******5*` |
| Répertoire | `C:\***********` |
| Secondes | `*` |

Le but est de fournir assez de contexte pour comprendre la démarche sans publier une fiche de réponses directe.

## 17. Checklist d’investigation

### Gestion de l’artefact

- [ ] Télécharger l’archive du lab
- [ ] Extraire l’archive prudemment
- [ ] Lister les fichiers extraits
- [ ] Identifier les types de fichiers
- [ ] Lire `hash.txt`
- [ ] Vérifier le hash avec `cat -A`
- [ ] Extraire proprement le MD5 avec une regex

### Threat intelligence

- [ ] Rechercher le MD5 sur VirusTotal
- [ ] Récupérer SHA1 et SHA256
- [ ] Utiliser SHA256 comme pivot principal
- [ ] Lire les métadonnées du fichier
- [ ] Examiner les informations PE
- [ ] Examiner les noms et relations
- [ ] Rechercher le hash sur ANY.RUN
- [ ] Rechercher le hash sur Hybrid Analysis si nécessaire

### Analyse sandbox

- [ ] Ouvrir le rapport ANY.RUN
- [ ] Identifier le processus principal
- [ ] Examiner les requêtes HTTP
- [ ] Examiner les requêtes DNS
- [ ] Examiner les connexions
- [ ] Distinguer le trafic système du trafic suspect
- [ ] Ouvrir la configuration malware
- [ ] Examiner les clés extraites
- [ ] Lire le mapping MITRE ATT&CK
- [ ] Inspecter les processus enfants
- [ ] Identifier les commandes de suppression ou nettoyage

### Documentation

- [ ] Documenter la méthodologie
- [ ] Documenter les métadonnées importantes
- [ ] Ajouter une table IOC
- [ ] Ajouter une timeline
- [ ] Ajouter un mapping MITRE ATT&CK
- [ ] Masquer les réponses directes dans le writeup public
- [ ] Rédiger les leçons apprises

## 18. Structure recommandée du writeup

Structure propre pour ce lab :

```text
1. Informations générales
2. Liens utiles
3. Contexte du scénario
4. Objectif de l’analyse
5. Préparation de l’environnement
6. Contenu de hash.txt
7. Correction du hash
8. Hashes identifiés
9. Métadonnées VirusTotal
10. Informations PE
11. Analyse ANY.RUN
12. Analyse réseau
13. Configuration du malware
14. Analyse MITRE ATT&CK
15. Analyse des processus enfants
16. Questions avec réponses masquées
17. IOCs
18. Timeline
19. Conclusion
20. Leçons apprises
```

## 19. Compétences travaillées

Ce lab permet de pratiquer :

- gestion sécurisée d’artefacts suspects
- investigation par hash
- pivot threat intelligence
- lecture de métadonnées statiques
- analyse de rapport sandbox
- interprétation du trafic réseau
- extraction de configuration malware
- mapping MITRE ATT&CK
- documentation d’IOCs
- rédaction publique responsable

## 20. Notes personnelles d’analyste

La leçon principale de ce lab est que le premier artefact fourni n’est pas toujours le malware.

Ici, l’investigation démarre avec un fichier texte contenant un hash. Cela impose une approche par pivot plutôt qu’une analyse directe du sample.

Autre point important : un rapport sandbox contient à la fois de l’activité malveillante et de l’activité système normale. Le trafic Windows, Microsoft ou certificat ne doit pas être confondu avec la communication C2.

L’analyste doit corréler :

```text
nom du processus
PID
URL
timestamp
type de contenu
configuration malware
mapping MITRE
ligne de commande du processus enfant
```

C’est cette corrélation qui permet de comprendre correctement le comportement observé.
