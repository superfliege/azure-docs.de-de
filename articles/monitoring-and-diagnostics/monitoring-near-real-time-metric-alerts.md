---
title: Unterstützte Ressourcen für neuere Azure Monitor-Metrikwarnungen
description: Referenz zu Supportmetriken und -protokollen für neuere Azure-Metrikwarnungen nahezu in Echtzeit.
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: d5eaa4dafc9c155d3e6f85bc67c578c8a12da7cf
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264509"
---
# <a name="supported-metrics-and-creation-methods-for-new-metric-alerts"></a>Unterstützten Metriken und Erstellungsmethoden für neue Metrikwarnungen
Azure Monitor unterstützt jetzt einen [neuen Metrikwarnungstyp](monitoring-overview-unified-alerts.md), der erhebliche Vorteile gegenüber älteren [klassischen Metrikwarnungen](insights-alerts-portal.md) aufweist. Die älteren Warnungen unterstützen eine [umfangreiche Liste von Metriken](monitoring-supported-metrics.md). Die neueren Warnungen unterstützen eine (wachsende) Teilmenge dieser umfangreicheren Liste. In diesem Artikel wird diese Teilmenge aufgeführt. 

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, Befehlszeilenschnittstelle, REST-Unterstützung
Derzeit können Sie neuere Metrikwarnungen nur im Azure-Portal, in der [REST-API](https://docs.microsoft.com/en-us/rest/api/monitor/metricalerts/createorupdate) oder in [Resource Manager-Vorlagen](monitoring-create-metric-alerts-with-templates.md) erstellen. Die Konfiguration neuerer Warnungen über PowerShell und die Azure-Befehlszeilenschnittstelle (Azure CLI 2.0) wird in Kürze unterstützt.

## <a name="metrics-and-dimensions-supported"></a>Unterstützte Metriken und Dimensionen
Neuere Metrikwarnungen unterstützen Warnungen für Metriken mit Dimensionen. Mithilfe von Dimensionen können Sie die Metrik nach der richtigen Ebene filtern. Sie können alle unterstützten Metriken und die dazugehörigen Dimensionen unter [Azure Monitor-Metrik-Explorer](monitoring-metric-charts.md) erkunden und visualisieren.

Im Anschluss finden Sie die vollständige Liste der Azure Monitor-Metrikquellen, die von den neueren Warnungen unterstützt werden:

|Ressourcentyp  |Unterstützte Dimensionen  | Verfügbare Metriken|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Ja        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Ja   | [Automation-Konten](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/V| [Batch-Konten](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/V     |[Redis Cache](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    N/V     | [Virtual Machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/V      |[Skalierungsgruppen für virtuelle Computer](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Ja| [Containergruppen](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.DataFactory/datafactories| Ja| [Data Factorys V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Ja     |[Data Factorys V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/V      |[Datenbank für MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/V     | [Datenbank für PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Ja      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Nein  | [Tresore](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/V    |[Logik-Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/V     | [Anwendungsgateways](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | N/V| [DNS-Zonen](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (nur für Standard-SKUs)| Ja| [Lastenausgleichsmodule](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/V       |[Öffentliche IP-Adressen](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/V | [Kapazitäten](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   N/V      |[Suchdienste](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Ja       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Ja     | [Speicherkonten](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Ja    | [Blobdienste](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Dateidienste](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [Warteschlangendienste](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) und [Tabellendienste](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/V       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    N/V     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (Vorschau) | Ja|[Log Analytics-Arbeitsbereiche](#log-analytics-logs-as-metrics-for-alerting)|


## <a name="log-analytics-logs-as-metrics-for-alerting"></a>Log Analytics-Protokolle als Metriken für Warnungen 

Sie können neuere Metrikwarnungen auch für gängige Log Analytics-Protokolle verwenden, die als Metriken aus der Protokollvorschau extrahiert wurden.  
- [Leistungsindikatoren](../log-analytics/log-analytics-data-sources-performance-counters.md) für Windows- und Linux-Computer
- [Heartbeat-Datensätze für Agent-Integritätsdiagnose](../operations-management-suite/oms-solution-agenthealth.md)
- Datensätze der [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md)
 
> [!NOTE]
> Bestimmte Metriken und/oder Dimensionen werden nur angezeigt, wenn im gewählten Zeitraum dafür Daten vorhanden sind. Diese Metriken sind für Kunden, die sich für die Vorschau entschieden haben, mit Arbeitsbereichen in folgenden Regionen verfügbar: USA, Osten; USA, Westen-Mitte; Europa, Westen. Wenn Sie diese Vorschauversion verwenden möchten, melden Sie sich über die [Umfrage](https://aka.ms/MetricLogPreview) an.

Die folgende Liste enthält die unterstützten protokollbasierten Log Analytics-Metrikquellen:

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



## <a name="payload-schema"></a>Nutzlast und Schema

Bei Verwendung einer ordnungsgemäß konfigurierten [Aktionsgruppe](monitoring-action-groups.md) enthält der POST-Vorgang für alle neueren Metrikwarnungen die folgende JSON-Nutzlast und das folgende Schema:

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
