---
title: Grundlegendes zu Azure IoT Hub-Aufträgen | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Planen der Ausführung von Aufträgen auf mehreren Geräten, die mit Ihrer IoT Hub-Instanz verbunden sind. Aufträge können Tags und gewünschte Eigenschaften aktualisieren und auf mehreren Geräten direkte Methoden aufrufen.'
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: fe78458f-4f14-4358-ac83-4f7bd14ee8da
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 367eb703465b2fbc6f1e06a383bc9df709cabe78
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="schedule-jobs-on-multiple-devices"></a>Planen von Aufträgen auf mehreren Geräten

Azure IoT Hub bietet eine Reihe von Bausteinen, z. B. [Eigenschaften und Tags von Gerätezwillingen][lnk-twin-devguide] und [Direkte Methoden][lnk-dev-methods].  Mit Back-End-Apps können Geräteadministratoren und -bediener in der Regel IoT-Geräte mithilfe eines Massenvorgangs und zu einem geplanten Zeitpunkt aktualisieren und mit ihnen interagieren.  Aufträge führen Updates von Gerätezwillingen und direkten Methoden für eine Gruppe von Geräten zu einem geplanten Zeitpunkt aus.  So kann ein Bediener beispielsweise mithilfe einer Back-End-App einen Auftrag für den Neustart einer Gruppe von Geräten im dritten Stock von Gebäude 43 zu einem Zeitpunkt initiieren und nachverfolgen, zu dem die Betriebsabläufe im Gebäude nicht gestört werden.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Erwägen Sie das Arbeiten mit Aufträgen, wenn Sie den Fortschritt der folgenden Aktivitäten für eine Gruppe von Geräten planen und nachverfolgen müssen:

* Aktualisieren gewünschter Eigenschaften
* Aktualisieren von Tags
* Aufrufen direkter Methoden

## <a name="job-lifecycle"></a>Auftragslebenszyklus
Aufträge werden vom Lösungs-Back-End eingeleitet und vom IoT Hub verwaltet.  Sie können einen Auftrag über einen dienstseitigen URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) auslösen und den Fortschritt eines in der Ausführung befindlichen Auftrags über einen dienstseitigen URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`) abfragen. Um den Status der zurzeit ausgeführten Aufträge nach der Initiierung eines Auftrags zu aktualisieren, führen Sie eine Auftragsabfrage aus.

> [!NOTE]
> Wenn Sie einen Auftrag initiieren, dürfen Eigenschaftennamen und -werte nur druckbare alphanumerische US-ASCII-Zeichen mit Ausnahme der folgenden enthalten: `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`.

## <a name="jobs-to-execute-direct-methods"></a>Aufträge zum Ausführen direkter Methoden
Der folgende Ausschnitt enthält die HTTPS 1.1-Anforderungsdetails, um eine [direkte Methode][lnk-dev-methods] für eine Gruppe von Geräten mithilfe eines Auftrags auszuführen:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }

Die Abfragebedingung kann auch wie unten dargestellt für eine einzelne Geräte-ID oder eine Liste mit Geräte-IDs gelten. Im Folgenden werden einige Beispiele vorgestellt:

```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
Im Artikel zur [IoT Hub-Abfragesprache][lnk-query] wird die IoT Hub-Abfragesprache ausführlicher erläutert.

## <a name="jobs-to-update-device-twin-properties"></a>Aufträge zum Aktualisieren der Eigenschaften von Gerätezwillingen
Der folgende Ausschnitt enthält die HTTPS 1.1-Anforderungsdetails, um die Eigenschaften von Gerätezwillingen mithilfe eines Auftrags zu aktualisieren:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }

## <a name="querying-for-progress-on-jobs"></a>Abfragen des Fortschritts von Aufträgen
Der folgende Ausschnitt enthält die HTTPS 1.1-Anforderungsdetails für das [Abfragen von Aufträgen][lnk-query]:

    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

Das „continuationToken“ wird aus der Antwort bereitgestellt.  

## <a name="jobs-properties"></a>Auftragseigenschaften
Die folgende Liste enthält die Eigenschaften und entsprechenden Beschreibungen, die beim Abfragen von Aufträgen oder Auftragsergebnissen verwendet werden können.

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| **jobId** |Für den Auftrag von der Anwendung angegebene ID. |
| **startTime** |Von der Anwendung angegebene Startzeit (ISO-8601) für den Auftrag. |
| **endTime** |Von IoT Hub angegebenes Datum (ISO-8601) für den Abschluss des Auftrags. Gilt nur, nachdem der Auftrag den Zustand „Abgeschlossen“ erreicht hat. |
| **type** |Auftragstypen: |
| | **scheduledUpdateTwin**: Ein Auftrag zum Aktualisieren einer Gruppe gewünschter Eigenschaften oder Tags. |
| | **scheduledDeviceMethod**: Ein Auftrag zum Aufrufen einer Gerätemethode für eine Gruppe von Gerätezwillingen. |
| **status** |Aktueller Status des Auftrags. Mögliche Werte für den Status: |
| | **pending**: Geplant und auf Auswahl durch den Auftragsdienst wartend. |
| | **scheduled**: Für einen Zeitpunkt in der Zukunft geplant. |
| | **running**: Derzeit aktiver Auftrag. |
| | **canceled**: Der Auftrag wurde abgebrochen. |
| | **failed**: Fehler beim Auftrag. |
| | **completed**: Auftrag wurde abgeschlossen. |
| **deviceJobStatistics** |Statistiken zur Ausführung des Auftrags. |
| | **deviceJobStatistics**-Eigenschaften: |
| | **deviceJobStatistics.deviceCount**: Anzahl der Geräte im Auftrag. |
| | **deviceJobStatistics.failedCount**: Anzahl der Geräte, bei denen der Auftrag fehlgeschlagen ist. |
| | **deviceJobStatistics.succeededCount**: Anzahl der Geräte, bei denen der Auftrag erfolgreich ausgeführt wurde. |
| | **deviceJobStatistics.runningCount**: Anzahl der Geräte, auf denen der Auftrag derzeit ausgeführt wird. |
| | **deviceJobStatistics.pendingCount**: Anzahl der Geräte, bei denen die Ausführung des Auftrags aussteht. |

### <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Azure IoT-SDKs für Geräte und Dienste][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.
* [IoT Hub-Abfragesprache für Gerätezwillinge, Aufträge und Nachrichtenrouting][lnk-query] beschreibt die IoT Hub-Abfragesprache. Verwenden Sie diese Abfragesprache, um aus IoT Hub Informationen zu Gerätezwillingen und Aufträgen abzurufen.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Um einige der in diesem Artikel beschriebenen Konzepte auszuprobieren, sehen Sie sich das folgende IoT Hub-Tutorial an:

* [Planen und Übertragen von Aufträgen][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
