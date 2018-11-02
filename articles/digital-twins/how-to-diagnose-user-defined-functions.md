---
title: 'Gewusst wie: Debuggen von UDFs in Azure Digital Twins | Microsoft-Dokumentation'
description: Richtlinie zum Debuggen von UDFs in Azure Digital Twins
author: stefanmsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: stefanmsft
ms.openlocfilehash: 852b2d35ae605f5529d162d52655fd258ca07c5a
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49946095"
---
# <a name="how-to-debug-issues-with-user-defined-functions-in-azure-digital-twins"></a>Gewusst wie: Debuggen von Problemen mit benutzerdefinierten Funktionen in Azure Digital Twins

Dieser Artikel beschreibt, wie Sie benutzerdefinierte Funktionen (User-Defined Functions, UDFs) diagnostizieren können. Danach werden einige der häufigsten Szenarien vorgestellt, die bei der Arbeit mit diesen Funktionen anzutreffen sind.

## <a name="debug-issues"></a>Debuggen von Problemen

Wenn Sie wissen, wie Probleme, die in Ihrer Azure Digital Twins-Instanz auftreten, diagnostiziert werden können, können Sie auf effiziente Weise das Problem bestimmen, die Ursache des Problems ermitteln und eine Lösung finden.

### <a name="enable-log-analytics-for-your-instance"></a>Aktivieren von Log Analytics für Ihre Instanz

Protokolle und Metriken für Ihre Azure Digital Twins-Instanz werden über Azure Monitor verfügbar gemacht. Bei der folgenden Dokumentation wird davon ausgegangen, dass Sie einen [Azure Log Analytics](../log-analytics/log-analytics-queries.md)-Arbeitsbereich über das [Azure-Portal](../log-analytics/log-analytics-quick-create-workspace.md), die [Azure CLI](../log-analytics/log-analytics-quick-create-workspace-cli.md) oder über [ PowerShell](../log-analytics/log-analytics-quick-create-workspace-posh.md) erstellt haben.

> [!NOTE]
> Wenn Sie Ereignisse zum ersten Mal an **Log Analytics** senden, tritt unter Umständen eine Verzögerung von 5 Minuten auf.

Lesen Sie den Artikel [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md), um Diagnoseeinstellungen für Ihre Azure Digital Twins-Instanz über das Portal, die Azure CLI oder über PowerShell zu aktivieren. Wählen Sie alle Protokollkategorien, Metriken und Ihren Azure Log Analytics-Arbeitsbereich aus.

### <a name="trace-sensor-telemetry"></a>Nachverfolgen von Sensortelemetriedaten

Stellen Sie sicher, dass die Diagnoseeinstellungen auf Ihrer Azure Digital Twins-Instanz aktiviert sind, alle Protokollkategorien aktiviert sind und dass die Protokolle an Azure Log Analytics gesendet werden.

Um eine Sensortelemetriemeldung den entsprechenden Protokollen zuzuordnen, können Sie eine Korrelations-ID für die Ereignisdaten angeben, die gesendet werden. Legen Sie dazu die `x-ms-client-request-id`-Eigenschaft auf eine GUID fest.

Öffnen Sie nach dem Senden von Telemetriedaten Azure Log Analytics, um Protokolle mithilfe der Korrelations-ID abzufragen:

```Kusto
AzureDiagnostics
| where CorrelationId = 'yourCorrelationIdentifier'
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| *yourCorrelationIdentifier* | Die Korrelations-ID, die für die Ereignisdaten angegeben wurde. |

Wenn Sie Ihre benutzerdefinierte Funktion, protokollieren, werden diese Protokolle in Ihrer Azure Log Analytics-Instanz mit der Kategorie `UserDefinedFunction` angezeigt. Um sie abzurufen, geben Sie die folgende Abfragebedingung in Azure Log Analytics ein:

```Kusto
AzureDiagnostics
| where Category = 'UserDefinedFunction'
```

Weitere Informationen zu leistungsstarken Abfragevorgängen finden Sie unter [Erste Schritte mit Abfragen in Log Analytics](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

## <a name="identify-common-issues"></a>Identifizieren von häufig auftretenden Problemen

Für die Problembehandlung Ihrer Lösung spielen sowohl die Diagnostizierung als auch die Ermittlung von häufig auftretenden Problemen eine wichtige Rolle. Einige häufig auftretende Probleme beim Entwickeln von benutzerdefinierten Funktionen werden im Folgenden zusammengefasst.

### <a name="ensure-a-role-assignment-was-created"></a>Sicherstellen, dass eine Rollenzuweisung erstellt wurde

Wenn keine Rollenzuweisung innerhalb der Verwaltungs-API erstellt wird, hat die benutzerdefinierte Funktion keinen Zugriff, um Aktionen wie das Senden von Benachrichtigungen, Abrufen von Metadaten und Festlegen von berechneten Werten in der Topologie auszuführen.

Überprüfen Sie, ob eine Rollenzuweisung für Ihre benutzerdefinierte Funktion über die Verwaltungs-API vorhanden ist:

```plaintext
GET https://yourManagementApiUrl/api/v1.0/roleassignments?path=/&traverse=Down&objectId=yourUserDefinedFunctionId
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| *yourManagementApiUrl* | Der vollständige URL-Pfad für Ihre Verwaltungs-API  |
| *yourUserDefinedFunctionId* | Die ID der benutzerdefinierten Funktion, für die Rollenzuweisungen abgerufen werden sollen|

Wenn keine Rollenzuweisung abgerufen wird, führen Sie die Schritte in dem Artikel [Gewusst wie: Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins](./how-to-user-defined-functions.md) durch.

### <a name="check-if-the-matcher-will-work-for-a-sensors-telemetry"></a>Überprüfen, ob der Matcher bei Telemetriedaten eines Sensors funktioniert

Mit dem folgenden Aufruf anhand der Verwaltungs-API Ihrer Azure Digital Twins-Instanzen können Sie feststellen, ob ein bestimmter Matcher für den angegebenen Sensor gilt.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/matchers/yourMatcherIdentifier/evaluate/yourSensorIdentifier?enableLogging=true
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| *yourManagementApiUrl* | Der vollständige URL-Pfad für Ihre Verwaltungs-API  |
| *yourMatcherIdentifier* | Die ID des Matchers, den Sie auswerten möchten |
| *yourSensorIdentifier* | Die ID des Sensors, den Sie auswerten möchten |

Antwort:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-will-trigger"></a>Überprüfen, wodurch ein Sensor ausgelöst wird

Mit dem folgenden Aufruf anhand der Verwaltungs-API Ihrer Azure Digital Twins-Instanzen ist es möglich, die Bezeichner der benutzerdefinierten Funktionen zu ermitteln, die durch die eingehenden Telemetriedaten des angegebenen Sensors ausgelöst werden:

```plaintext
GET https://yourManagementApiUrl/api/v1.0/sensors/yourSensorIdentifier/matchers?includes=UserDefinedFunctions
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| *yourManagementApiUrl* | Der vollständige URL-Pfad für Ihre Verwaltungs-API  |
| *yourSensorIdentifier* | Die ID des Sensors, der Telemetriedaten sendet |

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

Wenn keine Benachrichtigungen von der ausgelösten benutzerdefinierten Funktion aus empfangen werden, stellen sicher, dass der Objekttypparameter Ihrer Topologie dem Typ des Bezeichners entspricht, der verwendet wird.

**Negativbeispiel**:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

Dieses Szenario tritt auf, weil der verwendete Bezeichner auf einen Sensor verweist, obwohl der angegebene Objekttyp der Topologie „Space“ lautet.

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

1. **Drosselung**: Wenn Ihre benutzerdefinierte Funktion die Ausführungsratenlimits überschreitet, die im Artikel [Dienstlimits der öffentlichen Vorschau](./concepts-service-limits.md) genannt werden, wird sie gedrosselt. Bei einer Drosselung können weitere Vorgänge erst dann erfolgreich ausgeführt werden, wenn die Limits verfallen.

1. **Daten nicht gefunden**: Wenn Ihre benutzerdefinierte Funktion versucht, auf Metadaten zuzugreifen, die nicht vorhanden sind, tritt bei dem Vorgang ein Fehler auf.

1. **Nicht autorisiert**: Wenn für Ihre benutzerdefinierte Funktion keine Rollenzuweisung festgelegt ist oder nicht ausreichende Berechtigungen für den Zugriff auf bestimmte Metadaten aus der Topologie besitzt, tritt bei dem Vorgang ein Fehler auf.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [die Überwachung und Protokolle in Azure Digital Twins aktiviert werden](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).
