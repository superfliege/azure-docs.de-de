---
title: Antworten auf Warnungen in OMS Log Analytics | Microsoft-Dokumentation
description: "Mit Warnungen in Log Analytics werden wichtige Informationen in Ihrem OMS-Repository identifiziert, und Sie können proaktiv über Probleme informiert werden oder Aktionen aufrufen, um zu versuchen, die Probleme zu beheben.  In diesem Artikel wird beschrieben, wie Sie eine Warnungsregel erstellen, und es werden die verschiedenen Aktionen vorgestellt, die Sie durchführen können."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d936cf467ee7043b171cfc845f247f891f52f599
ms.sourcegitcommit: 4d90200f49cc60d63015bada2f3fc4445b34d4cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a>Hinzufügen von Aktionen zu Warnungsregeln in Log Analytics
Wenn eine [Warnung in Log Analytics erstellt wird](log-analytics-alerts.md), können Sie optional [eine Warnungsregel konfigurieren](log-analytics-alerts.md), um eine oder mehrere Aktionen auszuführen.  Dieser Artikel beschreibt die verschiedenen verfügbaren Aktionen und Details zu ihrer jeweiligen Konfiguration.

| Aktion | Beschreibung |
|:--|:--|
| [E-Mail](#email-actions) | Senden einer E-Mail mit den Details der Warnung an einen oder mehrere Empfänger |
| [Webhook](#webhook-actions) | Aufrufen eines externen Prozesses mit einer einzelnen HTTP POST-Anforderung |
| [Runbook](#runbook-actions) | Starten eines Runbooks in Azure Automation |


## <a name="email-actions"></a>E-Mail-Aktionen
Bei E-Mail-Aktionen wird eine E-Mail mit den Details der Warnung an einen oder mehrere Empfänger gesendet.  Sie können den Betreff der E-Mail angeben, aber der Inhalt ist ein von Log Analytics erstelltes Standardformat.  Darin sind zusammenfassende Informationen enthalten, z.B. der Name der Warnung sowie die Details von bis zu zehn Datensätzen, die von der Protokollsuche zurückgegeben werden.  Außerdem ist ein Link zu einer Protokollsuche in Log Analytics enthalten, mit der alle Datensätze dieser Abfrage zurückgegeben werden.   Der Absender der E-Mail lautet *Microsoft Operations Management Suite Team &lt;noreply@oms.microsoft.com&gt;*. 

E-Mail-Aktionen erfordern die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Betreff |Der Betreff der E-Mail.  Sie können den Haupttext der E-Mail nicht ändern. |
| Empfänger |Adressen aller E-Mail-Empfänger.  Verwenden Sie als Trennzeichen ein Semikolon (;), wenn Sie mehrere Adressen angeben. |


## <a name="webhook-actions"></a>Webhookaktionen

Mit Webhookaktionen können Sie einen externen Prozess über eine HTTP POST-Anforderung aufrufen.  Der aufgerufene Dienst sollte Webhooks unterstützen, und es sollte festgelegt werden, wie empfangene Nutzlasten verwendet werden.  Sie können auch eine REST-API aufrufen, die keine spezielle Unterstützung für Webhooks bietet, solange die Anforderung ein für die API verständliches Format hat.  Beispiele für die Verwendung eines Webhooks als Antwort auf eine Warnung sind das Senden einer Nachricht in [Slack](http://slack.com) oder Erstellen eines Vorfalls in [PagerDuty](http://pagerduty.com/).  Eine vollständige exemplarische Vorgehensweise für die Erstellung einer Warnungsregel mit einem Webhook zum Aufrufen von Slack finden Sie unter [Webhooks in Log Analytics-Warnungen](log-analytics-alerts-webhooks.md).

Webhook-Aktionen erfordern die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Webhook-URL |Die URL des Webhooks. |
| Benutzerdefinierte JSON-Nutzlast |Benutzerdefinierte Nutzlast, die mit dem Webhook gesendet wird.  Weitere Informationen siehe unten. |


Webhooks enthalten eine URL und eine Nutzlast im JSON-Format, wobei es sich um die an den externen Dienst gesendeten Daten handelt.  Die Nutzlast enthält standardmäßig die Werte in der folgenden Tabelle.  Sie können diese Nutzlast auch durch eine benutzerdefinierte eigene Nutzlast ersetzen.  In diesem Fall können Sie die Variablen in der Tabelle für die einzelnen Parameter verwenden, um deren Wert in die benutzerdefinierte Nutzlast einzubinden.

>[!NOTE]
> Wenn für Ihren Arbeitsbereich ein Upgrade auf die [neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) durchgeführt wurde, hat sich die Webhooknutzlast geändert.  Details des Formats werden in [Azure Log Analytics REST API](https://aka.ms/loganalyticsapiresponse) beschrieben.  Ein Beispiel finden Sie unten in den [Beispielen](#sample-payload).

| Parameter | Variable | Beschreibung |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Der Name der Warnungsregel. |
| AlertThresholdOperator |#thresholdoperator |Schwellenwertoperator für die Warnungsregel  *Größer als* oder *Kleiner als* |
| AlertThresholdValue |#thresholdvalue |Wert des Schwellenwerts für die Warnungsregel |
| LinkToSearchResults |#linktosearchresults |Link zur Log Analytics-Protokollsuche, mit der die Datensätze aus der Abfrage zurückgegeben werden, mit der die Warnung erstellt wurde |
| ResultCount |#searchresultcount |Anzahl von Datensätzen in den Suchergebnissen |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Endzeit für die Abfrage im UTC-Format |
| SearchIntervalInSeconds |#searchinterval |Zeitfenster für die Warnungsregel |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Startzeit für die Abfrage im UTC-Format |
| SearchQuery |#searchquery |Von der Warnungsregel verwendete Protokollsuchabfrage |
| SearchResults |Siehe unten |Von der Abfrage im JSON-Format zurückgegebene Datensätze.  Auf die ersten 5.000 Datensätze beschränkt. |
| WorkspaceID |#workspaceid |ID Ihres OMS-Arbeitsbereichs |

Sie können beispielsweise die folgende benutzerdefinierte Nutzlast angeben, die einen einzelnen Parameter wie *text*enthält.  Der Dienst, der von diesem Webhook aufgerufen wird, erwartet diesen Parameter.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Diese Beispielnutzlast wird ähnlich wie hier dargestellt aufgelöst, wenn sie an den Webhook gesendet wird.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Um die Suchergebnisse in eine benutzerdefinierte Nutzlast einzubinden, fügen Sie der JSON-Nutzlast die folgende Zeile als Eigenschaft der obersten Ebene hinzu.  

    "IncludeSearchResults":true

Sie können beispielsweise Folgendes verwenden, um eine benutzerdefinierte Nutzlast zu erstellen, die nur den Warnungsnamen und die Suchergebnisse enthält. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Ein vollständiges Beispiel für die Erstellung einer Warnungsregel mit einem Webhook, um einen externen Dienst zu starten, können Sie unter [Erstellen einer Warnungs-Webhook-Aktion in OMS Log Analytics zum Senden einer Nachricht an Slack](log-analytics-alerts-webhooks.md) durchgehen.


## <a name="runbook-actions"></a>Runbookaktionen
Bei Runbookaktionen wird ein Runbook in Azure Automation gestartet.  Zum Verwenden dieser Art von Aktion muss die [Automation-Lösung](log-analytics-add-solutions.md) im OMS-Arbeitsbereich installiert und konfiguriert sein.  Sie können eines der Runbooks im Automation-Konto auswählen, die Sie in der Automation-Lösung konfiguriert haben.

Runbook-Aktionen erfordern die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:---|
| Runbook | Runbook, das Sie starten möchten, wenn eine Warnung erstellt wird. |
| Run on (Ausführen auf) | Geben Sie **Azure** an, um das Runbook in der Cloud auszuführen.  Geben Sie **Hybrid Worker** an, um das Runbook auf einem Agent auszuführen, auf dem [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installiert ist.  |

Bei Runbookaktionen wird das Runbook mit einem [Webhook](../automation/automation-webhooks.md) gestartet.  Wenn Sie die Warnungsregel erstellen, wird für das Runbook automatisch ein neuer Webhook mit dem Namen **OMS Alert Remediation** gefolgt von einer GUID erstellt.  

Sie können die Parameter des Runbooks nicht direkt ausfüllen, aber der [$WebhookData-Parameter](../automation/automation-webhooks.md) enthält die Details zur Warnung, einschließlich der Ergebnisse der Protokollsuche, die sie ausgelöst hat.  Das Runbook muss **$WebhookData** als Parameter definieren, damit ein Zugriff auf die Eigenschaften der Warnung möglich ist.  Die Warnungsdaten sind jetzt in einer einzelnen Eigenschaft mit dem Namen **SearchResult** (für Runbookaktionen und Webhookaktionen mit Standardnutzlast) oder **SearchResults** (Webhookaktionen mit benutzerdefinierter Nutzlast einschließlich **IncludeSearchResults: true**) in der **RequestBody**-Eigenschaft von **$WebhookData** im Json-Format verfügbar.  Darin enthalten sind die Eigenschaften, die in der folgenden Tabelle aufgeführt sind.

>[!NOTE]
> Wenn für Ihren Arbeitsbereich ein Upgrade auf die [neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) durchgeführt wurde, hat sich die Runbooknutzlast geändert.  Details des Formats werden in [Azure Log Analytics REST API](https://aka.ms/loganalyticsapiresponse) beschrieben.  Ein Beispiel finden Sie unten in den [Beispielen](#sample-payload).  

| Knoten | Beschreibung |
|:--- |:--- |
| id |Pfad und GUID der Suche |
| __metadata |Informationen zur Warnung, einschließlich der Anzahl der Datensätze und des Status der Suchergebnisse |
| value |Separater Eintrag für jeden Datensatz in den Suchergebnissen.  Die Details des Eintrags stimmen mit den Eigenschaften und Werten des Datensatzes überein. |

Das folgende Runbook würde beispielsweise die von der Protokollsuche zurückgegebenen Datensätze extrahieren und basierend auf dem Typ der einzelnen Datensätze verschiedene Eigenschaften zuweisen.  Beachten Sie, dass das Runbook mit der Konvertierung von **RequestBody** aus dem JSON-Format beginnt, damit es in PowerShell als Objekt verarbeitet werden kann.

>[!NOTE]
> Beide dieser Runbooks verwenden **SearchResult**, also die Eigenschaft, die Ergebnisse für Runbookaktionen und Webhookaktionen mit Standardnutzlast enthält.  Wenn das Runbook aus einer Webhookantwort mit einer benutzerdefinierten Nutzlast aufgerufen würde, müssten Sie diese Eigenschaft in **SearchResults** ändern.

Das folgende Runbook funktioniert mit der Nutzlast aus einem [veralteten Log Analytics-Arbeitsbereich](log-analytics-log-search-upgrade.md).

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResult.value

    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer

        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }

        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }

Das folgende Runbook funktioniert mit der Nutzlast aus einem [aktualisierten Log Analytics-Arbeitsbereich](log-analytics-log-search-upgrade.md).

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody

    # Get all metadata properties    
    $AlertRuleName = $RequestBody.AlertRuleName
    $AlertThresholdOperator = $RequestBody.AlertThresholdOperator
    $AlertThresholdValue = $RequestBody.AlertThresholdValue
    $AlertDescription = $RequestBody.Description
    $LinktoSearchResults =$RequestBody.LinkToSearchResults
    $ResultCount =$RequestBody.ResultCount
    $Severity = $RequestBody.Severity
    $SearchQuery = $RequestBody.SearchQuery
    $WorkspaceID = $RequestBody.WorkspaceId
    $SearchWindowStartTime = $RequestBody.SearchIntervalStartTimeUtc
    $SearchWindowEndTime = $RequestBody.SearchIntervalEndtimeUtc
    $SearchWindowInterval = $RequestBody.SearchIntervalInSeconds

    # Get detailed search results
    if($RequestBody.SearchResult -ne $null)
    {
        $SearchResultRows    = $RequestBody.SearchResult.tables[0].rows 
        $SearchResultColumns = $RequestBody.SearchResult.tables[0].columns;

        foreach ($SearchResultRow in $SearchResultRows)
        {   
            $Column = 0
            $Record = New-Object –TypeName PSObject 
        
            foreach ($SearchResultColumn in $SearchResultColumns)
            {
                $Name = $SearchResultColumn.name
                $ColumnValue = $SearchResultRow[$Column]
                $Record | Add-Member –MemberType NoteProperty –Name $name –Value $ColumnValue -Force
                        
                $Column++
            }

            # Include code to work with the record. 
            # For example $Record.Computer to get the computer property from the record.
            
        }
    }



## <a name="sample-payload"></a>Beispielnutzlast
In diesem Abschnitt wird die Beispielnutzlast für Webhook- und Runbookaktionen sowohl in einem Vorgänger- als auch [aktualisierten Log Analytics-Arbeitsbereich](log-analytics-log-search-upgrade.md) gezeigt.

### <a name="webhook-actions"></a>Webhookaktionen
In beiden Beispielen wird **SearchResult** verwendet, also die Eigenschaft, die Ergebnisse für Webhookaktionen mit Standardnutzlast enthält.  Wenn der Webhook eine benutzerdefinierte Nutzlast verwendet hat, die Suchergebnisse enthält, wäre diese Eigenschaft **SearchResults**.

#### <a name="legacy-workspace"></a>Vorgängerarbeitsbereich.
Es folgt eine Beispielnutzlast für eine Webhookaktion in einem Vorgängerarbeitsbereich.

    {
    "WorkspaceId": "workspaceID",
    "AlertRuleName": "WebhookAlert",
    "SearchQuery": "Type=Usage",
    "SearchResult": {
        "id": "subscriptions/subscriptionID/resourceGroups/ResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspace-workspaceID/search/SearchGUID|10.1.0.7|2017-09-27T10-30-38Z",
        "__metadata": {
        "resultType": "raw",
        "total": 1,
        "top": 2147483647,
        "RequestId": "SearchID|10.1.0.7|2017-09-27T10-30-38Z",
        "CoreSummaries": [
            {
            "Status": "Successful",
            "NumberOfDocuments": 135000000
            }
        ],
        "Status": "Successful",
        "NumberOfDocuments": 135000000,
        "StartTime": "2017-09-27T10:30:38.9453282Z",
        "LastUpdated": "2017-09-27T10:30:44.0907473Z",
        "ETag": "636421050440907473",
        "sort": [
            {
            "name": "TimeGenerated",
            "order": "desc"
            }
        ],
        "requestTime": 361
        },
        "value": [
        {
            "Computer": "-",
            "SourceSystem": "OMS",
            "TimeGenerated": "2017-09-26T13:59:59Z",
            "ResourceUri": "/subscriptions/df1ec963-d784-4d11-a779-1b3eeb9ecb78/resourcegroups/mms-eus/providers/microsoft.operationalinsights/workspaces/workspace-861bd466-5400-44be-9552-5ba40823c3aa",
            "DataType": "Operation",
            "StartTime": "2017-09-26T13:00:00Z",
            "EndTime": "2017-09-26T13:59:59Z",
            "Solution": "LogManagement",
            "BatchesWithinSla": 8,
            "BatchesOutsideSla": 0,
            "BatchesCapped": 0,
            "TotalBatches": 8,
            "AvgLatencyInSeconds": 0.0,
            "Quantity": 0.002502,
            "QuantityUnit": "MBytes",
            "IsBillable": false,
            "MeterId": "a4e29a95-5b4c-408b-80e3-113f9410566e",
            "LinkedMeterId": "00000000-0000-0000-0000-000000000000",
            "id": "954f7083-cd55-3f0a-72cb-3d78cd6444a3",
            "Type": "Usage",
            "MG": "00000000-0000-0000-0000-000000000000",
            "__metadata": {
            "Type": "Usage",
            "TimeGenerated": "2017-09-26T13:59:59Z"
            }
        }
        ]
    },
    "SearchIntervalStartTimeUtc": "2017-09-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2017-09-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 1,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2017-09-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Type%3DUsage",
    "Description": null,
    "Severity": "Low"
    }


#### <a name="upgraded-workspace"></a>Aktualisierter Arbeitsbereich.
Es folgt eine Beispielnutzlast für eine Webhookaktion in einem aktualisierten Arbeitsbereich.

    {
    "WorkspaceId": "workspaceID",
    "AlertRuleName": "WebhookAlert",
    "SearchQuery": "Usage",
    "SearchResult": {
        "tables": [
        {
            "name": "PrimaryResult",
            "columns": [
            {
                "name": "TenantId",
                "type": "string"
            },
            {
                "name": "Computer",
                "type": "string"
            },
            {
                "name": "TimeGenerated",
                "type": "datetime"
            },
            {
                "name": "SourceSystem",
                "type": "string"
            },
            {
                "name": "StartTime",
                "type": "datetime"
            },
            {
                "name": "EndTime",
                "type": "datetime"
            },
            {
                "name": "ResourceUri",
                "type": "string"
            },
            {
                "name": "LinkedResourceUri",
                "type": "string"
            },
            {
                "name": "DataType",
                "type": "string"
            },
            {
                "name": "Solution",
                "type": "string"
            },
            {
                "name": "BatchesWithinSla",
                "type": "long"
            },
            {
                "name": "BatchesOutsideSla",
                "type": "long"
            },
            {
                "name": "BatchesCapped",
                "type": "long"
            },
            {
                "name": "TotalBatches",
                "type": "long"
            },
            {
                "name": "AvgLatencyInSeconds",
                "type": "real"
            },
            {
                "name": "Quantity",
                "type": "real"
            },
            {
                "name": "QuantityUnit",
                "type": "string"
            },
            {
                "name": "IsBillable",
                "type": "bool"
            },
            {
                "name": "MeterId",
                "type": "string"
            },
            {
                "name": "LinkedMeterId",
                "type": "string"
            },
            {
                "name": "Type",
                "type": "string"
            }
            ],
            "rows": [
            [
                "workspaceID",
                "-",
                "2017-09-26T13:59:59Z",
                "OMS",
                "2017-09-26T13:00:00Z",
                "2017-09-26T13:59:59Z",
                "/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.operationalinsights/workspaces/workspace-workspaceID",
                null,
                "Operation",
                "LogManagement",
                8,
                0,
                0,
                8,
                0,
                0.002502,
                "MBytes",
                false,
                "a4e29a95-5b4c-408b-80e3-113f9410566e",
                "00000000-0000-0000-0000-000000000000",
                "Usage"
            ]
            ]
        }
        ]
    },
    "SearchIntervalStartTimeUtc": "2017-09-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2017-09-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 1,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2017-09-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }


### <a name="runbooks"></a>Runbooks

#### <a name="legacy-workspace"></a>Vorgängerarbeitsbereich
Es folgt eine Beispielnutzlast für eine Runbookaktion in einem Vorgängerarbeitsbereich.

    {
        "SearchResult": {
            "id": "subscriptions/subscriptionID/resourceGroups/ResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspace-workspaceID/search/searchGUID|10.1.0.7|TimeStamp",
            "__metadata": {
                "resultType": "raw",
                "total": 1,
                "top": 2147483647,
                "RequestId": "searchGUID|10.1.0.7|2017-09-27T10-51-43Z",
                "CoreSummaries": [{
                    "Status": "Successful",
                    "NumberOfDocuments": 135000000
                }],
                "Status": "Successful",
                "NumberOfDocuments": 135000000,
                "StartTime": "2017-09-27T10:51:43.3075124Z",
                "LastUpdated": "2017-09-27T10:51:51.1002092Z",
                "ETag": "636421063111002092",
                "sort": [{
                    "name": "TimeGenerated",
                    "order": "desc"
                }],
                "requestTime": 511
            },
            "value": [{
                "Computer": "-",
                "SourceSystem": "OMS",
                "TimeGenerated": "2017-09-26T13:59:59Z",
                "ResourceUri": "/subscriptions/AnotherSubscriptionID/resourcegroups/SampleResourceGroup/providers/microsoft.operationalinsights/workspaces/workspace-workspaceID",
                "DataType": "Operation",
                "StartTime": "2017-09-26T13:00:00Z",
                "EndTime": "2017-09-26T13:59:59Z",
                "Solution": "LogManagement",
                "BatchesWithinSla": 8,
                "BatchesOutsideSla": 0,
                "BatchesCapped": 0,
                "TotalBatches": 8,
                "AvgLatencyInSeconds": 0.0,
                "Quantity": 0.002502,
                "QuantityUnit": "MBytes",
                "IsBillable": false,
                "MeterId": "a4e29a95-5b4c-408b-80e3-113f9410566e",
                "LinkedMeterId": "00000000-0000-0000-0000-000000000000",
                "id": "954f7083-cd55-3f0a-72cb-3d78cd6444a3",
                "Type": "Usage",
                "MG": "00000000-0000-0000-0000-000000000000",
                "__metadata": {
                    "Type": "Usage",
                    "TimeGenerated": "2017-09-26T13:59:59Z"
                }
            }]
        }
    }

#### <a name="upgraded-workspace"></a>Aktualisierter Arbeitsbereich
Es folgt eine Beispielnutzlast für eine Runbookaktion in einem aktualisierten Arbeitsbereich.

    {
    "WorkspaceId": "workspaceID",
    "AlertRuleName": "AutomationAlert",
    "SearchQuery": "Usage",
    "SearchResult": {
        "tables": [
        {
            "name": "PrimaryResult",
            "columns": [
            {
                "name": "TenantId",
                "type": "string"
            },
            {
                "name": "Computer",
                "type": "string"
            },
            {
                "name": "TimeGenerated",
                "type": "datetime"
            },
            {
                "name": "SourceSystem",
                "type": "string"
            },
            {
                "name": "StartTime",
                "type": "datetime"
            },
            {
                "name": "EndTime",
                "type": "datetime"
            },
            {
                "name": "ResourceUri",
                "type": "string"
            },
            {
                "name": "LinkedResourceUri",
                "type": "string"
            },
            {
                "name": "DataType",
                "type": "string"
            },
            {
                "name": "Solution",
                "type": "string"
            },
            {
                "name": "BatchesWithinSla",
                "type": "long"
            },
            {
                "name": "BatchesOutsideSla",
                "type": "long"
            },
            {
                "name": "BatchesCapped",
                "type": "long"
            },
            {
                "name": "TotalBatches",
                "type": "long"
            },
            {
                "name": "AvgLatencyInSeconds",
                "type": "real"
            },
            {
                "name": "Quantity",
                "type": "real"
            },
            {
                "name": "QuantityUnit",
                "type": "string"
            },
            {
                "name": "IsBillable",
                "type": "bool"
            },
            {
                "name": "MeterId",
                "type": "string"
            },
            {
                "name": "LinkedMeterId",
                "type": "string"
            },
            {
                "name": "Type",
                "type": "string"
            }
            ],
            "rows": [
            [
                "861bd466-5400-44be-9552-5ba40823c3aa",
                "-",
                "2017-09-26T13:59:59Z",
                "OMS",
                "2017-09-26T13:00:00Z",
                "2017-09-26T13:59:59Z",
            "/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.operationalinsights/workspaces/workspace-861bd466-5400-44be-9552-5ba40823c3aa",
                null,
                "Operation",
                "LogManagement",
                8,
                0,
                0,
                8,
                0,
                0.002502,
                "MBytes",
                false,
                "a4e29a95-5b4c-408b-80e3-113f9410566e",
                "00000000-0000-0000-0000-000000000000",
                "Usage"
            ]
        }
        ]
    },
    "SearchIntervalStartTimeUtc": "2017-09-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2017-09-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 1,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2017-09-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Critical"
    }



## <a name="next-steps"></a>Nächste Schritte
- Arbeiten Sie eine exemplarische Vorgehensweise für das [Konfigurieren eines Webooks](log-analytics-alerts-webhooks.md) mit einer Warnungsregel durch.  
- Informieren Sie sich darüber, wie Sie [Runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) schreiben, um von Warnungen identifizierte Probleme zu beheben.