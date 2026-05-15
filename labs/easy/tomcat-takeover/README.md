# Tomcat Takeover Lab

## Plateforme

CyberDefenders

Lien officiel : https://cyberdefenders.org/blueteam-ctf-challenges/tomcat-takeover/

## Informations

| Champ | Valeur |
|---|---|
| Lab | Tomcat Takeover |
| Parcours | SOC Analyst Tier 1 |
| Niveau | Level 3 |
| Difficulté | Easy |
| Catégorie | Network Forensics |
| Durée estimée | 30 minutes |
| Outils indiqués | Wireshark, NetworkMiner |
| Statut | En cours |

## Résumé

Ce lab consiste à analyser un fichier PCAP afin d’identifier une activité suspecte visant un serveur web Apache Tomcat.

Le scénario indique que l’équipe SOC a détecté une activité anormale sur un serveur web interne. Le fichier PCAP doit permettre de comprendre le périmètre de l’attaque et les actions réalisées par l’attaquant.

L’analyse est orientée **Network Forensics**, **Wireshark**, **analyse HTTP**, **Tomcat administration** et **compromission web**.

## Artefact fourni

| Fichier | Type | Commentaire |
|---|---|---|
| `web server.pcap` | PCAP | Capture réseau à analyser |

## Hashes de l’artefact

| Type | Valeur |
|---|---|
| MD5 | `9ab62c5a2a1f8d0030f8240355305e7` |
| SHA256 | `9a8db2ec46186ff541f8f307544a5ceb07ce702f36d36fd8ae964bcb53f716` |

## Objectifs pédagogiques

Les objectifs principaux du lab sont :

- analyser une capture réseau avec Wireshark
- identifier une activité de scan
- repérer l’adresse IP source de l’attaquant
- identifier les ports ouverts et le panneau d’administration web
- reconnaître les outils d’énumération utilisés
- analyser une tentative de brute-force HTTP
- identifier une authentification réussie
- retrouver un fichier malveillant uploadé
- comprendre une logique de reverse shell
- identifier une commande de persistance

## Questions du lab

| Numéro | Sujet |
|---|---|
| Q1 | Adresse IP source responsable des requêtes de scan |
| Q2 | Pays d’origine associé à l’adresse IP attaquante |
| Q3 | Port donnant accès au panneau d’administration web |
| Q4 | Outil utilisé pendant l’énumération |
| Q5 | Répertoire spécifique lié au panneau d’administration |
| Q6 | Identifiants utilisés avec succès pour se connecter |
| Q7 | Nom du fichier malveillant uploadé |
| Q8 | Commande planifiée pour maintenir la persistance |

## Structure du dossier

| Fichier | Description |
|---|---|
| `README.md` | Présentation du lab en français |
| `README_EN.md` | Présentation du lab en anglais |
| `README_TOMCAT_TAKEOVER.md` | Présentation détaillée du lab en français |
| `README_TOMCAT_TAKEOVER_EN.md` | Présentation détaillée du lab en anglais |
| `writeup-fr.md` | Writeup en français |
| `writeup-en.md` | Writeup en anglais |
| `notes.md` | Notes d’investigation en français |
| `notes-en.md` | Notes d’investigation en anglais |
| `iocs.md` | Indicateurs de compromission en français |
| `iocs-en.md` | Indicateurs de compromission en anglais |

## Choix de documentation

Aucune capture d’écran n’est nécessaire dans ce dossier.

Le lab peut être documenté avec une approche textuelle : commandes utilisées, observations Wireshark, filtres, flux HTTP, IOCs, timeline et mapping MITRE ATT&CK.

## Avertissement

Les réponses seront partiellement masquées dans les writeups publics afin d’éviter le copier-coller direct et de préserver l’intérêt pédagogique du lab.

Ce lab est documenté uniquement dans un cadre éducatif, défensif et contrôlé.
