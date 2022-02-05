---
layout: post
title: Shorts - Achtung beim Klonen von Virtualbox VMs
description: Dinge die man beim Klonen von VMs beachten sollte
summary: Manchmal stolpert man über kleine Fehler die einen am Ende stundenlange Arbeit kosten  
tags: coding shorts Windows
date: 2022-01-19 00:00:00
---

# Das Ausgangsproblem
Ich war dabei eine Testumgebung aufzubauen um mehrere VMs mit verschiedenen dockerisierten Applikationen laufen zu lassen.
Da ich faul bin habe ich mir gedacht dass ich eine VM aufsetze, Docker installiere und die Maschine dann klone. 
Normalerweise organisiere ich mir solche Setups mit mehreren VMs über ein Tool wie [Vagrant](https://www.vagrantup.com/). 
Diesmal habe ich aber einfach eine Maschine in Virtualbox aufgesetzt und diese dann geklont.

**BÖSER FEHLER!**

Denn obwohl ich daran gedacht habe beim klonen der VM anzuhakeln dass die neue Maschine eine neue Mac-Adresse bekommt, war das nicht ausreichend!

![Virtualbox klonen einer VM](/assets/pictures/Virtualbox_clone_dialogue.png)

Nach dem Klonen der VM konnte ich beide VMs hochfahren, musste aber feststellen dass diese sich im Netzwerk nicht sehen konnten.
Nach kurzer Suche dann die große Verwirrung:
*beide* Maschinen bekamen die gleiche IP zugewiesen!

## The plot thickens!

Nach 1,5 Stunden Sucherei und herumprobieren dann die Erkenntnis:

**zusammen mit der Maschine wurde auch die machine-id geklont**

Das war mir so nicht bewusst, obwohl es natürlich logisch ist. Die machine-id ist eine Identifikationsnummer die normalerweise während der Installation des Systems zufallsgeneriert wird und den Host eindeutig identifizieren soll.

Abrufen kann man die machine-id beispielsweise über:

```Bash
cat /etc/machine-id
```

Wer nochmal genauer nachlesen möchte kann eine deutsche Übersetzung der Manpage bspw. [bei Debian finden](https://manpages.debian.org/testing/manpages-de/machine-id.5.de.html).

## Die Lösung

Um mein Problem mit den doppelten IPs zu lösen musste ich also eine neue machine-id generieren.

Das kann man wie folgt machen:

```Bash
sudo rm -f /etc/machine-id
sudo dbus-uuidgen --ensure=/etc/machine-id
sudo rm /var/lib/dbus/machine-id
sudo dbus-uuidgen --ensure
reboot
```

Nach einem Reboot wurden dann endlich vom DHCP Server verschiedene IP-Adressen vergeben. 

In Zukunft werde ich aber doch wieder Vagrant verwenden...
 


