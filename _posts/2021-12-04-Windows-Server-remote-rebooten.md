---
layout: post
title: Shorts - Windows Server remote rebooten
description: Wie man Windows remote rebooten kann
summary: in der shorts Reihe werden wir uns kurz und knackig Lösungen für häufig auftretende SysAdmin Probleme anschauen. Diesmal geht es darum Windows per remote Befehl zu rebooten  
tags: coding shorts Windows
date: 2021-12-04 00:00:00
---

# der Shutdown Befehl

Neulich hatte ich ein Kundensystem das keine RDP Session mehr aufbauen wollte. In solchen Fällen ist es nützlich dass der shutdown Befehl auch übers Netzwerk ausgeführt werden kann:

```
shutdown /r /m \\{RemoteRechner} /t 60
```
dabei sind die einzelnen flags:

/r Restart

/m \\{IP/Hostname} Remote Rechner

/t time delay

Microsoft hat ebenfalls einen eigenen [Artikel](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/shutdown) zum shutdown Befehl und allen seinen flags.

# Windows Kommando Referenz

In diesem Zusammenhang ist sicher auch die [Windows Kommando Referenz](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands) sehr interessant. Hier findet man einen A bis Z Glossar aller Windows Server und Client Befehle für die Kommandozeile. Es lohnt sich hier mal durch zu scrollen!