---
layout: post
title: Testprojekt: Eine Chrome Extension mit ChatGPT bauen
description: in diesem Projekt möchte ich schauen wie weit ich bei der Entwicklung einer eigenen Chrome Extension durch Zuhilfenahme von AI Tools wie ChatGPT komme 
summary: in diesem Projekt möchte ich schauen wie weit ich bei der Entwicklung einer eigenen Chrome Extension durch Zuhilfenahme von AI Tools wie ChatGPT komme 
tags: Browser Extension Programmierung AI ChatGPT
date: 2023-03-01 00:00:00
---

## Fragestellung

Browser Extension schreiben um aus markierten Textstellen heraus einen CalDav Eintrag gegen Nextcloud zu starten und eine Task anzulegen oder zu ändern. Quasi eine Kopie der Möglichkeiten der Todoist Extension, aber open source und mit CalDav kompatibel. Leider bietet todoist scheinbar keine CalDav integration und ich konnte keine Open Source Lösung finden oder nur sehr aufwändiges Zeug wie ganze Projektmanagement Programme.

## ChatGPT erster Draft

Certainly! Creating a browser extension for Chrome involves using HTML, CSS, and JavaScript. Here's a basic example to get you started on a Chrome extension that updates CalDAV data on a Nextcloud instance. This example assumes you want to perform some actions when the extension button is clicked.

1. **Create the Extension Files:**

   Create a new directory for your extension and create the following files:

   - `manifest.json`: Describes the extension.

     ```json
     {
       "manifest_version": 2,
       "name": "CalDAV Updater",
       "version": "1.0",
       "description": "Update CalDAV data on Nextcloud",
       "permissions": [
         "storage",
         "activeTab"
       ],
       "browser_action": {
         "default_icon": "icon.png",
         "default_popup": "popup.html"
       },
       "icons": {
         "48": "icon.png",
         "128": "icon.png"
       },
       "content_scripts": [
         {
           "matches": ["<all_urls>"],
           "js": ["content.js"]
         }
       ]
     }
     ```

   - `popup.html`: The popup that appears when the extension icon is clicked.

     ```html
     <!DOCTYPE html>
     <html>
     <head>
       <title>CalDAV Updater</title>
       <script src="popup.js"></script>
     </head>
     <body>
       <button id="updateButton">Update CalDAV</button>
     </body>
     </html>
     ```

   - `popup.js`: JavaScript file for the popup.

     ```javascript
     document.getElementById('updateButton').addEventListener('click', function() {
       // Perform CalDAV update here
       // You can use browser.runtime.sendMessage or other methods to communicate with content.js
     });
     ```

   - `content.js`: JavaScript file injected into the pages. It can interact with the page's content.

     ```javascript
     // Content script logic goes here
     ```

2. **Load the Extension:**

   - Open Chrome and go to `chrome://extensions/`.
   - Enable "Developer mode" in the top right.
   - Click on "Load unpacked" and select your extension directory.

3. **Implement CalDAV Logic:**

   In the `popup.js` file, replace the comment with the actual logic to update CalDAV data. You may need to use an AJAX library or the `fetch` API to make requests to your Nextcloud instance's CalDAV endpoint.

   ```javascript
   document.getElementById('updateButton').addEventListener('click', function() {
     // Perform CalDAV update here
     // Example using fetch:
     fetch('https://your-nextcloud-instance/remote.php/dav/calendars/username/calendarname', {
       method: 'PROPFIND',
       headers: {
         'Authorization': 'Basic ' + btoa('your-username:your-password'),
         'Content-Type': 'application/xml'
       },
       // Include the necessary XML data for the CalDAV request
       // ...
     })
     .then(response => {
       if (!response.ok) {
         throw new Error('Network response was not ok');
       }
       return response.text();
     })
     .then(data => {
       // Process the CalDAV response data
       console.log(data);
     })
     .catch(error => {
       console.error('There was a problem with the fetch operation:', error);
     });
   });
   ```

   Note: It's essential to handle authentication securely. In this example, the credentials are hardcoded, but in a real-world scenario, you might want to use the `chrome.identity` API for secure authentication.

This is a basic example, and depending on your specific requirements, you may need to extend and customize the code. Be sure to consult the official Chrome Extension documentation for more advanced features and best practices: [Chrome Extension Developer Guide](https://developer.chrome.com/docs/extensions/mv3/getstarted/).
