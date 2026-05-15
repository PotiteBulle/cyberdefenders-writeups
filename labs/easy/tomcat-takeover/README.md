# Tomcat Takeover Lab

Statut : Terminé  
Langue : Français principal, miroir anglais inclus

## Vue d’ensemble

Ce dépôt contient le write-up final et les notes d’analyse du **Tomcat Takeover Lab**.

Le lab porte sur une analyse forensic réseau d’un serveur Apache Tomcat compromis à partir d’un fichier PCAP.  
L’investigation permet d’identifier l’attaquant, le service exposé, l’activité d’énumération, les identifiants valides utilisés, l’upload du fichier WAR malveillant, le reverse shell JSP déployé, ainsi que le mécanisme de persistance installé sur la machine victime.

## Structure du dépôt

```text
tomcat-takeover/
├── iocs-en.md
├── iocs.md
├── notes-en.md
├── notes.md
├── README_EN.md
├── README_TOMCAT_TAKEOVER_EN.md
├── README_TOMCAT_TAKEOVER.md
├── README.md
├── writeup-en.md
└── writeup-fr.md
```