---
layout: post
title: Shorts - tldr man pages light
description: In diesem Short werde ich kurz das Tool tldr vorstellen.
summary: In diesem Short werde ich kurz das Tool tldr vorstellen. Tldr gibt für einen gegebenen Befehl eine kurze, prägnante Beschreibung zusammen mit den meist verwendeten Befehlsparametern und Beispielaufrufen an. Dadurch muss man sich in der täglichen Arbeit nicht erst durch die manpages arbeiten.
tags: shorts tools cli
date: 2024-05-31 00:00:00
---
# Shorts - tldr: man pages light

[tldr](https://tldr.sh/), benannt nach einem alten Internet-Akronym für **t**oo **l**ong; **d**idn't **r**ead, ist ein tool das getreu seinem Namen die teilweise unnötig langen man pages ergänzen soll. Im Prinzip handelt es sich dabei um ein online repository von in Markdown verfassten Kurzanleitungen (oft mit Beispielen) für diverse Kommandozeilen Tools (primär Linux, aber ebenfalls MacOS und Windows!). Es gibt sowohl lokal installierbare Clients als auch [eine online Version](https://tldr.inbrowser.app/) des Tools. Während ich diesen Artikel verfasse gibt es bereits über 16.000 Seiten die ständig von einer aktiven Community erweitert werden.

Zur Veranschaulichung hier ein Beispiel wie der Kommandozeilen-Client aussieht, gezeigt ist der Node.js Client:

![Aufruf von tldr für den tar Befehl](/assets/pictures/tldrBeispiel.png)

Der Aufruf erfolgt ganz einfach über 

`tldr <Befehl>`

Wie man sieht erhält man in der Regel eine Kurzbeschreibung was der Befehl eigentlich macht sowie eine Liste mit Aufrufparametern und ggf. Vorschläge für alternative Programme die ähnliche Funktionen erfüllen. 

Die Hilfeseiten selbst sind in Markdown geschrieben und werden von der Community erstellt und verwaltet. 

## Installation

Wie eingangs geschrieben kann man die tldr Seiten auch online über [diese Website](https://tldr.inbrowser.app/) abrufen, es gibt aber auch diverse Clients die man installieren kann. Der momentan laut Projektseite beste Client ist der Node.js Client. Dieser lässt sich mittels NPM ganz einfach wie folgt installieren:

`npm install -g tldr`

[Weitere Clients](https://github.com/tldr-pages/tldr/wiki/tldr-pages-clients) kann man im Wiki finden, es gibt bspw. auch einen Python Client.

## Update

Nach der Installation kann man über

`tldr update`

ein Update der lokal gespeicherten Pages anstoßen.

## Übersetzungen

Es gibt ebenfalls ein Projekt das versucht die tldr pages in möglichst viele Sprachen zu übersetzen um diese zugänglicher zu machen. Den Status des Projekts kann man [hier abrufen](https://lukwebsforge.github.io/tldri18n/).

## Unterstützung

Man kann das Projekt auch unterstützen. Dies bietet sich gerade für Anfänger an die noch nicht viel mit Git gearbeitet haben. Das Projekt ermuntert jeden dazu eigene Seiten im [Repository des Projekts](https://github.com/tldr-pages/tldr) anzulegen oder bestehende zu verbessern. Dazu einfach im `/pages` Verzeichnis des Repository anlegen oder bearbeiten und danach ein pull request anstoßen. Bitte beachtet aber die [Contributing Guidelines](https://github.com/tldr-pages/tldr/blob/main/CONTRIBUTING.md)!