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

Egal wie man es dreht und wendet, awk ist schon ein wenig angestaubt aufgrund seines Alters. Trotzdem ist awk ein unglaublich starkes tool - sofern man sich innerhalb der Grenzen seines Einsatzbereiches bewegt. Sprachen wie *Perl* oder *Python* sind mit Sicherheit vielseitiger, aber wenn man "nur" Daten bspw. aus einer Log-Datei extrahieren möchte ist awk eines der besten tools die man nutzen kann da die Skripte relativ kurz sind (oft nur 1-2 Zeilen lang) und awk wesentlich schneller arbeitet als beispielsweise ein Python Skript. Ebenfalls eignet sich awk laut meiner Erfahrung ganz wunderbar zur Formatierung von Output, beispielsweise wenn man das Ergebnis einer SQL Abfrage ein wenig umformatieren und aufhübschen möchte.

Ein Awk-Programm besteht aus einer Reihe von **Muster-Aktion Paaren**. Das bedeutet, dass awk eine oder mehrere
Dateien zeilenweise durchsucht und mit dem vorgegebenen Muster abgleicht. Wird eine passende Zeile gefunden,
so wird die zugewiesene Aktion durchgeführt. Muster können hier beispielsweise durch regular expressions, Vergleichsoperationen auf Zahlen, Zeichenketten, Feldern, Variablen usw. vorgegeben sein. 

Bonuspunkte bekommt awk weil es in praktisch jeder Linux Basisinstallation zu finden ist und damit die Administration von Systemen mit unbekannter Konfiguration erleichtert.

Es gibt also durchaus noch Anwendungsszenarien für awk.

## Warum sollte ich awk nicht verwenden?

Grundsätzlich muss man sagen dass die Vorteile von sed, awk und Programmiersprachen wie Python oder Perl mit ihren Nachteilen korrelieren und sich oft auf die Komplexität eindampfen lassen.

Sed eignet sich für einfache oneliner und wenn es darum geht einfache Textmanipulationen vorzunehmen.

Wird es komplexer und benötigt man beispielsweise einfache Kontrollstrukturen so würde ich zu awk wechseln. Dabei hat awk aber ebenfalls seine Schwachstellen, beispielsweise beherrscht awk keine Deklaration von Variablen und multi-file input/output gestaltet sich komplex. Awk eignet sich deshalb vor allem für einfache CLI Pipelines um einfach und schnell Text/Output zu formatieren. Für mich ist awk beispielsweise das Mittel der Wahl um eine Log Datei nach Fehlermeldungen zu durchsuchen und diese Fehlermeldungen strukturiert in eine eigene Datei zu schreiben oder auf der Kommandozeile auszugeben. 

Python und Perl sind richtige Programmiersprachen (auch wenn awk, rein formal, auch schon dazu zählt) mit denen man praktisch alles umsetzen kann was man möchte. Dies wird aber durch eine erhöhte Komplexität erkauft und man wird Schwierigkeiten haben ein Python Skript ähnlich kurz zu halten wie in awk oder sed. Python und Perl sind zudem Zusatzpakete die nicht ohne weiteres auf jedem System vorinstalliert sind und die ebenfalls einen vergleichsweise großen Fußabdruck haben und Ressourcen verbrauchen. Für einfache Arbeiten als Systemadmin würde man hier also mit Kanonen auf Spatzen schießen.

## Arbeitsweise und Syntax von awk

`awk` arbeitet indem es Daten **zeilenweise** einliest und anhand eines vorgegebenen **Delimiters [^1], in Felder zerlegt** und diese dann ausgibt. Die Funktionsweise ist `sed` sehr ähnlich, aber erlaubt wesentlich komplexere Konstrukte.

Ein typischer awk Aufruf könnte beispielsweise so aussehen:

```awk
awk '/error/ { print $0 }' logfile.txt
```

Dabei erfolgt der Aufruf von awk mittels des Programmnamens `awk`, einem sogenannten **Muster-Aktionen-Paar** und (in aller Regel) einer Datei auf welche die Operationen angewendet werden sollen (`awk` kann aber natürlich auch mit Daten aus einem Eingabestrom zurechtkommen).

### Muster-Aktionen-Paare



## sed und awk arbeiten wunderbar zusammen

Man kann `awk` sehr gut mit `sed` kombinieren. Anbei ein paar Beispiele. Eine kurze `sed` Einführung gibt es im [Artikel zu sed]({% post_url 2023-01-20-Linux-Basics-sed %}).

## Weitere Informationen

Dieser Artikel kann und soll nur einen groben Überblick geben. Umfangreichere Informationen und eine ganze Liste der vordefinierten Variablen, Parameter etc. findet man entweder lokal in den `man` und `info` pages oder aber [online](https://www.gnu.org/software/gawk/manual/).  

Als refresher: die man pages ruft man auf über `man awk` und die info pages über `info awk`.

Als einfache und gute Alternative kann ich hier allerdings noch das Tool `tldr` empfehlen, über das ebenfalls nochmal ein Artikel folgen wird.

## Fußnoten



[^1]: Ein Delimiter kann beispielsweise ein Leerzeichen, Tab, Doppelpunkt, Komma o.ä. sein und in awk frei bestimmt werden.

 



