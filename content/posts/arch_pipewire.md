---
title: "Pulseaudio durch Pipewire ersetzen"
date: 2021-07-09T21:09:01+02:00
draft: false
tags: [arch, pipewire, pulseaudio]
---
[PipeWire](https://pipewire.org/), das Low-Level-Multimedia-Framework soll bei Audio und Video das Ruder übernehmen.

Es soll Aufnahme und Wiedergabe sowohl von Audio als auch von Video mit minimaler Latenz und Unterstützung für PulseAudio-, JACK-, ALSA- und GStreamer-basierte Anwendungen bieten.

PulseAudio lässt sich unter [Arch Linux](https://archlinux.org) sehr schnell ersetzen:
```bash
        sudo pacman -S pipewire pipewire-pulse
```    

Natürlich muss die Abfrage ob PulseAudio entfernet werden soll mit ‘j’ bestätigt werden. Ob PipeWire funktioniert kann mit _pactl_ überprüft werden
```bash
$ pactl info
 ----------
		Server String: /run/user/1000/pulse/native
		Library Protocol Version: 34
		Server Protocol Version: 35
		Is Local: yes
        Client Index: 53
        Tile Size: 65472
        User Name: christian
        Host Name: coldbeer
        Server Name: PulseAudio (on PipeWire 0.3.31)
        Server Version: 14.0.0
        Default Sample Specification: float32le 2ch 48000Hz
        Default Channel Map: front-left,front-right
        Default Sink: alsa_output.pci-0000_00_1b.0.analog-stereo
        Default Source: alsa_input.pci-0000_00_1b.0.analog-stereo
        Cookie: 1966:7e37
 ```  

Das war es schon. Natürlich gibt es im [Wiki](https://wiki.archlinux.org/title/PipeWire) noch einiges mehr zu PipeWire.
