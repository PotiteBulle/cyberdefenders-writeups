# Oski Lab

## Plateforme

CyberDefenders

Lien officiel : https://cyberdefenders.org/blueteam-ctf-challenges/oski/

## Informations

| Champ | Valeur |
|---|---|
| Lab | Oski |
| Parcours | SOC Analyst Tier 1 |
| Niveau | Level 1 |
| Difficulté | Easy |
| Catégorie | Threat Intel |
| Statut | Terminé |

## Sources

| Source | Lien |
|---|---|
| CyberDefenders | `https://cyberdefenders.org/blueteam-ctf-challenges/oski/` |
| VirusTotal | `https://www.virustotal.com/gui/file/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb` |
| ANY.RUN | `https://app.any.run/` |

## Résumé

Ce lab consiste à analyser un hash fourni par CyberDefenders afin de retrouver un sample malveillant, consulter ses métadonnées VirusTotal, puis analyser son comportement dans ANY.RUN.

Le sample est associé à un malware de type **Stealc stealer**.

L’analyse permet de travailler plusieurs compétences importantes pour un profil SOC et Blue Team :

- investigation par hash
- pivot threat intelligence
- lecture de métadonnées VirusTotal
- analyse sandbox ANY.RUN
- identification de communications C2
- lecture d’une configuration malware
- analyse de processus enfants
- mapping MITRE ATT&CK
- documentation d’IOCs
- rédaction de writeup avec réponses partiellement masquées

## Objectifs pédagogiques

Les objectifs principaux du lab sont :

- comprendre comment utiliser un hash comme point de départ d’investigation
- retrouver un sample à partir d’un hash MD5
- enrichir l’analyse avec SHA1 et SHA256
- identifier le type de fichier et ses métadonnées PE
- analyser les requêtes HTTP et les communications réseau
- comprendre le comportement d’un stealer
- identifier les éléments de configuration extraits par ANY.RUN
- documenter les observations dans un format propre et réutilisable

## Structure du dossier

| Fichier | Description |
|---|---|
| `README.md` | Présentation du lab en français |
| `README_EN.md` | Présentation du lab en anglais |
| `writeup-fr.md` | Writeup complet en français avec réponses partiellement masquées |
| `writeup-en.md` | Writeup complet en anglais avec réponses partiellement masquées |
| `notes-fr.md` | Notes professionnelles d’investigation en français |
| `notes-en.md` | Notes professionnelles d’investigation en anglais |
| `iocs-fr.md` | Indicateurs de compromission en français |
| `iocs-en.md` | Indicateurs de compromission en anglais |

## Choix de documentation

Aucune capture d’écran n’est incluse dans ce dossier.

Le writeup repose volontairement sur une documentation textuelle : commandes utilisées, observations, méthodologie, IOCs, mapping MITRE ATT&CK et références vers les plateformes d’analyse.

Ce choix permet de garder le dépôt plus léger, plus lisible et plus simple à maintenir.

## État du lab

| Élément | Statut |
|---|---|
| Récupération du hash | Terminé |
| Pivot VirusTotal | Terminé |
| Analyse ANY.RUN | Terminé |
| Mapping MITRE ATT&CK | Terminé |
| Writeup FR | Terminé |
| Writeup EN | Terminé |
| Notes FR | Terminé |
| Notes EN | Terminé |
| IOCs FR | Terminé |
| IOCs EN | Terminé |
| Captures d’écran | Non utilisées |

## Avertissement

Les réponses sont volontairement partiellement masquées afin d’éviter le copier-coller direct et de favoriser la compréhension de la méthode d’analyse.

Ce lab est documenté uniquement dans un cadre éducatif, défensif et contrôlé.

Aucune activité offensive réelle ou non autorisée n’est réalisée.
