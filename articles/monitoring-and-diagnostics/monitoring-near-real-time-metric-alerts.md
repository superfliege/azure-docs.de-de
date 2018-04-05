---
title: Near Real-Time Metric Alerts in Azure Monitor | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Near Real-Time Metric Alerts (Metrikwarnungen nahezu in Echtzeit) verwenden, um Azure-Ressourcenmetriken mit einer Häufigkeit ab 1 Minute zu überwachen.
author: snehithm
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: snmuvva, vinagara
ms.custom: ''
ms.openlocfilehash: 15b9b0b69f3805b3e3af1d3973fd3a77bea62ab9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="use-the-newer-metric-alerts-for-azure-services-in-azure-portal"></a>Verwenden Sie die neueren Metrikwarnungen für Azure-Dienste im Azure-Portal.
Azure Monitor unterstützt jetzt einen neuen Typ von Metrikwarnungen, der als „Near Real-Time Metric Alerts“ bezeichnet wird. 

Near Real-Time Metric Alerts unterscheiden sich in verschiedener Hinsicht von [klassischen Metrikwarnungen](insights-alerts-portal.md):

- **Verbesserte Latenz**: Near Real-Time Metric Alerts können jede Minute erneut ausgeführt werden. Ältere Metrikwarnungen werden immer nur alle fünf Minuten ausgeführt.
- **Unterstützung für mehrdimensionale Metriken**: Es können für dimensionale Metriken Warnungen ausgegeben werden, sodass Sie ein interessantes Segment der Metrik überwachen können.
- **Mehr Kontrolle über Metrikbedingungen:** Sie können mit Near Real-Time Metric Alerts umfangreichere Warnungsregeln definieren. Die Warnungen unterstützen die Überwachung der maximalen, minimalen und durchschnittlichen Werte sowie der Gesamtwerte der Metriken.
- **Kombinierte Überwachung mehrerer Metriken:** Near Real-Time Metric Alerts können mehrere Metriken (zurzeit bis zu zwei Metriken) mit einer einzigen Regel überwachen. Eine Warnung wird ausgelöst, wenn beide Metriken ihre jeweiligen Schwellenwerte für den angegebenen Zeitraum überschreiten.
- **Modulares Benachrichtigungssystem:** Near Real-Time Metric Alerts verwenden [Aktionsgruppen](monitoring-action-groups.md). Sie können mit Aktionsgruppen modulare Aktionen erstellen. Sie können Aktionsgruppen für mehrere Warnungsregeln wiederverwenden.
- **Metriken aus Protokollen**: Aus gängigen Protokolldaten, die in [Log Analytics](../log-analytics/log-analytics-overview.md) gesammelt werden, können Metriken in Azure Monitor extrahiert werden, und es können nahezu in Echtzeit Warnungen dazu erstellt werden.


## <a name="metrics-and-dimensions-supported"></a>Unterstützte Metriken und Dimensionen
Near Real-Time Metric Alerts unterstützen Warnungen für Metriken mit Dimensionen. Mithilfe von Dimensionen können Sie die Metrik nach der richtigen Ebene filtern. Sie können alle unterstützten Metriken und die dazugehörigen Dimensionen unter [Azure Monitor-Metrik-Explorer](monitoring-metric-charts.md) erkunden und visualisieren.

Hier ist die vollständige Liste mit Azure Monitor-basierten Metrikquellen angegeben, die für Near Real-Time Metric Alerts unterstützt werden:

|Ressourcentyp  |Unterstützte Dimensionen  | Verfügbare Metriken|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Ja        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Ja   | [Automation-Konten](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/V| [Batch-Konten](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/V     |[Redis Cache](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    N/V     | [Virtual Machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/V      |[Skalierungsgruppen für virtuelle Computer](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.DataFactory/factories     |   Ja     |[Data Factorys V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/V      |[Datenbank für MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/V     | [Datenbank für PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Ja      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.Logic/workflows     |     N/V    |[Logik-Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/V     | [Anwendungsgateways](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/publicipaddresses     |  N/V       |[Öffentliche IP-Adressen](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Search/searchServices     |   N/V      |[Suchdienste](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Ja       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Ja     | [Speicherkonten](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Ja    | [Blobdienste](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Dateidienste](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [Warteschlangendienste](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) und [Tabellendienste](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/V       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    N/V     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (Vorschau) | Ja|[Log Analytics-Arbeitsbereiche](#support-for-oms-logs-as-metrics-for-alerting)|


## <a name="create-a-newer-metric-alert"></a>Erstellen einer neueren Metrikwarnung
Derzeit können Sie neuere Metrikwarnungen nur im Azure-Portal oder in der REST-API erstellen. Unterstützung für die Konfiguration von Near Real-Time Metric Alerts über PowerShell und die Azure-Befehlszeilenschnittstelle (Azure CLI) wird bald verfügbar sein.

Informationen zum Erstellen einer neueren Metrikwarnung im Azure-Portal finden Sie unter [Erstellen einer Warnungsregel im Azure-Portal](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-newer-metric-alerts"></a>Erstellen von neueren Metrikwarnungen
Nach der Erstellung von Near Real-Time Metric Alerts können Sie die Warnungen mithilfe der Schritte in [Verwalten von Warnungen im Azure-Portal](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal) verwalten.

## <a name="support-for-oms-logs-as-metrics-for-alerting"></a>Unterstützung für OMS-Protokolle als Metriken für Warnungen

Sie können auch Near Real-Time Metric Alerts für gängige OMS-Protokolle verwenden, die als Metriken aus der Protokollvorschau extrahiert wurden.  
- [Leistungsindikatoren](../log-analytics/log-analytics-data-sources-performance-counters.md) für Windows- und Linux-Computer
- [Heartbeat-Datensätze für Agent-Integritätsdiagnose](../operations-management-suite/oms-solution-agenthealth.md)
- Datensätze der [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md)

Hier ist die vollständige Liste mit OMS-Protokoll-Metrikquellen angegeben, die für Near Real-Time Metric Alerts unterstützt werden:

Metrikname/-details  |Unterstützte Dimensionen  | Typ des Protokolls  |
|---------|---------|---------|
|Average_Avg. Datenträger s/gelesen     |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
| Average_Avg. Datenträger s/geschrieben     |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
| Average_Current Disk Queue Length   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
| Average_Disk Reads/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
| Average_Disk Transfers/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
|   Average_% Free Space    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
| Average_Available MBytes     |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
| Average_% Committed Bytes In Use    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
| Average_Bytes Received/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
|  Average_Bytes Sent/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
|  Average_Bytes Total/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
|  Average_% Processor Time    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
|   Average_Processor Queue Length    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
|   Average_% Free Inodes   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Free Space   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Used Inodes  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Used Space   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Disk Read Bytes/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Disk Reads/sec |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Disk Transfers/sec |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Disk Write Bytes/sec   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Disk Writes/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Free Megabytes |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Logical Disk Bytes/sec |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Available Memory |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Available Swap Space |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Used Memory  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Used Swap Space  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Available MBytes Memory    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Available MBytes Swap  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Page Reads/sec |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Page Writes/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Pages/sec  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Used MBytes Swap Space |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Used Memory MBytes |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Total Bytes Transmitted    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Total Bytes Received   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Total Bytes    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Total Packets Transmitted  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Total Packets Received |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Total Rx Errors    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Total Tx Errors    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Total Collisions   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Avg. Datenträger s/gelesen |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Avg. Datenträger s/übertragen |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Avg. Datenträger s/geschrieben    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Physical Disk Bytes/sec    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Pct Privileged Time    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Pct User Time  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Used Memory kBytes |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Virtual Shared Memory  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% DPC Time |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Idle Time    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Interrupt Time   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% IO Wait Time |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Nice Time    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Privileged Time  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% Processor Time   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_% User Time    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Free Physical Memory   |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Free Space in Paging Files |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Free Virtual Memory    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Processes  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Size Stored In Paging Files    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Uptime |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Users  |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Heartbeat  |     Ja – Computer, OSType, Version & SourceComputerId    |   Heartbeat-Datensätze |
|    Aktualisieren |     Ja – Computer, Product, Classification, UpdateState, Optional & Approved    |   Updateverwaltung |

> [!NOTE]
> Bestimmte Metriken und/oder Dimensionen werden nur angezeigt, wenn im gewählten Zeitraum dafür Daten vorhanden sind. Diese Metriken sind für Kunden, die sich für die Vorschau entschieden haben, mit Arbeitsbereichen in folgenden Regionen verfügbar: USA, Osten; USA, Westen-Mitte; Europa, Westen. Wenn Sie diese Vorschauversion verwenden möchten, melden Sie sich über die [Umfrage](https://aka.ms/MetricLogPreview) an.


## <a name="payload-schema"></a>Nutzlast und Schema

Der POST-Vorgang enthält die folgende JSON-Nutzlast bzw. folgendes JSON-Schema für alle Near Real-Time Metric Alerts, wenn eine richtig konfigurierte [Aktionsgruppe](monitoring-action-groups.md) verwendet wird:

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [neue Oberfläche „Warnungen“](monitoring-overview-unified-alerts.md).
* Erfahren Sie mehr über [Protokollwarnungen in Azure](monitor-alerts-unified-log.md).
* Erfahren Sie mehr über [Warnungen in Azure](monitoring-overview-alerts.md).
