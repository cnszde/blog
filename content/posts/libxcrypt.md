---
title: "libxcrypt und schwache Passwortverschlüsselungen"
date: 2021-06-18T22:44:38+02:00
tags: ["crypt","passwörter","verschlüsselung"]
categories: ["arch"]
draft: false
---
Ab Version 4.4.21 unterstützt libxcrypt keine scwachen Hashalgorithmen zur Passwortverschlüsselung mehr. Das betrifft die Anmeldung am Betriebssystem. Es kann bei Passwörtern, die noch mit alten Hashes verschlüsselt worden sind, dazu führen, dass man einmalig dazu aufgefordert wird, sein Passwort zu ändern. Displaymanager haben damit so ihre Probleme. Falls man also in Schwierigkeiten beim Anmelden kommt, sollte man sich einmal über ein Terminal anmelden und das Passwort ändern.
