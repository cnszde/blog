---
title: "Verschlüsselte Arch Linux installation"
date: 2021-07-08T20:55:45+02:00
draft: false
tags: [arch, verschluesselt, crypt, uefi, cryptsetup]
---
Ich beschreibe hier **mein** vorgehen [Arch](https://archlinux.org) verschlüsselt zu installieren. Ich werde auch nur die Grundlegende installation beschreiben. Fenstermanager und Co. lasse ich raus, da hat ja jeder so seine eigenen vorlieben.

Ich werde auch nicht das erstellen des USB-Sticks / CD des [Iso](https://archlinux.org/download/) erklären und auch nicht das herstellen einer Internetverbindung, etwas mitarbeit darf schon sein, es ist also **keine** _Copy und Paste_ Anleitung.

Eine gute Anlaufstelle bei Problemen ist natürlich das [Wiki](https://wiki.archlinux.org/) von Archlinux. Dort stehen die gleichen Informationen wie hier, allerdings wesentlich ausführlicher.

Partitionsschema wird sein (bis auf boot ist das alles natürlich änderbar):

* 512 MB boot
* 8 GB swap
* 40 GB root
* Rest für das home Verzeichniss

## Partition erstellen

```bash
gdisk /dev/sda
```

**Boot - Partition**

* Alle Partitionen auf der Festplatte werden gelöscht: „o“
* Neue Partition anlegen: „n“
* Als erste Partition festlegen: „1“
* Ersten Sektor auf 2048 setzen: \[ENTER\]
* Letzten Sektor setzen / auf 512 MB Partitionsgröße setzen: „+512M“
* „ef00“ als Partitionstyp wählen: „ef00“

**Hauptpartition**

* Neue Partition anlegen: „n"
* Als zweite Partition festlegen: „2“
* Ersten Sektor wählen: \[ENTER\]
* Letzten Sektor wählen: \[ENTER\]
* Partitionstyp wählen: \[ENTER\]

Mit ‘w’ werden die Änderungen geschrieben, was natürlich mit ‘y’ bestätigt werden muss

## Verschlüsselung einrichten

```bash
cryptsetup -c aes-xts-plain64 -y -s 512 luksFormat /dev/sda2
```

## LVM einrichten

```bash
## Öffnen der virtuellen Partition
cryptsetup luksOpen /dev/sda2 lvm

## LVM aktivieren
pvcreate /dev/mapper/lvm

## Volume Gruppe erstellen
vgreate main /dev/mapper/lvm
        
## swap Partition erstellen
lvcreate -L 8G -n swap main
        
## root Partition erstellen
lvcreate -L 40G -n root main
        
## home bekommt den Rest
lvcreate -l 100%FREE -n home main
        
## jetzt noch swap erstellen und aktivieren
mkswap /dev/mapper/main-swap
swapon /dev/mapper/main-swap
```

## Formatieren und mounten

Die Partitionen müssen noch formatiert werden

```bash
## formatieren
mkfs.vfat /dev/sda1 
mkfs.ext4 /dev/mapper/main-root
mkfs.ext4 /dev/mapper/main-home
        
## mounten der root Partition
mount /dev/mapper/main-root /mnt
        
## Verzeichnisse für boot und home erstellen
mkdir /mnt/boot
mkdir /mnt/home
        
## Den rest einhängen
mount /dev/sda1 /mnt/boot
mount /dev/mapper/main-home /mnt/home
```

## Basissystem installieren

```bash
pacstrap /mnt/ base base-devel linux linux-firmware networkmanager lvm2 iwd vim
```

## fstab erzeugen

```bash
genfstab -p /mnt > /mnt/etc/fstab
```

Es sollten 4 einträge in der /mnt/etc/fstab zu sehen sein.

## Konfigurieren

Dann kann in das “gechrooted” werden um dort noch so ein paar kleinigkeiten (Tastatur, Sprache, Zeitzone usw. ) einzustellen.

```bash
## chroot ins System
arch-chroot /mnt
        
## hostname setzen
echo computer >  /etc/hostname

## Sprache
echo LANG=de_DE.UTF-8 > /etc/locale.conf
        
## Tastaturbelegung
echo KEYMAP=de-latin1 > /etc/vconsole.conf
echo FONT=lat9w-16 >> /etc/vconsole.conf
        
## Spracheinstellung (Locale) setzen
vim /etc/locale.gen
## das # am Anfang folgender Zeilen entfernen: 
    #de_DE.UTF-8 UTF-8
    #de_DE ISO-8859-1
    #de_DE@euro ISO-8859-15
    #en_US.UTF-8
locale-gen
        
## Zeitzone setzen
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
```

## Kernel - Image erstellen

Damit das System beim booten alle nötigen Module laden kann, muss die mkinitcpio angepasst werden.

```bash
## In der datei /etc/mkinitcpio.conf folgendes ändern
MODULES=(ext4)
HOOKS=(base udev autodetect modconf block keyboard keymap encrypt lvm2 filesystems fsck shutdown)

## Kernel neu "bauen"
mkinitcpio -p linux
```

## Bootloader installieren

Ich nehme den UEFI - Bootloader von systemd, der ist einfach und unkompliziert.

```bash
bootctl install
```

Die folgenden Datei muss angelegt werden

```bash
vim /boot/loader/entries/arch.conf
## mit folgendem Inhalt
    title    Arch Linux
    linux    /vmlinuz-linux
    initrd   /initramfs-linux.img
    options  cryptdevice=/dev/sda2:main root=/dev/mapper/main-root rw lang=de init=/usr/lib/systemd/systemd locale=de_DE.UTF-8
```


Ein kleines Menü wird auch noch gebraucht

```bash
vim /boot/loader/loader.conf
## Mit folgendem Inhalt
    timeout 5
    default arch.conf
```

## Abschliesen der Grundinstallation

der Benutzer _root_ braucht noch ein Passwort

```bash
passwd
```

danach das System verlassen, die Platten aushängen und neu starten.

```bash
exit
umount /mnt/{boot,home,}
reboot
```

## ToDo

+ Dienste wie ACPIC, DBUS, CUPS installieren und starten
+ Benutzer anlegen
+ Grafische Oberfläche installieren
+ usw

Dabei hilft am besten das [arch-wiki](https://wiki.archlinux.org/title/General_recommendations)
