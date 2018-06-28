---
title: Senden von Pushbenachrichtigungen an Chrome-Apps mit Azure Notification Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Notification Hubs zum Senden von Pushbenachrichtigungen an eine Chrome-App verwenden.
services: notification-hubs
keywords: Mobile Pushbenachrichtigungen,Pushbenachrichtigungen,Pushbenachrichtigung,Chrome-Pushbenachrichtigungen
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 7bdc692104194bff4a25e6974ba72971af543cbf
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37028998"
---
# <a name="tutorial-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Tutorial: Senden von Pushbenachrichtigungen an Chrome-Apps mit Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In diesem Tutorial wird beschrieben, wie Sie einen Notification Hub erstellen und mithilfe von [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/) Pushbenachrichtigungen an eine Google Chrome-Beispiel-App senden. Die Chrome-App wird im Kontext eines Google Chrome-Browsers ausgeführt und beim Notification Hub registriert. 

> [!NOTE]
> Pushbenachrichtigungen von Chrome-Apps sind keine generischen browserinternen Benachrichtigungen. Sie sind speziell für das Browser-Erweiterbarkeitsmodell bestimmt. (Details finden Sie in der Übersicht zu Chrome-Apps unter [Chrome-Apps – Übersicht].) Zusätzlich zum Desktopbrowser können Chrome-Apps über Apache Cordova auch auf Mobilgeräten (Android und iOS) ausgeführt werden. Weitere Informationen finden Sie unter [Chrome-Apps auf Mobilgeräten] (Ausführen von Chrome-Apps auf Mobilgeräten unter Verwendung von Apache Cordova).

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * [Aktivieren von Google Cloud Messaging](#register)
> * [Konfigurieren Ihres Notification Hub](#configure-hub)
> * [Verbinden Ihrer Chrome-App mit dem Notification Hub](#connect-app)
> * [Senden einer Pushbenachrichtigung an Ihre Chrome-App](#send)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a id="register"></a>Aktivieren von Google Cloud Messaging
1. Navigieren Sie zur Website [Google Cloud Console](https://console.cloud.google.com/cloud-resource-manager), und melden Sie sich mit den Anmeldeinformationen für Ihr Google-Konto an.
2. Wählen Sie auf der Symbolleiste die Option **Create Project** (Projekt erstellen) aus. 

    ![Schaltfläche „Create Project“ (Projekt erstellen)](media/notification-hubs-chrome-get-started/create-project-button.png)
1. Geben Sie unter **Project Name** einen geeigneten Projektnamen an, und klicken Sie dann auf die Schaltfläche **Create**.
2. Wählen Sie auf der Symbolleiste das Benachrichtigungssymbol (Glocke) und dann die Meldung **Create Project** (Projekt erstellen) aus. 

    ![Benachrichtigung zum Erstellen eines Projekts](media/notification-hubs-chrome-get-started/project-creation-notification.png)
1. Notieren Sie sich die Projektnummer (**Project Number**) auf der Seite **Projects** (Projekte) für das Projekt, das Sie erstellt haben. Die Projektnummer wird in der Chrome-App als **GCM Sender ID** (GCM-Absender-ID) für die Registrierung bei GCM verwendet.
   
    ![Google Cloud Console – Projektnummer](media/notification-hubs-chrome-get-started/gcm-project-number.png)
3. Wählen Sie im Dashboard die Option **Go to APIs overview** (Zur API-Übersicht wechseln) aus. 

    ![Schaltfläche „Go to APIs overview“ (Zur API-Übersicht wechseln)](media/notification-hubs-chrome-get-started/go-to-apis-overview-button.png)
1. Wählen Sie auf der Seite „APIs & Services“ (APIs und Dienste) die Option **Enable APIs and Services** (APIs und Dienste aktivieren) aus. 

    ![Enable APIs and Services (APIs und Dienste aktivieren)](media/notification-hubs-chrome-get-started/enable-apis-and-services.png)
1. Durchsuchen Sie die Liste mit dem Schlüsselwort **cloud messaging**. Wählen Sie **Google Cloud Messaging** in der gefilterten Liste aus. 

    ![Google Cloud Messaging-API](media/notification-hubs-chrome-get-started/google-cloud-messaging-api.png)
1. Wählen Sie auf der Seite **Google Cloud Messaging** die Option **Enable** (Aktivieren) aus.

    ![Aktivieren von GCM](media/notification-hubs-chrome-get-started/enable-gcm.png)
1. Wechseln Sie auf der Seite **APIs & Services** (APIs und Dienste) zur Registerkarte **Credentials** (Anmeldeinformationen). Wählen Sie **Create credentials** (Anmeldeinformationen erstellen) und dann **API key** (API-Schlüssel) aus. 

    ![Schaltfläche zum Erstellen des API-Schlüssels](media/notification-hubs-chrome-get-started/create-api-key-button.png)
1. Wählen Sie im Dialogfeld **API key created** (API-Schlüssel erstellt) die Schaltfläche zum Kopieren aus, um den Schlüssel in die Zwischenablage zu kopieren. Speichern Sie den Schlüssel. Dieser Wert wird im nächsten Abschnitt zum Konfigurieren des Notification Hub verwendet, um das Senden von Pushbenachrichtigungen an GCM zu ermöglichen.

    ![API-Seite](media/notification-hubs-chrome-get-started/api-created-page.png)
12. Wählen Sie Ihren API-Schlüssel in der Liste **API keys** (API-Schlüssel) aus. Wählen Sie auf der Seite „API key“ (API-Schlüssel) die Option **IP addresses (web servers, cron jobs, etc.)** (IP-Adressen [Webserver, Cron-Aufträge usw.]) aus, geben Sie **0.0.0.0/0** als IP-Adresse ein, und klicken Sie auf „Save“ (Speichern). 

    ![Aktivieren der IP-Adressen](media/notification-hubs-chrome-get-started/enable-ip-addresses.png)

## <a id="configure-hub"></a>Erstellen Ihres Notification Hub
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

6. Wählen Sie **Google (GCM)** in der Kategorie **Benachrichtigungseinstellungen** aus, geben Sie den **API-Schlüssel** für das GCM-Projekt ein, und klicken Sie auf **Speichern**.

      ![Azure Notification Hubs – Google (GCM)](media/notification-hubs-chrome-get-started/configure-gcm-api-key.png)

## <a id="connect-app"></a>Verbinden Ihrer Chrome-App mit dem Notification Hub
Der Notification Hub ist jetzt für die Arbeit mit GCM konfiguriert, und Sie besitzen die Verbindungszeichenfolge, um die App sowohl für den Empfang als auch das Senden von Pushbenachrichtigungen zu registrieren.

### <a name="create-a-new-chrome-app"></a>Erstellen einer neuen Chrome-App
Das folgende Beispiel basiert auf dem [GCM-Beispiel für Chrome-Apps] und orientiert sich an der empfohlenen Erstellungsmethode für Chrome-Apps. In diesem Abschnitt werden die spezifischen Schritte für Azure Notification Hubs hervorgehoben. 

> [!NOTE]
> Es wird empfohlen, den Quellcode für diese Chrome-App aus dem [Notification Hub-Beispiel für Chrome-Apps]herunterzuladen. 

Die Chrome-App wird mit JavaScript erstellt, und Sie können dazu einen beliebigen Text-Editor verwenden. In der folgenden Abbildung ist die Chrome-App dargestellt:

![Google Chrome-App][15]

1. Erstellen Sie einen Ordner, und geben Sie ihm den Namen `ChromePushApp`. Der Name ist frei wählbar. Bei Verwendung eines eigenen Namens müssen Sie den Pfad in den erforderlichen Codesegmenten entsprechend anpassen.
2. Laden Sie die [crypto-js-Bibliothek] in den Ordner herunter, den Sie im zweiten Schritt erstellt haben. Dieser Bibliotheksordner enthält zwei Unterordner: `components` und `rollups`.
3. Erstellen Sie die Datei `manifest.json`. Alle Chrome-Apps verfügen über eine Manifestdatei. Sie enthält die App-Metadaten und, was noch wichtiger ist, alle Berechtigungen, die der App gewährt werden, wenn sie vom Benutzer installiert wird.
   
        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }
   
    Beachten Sie das `permissions`-Element, mit dem angegeben wird, dass diese Chrome-App Pushbenachrichtigungen von GCM empfangen darf. In der Beispiel-App wird außerdem eine Symboldatei (`gcm_128.png`) verwendet, die sich in der Quelle befindet und aus dem ursprünglichen GCM-Beispiel wiederverwendet wird. Sie können sie durch ein beliebiges Bild ersetzen, das die [Kriterien für Symbole](https://developer.chrome.com/apps/manifest/icons)erfüllt.
4. Erstellen Sie eine Datei namens `background.js` mit dem folgenden Code:
   
        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }
   
        var registerWindowCreated = false;
   
        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {
   
            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }
   
        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);
   
        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    Diese Datei blendet den HTML-Code des Chrome-App-Fensters (**register.html**) ein und definiert den Handler **messageReceived**, der die eingehende Pushbenachrichtigung verarbeitet.
5. Erstellen Sie eine Datei namens `register.html`, die die Benutzeroberfläche der Chrome-App definiert. 
   
   > [!NOTE]
   > In diesem Beispiel wird **CryptoJS v3.1.2**verwendet. Wenn Sie eine andere Version der Bibliothek heruntergeladen haben, sollten Sie sicherstellen, dass Sie die Version unter dem `src` -Pfad richtig ersetzen.
   > 
   > 
   
        <html>
   
        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
   
        <body>
   
        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>
   
        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>
   
        <br/>
   
        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
   
        <br/>
        <br/>
   
        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
   
        </body>
   
        </html>
6. Erstellen Sie eine Datei namens `register.js` mit dem Code in diesem Schritt. Diese Datei legt das Skript hinter `register.html`fest. Chrome-Apps erlauben keine Inline-Ausführung, daher müssen Sie ein separates Sicherungsskript für Ihre Benutzeroberfläche erstellen.
   
        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";
   
        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }
   
        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }
   
          document.getElementById("console").innerHTML = currentStatus  + status;
        }
   
        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);
   
          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }
   
        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }
   
        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";
   
          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });
   
          originalUri = endpoint + hubName;
        }
   
        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration
   
          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;
   
          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);
   
          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }
   
        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";
   
          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);
   
          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();
   
          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };
   
          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }
   
          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");
   
          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }
   
    Das Skript verfügt über die folgenden Schlüsselparameter:
   
   * **window.onload** definiert die Klickereignisse für die zwei Schaltflächen auf der Benutzeroberfläche. Der erste Ereignishandler für den Klick auf eine Schaltfläche führt die Registrierung bei GCM durch. Der andere verwendet die nach der Registrierung bei GCM zurückgegebene Registrierungs-ID für die Registrierung bei Azure Notification Hubs.
   * **updateLog** ist die Funktion, die die Codeprotokollinformationen zulässt. 
   * **registerWithGCM** ist der Klick-Handler für die erste Schaltfläche, der den `chrome.gcm.register`-Aufruf zum Registrieren der aktuellen Chrome-App-Instanz an GCM sendet.
   * **registerCallback** ist die Rückruffunktion, die bei der Rückgabe des GCM-Registrierungsaufrufs aufgerufen wird.
   * **registerWithNH** ist der Klick-Handler für die zweite Schaltfläche, der die Registrierung bei Notification Hubs durchführt. Er ruft die vom Benutzer angegebenen Werte für `hubName` und `connectionString` ab und erstellt den REST-API-Aufruf für die Notification Hubs-Registrierung.
   * **splitConnectionString** und **generateSaSToken** sind Hilfselemente, die die JavaScript-Implementierung eines Prozesses zur SaS-Tokenerstellung darstellen, der für alle REST-API-Aufrufe verwendet werden muss. Weitere Informationen finden Sie unter [Allgemeine Konzepte](http://msdn.microsoft.com/library/dn495627.aspx).
   * **sendNHRegistrationRequest** ist die Funktion, mit der ein HTTP-REST-Aufruf für Azure Notification Hubs durchgeführt wird.
   * **registrationPayload** definiert die Nutzlast der XML-Datei der Registrierung. Weitere Informationen finden Sie unter [Erstellen der Registrierungs-NH-REST-API]. Aktualisieren Sie die enthaltene Registrierungs-ID mit dem von GCM empfangenen Wert.
   * **client** ist eine Instanz von **XMLHttpRequest**, die die Anwendung für die HTTP POST-Anforderung verwendet. Aktualisieren Sie den Header `Authorization` mit `sasToken`. Bei erfolgreichem Abschluss dieses Aufrufs wird diese Chrome-App-Instanz bei Azure Notification Hubs registriert.

        Die gesamte Ordnerstruktur für dieses Projekt sollte der folgenden Struktur ähneln: ![Google Chrome-App – Ordnerstruktur][21]

### <a name="set-up-and-test-your-chrome-app"></a>Einrichten und Testen Ihrer Chrome-App
1. Öffnen Sie den Chrome-Browser. Öffnen Sie **Chrome-Erweiterungen**, und aktivieren Sie **Entwicklermodus**.
   
    ![Google Chrome – Entwicklermodus aktivieren][16]
2. Klicken Sie auf **Entpackte Erweiterung laden** , und navigieren Sie zu dem Ordner, in dem Sie die Dateien erstellt haben. Sie können optional auch das **Chrome Apps & Extensions Developer Tool** verwenden. Dieses Tool ist eine eigene Chrome-App, die aus dem Chrome Web Store installiert werden muss und erweiterte Debuggingfunktionen für die Chrome-App-Entwicklung bereitstellt.
   
    ![Google Chrome – Entpackte Erweiterung laden][17]
3. Wenn die Chrome-App fehlerfrei erstellt wird, wird sie angezeigt.
   
    ![Google Chrome – Chrome-App-Anzeige][18]
4. Geben Sie die **Projektnummer**, die Sie zuvor in der **Google Cloud Console** erhalten haben, als Absender-ID ein, und klicken Sie auf **Bei GCM registrieren**. Die Meldung **Registration with GCM succeeded**
   
    ![Google Chrome – Chrome-App-Anpassung][19]
5. Geben Sie den **Notification Hub-Namen** und die **DefaultListenSharedAccessSignature** ein, die Sie zuvor im Portal erhalten haben, und klicken Sie auf **Bei Azure Notification Hub** registrieren. Die Meldung **Notification Hub Registration successful!** und die Details der Antwort auf die Registrierung mit der Azure Notification Hubs-Registrierungs-ID werden angezeigt.
   
    ![Google Chrome – Notification Hub-Details angeben][20]  

## <a name="send"></a>Senden einer Benachrichtigung an Ihre Chrome-App
Senden Sie zu Testzwecken Chrome-Pushbenachrichtigungen über eine .NET-Konsolenanwendung. 

> [!NOTE]
> Sie können Pushbenachrichtigungen mit Notification Hubs von jedem beliebigen Back-End über die öffentliche <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-Schnittstelle</a> senden. Weitere plattformübergreifende Beispiele finden Sie im [Dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/).
> 
> 

1. Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus. Klicken Sie unter **Visual C#** auf **Windows**, auf **Konsolenanwendung** und dann auf **OK**.  Durch diesen Schritt wird ein neues Konsolenanwendungsprojekt erstellt.
2. Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** und dann auf **Paket-Manager-Konsole**. Daraufhin wird im unteren Fenster die Paket-Manager-Konsole angezeigt.
3. Führen Sie im Konsolenfenster folgenden Befehl aus:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
   Ein Verweis auf das Azure Service Bus SDK mit dem <a href="http://nuget.org/packages/WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus</a>-NuGet-Paket wird dem Projekt automatisch hinzugefügt.
4. Öffnen Sie `Program.cs`, und fügen Sie die folgende `using`-Anweisung hinzu:
   
        using Microsoft.Azure.NotificationHubs;
5. Fügen Sie in der `Program`-Klasse die folgende Methode hinzu:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
    Stellen Sie sicher, dass Sie den Platzhalter `<hub name>` durch den Namen des Notification Hub ersetzen, der im [Portal](https://portal.azure.com) auf der Seite Ihres Notification Hubs angezeigt wird. Ersetzen Sie außerdem den Verbindungszeichenfolgen-Platzhalter durch die Verbindungszeichenfolge `DefaultFullSharedAccessSignature` , die Sie im Abschnitt zur Notification Hub-Konfiguration abgerufen haben.
   
    > [!NOTE]
    > Stellen Sie sicher, dass Sie die Verbindungszeichenfolge mit Vollzugriff (**Full**) verwenden, nicht mit Abhörzugriff (**Listen**). Bei der Verbindungszeichenfolge mit Abhörzugriff ( **Listen** ) werden keine Berechtigungen zum Senden von Pushbenachrichtigungen gewährt.
6. Fügen Sie der `Main` -Methode die folgenden Aufrufe hinzu:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Stellen Sie sicher, dass Chrome ausgeführt wird, und führen Sie die Konsolenanwendung aus.
8. Auf dem Desktop sollte das folgende Benachrichtigungspopupfenster angezeigt werden.
   
    ![Google Chrome – Benachrichtigung][13]
9. Sie können Ihre Benachrichtigungen auch im Fenster „Chrome-Benachrichtigungen“ in der Taskleiste anzeigen (unter Windows), wenn Chrome ausgeführt wird.
   
    ![Google Chrome – Benachrichtigungsliste](./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png)

> [!NOTE]
> Sie müssen die Chrome-App nicht ausführen oder im Browser öffnen (der Chrome-Browser selbst muss allerdings ausgeführt werden). Außerdem wird eine konsolidierte Ansicht all Ihrer Benachrichtigungen im Chrome-Benachrichtigungsfenster angezeigt.
> 
> 

## <a name="next-steps"> </a>Nächste Schritte
In diesem Tutorial haben Sie Übertragungsbenachrichtigungen an alle Clients gesendet, die beim Back-End registriert sind. Um zu erfahren, wie Sie Pushbenachrichtigungen an bestimmte Geräte senden, fahren Sie mit dem folgenden Tutorial fort: 

> [!div class="nextstepaction"]
>[Senden von Pushbenachrichtigungen an bestimmte Geräte](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- Images. -->
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Notification Hub-Beispiel für Chrome-Apps]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Notification Hubs Overview]: notification-hubs-push-notification-overview.md
[Chrome-Apps – Übersicht]: https://developer.chrome.com/apps/about_apps
[GCM-Beispiel für Chrome-Apps]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-Apps auf Mobilgeräten]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Erstellen der Registrierungs-NH-REST-API]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[crypto-js-Bibliothek]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs Notify Users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
