---
layout: post
title: Linux Basics - awk Grundlagen
description: Teil der Artikelreihe über Linux Basics
summary: In diesem Teil werden wir uns einmal ansehen was awk für in mächtiges tool sein kann
tags: coding Linux
date: 2023-02-01 00:00:00
---

# awk - ein mächtiges Linuxtool zur Verarbeitung von Textdaten
Das Programm ***awk*** wurde in den 1970er Jahren bei den Bell Labs entwickelt und ist also schon etwas älter. Der Name des Programms leitet sich dabei von den Namen der drei Entwickler **A**ho, **W**einberger und **K**ernighan ab. Das Programm selber wurde entwickelt um strukturierte Daten aus Dateien auszulesen und anderweitig auszugeben, beispielsweise zur Erstellung von Reports.
Dabei ist *awk* mittlerweile so komplex dass es rein formal eine vollwertige Programmiersprache darstellt, das keyword hier ist *formal* denn obwohl man prinzipiell mit awk Programme schreiben könnte ist das tool nicht dafür ausgelegt und awk Programme sind eher als Kuriosität zu betrachten.
Da awk mittlerweile ein paar Jahre auf dem Buckel hat, wird das ursprüngliche Programm heute eigentlich nicht mehr verwendet, sondern die meisten Linux Distributionen bringen Weiterentwicklungen von awk mit wie bspw. [gawk](https://www.gnu.org/software/gawk/,), [mawk](https://invisible-island.net/mawk/) oder [nawk](https://linux.die.net/man/1/nawk). Diese sind in der Regel alle miteinander kompatibel, erweitern aber das Grundprogramm um eine Reihe von Funktionen, short cuts und *quality-of-life* Verbesserungen.

## Warum sollte ich awk verwenden?
Egal wie man es dreht und wendet, awk ist schon ein wenig angestaubt aufgrund seines Alters. Trotzdem ist awk ein unglaublich starkes tool - sofern man sich innerhalb der Grenzen seines Einsatzbereiches bewegt. Sprachen wie *Perl* oder *Python* sind mit Sicherheit vielseitiger, aber wenn man "nur" Daten bspw. aus einer Log-Datei extrahieren möchte ist awk eines der besten tools die man nutzen kann da die Skripte relativ kurz sind und awk wesentlich schneller arbeitet als beispielsweise ein Python Skript. Bonuspunkte bekommt awk weil es in praktisch jeder Linux Basisinstallation zu finden ist und damit die Administration unbekannter Systeme erleichtert.
Es gibt also durchaus noch Anwendungsszenarien für awk.

## Arbeitsweise und Syntax von awk

`awk` arbeitet indem es Daten **zeilenweise** einliest und anhand eines vorgegebenen **Delimiters in Felder zerlegt** und diese dann ausgibt. Die Funktionsweise ist `sed` sehr ähnlich, aber erlaubt wesentlich komplexere Konstrukte.

Ein typischer awk Aufruf könnte beispielsweise so aussehen:

```awk
awk '/error/ { print $0 }' logfile.txt
```

Dabei erfolgt der Aufruf von awk mittels des Programmnamens `awk`, einem sogenannten Muster-Aktionen-Paar und (in aller Regel) einer Datei auf welche die Operationen angewendet werden sollen (`awk` kann aber natürlich auch mit Daten aus einem Eingabestrom zurechtkommen).

### Muster-Aktionen-Paare



## sed und awk arbeiten wunderbar zusammen

Man kann `awk` sehr gut mit `sed` kombinieren. Anbei ein paar Beispiele. Eine kurze `sed` Einführung gibt es im [Artikel zu sed]({% post_url 2023-01-20-Linux-Basics-sed %}).





 



