---
title: "Mutt"
date: 2021-06-08T12:28:13+02:00
tags: ["email","mutt","muttrc"]
categories: ["Linux"]
draft: false
---
Um eMails zu Lesen und Schreiben nutze ich, trotz vieler anderen möglichkeiten immer noch Mutt. Für gMail sieht die .muttrc so aus:

```bash {linenos=inline}
  set from = "DIE EMAIL ADRESSE"
  set realname = "DER NAME"
  set use_from = yes
  set imap_user = "DIE EMAIL ADRESSE"
  set imap_pass = "DAS PASSWORT"
  set folder = "imaps://imap.gmail.com:993"
  set spoolfile = "+INBOX"
  set postponed ="+[Gmail]/Entwürfe"
  set trash = "+[Gmail]/Papierkorb"
  set record= "+[Gmail]/Gesendet"
  set smtp_url = "smtp://$imap_user@smtp.gmail.com:465/"
  set smtp_pass = "DAS SMTP-PASSWORT"
  ##LOCAL FOLDERS FOR CACHED HEADERS AND CERTIFICATES
  set header_cache =~/.mutt/cache/headers
  set message_cachedir =~/.mutt/cache/bodies
  set certificate_file =~/.mutt/certificates
  ##SECURING
  set move = no #Stop asking to "move read messages to mbox"!
  set imap_keepalive = 900
  ##Sort by newest conversation first.
  set sort = reverse-threads
  set sort_aux = last-date-received
  ##Set editor to create new email
  set editor="vim"
```
Selbstverständlich gibt es noch viel mehr möglichkeiten in der *.muttrc* aber um schnell mal nur eben Mails abrufen zu können reicht das. 
