# Oski Lab — IOCs

## 1. Objectif

Ce fichier centralise les indicateurs de compromission observés pendant le lab **Oski** de CyberDefenders.

Les valeurs liées aux réponses directes du lab sont volontairement partiellement masquées afin d’éviter le copier-coller direct des réponses et de garder une logique pédagogique.

## 2. Sources d’analyse

| Source | Usage |
|---|---|
| CyberDefenders | Scénario du lab et questions |
| VirusTotal | Hashes, métadonnées du fichier, informations PE |
| ANY.RUN | Analyse dynamique, configuration malware, trafic réseau, MITRE ATT&CK |
| MITRE ATT&CK | Référentiel des techniques observées |

## 3. Hashes du sample

| Type | Valeur | Commentaire |
|---|---|---|
| MD5 | `12c1842c3ccafe7408c23ebf292ee3d9` | Hash fourni dans `hash.txt` |
| SHA1 | `4b1af84cc11a8b1e290a18a4222a49526eadd10` | Hash identifié via VirusTotal |
| SHA256 | `a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` | Hash principal utilisé comme pivot |

## 4. Liens d’analyse

| Source | Lien |
|---|---|
| VirusTotal | `https://www.virustotal.com/gui/file/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` |
| ANY.RUN | `https://app.any.run` |
| CyberDefenders | `https://cyberdefenders.org/blueteam-ctf-challenges/oski/` |

## 5. Informations fichier

| Champ | Valeur | Commentaire |
|---|---|---|
| Nom observé | `VPN.exe` | Nom du sample observé dans ANY.RUN |
| Type | `Win32 EXE` | Exécutable Windows |
| Format | `PE32 executable GUI Intel 80386, for MS Windows` | Format PE 32 bits |
| Taille | `311.50 KB` | Taille observée via VirusTotal |
| Sections visibles | `.text`, `.data`, `.rsrc` | Sections PE observées |
| Imports visibles | `KERNEL32.dll`, `USER32.dll`, `GDI32.dll`, `ADVAPI32.dll` | Imports PE visibles |

## 6. Réseau

Les valeurs réseau directement liées aux réponses du lab sont partiellement masquées.

| Type | Valeur | Rôle | Commentaire |
|---|---|---|---|
| URL | `http://*******.28.***/5c0*******86.php` | C2 | Serveur de commande et contrôle identifié dans ANY.RUN |
| Fichier demandé | `sqlite*.dll` | Bibliothèque | Première bibliothèque demandée après infection |
| Méthode HTTP | `POST` | Communication C2 | Requêtes observées vers le serveur C2 |
| Méthode HTTP | `GET` | Téléchargement | Récupération d’une bibliothèque DLL |

## 7. Domaines observés non retenus comme C2

Ces domaines ont été observés dans les onglets réseau, mais ils correspondent principalement à du trafic système ou de fond généré par Windows, Microsoft, Google ou les services de certificats.

| Domaine | Commentaire |
|---|---|
| `www.microsoft.com` | Trafic système Windows |
| `activation-v2.sls.microsoft.com` | Activation Windows |
| `settings-win.data.microsoft.com` | Télémétrie ou configuration Windows |
| `watson.events.data.microsoft.com` | Rapport d’erreur Windows |
| `ocsp.digicert.com` | Vérification de certificat |
| `crl.microsoft.com` | Liste de révocation de certificats |
| `www.bing.com` | Trafic web de fond |
| `login.live.com` | Service Microsoft |
| `google.com` | Trafic web de fond |

## 8. Adresses IP

| IP | Rôle | Commentaire |
|---|---|---|
| `171.*****.*****.***` | Infrastructure C2 | Partiellement masquée |
| `172.178.240.163` | Trafic associé à `WerFault.exe` | Alerte réseau liée à Microsoft Dr Watson, non retenue comme C2 principal |

## 9. Fichiers observés

| Nom | Type | Commentaire |
|---|---|---|
| `VPN.exe` | Exécutable Windows | Sample observé dans ANY.RUN |
| `******.dll` | Bibliothèque DLL | Bibliothèque demandée après infection, valeur partiellement masquée |
| `hash.txt` | Fichier texte | Artefact initial fourni par le lab |

## 10. Chemins et répertoires

| Chemin | Action observée | Commentaire |
|---|---|---|
| `C:\Users\admin\AppData\Local\Temp\VPN.exe` | Auto-suppression du binaire | Chemin temporaire du sample |
| `C:\***********` | Suppression de DLL | Répertoire ciblé pour la suppression des fichiers `*.dll`, partiellement masqué |

## 11. Commande de nettoyage observée

La commande suivante a été observée dans le processus enfant `cmd.exe`.

```cmd
cmd.exe /c timeout /t * & del /f /q "C:\Users\admin\AppData\Local\Temp\VPN.exe" & del "C:\***********\*.dll" & exit
```

Cette commande montre :

- une temporisation avant suppression
- la suppression du binaire temporaire
- la suppression de fichiers DLL dans un répertoire spécifique
- une logique de nettoyage post-exécution

## 12. Malware configuration

| Élément | Valeur | Commentaire |
|---|---|---|
| Famille | `Stealc` | Identifiée via ANY.RUN |
| Type | `Stealer` | Malware orienté vol de données |
| Tracker | `Loader, Stealc, Stealer` | Tags observés dans ANY.RUN |
| Clé RC4 | `5*********************72*******74****` | Valeur partiellement masquée |
| C2 | `http://*******.28.***/5c0*******86.php` | Valeur partiellement masquée |

## 13. MITRE ATT&CK

| Tactique | Technique | ID | Commentaire |
|---|---|---|---|
| Credential Access | Credentials from Web Browsers | `T*******5*` | Technique liée au vol d’identifiants navigateur |
| Command and Control | Application Layer Protocol | T1071 | Communication HTTP observée |
| Exfiltration | Exfiltration Over Web Service ou équivalent | T1567 | Activité cohérente avec un stealer |

## 14. Niveau de confiance

| IOC | Niveau de confiance | Justification |
|---|---|---|
| Hashes MD5, SHA1, SHA256 | Élevé | Identifiants cryptographiques du sample |
| Nom `VPN.exe` | Moyen | Nom observé dans la sandbox, peut varier selon les soumissions |
| C2 partiellement masqué | Élevé | Visible dans la configuration malware et les requêtes HTTP |
| Bibliothèque DLL partiellement masquée | Élevé | Observée dans les requêtes HTTP |
| Répertoire de suppression DLL | Élevé | Visible dans la ligne de commande du processus enfant |
| Domaines Microsoft et Google | Faible pour l’attribution malware | Trafic système ou de fond |

## 15. Notes d’utilisation

Ces IOCs doivent être utilisés dans un contexte pédagogique et défensif.

Pour une investigation réelle, il faudrait :

- vérifier si les IOCs sont encore actifs
- enrichir les IPs et domaines avec d’autres sources
- corréler avec les logs proxy, DNS, EDR et SIEM
- vérifier la présence des hashes sur les endpoints
- rechercher les commandes de suppression dans les logs process
- créer des règles de détection adaptées si nécessaire

## 16. Avertissement

Ce fichier est produit dans le cadre d’un lab CyberDefenders.

Les valeurs sensibles liées aux réponses directes du lab sont partiellement masquées afin de préserver l’intérêt pédagogique de l’exercice.
