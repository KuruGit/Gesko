---
layout: post
title: Linux Terminal und file streams
description: Erklärungen zu nicht immer ganz intuitiven Linuxthemen
summary: In diesem Artikel werden wir uns einmal mit den Standard-Datenstreams in der Linux Kommandozeile befassen
tags: Linux file stream descriptors stdin stdout stderr
date: 2022-09-14 00:00:00
---
## Was sind file streams/descriptors

Wenn man unter Linux ein Programm ausführt sind per default Verhalten immer drei file streams aktiv (manchmal auch *descriptors* genannt):

| **Name**        | **symbolischer Name** | **Wert** | **Beispiel** |
|:---------------:|:---------------------:|:--------:|:------------:|
| standard input  | **stdin**             | 0        | Tastatur     |
| standard output | **stdout**            | 1        | Terminal     |
| standard error  | **stderr**            | 2        | log Datei    |

Normalerweise sind diese file streams dabei so belegt dass die **Standardeingabe** von der Tastatur kommt während die **Standardausgabe** und die **Fehlerausgabe** an das angeschlossene Ausgabegerät, also den Bildschirm gesendet wird. Wie man diese Ausgaben umleiten kann erläutere ich gleich ebenfalls noch.

## I/O Umleitungen und praktische Anwendung

Da ich selber dieses Konzept am Anfang auch schwer verständlich fand schauen wir uns ein paar Beispiele an:

Nehmen wir an wir haben ein Programm, dann können wir im Linux Terminal diesem Programm durch Verwendung des kleiner als ("<") eine beliebige Datei als input zuweisen, es bietet sich an das < als einen Richtungspfeil zu interpretieren:

```bash
/$ Programm < Input_Datei
```

Dadurch wird das Programm aufgerufen und gleichzeitig die Input_Datei als Input an das Programm gesendet.
Möchte man umgekehrt den output eines Programms an eine Datei schicken würde man die Richtung des "Pfeils" ändern, also ein größer-als Zeichen verwenden:

```bash
/$ Programm > Output_Datei
```

Hier würde analog der Output des Programms an "Output_Datei" weitergeleitet.

Wichtig ist an dieser Stelle dass < und > beides prinzipiell short-hands darstellen und man für den Output rein formal auch schreiben könnte:

```bash
/$ Programm 1> Output_Datei
```

Dies ist wichtig zu verstehen da unser Beispielprogramm gegebenenfalls auch noch Fehlercodes erzeugt, diese würden in unserem Beispiel weiterhin an die Standardausgabe gehen, also vermutlich auf dem Monitor ausgegeben werden.
Um die Errorausgabe in eine Datei auszugeben würde man analog zum obigen Beispiel folgendes machen:

```bash
/$ Programm 2> Error_Datei
```

In diesem Beispiel wird der Output des Programms an die Standardausgabe geschickt während etwaige Fehler in Error_Datei landen.

Möchte man sowohl Output als auch Fehlermeldungen in die gleiche Datei umleiten gibt es auch hierfür eine Kurzfassung:

```bash
/$ Programm > Output_Datei 2>&1
```

Der Zusatz 2>&1 sagt dabei einzig und alleine aus dass Fehlermeldungen ("2>")an die gleiche Stelle ausgeleitet werden sollen wie der standard output (&1), in diesem Fall würden also Output *und* Fehlermeldungen beide in Output_Datei gespeichert werden.

Unter Bash geht das Ganze sogar noch kürzer indem man die descriptors einfach auslässt da diese impliziert sind:

```bash
/$ Programm >& Output_Datei
```

## Ausblick

Nachdem wir nun gelernt haben wir man den Output eines Programms in eine Datei umleitet gibt es unter Linux auch die Möglichkeit den Output eines Programms direkt in ein anderes Programm umzuleiten. Dazu wird die Pipe "|" verwendet und mit diesem Thema beschäftigen wir uns im nächsten Artikel.