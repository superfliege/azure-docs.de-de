---
title: Erstellen eines IoT DevKit als Übersetzer mithilfe von Azure Functions und Cognitive Services | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie das Mikrofon mit einem IoT DevKit verwenden, um eine Sprachnachricht zu empfangen, und die Nachricht anschließend mit Azure Cognitive Services in die englische Sprache übersetzen.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: cd67e612dd020ba600e33ac8baf77bc094d8afd3
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42144359"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>Verwenden von IoT DevKit AZ3166 mit Azure Functions und Cognitive Services zum Erstellen eines Sprachübersetzers

In diesem Artikel erfahren Sie, wie Sie IoT DevKit mithilfe von [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) als Sprachübersetzer nutzen. Dabei wird Ihre Stimme aufgezeichnet und in englischen Text übersetzt, der auf dem DevKit-Bildschirm angezeigt wird.

Das [MXChip IoT DevKit](https://aka.ms/iot-devkit) ist ein mit Arduino kompatibles All-in-One-Board mit umfangreichen Peripheriegeräten und Sensoren. Sie können dafür mithilfe der [Visual Studio Code-Erweiterung für Arduino](https://aka.ms/arduino) entwickeln. Im Lieferumfang enthalten ist ein wachsender [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), der Sie durch das Erstellen eines Prototyps von Lösungen für das Internet der Dinge (Internet of Things, IoT) führt, die Microsoft Azure-Dienste nutzen.

## <a name="what-you-need"></a>Voraussetzungen

Befolgen Sie die Anweisungen im [Leitfaden zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started), um folgende Voraussetzungen zu erfüllen:

* Herstellen einer Wi-Fi-Verbindung mit dem DevKit
* Vorbereiten der Entwicklungsumgebung

Ein aktives Azure-Abonnement. Wenn Sie kein Abonnement haben, können Sie sich mit einer der beiden folgenden Methoden registrieren:

* [Kostenloses Microsoft Azure-Testkonto für 30 Tage](https://azure.microsoft.com/free/) aktivieren
* [Azure-Gutschrift](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) beanspruchen, wenn Sie MSDN- oder Visual Studio-Abonnent sind

## <a name="open-the-project-folder"></a>Öffnen des Projektordners

Öffnen Sie zuerst den Projektordner. 

### <a name="start-vs-code"></a>Starten von Visual Studio Code

- Stellen Sie sicher, dass Ihr DevKit mit Ihrem Computer verbunden ist.

- Starten Sie Visual Studio Code.

- Verbinden Sie das DevKit mit Ihrem Computer.

### <a name="open-the-arduino-examples-folder"></a>Öffnen des Ordners „Arduino-Beispiele“

Erweitern Sie auf der linken Seite **ARDUINO-BEISPIELE > Beispiele für MXCHIP AZ3166 > AzureIoT**, und wählen Sie **DevKitTranslator** aus. Daraufhin wird ein neues VS Code-Fenster mit dem Projektordner geöffnet. Wenn Sie die Beispiele für den Abschnitt „MXCHIP AZ3166“ nicht anzeigen können, stellen Sie sicher, dass Ihr Gerät ordnungsgemäß verbunden ist, und starten Sie VS Code neu.  

![IoT DevKit-Beispiele](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

Sie können das Beispiel auch über die Befehlspalette öffnen. Öffnen Sie mit der Tastenkombination `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) die Befehlspalette, geben Sie **Arduino** ein, suchen Sie dann nach **Arduino: Beispiele**, und wählen Sie sie aus.

## <a name="provision-azure-services"></a>Bereitstellen von Azure-Diensten

Drücken Sie im Projektmappenfenster `Ctrl+P` (macOS: `Cmd+P`), und geben Sie dann `task cloud-provision` ein.

Im Visual Studio Code-Terminal werden Sie über eine interaktive Befehlszeile durch die Bereitstellung aller erforderlichen Azure-Dienste geführt:

![Cloudbereitstellungsaufgabe](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="deploy-the-azure-function"></a>Bereitstellen der Azure-Funktions-App

Drücken Sie `Ctrl+P` (macOS: `Cmd+P`), um `task cloud-deploy` auszuführen und den Azure Functions-Code bereitzustellen. Dieser Vorgang dauert in der Regel 2 bis 5 Minuten.

![Cloudbereitstellungsaufgabe](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

Nachdem die Azure-Funktions-App bereitgestellt wurde, fügen Sie in der Datei „azure_config.h“ den Funktions-App-Namen ein. Sie können zum [Azure-Portal](https://portal.azure.com/) navigieren, um ihn zu suchen:

![Suchen des Namens der Azure-Funktions-App](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Wenn die Azure-Funktions-App nicht ordnungsgemäß funktioniert, lesen Sie zur Fehlerbehebung die [Seite „Komplikationsfehler bei Azure-Funktions-Apps“ in den häufig gestellten Fragen zum IoT DevKit ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function).

## <a name="build-and-upload-the-device-code"></a>Erstellen und Hochladen des Gerätecodes

1. Drücken Sie `Ctrl+P` (macOS: `Cmd+P`), um `task config-device-connection` auszuführen.

2. Sie werden im Terminal gefragt, ob Sie die Verbindungszeichenfolge verwenden möchten, die aus dem Schritt `task cloud-provision` abgerufen wird. Sie können auch eine eigene Geräte-Verbindungszeichenfolge eingeben, indem Sie **Neu erstellen** auswählen.

3. Das Terminal fordert Sie zum Wechseln in den Konfigurationsmodus auf. Halten Sie hierfür Taste A gedrückt, und drücken Sie anschließend gleichzeitig einmal die Reset-Taste. Auf dem Bildschirm werden die DevKit-ID und „Konfiguration“ angezeigt.

   ![Überprüfen und Hochladen des Arduino-Sketches](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. Sobald `task config-device-connection` abgeschlossen ist, klicken Sie auf `F1`, um VS Code-Befehle zu laden, und wählen Sie `Arduino: Upload` aus. Anschließend beginnt VS Code mit dem Überprüfen und Hochladen des Arduino-Sketches.

   ![Überprüfen und Hochladen des Arduino-Sketches](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

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

- Drücken Sie die Tasten „A“ und „B“, um durch die Quellsprachen zu scrollen und eine auszuwählen.

- Drücken Sie die Taste „B“, um zu sprechen. Lassen Sie sie los, um die Sprachnachricht zu senden und den Übersetzungstext abzurufen.

## <a name="how-it-works"></a>So funktioniert's

![mini-solution-voice-to-tweet-diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

Der Arduino-Sketch zeichnet Ihre Stimme auf und sendet dann eine HTTP-Anforderung zum Auslösen einer Azure-Funktion. Die Azure-Funktions-App ruft die Sprachübersetzer-API von Cognitive Services für die Übersetzung auf. Nachdem die Azure-Funktions-App den Übersetzungstext abgerufen hat, wird eine C2D-Nachricht (Cloud-to-Device) an das Gerät gesendet. Anschließend wird die Übersetzung auf dem Bildschirm angezeigt.

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [häufig gestellten Fragen zum IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfahren, wie Sie das IoT DevKit mithilfe von Azure Functions und Cognitive Services als Übersetzer verwenden können. In diesen Anweisungen wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden einer Visual Studio Code-Aufgabe zum Automatisieren von Cloudbereitstellungen
> * Konfigurieren einer Verbindungszeichenfolge für Azure IoT-Geräte
> * Bereitstellen der Azure-Funktions-App
> * Testen der Übersetzung der Sprachnachricht

Wechseln Sie zu den anderen Tutorials, um Folgendes zu lernen:

> [!div class="nextstepaction"]
> [Verbinden von IoT DevKit AZ3166 mit dem Solution Accelerator für die Azure IoT-Remoteüberwachung](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
