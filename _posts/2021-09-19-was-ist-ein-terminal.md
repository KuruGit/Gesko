---
layout: post
title: Linux Basics Teil 3 - Terminal, Shell, Command Line und Prompt
description: Teil 3 der Artikelreihe über Linux Basics
summary: In Teil 3 der Artikelreihe versuche ich diesmal zu erklären was der Unterschied zwischen einem Terminal, der Shell, der Kommandozeile und dem Prompt ist
tags: coding Linux
date: 2021-09-19 00:00:00
---

# Worum geht es hier überhaupt?

Fängt man mit Linux an stößt man unweigerlich irgendwann auf die Kommandozeile.
Viele Benutzer schrecken hiervor anfangs zurück weil die Eingabe von Befehlen und die Ausgabe auf einer Textzeile anfangs gewöhnungsbedürftig sind und viele Benutzer sich überfordert fühlen. 
Dieses Problem wird auch nicht dadurch besser dass viele Artikel und auch andere Benutzer in Foren und auf social media wie selbstverständlich mit unbekannten Fremdwörtern um sich werfen.
Dem möchte ich ein wenig mit diesem Artikel entgegen wirken.

## Ein wenig Historie und der Begriff des Terminals

Ein Terminal, auch genannt Konsole oder auch klassisch deutsch als *Datensichtgerät* bezeichnet war früher<sup>TM</sup> tatsächlich ein externes Gerät zur Abfrage und Eingabe von Daten auf einem Server. Im Prinzip handelte es sich dabei um einen Bildschirm samt Tastatur um auf einem Großrechner aus der Entfernung seine Arbeiten zu verrichten. Die Dinger sahen so aus:

![Bild eines alten Terminals](https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Televideo925Terminal.jpg/579px-Televideo925Terminal.jpg)[^Terminal]

Heute gibt es Terminals in dieser Form kaum noch und in den meisten Firmen und Einrichtungen haben sich die individuellen Personalcomputer durchgesetzt. 

### Was hat das nun mit Linux zu tun

Nachdem die Hardware Terminals im wesentlichen den Weg des Dodos gegangen sind ist das *Software* Terminal unter Linux nach wie vor lebendig.

Hierunter versteht man im wesentlichen eine Softwareemulation eines Terminals. Wenn man also sowas sieht hat man ein *Softwareterminal* vor sich:

![Bild eines Linux Terminals](/assets/pictures/2021-09-19-terminalexampe.png)

Der Begriff des Terminalemulators meint dabei das Fenster bzw. das *äußere* Programm. Selber macht der Terminalemulator mehr oder weniger viel, er bietet im wesentlichen den anderen Programmen nur eine *Bühne* um ausgeführt zu werden.
Einige Terminals, wie beispielsweise [kitty](https://sw.kovidgoyal.net/kitty/) welches ich momentan nutze, haben ein paar besondere Funktionen wie keyboard shortcuts, Tabbing, geteilte Fenster oder sogar ein eigenes Skripting-System.
Teilweise verschmelzen hier auch ein wenig die Grenzen zur Shell. Damit wären wir dann auch...

### Die Shell

Erinnert ihr euch noch als wir [im ersten Artikel]({% post_url 2021-08-14-Linux-Basics-Teil-1 %}) über die GNU Core Utils gesprochen haben? Im Prinzip ist die Shell das Programmgerüst über das diese weiteren Programme ausgeführt werden.
Verschiedene Shells bringen verschiedene Funktionalitäten und Standardprogramme mit sich. Korn, Bash, Zsh und viele andere sind Beispiele für Shells die sich mal mehr und mal weniger voneinander unterscheiden.

Welche Shell Ihr momentan verwendet könnt Ihr auf verschiedene Arten herausfinden:

Zum einen könnt ihr direkt in die Konfigurationsdateien hineinschauen.

~~~Bash
less /etc/passwd|grep user
~~~

Unter /etc/passwd sind Informationen zu dem einzelnen Usern abgelegt, rufe ich beispielsweise oberen Befehl für meinen User _david_ auf, so bekomme ich folgenden output:

~~~Bash
less /etc/passwd|grep david
david:x:1000:1000:david,,,:/home/david:/usr/bin/zsh
~~~
Die Einträge in dieser Datei sind durch _:_ geteilt und enthalten beispielsweise Informationen zum Usernamen, Home-Directory des Nutzers und so weiter. Wir werden uns diese Datei nochmal ansehen sobald wir behandeln wie man User anlegt und verwaltet. Uns interessiert gerade nur der letzte Eintrag:

*:/usr/bin/zsh*

Dieser sagt aus dass jedes neues Terminal dass für meinen User aufgerufen wird, erstmal mit ZSH als Shell startet. Hier könnte auch eine andere shell, beispielsweise die *bash* stehen. 

Ebenfalls kann man aber auch die Shell über Umgebungsvariablen herausfinden:

~~~Bash
echo $SHELL
~~~

gibt beispielsweise die *default* Shell an die das System benutzt. Hier ist zu beachten dass dies nicht zwangsläufig auch die aktuell laufende Shell sein muss, sondern nur die mit der das Terminal normalerweise startet!

Etwas *hackig* aber funktioniert normalerweise auch wäre dieser Befehl:

~~~Bash
echo $0
~~~
wobei $0 den aktuell laufenden Prozess anzeigt, in der Regel ist das die Shell. Eine andere Art den aktuellen Prozess anzuzeigen wäre aus der Variable $$ und wenn man es ganz genau wissen will kann man über den *ps* Befehl die Prozess-ID heraussuchen und auf den Namen des Prozesses filtern, das sieht dann so aus:

~~~Bash
ps -p "$$" -o command=""
~~~
 
Es gibt noch weitere Varianten die aber zum Teil auf spezifische Features einer Shell zugreifen. Selbst *ps -p* funktioniert anscheinend nicht auf allen Systemen, das habe ich persönlich allerdings noch nicht erlebt.




 
 
### Fußnoten
[Terminal]: Siehe [Wikipedia](https://de.wikipedia.org/wiki/Terminal_(Computer) für eine schöne Definition

