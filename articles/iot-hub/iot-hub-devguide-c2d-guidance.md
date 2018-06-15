---
title: Azure IoT Hub – C2D-Optionen (Cloud-zu-Gerät) | Microsoft Docs
description: Entwicklerhandbuch – Leitfaden, der angibt, wann direkte Methoden, gewünschte Eigenschaften von Gerätezwillingen oder C2D-Nachrichten für C2D-Kommunikationen verwendet werden sollen.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: ff81be4bbf6d297c623c5d98b5dc22a540112fcc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634436"
---
# <a name="cloud-to-device-communications-guidance"></a>Leitfaden zur C2D-Kommunikation
IoT Hub bietet drei Optionen für Geräte-Apps, um Funktionen einer Back-End-App verfügbar zu machen:

* [Direkte Methoden][lnk-methods] für Kommunikation, die eine sofortige Bestätigung des Ergebnisses erfordert. Direkte Methoden werden häufig für die interaktive Steuerung von Geräten verwendet, beispielsweise zum Einschalten eines Lüfters.
* [Gewünschte Eigenschaften von Gerätezwillingen][lnk-twins] für Befehle mit langer Ausführungszeit, die das Gerät in einen bestimmten gewünschten Zustand versetzen sollen. Legen Sie das Telemetriesendeintervall z.B. auf 30 Minuten fest.
* [Cloud-zu-Gerät-Nachrichten (Cloud-to-Device, C2D)][lnk-c2d] zum Senden unidirektionaler Benachrichtigungen an die Geräte-App

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Hier finden Sie einen detaillierten Vergleich verschiedener Optionen für die C2D-Kommunikation.

|  | Direkte Methoden | Gewünschte Eigenschaften von Gerätezwillingen | C2D-Nachrichten |
| ---- | ------- | ---------- | ---- |
| Szenario | Befehle, die sofortige Bestätigung erfordern, z.B. Einschalten eines Lüfters | Befehle mit langer Ausführungszeit, die das Gerät in einen bestimmten gewünschten Zustand versetzen sollen. Legen Sie das Telemetriesendeintervall z.B. auf 30 Minuten fest. | Senden unidirektionaler Benachrichtigungen an die Geräte-App. |
| Datenfluss | Bidirektional. Die Geräte-App kann sofort auf die Methode reagieren. Das Lösungs-Back-End empfängt ein auf die Anforderung kontextbezogenes Ergebnis. | Unidirektional. Die Geräte-App empfängt eine Benachrichtigung mit der Eigenschaftenänderung. | Unidirektional. Die Geräte-App empfängt die Nachricht.
| Dauerhaftigkeit | Mit getrennten Geräten wird kein Kontakt hergestellt. Dem Lösungs-Back-End wird gemeldet, dass das Gerät nicht verbunden ist. | Eigenschaftswerte werden im Gerätezwilling beibehalten. Das Gerät wird diese bei der nächsten erneuten Verbindung lesen. Eigenschaftswerte sind mit der [IoT Hub-Abfragesprache][lnk-query] erneut abrufbar. | Nachrichten können durch IoT Hub bis zu 48 Stunden aufbewahrt werden. |
| Ziele | Einzelgerät mit **deviceId** oder mehrere Geräte mit [Aufträgen][lnk-jobs]. | Einzelgerät mit **deviceId** oder mehrere Geräte mit [Aufträgen][lnk-jobs]. | Einzelgerät nach **deviceId**. |
| Größe | Die maximale Nutzlast für direkte Methoden beträgt 128KB. | Die Maximalgröße gewünschter Eigenschaften beträgt 8 KB. | Bis zu 64 KB für Nachrichten. |
| Frequency | Hoch. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. | Mittel. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. | Niedrig. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. |
| Protokoll | Mit MQTT oder AMQP verfügbar. | Mit MQTT oder AMQP verfügbar. | Mit allen Protokollen verfügbar. Gerät muss bei Verwendung von HTTPS einen Abruf tätigen. |

Erfahren Sie in den folgenden Tutorials, wie Sie direkte Methoden, gewünschte Eigenschaften und C2D-Nachrichten einsetzen:

* [Use direct methods][lnk-methods-tutorial] (Verwenden von direkten Methoden);
* [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-twin-properties] zu den gewünschten Eigenschaften von Gerätezwillingen; 
* [Tutorial: Senden von C2D-Nachrichten mithilfe von IoT Hub und „Node.js“][lnk-c2d-tutorial] für C2D-Nachrichten.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md
