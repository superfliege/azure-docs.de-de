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
ms.openlocfilehash: 88995b1f3350fe485e28efccc93779ae0a42eb97
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Near Real-Time Metric Alerts (Vorschau)
Azure Monitor unterstützt jetzt einen neuen Typ von Metrikwarnungen, der als „Near Real-Time Metric Alerts (Vorschau)“ bezeichnet wird. Dieses Feature ist zurzeit als öffentliche Vorschauversion verfügbar.

Near Real-Time Metric Alerts unterscheiden sich in verschiedener Hinsicht von regulären Metrikwarnungen:

- **Verbesserte Latenz:** Near Real-Time Metric Alerts können Änderungen an Metrikwerten mit einer Häufigkeit ab einer Minute überwachen.
- **Mehr Kontrolle über Metrikbedingungen:** Sie können mit Near Real-Time Metric Alerts umfangreichere Warnungsregeln definieren. Die Warnungen unterstützen die Überwachung der maximalen, minimalen und durchschnittlichen Werte sowie der Gesamtwerte der Metriken.
- **Metriken aus Protokollen**: Aus gängigen Protokolldaten, die in [Log Analytics](../log-analytics/log-analytics-overview.md) gesammelt werden, können Metriken in Azure Monitor extrahiert werden, und es können nahezu in Echtzeit Warnungen dazu erstellt werden.
- **Kombinierte Überwachung mehrerer Metriken:** Near Real-Time Metric Alerts können mehrere Metriken (zurzeit bis zu zwei Metriken) mit einer einzigen Regel überwachen. Eine Warnung wird ausgelöst, wenn beide Metriken ihre jeweiligen Schwellenwerte für den angegebenen Zeitraum überschreiten.
- **Modulares Benachrichtigungssystem:** Near Real-Time Metric Alerts verwenden [Aktionsgruppen](monitoring-action-groups.md). Sie können mit Aktionsgruppen modulare Aktionen erstellen. Sie können Aktionsgruppen für mehrere Warnungsregeln wiederverwenden.

> [!NOTE]
> Real-Time Metric Alerts ist zurzeit als öffentliche Vorschauversion verfügbar. Metriken aus Protokollfeatures befinden sich in der *eingeschränkten* öffentlichen Vorschauphase. Die Funktionen und die Benutzeroberfläche können jederzeit geändert werden.
>

## <a name="metrics-and-dimensions-supported"></a>Unterstützte Metriken und Dimensionen
Near Real-Time Metric Alerts unterstützen Warnungen für Metriken mit Dimensionen. Mithilfe von Dimensionen können Sie die Metrik nach der richtigen Ebene filtern. Sie können alle unterstützten Metriken und die dazugehörigen Dimensionen unter [Azure Monitor-Metrik-Explorer](monitoring-metric-charts.md) erkunden und visualisieren.

Hier ist die vollständige Liste mit Azure Monitor-basierten Metrikquellen angegeben, die für Near Real-Time Metric Alerts unterstützt werden:

|Metrikname/-details  |Unterstützte Dimensionen  |
|---------|---------|
|Microsoft.ApiManagement/service     | Ja        |
|Microsoft.Automation/automationAccounts     |     N/V    |
|Microsoft.Automation/automationAccounts     |   N/V      |
|Microsoft.Cache/Redis     |    N/V     |
|Microsoft.Compute/virtualMachines     |    N/V     |
|Microsoft.Compute/virtualMachineScaleSets     |   N/V      |
|Microsoft.DataFactory/factories     |   N/V      |
|Microsoft.DBforMySQL/servers     |   N/V      |
|Microsoft.DBforPostgreSQL/servers     |    N/V     |
|Microsoft.EventHub/namespaces     |   N/V      |
|Microsoft.Logic/workflows     |     N/V    |
|Microsoft.Network/applicationGateways     |    N/V     |
|Microsoft.Network/publicipaddresses     |  N/V       |
|Microsoft.Search/searchServices     |   N/V      |
|Microsoft.ServiceBus/namespaces     |  N/V       |
|Microsoft.Storage/storageAccounts     |    Ja     |
|Microsoft.Storage/storageAccounts/services     |     Ja    |
|Microsoft.StreamAnalytics/streamingjobs     |  N/V       |
|Microsoft.CognitiveServices/accounts     |    N/V     |


Für Metriken aus Protokollen werden derzeit die folgenden gängigen OMS-Protokolle unterstützt:
- [Leistungsindikatoren](../log-analytics/log-analytics-data-sources-performance-counters.md) für Windows- und Linux-Computer
- Heartbeat-Datensätze für Computer
- Datensätze der [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md)

Hier ist die vollständige Liste mit OMS-Protokoll-Metrikquellen angegeben, die für Near Real-Time Metric Alerts unterstützt werden:

Metrikname/-details  |Unterstützte Dimensionen  | Typ des Protokolls  |
|---------|---------|---------|
|Average_Avg. Disk sec/Read     |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
| Average_Avg. Disk sec/Write     |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Windows-Leistungsindikator      |
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
|    Average_Avg. Disk sec/Read |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Avg. Disk sec/Transfer |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
|    Average_Avg. Disk sec/Write    |     Ja – Computer, ObjectName, InstanceName, CounterPath & SourceSystem    |   Linux-Leistungsindikator      |
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
|    Update |     Ja – Computer, Product, Classification, UpdateState, Optional & Approved    |   Updateverwaltung |

> [!NOTE]
> Bestimmte Metriken oder Dimensionen werden nur angezeigt, wenn im gewählten Zeitraum dafür Daten vorhanden sind.

## <a name="create-a-near-real-time-metric-alert"></a>Erstellen von Near Real-Time Metric Alerts
Derzeit können Sie Near Real-Time Metric Alerts nur im Azure-Portal erstellen. Unterstützung für die Konfiguration von Near Real-Time Metric Alerts über PowerShell, die Azure-Befehlszeilenschnittstelle (CLI) und die REST-API von Azure Monitor wird bald verfügbar sein.

Die Oberfläche zum Erstellen von Near Real-Time Metric Alerts wurde auf die neue Seite **Warnungen (Vorschau)** verschoben. Obwohl auf der aktuellen Seite „Warnungen“ **Add Near Real-Time Metric alert** (Near Real-Time Metric Alert hinzufügen) angezeigt wird, werden Sie zur Seite **Warnungen (Vorschau)** umgeleitet.

Informationen zum Erstellen von Near Real-Time Metric Alerts finden Sie unter [Erstellen einer Warnungsregel im Azure-Portal](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Verwalten von Near Real-Time Metric Alerts
Nach der Erstellung von Near Real-Time Metric Alerts können Sie die Warnungen mithilfe der Schritte in [Verwalten von Warnungen im Azure-Portal](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal) verwalten.

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

* Erfahren Sie mehr über die [neue Oberfläche „Warnungen (Vorschau)“](monitoring-overview-unified-alerts.md).
* Erfahren Sie mehr über [Protokollwarnungen in Azure-Warnungen (Vorschau)](monitor-alerts-unified-log.md).
* Erfahren Sie mehr über [Warnungen in Azure](monitoring-overview-alerts.md).
