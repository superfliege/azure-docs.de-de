---
title: 'Schnellstart: Erkennen von Sprache, Java (Windows, Linux) – Speech-Dienste'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie eine einfache Java-Anwendung erstellen, mit der die Sprache des Benutzers über das Mikrofon Ihres Computers erfasst und transkribiert wird.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: fmegen
ms.openlocfilehash: 83d6c430d0973d4b45fcfc40bbb93ae283d1133d
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447581"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-java"></a>Schnellstart: Erkennen von Sprache mit dem Speech-SDK für Java

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erstellen Sie eine Java-Konsolenanwendung mit dem [Speech SDK](speech-sdk.md). Sie transkribieren über das Mikrofon Ihres Computers Sprache in Echtzeit in Text. Die Anwendung wird mit dem Maven-Paket für das Speech SDK und der Eclipse Java-IDE (v4.8) unter Windows (64 Bit) oder Ubuntu Linux 16.04/18.04 (64 Bit) erstellt. Sie wird in einer 64-Bit Java 8-JRE (Runtime Environment) ausgeführt.

> [!NOTE]
> Weitere Informationen zum SDK für sprachaktivierte Geräte und zum Roobo-Gerät finden Sie unter [SDK für sprachaktivierte Geräte](speech-devices-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Betriebssystem: Windows (64 Bit) oder Ubuntu Linux 16.04/18.04 (64 Bit)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) oder [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier können Sie den Schlüssel kostenlos erhalten](get-started.md).

Stellen Sie beim Ausführen von Ubuntu 16.04/18.04 sicher, dass diese Abhängigkeiten installiert sind, bevor Sie Eclipse starten.

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

Wenn Sie Windows (64-Bit) ausführen, stellen Sie sicher, dass Sie Microsoft Visual C++ Redistributable für Ihre Plattform installiert haben.
* [Microsoft Visual C++ Redistributable für Visual Studio 2017 herunterladen](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)


## <a name="create-and-configure-project"></a>Erstellen und Konfigurieren des Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Um Ihrem Java-Projekt eine neue leere Klasse hinzuzufügen, wählen Sie **Datei** > **Neu** > **Klasse** aus.

1. Geben Sie im Fenster **Neue Java-Klasse** **speechsdk.quickstart** in das Feld **Paket** und **Main** in das Feld **Name** ein.

   ![Screenshot des Fensters „Neue Java-Klasse“](media/sdk/qs-java-jre-06-create-main-java.png)

1. Ersetzen Sie den Code in `Main.java` mit dem folgenden Ausschnitt:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Drücken Sie F11, oder wählen Sie **Ausführen** > **Debuggen** aus.
Die nächsten 15 Sekunden der Spracheingabe vom Mikrofon werden erkannt und im Konsolenfenster protokolliert.

![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, z.B. das Auslesen von Sprache aus einer Audiodatei, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [Java-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Schnellstart: Übersetzen von Sprache, Java (Windows, Linux)](quickstart-translate-speech-java-jre.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
