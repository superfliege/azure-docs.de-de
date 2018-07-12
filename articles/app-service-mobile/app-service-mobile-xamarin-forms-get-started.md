---
title: Erste Schritte mit Mobile Apps unter Verwendung von Xamarin.Forms
description: In diesem Einführungstutorial erfahren Sie, wie Sie Mobile Apps für die Xamarin.Forms-Entwicklung verwenden.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/25/2018
ms.author: crdun
ms.openlocfilehash: b2dafbcf0e41e7387157590e145f74430686321c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38307118"
---
# <a name="create-a-xamarinforms-app-with-azure"></a>Erstellen einer Xamarin.Forms-App mit Azure

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

Dieses Tutorial zeigt, wie Sie einer mobilen Xamarin.Forms-App mithilfe des Mobile Apps-Features von Azure App Service als Back-End einen cloudbasierten Back-End-Dienst hinzufügen. Sie erstellen sowohl ein neues Mobile Apps-Back-End als auch eine einfache Xamarin.Forms-Aufgabenlisten-App, die App-Daten in Azure speichert.

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Mobile Apps-Lernprogramme für Xamarin.Forms.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto besitzen, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps, die Sie auch nach Ablauf der Testversion weiter nutzen können. Weitere Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio-Tools für Xamarin, in Visual Studio 2017 oder Visual Studio für Mac. Anweisungen finden Sie auf der [Xamarin-Installationsseite][Install Xamarin].

* (optional) Zum Erstellen einer iOS-App ist ein Mac mit Xcode 9.0 oder höher erforderlich. Zum Entwickeln von iOS-Apps kann Visual Studio für Mac verwendet werden. Alternativ können Sie Visual Studio 2017 verwenden (sofern der Mac im Netzwerk verfügbar ist).

## <a name="create-a-new-mobile-apps-back-end"></a>Erstellen eines neuen Mobile Apps-Back-Ends

Gehen Sie zum Erstellen eines neuen Mobile Apps-Back-Ends wie folgt vor:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sie haben nun ein Mobile Apps-Back-End eingerichtet, das Ihre mobilen Anwendungen verwenden können. Im nächsten Schritt laden Sie ein Serverprojekt für ein einfaches Aufgabenlisten-Back-End herunter und veröffentlichen es anschließend in Azure.

## <a name="configure-the-server-project"></a>Konfigurieren des Serverprojekts

Gehen Sie wie folgt vor, um das Serverprojekt für die Verwendung des Node.js- oder .NET-Back-Ends zu konfigurieren:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>Herunterladen und Ausführen der Xamarin.Forms-Lösung

Die Visual Studio-Tools für Xamarin sind zum Öffnen der Projektmappe erforderlich. Informationen finden Sie in den [Xamarin-Installationsanweisungen][Install Xamarin]. Sind die Tools bereits installiert, führen Sie die folgenden Schritte aus, um die Projektmappe herunterzuladen und zu öffnen:

### <a name="visual-studio"></a>Visual Studio

1. Öffnen Sie das [Azure-Portal].

2. Klicken Sie auf dem Blatt „Einstellungen“ für Ihre mobile App auf **Schnellstart** (unter „Bereitstellung“) > **Xamarin.Forms**. Klicken Sie unter Schritt 3 auf **Neue App erstellen** , falls noch nicht ausgewählt.  Klicken Sie dann auf die Schaltfläche **Herunterladen** .

   Dadurch wird ein Projekt heruntergeladen, das eine mit Ihrer mobilen App verbundene Clientanwendung enthält. Speichern Sie die komprimierte Projektdatei auf dem lokalen Computer und merken Sie sich, wo Sie sie gespeichert haben.

3. Extrahieren Sie das heruntergeladene Projekt, und öffnen Sie es dann in Visual Studio 2017.

   ![Extrahiertes Projekt in Visual Studio][8]

4. Führen Sie die nachstehenden Anweisungen aus, um die Android- oder Windows-Projekte bzw. das iOS-Projekt (sofern ein Mac-Computer im Netzwerk verfügbar ist) auszuführen.

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Öffnen Sie das [Azure-Portal].

2. Klicken Sie auf dem Blatt „Einstellungen“ für Ihre mobile App auf **Schnellstart** (unter „Bereitstellung“) > **Xamarin.Forms**. Klicken Sie unter Schritt 3 auf **Neue App erstellen** , falls noch nicht ausgewählt.  Klicken Sie dann auf die Schaltfläche **Herunterladen** .

   Dadurch wird ein Projekt heruntergeladen, das eine mit Ihrer mobilen App verbundene Clientanwendung enthält. Speichern Sie die komprimierte Projektdatei auf dem lokalen Computer und merken Sie sich, wo Sie sie gespeichert haben.

3. Extrahieren Sie das heruntergeladene Projekt, und öffnen Sie es dann in Visual Studio für Mac.

   ![Extrahiertes Projekt in Visual Studio für Mac][9]

4. Führen Sie die nachstehenden Anweisungen aus, um die Android- oder iOS-Projekte auszuführen.



## <a name="optional-run-the-android-project"></a>(Optional) Ausführen des Android-Projekts

In diesem Abschnitt führen Sie das Xamarin.Android-Projekt aus. Wenn Sie nicht mit Android-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

### <a name="visual-studio"></a>Visual Studio

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt (Droid), und wählen Sie **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Build** die Option **Konfigurations-Manager** aus.

3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** neben dem Android-Projekt die Kontrollkästchen **Erstellen** und **Bereitstellen**. Stellen Sie sicher, dass für das Projekt mit freigegebenem Code das Kontrollkästchen **Erstellen** aktiviert ist.

4. Drücken Sie **F5**, oder klicken Sie auf die Schaltfläche **Starten**, um das Projekt zu erstellen und die App in einem Android-Emulator zu starten.

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und wählen Sie **Als Startprojekt festlegen** aus.

2. Klicken Sie auf das Menü **Ausführen** und dann auf **Debuggen starten**, um das Projekt zu erstellen und die App in einem Android-Emulator zu starten.



Geben Sie in der App einen aussagekräftigen Text ein (beispielsweise *Xamarin kennenlernen*), und wählen Sie anschließend das Pluszeichen (**+**) aus.

![Android-To-Do-App][11]

Dadurch wird eine POST-Anforderung an das neue, in Azure gehostete Mobile Apps-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom Mobile Apps-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

> [!NOTE]
> Den Code, der auf Ihr Mobile Apps-Back-End zugreift, finden Sie in der C#-Datei **TodoItemManager.cs** des Projekts mit freigegebenem Code in der Projektmappe.
>

## <a name="optional-run-the-ios-project"></a>(Optional) Ausführen des iOS-Projekts

In diesem Abschnitt führen Sie das Xamarin.iOS-Projekt für iOS-Geräte aus. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

### <a name="visual-studio"></a>Visual Studio

1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und wählen Sie **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Build** die Option **Konfigurations-Manager** aus.

3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** neben dem iOS-Projekt die Kontrollkästchen **Erstellen** und **Bereitstellen**. Stellen Sie sicher, dass für das Projekt mit freigegebenem Code das Kontrollkästchen **Erstellen** aktiviert ist.

4. Drücken Sie **F5** , um das Projekt zu erstellen und die App im iPhone-Emulator zu starten.

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und wählen Sie **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Ausführen** die Option **Debuggen starten** aus, um das Projekt zu erstellen und die App im iPhone-Emulator zu starten.



Geben Sie in der App einen aussagekräftigen Text ein (beispielsweise *Xamarin kennenlernen*), und wählen Sie anschließend das Pluszeichen (**+**) aus.

![iOS-To-Do-App][10]

Dadurch wird eine POST-Anforderung an das neue, in Azure gehostete Mobile Apps-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom Mobile Apps-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

> [!NOTE]
> Den Code, der auf Ihr Mobile Apps-Back-End zugreift, finden Sie in der C#-Datei **TodoItemManager.cs** des Projekts mit freigegebenem Code in der Projektmappe.
>

## <a name="optional-run-the-windows-project"></a>(Optional) Ausführen des Windows-Projekts

In diesem Abschnitt führen Sie das Xamarin.Forms-UWP-Projekt (Universelle Windows-Plattform) für Windows-Geräte aus. Wenn Sie nicht mit Windows-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

### <a name="visual-studio"></a>Visual Studio

1. Klicken Sie mit der rechten Maustaste auf ein beliebiges UWP-Projekt, und wählen Sie **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Build** die Option **Konfigurations-Manager** aus.

3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** neben dem ausgewählten Windows-Projekt die Kontrollkästchen **Erstellen** und **Bereitstellen**. Stellen Sie sicher, dass für das Projekt mit freigegebenem Code das Kontrollkästchen **Erstellen** aktiviert ist.

4. Um das Projekt zu erstellen und die App in einem Windows-Emulator zu starten, drücken Sie **F5**, oder klicken Sie auf die Schaltfläche **Starten**. (Der Text **Lokaler Computer** sollte angezeigt werden.)

> [!NOTE]
> Das Windows-Projekt kann nicht unter macOS ausgeführt werden.



Geben Sie in der App einen aussagekräftigen Text ein (beispielsweise *Xamarin kennenlernen*), und wählen Sie anschließend das Pluszeichen (**+**) aus.

Dadurch wird eine POST-Anforderung an das neue, in Azure gehostete Mobile Apps-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom Mobile Apps-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

![UWP-To-Do-App][12]

> [!NOTE]
> Den Code, der auf Ihr Mobile Apps-Back-End zugreift, finden Sie in der C#-Datei **TodoItemManager.cs** des Portable Class Library-Projekts Ihrer Projektmappe.
>

## <a name="troubleshooting"></a>Problembehandlung

Sollten beim Erstellen der Projektmappe Probleme auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die neueste Version von Xamarin.Forms. Aktualisieren Sie außerdem im Android-Projekt die Xamarin.Android-Supportpakete. Schnellstartprojekte enthalten unter Umständen nicht immer die neuesten Versionen.

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen von Authentifizierung zu Ihrer App](app-service-mobile-xamarin-forms-get-started-users.md): Erfahren Sie, wie Sie Benutzer Ihrer App bei einem Identitätsanbieter authentifizieren.

* [Hinzufügen von Pushbenachrichtigungen zu Ihrer App:](app-service-mobile-xamarin-forms-get-started-push.md) Hier erfahren Sie, wie Sie Ihrer App die Unterstützung von Pushbenachrichtigungen hinzufügen und Ihr Mobile Apps-Back-End für die Verwendung von Azure Notification Hubs zum Senden der Pushbenachrichtigungen konfigurieren.

* [Aktivieren der Offlinesynchronisierung für Ihre App:](app-service-mobile-xamarin-forms-get-started-offline-data.md) Hier erfahren Sie, wie Sie mithilfe eines Mobile App-Back-Ends Ihrer App Offlineunterstützung hinzufügen. Mit der Offlinesynchronisierung können Sie die Daten Ihrer mobilen App auch ohne Netzwerkverbindung anzeigen, hinzufügen oder ändern.

* [Verwenden des verwalteten Clients für Mobile Apps:](app-service-mobile-dotnet-how-to-use-client-library.md) Hier erfahren Sie, wie Sie das SDK für verwaltete Clients in Ihrer Xamarin-App verwenden.

* [Verwenden anderer Azure-Dienste mit Xamarin.Forms:](https://docs.microsoft.com/xamarin/xamarin-forms/data-cloud/) Hier finden Sie Informationen zum Hinzufügen zusätzlicher Azure-Features wie Suche, Speicher und Cognitive Services zu Xamarin.Forms-Apps.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png

<!-- URLs. -->
[Install Xamarin]: https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-Portal]: https://portal.azure.com/
