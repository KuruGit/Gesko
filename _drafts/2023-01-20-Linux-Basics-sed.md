---
layout: post
title: Linux Basics - sed Grundlagen
description: Teil der Artikelreihe über Linux Basics
summary: In diesem Teil werden wir uns einmal ansehen was sed für ein mächtiges tool sein kann
tags: coding Linux
date: 2023-01-20 00:00:00
---

# sed - ein mächtiges Tool zur Verarbeitung von Text/Streaming-Daten

[TODO:Link auf anderen Artikel einfügen!] Nachdem wir uns zuletzt angesehen haben wie data streams funktionieren und wie man Daten von einem Programm ins nächste pipen kann, wollen wir uns heute eines der beliebteren Tools für diesen Zweck ansehen. 

Der Name sed steht für *stream editor* und mit dem tool lassen sich weitreichende Änderungen an Texten durchführen wie beispielsweise search, delete oder replace Operationen. 

Dabei kommt sed auch mit *regular expressions* (kurz: *RegEx*) klar und erlaubt so die Suche und den Austausch von Textstrings mittels pattern matching.

Sed funktioniert dabei so dass es Ausgangsdaten entweder aus einer Datei oder direkt aus einem output stream erhält, diese Daten in einem *working space* ablegt und nach Ausführung dann in einen output stream (meistens stdout) ausgibt. Dabei werden die Ausgangsdaten (also auch Ausgangsdateien) erstmal nicht verändert, es sei denn man forciert dies durch Nutzung des -i (für *in place*) Schalters.

## Syntax

Sed folgt den normalen Konventionen eines Programmaufrufs unter Linux:

```Bash
# Triviale Syntax, bei einzelnem command kann man dies einfach angeben, bei mehreren Operationen muss man den -e Schalter vorsetzen

sed [optional:flags] command <filename>

# In diesem Beispiel sucht und ersetzt sed im kompletten Text der Datei Beispieltext.txt das Wort "dieses" durch "jenes"

sed -e s/dieses/jenes/g Beispieltext.txt

# Man kann auch direkt Daten aus einem Datenstream ersetzen

echo "I hate you!" | sed s/hate/love/
```

Was passiert hier also?

Der *sed* Befehl ruft das Programm auf, im einfachsten Fall folgt dann eine durchzuführende Operation und der Dateiname. 
In unseren Beispielen oben führen wir dabei eine (s)ubstitution durch. Zwischen den Slashes "/" werden dabei der Suchbegriff und der Ersatzbegriff eingeschlossen. 

Beim ersten Beispiel folgt der (g)lobal Schalter der angibt dass der Ersatz global, also für den gesamten Text erfolgen soll. Ohne den global-Schalter würde nur das erste Auftreten von "dieses" ersetzt. 

Im zweiten Beispiel haben wir den (g)lobal-Schalter weggelassen da wir sowieso nur das eine Wort ersetzen wollen. 

Das zweite Beispiel illustriert dabei dass sed nicht unbedingt mit einer Datei gefüttert werden muss sondern auch direkt Daten bspw. aus einem anderen Befehl erhalten kann.

 



