---
layout: post
title: Shorts - Windows für mehrere RDP Sessions konfigurieren
description: Wie man Windows für mehrere parallele RDP Sessions konfigurieren kann
summary: in der shorts Reihe werden wir uns kurz und knackig Lösungen für häufig auftretende SysAdmin Probleme anschauen. Diesmal geht es darum Windows für mehrere parallele RDP Sessions einzurichten.  
tags: coding shorts Windows
date: 2021-09-12 00:00:00
---

# Problemstellung

Seit *Windows Server 2012* ist es nicht mehr möglich mehrere RDP Sessions zu dem gleichen Server herzustellen. Man kann dieses Feature durch eine Anpassung der Gruppenrichtlinien jedoch wieder aktivieren. Wie das geht wird im folgenden beschrieben:

## Howto: Mehrere RDP Sessions erlauben

- Auf dem Server einloggen auf dem Remote Desktop Services installiert sind
- den Gruppenrichtlinieneditor **gpedit.msc** öffnen (Windows-Start -> gpedit.msc in Suchfeld eingeben)
- Computerkonfiguration –> Administrative Vorlagen –> Windows-Komponenten –> Remotedesktopdienste –> Remotedesktopsitzungs-Host –> Verbindungen 
- die Richtlinie „Remotedesktopdienste-Benutzer auf eine Remotedesktopdienste-Sitzung beschränken„ **deaktivieren** 
- Danach kann in der Richtlinie „Anzahl der Verbindungen einschränken“ die maximale Anzahl von RDP-Sessions konfiguriert werden.[^Sessions]


## Fußnoten

[^Sessions]: Ich habe hier mehrfach gehört dass es egal ist wieviele Sessions man hier einträgt, die maximale Anzahl Sessions ist >>2<<. Das habe ich aber bisher noch nicht nachgeprüft.

