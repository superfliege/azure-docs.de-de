---
title: Abrufen einer Twitter-Meldung mit Azure Functions | Microsoft-Dokumentation
description: Verwenden Sie den Bewegungssensor zum Erkennen von Erschütterungen, und suchen Sie mit Azure Functions einen zufälligen Tweet zu einem festgelegten Hashtag.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/07/2018
ms.author: liydu
ms.openlocfilehash: 0c3df0642d36aa1c4471c433fe9c1d3fa3b0880d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631760"
---
# <a name="shake-shake-for-a-tweet----retrieve-a-twitter-message-with-azure-functions"></a>Nutzung des Bewegungssensors zum Abrufen einer Twitter-Nachricht mit Azure Functions

In diesem Projekt erfahren Sie, wie Sie den Bewegungssensor verwenden, um mithilfe von Azure Functions ein Ereignis auszulösen. Die App ruft einen zufälligen Tweet mit einem Hashtag (#) ab, das Sie in einem Arduino-Sketch konfigurieren. Der Tweet ist auf der DevKit-Anzeige zu sehen.

## <a name="what-you-need"></a>Voraussetzungen

Befolgen Sie die Anweisungen im [Leitfaden zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started), um folgende Voraussetzungen zu erfüllen:

* Stellen Sie eine Wi-Fi-Verbindung mit dem DevKit her.
* Bereiten Sie die Entwicklungsumgebung vor.

Ein aktives Azure-Abonnement. Wenn Sie kein Abonnement haben, können Sie sich mit einer der beiden folgenden Methoden registrieren:

* [Kostenloses Microsoft Azure-Testkonto für 30 Tage](https://azure.microsoft.com/free/) aktivieren
* [Azure-Gutschrift](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) beanspruchen, wenn Sie MSDN- oder Visual Studio-Abonnent sind

## <a name="open-the-project-folder"></a>Öffnen des Projektordners

### <a name="start-vs-code"></a>Starten von Visual Studio Code

- Stellen Sie sicher, dass DevKit **nicht** mit Ihrem Computer verbunden ist.
- Starten Sie Visual Studio Code.
- Verbinden Sie das DevKit mit Ihrem Computer.

> [!NOTE]
> Beim Start von VS Code wird möglicherweise eine Fehlermeldung angezeigt, dass die Arduino-IDE oder das zugehörige Boardpaket nicht gefunden werden kann. Wenn dieser Fehler auftritt, schließen Sie VS Code und starten Sie die Arduino-IDE erneut. VS Code sollte nun den Arduino-IDE-Pfad ordnungsgemäß erkennen.

### <a name="open-arduino-examples-folder"></a>Öffnen des Ordners „Arduino-Beispiele“

Erweitern Sie auf der linken Seite **ARDUINO-BEISPIELE**, wechseln Sie zu **Beispiele für MXCHIP AZ3166 > AzureIoT**, und wählen Sie **ShakeShake**. Daraufhin wird ein neues VS Code-Fenster mit einem Projektordner geöffnet.

![mini-solution-examples](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_examples.png)

> [!NOTE]
> Sie können auch ein Beispiel über die Befehlspalette öffnen. Öffnen Sie mit der Tastenkombination `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) die Befehlspalette, geben Sie **Arduino** ein, suchen Sie dann nach **Arduino: Beispiele**, und wählen Sie sie aus.

## <a name="provision-azure-services"></a>Bereitstellen von Azure-Diensten

Führen Sie im Projektmappenfenster Ihre Aufgabe über `Ctrl+P` (macOS: `Cmd+P`) durch Eingabe von `task cloud-provision` aus.

Im Visual Studio Code-Terminal werden Sie über eine interaktive Befehlszeile durch die Bereitstellung der erforderlichen Azure-Dienste geführt:

![Cloudbereitstellung](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-provision.png)

> [!NOTE]
> Falls die Seite beim Versuch der Anmeldung bei Azure im Ladevorgang hängen bleibt, helfen Ihnen die Informationen in diesem [FAQ-Schritt](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure).
 
## <a name="modify-the-hashtag"></a>Ändern des Hashtags

Öffnen Sie `ShakeShake.ino`, und suchen Sie nach dieser Codezeile:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

Ersetzen Sie die Zeichenfolge `iot` in den geschweiften Klammern durch Ihr bevorzugtes Hashtag. DevKit ruft später einen zufälligen Tweet ab, der das in diesem Schritt angegebene Hashtag enthält.

## <a name="deploy-azure-functions"></a>Deploy Azure Functions (Bereitstellen von Azure Functions)

Drücken Sie `Ctrl+P` (macOS: `Cmd+P`), um `task cloud-deploy` auszuführen und mit der Bereitstellung des Azure Functions-Codes zu beginnen:

![Cloudbereitstellung](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-deploy.png)

> [!NOTE]
> In einigen Fällen funktioniert Azure Functions nicht ordnungsgemäß. Sollte dieses Problem auftreten, finden Sie weitere Informationen in diesem [Schritt in den häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#compilation-error-for-azure-function).

## <a name="build-and-upload-the-device-code"></a>Erstellen und Hochladen des Gerätecodes

### <a name="windows"></a>Windows

1. Verwenden Sie `Ctrl+P` zum Ausführen von `task device-upload`.
2. Das Terminal fordert Sie zum Wechseln in den Konfigurationsmodus auf. Gehen Sie dazu wie folgt vor:

   * Halten Sie die Taste A gedrückt.
   * Drücken Sie gleichzeitig einmal die Reset-Taste.

3. Auf dem Bildschirm werden die DevKit-ID und „Konfiguration“ angezeigt.
4. Dies legt die Verbindungszeichenfolge fest, die aus dem `task cloud-provision`-Schritt abgerufen wird.
5. Anschließend startet VS Code die Überprüfung und den Upload des Arduino-Sketchs in Ihr DevKit: ![device-upload](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)
6. Das DevKit startet neu und beginnt mit der Ausführung des Codes.

> [!NOTE]
> Möglicherweise wird Ihnen folgende Meldung angezeigt: „Fehler: AZ3166: Unbekanntes Paket.“ Dieser Fehler tritt auf, wenn der Boardpaketindex nicht ordnungsgemäß aktualisiert wurde. Zur Behebung dieses Problems finden Sie weitere Informationen in diesem [Schritt in den häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

### <a name="macos"></a>macOS

1. Setzen Sie DevKit in den Konfigurationsmodus: Halten Sie hierfür Taste A gedrückt, und drücken Sie anschließend gleichzeitig einmal die Reset-Taste. Auf dem Bildschirm wird „Konfiguration“ angezeigt.
2. Verwenden Sie `Cmd+P` zum Ausführen von `task device-upload`, um die Verbindungszeichenfolge festzulegen, die im `task cloud-provision`-Schritt abgerufen wird.
3. Anschließend startet VS Code die Überprüfung und den Upload des Arduino-Sketchs in Ihr DevKit: ![device-upload](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)
4. Das DevKit startet neu und beginnt mit der Ausführung des Codes.

> [!NOTE]
> Möglicherweise wird Ihnen folgende Meldung angezeigt: „Fehler: AZ3166: Unbekanntes Paket.“ Dieser Fehler tritt auf, wenn der Boardpaketindex nicht ordnungsgemäß aktualisiert wurde. Zur Behebung dieses Problems finden Sie weitere Informationen in diesem [Schritt in den häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Testen des Projekts

Drücken Sie nach der App-Initialisierung einmalig die Taste A, und schütteln Sie dann vorsichtig das DevKit-Board. Durch diese Aktion wird ein zufälliger Tweet abgerufen, der das zuvor von Ihnen angegebene Hashtag enthält. Innerhalb weniger Sekunden ist ein Tweet auf der DevKit-Anzeige zu sehen:

### <a name="arduino-application-initializing"></a>Arduino-Anwendung wird initialisiert...
![Arduino-Anwendung wird initialisiert.](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-1.png)

### <a name="press-a-to-shake"></a>Drücken der Taste A, um zu schütteln...
![Drücken der Taste A, um zu schütteln.](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-2.png)

### <a name="ready-to-shake"></a>Bereit zum Schütteln...
![Bereit zum Schütteln](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-3.png)

### <a name="processing"></a>Wird verarbeitet...
![Verarbeitung](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-4.png)

### <a name="press-b-to-read"></a>Drücken der Taste B, um zu lesen...
![Drücken der Taste B, um zu lesen.](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-5.png)

### <a name="display-a-random-tweet"></a>Anzeige eines zufälligen Tweets...
![Anzeige eines zufälligen Tweets](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-6.png)

- Drücken Sie erneut A, und schütteln Sie das Board vorsichtig für einen neuen Tweet.
- Drücken Sie B, um durch den restlichen Tweet zu scrollen.

## <a name="how-it-works"></a>So funktioniert's

![Diagramm](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/diagram.png)

Der Arduino-Sketch sendet ein Ereignis an das Azure IoT Hub. Dieses Ereignis löst die Azure Functions-App aus. Die Azure Functions-App enthält die Logik zum Verbinden mit der Twitter-API und Abrufen eines Tweets. Der Text wird dann in einer C2D-Nachricht (Cloud-zu-Gerät) umschlossen und zurück an das Gerät gesendet.

## <a name="optional-use-your-own-twitter-bearer-token"></a>Optional: Verwenden eines eigenen Twitter-Bearertokens

Zu Testzwecken wird in diesem Projekt ein vorkonfiguriertes Twitter-Bearertoken verwendet. Es gibt jedoch ein [Ratenlimit](https://dev.twitter.com/rest/reference/get/search/tweets) für jedes Twitter-Konto. Wenn Sie die Verwendung eines eigenen Tokens in Betracht ziehen, führen Sie die folgenden Schritte aus:

1. Öffnen Sie das [Twitter-Entwicklerportal](https://dev.twitter.com/), um eine neue Twitter-App zu registrieren.

2. [Holen Sie sich Verbraucherschlüssel und -geheimnis](https://support.yapsody.com/hc/en-us/articles/203068116-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-) für Ihre App.

3. Verwenden Sie ein [Dienstprogramm](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/), um aus diesen beiden Schlüsseln ein Twitter-Bearertoken zu generieren.

4. Wechseln Sie im [Azure-Portal](https://portal.azure.com/) {:target="_blank"} in die **Ressourcengruppe**, und suchen Sie die Azure-Funktion (Typ: App-Dienst) für Ihr Bewegungssensorprojekt. Der Name enthält immer die Zeichenfolge „shake...“.
  ![azure-function](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function.png)

5. Aktualisieren Sie den Code für `run.csx` unter **Funktionen > shakeshake-cs** mit Ihrem eigenen Token:
  ```csharp
  ...
  string authHeader = "Bearer " + "[your own token]";
  ...
  ```
  ![Twitter-Token](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/twitter-token.png)

6. Speichern Sie die Datei, und klicken Sie auf **Ausführen**.


## <a name="problems-and-feedback"></a>Probleme und Feedback

### <a name="the-screen-displays-no-tweets-while-every-step-has-run-successfully"></a>Auf der Anzeige ist „Keine Tweets“ (No tweets) zu sehen, obwohl jeder Schritt erfolgreich verlaufen ist.

Dies tritt normalerweise zum ersten Mal auf, wenn Sie das Beispiel bereitstellen und ausführen, da die Funktions-App zwischen ein paar Sekunden und einer Minute für den Kaltstart der App benötigt. Oder, beim Ausführen des Codes treten einige Abweichungen auf, die einen Neustart der App verursachen. Wenn diese Bedingung eintritt, kann die App des Geräts ein Timeout für das Abrufen des Tweets erhalten. In diesem Fall können Sie eine oder beide dieser Methoden ausprobieren, um das Problem zu lösen:

1. Klicken Sie im DevKit auf die Schaltfläche zum Zurücksetzen, um die Geräte-App erneut auszuführen.

2. Suchen Sie im [Azure-Portal](https://portal.azure.com/) nach der Azure Functions-App, die Sie erstellt haben, und starten Sie diese neu: ![azure-function-restart](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function-restart.png)

### <a name="feedback"></a>Feedback

Wenn andere Probleme auftreten, lesen Sie die [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie die Verbindung eines DevKit-Geräts mit dem Solution Accelerator für die Azure IoT-Remoteüberwachung herstellen und einen Tweet abrufen, werden die folgenden nächsten Schritte empfohlen:

* [Solution Accelerator für die Azure IoT-Remoteüberwachung: Übersicht](https://docs.microsoft.com/azure/iot-suite/)
