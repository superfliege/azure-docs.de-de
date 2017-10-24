---
title: "Überwachen der Integrität von Azure IoT Hub | Microsoft-Dokumentation"
description: "Verwenden Sie Azure Monitor und Azure Resource Health zur schnellen Überwachung Ihres IoT Hubs und Diagnose von Problemen."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/09/2017
ms.author: kgremban
ms.openlocfilehash: 3051af03d0c1433db98bcc674a072188e7ce80e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Schnelle Überwachung der Integrität von Azure IoT Hub und Diagnose von Problemen

Unternehmen, die Azure IoT Hub implementieren, erwarten zuverlässige Leistung von ihren Ressourcen. Damit Sie Ihre Vorgänge immer im Blick haben, ist IoT Hub vollständig in [Azure Monitor][lnk-AM] und [Azure Resource Health][lnk-ARH] integriert. Diese beiden Dienste liefern Ihnen gemeinsam die Daten, die Sie benötigen, um Ihre IoT-Lösungen in Betrieb und einem ordnungsgemäßen Zustand zu halten. 

Azure Monitor ist eine einzelne Überwachungs- und Protokollierungsquelle für alle Azure-Dienste. Sie können die Protokolle, die Azure Monitor generiert, zur benutzerdefinierten Verarbeitung an OMS Log Analytics, Event Hubs oder Azure Storage senden. Die Metriken und Diagnoseeinstellungen von Azure Monitor liefern Ihnen Echtzeiteinblicke in die Leistung Ihrer Ressourcen. Um mehr über das [Verwenden von Azure Monitor](#use-azure-monitor) mit Ihrem IoT Hub zu erfahren, lesen Sie nun diesen Artikel. 

Azure Resource Health unterstützt Sie bei der Diagnose und bei Supportanfragen, wenn ein Problem mit Azure Auswirkungen auf Ihre Ressourcen hat. Ein personalisiertes Dashboard zeigt aktuelle und vergangene Integritätsstatus für Ihre IoT Hubs. Um mehr über das [Verwenden von Azure Resource Health](#use-azure-resource-health) mit Ihrem IoT Hub zu erfahren, lesen Sie nun diesen Artikel. 

Zusätzlich zur Integration in diese beiden Dienste bietet IoT Hub auch eigene Metriken, die Sie verwenden können, um den Status Ihrer IoT-Ressourcen zu verstehen. Weitere Informationen finden Sie unter [Grundlegendes zu IoT Hub-Metriken][lnk-metrics].

## <a name="use-azure-monitor"></a>Verwenden von Azure Monitor

Azure Monitor liefert Diagnoseinformationen auf Ressourcenebene, was bedeutet, dass Sie Vorgänge überwachen können, die in Ihrem IoT Hub erfolgen. 

Die Diagnoseeinstellungen von Azure Monitor ersetzen die IoT Hub-Vorgangsüberwachung. Wenn Sie derzeit die Vorgangsüberwachung verwenden, sollten Sie Ihre Workflows migrieren. Weitere Informationen finden Sie unter [Migrieren Ihres IoT Hub von der Vorgangsüberwachung zu Diagnoseeinstellungen][lnk-migrate].

Weitere Informationen zu den bestimmten Metriken und Ereignissen, die Azure Monitor überwacht, finden Sie unter [Unterstützte Metriken von Azure Monitor][lnk-AM-metrics] und [Unterstützte Dienste, Schemas und Kategorien für Azure-Diagnoseprotokolle][lnk-AM-schemas].

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Grundlegendes zu den Protokollen

Azure Monitor verfolgt verschiedene Vorgänge nach, die in IoT Hub auftreten. Jede Kategorie verfügt über ein Schema, das definiert, wie Ereignisse in dieser Kategorie gemeldet werden. 

#### <a name="connections"></a>Verbindungen

Die Kategorie für Verbindungen verfolgt Fehler nach, die auftreten, wenn Geräte Verbindungen mit dem IoT Hub herstellen oder trennen. Das Nachverfolgen dieser Kategorie ist nützlich zum Bestimmen nicht autorisierter Verbindungsversuche und zum Nachverfolgen, wann eine Verbindung mit Geräten in Bereichen mit schlechter Konnektivität unterbrochen wird.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Information",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}", 
    "location": "Resource location"
}
```

#### <a name="cloud-to-device-commands"></a>C2D-Befehle

Die Kategorie der C2D-Befehle dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur C2D-Nachrichtenpipeline stehen. Zu dieser Kategorie gehören Fehler, die beim Senden von C2D-Nachrichten (z.B. nicht autorisierter Absender), Empfangen von C2D-Nachrichten (z.B. Zustellungsanzahl überschritten) und C2D-Nachrichtenfeedback (z.B. Feedback abgelaufen) auftreten. Diese Kategorie fängt keine Fehler eines Geräts ab, das eine C2D-Nachricht nicht ordnungsgemäß verarbeitet, wenn die C2D-Nachricht erfolgreich übermittelt wurde.

```json
{
    "time": " UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "messageExpired",
    "category": "C2DCommands",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddresss\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
    "location": "Resource location"
}
```

#### <a name="device-identity-operations"></a>Geräte-Identitätsvorgänge

Die Kategorie für Geräte-Identitätsvorgänge dient der Nachverfolgung von Fehlern, die auftreten, wenn Sie versuchen, einen Eintrag in der Identitätsregistrierung Ihres IoT Hubs zu erstellen, zu aktualisieren oder zu löschen. Das Nachverfolgen dieser Kategorie ist für Bereitstellungsszenarien nützlich.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "get",
    "category": "DeviceIdentityOperations",
    "level": "Error",    
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"maskedIpAddress\":\"<maskedIpAddress>\",\"deviceId\":\"<deviceId>\", \"statusCode\":\"4XX\"}",
    "location": "Resource location"
}
```

#### <a name="routes"></a>Routen

Die Kategorie „Nachrichtenweiterleitung“ verfolgt Fehler, die während der Auswertung der Nachrichtenweiterleitung auftreten, und die durch IoT Hub wahrgenommene Endpunktintegrität. Diese Kategorie umfasst z.B. folgende Ereignisse: Eine Regel wird als „nicht definiert“ ausgewertet, IoT Hub markiert einen Endpunkt als inaktiv und alle anderen Fehler, die von einem Endpunkt empfangen werden. Diese Kategorie umfasst keine bestimmten Fehler zu den Nachrichten selbst (z.B. Gerätedrosselungsfehler), die in der Kategorie „Gerätetelemetrie“ gemeldet werden.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "endpointUnhealthy",
    "category": "Routes",
    "level": "Error",
    "properties": "{\"deviceId\": \"<deviceId>\",\"endpointName\":\"<endpointName>\",\"messageId\":<messageId>,\"details\":\"<errorDetails>\",\"routeName\": \"<routeName>\"}",
    "location": "Resource location"
}
```

#### <a name="device-telemetry"></a>Gerätetelemetrie

Die Kategorie für Gerätetelemetrie dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur Telemetriepipeline stehen. Zu dieser Kategorie gehören Fehler, die beim Senden von Telemetrie-Ereignissen (z.B. Drosselung) und Empfangen von Telemetrie-Ereignissen (z.B. nicht autorisierte Leser) auftreten. Diese Kategorie kann keine Fehler abfangen, die vom Code verursacht werden, der auf dem Gerät selbst ausgeführt wird.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "ingress",
    "category": "DeviceTelemetry",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"deviceId\":\"<deviceId>\",\"batching\":\"0\",\"messageSizeInBytes\":\"<messageSizeInBytes>\",\"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\",\"partitionId\":\"1\"}", 
    "location": "Resource location"
}
```

#### <a name="file-upload-operations"></a>Dateiuploadvorgänge

Die Dateiuploadkategorie dient der Nachverfolgung von Fehlern, die auf dem IoT Hub auftreten und in Beziehung zur Dateiuploadfunktionalität stehen. In diese Kategorie fällt Folgendes:

* Fehler, die beim SAS-URI auftreten, beispielsweise wenn er abläuft, bevor ein Gerät den Hub über einen abgeschlossenen Upload informiert
* Vom Gerät gemeldete Uploadfehler
* Fehler, die auftreten, wenn während der IoT Hub-Benachrichtigungserstellung eine Datei nicht im Speicher gefunden wird

Diese Kategorie kann keine Fehler abfangen, die auftreten, während das Gerät eine Datei in den Speicher hochlädt.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "ingress",
    "category": "FileUploadOperations",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"blobUri\":\"http//bloburi.com\"}",
    "location": "Resource location"
}
```

#### <a name="cloud-to-device-twin-operations"></a>Cloud-zu-Gerät-Zwillingsvorgänge

Die Kategorie der Cloud-zu-Gerät-Zwillingsvorgänge verfolgt von einem Dienst initiierte Ereignisse auf Gerätezwillingen nach. Diese Vorgänge können das Abrufen des Zwillings, Aktualisieren gemeldeter Eigenschaften und Abonnieren gewünschter Eigenschaften einschließen.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "read",
    "category": "C2DTwinOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\"}", 
    "location": "Resource location"
}
```

#### <a name="device-to-cloud-twin-operations"></a>Gerät-zu-Cloud-Zwillingsvorgänge

Die Kategorie der Gerät-zu-Cloud-Zwillingsvorgänge verfolgt von einem Gerät initiierte Ereignisse auf Gerätezwillingen nach. Diese Vorgänge können das Abrufen des Zwillings, Aktualisieren oder Ersetzen von Tags und Aktualisieren oder Ersetzen gewünschter Eigenschaften einschließen. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "update",
    "category": "D2CTwinOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authenticationType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\"}", 
    "location": "Resource location"
}
```

#### <a name="twin-queries"></a>Zwillingsabfragen

Die Kategorie der Zwillingsabfragen berichtet über Abfrageanforderungen für Gerätezwillinge, die in der Cloud initiiert werden. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "query",
    "category": "TwinQueries",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"query\":\"<twin query>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\",\"pageSize\":\"<pageSize>\", \"continuation\":\"<true, false>\", \"resultSize\":\"<resultSize>\"}", 
    "location": "Resource location"
}
```

#### <a name="jobs-operations"></a>Auftragsvorgänge

Die Kategorie der Auftragsvorgänge berichtet über Auftragsanforderungen zum Aktualisieren von Gerätezwillingen oder Aufrufen direkter Methoden auf mehreren Geräten. Diese Anforderungen werden in der Cloud initiiert. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "jobCompleted",
    "category": "JobsOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"jobId\":\"<jobId>\", \"sdkVersion\": \"<sdkVersion>\",\"messageSize\": <messageSize>,\"filter\":\"DeviceId IN ['1414ded9-b445-414d-89b9-e48e8c6285d5']\",\"startTimeUtc\":\"Wednesday, September 13, 2017\",\"duration\":\"0\"}", 
    "location": "Resource location"
}
```

#### <a name="direct-methods"></a>Direkte Methoden

Die Kategorie der direkten Methoden verfolgt Anforderung-Antwort-Interaktionen, die an die einzelnen Geräte gesendet werden. Diese Anforderungen werden in der Cloud initiiert. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "send",
    "category": "DirectMethods",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\", \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}", 
    "location": "Resource location"
}
```

### <a name="read-logs-from-azure-event-hubs"></a>Lesen von Protokollen aus Azure Event Hubs

Nachdem Sie die Ereignisprotokollierung über Diagnoseeinstellungen eingerichtet haben, können Sie Anwendungen erstellen, die die Protokolle lesen, sodass Sie anhand der darin enthaltenen Informationen Maßnahmen ergreifen können. Dieser Beispielcode ruft Protokolle von einem Event Hub ab:

```
class Program 
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}"; 
    static string monitoringEndpointName = "{your AMS event hub endpoint name}"; 
    static EventHubClient eventHubClient; 
//This is the Diagnostic Settings schema 
    class AzureMonitorDiagnosticLog 
    { 
        string time { get; set; } 
        string resourceId { get; set; } 
        string operationName { get; set; } 
        string category { get; set; } 
        string level { get; set; } 
        string resultType { get; set; } 
        string resultDescription { get; set; } 
        string durationMs { get; set; } 
        string callerIpAddress { get; set; } 
        string correlationId { get; set; } 
        string identity { get; set; } 
        string location { get; set; } 
        Dictionary<string, string> properties { get; set; } 
    }; 
    static void Main(string[] args) 
    { 
        Console.WriteLine("Monitoring. Press Enter key to exit.\n"); 
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName); 
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds; 
        CancellationTokenSource cts = new CancellationTokenSource(); 
        var tasks = new List<Task>(); 
        foreach (string partition in d2cPartitions) 
        { 
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token)); 
        } 
        Console.ReadLine(); 
        Console.WriteLine("Exiting..."); 
        cts.Cancel(); 
        Task.WaitAll(tasks.ToArray()); 
    } 
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct) 
    { 
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow); 
        while (true) 
        { 
            if (ct.IsCancellationRequested) 
            { 
                await eventHubReceiver.CloseAsync(); 
                break; 
            } 
            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10)); 
            if (eventData != null) 
            { 
                string data = Encoding.UTF8.GetString(eventData.GetBytes()); 
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data); 
                var deserializer = new JavaScriptSerializer(); 
                //deserialize json data to azure monitor object 
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result); 
 
            } 
        } 
    } 
} 
```

## <a name="use-azure-resource-health"></a>Verwenden von Azure Resource Health

Verwenden Sie Azure Resource Health, um zu überwachen, ob Ihr IoT Hub ausgeführt wird. Sie können auch erfahren, ob ein regionaler Ausfall Auswirkungen auf die Integrität Ihres IoT Hub hat. Um bestimmte Details des Integritätsstatus Ihres Azure IoT Hub zu verstehen, empfehlen wir Ihnen das [Verwenden von Azure Monitor](#use-azure-monitor). 

Azure IoT Hub zeigt die Integrität auf regionaler Ebene an. Wenn ein regionaler Ausfall Auswirkungen auf Ihren IoT Hub hat, wird der Integritätsstatus **Unbekannt** angezeigt. Weitere Informationen zu den spezifischen Integritätsprüfungen, die Azure Resource Health durchführt, finden Sie unter [Ressourcentypen und Integritätsprüfungen in Azure Resource Health][lnk-ARH-checks].

Um die Integrität Ihrer IoT Hubs zu überprüfen, gehen Sie folgendermaßen vor:

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
1. Navigieren Sie zu **Dienstintegrität** > **Ressourcenintegrität**.
1. Wählen Sie in der Dropdownliste Ihr Abonnement und **IoT Hub** aus.

Weitere Informationen zum Interpretieren von Integritätsdaten finden Sie unter [Übersicht über Azure Resource Health][lnk-ARH].

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu IoT Hub-Metriken][lnk-metrics]
- [IoT-Remoteüberwachung und -Benachrichtigungen mit Azure Logic Apps zum Herstellen einer Verbindung zwischen Ihrem IoT Hub und Postfach][lnk-monitoring-notifications]


[lnk-AM]: ../monitoring-and-diagnostics/index.yml
[lnk-ARH]: ../service-health/resource-health-overview.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-migrate]: iot-hub-migrate-to-diagnostics-settings.md
[lnk-AM-metrics]: ../monitoring-and-diagnostics/monitoring-supported-metrics.md
[lnk-AM-schemas]: ../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md
[lnk-ARH-checks]: ../service-health/resource-health-checks-resource-types.md
[lnk-monitoring-notifications]: iot-hub-monitoring-notifications-with-azure-logic-apps.md