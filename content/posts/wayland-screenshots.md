---
title: "Screenshots mit Wayland (Sway)"
date: 2021-07-22T19:46:41+02:00
draft: false
tags: [screenshots, wayland, sway]
---
Mein Desktop läuft mit [Sway](https://swaywm.org/) unter [Wayland](https://wayland.freedesktop.org/). Viele Screenshot-Programme funktionieren allerdings nur unter
[X11](https://www.x.org/wiki/) oder sind mir zu  aufgeblasen.
Die Lösung für mich liegt in 2 kleinen Programmen, zum einen in [grim](https://github.com/emersion/grim) und zum anderen in [slurp](https://github.com/emersion/slurp).

Mit einem kleinen Einzeiler, kann man von einem zuvor markierten Bereich einen Screenshot erstellen.

```bash
grim -g "$(slurp)" screenshot.png
```

Etwas komfortabler geht es mit einem Script:

$ vim shot.sh

```bash
#!/usr/bin/sh
grim -g "$(slurp)" ~/Bilder/screenshot_$(date +%d.%m-%H:%M).png
```

Das Script mit `chmod +x shot.sh` ausführbar machen.

Der Screenshot wird im eigenen Verzeichniss unter Bilder gespeichert. Zum Unterscheiden der Screenshots werden diese mit aktuellem Datum und Uhrzeit im Dateinamen gespeichert. (z.B. screenshot_22.07-09:12.png)

Wenn man das Script noch nach /usr/local/bin kopiert, ist es Systemweit verfügbar.

`sudo cp shot.sh /usr/local/bin/shot`

Man kann es jetzt einfach aus jedem Terminal mit

`$ shot`

aufrufen
