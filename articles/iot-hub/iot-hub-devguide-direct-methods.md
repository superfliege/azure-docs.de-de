---
title: Informationen zu direkten Azure IoT Hub-Methoden | Microsoft Docs
description: "Entwicklerhandbuch – Verwenden von direkten Methoden zum Aufrufen von Code auf Ihren Geräten von einer Service-App"
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 243845139c7ae0389333d7490098ef73f95dceac
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Verstehen und Aufrufen direkter Methoden von IoT Hub
IoT Hub gibt Ihnen die Möglichkeit, direkte Methoden auf Geräten von der Cloud aus aufzurufen. Direkte Methoden stellen eine Anforderung-Antwort-Interaktion mit einem Gerät dar, die einem HTTP-Aufruf darin ähnelt, dass sie unverzüglich (nach einem vom Benutzer angegebenen Timeout) zu einem Erfolg oder Fehler führt. Diese Methode ist hilfreich für Szenarien, in denen die unmittelbare Vorgehensweise davon abhängt, ob das Gerät antworten konnte, z.B. beim Senden eines SMS-Weckrufs an ein Gerät, wenn es offline ist (da eine SMS teurer als ein Methodenaufruf ist).
Jedes Gerätemethode hat ein einzelnes Gerät als Ziel. [Aufträge][lnk-devguide-jobs] bieten eine Möglichkeit zum Aufrufen von direkten Methoden auf mehreren Geräten und zum Planen von Methodenaufrufen für nicht verbundene Geräte.

Jeder Benutzer mit der Berichtigung **Dienstverbindung** für IoT Hub kann eine Methode auf einem Gerät aufrufen.

Direkte Methoden entsprechen einem Anforderung-Antwort-Schema und sind für Kommunikation bestimmt, die sofortige Bestätigung ihres Ergebnisses erfordert, in der Regel über interaktive Steuerung des Geräts, z.B. Einschalten eines Lüfters.

Falls Sie weitere Informationen dazu benötigen, was die Verwendung von gewünschten Eigenschaften, direkten Methoden oder C2D-Nachrichten betrifft, hilft Ihnen der [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance] weiter.

## <a name="method-lifecycle"></a>Methodenlebenszyklus
Direkte Methoden werden auf dem Gerät implementiert und können für eine ordnungsgemäße Instanziierung null oder mehr Eingaben in der Methodennutzlast erfordern. Sie rufen eine direkte Methode über einen dienstseitigen URI (`{iot hub}/twins/{device id}/methods/`) auf. Ein Gerät empfängt direkte Methoden über ein gerätespezifisches MQTT-Thema (`$iothub/methods/POST/{method name}/`) oder über AMQP-Links (Anwendungseigenschaften `IoThub-methodname` und `IoThub-status`). 

> [!NOTE]
> Wenn Sie eine direkte Methode auf einem Gerät aufrufen, können Eigenschaftennamen und -werte nur druckbare alphanumerische US-ASCII-Zeichen mit Ausnahme der folgenden enthalten: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Direkte Methoden sind synchron und werden nach der Wartezeit (Standardeinstellung: 30 Sekunden, einstellbar bis 3.600 Sekunden) entweder mit einem Erfolg oder Fehler abgeschlossen. Direkte Methoden sind hilfreich bei interaktiven Szenarios, in denen ein Gerät genau dann agieren soll, wenn es online ist und Befehle empfängt, z.B. Einschalten eines Lichts von einem Telefon aus. In diesen Szenarios soll der Erfolg oder Misserfolg unmittelbar erkennbar sein, damit der Clouddienst so schnell wie möglich auf das Ergebnis reagieren kann. Das Gerät kann einen Nachrichtentext als Ergebnis der Methode zurückgeben, dies ist für die Methode aber nicht erforderlich. Es gibt keine Garantie für die Sortierung oder eine Parallelitätssemantik für Methodenaufrufe.

Direkte Methoden erfolgen von der Cloudseite nur über HTTPS und von der Geräteseite über MQTT oder AMQP.

Die Nutzlast für Methodenanforderungen und -antworten ist ein JSON-Dokument mit bis zu 128 KB.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Aufrufen einer direkten Methode aus einer Back-End-App
### <a name="method-invocation"></a>Methodenaufruf
Direkte Methodenaufrufe auf einem Gerät sind HTTPS-Aufrufe, die Folgendes umfassen:

* Den spezifischen *URI* für das Gerät (`{iot hub}/twins/{device id}/methods/`)
* Die POST-*Methode*
* *Header*, die Autorisierung, Anforderungs-ID, Inhaltstyp und Inhaltscodierung enthalten
* Einen transparenten JSON-*Haupttext* im folgenden Format:

   ```
   {
       "methodName": "reboot",
       "responseTimeoutInSeconds": 200,
       "payload": {
           "input1": "someInput",
           "input2": "anotherInput"
       }
   }
   ```

Timeout in Sekunden. Wenn kein Timeout festgelegt ist, lautet der Standardwert 30 Sekunden.

### <a name="response"></a>response
Die Back-End-App empfängt eine Antwort, die Folgendes umfasst:

* *HTTP-Statuscode*, der für Fehler von IoT Hub verwendet wird. Dazu gehören z.B. 404-Fehler für Geräte, die derzeit nicht verbunden sind
* *Header*, die ETag, Anforderungs-ID, Inhaltstyp und Inhaltscodierung enthalten
* Einen JSON-*Haupttext* im folgenden Format:

   ```   {
       "status" : 201,
       "payload" : {...}
   }
   ```

   `status` und `body` werden vom Gerät bereitgestellt und für die Antwort mit dem Statuscode und/oder der Beschreibung des Geräts verwendet.

## <a name="handle-a-direct-method-on-a-device"></a>Behandeln einer direkten Methode auf einem Gerät
### <a name="mqtt"></a>MQTT
#### <a name="method-invocation"></a>Methodenaufruf
Geräte empfangen direkte Methodenanforderungen zum MQTT-Thema: `$iothub/methods/POST/{method name}/?$rid={request id}`

Der vom Gerät empfangene Text weist das folgende Format auf:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Methodenanforderungen sind QoS 0.

#### <a name="response"></a>response
Das Gerät sendet Antworten an `$iothub/methods/res/{status}/?$rid={request id}`, wobei Folgendes gilt:

* Die `status`-Eigenschaft ist der vom Gerät bereitgestellte Status der Methodenausführung.
* Die `$rid`-Eigenschaft ist die Anforderungs-ID des von IoT Hub empfangenen Methodenaufrufs.

Der Haupttext wird durch das Gerät festgelegt und kann jeden beliebigen Status aufweisen.

### <a name="amqp"></a>AMQP
#### <a name="method-invocation"></a>Methodenaufruf
Das Gerät empfängt Anforderungen direkter Methoden durch Erstellen eines Empfangslinks für die Adresse `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`.

Die AMQP-Nachricht geht bei dem Empfangslink ein, der die Methodenanforderung darstellt. Sie umfasst Folgendes:
* Die Korrelations-ID-Eigenschaft, die eine Anforderungs-ID enthält, die mit der entsprechenden Methodenantwort zurückgegeben werden soll
* Eine Anwendungseigenschaft namens `IoThub-methodname`, die den Namen der aufgerufenen Methode enthält
* Den AMQP-Nachrichtentext mit der Methodennutzlast im JSON-Format

#### <a name="response"></a>response
Das Gerät erstellt einen Sendelink, um die Methodenantwort an der Adresse `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound` zurückzugeben.

Die Antwort der Methode wird über den Sendelink zurückgegeben und umfasst Folgendes:
* Die Korrelations-ID-Eigenschaft, die die Anforderungs-ID enthält, die in der Anforderungsnachricht der Methode übergeben wurde
* Eine Anwendungseigenschaft namens `IoThub-status`, die den vom Benutzer angegebenen Methodenstatus enthält
* Den AMQP-Nachrichtentext mit der Methodenantwort im JSON-Format

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Azure IoT-SDKs für Geräte und Dienste][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.
* Unter [IoT Hub-Abfragesprache für Gerätezwillinge, Aufträge und Nachrichtenrouting][lnk-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen zu Gerätezwillingen und Aufträgen abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie erfahren haben, wie Sie direkte Methoden verwenden, sind möglicherweise die folgenden Themen im IoT Hub-Entwicklerhandbuch für Sie interessant:

* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, ist möglicherweise das folgende IoT Hub-Tutorial für Sie interessant:

* [Verwenden von direkten Methoden][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
