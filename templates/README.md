# Templates

Ce dossier contient les modèles utilisés pour documenter les labs CyberDefenders.

Les templates servent à garder une structure cohérente entre les différents labs et à faciliter la rédaction des writeups, notes d'investigation et fichiers IOCs.

## Fichiers disponibles

| Fichier | Description |
|---|---|
| `writeup-template-fr.md` | Template de writeup en français |
| `writeup-template-en.md` | Template de writeup en anglais |
| `notes-template-fr.md` | Template de notes d'investigation en français |
| `notes-template-en.md` | Template de notes d'investigation en anglais |
| `iocs-template-fr.md` | Template d'IOCs en français |
| `iocs-template-en.md` | Template d'IOCs en anglais |

## Utilisation recommandée

Pour créer un nouveau lab :

1. Créer un dossier dans `labs/easy/`, `labs/medium/` ou `labs/hard/`
2. Copier les templates nécessaires
3. Renommer les fichiers selon le lab
4. Remplir les sections progressivement pendant l'investigation
5. Masquer les réponses directes avant publication si nécessaire

## Exemple

```text
labs/easy/new-lab/
├── readme_NEWLAB.md
├── Readme_NEWLAB_EN.md
├── writeup-fr.md
├── writeup-en.md
├── notes-fr.md
├── notes-en.md
├── iocs-fr.md
└── iocs-en.md
```

## Principe

Les templates privilégient :

- la clarté
- la reproductibilité
- la méthode
- la documentation des preuves
- la séparation entre notes, writeup et IOCs
- la publication responsable avec réponses partiellement masquées
