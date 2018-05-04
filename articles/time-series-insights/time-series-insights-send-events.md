---
title: Senden von Ereignissen an eine Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Event Hubs erstellen und konfigurieren. Außerdem erfahren Sie, wie Sie eine Beispielanwendung ausführen, um Ereignisse für die Anzeige in Azure Time Series Insights mithilfe von Push zu übertragen.
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MarkMcGeeAtAquent
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 04/09/2018
ms.openlocfilehash: b418d1114cf6b906dcdee46bbf7e094cbc4a0521
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Senden von Ereignissen an die Azure Time Series Insights-Umgebung mithilfe von Event Hub
In diesem Artikel erfahren Sie, wie Sie Event Hubs erstellen und konfigurieren. Außerdem erfahren Sie, wie Sie eine Beispielanwendung ausführen, um Ereignisse mithilfe von Push zu übertragen. Wenn Sie bereits über einen Event Hub mit Ereignissen im JSON-Format verfügen, überspringen Sie dieses Tutorial, und sehen Sie sich Ihre Umgebung in [Time Series Insights](https://insights.timeseries.azure.com) an.

## <a name="configure-an-event-hub"></a>Konfigurieren eines Event Hubs
1. Führen Sie zum Erstellen eines Event Hubs die in der [Event Hub-Dokumentation](../event-hubs/event-hubs-create.md) beschriebenen Schritte aus.

2. Suchen Sie über die Suchleiste nach **Event Hub**. Klicken Sie in der Liste mit den zurückgegebenen Ergebnissen auf **Event Hubs**.

3. Klicken Sie auf den Namen Ihres Event Hubs, um ihn auszuwählen.

4. Klicken Sie unter **Entitäten** im mittleren Konfigurationsfenster erneut auf **Event Hubs**.

5. Wählen Sie den Namen des Event Hubs aus, um ihn zu konfigurieren.

  ![Auswählen der Event Hub-Consumergruppe](media/send-events/consumer-group.png)

6. Klicken Sie unter **Entitäten** auf **Consumergruppen**.
 
7. Erstellen Sie eine Consumergruppe, die ausschließlich von Ihrer Time Series Insights-Ereignisquelle verwendet wird.

   > [!IMPORTANT]
   > Achten Sie darauf, dass diese Consumergruppe nicht von einem anderen Dienst (beispielsweise durch einen Stream Analytics-Auftrag oder durch eine andere Time Series Insights-Umgebung) verwendet wird. Wenn die Consumergruppe von anderen Diensten verwendet wird, wirkt sich das negativ auf Lesevorgänge für diese Umgebung und andere Dienste aus. Wenn Sie „$Default“ als Consumergruppe verwenden, wird sie unter Umständen von anderen Lesern wiederverwendet.

8. Klicken Sie unter der Überschrift **Einstellungen** auf **Freigegebene Zugriffsrichtlinien**.

9. Erstellen Sie für den Event Hub die Richtlinie **MySendPolicy**. Diese wird im CSharp-Beispiel zum Senden von Ereignissen verwendet.

  ![Auswählen von „SAS-Richtlinien“ und Klicken auf „Hinzufügen“](media/send-events/shared-access-policy.png)  

  ![Hinzufügen der neuen SAS-Richtlinie](media/send-events/shared-access-policy-2.png)  

## <a name="add-time-series-insights-reference-data-set"></a>Hinzufügen eines Time Series Insights-Verweisdatasets 
Durch die Nutzung von Verweisdaten in TSI erhalten Ihre Telemetriedaten einen Kontextbezug.  Dieser Kontext verleiht Ihren Daten eine Bedeutung, und sie können einfacher gefiltert und aggregiert werden.  TSI fügt Verweisdaten zur Eingangszeit und kann die Daten nicht nachträglich verknüpfen.  Daher ist es wichtig, Verweisdaten hinzuzufügen, bevor Sie eine Ereignisquelle mit Daten hinzufügen.  Daten wie der Standort oder Sensortyp sind nützliche Dimensionen, die Sie beispielsweise mit der ID eines Geräts, Tags oder Sensors verknüpfen können, um das Slicing und die Filterung zu vereinfachen.  

> [!IMPORTANT]
> Die Konfiguration eines Verweisdatasets ist sehr wichtig, wenn Sie Verlaufsdaten hochladen.

Stellen Sie sicher, dass Verweisdaten vorhanden sind, wenn Sie einen Massenupload von Verlaufsdaten für TSI durchführen.  Beachten Sie, dass TSI sofort mit dem Lesen von einer verknüpften Ereignisquelle beginnt, wenn diese Ereignisquelle über Daten verfügt.  Es ist hilfreich, mit dem Verknüpfen einer Ereignisquelle mit TSI zu warten, bis Ihre Verweisdaten vorhanden sind. Dies gilt besonders, wenn diese Ereignisquelle Daten enthält. Alternativ hierzu können Sie mit dem Übertragen von Daten per Pushvorgang auf die Ereignisquelle auch warten, bis das Verweisdataset bereitgestellt wurde.

Für die Verwaltung von Verweisdaten stehen eine webbasierte Benutzeroberfläche im TSI-Explorer und eine programmgesteuerte C#-API zur Verfügung. Der TSI-Explorer enthält eine grafische Benutzeroberfläche zum Hochladen von Dateien oder Einfügen von vorhandenen Verweisdatasets im JSON- oder CSV-Format. Mit der API können Sie bei Bedarf eine benutzerdefinierte App erstellen.

Weitere Informationen zum Verwalten von Verweisdaten in Time Series Insights finden Sie im [Artikel zu Verweisdaten](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="create-time-series-insights-event-source"></a>Erstellen der Time Series Insights-Ereignisquelle
1. Falls Sie noch keine Ereignisquelle erstellt haben, führen Sie [diese Schritte](time-series-insights-how-to-add-an-event-source-eventhub.md) aus.

2. Geben Sie **deviceTimestamp** als Name der timestamp-Eigenschaft an. Diese Eigenschaft wird im C#-Beispiel als tatsächlicher Zeitstempel verwendet. Bei der timestamp-Eigenschaft muss die Groß-/Kleinschreibung beachtet werden, und Werte müssen das Format __yyyy-MM-ddTHH:mm:ss.FFFFFFFK__ besitzen, wenn sie als JSON-Code an Event Hub gesendet werden. Sollte die Eigenschaft im Ereignis nicht vorhanden sein, wird der Zeitpunkt verwendet, zu dem der Event Hub in die Warteschlange eingereiht wurde.

  ![Erstellen der Ereignisquelle](media/send-events/event-source-1.png)

## <a name="sample-code-to-push-events"></a>Beispielcode zum Übertragen von Ereignissen mithilfe von Push
1. Navigieren Sie zur Event Hub-Richtlinie **MySendPolicy**. Kopieren Sie die **Verbindungszeichenfolge** mit dem Richtlinienschlüssel.

  ![Kopieren der MySendPolicy-Verbindungszeichenfolge](media/send-events/sample-code-connection-string.png)

2. Führen Sie den folgenden Code aus. Dieser Code sendet 600 Ereignisse über jedes der drei Geräte. Aktualisieren Sie `eventHubConnectionString` mit Ihrer Verbindungszeichenfolge.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a>Unterstützte JSON-Formen
### <a name="sample-1"></a>Beispiel 1

#### <a name="input"></a>Eingabe

Ein einfaches JSON-Objekt.

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---one-event"></a>Ausgabe – ein Ereignis

|id|timestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Beispiel 2

#### <a name="input"></a>Eingabe
Ein JSON-Array mit zwei JSON-Objekten. Jedes JSON-Objekt wird in ein Ereignis konvertiert.
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---two-events"></a>Ausgabe – zwei Ereignisse

|id|timestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Beispiel 3

#### <a name="input"></a>Eingabe

Ein JSON-Objekt mit einem geschachtelten JSON-Array, das zwei JSON-Objekte enthält:
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---two-events"></a>Ausgabe – zwei Ereignisse
Beachten Sie, dass die location-Eigenschaft in die einzelnen Ereignisse kopiert wird.

|location|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Beispiel 4

#### <a name="input"></a>Eingabe

Ein JSON-Objekt mit einem geschachtelten JSON-Array, das zwei JSON-Objekte enthält. Diese Eingabe zeigt, dass die globalen Eigenschaften vom komplexen JSON-Objekt dargestellt werden können.

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---two-events"></a>Ausgabe – zwei Ereignisse

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

### <a name="json-shaping-strategies"></a>Strategien für die JSON-Gestaltung
Wir verwenden das folgende Beispiel für ein Ereignis als Ausgangspunkt und beschreiben dann damit verbundene Probleme und Strategien zur Behebung.

#### <a name="payload-1"></a>Nutzlast 1:
```json
[{
            "messageId": "LINE_DATA",
            "deviceId": "FXXX",
            "timestamp": 1522355650620,
            "series": [{
                        "chId": 3,
                        "value": -3750.0
                  }, {
                        "chId": 13,
                        "value": 0.58015072345733643
                  }, {
                        "chId": 11,
                        "value": 800.0
                  }, {
                        "chId": 21,
                        "value": 0.0
                  }, {
                        "chId": 14,
                        "value": -999.0
                  }, {
                        "chId": 37,
                        "value": 2.445906400680542
                  }, {
                        "chId": 39,
                        "value": 0.0
                  }, {
                        "chId": 40,
                        "value": 1.0
                  }, {
                        "chId": 1,
                        "value": 1.0172575712203979
                  }
            ],
            "EventProcessedUtcTime": "2018-03-29T20:36:21.3245900Z",
            "PartitionId": 2,
            "EventEnqueuedUtcTime": "2018-03-29T20:34:11.0830000Z",
            "IoTHub": {
                  "MessageId": "<17xxx2xx-36x0-4875-9x1x-x428x41x1x68>",
                  "CorrelationId": "<x253x5xx-7xxx-4xx3-91x4-xxx3bx2xx0x3>",
                  "ConnectionDeviceId": "AAAA-ZZ-001",
                  "ConnectionDeviceGenerationId": "<123456789012345678>",
                  "EnqueuedTime": "2018-03-29T20:34:10.7990000Z",
                  "StreamId": null
            }
      }
]
 ```

Wenn Sie dieses Array mit Ereignissen als Nutzlast per Pushvorgang an TSI übertragen, wird es als ein Ereignis pro gemessenem Wert gespeichert. Auf diese Weise werden ggf. übermäßig viele Ereignisse erstellt, was meist nicht ideal ist. Beachten Sie, dass Sie Verweisdaten in TSI nutzen können, um aussagekräftige Namen als Eigenschaften hinzuzufügen.  Beispielsweise können Sie das folgende Verweisdataset erstellen: Key Property = chId:  

chId  Measure               Unit 24    Engine Oil Pressure   PSI 25    CALC Pump Rate        bbl/min

Weitere Informationen zum Verwalten von Verweisdaten in Time Series Insights finden Sie im [Artikel zu Verweisdaten](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

Ein anderes Problem in Bezug auf die erste Nutzlast ist, dass der Zeitstempel in Millisekunden angegeben ist. TSI akzeptiert nur Zeitstempel mit ISO-Formatierung. Eine Lösung ist das Belassen des Standardverhaltens von Zeitstempeln in TSI, indem ein in die Warteschlange eingereihter Zeitstempel verwendet wird.

Als Alternative zur obigen Nutzlast sehen wir uns ein anderes Beispiel an.  

#### <a name="payload-2"></a>Nutzlast 2:
```json
{
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "STATE Engine State": 1,
      "unit": "NONE"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "MPC_AAAA-ZZ-001",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Well Head Px 1": -494162.8515625,
      "unit": "psi"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate": 0,
      "unit": "bbl/min"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Engine Fuel Pressure": 0,
      "unit": "psi"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Engine Oil Pressure": 0.58015072345733643,
      "unit": "psi"
}
```

Wie bei Nutzlast 1 auch, speichert TSI jeden gemessenen Wert als eindeutiges Ereignis.  Der wichtige Unterschied ist hierbei, dass TSI den *Zeitstempel* hier richtig liest (ISO-Format).  

Falls Sie die Anzahl von gesendeten Ereignissen verringern müssen, können Sie die Informationen wie unten angegeben senden.  

#### <a name="payload-3"></a>Nutzlast 3:
```json
{
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate": 0,
      "CALC Pump Rate.unit": "bbl/min"
      "Engine Oil Pressure": 0.58015072345733643,
      "Engine Oil Pressure.unit": "psi"
      "Engine Fuel Pressure": 0,
      "Engine Fuel Pressure.unit": "psi"
}
```
Unten ist ein abschließender Vorschlag angegeben.

#### <a name="payload-4"></a>Nutzlast 4:
```json
{
              "line": "Line01",
              "station": "Station 11",
              "gatewayid": "AAAA-ZZ-001",
              "deviceid": "F12XX",
              "timestamp": "2018-03-29T20:34:15.0000000Z",
              "CALC Pump Rate": {
                           "value": 0,
                           "unit": "bbl/min"
              },
              "Engine Oil Pressure": {
                           "value": 0.58015072345733643,
                           "unit": "psi"
              },
              "Engine Fuel Pressure": {
                           "value": 0,
                           "unit": "psi"
              }
}
```

In diesem Beispiel ist die Ausgabe nach der Vereinfachung des JSON-Codes angegeben:

```json
{
      "line": "Line01",
      "station": "Station 11",,
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate.value": 0,
      "CALC Pump Rate.unit": "bbl/min"
      "Engine Oil Pressure.value": 0.58015072345733643,
      "Engine Oil Pressure.unit": "psi"
      "Engine Fuel Pressure.value": 0,
      "Engine Fuel Pressure.unit": "psi"
}
```

Sie können unterschiedliche Eigenschaften für jeden Kanal im eigenen JSON-Objekt frei definieren und die Anzahl von Ereignissen trotzdem gering halten. Sie sollten aber berücksichtigen, dass für diesen vereinfachten Ansatz mehr Speicherplatz erforderlich ist. Die TSI-Kapazität basiert sowohl auf Ereignissen als auch auf der Größe – je nachdem, was jeweils zuerst angegeben ist.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Anzeigen Ihrer Umgebung im Time Series Insights-Explorer](https://insights.timeseries.azure.com)
