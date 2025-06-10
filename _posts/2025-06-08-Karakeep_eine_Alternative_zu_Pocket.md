---
layout: post
title: Karakeep als Alternative zu Pocket
description: Da Pocket demnächst von Mozilla abgeschaltet wird ist es Zeit eine Alternative zu suchen. Enter Karakeep.
summary: Da Pocket demnächst von Mozilla abgeschaltet wird ist es Zeit eine Alternative zu suchen. Enter Karakeep.
tags: Linux Server Administration Karakeep Pocket read-later Apps
date: 2025-06-08 00:00:00

---

# Vorwort

[Pocket wird Anfang Juli '25 abgeschaltet](https://support.mozilla.org/de/kb/Zukunft-von-Pocket) und im Oktober wird kurzer Prozess gemacht und die Konten werden gelöscht, also brauchen wir eine Alternative. Am besten selbstgehostet und open source. Nach etwas herumsuchen bin ich bei Karakeep gelandet, es gibt aber zum Glück eine ganze Batterie an Alternativen:

- [Karakeep](https://karakeep.app/)
- [Omnivore](https://github.com/omnivore-app/omnivore)
- [Grimoire](https://grimoire.pro/)
- [Wallabag](https://wallabag.org/)
- [Readeck](https://readeck.org/en/)

und das sind nur ein paar der Optionen.

Schlussendlich hatte ich bei meiner kurzen Recherche den Eindruck dass Karakeep ([vormals Hoarder](https://youtu.be/TDWombBvK8c)) und Wallabag den besten Eindruck machen. Beide verfügen über eine solide UI, Extensions für die gängigen Browser und mobile Apps für unterwegs. 

![Homepage](/assets/pictures/homepage-eaef57c6d48f2e5ddf6720b1d483a995.png)

Karakeep wirbt zusätzlich noch mit einer AI Integration von Ollama oder ChatGPT für automatisches Tagging, Video Transcription, OCR usw. Da ich gerade mit einer ChatGPT Plus Membership experimentiere bin ich deshalb erst einmal bei Karakeep gelandet.

# Administration

## Installation

Die Installation gestaltet sich dank Docker Compose sehr einfach:

```yaml
services:
  Karakeep:
    image: ghcr.io/karakeep-app/karakeep:${KARAKEEP_VERSION:-release}
    restart: unless-stopped
    volumes:
      # By default, the data is stored in a docker volume called "data".
      # If you want to mount a custom directory, change the volume mapping to:
      - /dein/lokaler/Datenordner:/data #mappt lokales Filesystem auf den /data Ordner im Container
      #- data:/data
    ports:
      - 3000:3000 # Port außen:Port im Container
    environment:
      NEXTAUTH_SECRET: # hier zufälliger Key -> bspw. generieren über: openssl rand -base64 36
      MEILI_ADDR: http://meilisearch:7700
      BROWSER_WEB_URL: http://chrome:9222
      # OPENAI_API_KEY: ...

      # You almost never want to change the value of the DATA_DIR variable.
      # If you want to mount a custom directory, change the volume mapping above instead.
      DATA_DIR: /data # DON'T CHANGE THIS
  chrome:
    image: gcr.io/zenika-hub/alpine-chrome:123
    restart: unless-stopped
    command:
      - --no-sandbox
      - --disable-gpu
      - --disable-dev-shm-usage
      - --remote-debugging-address=0.0.0.0
      - --remote-debugging-port=9222
      - --hide-scrollbars
  meilisearch:
    image: getmeili/meilisearch:v1.13.3
    restart: unless-stopped
    environment:
      MEILI_NO_ANALYTICS: "true"
      MEILI_MASTER_KEY: # hier zufälliger Key -> bspw. generieren über: openssl rand -base64 36
    volumes:
      - meilisearch:/meili_data
volumes:
  meilisearch: null
  data: null
networks:
  nginx-net:
    external: true

```

Das ist erstmal die Basiskonfiguration laut [Anleitung bei Karakeep](https://docs.karakeep.app/Installation/docker). *Zu beachten fand ich hierbei dass man einmal selbst die Auth-Keys generieren muss mit denen NextAuth und MeiliSearch arbeiten.* Das geht aber ganz einfach aus der Kommandozeile mit dem Befehl:

`openssl rand -base64 36`

Was mich aber eine Weile zur Verzweiflung getrieben hat war dass ich ständig eine Exception bekommen habe beim Aufruf der Applikation. Am Ende hat sich herausgestellt dass ich, weil ich mit Docker Compose über Dockge die Container starte, *die Einträge für die beiden Keys direkt wie oben angegeben als environment Variablen in der docker-compose.yml übergeben musste*. Den separaten Eintrag aus dem .env file will entweder Dockge oder Docker Compose in diesem Fall nicht korrekt verarbeiten.

Apropos env File:

## Environment Variablen

Das .env File sollte laut Anleitung folgende Standard Variablen enthalten:

``` yaml
KARAKEEP_VERSION=release
NEXTAUTH_SECRET=super_random_string
MEILI_MASTER_KEY=another_random_string
NEXTAUTH_URL=http://localhost:3000 # falls ihr eine Subdomain habt kommt die hier rein
```

eine ausführliche Liste mit Variablen, bspw. zur Integration von OpenAI findet sich hier. Nachfolgend die Variablen die ich angepasst habe:

| Name                                 | Funktion                                                     |
| ------------------------------------ | ------------------------------------------------------------ |
| OPENAI_API_KEY                       | Euer OpenAI Api Key, näheres zur Einrichtung weiter unten    |
| INFERENCE_LANG                       | Die Sprache mit der Tags generiert werden, Standard ist englisch |
| INFERENCE_CONTEXT_LENGTH: 2048       | Abhängig dieser Tokenlänge wird der Text mit AI gecrawled um die Tags zu erstellen. Längerer Text bedeutet besseres Ergebnis aber ggf. auch teurer bzw. benötigt mehr Rechenleistung |
| CRAWLER_FULL_PAGE_SCREENSHOT: True   | versucht einen kompletten Screenshot der Seite anzulegen, verbraucht naturgemäß mehr Speicher |
| CRAWLER_VIDEO_DOWNLOAD: True         | Versucht bspw. YouTube Videos herunterzuladen. Benutzt im Hintergrund [yt-dlp](https://github.com/yt-dlp/yt-dlp) |
| CRAWLER_VIDEO_DOWNLOAD_MAX_SIZE: 100 | Setzt die max. Größe des runtergeladenen Videos, Qualität wird entsprechend angepasst |
| OCR_LANGS: deu,eng,equ,fra,spa       | Setzt die Sprachen für OCR. equ setzt die Erkennung für math. Formeln! Benutzt im Hintergrund [Tesseract](https://github.com/tesseract-ocr/tesseract) |
| MAX_ASSET_SIZE_MB: 100               | Setzt die max. Asset Size. Standard sind 4MB was mit der SingleFile Extension s.u. zu wenig ist |

## OpenAI Api Key erstellen

Damit Karakeep auf die OpenAI API zugreifen kann benötigt man einen API-Key. Diesen kann man im *OpenAI Dashboard* in seinem Account erstellen:

<img src="/assets/pictures/image-20250608020519006.png" alt="image-20250608020519006" style="zoom:50%;" />

Diesen Key direkt kopieren, *er wird nur einmal angezeigt*. Danach dann den Key in die `OPENAI_API_KEY` Variable eintragen.

## Container starten, Reverse Proxy konfigurieren und andere Arbeiten

Im Anschluss, abhängig von eurer Konfiguration müsst ihr den Karakeep Hauptcontainer starten und noch in euer Container-Netzwerk einbinden. Ich habe hier ein eigenes Netz für einen NGINX Proxy Manager eingerichtet der sich um die Zertifikate kümmert und Anfragen an die Subdomain weiterleitet die ich für Karakeep eingerichtet habe. Betreibt ihr Karakeep lokal sollte es einfach über `http://localhost:3000` erreichbar sein.

In meinem Fall sah der weitere Workflow so aus:

### Container starten

läuft in meinem Fall über die Dockge WebUi, ansonsten über

`docker compose up -d`

### checken ob die Container hochgekommen sind

`docker ps|grep kara`

### Falls kein Docker Netzwerk vorhanden, eins anlegen

`docker network create nginx-net`

### Container in das Netzwerk hängen

Es sollte mehrere Karakeep Container geben, ihr braucht den Hauptcontainer der auf dem in der Compose Datei festgelegten Ports antwortet. Den Namen könnt ihr aus dem Ergebnis des `docker ps` Befehls kopieren:

`docker network connect <Netzwerkname> <Containername>`

### checken ob der Container im Docker Netzwerk hängt

`docker network inspect <Netzwerkname>`

# Login und Usage

Wenn alles geklappt hat können wir nun die Karakeep Login Seite entweder über `localhost:3000` oder den in eurem Reverse Proxy konfigurierten Link erreichen:

<img src="/assets/pictures/image-20250608035733256.png" alt="Karakeep Login Page" style="zoom:50%;" />

Beim ersten Login/Sign Up *wird automatisch ein Admin Benutzer angelegt*. Da

Man kann nun entweder über das **NEW ITEM** Feld einen Link, Notiz oder Screenshot einfügen oder direkt Webseiten über die Browser Extensions oder mobile Apps hinzufügen. Die Apps sind selbsterklärend und erwarten die URL für eure Karakeep Instanz und eure Zugangsdaten für euren Karakeep User.

## User Einstellungen

In den User Einstellungen kann man dann weitere Dinge konfigurieren wie RSS Feeds die gecrawled werden sollen, Prompts für die AI Zusammenfassung von Texten usw. Die User Settings erreicht ihr über das Menü rechts oben:

<img src="/assets/pictures/image-20250608040523319.png" alt="User Settings" style="zoom:50%;" />

Hier kann man dann alles von AI über Webhooks, RSS Feeds oder auch Imports aus anderen Tools wie Omnivore, Pocket, Linkwarden usw. durchführen oder auch API Keys generieren. Wir werden einen API Key im nächsten Teil benötigen, also könnt ihr auch direkt einen anlegen.

<img src="/assets/pictures/image-20250608040818381.png" alt="User Settings Detailansicht" style="zoom:50%;" />

## Einträge bearbeiten

Einzelne Einträge sehen so aus:

<img src="/assets/pictures/image-20250608042306182.png" alt="image-20250608042306182" style="zoom:50%;" />

Über die beiden Symbole rechts unten kann man entweder weitere Optionen über die `...` anzeigen (bspw. einen neuen Crawl anstoßen, Eintrag archivieren, löschen...)

Oder man kann den detaillierten Eintrag aufrufen über das maximieren Symbol. Hier findet man bspw. die Option Tags zu bearbeiten, findet einen Transcript zum Inhalt und kann sich auf Wunsch eine Zusammenfassung per AI einfügen lassen oder eigene Notizen schreiben:



<img src="/assets/pictures/image-20250608042555196.png" alt="Detailansicht Karakeep Eintrag" style="zoom:50%;" />

Die Zusammenfassung mittels AI sieht dann bspw. so aus:

<img src="/assets/pictures/image-20250608042732874.png" alt="AI Summarization eines Eintrags" style="zoom:50%;" />

Achtet nur darauf, dass dies ggf. OpenAI Tokens kostet und damit nicht kostenfrei ist. Ich beobachte mal wie teuer der ganze Spaß ist und poste dann einen Nachtrag in ein paar Wochen oder Monaten.

# Einbinden SingleFile Extension als Workaround für Captcha Sites

Leider musste ich feststellen, dass Seiten die durch CAPTCHAS gesichert sind nicht so einfach mit Karakeep funktionieren (was technisch natürlich auch so gewollt ist). Ein **Workaround** ist momentan die Verwendung der SingleFile Extension als Browser Extension für Firefox oder Chrome. Diese Extension erlaubt es eine ganze Website als html Datei zu speichern. Karakeep erlaubt es dann diese Seite als Asset hochzuladen und zu verarbeiten. 

Momentan funktioniert diese Lösung nur mit dem aktuellen Nightly Release, soll aber mit in das nächste Release einfließen. Deshalb verweise ich hier nur auf die [Anleitung bei Karakeep](https://docs.karakeep.app/next/Guides/singlefile/) selbst, da sich natürlich noch einiges ändern kann und nicht jeder den instabilen Nightly Build verwenden möchte.  

Hier die momentane Anleitung in Stichpunkten:

1. Installiere die [SingleFile extension](https://github.com/gildas-lormeau/SingleFile).
2. In den Einstellungen der Extension auswählen `Destinations`.
3. Wähle `upload to a REST Form API`.
4. Bei der URL folgendes eintragen: `https://YOUR_SERVER_ADDRESS/api/v1/bookmarks/singlefile`.
5. Beim `authorization token` den API Token eintragen den ihr euch in den User Settings von Karakeep generiert habt
6. Das`data field name` auf`file`.
7. Das`URL field name` auf `url`.
8. (Optional) Fügt `&ifexists=MODE` zum URL Feld hinzu MODE entspricht einem der Werte: `skip`, `overwrite`, `overwrite-recrawl`, `append`, oder `append-recrawl`. Bei mir steht dies gerade auf `overwrite`
9. Man sollte außerdem noch die `MAX_ASSET_SIZE_MB: 100` in den ENV Variablen setzen. Standard ist 4MB und das könnte zu wenig sein, da die gerippten Seiten ggf. relativ groß werden

