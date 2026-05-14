# CyberDefenders Writeups

Dépôt personnel dédié à mes writeups CyberDefenders.

CyberDefenders est une plateforme d'entraînement Blue Team et SOC proposant des labs, des cyber ranges, des scénarios d'investigation, des parcours de formation et des certifications.

Site officiel : https://cyberdefenders.org/

## Objectif du dépôt

Ce dépôt sert à documenter ma progression en cybersécurité défensive à travers des labs CyberDefenders.

- Analyse SOC
- Blue Team
- Digital Forensics
- Incident Response
- Threat Hunting
- Malware triage
- Analyse réseau
- Analyse de logs Windows
- Investigation SIEM
- Détection avec Sigma et YARA
- Rédaction de rapports techniques

## Structure

```text
cyberdefenders-writeups/
├── README.md
├── labs/
│   ├── easy/
│   ├── medium/
│   └── hard/
├── templates/
│   ├── writeup-template-fr.md
│   └── writeup-template-en.md
├── notes/
│   ├── methodology.md
│   ├── tools.md
│   ├── commands-cheatsheet.md
│   └── soc-glossary.md
└── assets/
    └── screenshots/
```

## Format des writeups

Chaque writeup suivra une structure d'investigation claire :

1. Contexte du lab
2. Objectifs de l'analyse
3. Outils utilisés
4. Méthodologie
5. Étapes d'investigation
6. Éléments observés
7. Timeline
8. Indicateurs de compromission
9. Conclusion
10. Leçons apprises

## Exemple de structure pour un lab

```text
labs/easy/nom-du-lab/
├── README.md
├── writeup-fr.md
├── writeup-en.md
└── screenshots/
```

## Méthodologie personnelle

Chaque investigation doit rester lisible et reproductible.

Je documente :

- Les hypothèses de départ
- Les commandes utilisées
- Les artefacts analysés
- Les éléments suspects
- Les preuves importantes
- Les erreurs ou pistes abandonnées
- Les conclusions défensives

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
- SIEM ou interface fournie par le lab

## Note

Les writeups publiés ici sont rédigés dans un but pédagogique et défensif.

Ils ne doivent pas être utilisés pour contourner l'apprentissage ou simplement copier des réponses.

L'objectif est de comprendre la méthodologie, les outils et le raisonnement derrière chaque investigation.
