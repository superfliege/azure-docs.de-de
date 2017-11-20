---
title: Senden von Ereignissen an eine Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: "In diesem Tutorial erfahren Sie, wie Sie Event Hubs erstellen und konfigurieren. Außerdem erfahren Sie, wie Sie eine Beispielanwendung ausführen, um Ereignisse für die Anzeige in Azure Time Series Insights mithilfe von Push zu übertragen."
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
ms.date: 11/15/2017
ms.openlocfilehash: 543fafac63423ab874c6c8e40d91a1ce0f161987
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Senden von Ereignissen an die Azure Time Series Insights-Umgebung mithilfe von Event Hub
In diesem Artikel erfahren Sie, wie Sie Event Hubs erstellen und konfigurieren. Außerdem erfahren Sie, wie Sie eine Beispielanwendung ausführen, um Ereignisse mithilfe von Push zu übertragen. Wenn Sie bereits über einen Event Hub mit Ereignissen im JSON-Format verfügen, überspringen Sie dieses Tutorial, sehen Sie sich Ihre Umgebung in [Time Series Insights](https://insights.timeseries.azure.com) an.

## <a name="configure-an-event-hub"></a>Konfigurieren eines Event Hubs
1. Führen Sie zum Erstellen eines Event Hubs die in der [Event Hub-Dokumentation](../event-hubs/event-hubs-create.md) beschriebenen Schritte aus.

2. Suchen Sie über die Suchleiste nach **Event Hub**. Klicken Sie in der Liste mit den zurückgegebenen Ergebnissen auf **Event Hubs**.

3. Klicken Sie auf den Namen Ihres Event Hubs, um ihn auszuwählen.

4. Klicken Sie unter „Entitäten“ im mittleren Konfigurationsfenster erneut auf **Event Hubs**.

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
#### <a name="output---1-event"></a>Ausgabe: ein Ereignis

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
#### <a name="output---2-events"></a>Ausgabe: zwei Ereignisse

|id|timestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Beispiel 3

#### <a name="input"></a>Eingabe

Ein JSON-Objekt mit einem geschachtelten JSON-Array, das zwei JSON-Objekte enthält.
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
#### <a name="output---2-events"></a>Ausgabe: zwei Ereignisse
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
#### <a name="output---2-events"></a>Ausgabe: zwei Ereignisse

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Nächste Schritte
Anzeigen Ihrer Umgebung im [Time Series Insights-Explorer](https://insights.timeseries.azure.com)
