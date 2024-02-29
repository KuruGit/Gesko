---
layout: post
title: Das Zettelkasenprinzip mit Joplin und Open Source Tools umgesetzt
description: Hier möchte ich ein wenig auf meine Toolchain eingehen mit der ich mir geräteübergreifend einen Zettelkasten eingerichtet habe 
summary: Hier möchte ich ein wenig auf meine Toolchain eingehen mit der ich mir geräteübergreifend einen Zettelkasten eingerichtet habe 
tags: Linux Schreiben Texte Typora Joplin Markdown textfiles
date: 2023-04-01 00:00:00
---

# Was ist ein Zettelkasten und warum brauche ich den?

kurzer Abriss des Zettelkastens, Bibtex Referenzen, Artikel schreiben, Tags und Quer-Referenzen zwischen Notizen und dem Konzept "Schreiben um zu verstehen". Abgrenzung verschiedener Notiz-Typen (Archiv, Recherche und Artikel, kurzfristige Notizen und Ideen)

- https://de.wikipedia.org/wiki/Niklas_Luhmann als Erfinder des Zettelkastens, damals aber noch analog
- sehr gutes Buch zum Thema, allerdings mit vielen Verweisen auf Studien etc: [Sönke Ahrens - Das Zettelkasten-Prinzip (affiliate Link)](https://amzn.to/49KLIuE)

### Das Zettelkasten-Prinzip kurz erklärt

- atomare Notizen: es werden nur kleine, in sich abgeschlossene und klar abgegrenzte Notizen erstellt um einen Fokus zu behalten. Ausnahme: Zusammenführung der Notizen zum Schreiben eines Artikels, siehe weiter unten Typora 
- "Tags" und eindeutige Bezeichner, bspw. in Form von "unique identifiers" oder "Seriennummern" erlauben die genaue Zuordnung von Notizen und die Zuordnung untereinander. Notizen sollten über Referenzen untereinander vernetzt werden um so auf Dauer ein Netzwerk aus Notizen zu bilden und thematische Querverweise sichtbar zu machen
- Notizen sollten langlebig sein. Es gibt nach dem Konzept von Luhmann verschiedene Typen von Notizen, wichtig sind letztlich aber die Notizen die dauerhaft angelegt werden. 
- Man sollte es vermeiden nur aus Gewohnheit Notizen anzulegen ("Textmarker Syndrom" in Lehrbüchern)
- Nach Luhmann ergibt sich automatisch mit der Zeit eine Organisation der Notizen anhand ihrer Querverweise. Hier weichen wir vom Konzept von Luhmann ab da uns die Technik hier andere Möglichkeiten eröffnet als ein händisch geführter Zettelkasten und wir außerdem bspw. die Möglichkeit nutzen wollen Notizbücher in Joplin komplett zu exportieren.
- Notizen sollten iterativ erstellt und bei neuen Erkenntnissen einer Revision unterzogen werden. Um auf einen vorigen Punkt zurück zu kommen kann man hierdurch auch ab und zu feucht durchwischen und Notizen entfernen die man nicht gebraucht (siehe Textmarker Syndrom).
- Zeichnen Sie alles auf! Dies widerspricht dem vorigen Punkt in gewisser Hinsicht, es geht aber darum auch flüchtige Gedanken erstmal zu notieren und Querverweise zu erstellen da sich hierdurch ggf. ungewöhnliche und vorher unbekannte Verbindungen zwischen Themen ergeben die man vorher nicht gesehen hat. Eine gute Möglichkeit ist es die Notizen in einen Ideen Ordner zu stecken und dort zu sammeln und nach einer gewissen Zeit zu rekapitulieren.
- auch oberflächliche Bezüge herstellen: aus vorigem Grund ergibt sich auch warum man ggf. oberflächliche Verbindungen genauer analysieren sollte und sich ab und an einen Graph seiner Notizen ansehen sollte um ungewöhnliche Verbindungen zu erkennen und ein tieferes Verständnis für Zusammenhänge zu entwickeln
- 

## Schreiben um zu verstehen

Erklärung des Konzeptes aus dem Buch "Writing to learn" von William Zinsser. Weiterer Verweis auf "rubber ducking", also ein besseres Verständnis für ein Thema das man selber erklären oder sogar unterrichten kann

## Gefahren

Eine Gefahr ist dass man anfängt alles, auch Unwichtiges zu notieren und zu protokollieren. Gerade in der digitalen Welt und mit Dingen wie einem Web Clipper neigt man schnell zum hoarding.

## Mögliche Tools

kurzes Beispiel dass Word in 2007 meine B.Sc. gefressen hat, ich damals noch Word 2003 benutzt habe mit .doc Format und die Unsicherheit ob dieses Format noch lange unterstützt wird und wie stabil diese Dateien eigentlich sind wenn die Arbeit damals von der “richtigen” Version schon gefressen wurde

## Warum Joplin?

- open source
- sicher: Joplin supports end-to-end encryption 
- einfache plain text Notizen bzw Markdown und komfortable Live Ansicht des fertigen Artikels
- complete metadata (geolocation, updated time, created time, etc.) und Tags/Keywords
- Import von Evernote und anderen Systemen möglich
- Export in verschiedenste Formate, unter anderem auch einfacher Export ganzer Notizbücher als PDF,Markdown, HTML o.ä.
- Integration von Plugins (insbesondere Webclipper, BibTex, Draw.io, eMail Plugin, Anki Sync)
- Synchronisierung über verschiedene Plattformen mittels Nextcloud/WebDav, Dropbox, Onedrive, GoogleDrive oder einer eigenen Joplin Cloud oder wenn man sich die Mühe nicht machen will geht auch einfach ein S3 Bucket als Ziel
- offline Arbeit ist möglich (keine Web App), Synch erfolgt sobald wieder Internet verfügbar ist
- Kompatibel mit verschiedenen Systemen wie Windows, macOS, Linux, Android, and iOS
- Standalone App ohne Installer (-> einfacher Betrieb auf fremden oder Arbeitsrechnern)
- einfache Integration externer Editoren wie Typora
- Anhängen sämtlicher Dateitypen und direktes Einbinden von gängigen Dateien in den Fließtext (Grafiken, Diagramme, Animationen, Videos usw.)

Nachteile von Joplin

Der größte Nachteil ist dass Joplin nicht, wie einige andere Notizprogramme, eine einfache flat file Architektur verfolgt sondern ein relativ komplexes Backend inklusive einer SQLite Datenbank verwendet. Ob einen das stört muss man selber entscheiden, für mich haben sich bisher noch keine Nachteile ergeben und man kann auch alle Notizen simpel exportieren.

![Entnommen aus: https://joplinapp.org/help/dev/spec/architecture](assets/Application.png)



## Wie bekomme ich meine Notizen nach Joplin?

- Direkteingabe über die Applikation
- Webclipper -> Clipping von Artikeln
- Als Audio file. Ich benutze dafür den Google Recorder (nur für Pixel Handies?) der eine Transkribierung in Text erlaubt. Danach kann man dann den transkribierten Text oder die Audiodatei mit Joplin über die share Funktionalität teilen und dort eine Notiz anlegen und diese dann ggf. weiter in Joplin bearbeiten



