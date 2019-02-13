---
title: Überwachen der Gerätekonnektivität mit dem Azure IoT Central-Explorer
description: Überwachen Sie Gerätemeldungen, und beobachten Sie Änderungen bei Gerätezwillingen über die Befehlszeilenschnittstelle des IoT Central-Explorers.
author: viv-liu
ms.author: viviali
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: e510cfbd89ab8dcd8dccd9a8b95a49a057c9b54f
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824937"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Überwachen der Gerätekonnektivität mit dem Azure IoT Central-Explorer

*Dieses Thema gilt für Generatoren und Administratoren.*

Verwenden Sie die Befehlszeilenschnittstelle des IoT Central-Explorers, um Nachrichten anzuzeigen, die Ihre Geräte an IoT Central senden, und Änderungen im IoT Hub-Zwilling zu beobachten. Mithilfe dieses Open Source-Tools können Sie tiefere Einblicke in den Status der Gerätekonnektivität gewinnen und Probleme von Gerätemeldungen diagnostizieren, die die Cloud oder Geräte nicht erreichen, die auf Änderungen beim Zwilling nicht reagieren.

## <a name="visit-the-iotc-explorer-repo-in-githubhttpsakamsiotciotcexplorercligithub"></a>[Das iot-explorer-Repository in GitHub besuchen](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Voraussetzungen
+ Node.js, Version 8.x oder höher – https://nodejs.org
+ Sie müssen einen Administrator für Ihre App bitten, ein Zugriffstoken für Sie zu generieren, das in iotc-explorer verwendet werden soll.

## <a name="installing-iotc-explorer"></a>Installieren von iotc-explorer

Führen Sie den folgenden Befehl an der Befehlszeile zum Installieren aus:

```
npm install -g iotc-explorer
```

> [!NOTE]
> Normalerweise müssen Sie in Unix-ähnlichen Umgebungen den Befehl "install" mit `sudo` ausführen.

## <a name="running-iotc-explorer"></a>Ausführen von iotc-explorer

Nachfolgend sind einige Befehle und allgemeine Optionen aufgelistet, die Sie bei Verwendung von `iotc-explorer` ausführen können. Wenn Sie die vollständige Sammlung von Befehlen und Optionen anzeigen möchten, können Sie `--help` an `iotc-explorer` oder einen seiner Unterbefehle übergeben.

### <a name="login"></a>Anmeldung

Bevor Sie beginnen können, müssen Sie einen Administrator für Ihre IoT Central-Anwendung bitten, ein Zugriffstoken für Sie zu generieren. Der Administrator führt die folgenden Schritte aus:
1. Wechseln Sie zu **Verwaltung/Zugriffstoken**. 
1. Klicken Sie auf **Generate (Generieren)**.
    ![Screenshot der Seite mit Zugriffstokens](media/howto-use-iotc-explorer-experimental/accesstokenspage.png)

1. Geben Sie einen Tokennamen ein, klicken Sie auf **Weiter**, und **kopieren Sie den Tokenwert**.
    > [!NOTE]
    > Weil der Tokenwert nur einmal angezeigt wird, muss er vor dem Schließen des Dialogfelds kopiert werden. Nach dem Schließen wird er nie wieder angezeigt.

    ![Screenshot des Dialogfelds zum Kopieren des Zugriffstokens](media/howto-use-iotc-explorer-experimental/copyaccesstoken.png)

Danach können Sie sich mit diesem Token an der Befehlszeilenschnittstelle anmelden, indem Sie Folgendes ausführen:

```sh
iotc-explorer login "<Token value>"
```

Wenn Sie das Token lieber nicht in Ihrem Shell-Verlauf beibehalten möchten, können Sie es weglassen und stattdessen bei der folgenden Aufforderung eingeben:

```
iotc-explorer login
```

### <a name="monitor-device-messages"></a>Überwachen von Gerätemeldungen

Sie können die von einem bestimmten Gerät oder allen Geräten in Ihrer Anwendung ankommenden Meldungen mit dem Befehl `monitor-messages` überwachen. Dadurch wird ein Watcher gestartet, der neue Meldungen bei ihrem Eintreffen kontinuierlich ausgibt.

Um alle Geräte in Ihrer Anwendung zu überwachen, führen Sie den folgenden Befehl aus:

```
iotc-explorer monitor-messages
```

Ausgabe:

![Ausgabe des Befehls „monitor-messages“](media/howto-use-iotc-explorer-experimental/monitormessages.png)

Um ein bestimmtes Gerät zu überwachen, fügen Sie die Geräte-ID einfach am Ende des Befehls hinzu:

```
iotc-explorer monitor-messages <your-device-id>
```

Sie können auch dafür sorgen, dass der Befehl in einem computerfreundlicheren Format ausgegeben wird, indem Sie ihm die `--raw`-Option hinzufügen:

```
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Gerätezwilling abrufen

Mit dem Befehl `get-twin` können Sie die Inhalte des Zwillings für ein IoT Central-Gerät abrufen. Führen Sie zu diesem Zweck den folgenden Befehl aus:

```
iotc-explorer get-twin <your-device-id>
```

Ausgabe:

![Ausgabe des Befehls „get-twin“](media/howto-use-iotc-explorer-experimental/getdevicetwin.png)

So wie bei `monitor-messages` erhalten Sie eine computerfreundlichere Ausgabe, indem Sie die `--raw`-Option übergeben:

```
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie erfahren haben, wie der IoT Central-Explorer verwendet wird, erkunden Sie idealerweise als nächsten Schritt das [Verwalten von Geräten mit IoT Central](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
