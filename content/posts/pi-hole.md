---
title: "Pi Hole Standalone"
date: 2021-06-13T13:41:49+02:00
tags: ["pi-hole","werbung","arch","aur"]
categories: ["linux","arch"]
draft: false
---
**Pi-hole** ist eine Software mit der Funktion eines Tracking- und Werbeblockers. 
Es übernimmt damit die Aufgabe, Domainanfragen der verbundenen Clients aufzulösen und in IP-Adressen umzuwandeln. Auf der Basis von Ausschlusslisten von bekannten Werbe- oder Trackingdomains und benutzerdefinierten Ausschlusslisten werden Anfragen entweder an konfigurierbare andere DNS-Server weitergeleitet oder, falls eine angefragte Domain in einer Ausschlusslisten existieren sollte, eine technisch unbrauchbare IP-Adresse an den Client ausgeliefert (sog. DNS sinkhole). Durch die Übermittlung einer unbrauchbaren IP-Adresse an den Client kann dieser auf die angefragte Domain nicht zugreifen und folglich Werbung und/oder Tracking-Inhalte nicht abrufen. 

Das ganze gibt es auch als Standalone-Version. 
Unter Arch kann man das mit yay recht einfach installieren
```bash
yay -S pi-hole-standalone
```
Nach der installation den Systemdienst starten und den DNS auf *127.0.0.1* setzen
```bash
sudo systemctl enable pihole-FTL.service
```
Entweder man startet das Netzwerk neu, oder gleich den ganzen Rechner