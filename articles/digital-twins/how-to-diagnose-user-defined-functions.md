---
title: 'Gewusst wie: Debuggen von UDFs in Azure Digital Twins | Microsoft-Dokumentation'
description: Richtlinie zum Debuggen von UDFs in Azure Digital Twins.
author: stefanmsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: stefanmsft
ms.custom: seodec18
ms.openlocfilehash: 6122cd4507ed0883d1b78ca519269c25098e55ff
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961413"
---
# <a name="how-to-debug-user-defined-functions-in-azure-digital-twins"></a>Debuggen von benutzerdefinierten Funktionen in Azure Digital Twins

Dieser Artikel beschreibt, wie Sie benutzerdefinierte Funktionen diagnostizieren und debuggen. Danach werden einige der häufigsten Szenarien vorgestellt, die beim Debuggen dieser Funktionen anzutreffen sind.

>[!TIP]
> Lesen Sie [Vorgehensweise: Konfigurieren von Überwachung und Protokollierung](./how-to-configure-monitoring.md), um Informationen zum Einrichten der Debuggingtools in Azure Digital Twins mithilfe von Aktivitätsprotokollen, Diagnoseprotokollen und Azure Monitor zu erhalten.

## <a name="debug-issues"></a>Debuggen von Problemen

Wenn Sie wissen, wie Probleme in Ihrer Azure Digital Twins-Instanz diagnostiziert werden, können Sie auf effiziente Weise das Problem und seine Ursache ermitteln und eine Lösung finden.

### <a name="enable-log-analytics-for-your-instance"></a>Aktivieren der Protokollanalyse für Ihre Instanz

Protokolle und Metriken für Ihre Azure Digital Twins-Instanz werden in Azure Monitor angezeigt. Bei dieser Dokumentation wird davon ausgegangen, dass Sie einen [Azure Monitor-Protokolle](../azure-monitor/log-query/log-query-overview.md)-Arbeitsbereich über das [Azure-Portal](../azure-monitor/learn/quick-create-workspace.md), die [Azure-Befehlszeilenschnittstelle](../azure-monitor/learn/quick-create-workspace-cli.md) oder [PowerShell](../azure-monitor/learn/quick-create-workspace-posh.md) erstellt haben.

> [!NOTE]
> Wenn Sie zum ersten Mal Ereignisse an Azure Monitor-Protokolle senden, tritt unter Umständen eine Verzögerung von 5 Minuten auf.

Informationen zum Konfigurieren von Überwachung und Protokollierung für Azure Digital Twins-Ressourcen finden Sie unter [Vorgehensweise: Konfigurieren von Überwachung und Protokollierung](./how-to-configure-monitoring.md).

Informationen zum Konfigurieren von Diagnoseprotokolleinstellungen in Azure Digital Twins über das Portal, die Azure-Befehlszeilenschnittstelle oder PowerShell finden Sie im Artikel [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../azure-monitor/platform/diagnostic-logs-overview.md).

>[!IMPORTANT]
> Wählen Sie alle Protokollkategorien, Metriken und Ihren Azure Log Analytics-Arbeitsbereich aus.

### <a name="trace-sensor-telemetry"></a>Nachverfolgen von Sensortelemetriedaten

Vergewissern Sie sich beim Verfolgen von Sensortelemetriedaten, dass die Diagnoseeinstellungen für Ihre Azure Digital Twins-Instanz aktiviert sind. Stellen Sie sicher, dass alle gewünschten Protokollkategorien ausgewählt sind. Bestätigen Sie abschließend, dass die gewünschten Protokolle an Azure Monitor-Protokolle gesendet werden.

Um eine Sensortelemetriemeldung den entsprechenden Protokollen zuzuordnen, können Sie eine Korrelations-ID für die Ereignisdaten angeben, die gesendet werden. Legen Sie dazu die `x-ms-client-request-id`-Eigenschaft auf eine GUID fest.

Öffnen Sie nach dem Senden von Telemetriedaten die Protokollanalyse, um Protokolle mithilfe der Korrelations-ID abzufragen:

```Kusto
AzureDiagnostics
| where CorrelationId == 'YOUR_CORRELATION_IDENTIFIER'
```

| Abfragewert | Ersetzen durch |
| --- | --- |
| YOUR_CORRELATION_IDENTIFIER | Die Korrelations-ID, die für die Ereignisdaten angegeben wurde. |

Wenn Sie die Protokollierung für Ihre benutzerdefinierte Funktion aktivieren, werden diese Protokolle in Ihrer Instanz der Protokollanalyse mit der Kategorie `UserDefinedFunction` angezeigt. Um sie abzurufen, geben Sie die folgende Abfragebedingung in der Protokollanalyse ein:

```Kusto
AzureDiagnostics
| where Category == 'UserDefinedFunction'
```

Weitere Informationen zu leistungsstarken Abfragevorgängen finden Sie unter [Erste Schritte mit Abfragen in Log Analytics](../azure-monitor/log-query/get-started-queries.md).

## <a name="identify-common-issues"></a>Identifizieren von häufig auftretenden Problemen

Für die Problembehandlung Ihrer Lösung spielen sowohl die Diagnostizierung als auch die Ermittlung von häufig auftretenden Problemen eine wichtige Rolle. Einige häufig auftretende Probleme beim Entwickeln von benutzerdefinierten Funktionen werden in den folgenden Unterabschnitten zusammengefasst.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="check-if-a-role-assignment-was-created"></a>Überprüfen, ob eine Rollenzuweisung erstellt wurde

Wenn keine Rollenzuweisung innerhalb der Verwaltungs-API erstellt wird, hat die benutzerdefinierte Funktion keinen Zugriff zum Ausführen von Aktionen wie dem Senden von Benachrichtigungen, Abrufen von Metadaten und Festlegen von berechneten Werten in der Topologie.

Überprüfen Sie, ob eine Rollenzuweisung für Ihre benutzerdefinierte Funktion über die Verwaltungs-API vorhanden ist:

```plaintext
GET YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=Down&objectId=YOUR_USER_DEFINED_FUNCTION_ID
```

| Parameterwert | Ersetzen durch |
| --- | --- |
| YOUR_USER_DEFINED_FUNCTION_ID | Die ID der benutzerdefinierten Funktion, für die Rollenzuweisungen abgerufen werden sollen|

Informationen, wenn keine Rollenzuweisungen vorhanden sind, finden Sie unter [Gewusst wie: Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins](./how-to-user-defined-functions.md).

### <a name="check-if-the-matcher-works-for-a-sensors-telemetry"></a>Überprüfen, ob der Matcher für die Telemetriedaten eines Sensors funktioniert

Mit dem folgenden Aufruf der Verwaltungs-API Ihrer Azure Digital Twins-Instanzen können Sie feststellen, ob ein bestimmter Matcher für den angegebenen Sensor gilt.

```plaintext
GET YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_IDENTIFIER/evaluate/YOUR_SENSOR_IDENTIFIER?enableLogging=true
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_MATCHER_IDENTIFIER* | Die ID des Matchers, den Sie auswerten möchten |
| *YOUR_SENSOR_IDENTIFIER* | Die ID des Sensors, den Sie auswerten möchten |

Antwort:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-triggers"></a>Überprüfen, wodurch ein Sensor ausgelöst wird

Mit dem folgenden Aufruf der Verwaltungs-APIs von Azure Digital Twins können Sie die Bezeichner der benutzerdefinierten Funktionen ermitteln, die durch die eingehenden Telemetriedaten des angegebenen Sensors ausgelöst werden:

```plaintext
GET YOUR_MANAGEMENT_API_URL/sensors/YOUR_SENSOR_IDENTIFIER/matchers?includes=UserDefinedFunctions
```

| Parameter | Ersetzen durch |
| --- | --- |
| *YOUR_SENSOR_IDENTIFIER* | Die ID des Sensors zum Senden von Telemetriedaten |

Antwort:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Problem mit dem Empfang von Benachrichtigungen

Wenn Sie keine Benachrichtigungen von der ausgelösten benutzerdefinierten Funktion erhalten, vergewissern Sie sich, dass der Objekttypparameter Ihrer Topologie dem Typ des verwendeten Bezeichners entspricht.

**Negativbeispiel**:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

Dieses Szenario tritt auf, weil der verwendete Bezeichner auf einen Sensor verweist, obwohl der angegebene Objekttyp der Topologie `Space` lautet.

**Positivbeispiel**:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

Die einfachste Möglichkeit, dieses Problem zu verhindern, besteht darin, die `Notify`-Methode für das Metadatenobjekt zu verwenden.

Beispiel:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Allgemeine Diagnoseausnahmen

Wenn Sie Diagnoseeinstellungen aktivieren, können die folgenden allgemeinen Ausnahmen auftreten:

1. **Drosselung**: Wenn Ihre benutzerdefinierte Funktion die Ausführungsratenlimits überschreitet, die im Artikel [Dienstlimits der öffentlichen Vorschau](./concepts-service-limits.md) genannt werden, wird sie gedrosselt. Es werden keine weiteren Vorgänge erfolgreich ausgeführt, bis die Drosselungslimits ablaufen.

1. **Daten nicht gefunden:** Wenn Ihre benutzerdefinierte Funktion versucht, auf Metadaten zuzugreifen, die nicht vorhanden sind, tritt bei dem Vorgang ein Fehler auf.

1. **Nicht autorisiert:** Wenn für Ihre benutzerdefinierte Funktion keine Rollenzuweisung festgelegt ist oder sie keine ausreichenden Berechtigungen für den Zugriff auf bestimmte Metadaten aus der Topologie besitzt, tritt bei dem Vorgang ein Fehler auf.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [die Überwachung und Protokolle in Azure Digital Twins aktiviert werden](../azure-monitor/platform/activity-logs-overview.md).
