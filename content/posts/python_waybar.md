---
title: "Waybar - benachrichtigen bei neuen Emails"
date: 2021-07-11T21:09:46+02:00
draft: false
tags: [arch, sway, waybar, python, email]
---
Für meinen Desktop nutze ich [Sway](https://swaywm.org/) zusammen mit der [Waybar](https://github.com/Alexays/Waybar). Da ich meine Emails mit [Mutt](http://www.mutt.org/) lese und schreibe hätte ich gerne eine Benachrichtung in der Waybar-Leiste.

Das ist relativ leicht mit etwas [Python](https://www.python.org/) zu realsieren.

Pyhton muss natürlich installiert sein, zusätzlich wird noch [imapclient](https://pypi.org/project/IMAPClient/) benötigt:
```bash
    pip install imapclient
```    

Folgender Code fragt den Mailserver ab:

`$ vim ./config/waybar/mail.py`

```python
#!/usr/bin/env python3
import email
import imaplib
from imapclient import IMAPClient
server = IMAPClient('MAILSERVER', use_uid=True)
server.login('BENUTZERNAME', 'PASSWORD')
select_info = server.select_folder('INBOX')
messages = server.search("UNSEEN")
print ("%d Nachrichten" % len(messages))
for msgid, data in server.fetch(messages, ['ENVELOPE']).items():
    envelope = data[b'ENVELOPE']
    print('id #%d: "%s" received %s' % (msgid, envelope.subject.decode(), envelope.date))
b'Logging out'
```    

Natürlich müssen bei **MAILSERVER**, **BENUTZERNAME** und **PASSWORD** die eigenen Daten eingetragen werden

Das Script mit `chmod +x mail.py` noch ausführbar machen und man kann es direkt mit `~/.config/waybar/mail.py` testen. Wenn das funktioniert kann man es in die Konfiguration der Waybar einfügen

In der `~/.config/waybar/config` fügt man _custom/mail_ in der gewünschten Position hinzu, z.B. so:
```config
    [...]
     "modules-left": ["sway/workspaces", "sway/mode"],
        "modules-center": ["sway/window"],
        "modules-right": ["mpd", "pulseaudio", "custom/mail", "network", "battery", "custom/weather", "clock", "tray"],
        "sway/window": {
    [...]
```    

In der gleichen Datei muss noch der zugehörige Teil zum Anzeigen, bzw. Abfragen der Mail eingetragen werden:

```config
    [...]
     "custom/mail": {
            "format":{},
            "interval": 360,
            "exec": "~/.config/waybar/mail.py",
            "on-click": "exec mutt"
    		},
    [...] 
```    

Der _interval_ bezieht sich auf Sekunden und fragt das Script alle 5 Minuten ab, ob neue Emails da sind.
