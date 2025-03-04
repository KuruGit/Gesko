---
layout: post
title: Shorts - PIPESTATUS
description: In diesem Short werde ich kurz das Tool tldr vorstellen.
summary: In diesem Short werde ich kurz das Tool tldr vorstellen. Tldr gibt für einen gegebenen Befehl eine kurze, prägnante Beschreibung zusammen mit den meist verwendeten Befehlsparametern und Beispielaufrufen an. Dadurch muss man sich in der täglichen Arbeit nicht erst durch die manpages arbeiten.
tags: shorts tools cli
date: 2024-05-31 00:00:00

---

# Shorts - PIPESTATUS um die Fehlercodes verketteter Befehle auszugeben

## `$?` gibt nur den Exit Code des letzten Befehls aus

Es kommt öfter vor dass man den Exit Status eines Befehls oder Scripts abrufen möchte. Dazu kann man bspw. die `$?` Variable verwenden wie in folgendem Beispiel:

```sh
#!/bin/bash
grep unbekannt.meineDomain.com /etc/hosts
if [ "$?" -ne "0" ]; then
    echo "Ich kann keinen Eintrag in der hosts Datei finden!"
  exit 2
fi
```

Die `$?` Variable gibt den Exit Code des letzten ausgeführten Befehls aus, in unserem Beispiel oben wird also in der if-Bedingung geprüft ob der Exit Code des vorigen Befehls ungleich (-ne) "0" ist ("0" bedeutet "success", alles andere "fail"). Soweit, so gut.

Was macht man aber wenn man bspw. eine Pipe hat, also mehrere Befehle aneinander kettet? Hier kann einem die PIPESTATUS Variable dienlich sein.

Nehmen wir unser voriges Beispiel, aber pipen wir die Ausgabe von `grep` in `tee`:

```sh
#!/bin/bash
grep unbekannt.meineDomain.com /etc/hosts 2>&1 | tee /tmp/grep_results.txt
if [ "$?" -ne "0" ]; then
    echo "Ich kann keinen Eintrag in der hosts Datei finden!"
  exit 2
fi
```

In diesem Fall würde `$?` den Exit Code von `tee` und nicht von `grep` ausgeben.

Visuell lässt sich das bspw. auch so darstellen, auch wenn das Beispiel konstruiert ist:

```sh
❯ true | false | true | false | true
❯ echo $?
0
```

Die Befehle `true` und `false` machen genau das was drauf steht und geben "true" (also Exit Code 0) oder "false" (also einen Exit Code ungleich 0) aus. Wie man sieht gibt `$?` nur den Exit Code des letzten Befehls wieder, egal ob in unserer Pipe irgendwo etwas schiefgelaufen ist.

## PIPESTATUS speichert als Array Variable alle zuletzt gelaufenen Befehle

Wie in der Überschrift steht handelt es sich bei PIPESTATUS um eine Array Variable welche die Exit Codes unserer zuletzt ausgeführten Befehle:

```sh 
❯ true | false | true | false | true
❯ echo $pipestatus
0 1 0 1 0
```

> **Achtung**: Ich benutze hier die Z-Shell, deshalb nennt sich die Variable hier `$pipestatus` in Bash würde die Variable groß geschrieben werden und `$PIPESTATUS`heißen!

Die crux an der Sache ist leider, dass `$PIPESTATUS` nicht persistent ist und in unserem Beispiel direkt durch den Exit Code vom `echo` Befehl überschrieben wird. Hier muss man ggf. mit einer Hilfsvariable arbeiten und das Array zwischenspeichern wenn man die Ergebnisse später analysieren möchte.



