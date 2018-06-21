---
title: Entwickeln für eingeschränkte Geräte mit Azure IoT Hub | Microsoft-Dokumentation
description: Entwicklerhandbuch – Anleitungen zum Entwickeln mit Azure IoT-SDKs für eingeschränkte Geräte.
services: iot-hub
documentationcenter: c
author: yzhong94
manager: timlt
editor: ''
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2018
ms.author: yizhon
ms.openlocfilehash: 62065b78e3f8191c6423ba9dba4a8f7d16fad114
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659477"
---
# <a name="develop-for-constrained-devices-using-azure-iot-sdks"></a>Entwickeln für eingeschränkte Geräte mit Azure IoT-SDKs

## <a name="develop-using-azure-iot-hub-c-sdk"></a>Entwickeln mit dem Azure IoT Hub SDK für C
Das Azure IoT Hub SDK für C ist in ANSI C (C99) geschrieben, wodurch es sich gut für eine Vielzahl von Plattformen mit geringem Datenträger- und Arbeitsspeicherbedarf eignet.  Der empfohlene Arbeitsspeicher beträgt mindestens 64 KB. Der genaue Arbeitsspeicherbedarf hängt jedoch vom verwendeten Protokoll, der Anzahl der geöffneten Verbindungen sowie der Zielplattform ab.

Das SDK für C ist in Paketform über apt-get NuGet und MBED verfügbar.  Wenn Sie eingeschränkte Geräte zum Ziel haben, können Sie das SDK lokal für Ihre Zielplattform erstellen. In dieser Dokumentation wird veranschaulicht, wie mit [cmake][lnk-cmake] bestimmte Funktionen entfernt werden, um den Platzbedarf des SDK für C zu reduzieren.  Darüber hinaus werden in dieser Dokumentation die bewährten Programmiermodelle für die Arbeit mit eingeschränkten Geräten erläutert.

### <a name="building-the-c-sdk-for-constrained-devices"></a>Erstellen des SDK für C für eingeschränkte Geräte
#### <a name="prerequisites"></a>Voraussetzungen
Führen Sie die in diesem [Installationshandbuch][lnk-devbox-setup] beschriebenen Schritte aus, um Ihre Entwicklungsumgebung auf das Erstellen des SDK für C vorzubereiten.  Vor dem Erstellen mit „cmake“ können Sie cmake-Flags zum Entfernen nicht verwendeter Funktionen aufrufen.

#### <a name="remove-additional-protocol-libraries"></a>Entfernen zusätzlicher Protokollbibliotheken
Das SDK für C unterstützt derzeit fünf Protokolle: MQTT, MQTT über WebSocket, AMQPs, AMQP über WebSocket und HTTPS.    Bei den meisten Szenarien müssen auf einem Client ein bis zwei Protokolle ausgeführt werden. Daher können Sie die Protokollbibliothek, die Sie nicht verwenden, aus dem SDK entfernen.  Weitere Informationen zur Auswahl des entsprechenden Kommunikationsprotokolls für Ihr Szenario finden Sie in [diesem Dokument][lnk-choosing-protocol].  MQTT ist beispielsweise ein einfaches Protokoll, das für eingeschränkte Geräte oft besser geeignet ist.

Sie können die AMQP- und HTTP-Bibliothek mit dem folgenden cmake-Befehl entfernen:
```
cmake -Duse_amqp=OFF -Duse_http=OFF <Path_to_cmake>
```

#### <a name="remove-sdk-logging-capability"></a>Entfernen der SDK-Protokollierungsfunktion
Das SDK für C bietet insgesamt eine umfangreiche Protokollierung, um das Debuggen zu erleichtern. Sie können die Protokollierungsfunktion für Produktionsgeräte mit dem folgenden cmake-Befehl entfernen:
```
cmake -Dno_logging=OFF <Path_to_cmake>
```

#### <a name="remove-upload-to-blob-capability"></a>Entfernen der Funktion „In Blobspeicher hochladen“
Sie können umfangreiche Dateien mithilfe der integrierten Funktion im SDK in den Azure-Speicher hochladen.  Azure IoT Hub fungiert als Verteiler für ein zugeordnetes Azure-Speicherkonto.  Mit dieser Funktion können Sie Mediendateien, große Telemetriebatches und Protokolle senden.  Weitere Informationen zum Hochladen von Dateien mit IoT Hub finden Sie in [diesem Dokument][lnk-hub-file-upload].  Wenn diese Funktionalität für die Anwendung nicht erforderlich ist, können Sie diese Funktion mit dem folgenden cmake-Befehl entfernen:
```
cmake -Ddont_use_uploadtoblob=ON <Path_to_cmake>
```
#### <a name="running-strip-on-linux-environment"></a>Ausführen des strip-Befehls in Linux-Umgebung
Wenn die Binärdateien auf einem Linux-System ausgeführt werden, können Sie mit dem [strip-Befehl][lnk-strip] die Größe der endgültigen Anwendung nach dem Kompilieren reduzieren.
```
strip -s <Path_to_executable>
```

### <a name="programming-models-for-constrained-devices"></a>Programmiermodelle für eingeschränkte Geräte
#### <a name="avoid-using-the-serializer"></a>Vermeiden der Verwendung des Serialisierungsprogramms
Das SDK für C verfügt über ein optionales [Serialisierungsprogramm][lnk-serializer], mit dem Sie deklarative Zuordnungstabellen zum Definieren von Methoden und Eigenschaften von Gerätezwillingen verwenden können.  Das Serialisierungsprogramm soll die Entwicklung vereinfachen, verursacht aber zusätzlichen Aufwand, was für eingeschränkte Geräte nicht optimal ist.  Denken Sie in diesem Fall über die Verwendung von APIs eines einfachen Clients und das Analysieren von JSON mithilfe eines Lightweight-Parsers wie [parson][lnk-parson] nach.

#### <a name="use-the-lower-layer-ll"></a>Verwenden der niedrigeren Ebene (_Lower Layer, LL_)
Das SDK für C unterstützt zwei Programmiermodelle.  Eine Gruppe hat APIs mit dem Infix _LL_, das für die niedrigere Ebene steht.  Dieser Satz von APIs ist weniger umfangreich und erstellt keine Arbeitsthreads, sodass der Benutzer die Planung manuell steuern muss.  Die _LL_-APIs für den Geräteclient beispielsweise befinden sich in dieser [Headerdatei](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client_ll.h).  Eine andere Gruppe von APIs ohne den _LL_-Index wird als „Convenience Layer“ (Komfortebene) bezeichnet, auf der ein Arbeitsthread automatisch erstellt wird.  Die Convenience Layer-APIs für den Geräteclient beispielsweise befinden sich in dieser [Headerdatei](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client.h).  Bei eingeschränkten Geräten, bei denen jeder zusätzliche Thread einen erheblichen Prozentsatz der Systemressourcen belegen kann, sollten Sie die Verwendung von _LL_-APIs in Betracht ziehen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure IoT C SDK-Architektur:
-   [Azure IoT C SDK-Quellcode](https://github.com/Azure/azure-iot-sdk-c/)
-   [Einführung in das Azure IoT-Geräte-SDK für C](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-sdk-c-intro)

------
[lnk-cmake]: https://cmake.org/
[lnk-devbox-setup]:  https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-choosing-protocol]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols
[lnk-hub-file-upload]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-file-upload
[lnk-strip]: https://en.wikipedia.org/wiki/Strip_(Unix)
[lnk-serializer]: https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer
[lnk-parson]: https://github.com/kgabis/parson