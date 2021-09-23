---
title: "Hugo - Script für den loaklen Web-Server"
date: 2021-06-08T11:55:32+02:00
tags: ["hugo","blog","server","apache"]
categories: ["linux"]
draft: false
---
Für meinen Gedanken und Notizen nutze ich [Hugo](https://gohugo.io), ein schlankes, schnelles Framework um Webseiten zu erstellen, ganz ohne php und co. Allerdings möchte ich nicht immer den *Hugo* eigenen Webserver starten sondern einen Webserver. In meinem Fall ist das [Apache](http://httpd.apache.org/). Natürlich funktioniert das ganze auch mit jedem anderen Webserver, wenn man die Pfade anpasst. 

Im Hauptverzeichnis des Blogs erstellen wir eine Datei **vim transfer.sh** mit folgendem inhalt:

```bash {linenos=inline}
    #!/usr/bin/bash
    rm -r public/*
    sudo rm -r /srv/http/*
    hugo
    sudo cp -rv public/* /srv/http
```
Das Script sollte eigentlich selbsterklärend sein. 
