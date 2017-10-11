---
title: Erste Schritte mit Android-Apps Azure Mobile Engagement
description: "Erfahren Sie, wie Azure Mobile Engagement mit Analysen und Push-Benachrichtigungen für Android-apps zu verwenden."
services: mobile-engagement
documentationcenter: android
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3c286c6d-cfef-4e3e-9b2c-715429fe82db
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: dc255a930bf71e6ef6d964bc5e3472a38ce4e467
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Erste Schritte mit Azure Mobile Engagement für Android-apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema erfahren Sie, wie Azure Mobile Engagement zu verwenden, um Ihre app-Nutzung zu verstehen und zum Senden von Pushbenachrichtigungen an segmentierte Benutzer von einer Android-Anwendung.
Dieses Lernprogramm veranschaulicht die einfache broadcast Szenario mithilfe von Mobile Engagement. In, erstellen Sie eine leere Android-app, die grundlegende Daten erfasst und Pushbenachrichtigungen mithilfe von Google Cloud Messaging (GCM) empfängt.

## <a name="prerequisites"></a>Erforderliche Komponenten
Abschluss dieses Lernprogramms erfordert die [Android Developer Tools](https://developer.android.com/sdk/index.html), einschließlich Android Studio integrierte Entwicklungsumgebung und die neueste Android-Plattform.

Darüber hinaus müssen die [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).

> [!IMPORTANT]
> Um dieses Lernprogramm abzuschließen, benötigen Sie ein aktives Azure-Konto. Wenn Sie keines Konto haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Einrichten von Mobile Engagement für Ihr Android-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinden Sie die Anwendung, die Mobile Engagement-Back-End
In diesem Lernprogramm wird eine "grundlegende Integration", die den minimalen Satz Daten sammeln und Senden einer Pushbenachrichtigung erforderlich ist. Sie erstellen eine einfache app mit Android Studio, um die Integration zu veranschaulichen.

Die vollständige Integration-Dokumentation finden Sie in der [Mobile Engagement Android SDK-Integration](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Erstellen eines Android-Projekts
1. Starten Sie **Android Studio**, und wählen Sie im Popupfenster **starten ein neues Projekts für Android Studio**.

    ![][1]
2. Geben Sie eine app-Namen und das Unternehmen die Domäne an. Notieren Sie was Sie ausgefüllt werden, da Sie später benötigen. Klicken Sie auf **Weiter**.

    ![][2]
3. Wählen Sie die Ziel-Formfaktor und API-Ebene, und klicken Sie auf **Weiter**.

   > [!NOTE]
   > Mobile Engagement ist eine API-Ebene 10, minimale (Android 2.3.3) erforderlich.
   >
   >

    ![][3]
4. Wählen Sie **Blank Activity** hier also die einzige Bildschirm für diese app, und klicken Sie auf **Weiter**.

    ![][4]
5. Lassen Sie schließlich die Standardwerte, und klicken Sie auf **Fertig stellen**.

    ![][5]

Android Studio erstellt jetzt die Demo-app, die in der wir Mobile Engagement zu integrieren.

### <a name="include-the-sdk-library-in-your-project"></a>Die SDK-Bibliothek in Ihr Projekt einschließen
1. Herunterladen der [Mobile Engagement-Android-SDK](https://aka.ms/vq9mfn).
2. Die Archivdatei, zu einem Ordner auf Ihrem Computer zu extrahieren.
3. Identifizieren Sie die JAR-Bibliothek für die aktuelle Version des SDK und in die Zwischenablage kopieren.

      ![][6]
4. Navigieren Sie zu der **Projekt** Abschnitt (1) und fügen Sie die JAR im Ordner "Bibliotheken" (2).

      ![][7]
5. Um die Bibliothek zu laden, synchronisieren Sie das Projekt.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Verbinden Sie die Anwendung Mobile Engagement-Back-End mit der Verbindungszeichenfolge
1. Kopieren Sie die folgenden Codezeilen, in dem Erstellen der Aktivität (müssen nur an einem Ort Ihrer Anwendung, in der Regel die Hauptaktivität erfolgen). Für dieses Beispiel-app -> Öffnen Sie die Verwendung des Layoutnamens unter Src Main-Java-Ordner >, und fügen Sie Folgendes hinzu:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. Lösen Sie die Verweise durch Drücken von Alt + Eingabetaste oder Hinzufügen von folgende Import-Anweisungen ein:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. Wechseln Sie zurück zum klassischen Azure-Verwaltungsportal in Ihrer app **Verbindungsinformationen** Seite, und kopieren Sie die **Verbindungszeichenfolge**.

      ![][9]
4. Fügen Sie ihn in die `setConnectionString` Parameter, und Ersetzen Sie dabei die gesamte Zeichenfolge, die im folgenden Code gezeigt:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Hinzufügen von Berechtigungen und eine Dienstdeklaration
1. Diese Berechtigungen auf die "Manifest.xml" des Projekts hinzufügen, direkt vor oder nach der `<application>` Tag:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. Um den Agent-Dienst zu deklarieren, fügen Sie diesen Code zwischen den `<application>` und `</application>` Tags:

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. Ersetzen Sie in den Code, die Sie eingefügt haben, `"<Your application name>"` unter "Bezeichnung" wird das im der **Einstellungen** Menü, in denen Dienste angezeigt, die auf dem Gerät ausgeführt. Sie können z. B. das Wort "Dienst" in der Beschriftung hinzufügen.

### <a name="send-a-screen-to-mobile-engagement"></a>Senden Sie einen Bildschirm Mobile Engagement
Zum Senden von Daten beginnen, und stellen Sie sicher, dass der Benutzer aktiv sind, müssen Sie mindestens ein Bildschirm (Aktivität) an die Mobile Engagement-Back-End senden.

Wechseln Sie zu **MainActivity.java** und fügen Sie Folgendes ein, um die Basisklasse der ersetzen **Activity\_main** auf **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> Ist die Basisklasse nicht *Aktivität*, wenden Sie sich an [erweiterte Android Reporting](mobile-engagement-android-advanced-reporting.md) zur Verwendung von anderen Klassen erben.
>
>

Kommentieren Sie die folgende Zeile in diesem Szenario simple Sample-Beispiel aus:

    // setSupportActionBar(toolbar);

Wenn Sie beibehalten möchten die `ActionBar` in Ihrer app finden Sie unter [erweiterte Android Reporting](mobile-engagement-android-advanced-reporting.md).

## <a name="connect-app-with-real-time-monitoring"></a>Verbinden Sie die Anwendung mit Echtzeit-Überwachung
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Aktivieren Sie Pushbenachrichtigungen und in-app-messaging
Während eine Kampagne Sie können Mobile Engagement interagieren und Ihre Benutzer mit Pushbenachrichtigungen und in-app-messaging zu erreichen. Dieses Modul wird im Portal für Mobile Einbindung REACH aufgerufen.
Im folgende Abschnitt richtet Ihre app diese empfangen.

### <a name="copy-sdk-resources-in-your-project"></a>Kopieren Sie die SDK-Ressourcen in Ihrem Projekt
1. Navigieren Sie zurück zu Ihrem Inhalt des SDK herunterladen und kopieren Sie die **Res** Ordner.

    ![][10]
2. Wechseln Sie zurück zum Android Studio, wählen Sie die **main** Verzeichnis der Projektdateien, und fügen Sie ihn, um die Ressourcen zu Ihrem Projekt hinzufügen.

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Nächste Schritte
Wechseln Sie zu [Android SDK](mobile-engagement-android-sdk-overview.md) zum Abrufen von ausführliche Informationen über die SDK-Integration.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
