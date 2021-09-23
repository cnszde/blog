---
title: "CryptSetup Passwort ändern"
date: 2021-09-18T12:21:48+02:00
draft: false
---
Das Ändern des Passworts auf einem LUKS-Laufwerk mit nur einem Passwort ist einfach: Terminal öffnen und folgenden Befehl ausführen, indem der aktuelle Standort des Laufwerks durch "sdX" ersetzt wird. Dann das bestehende Kennwort eingeben, um ein Neues erstellen zu können.
```
sudo cryptsetup luksChangeKey /dev/sdX
```
