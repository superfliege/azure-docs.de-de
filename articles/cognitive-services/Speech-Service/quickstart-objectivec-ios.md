---
title: 'Schnellstart: Erkennen von Sprache in Objective-C unter iOS mit dem Spracherkennungsdienst-SDK'
titleSuffix: Azure Cognitive Services
description: Hier finden Sie Informationen zum Erkennen von Sprache in Objective-C unter iOS mit dem Spracherkennungsdienst-SDK.
services: cognitive-services
author: chlandsi
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: chlandsi
ms.openlocfilehash: 7d1e05e13e55b8b7bc07eda71d63f96f12c81ff9
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219187"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-speech-service-sdk"></a>Schnellstart: Erkennen von Sprache in Objective-C unter iOS mit dem Spracherkennungsdienst-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erfahren Sie, wie Sie eine iOS-App in Objective-C mit dem Cognitive Services Speech SDK erstellen, um eine Audiodatei mit aufgezeichneter Sprache in Text zu transkribieren.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Weitere Informationen finden Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* Ein Mac mit installiertem Xcode 9.4.1 als iOS-Entwicklungsumgebung. Dieses Tutorial bezieht sich auf die iOS-Versionen 11.4. Wenn Sie noch nicht über Xcode verfügen, können Sie Xcode aus dem [App Store](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) installieren.

## <a name="get-the-speech-sdk-for-ios"></a>Abrufen des Speech SDK für iOS

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Die aktuelle Version des Cognitive Services Speech SDK ist `1.1.0`.

Das Cognitive Services Speech SDK für Mac und iOS wird zurzeit als Cocoa-Framework bereitgestellt.
Es kann von https://aka.ms/csspeech/iosbinary heruntergeladen werden. Laden Sie die Datei in Ihr Basisverzeichnis herunter.

## <a name="create-an-xcode-project"></a>Erstellen eines Xcode-Projekts 

Starten Sie Xcode, und starten Sie ein neues Projekt, indem Sie auf **Datei** > **Neu** > **Projekt** klicken.
Wählen Sie im Dialogfeld zum Auswählen der Vorlage die Vorlage „Einzelansicht-App (iOS)“ aus.

Treffen Sie in den nachfolgenden Dialogfeldern die folgende Auswahl:

1. Dialogfeld „Projektoptionen“
    1. Geben Sie einen Namen für die Schnellstart-App ein, z.B. `helloworld`.
    1. Geben Sie einen entsprechenden Organisationsnamen und die Organisations-ID ein, wenn Sie bereits über ein Apple-Entwicklerkonto verfügen. Zu Testzwecken können Sie einfach einen beliebigen Namen wie `testorg` auswählen. Um die App zu signieren, benötigen Sie ein ordnungsgemäßes Bereitstellungsprofil. Details finden Sie auf der [Apple Developer-Website](https://developer.apple.com/).
    1. Stellen Sie sicher, dass Objective-C als Sprache für das Projekt ausgewählt wird.
    1. Deaktivieren Sie alle Kontrollkästchen für Tests und Kerndaten.
    ![Projekteinstellungen](media/sdk/qs-objectivec-project-settings.png)
1. Auswählen des Projektverzeichnisses
    1. Wählen Sie das Basisverzeichnis aus, in dem das Projekt gespeichert werden soll. Dadurch wird ein Verzeichnis `helloworld` in Ihrem Basisverzeichnis erstellt, das alle Dateien für das Xcode-Projekt enthält.
    1. Deaktivieren Sie die Erstellung eines Git-Repositorys für dieses Beispielprojekt.
    1. Passen Sie die Pfade zum SDK in den *Projekteinstellungen* an.
        1. Fügen Sie auf der Registerkarte **Allgemein** unter dem Header **Eingebettete Binärdateien** die SDK-Bibliothek als Framework hinzu: **Eingebettete Binärdateien hinzufügen** > **Andere hinzufügen...**. Navigieren Sie zu Ihrem Basisverzeichnis, und wählen Sie die Datei `MicrosoftCognitiveServicesSpeech.framework` aus. Dadurch wird auch automatisch die SDK-Bibliothek dem Header **Verknüpftes Framework und Bibliotheken** hinzugefügt.
        ![Hinzugefügtes Framework](media/sdk/qs-objectivec-framework.png)
        1. Navigieren Sie zur Registerkarte **Buildeinstellungen**, und aktivieren Sie **Alle** Einstellungen.
        1. Fügen Sie das Verzeichnis `$(SRCROOT)/..` den *Frameworksuchpfaden* unter der Überschrift **Suchpfade** hinzu.
        ![Einstellung „Frameworksuchpfad“](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Einrichten der Benutzeroberfläche

Die Benutzeroberfläche der Beispiel-App ist sehr einfach und besteht aus zwei Schaltflächen, um die Spracherkennung entweder mit einer Datei oder mit einer Mikrofoneingabe zu starten, und einer Beschriftung zum Anzeigen des Ergebnisses.
Die Benutzeroberfläche wird im `Main.storyboard`-Teil des Projekts eingerichtet.
Öffnen Sie die XML-Ansicht des Storyboards, indem Sie mit der rechten Maustaste auf den Eintrag `Main.storyboard` der Projektstruktur klicken und **Öffnen als...** > **Quellcode** auswählen.
Ersetzen Sie den automatisch generierten XML-Code durch Folgendes:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Laden Sie die [WAV-Beispieldatei](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) herunter, indem Sie mit der rechten Maustaste auf den Link klicken und dann **Ziel speichern unter...** auswählen. Fügen Sie die WAV-Datei dem Projekt als Ressource hinzu, indem Sie sie aus einem Suchfenster in die Stammebene der Projektansicht ziehen.
Klicken Sie im nachfolgenden Dialogfeld auf **Fertig stellen**, ohne die Einstellungen zu ändern.
1. Ersetzen Sie den Inhalt der automatisch generierten Datei `ViewController.m` durch Folgendes:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.
1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).
1. Fügen Sie die Anforderung für den Zugriff auf das Mikrofon hinzu. Klicken Sie mit der rechten Maustaste auf den Eintrag `Info.plist` der Projektstruktur, und klicken Sie anschließend auf **Öffnen als...** > **Quellcode**. Fügen Sie die folgenden Zeilen im Abschnitt `<dict>` hinzu, und speichern Sie die Datei dann.
    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="building-and-running-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Zeigen Sie die Debugausgabe an (**Ansicht** > **Debugbereich** > **Konsole aktivieren**).
1. Wählen Sie als Ziel für die App in der Liste des Menüs **Produkt** -> **Ziel** entweder den iOS-Simulator oder ein mit dem Entwicklungscomputer verbundenes iOS-Gerät aus.
1. Erstellen Sie den Beispielcode im iOS-Simulator, und führen Sie ihn aus, indem Sie **Produkt** -> **Ausführen** aus dem Menü auswählen oder auf die Schaltfläche **Wiedergeben** klicken.
Das Speech SDK unterstützt zurzeit nur 64-Bit-iOS-Plattformen.
1. Nachdem Sie in der App auf die Schaltfläche „Recognize (File)“ (Erkennen (Datei)) geklickt haben, sollte der Inhalt der Audiodatei „What's the weather like?“ (Wie ist das Wetter?) im unteren Teil des Bildschirms angezeigt werden.

 ![Simulierte iOS-App](media/sdk/qs-objectivec-simulated-app.png)

1. Nachdem Sie in der App auf die Schaltfläche „Recognize (Microphone)“ (Erkennen (Mikrofon)) geklickt und ein paar Worte gesagt haben, sollte der gesprochene Text im unteren Teil des Bildschirms angezeigt werden.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/objectivec-ios`.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen der Beispiele](speech-sdk.md#get-the-samples)

