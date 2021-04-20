# 📑 LB3 Dokumentation - Wordpress mit Datenbank 📑
<p align="left">
  <img height = "240" src="./media/docker.png">
  <img height = "240" src="./media/wordpress.png">
</p>

_Erstellt von [Raphael Frisano](https://github.com/RaphaelFrisano) am 20.04.2021_

---
<br>

# 🖼️ Grafische Übersicht der Umgebung 🖼️

<img src="./media/network.png">

<h2>☄️ Ports & Verbindungen ☄️</h2>

phpmyadmin ➡️ Port 3306 ➡️ mysql<br>
phpmyadmin ➡️ Port 8080 ➡️ Virtual Machine

wordpress ➡️ Port 3306 ➡️ mysql<br>
wordpress ➡️ Port 8000 ➡️ Virtual Machine

Virtual Machine ➡️ Port 8000 & 8080 ➡️ Host System + Netzwerk


# 📜 Projektbeschreibung 📜
> Mit Docker compose werden automatisch 3 Container gestartet;<br>
> Als erstes wird der Container erstellt, auf dem die mysql Datenbank läuft.<br>
> Danach wird der zweite Container mit PHPmyadmin gestartet um die Datenbank über einen Browser managen zu können.<br>
> Zu gute letzt wird dann als drittes der Container mit Wordpress erstellt, welcher die mysql Datenbank im hintergrund verwendet.

# ❔ Wie es funktioniert ❔
*Das ganze Projekt kann auf der von Vagrant erstellten VM mit 2 einfachen commands gestartet werden, da Docker Compose schon installiert wurde und man sich in SMB mit seinem Windows login einloggt.*

```
cd /vagrant
docker-compose -f dockerfile.yml up -d
```


Am start des Files wird ein Volume für die Datenbank / Website erstellt, sowieein Netzwerk so dass alle Container miteinander kommunizieren können.
```yml
volumes:
  db_data:
networks:
  wpsite:
```

Der rest des ganzen Files ist in 3 Teile aufgeteilt:

<h2>🐬 mysql Konfiguration 🐬</h2>
Hier wird als erstes der Container "db" erstellt, mit Passwort, User, etc. für die Datenbank.

Es wird auch angegeben das der Container nach einem Docker Neustart wieder aufgestartet werden soll und das er im Netzwerk wpsite ist.

```yml
db:
  image: mysql:latest
  volumes:
    - db_data:/var/lib/mysql
  restart: always
  environment:
    MYSQL_ROOT_PASSWORD: password
    MYSQL_DATABASE: mybase
    MYSQL_USER: raphael
    MYSQL_PASSWORD: password
  networks:
    - wpsite
```

<h2>⛵ phpmyadmin Konfiguration ⛵</h2>
Danach wird hier der Container phpmyadmin erstellt. Dies jedoch nur imfall das der Container "db" schon läuft. Es werden alle Daten angegeben sodass sich phpmyadmin auf die DB einloggen kann und die Ports zur Weboberfläche werden weitergeleitet.

Es wird auch angegeben das der Container nach einem Docker Neustart wieder aufgestartet werden soll und das er im Netzwerk wpsite ist.

```yml
phpmyadmin:
  depends_on:
    - db
  image: phpmyadmin:latest
  restart: always
  ports:
    - '8080:80'
  environment:
    PMA_HOST: db
    MYSQL_ROOT_PASSWORD: password
  networks:
    - wpsite
```

<h2>📰 Wordpress Konfiguration 📰</h2>
Zu guter letzt wird dann der Wordpress Container aufgesetzt. Dies jedoch nur imfall das der Container "db" schon läuft. Es werden alle Daten angegeben sodass sich Wordpress auf die DB einloggen kann und die Ports zur Website werden weitergeleitet.
Das Volumen für die Website wird auch schon erstellt.

Es wird auch angegeben das der Container nach einem Docker Neustart wieder aufgestartet werden soll und das er im Netzwerk wpsite ist.

```yml
wordpress:
  depends_on:
    - db
  image: wordpress:latest
  ports:
    - '8000:80'
  restart: always
  volumes: ['./:/var/www/html']
  environment:
    WORDPRESS_DB_HOST: db:3306
    WORDPRESS_DB_USER: raphael
    WORDPRESS_DB_PASSWORD: password
  networks:
    - wpsite
```


# 🔧 Testing 🔧
<h3>✔️ Erstellung der Container ✔️</h3>
Es konnten alle Container erfolgreich erstellt werden.<br>
<img height = "" src="./media/all_created.png"><br>

Genauso wurde auch das Volumen sowie das Netztwerk erstellt.<br>
<img height = "" src="./media/internal_network.png"><br>
<img width = "600" src="./media/internal_network2.png"><br>

<h3>❌ Verbindung auf Port 8080 und 8000 ❌</h3>
Leider gab es diesesmal auch wieder Probleme. Egal ob man auf phpmyadmin (Port 8080) oder Wordpress (Port 8000) zugreifen will, bekomme ich diesen Error. Ich hatte versucht ihn zu fixen intem ich andere Ports versucht habe was jedoch auch nicht viel geholfen hatte.<br>
Eine Vermutung von mir könnte sein das es etwas an meinem Heimnetz zu tun hat da wir recht strikte Regeln im Netz haben, weswegen das Netz probleme hat eine von einem Container / VM kommende Verbindung zuzulassen.<br>
<img height = "" src="./media/error.png"><br>

# 📚 Quellen 📚
<h3>Für Projekt benötigte Container</h3>

[mysql](https://hub.docker.com/_/mysql)<br>
[phpmyadmin](https://hub.docker.com/_/phpmyadmin)<br>
[wordpress](https://hub.docker.com/_/wordpress)<br>

<h3>Tutorials / Informationen</h3>

[Screencasts](https://www.nanoo.tv/link/v/DDKjuKJG)<br>
[Docker cheatsheet](https://tbzedu.sharepoint.com/sites/M300_Documents/Freigegebene%20Dokumente/Forms/AllItems.aspx?id=%2Fsites%2FM300%5FDocuments%2FFreigegebene%20Dokumente%2Fcheat%2Dsheet%2Dv2%2Epdf&parent=%2Fsites%2FM300%5FDocuments%2FFreigegebene%20Dokumente&p=true&originalPath=aHR0cHM6Ly90YnplZHUuc2hhcmVwb2ludC5jb20vOmI6L3MvTTMwMF9Eb2N1bWVudHMvRVJZLUdwRjR3V2hKdEJJSV8tV0htSmtCNURPRWVZTkNfNExxa0tVSlRGSnlCdz9ydGltZT1jU3RweGNBRDJVZw)<br>
[Dockerhub (Einzelne Container)](https://hub.docker.com/)