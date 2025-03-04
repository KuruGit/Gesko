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

## Umleitung mittels der Pipe in ein anderes Programm
Nachdem wir nun gelernt haben wie man den Output eines Programms in eine Datei umleitet gibt es unter Linux auch die Möglichkeit den Output eines Programms direkt in ein anderes Programm umzuleiten, also den stdout eines Programms als stdin in ein weiteres Programm zu leiten - oder anders ausgedrückt: zu ***pipen***! 

Verwendet wird dafür die so genannte Pipe, also das Symbol was auf einer deutschen Tastatur erscheint wenn man <kbd><kbd>AltGr</kbd>+<kbd><</kbd></kbd> drückt. Das Symbol für die Pipe ist ein vertikaler Strich: <code>|</code>.

Programme die über eine Pipe einen Datenstrom empfangen werden auch ***Filter*** genannt.

**Achtung!** Wichtig zu beachten ist dass Pipes **unidirektional** sind! Das heisst Daten werden immer <code>von | links | nach | rechts</code> durch die Pipes geleitet. 

### Syntax

Die Syntax ist relativ einfach:

```bash
command_1 | command_2 | command_3 | .... | command_N
```

man führt also ***command_1*** aus wie man es immer ausführen würde, also bspw. inklusive aller Parameter. Der ***stdout*** von ***command_1*** wird dann in ***command_2*** als ***stdin*** gepiped und so weiter.

### Beispiele

Kommen wir nun zu ein paar anschaulichen, aber auch nützlichen Beispielen dafür wie man die Pipe nutzen kann:

Nehmen wir an wir möchten eine sortierte Liste mit allen Benutzern des aktuellen Systems. Das kann man relativ leicht wie folgt durchführen:

```bash
cat /etc/passwd | sort | awk -F: '{print $1}'
```

Zur Erklärung: Als erstes wollen wir die Einträge aller User des Systems aus /etc/passwd mittels ***cat*** auslisten. Da diese Liste allerdings nicht sortiert ist, pipen wir sie in das ***sort*** Programm, dort werden die Zeilen dann alphabetisch sortiert. Als letztes wird es (nur ein bisschen) komplizierter weil wir nun unsere sortierte Liste mit Usern noch bereinigen möchten. Wir benötigen nämlich eigentlich nur die Liste der Namen, der Rest ist uns erstmal egal. In diesem Beispiel verwende ich dafür das Programm ***awk*** das wir an anderer Stelle auch nochmal besprechen werden. 

Das Ergebnis obiger Operation könnte bspw. so aussehen:

![Piping Beispiel 1](/assets/pictures/PipingExample1.png)

Man könnte nun noch weiter gehen. Möchtest du wissen wieviele Benutzer das System hat? Einfach das Ergebnis obiger Operation in das Programm wordcount mit dem -l flag pipen:

```bash
cat /etc/passwd | sort | awk -F: '{print $1}' | wc -l 
```

Das Beste an der ganzen Sache ist dass nur der Datenstrom zwischen den Programmen angepasst wird, die ursprüngliche passwd Datei wird **nicht** verändert!

Gerne wird die Pipe auch mit grep verwendet um das Ergebnis einer vorigen Operation nochmal mit irgendwelchen regular expression Regeln zu filtern.

Bleiben wir bei unserem passwd Beispiel könnten wir beispielsweise einfach nach einem bestimmten User filtern, also beispielsweise:

```Bash
cat /etc/passwd | grep TestUser
```

Dies würde nur die Zeile der passwd auswerfen die den String "TestUser" enthält. 

Ein anderes gern benutztes Programm in der Pipe ist ***uniq*** über das sich doppelte Einträge etc. ausfiltern lassen. Der Fantasie sind hier keine Grenzen gesetzt und es ist eine gute Idee sich mit dem Thema zu beschäftigen. Leider ist die Pipe dabei nur so mächtig wie das eigene Verständnis von den Tools die auf dem System verfügbar sind, aber wenn man das Prinzip einmal verstanden hat kann man sich richtig mächtige, automatisierte Workflows zusammen bauen die einem viel Arbeit abnehmen.
