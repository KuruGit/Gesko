---
layout: post
title: Linux Basics Teil 2 - Distributionen
description: Teil 2 der Artikelreihe über Linux 
summary: In Teil 2 der Artikelreihe widmen wir uns nach der Diskussion über den Kernel und die coreutils den höheren Systemschichten die schlußendlich in einer Distribution als Gesamt-OS münden.  
tags: coding Linux
date: 2021-08-22 00:00:00
---

# Rekapitulation Teil 1

Im letzten Artikel haben wir kurz besprochen dass der Kernel das eigentliche Linux Basissystem bildet, welches die Kommunikation zwischen Software und Hardware bereitstellt.

Darauf setzen dann die Basis- oder auch coreutils auf. Die meisten Systeme benutzen hierfür die vom GNU Projekt erstellten open source Varianten der tools die bereits aus dem proprietären UNIX System bekannt waren. Das daraus entstehende System wird (laut Richard Stallmann) GNU/Linux genannt, auch wenn es hier keine Einigkeit gibt und wir der Einfachheit halber in Zukunft nur von "Linux" sprechen werden. 

Die Unterscheidung ist dennoch wichtig da es auch Linux Systeme gibt die komplett ohne Beteiligung des GNU Projekts auskommen. Dazu gehören bspw. Alpine Linux oder diverse BSD Varietäten, wodurch nicht immer sichergestellt ist dass jedes tool auf jedem System gleich funktioniert. Man muss also ggf. seine Scripte regelmäßig prüfen und vor dem Rollout auf einem neuen System die Kompatibilität checken.

# Was ist eine Linux Distribution?

