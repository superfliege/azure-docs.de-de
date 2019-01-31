---
title: Erste Schritte mit dem Speech-Geräte-SDK – Speech Services
titleSuffix: Azure Cognitive Services
description: Voraussetzungen und Anweisungen für den Einstieg in das SDK für sprachaktivierte Geräte.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 85e250f3dee3dbfc9e09524d5f3977a143ee8bac
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213258"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Erste Schritte mit dem SDK für sprachaktivierte Geräte

In diesem Artikel wird beschrieben, wie Sie Ihren Entwicklungs-PC und Ihr Speech-Geräte-Development Kit konfigurieren, um sprachaktivierte Geräte mit dem Speech-Geräte-SDK zu entwickeln. Anschließend erstellen Sie eine Beispielanwendung und stellen diese auf dem Gerät bereit.

Der Quellcode für die Beispielanwendung liegt dem Speech-Geräte-SDK bei. Es ist auch auf [GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit der Entwicklung mit dem Speech-Geräte-SDK beginnen, sammeln Sie die Informationen und Software, die Sie benötigen:

* Beziehen Sie das [Development Kit von ROOBO](http://ddk.roobo.com/). Die Kits sind mit linearen oder kreisförmigen Mikrofonarraykonfigurationen erhältlich. Wählen Sie die richtige Konfiguration für Ihre Anforderungen aus.

    |Development Kit-Konfiguration|Sprecherstandort|
    |-----------------------------|------------|
    |Kreisförmig|In beliebiger Richtung vom Gerät|
    |Linear|Vor dem Gerät|

* Holen Sie sich die neueste Version des Speech-Geräte-SDK, einschließlich einer Android-Beispiel-App, von der [Downloadwebsite](https://shares.datatransfer.microsoft.com/) des Speech-Geräte-SDK. Extrahieren Sie die ZIP-Datei in einen lokalen Ordner, z.B. „C:\SDSDK“.

* Installieren Sie [Android Studio](https://developer.android.com/studio/) und [Vysor](http://vysor.io/download/) auf Ihrem PC.

* Rufen Sie einen [Abonnementschlüssel für den Speech-Dienst](get-started.md) ab. Sie können eine kostenlose 30-Tage-Testversion oder einen Schlüssel von Ihrem Azure-Dashboard erhalten.

* Wenn Sie die Absichtserkennung des Speech-Diensts nutzen möchten, abonnieren Sie den [Language Understanding Intelligent Service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) und [rufen Sie einen Abonnementschlüssel ab](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription).

    Sie können [ein einfaches LUIS-Modell erstellen](https://docs.microsoft.com/azure/cognitive-services/luis/) oder das LUIS-Beispielmodell „LUIS-example.json“ verwenden. Das LUIS-Beispielmodell ist auf der [Downloadwebsite des Speech-Geräte-SDK](https://shares.datatransfer.microsoft.com/) verfügbar. Laden Sie die JSON-Datei Ihres Modells in das [LUIS-Portal](https://www.luis.ai/home) hoch, indem Sie **Neue App importieren** und dann die JSON-Datei auswählen.

## <a name="set-up-the-development-kit"></a>Einrichten des Development Kits

1. Schließen Sie das Development Kit mit einem Mini-USB-Kabel an einen Computer oder Netzteil an. Wenn das Kit verbunden ist, leuchtet eine grüne Betriebsanzeige unter dem obersten Board.

1. Schließen Sie das Development Kit mit einem zweiten Mini-USB-Kabel an einen Computer an.

    ![Anschließen eines Development Kits](media/speech-devices-sdk/qsg-1.png)

1. Richten Sie Ihr Development Kit für eine kreisförmige oder lineare Konfiguration aus.

    |Development Kit-Konfiguration|Ausrichtung|
    |-----------------------------|------------|
    |Kreisförmig|Aufrecht, Mikrofone zur Decke gerichtet|
    |Linear|Auf der Seite, Mikrofone zu Ihnen gerichtet (siehe folgende Abbildung)|

    ![Lineare Ausrichtung des Development Kits](media/speech-devices-sdk/qsg-2.png)

1. Installieren Sie die Zertifikate und die Weckwort-Tabellendatei (Schlüsselwort), und legen Sie die Berechtigungen für das Soundgerät fest. Geben Sie die folgenden Befehle in einem Eingabeaufforderungsfenster ein:

   ```
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Diese Befehle verwenden die Android Debug Bridge, `adb.exe`, die Teil der Android Studio-Installation ist. Dieses Tool befindet sich in „C:\Users\[Benutzername]\AppData\Local\Android\Sdk\platform-tools“. Sie können dieses Verzeichnis Ihrem Pfad hinzufügen, um das Aufrufen von `adb` zu vereinfachen. Andernfalls müssen Sie den vollständigen Pfad zu Ihrer Installation von „adb.exe“ in jedem Befehl angeben, der `adb` aufruft.

    > [!TIP]
    > Schalten Sie das Mikrofon und die Lautsprecher Ihres Computers stumm, um sicherzustellen, dass die Mikrofone des Development Kits verwendet werden. Auf diese Weise lösen Sie nicht versehentlich das Gerät mit Audio vom PC aus.

1.  Starten Sie Vysor auf Ihrem Computer.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  Ihr Gerät sollte unter **Gerät auswählen** aufgeführt sein. Wählen Sie die Schaltfläche **Anzeigen** neben dem Gerät aus.

1.  Stellen Sie eine Verbindung mit Ihrem drahtlosen Netzwerk her, indem Sie das Ordnersymbol und dann **Einstellungen** > **WLAN** auswählen.

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
    >
    > Wenn Sie einen Lautsprecher an das Development Kit anschließen möchten, verwenden Sie dazu den Audioausgang. Sie sollten außerdem einen hochwertigen 3,5-mm-Lautsprecher auswählen.
    >
    > ![Vysor-Audio](media/speech-devices-sdk/qsg-14.png)

## <a name="run-a-sample-application"></a>Ausführen einer Beispielanwendung

Um die ROOBO-Tests durchzuführen und das Setup Ihres Development Kits zu überprüfen, erstellen und installieren Sie die Beispielanwendung:

1.  Starten Sie Android Studio.

1.  Wählen Sie die Option zum **Öffnen eines vorhandenen Android Studio-Projekts** aus.

    ![Android Studio – Öffnen eines vorhandenen Projekts](media/speech-devices-sdk/qsg-5.png)

1.  Wechseln Sie zu „C:\SDSDK\Android-Sample-Release\example“. Wählen Sie **OK** aus, um das Beispielprojekt zu öffnen.

1.  Fügen Sie dem Quellcode den Abonnementschlüssel für Ihre Spracherkennung hinzu. Wenn Sie die Absichtserkennung ausprobieren möchten, fügen Sie auch Ihren Abonnementschlüssel für den [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) und die Anwendungs-ID hinzu.

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

    Sie können auch [ein benutzerdefiniertes Aktivierungswort erstellen](speech-devices-sdk-create-kws.md).

    So installieren Sie das Aktivierungswort, das Sie verwenden möchten

    * Erstellen Sie einen Ordner „keyword“ im Ordner „data“ auf dem Gerät, indem Sie die folgenden Befehle im Eingabeaufforderungsfenster ausführen:

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Kopieren Sie die Dateien „kws.table“, „kws_k.fst“ und „words_kw.txt“ in den Ordner „\data\keyword“ des Geräts. Führen Sie die folgenden Befehle in einem Eingabeaufforderungsfenster aus. Wenn Sie ein [benutzerdefiniertes Aktivierungswort](speech-devices-sdk-create-kws.md) erstellt haben, befindet sich die aus dem Web generierte Datei „kws.table“ im gleichen Verzeichnis wie die Dateien „kws.table“, „kws_k.fst“ und „words_kw.txt“. Verwenden Sie für ein benutzerdefiniertes Aktivierungswort den Befehl `adb push C:\SDSDK\Android-Sample-Release\keyword\[wake_word_name]\kws.table /data/keyword`, um die Datei „kws.table“ zum Development Kit zu pushen:

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```

    * Verweisen Sie in der Beispielanwendung auf diese Dateien. Suchen Sie die folgenden Zeilen in „MainActivity.java“. Stellen Sie sicher, dass das angegebene Schlüsselwort das von Ihnen verwendete ist, und dass der Pfad auf die Datei `kws.table` zeigt, die Sie an das Gerät gepusht haben.

        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > In eigenem Code können Sie die Datei „kws.table“ verwenden, um eine Schlüsselwortmodell-Instanz zu erstellen und die Erkennung zu starten:
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Aktualisieren Sie die folgenden Zeilen mit den Geometrieeinstellungen des Mikrofonarrays:

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```
    In der folgenden Tabelle werden die verfügbaren Werte beschrieben:

    |Variable|Bedeutung|Verfügbare Werte|
    |--------|-------|----------------|
    |`DeviceGeometry`|Konfiguration des physischen Mikrofons|Für kreisförmiges Development Kit: `Circular6+1` |
    |||Für lineares Development Kit: `Linear4`|
    |`SelectedGeometry`|Konfiguration des Softwaremikrofons|Für ein kreisförmiges Development Kit, das alle Mikrofone verwendet: `Circular6+1`|
    |||Für ein kreisförmiges Development Kit, das vier Mikrofone verwendet: `Circular3+1`|
    |||Für ein lineares Development Kit, das alle Mikrofone verwendet: `Linear4`|
    |||Für ein lineares Development Kit, das zwei Mikrofone verwendet: `Linear2`|


1.  Wählen Sie zum Erstellen der Anwendung im Menü **Ausführen** die Option **App ausführen** aus. Das Dialogfeld **Bereitstellungsziel auswählen** wird angezeigt.

1. Wählen Sie Ihr Gerät und dann **OK** aus, um die Anwendung auf dem Gerät bereitzustellen.

    ![Dialogfeld „Bereitstellungsziel auswählen“](media/speech-devices-sdk/qsg-7.png)

1.  Die Beispielanwendung des Speech-Geräte-SDK wird gestartet und zeigt die folgenden Optionen an:

    ![Speech-Geräte-SDK-Beispielanwendung und Optionen](media/speech-devices-sdk/qsg-8.png)

1. Experimentieren Sie.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="certificate-failures"></a>Zertifikatfehler

Wenn bei der Verwendung des Speech-Diensts Zertifikatfehler auftreten, stellen Sie sicher, dass Datum und Uhrzeit des Geräts korrekt sind:

1. Wechseln Sie zu **Einstellungen**. Wählen Sie unter **System** die Option **Datum und Uhrzeit** aus.

    ![Wählen Sie unter „Einstellungen“ die Option „Datum und Uhrzeit“ aus.](media/speech-devices-sdk/qsg-12.png)

1. Belassen Sie die Option **Automatische Datums- und Uhrzeiteinstellung** aktiviert. Wählen Sie unter **Zeitzone auswählen** die aktuelle Zeitzone aus.

    ![Auswählen der Optionen für Datum und Zeitzone](media/speech-devices-sdk/qsg-13.png)

    Wenn Sie sehen, dass die Zeit des Development Kits mit der Zeit Ihres PC übereinstimmt, ist das Development Kit mit dem Internet verbunden.

    Weitere Informationen zur Entwicklung finden Sie im [ROOBO-Entwicklungshandbuch](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

### <a name="audio"></a>Audio

ROOBO bietet ein Tool, das sämtliche Audiosignale im Flashspeicher erfasst. Es kann Ihnen beim Beheben von Audioproblemen helfen. Für jede Development Kit-Konfiguration wird eine Version des Tools bereitgestellt. Wählen Sie Ihr Gerät auf der [ROOBO-Website](http://ddk.roobo.com/) aus, und wählen Sie dann unten auf der Seite den Link **ROOBO-Tools** aus.
