---
title: "Automatische übersetzen von LaTex Quellcode"
date: 2021-06-08T15:54:58+02:00
tags: ["quellcode","latex"]
categories: ["linux","latex"]
draft: false
---
Schreibt man gerade an einem LaTex-Dokument, schaut der Workflow eigentlich immer relativ gleich aus: Tex-Quellcode in einem Texteditor schreiben, nach einer Änderung Quelltext in zum Beispiel PDF übersetzen und das Ergebnis anschließend anschauen. Das häufige hin und her Wechseln zwischen Terminal und den beiden anderen Fenstern kann gerade bei längerem Arbeiten nerven. Eine eigene Entwicklungsumgebung finde ich für LaTex dagegen auch übertrieben. 

Abhilfe schafft hier das Programm **latexmk**, was leider die Einschränkung hat das standardmäßig der Adobe Reader benutzt wird. Um das zu ändern einfach folgendes in die .latexmkrc eintragen. Ich nutze mupdf, es funktioniert natürlich auch mit allen anderen PDF-Readern. 
```bash
echo '$pdf_previewer = "start mupdf";' >> ~/.latexmkrc
```

**latexmk** wird dann wie folgt aufgerufen:
```bash
latexmk -pdf -pvc dokument.tex
```
Das ergibt gerade bei größeren Dokumenten ein besseren Workflow.

