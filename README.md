# CyberDefenders Writeups

Dépôt personnel dédié à mes writeups CyberDefenders.

CyberDefenders est une plateforme d'entraînement Blue Team et SOC proposant des labs, des cyber ranges, des scénarios d'investigation, des parcours de formation et des certifications.

Site officiel : https://cyberdefenders.org/

## Objectif du dépôt

Ce dépôt sert à documenter ma progression en cybersécurité défensive à travers des labs CyberDefenders.

Les domaines travaillés sont notamment :

- analyse SOC
- Blue Team
- Digital Forensics
- Incident Response
- Threat Hunting
- Malware triage
- analyse réseau
- analyse de logs Windows
- investigation SIEM
- détection avec Sigma et YARA
- rédaction de rapports techniques

## Structure du dépôt

```text
cyberdefenders-writeups/
├── README.md
├── README_EN.md
├── labs/
│   ├── README.md
│   ├── easy/
│   ├── medium/
│   └── hard/
├── templates/
│   ├── README.md
│   ├── README_EN.md
│   ├── writeup-template-fr.md
│   ├── writeup-template-en.md
│   ├── notes-template-fr.md
│   ├── notes-template-en.md
│   ├── iocs-template-fr.md
│   └── iocs-template-en.md
└── notes/
    ├── methodology.md
    ├── tools.md
    ├── commands-cheatsheet.md
    └── soc-glossary.md
```

## Format des writeups

Chaque writeup suit une structure d'investigation claire :

1. Informations générales
2. Liens utiles
3. Contexte du lab
4. Objectif de l'analyse
5. Préparation de l'environnement
6. Artefacts fournis
7. Méthodologie
8. Analyse technique
9. Réponses aux questions avec réponses partiellement masquées
10. IOCs
11. Timeline
12. Conclusion
13. Leçons apprises

## Exemple de structure pour un lab

```text
labs/easy/nom-du-lab/
├── readme_NOMDULAB.md
├── Readme_NOMDULAB_EN.md
├── writeup-fr.md
├── writeup-en.md
├── notes-fr.md
├── notes-en.md
├── iocs-fr.md
└── iocs-en.md
```

## Méthodologie personnelle

Chaque investigation doit rester lisible, structurée et reproductible.

Je documente :

- les hypothèses de départ
- les commandes utilisées
- les artefacts analysés
- les éléments suspects
- les preuves importantes
- les erreurs ou pistes abandonnées
- les conclusions défensives
- les limites de l'analyse
- les leçons apprises

## Outils possibles

Selon le lab, les outils utilisés peuvent inclure :

- Wireshark
- CyberChef
- Volatility
- Autopsy
- FTK Imager
- Windows Event Viewer
- PowerShell
- Python
- YARA
- Sigma
- jq
- grep
- strings
- VirusTotal
- ANY.RUN
- Hybrid Analysis
- MITRE ATT&CK
- SIEM ou interface fournie par le lab

## Gestion des réponses

Les réponses directes aux questions des labs peuvent être partiellement masquées dans les writeups publics.

L'objectif est d'éviter le simple copier-coller tout en montrant la méthode d'investigation, les outils utilisés et le raisonnement technique.

Exemple :

```text
Réponse complète : non publiée directement
Réponse masquée : http://******.example/***.php
Méthode : documentée clairement
```

## Avertissement

Les writeups publiés ici sont rédigés dans un but pédagogique, défensif et éthique.

Ils ne doivent pas être utilisés pour contourner l'apprentissage ou simplement copier des réponses.

L'objectif est de comprendre la méthodologie, les outils et le raisonnement derrière chaque investigation.

Aucune activité offensive réelle ou non autorisée n'est réalisée dans le cadre de ce dépôt.
