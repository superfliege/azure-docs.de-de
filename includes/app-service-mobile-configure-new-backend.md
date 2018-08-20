---
title: Includedatei
description: Includedatei
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/25/2018
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 1d3bfb7bc8a5432392dba3b0c5019902b3e59773
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513778"
---
1. Klicken Sie auf die Schaltfläche **App Services**, wählen Sie Ihr Mobile Apps-Back-End aus, wählen Sie **Schnellstart** aus, und wählen Sie anschließend Ihre Clientplattform (iOS, Android, Xamarin oder Cordova) aus.

    ![Azure-Portal mit hervorgehobenem Mobile Apps-Schnellstart][quickstart]

1. Ist keine Datenbankverbindung konfiguriert, gehen Sie wie folgt vor, um eine zu erstellen:

    ![Azure-Portal mit Mobile Apps – Herstellen der Datenbankverbindung][connect]

    a. Erstellen Sie eine neue SQL-Datenbank und einen Server. Sie müssen für das Feld mit dem Namen der Verbindungszeichenfolge unter Umständen den Standardwert „MS_TableConnectionString“ übernehmen, um Schritt 3 weiter unten ausführen zu können.

    ![Azure-Portal mit Mobile Apps – Erstellen einer neuen Datenbank und eines Servers][server]

    b. Warten Sie, bis die Erstellung der Datenverbindung erfolgreich abgeschlossen wurde.

    ![Azure-Portal Benachrichtigung bei erfolgreicher Erstellung der Datenverbindung][notification]

    c. Die Herstellung der Datenverbindung muss erfolgreich sein.

    ![Azure Portal-Benachrichtigung: „Sie verfügen bereits über eine Datenverbindung.“][already-connection]

1. Wählen Sie unter **2. Tabellen-API erstellen** als **Back-End-Sprache** die Option „Node.js“ aus.

1. Akzeptieren Sie die Bestätigung, und wählen Sie dann **TodoItem-Tabelle erstellen** aus.
    Dadurch wird in Ihrer Datenbank eine neue Aufgabenlistentabelle erstellt.

    >[!IMPORTANT]
    > Bei der Umstellung eines vorhandenen Back-Ends auf Node.js werden sämtliche Inhalte überschrieben. Wenn Sie stattdessen ein .NET-Back-End erstellen möchten, lesen Sie [Vorgehensweise: Erstellen eines .NET Mobile App-Back-Ends][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
