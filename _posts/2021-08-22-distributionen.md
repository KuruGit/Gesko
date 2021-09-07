---
layout: post
title: Linux Basics Teil 2 - Distributionen
description: Teil 2 der Artikelreihe über Linux 
summary: In Teil 2 der Artikelreihe widmen wir uns nach der Diskussion über den Kernel und die coreutils den höheren Systemschichten die schlußendlich in einer Distribution als Gesamt-OS münden.  
tags: coding Linux
date: 2021-08-22 00:00:00
---

# Rekapitulation Teil 1

Im [letzten Artikel]({% post_url 2021-08-14-Linux-Basics-Teil-1 %}) haben wir kurz besprochen dass der Kernel das eigentliche Linux Basissystem darstellt, welches die Schnittstelle zwischen Software und Hardware bereitstellt.

Darauf setzen dann die Basis- oder auch coreutils auf. Die meisten Systeme benutzen hierfür die vom GNU Projekt erstellten open source Varianten der tools, die bereits aus dem proprietären UNIX System bekannt waren. Das daraus entstehende System wird (laut Richard Stallmann) GNU/Linux genannt, auch wenn es hier keine Einigkeit gibt und wir der Einfachheit halber in Zukunft nur von "Linux" sprechen werden. 

Die Unterscheidung ist dennoch wichtig da es auch Linux Systeme gibt die komplett ohne Beteiligung des GNU Projekts auskommen. Dazu gehören bspw. Alpine Linux oder diverse BSD Varietäten, wodurch nicht immer sichergestellt ist dass jedes tool auf jedem System gleich funktioniert. Man muss also ggf. seine Scripte regelmäßig prüfen und vor dem Rollout auf einem neuen System die Kompatibilität checken.

# Was ist eine Linux Distribution?

Eine Linux Distribution nimmt schließlich das GNU/Linux Basissystem und erweitert dieses um weitere Softwarepakete um ein komplettes Betriebssystem zu bauen. Hier wird es leider kompliziert da es eine ganze Reihe an Linux Distributionen gibt die sich teils in der zugrundeliegenden Philosophie, teils im Anwendungsbereich und der technischen Funktionalität unterscheiden.

So gibt es die klassischen Desktop-Distributionen die im Prinzip ein komplettes Betriebssystem für den Endanwender bereitstellen,inklusive grafischer Oberfläche und einem üppigen Softwarepaket.

Daneben gibt es reduzierte Varianten die beispielsweise im Serverbereich auf die grafische Oberfläche verzichten oder sogar so weit reduziert sind dass sie nur die notwendige Funktionalität bieten um bspw. einen Kleinstcomputer oder eine Waschmaschine zu steuern.

Daneben gibt es auch Varianten die für einen speziellen Zweck vorkonfiguriert wurden und bestimmte Softwarepakete direkt mitliefern, beispielsweise im Bereich Hacking und PenTesting.

Ebenso gibt es Distributionen die sich philosophisch unterscheiden. Beispielsweise gibt es diverse Distributionen die auf Software des GNU Projekts verzichten oder keine proprietäre Software einbinden für die der Quellcode nicht offen liegt (bspw. Grafikkarten- oder Druckertreiber).

Dementsprechend ist das Linux Umfeld riesengroß und es fällt einem Anfänger sicherlich schwer eine geeignete Distribution zu finden. Je nach Quelle gibt es mehrere hundert bis etwas über eintausend aktiv entwickelte Distributionen. Da fällt es schwer die richtige zu finden.

Um einen Einstieg zu finden wollen wir uns ein paar der beliebtesten Distributionen kurz anschauen und diskutieren.

## Enterprise Linux

Auch wenn Linux momentan noch ein Schattendasein auf dem Desktop fristet ist es ein ernstzunehmendes Serverbetriebssystem und hat dort, nach Microsoft Windows, den zweithöchsten Marktanteil mit >13% (siehe [Statista](https://www.statista.com/statistics/915085/global-server-share-by-os/)). Im wesentlichen gibt es hier nur zwei wirkliche Konkurrenten: [Suse Linux Enterprise (SLES)](https://www.suse.com/de-de/) und [Red Hat Linux](https://www.redhat.com/de/). Beide führe ich hier nur der Vollständigkeit auf, da entsprechende Lizenzen für diese Betriebssystem auch wirklich "Enterprise" sind und mehrere tausend Euro betragen können. 

Trotzdem ist es für den geneigten Nutzer vielleicht ganz spannend sich mit beiden auseinander zu setzen, da sowohl Suse als auch Redhat eigene Fortbildungen und Zertifizierungen anbieten. Für Suse wäre hier ein guter Startpunkt der [Suse Certified Administrator (SCA)](https://training.suse.com/certification/sca-sles-15/) und für Red Hat der [Red Hat Certified Systems Administrator (RHCSA)](https://www.redhat.com/de/services/certification/rhcsa).

Als Alternative lässt sich sicherlich noch die Distributions-agnostische Zertifizierung LPIC-1(und höhere) des Linux Professional Institute. Der Vorteil ist dass diese Prüfung sich nicht auf ein spezielles Linuxsystem stützt sondern allgemein gehalten ist.

Preislich liegt die LPIC-1 Zertifizierung mit 160€/Prüfung auf dem gleichen Niveau wie die Suse Zertifizierung. Red Hat schert hier ziemlich aus und ruft mit 515€ schon eine ganze Stange mehr Geld auf.

Welche Zertifizierung man tatsächlich bevorzugt ist im wesentlichen Geschmackssache, inhaltlich ähneln diese sich doch relativ stark und es gab auch einmal die Möglichkeit sich von Suse ein LPIC-1 Zertifikat ausstellen zu lassen.

## Dekstop Linux

### Debian/Ubuntu Derivate

Debian ist eine sehr lang gereifte (Bj. 1993) Linux Distribution deren Markenzeichen es ist nur absolut sichere und getestete Software auszuliefern. Dadurch ist Debian immer etwas hinter der Zeit, erkauft sich damit aber eine sehr hohe Systemstabilität. Debian ist aus diesem Grund insbesondere im Serverumfeld sehr beliebt und eignet sich für Desktop User, die nicht nur 10 Jahre alte Software nutzen möchten, eher weniger.

#### Ubuntu

Hier kommt Ubuntu ins Spiel das als Abkömmling von Debian konzipiert wurde mit dem Ziel Debian für den Normaluser nutzbar zu machen. Mittlerweile ist Ubuntu ebenfalls eine sehr ausgereifte Distribution (Bj. 2004) und veröffentlicht in einem halbjährlichen Rhythmus eine neue Version, sowie alle zwei Jahre ein long term release mit erweitertem Support. Ebenfalls dringt Ubuntu langsam aber sicher in den Enterprise Raum vor und nimmt Suse und Red Hat anscheinend Anteile ab. Allerdings ist das nur meine persönliche Erfahrung, mir sind hierzu keine aussagekräftigen Statistiken bekannt.

**Eigenschaften von Ubuntu**

Ubuntu basiert wie eingangs erwähnt auf Debian und benutzt deshalb auch den Debian Package Manager der es sehr einfach macht Software auf dem System zu installieren. Da Ubuntu kein reines Community Projekt ist, sondern von der Firma Canonical weiterentwickelt wird, steht hier auch ordentlich Manpower hinter dem Projekt. Software sowie Betriebssystem selber werden umfassend getestet und auf qualitativ hohem Niveau weiterentwickelt. Die Dokumentation ist exzellent und man findet quasi zu jeder Frage die man hat eine Antwort. Im Gegensatz zu vielen anderen Distributionen gibt es für Ubuntu ebenfalls eine große deutschsprachige Community falls man sich als Anfänger nicht direkt den englischen Fachjargon einiger Foren zutraut.
Ubuntu ist eine GNU/Linux Distribution und schwimmt damit im Mainstream mit, was gerade Anfängern entgegen kommen sollte. Als Standard-Shell kommt [Bash](https://www.gnu.org/software/bash/) zum Einsatz, eine weitverbreitete und beliebte Shell. 
Für den [Window Manager](https://de.wikipedia.org/wiki/Fenstermanager) (also die grafische Benutzeroberfläche) greift Ubuntu in seiner Standard Version auf Gnome zurück, einen soliden und weitverbreiteten Window Manager der eine gute Balance zwischen Ressourcenverbrauch, Usability und Eye Candy findet. Gnome sollte auf allen heute noch gebräuchlichen Computern und Laptops einwandfrei laufen. Solltest Du planen Ubuntu auf einem Kleinstrechner wie einem [Raspberry PI](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) zu installieren bietet Ubuntu allerdings auch andere [Flavours](https://ubuntu.com/download/flavours) an.

**Fazit**

Ubuntu ist ein Allrounder. Dabei macht Ubuntu nichts schlecht, geht aber auch keine Risiken ein. Dadurch dass das System von einer Firma weiterentwickelt wird und auch im Enterprise Umfeld aktiv ist kann man davon ausgehen dass Ubuntu nicht über Nacht vom Markt verschwindet, was gerade Anfängern Sicherheit geben sollte.
Der Großteil der verwendeten Software entspricht dem Standard und die Dokumentation ist hervorragend und die Community riesig. Gerade Anfängern sollte das entgegen kommen.

#### openSUSE
Suse führe ich hier aus drei Gründen auf. Zum einen, um das einmal vorweg zu nehmen, stand das S.u.S.E. in SuSE Linux ursprünglich als Akronym für **S**oftware- **u**nd **S**ystem-**E**ntwicklung. Das ist deshalb so weil SUSE ursprünglich aus Deutschland kommt und 1992 in Nürnberg das Licht der Welt erblickte. 

**Eigenschaften von SUSE** 

Neben diesem kleinen Ausflug in den Lokalpatriotismus bietet SUSE, bzw. sein freier Ableger openSUSE auch einige andere interessante Eigenschaften.

Zum einen ist SUSE quasi eine Eigenentwicklung die, im Gegensatz zu vielen anderen Distributionen, eben *nicht* auf einer der größeren Distributionen wie Debian/Ubuntu, Arch und wie sie alle heißen basiert. 
Das kann man als Vor- oder Nachteil sehen, hebt SUSE aber mit einer Reihe an Software Eigenentwicklungen aus der Masse heraus. 
So benutzt SUSE einen eigenen [Package Manager](https://de.opensuse.org/Zypper), ein eigenes [Snapshot-](https://de.opensuse.org/openSUSE:Snapper_Tutorial) und bringt, jedenfalls in der Enterprise Version, auch professionelle Tools für Aufgaben wie [Hochverfügbarkeit](https://www.suse.com/de-de/products/highavailability/), [Storage Management](https://www.suse.com/de-de/products/longhorn/) oder [Cotainerverwaltung](https://www.suse.com/de-de/products/suse-rancher/) mit. Daneben ist SUSE Linux auch der Partner der Wahl für [SAP](https://www.suse.com/de-de/solutions/run-sap-solutions/).

Insgesamt stellt SUSE damit ein umfangreiches, stabiles System mit mehreren Jahrzehnten dar. Dies ist eine der Distributionen die nicht so schnell verschwinden werden.

Dabei wird SUSE in verschiedenen Varianten entwickelt. SLES, also **S**USE **L**INUX **E**NTERPRISE **S**ERVER, ist das kommerzielle Enterprise Angebot von SUSE. 

Daneben gibt es noch die "freie" Variante openSUSE die von einer starken und aktiven Community getrieben wird und in zwei Varianten daher kommt: einem rolling release[^rollingRelease] ([Tumbleweed](https://en.opensuse.org/Portal:Tumbleweed)) und einem statischen release ([Leap](https://www.opensuse.org/#Leap)). 

**Fazit**
SUSE verfügt über eine sehr aktive Community und hat eine große Firma die das Projekt mittlerweile über Jahrzehnte voran treibt. 
Neben Red Hat Linux ist SUSE die zweite Distribution die fest im Enterprise Bereich verankert ist, was es sicherlich für Menschen mit beruflichen Ambitionen im Linuxumfeld zusätzlich interessant macht. Aus dem größeren Backing ergeben sich unter anderem auch Neu- und Weiterentwicklungen die andere, kleinere Projekte vermutlich so nicht stemmen könnten, gerade das Snapshot und Backup Konzept von SUSE ist schon sehr ausgereift. 
Für Anfänger ist es außerdem sehr angenehm dass SUSE quasi alle administrativen Aufgaben die auf dem System anfallen in seiner YAST Software unter einer einheitlichen Oberfläche vereint.

#### Fedora


#### CentOS


#### Manjaro


#### ElementaryOS


### Fußnoten
[^rollingRelease]: Unter einem rolling release versteht man eine Distribution ohne feste Versionsnummern. Diese Distribution wird ständig und nach Bedarf weiterentwickelt und nicht nach einem festgelegten Zeitplan.