---
layout: post
title: Linux basic server setup und hardening
description: anlegen und hardening eines Linux Servers
summary: Was mache ich nach der Installation eines Ubuntu Basissystems und wie sichere ich meinen Server ab
tags: Linux Server Administration Hardening Security
date: 2025-03-04 00:00:00

---

# Vorwort

Ich habe vor ein paar Wochen eine Mail bekommen weil mein Hosting-Anbieter meinen Tarif auslaufen lässt und nun wegen "Mehraufwand" den Preis anheben will. Ich habe also in der Folge meinen Vertrag gekündigt und bin auf einen (günstigeren) Tarif gewechselt. Damit geht ein Serverwechsel einher und ich muss meine Daten und Applikationen auf einen neuen Server migrieren. 

Dies nutze ich jetzt für mich um einfach mal wieder meine (Sicherheits-) Strategie zum Aufsetzen eines Servers zu überprüfen und eventuell anzupassen oder zu aktualisieren.

Das hier wird also quasi ein Zusammenschrieb meiner aktuellen Strategie.

Der Server den ich einrichten werde ist ein VPS System mit bereits vorinstalliertem Ubuntu 24.04 LTS (Stand 30.03.25), vollständig gepatcht.

# Servereinrichtung

## Einzelschritte

### System updaten

Erst einmal update ich das Basissystem auf einen aktuellen Patchlevel, danach lege ich über das Admin-Panel meines Hosters einen Snapshot an um ein cleanes, aktuelles System zu haben.

```bash
apt update
apt upgrade
```

### Neuen non-root Benutzer anlegen

Danach lege ich immer einen allgemeinen Admin User an der zwar in der sudo Gruppe enthalten ist aber nicht direkt root Berechtigungen hat. 

```bash
adduser <user>
usermod -aG sudo <user> //fügt den Nutzer der sudoers Gruppe hinzu
```

### Hostnamen wechseln

Bei meinem Hoster haben die Rechner immer kryptische Bezeichnungen die sich kein Mensch merken kann, also:

```bash
sudo vi /etc/hostname
```

dort den Hostnamen anpassen, danach *reboot*

Das war der *oldschool* Weg, moderne systemd basierte Systeme können auch einfach das hier machen:

```bash
hostnamectl set-hostname <hostname>
```

### ssh konfigurieren

Erst einmal die alte Konfiguration sichern bevor wir etwas anpassen:

```bash
cp /etc/ssh/sshd_config ~/sshd_config_original
```

#### ssh Zertifikate konfigurieren

Wir setzen einmal voraus dass der Server bereits einen laufenden SSH Daemon hat. Diesen wollen wir nun von Passwörtern auf Zertifikate umstellen.

Die Zertifikate werden normalerweise asymmetrisch auf dem Client, also eurem PC erzeugt. Ihr erhaltet daraufhin einen privaten Schlüssel, der niemals euren Rechner verlässt, und einen öffentlichen Schlüssel der mit dem Ziel-Rechner (also eurem Server) geteilt wird.

##### Erzeugung des Schlüssels

wenn ihr noch keinen Schlüssel besitzt **führt folgendes auf eurem Rechner aus**:

```bash
ssh-keygen
```

Dadurch landen eure keys in eurem ssh Verzeichnis, normalerweise in einem versteckten Verzeichnis unter `~/.ssh/`. Dabei werden zwei Dateien erzeugt, einmal `id_rsa` (private key) und `id_rsa.pub` (public key), jedenfalls wenn man die vorgeschlagenen Namen beibehält.

Beim Erstellen der Keys wird abgefragt ob man eine Passphrase erstellen möchte. Dieses Passwort dient nicht der Verschlüsselung der Verbindung sondern nur der Absicherung des lokalen Zertifikats, ist also bspw. sinnvoll wenn man sich den Rechner mit mehreren anderen teilt. Ansonsten kann man darauf verzichten. 

Nun kann man den Public Key auf seinen Server hochladen, das geht entweder manuell mittels rsync oder direkt per ssh:

```bash
ssh-copy-id <user>@<IP-Adresse>
```

#### sshd_config anpassen

Danach kann man ein paar Konfigurationsparameter (normalerweise in `/etc/ssh/sshd_config`) anpassen um das System sicherer zu machen, ich passe immer an:

```bash
PermitEmptyPasswords no #verhindert leere Passwörter - duh
AllowUsers <Username> #lässt nur Verbindungen bestimmter Useraccounts zu. Auch für Gruppen möglich
Port <Portnummer> #setzt einen eigenen Port - gut gegen automatisierte Portscans und Angriffe
PermitRootLogin no #verhindert zusätzlich Logins als Root User
PasswordAuthentication no #erst setzen NACHDEM man Zertifikatsbasierte Auth eingerichtet hat!
```

Danach einmal den Dienst oder Server neustarten:

`systemctl restart ssh`

#### SSH Connection Alias setzen

Um den Connect Befehl mittels ssh etwas weniger sperrig zu gestalten können wir noch einen Alias setzen, dazu muss auf *eurem lokalen Client* im Verzeichnis `~/.ssh` eine Datei `config` mit folgendem Inhalt angelegt werden:

```bash
Host <Alias> # der Name für die Verbindung
User <User> # der Username mit dem ihr euch per ssh verbindet, siehe sshd_config unter AllowUsers
Hostname <Server-IP> # hier entweder die IP oder FQDN des Servers
Port 2222 # der Port über den ihr zu ssh verbindet, siehe ssd_config
```

### Automatisch Sicherheitspatches einspielen

Ubuntu bringt ein nützliches Tool mit das automatisch mittels cronjob prüft ob Sicherheitsupdates vorliegen und diese automatisch installiert. Das tool `unattended-upgrades` sollte standardmäßig installiert sein, man muss es nur einmal anschalten mittels:

```bash
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

Wer die Einstellungen weiter verfeinern möchte findet die Konfigurationsdatei unter `/etc/apt/apt.conf.d/50unattended-upgrades`

### MOTD konfigurieren

Die MOTD (Message of the day) ist eine Quality of life Anpassung - kann man machen, muss man aber nicht.

Ich persönlich passe sie gerne an um direkt beim Login ein paar interessante Systeminformationen abzugreifen, dazu gehören

- Hostname
- Systemauslastung
- Status System-Services (läuft fail2ban, läuft ufw, läuft Podman/Containerisierung,...)
- Status Intrusion Detection (bspw. fail2ban)
- Status laufende Container
- Status verfügbare Updates, Reboot notwendig 

Wichtig zu verstehen ist, dass die MOTD normalerweise eine statische Textdatei ist. Unter Ubuntu gibt es aber `update-motd` wodurch es möglich ist die MOTD Datei semi-dynamisch erzeugen zu lassen. Dazu wird die MOTD alle paar Minuten mittels eines Cronjobs neu erstellt. Die zugrundeliegenden files um die MOTD zusammenzusetzen liegen unter `/etc/update-motd.d/*` und können bspw. auch Skripte enthalten. 

### Podman/Container Updates automatisieren

### UFW konfigurieren

### fail2ban konfigurieren



