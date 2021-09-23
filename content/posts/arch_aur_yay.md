---
title: "Arch - yay Error libalpm.so.12"
date: 2021-06-01T19:08:28+02:00
tags: ["pacman","wrapper","go","aur","helper"]
categories: ["linux","arch"]
draft: false
---
Für Pakete aus den aur (archlinux user repository) benutze ich das kleine Programm [yay](https://aur.archlinux.org/packages/yay/). 
Dieses begrüßte mich heute mit einem fröhlichem
```bash 
error while loading shared libraries: libalpm.so.12
```
Die Lösung ist aber recht einfach. Das PKG-Build erneut von der Webseite laden und es erneut bauen. 

```bash
sudo pacman -R yay
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```
Danach funktionert das Programm wieder wie gewohnt. Es ist vielleicht nicht die elgeganteste Methode aber in diesem Fall die schnellste. 





