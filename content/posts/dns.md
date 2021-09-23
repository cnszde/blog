---
title: "Arch Linux - festen DNS eintragen"
date: 2021-06-04T11:04:20+02:00
tags: ["dns","arch"]
categories: ["Linux","Arch"]
draft: false
---
Bei mir läuft im hintergrund der NetworkManager um mich mit verschiedenen WLAN zu verbinden, alles schön mit DHCP. Was ich allerdings nicht möchte ist, das mir über DHCP auch der DNS zur Verfügung gestellt wird. 

Wenn man nun in der **/etc/resolv.conf** einen anderen DNS-Server einträgt wird der natürlich wieder vom NetworkManager überschrieben. Natürlich kann man das auch im NetworkManager selbst handhaben, aber als Kind der Konsole gibt es auch einen anderen weg. 

Zuerst legen wir eine Datei in **/etc/NetworkManager/conf.d/** mit folgendem inhalt an: 
```bash
[main]
dns=none
```
Danach ändert man die **/etc/resolv.conf** nach seinen wünschen ab. Als Beispiel habe ich hier die DNS-Server von google genommen, natürlich geht aber auch jeder anderer DNS-Server. 
```bash
nameserver 8.8.8.8
nameserver 8.8.4.4
# und einen für IPV6
nameserver 2001:4860:4860::8888
```
Wie immer, gibt es noch zig andere wege das umzusetzen z.B.  mit dem Dienst von **systemd-resolv**
 
