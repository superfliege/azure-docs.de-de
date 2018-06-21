---
title: IoT DevKit als Übersetzer mithilfe von Azure Functions und Cognitive Services | Microsoft-Dokumentation
description: Verwenden eines Mikrofons mit IoT DevKit, um Sprachnachrichten zu empfangen, und Azure Cognitive Services, um die Nachrichten in die englische Sprache zu übersetzen
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: b65888ced67ba3c236c31678cf452d40489bf028
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631777"
---
# <a name="use-iot-devkit-az3166-with-azure-function-and-cognitive-services-to-make-a-language-translator"></a>Verwenden von IoT DevKit AZ3166 mit Azure Functions und Cognitive Services zum Erstellen eines Sprachübersetzers

In diesem Artikel erfahren Sie, wie Sie IoT DevKit mithilfe von [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) als Sprachübersetzer nutzen. Dabei wird Ihre Stimme aufgezeichnet und in englischen Text übersetzt, der auf dem DevKit-Bildschirm angezeigt wird.

Das [MXChip IoT DevKit](https://aka.ms/iot-devkit) ist ein mit Arduino kompatibles All-in-One-Board mit umfangreichen Peripheriegeräten und Sensoren. Sie können dafür mithilfe der [Visual Studio Code-Erweiterung für Arduino](https://aka.ms/arduino) entwickeln. Im Lieferumfang enthalten ist ein wachsender [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), der Sie durch das Erstellen eines Prototyps von Lösungen für das Internet der Dinge (Internet of Things, IoT) führt, die Microsoft Azure-Dienste nutzen.

## <a name="what-you-need"></a>Voraussetzungen

Befolgen Sie die Anweisungen im [Leitfaden zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started), um folgende Voraussetzungen zu erfüllen:

* Herstellen einer Wi-Fi-Verbindung mit dem DevKit
* Vorbereiten der Entwicklungsumgebung

Ein aktives Azure-Abonnement. Wenn Sie kein Abonnement haben, können Sie sich mit einer der beiden folgenden Methoden registrieren:

* [Kostenloses Microsoft Azure-Testkonto für 30 Tage](https://azure.microsoft.com/free/) aktivieren
* [Azure-Gutschrift](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) beanspruchen, wenn Sie MSDN- oder Visual Studio-Abonnent sind

## <a name="step-1-open-the-project-folder"></a>Schritt 1: Öffnen des Projektordners

### <a name="a-start-vs-code"></a>A. Starten von Visual Studio Code

- Stellen Sie sicher, dass DevKit nicht mit Ihrem Computer verbunden ist.
- Starten von Visual Studio Code
- Verbinden Sie das DevKit mit Ihrem Computer.

### <a name="b-open-the-arduino-examples-folder"></a>B: Öffnen des Ordners „Arduino-Beispiele“

Erweitern Sie auf der linken Seite **ARDUINO-BEISPIELE > Beispiele für MXCHIP AZ3166 > AzureIoT**, und wählen Sie **DevKitTranslator** aus. Daraufhin wird ein neues Visual Studio Code-Fenster mit dem Projektordner „DEVKITTRANSLATOR“ geöffnet.

![IoT DevKit-Beispiele](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

> [!NOTE]
> Sie können auch ein Beispiel über die Befehlspalette öffnen. Öffnen Sie mit der Tastenkombination `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) die Befehlspalette, geben Sie **Arduino** ein, suchen Sie dann nach **Arduino: Beispiele**, und wählen Sie sie aus.

## <a name="step-2-provision-azure-services"></a>Schritt 2: Bereitstellen von Azure-Diensten

Drücken Sie im Projektmappenfenster `Ctrl+P` (macOS: `Cmd+P`), und geben Sie dann `task cloud-provision` ein.

Im Visual Studio Code-Terminal werden Sie über eine interaktive Befehlszeile durch die Bereitstellung aller erforderlichen Azure-Dienste geführt:

![Cloudbereitstellungsaufgabe](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="step-3-deploy-azure-functions"></a>Schritt 3: Deploy Azure Functions (Bereitstellen von Azure Functions)

Drücken Sie `Ctrl+P` (macOS: `Cmd+P`), um `task cloud-deploy` auszuführen und den Azure Functions-Code bereitzustellen. Dieser Vorgang dauert in der Regel 2 bis 5 Minuten:

![Cloudbereitstellungsaufgabe](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

Nachdem Azure Functions bereitgestellt wurde, fügen Sie in der Datei „azure_config.h“ den Funktions-App-Namen ein. Sie können zum [Azure-Portal](https://portal.azure.com/) navigieren, um ihn zu suchen:

![Suchen des Namens der Azure-Funktions-App](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Wenn die Azure-Funktion nicht ordnungsgemäß funktioniert, finden Sie in diesem [FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function)-Abschnitt Hinweise zur Fehlerbehebung.

## <a name="step-4-build-and-upload-the-device-code"></a>Schritt 4: Erstellen und Hochladen des Gerätecodes

1. Drücken Sie `Ctrl+P` (macOS: `Cmd+P`), um `task config-device-connection` auszuführen.

2. Sie werden im Terminal gefragt, ob Sie die Verbindungszeichenfolge verwenden möchten, die aus dem Schritt `task cloud-provision` abgerufen wird. Sie können auch eine eigene Geräte-Verbindungszeichenfolge eingeben, indem Sie **Neu erstellen** auswählen.

3. Das Terminal fordert Sie zum Wechseln in den Konfigurationsmodus auf. Halten Sie hierfür Taste A gedrückt, und drücken Sie anschließend gleichzeitig einmal die Reset-Taste. Auf dem Bildschirm werden die DevKit-ID und „Konfiguration“ angezeigt.
  ![Überprüfen und Hochladen des Arduino-Sketches](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. Sobald `task config-device-connection` abgeschlossen ist, klicken Sie auf `F1`, um Visual Studio Code-Befehle zu laden, und wählen Sie `Arduino: Upload` aus. Anschließend beginnt Visual Studio Code mit dem Überprüfen und Hochladen des Arduino-Sketches: ![Überprüfen und Hochladen des Arduino-Sketches](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

Das DevKit startet neu und beginnt mit der Ausführung des Codes.

## <a name="test-the-project"></a>Testen des Projekts

Befolgen Sie nach der App-Initialisierung die Anweisungen auf dem DevKit-Bildschirm. Die Standardquellsprache ist Chinesisch.

So wählen Sie eine andere Sprache für die Übersetzung aus

1. Drücken Sie Taste A, um in den Setupmodus zu gelangen.
2. Drücken Sie Taste B, um durch alle unterstützten Quellsprachen zu scrollen.
3. Drücken Sie Taste A, um Ihre Auswahl der Quellsprache zu bestätigen.
4. Halten Sie Taste B beim Sprechen gedrückt, und lassen Sie Taste B dann los, um die Übersetzung auszulösen.
5. Der übersetzte Text wird in englischer Sprache auf dem Bildschirm angezeigt.

![Scrollen zum Auswählen der Sprache](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Übersetzungsergebnis](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

Auf dem Bildschirm mit dem Übersetzungsergebnis haben Sie folgende Möglichkeiten:

- Drücken Sie die Tasten A und B, um durch die Quellsprachen zu scrollen und eine auszuwählen.
- Drücken Sie Taste B, um zu sprechen, und lassen Sie sie los, um die Stimme zu senden und den Übersetzungstext abzurufen.

## <a name="how-it-works"></a>So funktioniert's

![mini-solution-voice-to-tweet-diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

Der Arduino-Sketch zeichnet Ihre Stimme auf und sendet dann eine HTTP-Anforderung zum Auslösen von Azure Functions. Azure Functions ruft die Sprachübersetzer-API von Cognitive Services für die Übersetzung auf. Nachdem Azure Functions den Übersetzungstext abgerufen hat, wird eine C2D-Nachricht an das Gerät gesendet. Anschließend wird die Übersetzung auf dem Bildschirm angezeigt.

## <a name="change-device-id"></a>Ändern der Geräte-ID

Die in Azure IoT Hub registrierte standardmäßige Geräte-ID lautet **AZ3166**. Wenn Sie diese ändern möchten, folgen Sie den [hier angegebenen Anweisungen](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

Nun können Sie IoT DevKit mithilfe von Azure Functions und Cognitive Services als Übersetzer verwenden. In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Verwenden einer Visual Studio Code-Aufgabe zum Automatisieren von Cloudbereitstellungen
> * Konfigurieren einer Verbindungszeichenfolge für Azure IoT-Geräte
> * Bereitstellen von Azure Functions
> * Testen der Übersetzung der Sprachnachricht

Wechseln Sie zu den anderen Tutorials, um Folgendes zu lernen:

> [!div class="nextstepaction"]
> [Verbinden von IoT DevKit AZ3166 mit dem Solution Accelerator für die Azure IoT-Remoteüberwachung](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
