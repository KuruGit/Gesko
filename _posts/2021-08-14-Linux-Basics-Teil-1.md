---
layout: post
title: Linux Basics Teil 1 - Einführung
description: Dies ist der erste Artikel in einer Artikelreihe über Linuxsysteme
summary: Diese Artikelreihe soll als eine Art Knowledgebase dienen und alles rund um Linux bereitstellen, wir werden alles rund um den Aufbau von Linuxsystemen, Tools, Entwicklung bis hin zum Server hardening behandeln. Die Artikel sollen dabei ständig weiterentwickelt werden und unter anderem Cookbook Charakter haben indem sie bspw. im Fall von Linuxtools direkte Anwendungsgebiete aufzeigen. 
tags: coding Linux
date: 2021-08-14 00:00:00
---
# Was ist Linux überhaupt? Eine kurze Einleitung.

Sicherlich haben die meisten schon von Linux gehört, insbesondere Leute die (vermutlich gezielt) nach einem Blog wie meinem suchen. Deshalb möchte ich mich auch nicht mit ausschweifenden Erklärungen aufhalten. Dennoch gibt es ein paar (historische) Begriffe und Zusammenhänge die vielleicht ganz interessant zu wissen sind. Vielleicht sollte man diesen Abschnitt als eine Art sehr rudimentären Glossar betrachten.

## Linux

[Linux.com](https://www.linux.com/what-is-linux/) sagt zu der Frage was Linux ist folgendes:

> Just like Windows, iOS, and Mac OS, Linux is an operating system. In fact, one of the most popular platforms on the planet, Android, is powered by the Linux operating system. An operating system is software that manages all of the hardware resources associated with your desktop or laptop. To put it simply, the operating system manages the communication between your software and your hardware. Without the operating system (OS), the software wouldn?t function.

Das ist soweit auch richtig, aber doch in der Definition sehr breit gezogen. Spricht man von Linux im engeren Sinne meint man damit eigentlich immer den _Linux Kernel_. Dabei versteht man unter dem Linux Kernel schlicht die basale Softwareebene die quasi die Kommunikation zwischen der Hardware (sprich: Prozessor, Arbeitsspeicher, Festplatten, ...) sicherstellt und die Kommunikation mit dem "eigentlichen" Betriebssystem sicherstellt. Hier drängt sich natürlich der Verdacht auf dass auch andere Betriebssysteme etwas wie  einen Kernel verwenden und so ist es auch:  sowohl Microsoft Windows als auch MacOS (bzw. Darwin als Grundlage) verwenden ebenfalls einen Kernel. Da diese Systeme dem Nutzer aber weniger Mitspracherecht einräumen bekommt man von dieser Systemkomponente (im Guten wie im Schlechten) kaum etwas mit.

 ![Die Aufgabenbereiche des Linux Kernels](https://upload.wikimedia.org/wikipedia/commons/4/46/Linux_Kernel_Struktur.svg)_Die Aufgabenbereiche des Linux Kernels_[^Kernel]

## GNU/Linux, Unix, Posix und die coreutils

Im Zusammenhang mit Linux hört man immer wieder Begriffe wie Unix, GNU, coreutils und andere und das kann sehr verwirrend werden. Ich möchte die Begrifflichkeiten hier etwas entwirren.

Unix war ursprünglich ein von Bell Labs entwickeltes Betriebssystem das Ende der 1960er Jahre entwickelt wurde. Allgemein eines der Features war dass Firmen das Betriebssystem an ihre Bedürfnisse anpassen konnten. Dadurch entwickelten sich sehr viele unterschiedliche Ableger des ursprünglichen Systems. Der Begriff UNIX ist übrigens eine eingetragene Marke und darf nur von zertifizierten Produkten verwendet werden. Unix-artige Systeme werden deshalb in aller Regel als "Unix" bezeichnet.

Die Unix-artigen Systeme kann man dabei hauptsächlich in zwei Gruppen einteilen:

- Unix-derivate Systeme
- Unixoide Systeme
<br>
<br>

### Unix Derivate

Unter Unix Derivaten versteht man im wesentlichen Betriebssysteme die auf dem alten Unix Quellcode von Bell Labs aufbauen und entwickelt wurden. Dazu zählen bspw. das ursprüngliche BSD, HP-UX, AIX, Solaris und auch MacOS.

### Unixoide Systeme

Im Gegensatz dazu stehen die Unixoiden Systeme die zwar mit Unix (mehr oder weniger) kompatibel sind und den zugrundeliegenden POSIX Standard erfüllen, aber ohne den ursprünglichen Quellcode von Bell Labs entwickelt wurden. Hierzu gehören beispielsweise das (moderne) BSD, aber auch zum Beispiel Linux.

### POSIX und die Kompatibilität

Wie zuvor angedeutet gibt es einen Standard der die Funktionalitäten eines Unix Systems regelt um eine Portabilität und Kompatibilität zwischen den diversen Unix Derivaten zu gewährleisten. Das Reglement des Standards findet man beispielsweise hier: [POSIX](https://pubs.opengroup.org/onlinepubs/9699919799.2018edition/).

### GNU und die coreutils

GNU (steht für _"GNU is not UNIX"_) wurde ursprünglich als Initiative gegründet um ein Unix-artiges System mit kompatiblen Tools zu entwickeln welches eine komplett freie Alternative zum Produkt der Bell Labs darstellen sollte. Zu diesem Zweck wurden beispielsweise die ganzen Basisprogramme von Unix für das GNU Projekt neu geschrieben (_"coreutils"_).

Dabei wurde aber bewusst an einigen Stellen der POSIX Standard verletzt und es wurden Programme teils um diverse Funktionen erweitert die so in anderen Unix Systemen nicht vorkommen!

Leider handelt es sich dabei sehr häufig um tatsächlich genutzte Funktionalitäten. Ein Beispiel wäre folgendes

```Bash
sed -i 's/x/u/' meineDatei
```
In diesem Beispiel durchsucht das Programm "sed" die Datei "meineDatei" und ersetzt jedes "x" durch ein "u". Sed ist eigentlich aber ein Stream-Editor und verändert deshalb nur den output ohne die ursprüngliche Datei zu verändern. Dafür ist das -i flag da, das sed signalisiert dass es die ursprüngliche Datei überschreiben soll.

_Dieses flag ist __nicht__ POSIX konform!_

Das bedeutet dass das -i flag so nur unter den GNU coreutils verfügbar ist. Damit sind beispielsweise entsprechend verfasste Skripte nicht frei auf andere Unix-Systeme übertragbar und es kann durchaus vorkommen dass man auf seinem Ubuntu System ein Skript schreibt und dieses dann auf dem Mac vom Chef nicht lauffähig ist.

Leider ist mir nicht bekannt dass es irgendwo eine ausführliche Liste dieser inkompatiblen flags geben würde. Man ist hier also leider ein wenig der Göttin Fortuna ausgeliefert.

### Alternativen

Ich möchte allerdings nicht verschweigen dass es auch POSIX kompatible Alternativen gibt, beispielsweise [Plan 9 from User Space](https://9fans.github.io/plan9port/).

Diese Alternativen zu den GNU coreutils bringen allerdings ganz eigene Probleme mit sich, unter anderem den reduzierten (POSIX entsprechenden) Funktionsumfang und teilweise auch die Größe der Projekte und die dadurch entstehenden Risiken was die Zukunft, Bugfixing etc. eines Projekts angeht.

Es gibt auch komplette, GNU freie, Linux Distributionen wie beispielsweise [Alpine](https://www.alpinelinux.org/)
<br>
<hr>
<br>

## Fußnoten
[^Kernel]: Das Bild wurde aus dem umfangreichen [Artikel auf Wikipedia](https://de.wikipedia.org/wiki/Linux) übernommen
