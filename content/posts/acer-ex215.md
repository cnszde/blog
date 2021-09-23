---
title: "Acer Ex215-21-48SC"
date: 2021-06-04T16:21:40+02:00
tags: ["laptop","linux","hardware","installation"]
categories: ["linux","hardware"]
draft: false
---
Ich habe günstig den o.G. Laptop bekommen, natürlich mit Windows 10. Kompletten technischen Daten kann man sich [hier](https://www.notebooksbilliger.de/acer+extensa+15+ex215+21+46sc+634718) oder [hier](https://www.amazon.de/Acer-Extensa-EX215-21-46SC-A4-9120e-Windows/dp/B07ZHLW96Z)anschauen. 

Windows 10, Office und Co laufen auf der Kiste, da ich aber lieber ein [Archlinux](https://archlinux.org) benutzen wollte, eben im Bios Secure Boot ausschalten (Master Password muss vergeben sein) und Booten über USB-Stick (F12) erlauben. 

Vom Stick booten, Tastatur auf Deutsch umstellen und die Wlan Verbindung herstellen. Das alles ist gut im [wiki](https://wiki.archlinux.org/title/Installation_guide) beschrieben. 

Ein freundliches **fdisk -l** um zu schauen welche Festplatte erkannt wurden, brachte ein kleines "ups" zum vorschein. Es wurd nur der USB-Stick erkannt, keine Festplatte. 
Der Grund dafür ist, das die Festplatte eine [NvME-Festplatte](https://de.wikipedia.org/wiki/NVM_Express) ist und der notwendige Kernel Treiber nicht geladen wird. 

Abhilfe schafft im Boot-Menü (Taste 'e') folgendes in die Kernel-Zeile zu tippen:
```bash
nvme_core.default_ps_max_latency_us=0
```
Nach dem booten sieht dann auch das System die Festplatte und man kann wie gewohnt installieren. Das gillt im übrigen auch für [Debian](https://debian.org)

Ist die installation fertig muss natürlich die Zeile auch noch in der **/etc/default/grub** eingefügt werden.
```bash
GRUB_CMDLINE_LINUX_DEFAULT="nvme_core.default_ps_max_latency_us=0"
```
Danach noch ein 
```bash
grub-mkconfig -o /boot/grub/grub.cfg

```
und man kann die chroot Umgebung verlassen. Der Laptop startet normal und man kann mit der restlichen Installation fortfahren. 
