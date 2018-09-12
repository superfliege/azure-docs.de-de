---
title: Grundlegendes zum Azure IoT Hub-Nachrichtenformat | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Beschreibung des Formats und des erwarteten Inhalts von IoT Hub-Nachrichten.'
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 7c08848698f07d64bbbff429682c18525659f7bf
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286516"
---
# <a name="create-and-read-iot-hub-messages"></a>Erstellen und Lesen von IoT Hub-Nachrichten

Um eine nahtlose Interoperabilität zwischen Protokollen zu gewährleisten, definiert IoT Hub ein gemeinsames Nachrichtenformat für alle geräteseitigen Protokolle. Dieses Nachrichtenformat wird sowohl für [D2C][lnk-d2c]- als auch für [C2D][lnk-c2d]-Nachrichten verwendet. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Eine [IoT Hub-Nachricht][lnk-messaging] enthält Folgendes:

* Einen vordefinierten Satz von *Systemeigenschaften* wie unten aufgeführt.
* Einen Satz an *Anwendungseigenschaften*. Ein Wörterbuch mit Zeichenfolgeneigenschaften. Die Anwendung kann diese definieren und darauf zugreifen, ohne den Nachrichtentext deserialisieren zu müssen. IoT Hub ändert diese Eigenschaften nie.
* Ein nicht lesbarer binärer Textkörper.

Eigenschaftennamen und Eigenschaftswerte dürfen nur alphanumerische ASCII-Zeichen sowie die Zeichen ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` enthalten. Dies gilt für folgende Situationen:  

* Senden von Gerät-zu-Cloud-Nachrichten über das Protokoll HTTPS
* Senden von Cloud-an-Gerät-Nachrichten.

Weitere Informationen zum Codieren und Decodieren von Nachrichten, die über verschiedene Protokollen gesendet werden, finden Sie unter [Azure IoT-SDKs][lnk-sdks].

In der folgenden Tabelle werden die Systemeigenschaften in IoT Hub-Nachrichten aufgeführt.

| Eigenschaft | BESCHREIBUNG | Kann der Benutzer festgelegt werden? |
| --- | --- | --- |
| MessageId |Eine vom Benutzer festgelegte Kennung für die Nachricht; wird für Anforderung-Antwort-Muster verwendet. Format: Eine Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung (bis zu 128 Zeichen lang), die aus alphanumerischen ASCII-Zeichen (7 Bit) + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`besteht. | JA |
| Sequenznummer |Eine Nummer (für jede Gerätewarteschlange eindeutig), die jeder C2D-Nachricht von IoT Hub zugewiesen wird | „Nein“ bei C2D-Nachrichten; andernfalls „Ja“. |
| To |Ein Ziel, das in [C2D][lnk-c2d]-Nachrichten angegeben wird. | „Nein“ bei C2D-Nachrichten; andernfalls „Ja“. |
| ExpiryTimeUtc |Datum und Uhrzeit des Nachrichtenablaufs. | JA |
| EnqueuedTime |Datum und Uhrzeit des Empfangs der [C2D][lnk-c2d]-Nachricht durch IoT Hub. | „Nein“ bei C2D-Nachrichten; andernfalls „Ja“. |
| CorrelationId |Eine Zeichenfolgeneigenschaft in einer Antwortnachricht, die normalerweise die Nachrichten-ID der Anforderung im Anforderung-Antwort-Muster enthält. | JA |
| UserId |Eine ID zum Festlegen des Ursprungs von Nachrichten. Wenn IoT Hub Nachrichten generiert, wird diese Eigenschaft auf `{iot hub name}`festgelegt. | Nein  |
| Ack |Ein Feedbacknachrichtengenerator. Diese Eigenschaft wird in C2D-Nachrichten verwendet, um IoT Hub anzuweisen, als Ergebnis der Nachrichtenverarbeitung durch das Gerät Feedbacknachrichten zu generieren. Mögliche Werte: **Kein** (Standardeinstellung): Es wird keine Feedbacknachricht generiert. **Positiv**: Es wird eine Feedbacknachricht empfangen, wenn die Nachricht abgeschlossen wurde. **Negativ**: Es wird eine Feedbacknachricht empfangen, wenn die Nachricht ohne vollständige Verarbeitung durch das Gerät abgelaufen ist (oder die maximale Anzahl von Zustellversuchen erreicht wurde). **Voll**: Feedback wird sowohl bei erfolgreicher als auch nicht erfolgreicher Nachrichtenverarbeitung generiert. Weitere Informationen finden Sie unter [Nachrichtenfeedback][lnk-feedback]. | JA |
| ConnectionDeviceId |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **deviceId** des Geräts, das die Nachricht sendet. | „Nein“ bei D2C-Nachrichten; andernfalls „Ja“. |
| ConnectionDeviceGenerationId |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **generationId** (gemäß [Geräteidentitätseigenschaften][lnk-device-properties]) des Geräts, das die Nachricht gesendet hat. | „Nein“ bei D2C-Nachrichten; andernfalls „Ja“. |
| ConnectionAuthMethod |Eine von IoT Hub für D2C-Nachrichten festgelegte Authentifizierungsmethode. Diese Eigenschaft enthält Informationen zu der Methode, die zum Authentifizieren des Geräts verwendet wird, das die Nachricht sendet. Weitere Informationen finden Sie unter [D2C-Anti-Spoofing][lnk-antispoofing]. | „Nein“ bei D2C-Nachrichten; andernfalls „Ja“. |
| CreationTimeUtc | Datum und Uhrzeit der Erstellung der Nachricht auf einem Gerät. Ein Gerät muss diesen Wert explizit festlegen. | JA |

## <a name="message-size"></a>Nachrichtengröße

IoT Hub misst die Nachrichtengröße auf „protokollagnostische“ Weise, indem nur die tatsächliche Nutzlast berücksichtigt wird. Die Summe für die Größe in Byte wird wie folgt berechnet:

* Text in Byte
* Größe aller Werte der Nachrichtensystemeigenschaften in Byte
* Größe aller Benutzereigenschaftsnamen und -werte in Byte

Die Eigenschaftennamen und -werte sind auf ASCII-Zeichen beschränkt, sodass die Länge der Zeichenfolgen der Größe in Byte entspricht.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu Größenbeschränkungen für Nachricht in IoT Hub finden Sie unter [IoT Hub-Kontingente und -Drosselung][lnk-quotas].

Informationen zum Erstellen und Lesen von IoT Hub-Nachrichten in verschiedenen Programmiersprachen finden Sie in den [Schnellstarts][lnk-get-started].

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: quickstart-send-telemetry-node.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
