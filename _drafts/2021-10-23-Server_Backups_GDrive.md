---
layout: post
title: Backups erstellen und in Google Drive und Onedrive schieben
description: Hier schauen wir uns an wie wir uns ein kleines Skript basteln können um unsere Backups von Server oder PC nach Google Drive oder Onedrive zu schieben
summary: Hier schauen wir uns an wie wir uns ein kleines Skript basteln können um unsere Backups von Server oder PC nach Google Drive oder Onedrive zu schieben
tags: Server Administration Backup Cloud Google Drive Onedrive
date: 2021-10-23 00:00:00
---
# Einleitung

Nachdem vor ein paar Tagen eine [neue, alte SSH Lücke aufgetaucht ist](https://www.heise.de/news/RegreSSHion-Sicherheitsluecke-in-OpenSSH-gibt-geduldigen-Angreifern-Root-Rechte-9784976.html) habe ich mal wieder sämtliche meiner eigenen guten Ratschläge ignoriert und *mal eben* in der Mittagspause ein `apt update` auf dem Server durchgeführt. Natürlich habe ich vorher keinen Snapshot oder Backup gemacht und mir dabei meine Apache2 Installation kaputt gemacht.

Zwei Tage später läuft der Webserver nun über Nginx, ich bin wieder etwas weiser und mir ist die Idee gekommen hier einmal etwas über Backups zu schreiben.

# Die goldene 3-2-1 Regel

Die Klassiker sind oft aus gutem Grund Klassiker und einer dieser Klassiker ist die goldene 3-2-1 Regel für Backups. Damit ist gemeint dass man mindestens **drei Kopien** seiner Daten hat: die Originale und zwei Backups, auf **zwei verschiedenen Speichermedien** und wenigstens **eine Kopie off-site.**

Das ist aus vielen Gründen sinnvoll und zu der Regel gibt es auch unzählige Artikel da draußen. 

# Der Schlachtplan

Meine Umsetzung sieht nun so aus dass ich, da ich meinen Server hauptsächlich privat nutze, gerne eine einfache Backup Möglichkeit ohne viel Schnick-Schnack wollte die meine vorhandenen Ressourcen vernünftig ausnutzt. 

Tape Backups fallen also bei mir weg.

Ich habe aber (AWS kompatiblen) Object Storage bei meinem Hoster, ein Google Drive bei Google und ein Onedrive bei Microsoft. 

Mein Plan sieht nun so aus mir ein kleines Skript zu schreiben das meine Backups entgegen nimmt, verschlüsselt und regelmäßig auf den Cloud Speicher des jeweiligen Anbieters schiebt. 

Dafür benötigen wir teils Tools der jeweiligen Anbieter die uns erst ermöglichen uns gegen die jeweiligen Dienste zu verbinden. Dazu später mehr.

# Die Umsetzung

blablabla

## Contabo Object Storage

## Google Drive

## Onedrive

