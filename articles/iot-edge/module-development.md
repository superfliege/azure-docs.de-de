---
title: "Entwickeln von Modulen für Azure IoT Edge | Microsoft-Dokumentation"
description: "Informationen zum Erstellen von benutzerdefinierten Modulen für Azure IoT Edge"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 95b1d5d4e5e11f96b6abb17f0aeba935cc65512d
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules---preview"></a>Grundlegendes zu den Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen – Vorschau

In diesem Artikel wird erläutert, welche Funktionalitäten beim Schreiben von Anwendungen, die als IoT Edge-Modul ausgeführt werden, verfügbar sind und wie sie genutzt werden.

## <a name="iot-edge-runtime-environment"></a>IoT Edge-Laufzeitumgebung
Die IoT Edge-Laufzeit bietet die Infrastruktur, um die Funktionalität mehrerer IoT Edge-Module zu integrieren und auf IoT Edge-Geräten bereitstellen. Theoretisch kann jedes Programm als IoT Edge-Modul verpackt werden. Um die Kommunikations- und Verwaltungsfunktionalitäten von IoT Edge jedoch umfassend zu nutzen, kann ein in einem Modul ausgeführtes Programm eine Verbindung mit dem lokalen IoT Edge-Hub, der in die IoT Edge-Laufzeit integriert ist, herstellen.

## <a name="using-the-iot-edge-hub"></a>Verwenden des IoT Edge-Hubs
Der IoT Edge-Hub bietet zwei Hauptfunktionalitäten: Proxy für IoT Hub und lokale Kommunikation.

### <a name="iot-hub-primitives"></a>IoT Hub-Primitive
IoT Hub behandelt eine Modulinstanz analog zu einem Gerät. Dies bedeutet Folgendes:

* Die Modulinstanz hat einen Modulzwilling, der vom [Gerätezwilling][lnk-devicetwin] und den anderen Modulzwillingen des Geräts getrennt und isoliert ist.
* Sie kann [D2C-Nachrichten senden][lnk-iothub-messaging].
* Sie kann [direkte Methoden][lnk-methods] empfangen, die speziell an ihre Identität gerichtet sind.

Derzeit kann ein Modul keine C2D-Nachrichten empfangen und keine Dateien hochladen.

Wenn Sie ein Modul schreiben, können Sie einfach das [Azure-IoT-Geräte-SDK][lnk-devicesdk] verwenden, um eine Verbindung mit dem IoT Edge-Hub herzustellen, und die oben beschriebene Funktionalität auf die gleiche Weise verwenden, wie IoT Hub für eine Geräteanwendung verwendet wird. Der einzige Unterschied ist, dass Sie vom Anwendungs-Back-End auf die Modulidentität statt auf die Geräteidentität verweisen müssen.

Ein Beispiel für eine Modulanwendung, die D2C-Nachrichten sendet und den Modulzwilling verwendet, finden Sie unter [Develop and deploy an IoT Edge module to a simulated device][lnk-tutorial2] (Entwickeln und Bereitstellen eines IoT Edge-Moduls auf einem simulierten Gerät, in englischer Sprache)

### <a name="device-to-cloud-messages"></a>D2C-Nachrichten
Um die komplexe Verarbeitung von D2C-Nachrichten zu ermöglichen, stellt der IoT Edge-Hub deklaratives Routing von Nachrichten zwischen Modulen und zwischen Modulen und IoT Hub bereit.
So können Module Nachrichten abfangen und verarbeiten, die von anderen Modulen gesendet wurden, und in komplexen Pipelines verteilen.
Im Artikel [Understand how IoT Edge modules can be used, configured, and reused - preview] [ lnk-module-comp] (Grundlegendes zur Verwendung, Konfiguration und Wiederverwendung von IoT Edge-Modulen – Vorschau, in englischer Sprache) wird erläutert, wie Module mithilfe von Routen zu komplexen Pipelines kombiniert werden.

Ein IoT Edge-Modul kann im Gegensatz zu einer normalen IoT Hub-Geräteanwendung D2C-Nachrichten empfangen, für deren Übertragung der lokale IoT Edge-Hub als Proxy fungiert, um sie zu verarbeiten.

Der IoT Edge-Hub verteilt die Nachrichten auf der Grundlage deklarativer Routen, die im Artikel [Understand how IoT Edge modules can be used, configured, and reused - preview] [ lnk-module-comp] (Grundlegendes zur Verwendung, Konfiguration und Wiederverwendung von IoT Edge-Modulen – Vorschau, in englischer Sprache) beschrieben werden. Wenn Sie ein IoT Edge-Modul entwickeln, können Sie diese Nachrichten empfangen, indem Sie Meldungshandler festlegen, wie im Tutorial [Develop and deploy an IoT Edge module to a simulated device][lnk-tutorial2] (Entwickeln und Bereitstellen eines IoT Edge-Moduls auf einem simulierten Gerät, in englischer Sprache) gezeigt.

Um die Erstellung von Routen zu vereinfachen, wird in IoT Edge das Konzept von *Eingangs*- und *Ausgangs*-Modulendpunkten eingeführt. Ein Modul kann alle an es weitergeleiteten D2C-Nachrichten empfangen, ohne einen Eingang anzugeben, und D2C-Nachrichten senden, ohne einen Ausgang anzugeben.
Die Verwendung expliziter Eingänge und Ausgänge erleichtert jedoch das Verständnis der Routingregeln. Unter [Understand how IoT Edge modules can be used, configured, and reused - preview] [ lnk-module-comp] (Grundlegendes zur Verwendung, Konfiguration und Wiederverwendung von IoT Edge-Modulen – Vorschau, in englischer Sprache) finden Sie weitere Informationen zu Routingregeln sowie Eingangs- und Ausgangsendpunkten für Module.

Schließlich werden vom Edge-Hub behandelte D2C-Nachrichten mit den folgenden Eigenschaften gekennzeichnet:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| $connectionDeviceId | Die Geräte-ID des Clients, der die Nachricht gesendet hat. |
| $connectionModuleId | Die Modul-ID des Moduls, das die Nachricht gesendet hat. |
| $inputName | Der Eingang, an dem die Nachricht empfangen wurde. Kann leer sein. |
| $outputName | Der zum Senden der Nachricht verwendete Ausgang. Kann leer sein. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Herstellen einer Verbindung von einem Modul mit dem IoT Edge Hub
Das Herstellen der Verbindung von einem Modul mit dem lokalen IoT Edge-Hub erfolgt in zwei Schritten: Verwenden der Verbindungszeichenfolge, die beim Starten des Moduls von der IoT Edge-Laufzeit bereitgestellt wird, und Sicherstellen, dass die Anwendung das vom Edge IoT-Hub auf diesem Gerät vorgelegte Zertifikat akzeptiert.

Die zu verwendende Verbindungszeichenfolge wird von der IoT Edge-Laufzeit in der Umgebungsvariablen `EdgeHubConnectionString` eingefügt. Dadurch ist sie für jedes Programm, das sie verwenden möchte, verfügbar.

Analog dazu wird das zum Überprüfen der Verbindung mit dem IoT Edge-Hub zu verwendende Zertifikat von der IoT Edge-Laufzeit in eine Datei eingefügt, deren Pfad in der Umgebungsvariablen `EdgeModuleCACertificateFile` verfügbar ist.

Im Tutorial [Develop and deploy an IoT Edge module to a simulated device][lnk-tutorial2] (Entwickeln und Bereitstellen eines IoT Edge-Moduls auf einem simulierten Gerät, in englischer Sprache) wird gezeigt, wie Sie sicherstellen, dass sich das Zertifikat im Computerspeicher oder in der Modulanwendung befindet. Selbstverständlich eignet sich hierzu auch jede andere Methode zum Vertrauen von Verbindungen, die dieses Zertifikat verwenden.

## <a name="packaging-as-an-image"></a>Verpacken als Image
IoT Edge-Module werden als Docker-Images verpackt.
Sie können direkt die Docker-Toolkette oder Visual Studio Code verwenden, wie im Tutorial [Develop and deploy an IoT Edge module to a simulated device][lnk-tutorial2] (Entwickeln und Bereitstellen eines IoT Edge-Moduls auf einem simulierten Gerät, in englischer Sprache) gezeigt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie ein Modul entwickelt haben, informieren Sie sich im Artikel [Deploy and monitor IoT Edge modules at scale][lnk-howto-deploy] (Bereitstellen und Überwachen von IoT Edge-Modulen in großem Maßstab, in englischer Sprache), wie Sie zahlreiche IoT Edge-Module gemeinsam bereitstellen und überwachen.

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-devicetwin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-messaging]: ../iot-hub/iot-hub-devguide-messaging.md
[lnk-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-tutorial2]: tutorial-csharp-module.md
[lnk-module-comp]: module-composition.md
[lnk-howto-deploy]: how-to-deploy-monitor.md
