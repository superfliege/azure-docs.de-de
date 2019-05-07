---
title: 'Schnellstart: Ausführen des Speech Devices SDK unter Android – Speech-Dienste'
titleSuffix: Azure Cognitive Services
description: Voraussetzungen und Anweisungen für die ersten Schritte mit einem Android Speech Devices SDK.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: d5af2bb61eeb986f02a31d45ff9236ecc0c8427e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025738"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-android"></a>Schnellstart: Ausführen der Speech Devices SDK-Beispiel-App unter Android

In dieser Schnellstartanleitung erfahren Sie, wie Sie das Speech Devices SDK für Android verwenden, um ein sprachaktiviertes Produkt zu erstellen.

Für diese Anleitung wird ein [Azure Cognitive Services-Konto](get-started.md) mit einer Ressource für die Speech-Dienste benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/) einen Abonnementschlüssel abrufen.

Der Quellcode für die Beispielanwendung liegt dem Speech-Geräte-SDK bei. Es ist auch auf [GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie das Speech Devices SDK verwenden, sind folgende Schritte erforderlich:

* Befolgen Sie die Anweisungen im [Development Kit](get-speech-devices-sdk.md), um das Gerät zu einzuschalten.

* Laden Sie die aktuelle Version des [Speech Devices SDK](https://aka.ms/sdsdk-download) herunter, und extrahieren Sie die ZIP in Ihrem Arbeitsverzeichnis.
   > [!NOTE]
   > Die ZIP-Datei enthält die Android-Beispiel-App.

* Beziehen eines [Azure-Abonnementschlüssels für die Speech-Dienste](get-started.md)

* Wenn Sie die Speech-Dienste zum Identifizieren von Absichten (oder Aktionen) in Benutzeräußerungen verwenden möchten, benötigen Sie ein [LUIS-Abonnement (Language Understanding Intelligent Service)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription). Weitere Informationen zu LUIS und zur Absichtserkennung finden Sie unter [Tutorial: Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp).

    Sie können [ein einfaches LUIS-Modell erstellen](https://docs.microsoft.com/azure/cognitive-services/luis/) oder das LUIS-Beispielmodell „LUIS-example.json“ verwenden. Das LUIS-Beispielmodell ist auf der [Downloadwebsite des Speech-Geräte-SDK](https://aka.ms/sdsdk-luis) verfügbar. Laden Sie die JSON-Datei Ihres Modells in das [LUIS-Portal](https://www.luis.ai/home) hoch, indem Sie **Neue App importieren** und dann die JSON-Datei auswählen.

* Installieren Sie [Android Studio](https://developer.android.com/studio/) und [Vysor](https://vysor.io/download/) auf Ihrem PC.

## <a name="set-up-the-device"></a>Einrichten des Geräts

1. Starten Sie Vysor auf Ihrem Computer.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1. Ihr Gerät sollte unter **Gerät auswählen** aufgeführt sein. Wählen Sie die Schaltfläche **Anzeigen** neben dem Gerät aus.

1. Stellen Sie eine Verbindung mit Ihrem drahtlosen Netzwerk her, indem Sie das Ordnersymbol und dann **Einstellungen** > **WLAN** auswählen.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)

    > [!NOTE]
    > Wenn Ihr Unternehmen über Richtlinien in Bezug auf verbundene Geräte für das WLAN-System verfügt, müssen Sie die MAC-Adresse abrufen und sich an Ihre IT-Abteilung wende, damit es mit Ihrem WLAN-System verbunden wird.
    >
    > Zum Abrufen der MAC-Adresse des Development Kits wählen Sie das Ordnersymbol auf dem Desktop des Development Kits aus.
    >
    >  ![Vysor-Dateiordner](media/speech-devices-sdk/qsg-10.png)
    >
    > Wählen Sie **Settings**aus. Suchen Sie nach „Mac-Adresse“, und wählen Sie dann **Mac-Adresse** > **Erweitertes WLAN** aus. Notieren Sie sich die MAC-Adresse, die im unteren Bereich des Dialogfelds angezeigt wird.
    >
    > ![Vysor-MAC-Adresse](media/speech-devices-sdk/qsg-11.png)
    >
    > Einige Unternehmen haben eine zeitliche Begrenzung, wie lange ein Gerät mit den WLAN-Systemen verbunden sein darf. Möglicherweise müssen Sie die Registrierung des Development Kits mit Ihrem WLAN-System nach einer bestimmten Anzahl von Tagen verlängern.

## <a name="run-the-sample-application"></a>Ausführen der Beispielanwendung

Erstellen und installieren Sie zum Überprüfen des Setups Ihres Development Kits die Beispielanwendung:

1. Starten Sie Android Studio.

1. Wählen Sie die Option zum **Öffnen eines vorhandenen Android Studio-Projekts** aus.

   ![Android Studio – Öffnen eines vorhandenen Projekts](media/speech-devices-sdk/qsg-5.png)

1. Wechseln Sie zu „C:\SDSDK\Android-Sample-Release\example“. Wählen Sie **OK** aus, um das Beispielprojekt zu öffnen.

1. Fügen Sie dem Quellcode den Abonnementschlüssel für Ihre Spracherkennung hinzu. Wenn Sie die Absichtserkennung ausprobieren möchten, fügen Sie auch Ihren Abonnementschlüssel für den [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) und die Anwendungs-ID hinzu.

   Ihre Schlüssel und Anwendungsinformationen müssen in die folgenden Zeilen der Quelldatei „MainActivity.java“ eingefügt werden:

   ```java
   // Subscription
   private static final String SpeechSubscriptionKey = "[your speech key]";
   private static final String SpeechRegion = "westus";
   private static final String LuisSubscriptionKey = "[your LUIS key]";
   private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
   private static final String LuisAppId = "[your LUIS app ID]"
   ```

1. Das standardmäßige Aktivierungswort (Schlüsselwort) ist „Computer“. Auf Wunsch können Sie eines der anderen angebotenen Aktivierungswörter wie „Maschine“ oder „Assistent“ ausprobieren. Die Ressourcendateien für diese alternativen Aktivierungswörter finden Sie im Speech-Geräte-SDK im Ordner „keyword“. „C:\SDSDK\Android-Sample-Release\keyword\Computer“ enthält beispielsweise die Dateien, die für das Aktivierungswort „Computer“ verwendet werden.

   > [!TIP]
   > Sie können auch [ein benutzerdefiniertes Aktivierungswort erstellen](speech-devices-sdk-create-kws.md).

    Um ein neues Wort für die Aktivierung zu verwenden, aktualisieren Sie die folgenden zwei Codezeilen in `MainActivity.java`, und kopieren Sie das Paket mit dem Aktivierungswort in Ihre App. Wenn Sie beispielsweise das Aktivierungswort „Machine“ aus dem Aktivierungswortpaket „kws-machine.zip“ verwenden möchten, gehen Sie wie folgt vor:

   * Kopieren Sie das Aktivierungswortpaket in den Ordner „C:\SDSDK\Android-Sample-Release\example\app\src\main\assets\“.
   * Aktualisieren Sie die Datei `MainActivity.java` mit dem Schlüsselwort und dem Paketnamen:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

1. Aktualisieren Sie die folgenden Zeilen mit den Geometrieeinstellungen des Mikrofonarrays:

   ```java
   private static final String DeviceGeometry = "Circular6+1";
   private static final String SelectedGeometry = "Circular6+1";
   ```

   In der folgenden Tabelle sind die unterstützten Werte aufgeführt:

   |Variable|Bedeutung|Verfügbare Werte|
   |--------|-------|----------------|
   |`DeviceGeometry`|Konfiguration des physischen Mikrofons|Für kreisförmiges Development Kit: `Circular6+1` |
   |||Für lineares Development Kit: `Linear4`|
   |`SelectedGeometry`|Konfiguration des Softwaremikrofons|Für ein kreisförmiges Development Kit, das alle Mikrofone verwendet: `Circular6+1`|
   |||Für ein kreisförmiges Development Kit, das vier Mikrofone verwendet: `Circular3+1`|
   |||Für ein lineares Development Kit, das alle Mikrofone verwendet: `Linear4`|
   |||Für ein lineares Development Kit, das zwei Mikrofone verwendet: `Linear2`|

1. Wählen Sie zum Erstellen der Anwendung im Menü **Ausführen** die Option **App ausführen** aus. Das Dialogfeld **Bereitstellungsziel auswählen** wird angezeigt.

1. Wählen Sie Ihr Gerät und dann **OK** aus, um die Anwendung auf dem Gerät bereitzustellen.

    ![Dialogfeld „Bereitstellungsziel auswählen“](media/speech-devices-sdk/qsg-7.png)

1. Die Beispielanwendung des Speech-Geräte-SDK wird gestartet und zeigt die folgenden Optionen an:

   ![Speech-Geräte-SDK-Beispielanwendung und Optionen](media/speech-devices-sdk/qsg-8.png)

1. Experimentieren Sie.

## <a name="troubleshooting"></a>Problembehandlung

   Wenn Sie keine Verbindung mit dem Speech-Gerät herstellen können: Geben Sie den folgenden Befehl in ein Eingabeaufforderungsfenster ein. Daraufhin wird eine Liste mit Geräten zurückgegeben:

   ```powershell
    adb devices
   ```

   > [!NOTE]
   > Dieser Befehl verwendet Android Debug Bridge (`adb.exe`). Dieses Tool ist Teil der Android Studio-Installation. Dieses Tool befindet sich in „C:\Users\[Benutzername]\AppData\Local\Android\Sdk\platform-tools“. Sie können dieses Verzeichnis Ihrem Pfad hinzufügen, um das Aufrufen von `adb` zu vereinfachen. Andernfalls müssen Sie den vollständigen Pfad zu Ihrer Installation von „adb.exe“ in jedem Befehl angeben, der `adb` aufruft.
   >
   > Wenn die Fehlermeldung `no devices/emulators found` angezeigt wird, überprüfen Sie, ob das USB-Kabel angeschlossen ist und ein hochwertiges Kabel verwendet wird.
   >

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Lesen Sie die Anmerkungen zu dieser Version](devices-sdk-release-notes.md)
