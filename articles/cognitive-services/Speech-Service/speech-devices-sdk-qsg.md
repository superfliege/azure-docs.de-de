---
title: Erste Schritte mit dem SDK für sprachaktivierte Geräte
description: Voraussetzungen und Anweisungen für den Einstieg in das SDK für sprachaktivierte Geräte.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jerkin
ms.openlocfilehash: 266315a731eec8a2c0ab0a880ce9e1db58331184
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283135"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Erste Schritte mit dem SDK für sprachaktivierte Geräte

In diesem Artikel wird beschrieben, wie Sie Ihren Entwicklungs-PC und Ihr SDK für sprachaktivierte Geräte konfigurieren, um sprachaktivierte Geräte mit dem SDK für sprachaktivierte Geräte zu entwickeln. Anschließend erstellen Sie eine Beispielanwendung und stellen diese auf dem Gerät bereit. 

Der Quellcode für die Beispielanwendung liegt dem SDK für sprachaktivierte Geräte bei und ist auch [auf GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit der Entwicklung mit dem SDK für sprachaktivierte Geräte beginnen, sammeln Sie die Informationen und Software, die Sie benötigen.

* Beziehen Sie das SDK [von Roobo](http://ddk.roobo.com/). Die Kits sind mit linearen oder kreisförmigen Mikrofonarraykonfigurationen erhältlich; wählen Sie das richtige für Ihre Bedürfnisse aus.

    |Development Kit-Konfiguration|Sprecherstandort|
    |-----------------------------|------------|
    |Kreisförmig|In beliebiger Richtung vom Gerät|
    |Linear|Vor dem Gerät|

* Holen Sie sich die neueste Version des SDKs für sprachaktivierte Geräte, einschließlich einer Android-Beispielanwendung, von der [Downloadwebsite](https://shares.datatransfer.microsoft.com/) des SDKs für sprachaktivierte Geräte. Extrahieren Sie die ZIP-Datei in einen lokalen Ordner (z. B. `C:\SDSDK`).

* Installieren Sie [Android Studio](https://developer.android.com/studio/) und [Vysor](http://vysor.io/download/) auf Ihrem PC.

* Beziehen Sie sich einen [Abonnementschlüssel](get-started.md) eines Spracherkennungsdiensts. Sie können eine kostenlose 30-Tage-Testversion oder einen Schlüssel von Ihrem Azure-Dashboard erhalten.

* Wenn Sie die Absichtserkennung des Spracherkennungsdiensts nutzen möchten, abonnieren Sie den [Language Understanding Intelligent Service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) und [rufen Sie einen Abonnementschlüssel ab](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/azureibizasubscription). 

    Sie können [ein einfaches LUIS-Modell](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/) erstellen oder das LUIS-Beispielmodell `LUIS-example.json` verwenden, das über die [Downloadwebsite](https://shares.datatransfer.microsoft.com/) des SDKs für sprachaktivierte Geräte verfügbar ist. Laden Sie die JSON-Datei Ihres Modells in das [LUIS-Portal](https://www.luis.ai/home) hoch, indem Sie auf **Neue App importieren** klicken und die JSON-Datei auswählen.

## <a name="set-up-the-development-kit"></a>Einrichten des Development Kits

1. Schließen Sie das Netzteil des Development Kits an. Eine grüne Stromanzeige sollte unter dem oberen Board aufleuchten.

1. Schließen Sie das Development Kit mit einem Mini-USB-Kabel an einen Computer an.

    ![Anschließen eines Development Kits](media/speech-devices-sdk/qsg-1.jpg)

1. Richten Sie Ihr Development Kit ordnungsgemäß aus.

    |Development Kit-Konfiguration|Ausrichtung|
    |-----------------------------|------------|
    |Kreisförmig|Aufrecht, Mikrofone zur Decke gerichtet|
    |Linear|Auf der Seite, Mikrofone zu Ihnen gerichtet (siehe unten)|

    ![lineare Ausrichtung des Developer Kits](media/speech-devices-sdk/qsg-2.jpg)

1. Installieren Sie die Zertifikate und die Weckwort-Tabellendatei (Schlüsselwort), und legen Sie die Berechtigungen für das Soundgerät fest. Geben Sie die folgenden Befehle in einem Befehlsfenster ein.

    > [!NOTE]
    > Diese Befehle verwenden die Android Debug Bridge, `adb.exe`, die Teil der Android Studio-Installation ist. Das Tool befindet sich unter `C:\Users\[user name]\AppData\Local\Android\Sdk\platform-tools`. Sie können dieses Verzeichnis zu Ihrem Pfad hinzufügen, um es einfacher zu machen, `adb` aufzurufen. Andernfalls müssen Sie den vollständigen Pfad zu Ihrer Installation von `adb.exe` in jedem Befehl angeben, der `adb` aufruft.

    ```
    adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
    adb shell
    cd /data/ 
    chmod 777 roobo_setup.sh
    ./roobo_setup.sh
    exit
    ```

    > [!TIP]
    > Schalten Sie Mikrofon und Lautsprecher Ihres PCs stumm. Auf diese Weise können Sie sicher sein, dass Sie mit den Mikrofonen des Development Kits arbeiten und das Gerät nicht versehentlich mit Audio vom PC auslösen.
    
1.  Starten Sie Vysor auf Ihrem Computer.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  Ihr Gerät sollte unter „Gerät auswählen“ aufgeführt sein. Klicken Sie auf die Schaltfläche **Anzeigen** neben dem Gerät. 
 
1.  Stellen Sie die Verbindung mit Ihrem drahtlosen Netzwerk her, indem Sie auf **Einstellungen** und dann auf **WLAN** klicken.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)
 
## <a name="run-a-sample-application"></a>Ausführen einer Beispielanwendung

Um die Roobo-Tests durchzuführen und das Setup Ihres Development Kits zu überprüfen, erstellen und installieren Sie die Beispielanwendung.

1.  Starten Sie Android Studio.

1.  Wählen Sie die Option zum Öffnen eines vorhandenen Android Studio-Projekts aus.

    ![Android Studio, vorhandenes Projekt öffnen](media/speech-devices-sdk/qsg-5.png)
 
1.  Navigieren Sie zu `C:\SDSDK\Android-Sample-Release\example`, und klicken Sie dann auf **OK**, um das Beispielprojekt zu öffnen.
 
1.  Fügen Sie dem Quellcode den Abonnementschlüssel für Ihre Spracherkennung hinzu. Wenn Sie die Absichtserkennung ausprobieren möchten, fügen Sie auch Ihren Abonnementschlüssel für den [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) und die Anwendungs-ID hinzu. 

    Ihre Schlüssel und Anwendungsinformationen müssen in die folgenden Zeilen der Quelldatei `MainActivity.java` eingefügt werden.

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app id]"
    ```

1. Das standardmäßige Weckwort (Schlüsselwort) ist „Computer“.  Wenn Sie möchten, können Sie eines der anderen angebotenen Weckwörter „Maschine“ und „Assistent“ ausprobieren. Die Ressourcendateien für diese alternativen Wörter finden Sie im SDK für sprachaktivierte Geräte im Ordner „keyword“. Beispielsweise enthält `C:\SDSDK\Android-Sample-Release\keyword\Computer` die für „Computer“ verwendeten Dateien.

    Sie können auch [ein benutzerdefiniertes Weckwort erstellen](speech-devices-sdk-create-kws.md).

    So installieren Sie das gewünschte Weckwort:
 
    * Erstellen Sie einen Ordner `keyword` im Ordner „\data\“ auf dem Gerät, indem Sie die folgenden Befehle im Befehlsfenster ausführen.

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Kopieren Sie die Dateien `kws.table`, `kws_g.fst`, `kws_k.fst` und `words_kw.txt`) in den Ordner „\data\keyword\“ auf dem Gerät. Führen Sie die folgenden Befehle in einem Befehlsfenster aus.

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_g.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Verweisen Sie in der Beispielanwendung auf diese Dateien. Suchen Sie die folgenden Zeilen in `MainActivity.java`. Stellen Sie sicher, dass das angegebene Schlüsselwort das von Ihnen verwendete ist, und dass der Pfad auf die Datei `kws.table` zeigt, die Sie per Push an das Gerät gesendet haben.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > In Ihrem eigenen Code können Sie die Datei `kws.table` verwenden, um eine Schlüsselwortmodell-Instanz zu erstellen und die Erkennung wie folgt zu starten.
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Aktualisieren Sie die folgenden Zeilen mit den Geometrieeinstellungen des Mikrofonarrays.

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```

    |Variable|Bedeutung|Verfügbare Werte|
    |--------|-------|----------------|
    |`DeviceGeometry`|Konfiguration des physischen Mikrofons|`Circular6+1` für kreisförmiges Development Kit|
    ||| `Linear4` für lineares Development Kit|
    |`SelectedGeometry`|Konfiguration des Softwaremikrofons|`Circular6+1` für kreisförmiges Developer Kit unter Verwendung aller Mikrofone|
    |||`Circular3+1` für kreisförmiges Developer Kit unter Verwendung von vier Mikrofonen|
    |||`Linear4` für lineares Developer Kit unter Verwendung aller Mikrofone|
    |||`Linear2` für lineares Developer Kit unter Verwendung von zwei Mikrofonen|


1.  Erstellen Sie die Anwendung, indem Sie **App ausführen** aus dem Menü „Ausführen“ auswählen. Das Dialogfeld „Bereitstellungsziel auswählen“ wird angezeigt. Wählen Sie Ihr Gerät aus, und klicken Sie auf **OK**, um die Anwendung auf dem Gerät bereitzustellen.

    ![Bereitstellungsziel auswählen](media/speech-devices-sdk/qsg-7.png)
 
1.  Die Beispielanwendung des SDKs für sprachaktivierte Geräte wird gestartet und zeigt die folgenden Optionen an.

    ![Beispielanwendung für sprachaktivierte Geräte](media/speech-devices-sdk/qsg-8.png)

1. Experimentieren sie!

## <a name="troubleshooting"></a>Problembehandlung

Wenn bei der Verwendung des Spracherkennungsdiensts Zertifikatsfehler auftreten, stellen Sie sicher, dass Datum und Uhrzeit des Geräts korrekt sind.

Weitere Informationen zur Entwicklung finden Sie im Roobo-[Entwicklungshandbuch](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

Roobo stellt ein Tool zur Verfügung, das alle Audiodaten in den Flashspeicher aufnimmt, was bei der Fehlerbehebung von Audioproblemen helfen kann. Für jede Development Kit-Konfiguration wird eine Version des Tools bereitgestellt. Wählen Sie Ihr Gerät auf [der Roobo-Website](http://ddk.roobo.com/) aus, und klicken Sie dann auf den Link **ROOBO-Tools** unten auf der Seite.
