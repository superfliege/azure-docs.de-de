---
title: Entwickeln von Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Informationen zum Erstellen von benutzerdefinierten Modulen für Azure IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: cb97e2cf6d554753f64afc76de84f43e38443909
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567229"
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules"></a>Grundlegendes zu den Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen

In diesem Artikel wird erläutert, welche Funktionalitäten beim Schreiben von Anwendungen, die als IoT Edge-Modul ausgeführt werden, verfügbar sind und wie sie genutzt werden.

## <a name="iot-edge-runtime-environment"></a>IoT Edge-Laufzeitumgebung
Die IoT Edge-Laufzeit bietet die Infrastruktur, um die Funktionalität mehrerer IoT Edge-Module zu integrieren und auf IoT Edge-Geräten bereitstellen. Theoretisch kann jedes Programm als IoT Edge-Modul verpackt werden. Um die Kommunikations- und Verwaltungsfunktionalitäten von IoT Edge jedoch umfassend zu nutzen, kann ein in einem Modul ausgeführtes Programm eine Verbindung mit dem lokalen IoT Edge-Hub, der in die IoT Edge-Laufzeit integriert ist, herstellen.

## <a name="using-the-iot-edge-hub"></a>Verwenden des IoT Edge-Hubs
Der IoT Edge-Hub bietet zwei Hauptfunktionalitäten: Proxy für IoT Hub und lokale Kommunikation.

### <a name="iot-hub-primitives"></a>IoT Hub-Primitive
IoT Hub behandelt eine Modulinstanz analog zu einem Gerät. Dies bedeutet Folgendes:

* Die Modulinstanz hat einen Modulzwilling, der vom [Gerätezwilling](../iot-hub/iot-hub-devguide-device-twins.md) und den anderen Modulzwillingen des Geräts getrennt und isoliert ist.
* Sie kann [Gerät-zu-Cloud-Nachrichten](../iot-hub/iot-hub-devguide-messaging.md) senden.
* Sie kann [direkte Methoden](../iot-hub/iot-hub-devguide-direct-methods.md) empfangen, die speziell an ihre Identität gerichtet sind.

Derzeit kann ein Modul keine C2D-Nachrichten empfangen und keine Dateien hochladen.

Wenn Sie ein Modul schreiben, können Sie das [Azure IoT-Geräte-SDK](../iot-hub/iot-hub-devguide-sdks.md) verwenden, um eine Verbindung mit dem IoT Edge-Hub herzustellen, und die oben beschriebene Funktionalität auf die gleiche Weise verwenden, wie IoT Hub für eine Geräteanwendung verwendet wird. Der einzige Unterschied ist, dass Sie vom Anwendungs-Back-End auf die Modulidentität statt auf die Geräteidentität verweisen müssen.

Ein Beispiel für eine Modulanwendung, die Gerät-zu-Cloud-Nachrichten sendet und den Modulzwilling verwendet, finden Sie unter [Entwickeln und Bereitstellen eines IoT Edge-Moduls auf einem simulierten Gerät](tutorial-csharp-module.md).

### <a name="device-to-cloud-messages"></a>D2C-Nachrichten
Um die komplexe Verarbeitung von D2C-Nachrichten zu ermöglichen, stellt der IoT Edge-Hub deklaratives Routing von Nachrichten zwischen Modulen und zwischen Modulen und IoT Hub bereit. Durch deklaratives Routing können Module Nachrichten abfangen und verarbeiten, die von anderen Modulen gesendet wurden, und in komplexen Pipelines verteilen. Im Artikel [Modulzusammensetzung](module-composition.md) wird erläutert, wie Module mithilfe von Routen in komplexen Pipelines kombiniert werden.

Ein IoT Edge-Modul kann im Gegensatz zu einer normalen IoT Hub-Geräteanwendung Gerät-zu-Cloud-Nachrichten empfangen, für deren Übertragung der lokale IoT Edge-Hub als Proxy fungiert, um sie zu verarbeiten.

Der IoT Edge-Hub verteilt die Nachrichten an Ihr Modul auf der Grundlage deklarativer Routen, die im Artikel [Modulzusammensetzung](module-composition.md) beschrieben werden. Beim Entwickeln eines IoT Edge-Moduls können Sie diese Nachrichten empfangen, indem Sie Meldungshandler festlegen.

Um die Erstellung von Routen zu vereinfachen, wird in IoT Edge das Konzept von *Eingangs*- und *Ausgangs*-Modulendpunkten eingeführt. Ein Modul kann alle an es weitergeleiteten D2C-Nachrichten empfangen, ohne einen Eingang anzugeben, und D2C-Nachrichten senden, ohne einen Ausgang anzugeben.
Die Verwendung expliziter Eingänge und Ausgänge erleichtert jedoch das Verständnis der Routingregeln. Weitere Informationen zu Routingregeln und Eingangs- und Ausgangsendpunkten für Module finden Sie unter [Modulzusammensetzung](module-composition.md).

Schließlich werden vom Edge-Hub behandelte D2C-Nachrichten mit den folgenden Eigenschaften gekennzeichnet:

| Eigenschaft | BESCHREIBUNG |
| -------- | ----------- |
| $connectionDeviceId | Die Geräte-ID des Clients, der die Nachricht gesendet hat. |
| $connectionModuleId | Die Modul-ID des Moduls, das die Nachricht gesendet hat. |
| $inputName | Der Eingang, an dem die Nachricht empfangen wurde. Kann leer sein. |
| $outputName | Der zum Senden der Nachricht verwendete Ausgang. Kann leer sein. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Herstellen einer Verbindung von einem Modul mit dem IoT Edge Hub
Die Herstellung einer Verbindung mit dem lokalen IoT Edge-Hub von einem Modul umfasst zwei Schritte: 
1. Verwendung der Verbindungszeichenfolge, die von der IoT Edge-Laufzeit bereitgestellt werden, wenn Ihr Modul gestartet wird.
2. Sicherstellen, dass Ihre Anwendung das vom IoT Edge-Hub auf diesem Gerät vorgelegte Zertifikat akzeptiert.

Die zu verwendende Verbindungszeichenfolge wird von der IoT Edge-Laufzeit in der Umgebungsvariablen `EdgeHubConnectionString` eingefügt. Dadurch ist sie für jedes Programm, das sie verwenden möchte, verfügbar.

Analog dazu wird das zum Überprüfen der Verbindung mit dem IoT Edge-Hub zu verwendende Zertifikat von der IoT Edge-Laufzeit in eine Datei eingefügt, deren Pfad in der Umgebungsvariablen `EdgeModuleCACertificateFile` verfügbar ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie ein Modul entwickelt haben, informieren Sie sich darüber, wie Sie [IoT Edge-Module bedarfsgerecht bereitstellen und überwachen](how-to-deploy-monitor.md).

