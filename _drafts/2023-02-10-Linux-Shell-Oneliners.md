---
layout: post
title: Linux Basics - Shell Oneliner
description: Eine Liste mit nützlichen Shell Onelinern; wird stetig weiterentwickelt
summary: Eine Liste mit nützlichen Shell Onelinern; wird stetig weiterentwickelt
tags: coding Linux Shell Oneliner Bash
date: 2023-02-01 00:00:00
---

# Systemmanagement

## Finden und sortieren

### die größten Dateien und Ordner in einem bestimmten Ordner finden; sortieren; nur die 5 größten ausgeben

`du -a /home | sort -n -r | head -n 5`

### ... nur Dateien finden

`find -type f -exec du -Sh {} + | sort -rh | head -n 5`

## Usermanagement

### Lock User Account

`sudo usermod -L -e 1 {user}`

- -L: locks Account
- -e 1: sets expiration date to Jan 02, 1970 and prohibits users from changing time settings and logging in again

## Tools

### fail2ban

#### check jail status

`sudo fail2ban-client status nextcloud`

#### unban IP

`sudo fail2ban-client set $jail unbanip $ip`

