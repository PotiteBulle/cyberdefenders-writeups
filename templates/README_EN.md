# Templates

This folder contains the templates used to document CyberDefenders labs.

The templates help keep a consistent structure across labs and make it easier to write writeups, investigation notes, and IOC files.

## Available Files

| File | Description |
|---|---|
| `writeup-template-fr.md` | French writeup template |
| `writeup-template-en.md` | English writeup template |
| `notes-template-fr.md` | French investigation notes template |
| `notes-template-en.md` | English investigation notes template |
| `iocs-template-fr.md` | French IOC template |
| `iocs-template-en.md` | English IOC template |

## Recommended Usage

To create a new lab:

1. Create a folder in `labs/easy/`, `labs/medium/`, or `labs/hard/`
2. Copy the required templates
3. Rename the files according to the lab
4. Fill in the sections progressively during the investigation
5. Mask direct answers before publication if needed

## Example

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

## Principle

The templates focus on:

- clarity
- reproducibility
- methodology
- evidence documentation
- separation between notes, writeup, and IOCs
- responsible publication with partially masked answers
