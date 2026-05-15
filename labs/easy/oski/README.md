# Oski Lab

## Plateforme

CyberDefenders

Lien officiel : https://cyberdefenders.org/blueteam-ctf-challenges/oski/

## Informations du lab

| Champ | Valeur |
|---|---|
| Nom du lab | Oski |
| Parcours | SOC Analyst Tier 1 |
| Niveau | Level 1 |
| Difficulté | Easy |
| Durée estimée | 30 minutes |
| Catégorie | Threat Intel |
| Outils indiqués | VirusTotal, ANY.RUN |

## Objectif du lab

L’objectif de ce lab est d’analyser un rapport sandbox ANY.RUN afin d’identifier le comportement d’un malware de type stealer.

Le scénario demande d’extraire des informations importantes comme la date de création du malware, le serveur C2, les bibliothèques contactées après infection, la clé RC4 utilisée, les techniques MITRE ATT&CK observées, le dossier ciblé pour la suppression de DLL et le délai avant auto-suppression.

## Scénario

Un employé reçoit un email intitulé **Urgent New Order** provenant d’un client.

Lorsqu’il tente d’accéder à la facture jointe, il découvre que le document contient de fausses informations de commande.

Par la suite, une alerte SIEM est générée concernant le téléchargement d’un fichier potentiellement malveillant.

Une première investigation indique que le fichier PPT pourrait être responsable de ce téléchargement.

## Questions du lab

| Numéro | Sujet |
|---|---|
| Q1 | Heure de création du malware |
| Q2 | Serveur C2 communiqué par le fichier PPT |
| Q3 | Première bibliothèque demandée après infection |
| Q4 | Clé RC4 utilisée pour déchiffrer une chaîne encodée en base64 |
| Q5 | Technique MITRE ATT&CK principale utilisée pour voler les mots de passe |
| Q6 | Répertoire ciblé pour la suppression des DLL |
| Q7 | Temps avant auto-suppression après exfiltration |

## Fichiers du dossier

| Fichier | Description |
|---|---|
| `README.md` | Présentation du lab |
| `writeup-fr.md` | Writeup complet en français |
| `writeup-en.md` | Writeup complet en anglais |
| `notes.md` | Notes personnelles pendant l’analyse |
| `iocs.md` | Indicateurs de compromission |
| `screenshots/` | Captures utiles pour documenter le lab |

## Statut

En cours

## Avertissement

Ce lab est documenté uniquement dans un cadre éducatif, défensif et contrôlé.

Aucune activité offensive réelle ou non autorisée n’est réalisée.
