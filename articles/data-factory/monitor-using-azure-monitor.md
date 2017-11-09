---
title: "Überwachen von Data Factorys mit Azure Monitor | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Data Factory-Pipelines mit Azure Monitor überwachen können, indem Sie Diagnoseprotokolle mit Informationen aus Azure Data Factory aktivieren."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: shlo
ms.openlocfilehash: 3d9ec6325e25477bf4ee0475caeca64b75b1f89f
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-data-factories-using-azure-monitor"></a>Überwachen von Data Factorys mit Azure Monitor  
Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Die Überwachung stellt Daten bereit, auf deren Grundlage die ordnungsgemäße Ausführung der Anwendung sichergestellt werden kann. Sie trägt auch zur Vermeidung potenzieller Probleme bei und hilft bei der Behandlung bereits aufgetretener Probleme. Darüber hinaus können Sie auf der Grundlage von Überwachungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen. Mithilfe dieser Kenntnisse können Sie die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder Aktionen automatisieren, die andernfalls manuell ausgeführt werden müssten.

Azure Monitor stellt Infrastrukturmetriken auf Basisebene und Protokolle für die meisten Dienste in Microsoft Azure bereit. Weitere Informationen finden Sie in der [Übersicht über Azure Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor). Azure-Diagnoseprotokolle sind von einer Ressource ausgegebene Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Data Factory gibt die Diagnoseprotokolle in Azure Monitor. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts in der allgemein verfügbaren Version (GA) verwenden, lesen Sie den Artikel [Überwachen und Verwalten von Azure Data Factory-Pipelines mit dem Azure-Portal und PowerShell](v1/data-factory-monitor-manage-pipelines.md).

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

* Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem **Speicherkonto** gespeichert werden. In den Diagnoseeinstellungen können Sie eine Aufbewahrungsdauer (in Tagen) angeben.
* Sie können Diagnoseprotokolle zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI an **Event Hubs** streamen.
* Sie können sie mit **Operations Management Suite (OMS) Log Analytics** analysieren.

Sie können ein Speicherkonto oder einen Event Hub-Namespace verwenden, das/der sich nicht im gleichen Abonnement befindet wie die Ressource, die Protokolle ausgibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden Zugriff gemäß der rollenbasierten Zugriffssteuerung auf beide Abonnements.

## <a name="set-up-diagnostic-logs"></a>Einrichten von Diagnoseprotokollen

### <a name="diagnostic-settings"></a>Diagnoseeinstellungen
Diagnoseprotokolle für Nicht-Computeressourcen werden mithilfe von Diagnoseeinstellungen konfiguriert. Diagnoseeinstellungen für ein Ressourcensteuerelement:

* Wohin Diagnoseprotokolle gesendet werden sollen (Speicherkonto, Event Hubs und/oder OMS Log Analytics)
* Welche Protokollkategorien gesendet werden sollen
* Wie lange die einzelnen Protokollkategorien in einem Speicherkonto beibehalten werden sollen
* Wenn für die Beibehaltungsdauer 0 Tage festgelegt sind, bedeutet dies, dass Protokolle unbegrenzt beibehalten werden. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden.
* Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert.
* Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

### <a name="enable-diagnostic-logs-via-rest-apis"></a>Aktivieren von Diagnoseprotokollen über die REST-API

Erstellen oder Aktualisieren einer Diagnoseeinstellung in der REST-API von Azure Monitor

**Anforderung**
```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Header**
* Ersetzen Sie `{api-version}` durch `2016-09-01`.
* Ersetzen Sie `{resource-id}` durch die Ressourcen-ID der Ressource, deren Diagnoseeinstellungen Sie bearbeiten möchten. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-portal.md).
* Legen Sie den Header `Content-Type` auf `application/json` fest.
* Legen Sie den Autorisierungsheader auf ein JSON-Webtoken fest, das Sie aus Azure Active Directory abgerufen haben. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen](../active-directory/develop/active-directory-authentication-scenarios.md).

**Text**
```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<OMSName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
} 
```

| Eigenschaft | Typ | Beschreibung |
| --- | --- | --- |
| storageAccountId |String | Die Ressourcen-ID des Speicherkontos, an das Diagnoseprotokolle gesendet werden sollen. |
| serviceBusRuleId |String | Die Service Bus-Regel-ID des Service Bus-Namespace, in dem Event Hubs für das Streaming von Diagnoseprotokollen erstellt werden sollen. Die Regel-ID hat dieses Format: {Service Bus-Ressourcen-ID}/authorizationrules/{Schlüsselname}.|
| workspaceId | Komplexer Typ | Array metrischer Zeiteinheiten und ihrer Aufbewahrungsrichtlinien. Diese Eigenschaft ist derzeit leer. |
|Metriken| Parameterwerte der Pipelineausführung, die an die aufgerufene Pipeline übergeben werden sollen| Ein JSON-Objekt, das Parameternamen Argumentwerten zuordnet | 
| Protokolle| Komplexer Typ| Der Name einer Diagnoseprotokollkategorie für einen Ressourcentyp. Um die Liste der Diagnoseprotokollkategorien für eine Ressource zu erhalten, führen Sie zuerst einen GET-Vorgang zum Abrufen von Diagnoseeinstellungen aus. |
| category| String| Array von Protokollkategorien und ihrer Aufbewahrungsrichtlinien |
| timeGrain | String | Die Granularität von Metriken, die im Zeitformat ISO 8601 erfasst werden. Muss „PT1M“ (eine Minute) sein|
| Aktiviert| Boolean | Gibt an, ob die Sammlung dieser Metrik- oder Protokollkategorie für diese Ressource aktiviert ist|
| retentionPolicy| Komplexer Typ| Beschreibt die Aufbewahrungsrichtlinie für eine Metrik- oder Protokollkategorie. Wird nur für die Speicherkonto-Option verwendet.|
| Tage| int| Anzahl der Tage, die Metriken oder Protokolle aufbewahrt werden sollen. Beim Wert 0 werden die Protokolle dauerhaft gespeichert. Wird nur für die Speicherkonto-Option verwendet. |

**Antwort**

200 – OK


```json
{
    "id": "/subscriptions/1e42591f-1f0c-4c5a-b7f2-a268f6105ec5/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<OMSName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

Abrufen von Informationen zu einer Diagnoseeinstellung in der REST-API von Azure Monitor

**Anforderung**
```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Header**
* Ersetzen Sie `{api-version}` durch `2016-09-01`.
* Ersetzen Sie `{resource-id}` durch die Ressourcen-ID der Ressource, deren Diagnoseeinstellungen Sie bearbeiten möchten. Weitere Informationen finden Sie unter „Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen“.
* Legen Sie den Header `Content-Type` auf `application/json` fest.
* Legen Sie den Autorisierungsheader auf ein JSON-Webtoken fest, das Sie aus Azure Active Directory abgerufen haben. Weitere Informationen finden Sie unter „Authentifizieren von Anforderungen“.

**Antwort**

200 – OK

```json
{
    "id": "/subscriptions/1e42591f-1f0c-4c5a-b7f2-a268f6105ec5/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/1e42591f-1f0c-4c5a-b7f2-a268f6105ec5/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/1e42591f-1f0c-4c5a-b7f2-a268f6105ec5/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/0ee78edb-a0ad-456c-a0a2-901bf542c102/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```
Weitere Informationen hier](https://msdn.microsoft.com/de-de/library/azure/dn931932.aspx)

## <a name="schema-of-logs--events"></a>Schema von Protokollen und Ereignissen

### <a name="activity-run-logs-attributes"></a>Attribute der Protokolle von Aktivitätsausführungen

```json
{  
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties:" 
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| Eigenschaft | Typ | Beschreibung | Beispiel |
| --- | --- | --- | --- |
| Ebene |String | Ebene der Diagnoseprotokolle. Bei Protokollen von Aktivitätsausführungen ist dies stets Ebene 4. | `4`  |
| correlationId |String | Eindeutige ID zum durchgängigen Nachverfolgen einer bestimmten Anforderung | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| in | String | Zeitpunkt des Ereignisses im Zeitraum, UTC-Format | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|activityRunId| String| ID der Aktivitätsausführung | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|pipelineRunId| String| ID der Pipelineausführung | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| String | Mit der Data Factory-Ressource verknüpfte Ressourcen-ID | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | Kategorie der Diagnoseprotokolle. Legen Sie diese Eigenschaft auf „ActivityRuns“ fest | `ActivityRuns` |
|level| String | Ebene der Diagnoseprotokolle. Legen Sie diese Eigenschaft auf „Informational“ fest | `Informational` |
|operationName| String |Der Name der Aktivität mit Status. Bei Status „start heartbeat“ `MyActivity -`. Bei Status „end heartbeat“ `MyActivity - Succeeded` mit Endstatus | `MyActivity - Succeeded` |
|pipelineName| String | Name der Pipeline | `MyPipeline` |
|activityName| String | Der Name der Aktivität | `MyActivity` |
|Start| String | Start der Aktivitätsausführung im Zeitraum, UTC-Format | `2017-06-26T20:55:29.5007959Z`|
|end| String | Ende der Aktivitätsausführung im Zeitraum, UTC-Format Wenn die Aktivität noch nicht beendet ist (Diagnoseprotokoll für eine startende Aktivität), wird der Standardwert `1601-01-01T00:00:00Z` festgelegt.  | `2017-06-26T20:55:29.5007959Z` |


### <a name="pipeline-run-logs-attributes"></a>Attribute der Protokolle von Pipelineausführungen

```json
{  
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties": 
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| Eigenschaft | Typ | Beschreibung | Beispiel |
| --- | --- | --- | --- |
| Ebene |String | Ebene der Diagnoseprotokolle. Bei Protokollen von Aktivitätsausführungen ist dies Ebene 4. | `4`  |
| correlationId |String | Eindeutige ID zum durchgängigen Nachverfolgen einer bestimmten Anforderung | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| in | String | Zeitpunkt des Ereignisses im Zeitraum, UTC-Format | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|runId| String| ID der Pipelineausführung | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| String | Mit der Data Factory-Ressource verknüpfte Ressourcen-ID | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | Kategorie der Diagnoseprotokolle. Legen Sie diese Eigenschaft auf „PipelineRuns“ fest | `PipelineRuns` |
|level| String | Ebene der Diagnoseprotokolle. Legen Sie diese Eigenschaft auf „Informational“ fest | `Informational` |
|operationName| String |Name der Pipeline mit Status. „Pipeline - Succeeded“ mit Endstatus nach Abschluss der Pipelineausführung| `MyPipeline - Succeeded` |
|pipelineName| String | Name der Pipeline | `MyPipeline` |
|Start| String | Start der Aktivitätsausführung im Zeitraum, UTC-Format | `2017-06-26T20:55:29.5007959Z`|
|end| String | Ende der Aktivitätsausführungen im Zeitraum, UTC-Format Wenn die Aktivität noch nicht beendet ist (Diagnoseprotokoll für eine startende Aktivität), wird der Standardwert `1601-01-01T00:00:00Z` festgelegt.  | `2017-06-26T20:55:29.5007959Z` |
|status| String | Der Endstatus der Pipelineausführung („Succeeded“ oder „Failed“) | `Succeeded`|


### <a name="trigger-run-logs-attributes"></a>Attribute der Protokolle von Triggerausführungen

```json
{ 
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
} 

```

| Eigenschaft | Typ | Beschreibung | Beispiel |
| --- | --- | --- | --- |
| Ebene |String | Ebene der Diagnoseprotokolle. Legen Sie diese für Protokolle von Aktivitätsausführungen auf 4 fest. | `4`  |
| correlationId |String | Eindeutige ID zum durchgängigen Nachverfolgen einer bestimmten Anforderung | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| in | String | Zeitpunkt des Ereignisses im Zeitraum, UTC-Format | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|triggerId| String| ID der Triggerausführung | `08587023010602533858661257311` |
|resourceId| String | Mit der Data Factory-Ressource verknüpfte Ressourcen-ID | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | Kategorie der Diagnoseprotokolle. Legen Sie diese Eigenschaft auf „PipelineRuns“ fest | `PipelineRuns` |
|level| String | Ebene der Diagnoseprotokolle. Legen Sie diese Eigenschaft auf „Informational“ fest | `Informational` |
|operationName| String |Name des Triggers mit Endstatus, ob dieser erfolgreich ausgelöst wurde. „MyTrigger - Succeeded“, wenn der Heartbeat erfolgreich war| `MyTrigger - Succeeded` |
|triggerName| String | Name des Triggers | `MyTrigger` |
|triggerType| String | Typ des Triggers (manueller Trigger oder Zeitplantrigger) | `ScheduleTrigger` |
|triggerEvent| String | Ereignis des Triggers | `ScheduleTime - 2017-07-06T01:50:25Z` |
|Start| String | Beginn der Triggerauslösung im Zeitraum, UTC-Format | `2017-06-26T20:55:29.5007959Z`|
|status| String | Endstatus, der angibt, ob der Trigger erfolgreich ausgelöst wurde („Succeeded“ oder „Failed“) | `Succeeded`|

### <a name="metrics"></a>Metriken

Mit Azure Monitor können Sie Telemetriedaten verwenden, um sich einen Überblick über Leistung und Integrität Ihrer Workloads in Azure zu verschaffen. Die wichtigsten Typen von Telemetriedaten sind Metriken (auch Leistungsindikatoren genannt), die von den meisten Azure-Ressourcen ausgegeben werden. Azure Monitor bietet Ihnen verschiedene Möglichkeiten, diese Metriken für die Überwachung und Problembehandlung zu konfigurieren und zu nutzen.

ADFV2 gibt die folgenden Metriken aus.

| **Metrik**           | **Metrikanzeigename**         | **Einheit** | **Aggregationstyp** | **Beschreibung**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRun | Metriken zu erfolgreichen Pipelineausführungen | Anzahl    | Gesamt                | Insgesamt erfolgreiche Pipelineausführungen in einem Zeitfenster von einer Minute |
| PipelineFailedRuns   | Metriken zu fehlerhaften Pipelineausführungen    | Anzahl    | Gesamt                | Insgesamt fehlerhafte Pipelineausführungen in einem Zeitfenster von einer Minute    |
| ActiviySucceededRuns | Metriken zu erfolgreichen Aktivitätsausführungen | Anzahl    | Gesamt                | Insgesamt erfolgreiche Aktivitätsausführungen in einem Zeitfenster von einer Minute  |
| ActivityFailedRuns   | Metriken zu fehlerhaften Aktivitätsausführungen    | Anzahl    | Gesamt                | Insgesamt fehlerhafte Aktivitätsausführungen in einem Zeitfenster von einer Minute     |
| TriggerSucceededRuns | Metriken zu erfolgreichen Triggerausführungen  | Anzahl    | Gesamt                | Insgesamt erfolgreiche Triggerausführungen in einem Zeitfenster von einer Minute   |
| TriggerFailedRuns    | Metriken zu fehlerhaften Triggerausführungen     | Anzahl    | Gesamt                | Insgesamt fehlerhafte Triggerausführungen in einem Zeitfenster von einer Minute      |

Befolgen Sie für den Zugriff auf die Metriken die Anweisungen in diesem Artikel: https://docs.microsoft.com/de-de/azure/monitoring-and-diagnostics/monitoring-overview-metrics 

## <a name="next-steps"></a>Nächste Schritte
Im Artikel [Programmgesteuertes Überwachen und Verwalten von Pipelines](monitor-programmatically.md) erfahren Sie mehr zum Überwachen und Verwalten von Pipelines. 