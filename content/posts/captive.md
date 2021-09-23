---
title: "Zugriff auf WiFi captive portal bei Nutzung eines eigenen DNS"
date: 2021-06-07T17:08:46+02:00
tags: ["dns","captive portal","WiFi"]
categories: ["linux"]
draft: false
---
Ich hatte häufig Probleme mich bei [captive portals](https://de.wikipedia.org/wiki/Captive_Portal#:~:text=etwa%20unausweichliches%20Portal%20von%20englisch,Zustimmung%20des%20Nutzer%20an%20bestimmte)  von öffentlichen WLANs, z.B. in Flughäfen, einzuloggen da ich per */etc/resolv.conf* einen eigenen DNS-Server, in meinem Fall dnscrypt, einsetze. Durch das Nutzen eines fest vorgegebenen DNS-Servers, statt der automatischen Nutzung des DNS des WLANs, kommt man nicht auf die Vorschaltseite auf der man sich für das WiFi freischalten kann.
Mit folgendem Vorgehen konnte ich aber bei meinen letzten Versuchen in öffentlichen WiFis immer die Vorschaltseite öffnen:
```bash
route -n
```
Kernel IP routing table
|Destination | Gateway | Genmask | Flags | Metric | Ref | Use|iface |
|:----------|:--------|:--------|:-----|:------|:---|:------|:---|
| 0.0.0.0    |**192.168.2.1**| 0.0.0.0| UG| 600| 0| 0| wlan0|
|192.168.2.0|0.0.0.0|255.255.255.0|U|600|0|0|wlan0|

In der Zeile startend mit *0.0.0.0* findet man in der zweiten Spalte die IP des Netzwerkrouters (Gateway), gibt man diese mit vorangestelltem *http://* in den Browser ein sollte man das captive portal zu sehen bekommen. Bei mir hat das bisher zuverlässig funktioniert.

In diesem Beispiel müsste der Aufruf wie folgt lauten:
```bash
http://192.168.2.1
```