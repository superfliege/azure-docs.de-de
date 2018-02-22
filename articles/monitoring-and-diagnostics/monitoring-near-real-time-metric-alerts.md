---
title: Near Real-Time Metric Alerts in Azure Monitor | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Near Real-Time Metric Alerts (Metrikwarnungen nahezu in Echtzeit) verwenden, um Azure-Ressourcenmetriken mit einer Häufigkeit ab 1 Minute zu überwachen."
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: 6370f4596e6b20962c6de0dbcbd5f86c3b7777cc
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Near Real-Time Metric Alerts (Vorschau)
Azure Monitor unterstützt jetzt einen neuen Typ von Metrikwarnungen, der als „Near Real-Time Metric Alerts (Vorschau)“ bezeichnet wird. Dieses Feature ist zurzeit als öffentliche Vorschauversion verfügbar.

Near Real-Time Metric Alerts unterscheiden sich in verschiedener Hinsicht von regulären Metrikwarnungen:

- **Verbesserte Latenz:** Near Real-Time Metric Alerts können Änderungen an Metrikwerten mit einer Häufigkeit ab einer Minute überwachen.
- **Mehr Kontrolle über Metrikbedingungen:** Sie können mit Near Real-Time Metric Alerts umfangreichere Warnungsregeln definieren. Die Warnungen unterstützen die Überwachung der maximalen, minimalen und durchschnittlichen Werte sowie der Gesamtwerte der Metriken.
- **Kombinierte Überwachung mehrerer Metriken:** Near Real-Time Metric Alerts können mehrere Metriken (zurzeit bis zu zwei Metriken) mit einer einzigen Regel überwachen. Eine Warnung wird ausgelöst, wenn beide Metriken ihre jeweiligen Schwellenwerte für den angegebenen Zeitraum überschreiten.
- **Modulares Benachrichtigungssystem:** Near Real-Time Metric Alerts verwenden [Aktionsgruppen](monitoring-action-groups.md). Sie können mit Aktionsgruppen modulare Aktionen erstellen. Sie können Aktionsgruppen für mehrere Warnungsregeln wiederverwenden.

> [!NOTE]
> Das Feature Real-Time Metric Alerts ist zurzeit als öffentliche Vorschauversion verfügbar. Die Funktionen und die Benutzeroberfläche können jederzeit geändert werden.
>

## <a name="resources-you-can-use-with-near-real-time-metric-alerts"></a>Ressourcen, die Sie mit Near Real-Time Metric Alerts verwenden können
Im Folgenden finden Sie eine vollständige Liste der Ressourcentypen, die von Real-Time Metric Alerts unterstützt werden:

* Microsoft.ApiManagement/service
* Microsoft.Automation/automationAccounts
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Storage/storageAccounts
* Microsoft.Storage/storageAccounts/services
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.CognitiveServices/accounts

## <a name="near-real-time-metric-alerts-for-metrics-that-use-dimensions"></a>Near Real-Time Metric Alerts für Metriken mit Dimensionen
Near Real-Time Metric Alerts unterstützen Warnungen für Metriken mit Dimensionen. Mithilfe von Dimensionen können Sie die Metrik nach der richtigen Ebene filtern. Near Real-Time Metric Alerts für Metriken mit Dimensionen werden für folgende Ressourcentypen unterstützt:

* Microsoft.ApiManagement/service
* Microsoft.Storage/storageAccounts (nur für Speicherkonten in Regionen der USA unterstützt)
* Microsoft.Storage/storageAccounts/services (nur für Speicherkonten in Regionen der USA unterstützt)

## <a name="create-a-near-real-time-metric-alert"></a>Erstellen von Near Real-Time Metric Alerts
Derzeit können Sie Near Real-Time Metric Alerts nur im Azure-Portal erstellen. Unterstützung für die Konfiguration von Near Real-Time Metric Alerts über PowerShell, die Azure-Befehlszeilenschnittstelle (CLI) und die REST-API von Azure Monitor wird bald verfügbar sein.

Die Oberfläche zum Erstellen von Near Real-Time Metric Alerts wurde auf die neue Seite **Warnungen (Vorschau)** verschoben. Obwohl auf der aktuellen Seite „Warnungen“ **Add Near Real-Time Metric alert** (Near Real-Time Metric Alert hinzufügen) angezeigt wird, werden Sie zur Seite **Warnungen (Vorschau)** umgeleitet.

Informationen zum Erstellen von Near Real-Time Metric Alerts finden Sie unter [Erstellen einer Warnungsregel im Azure-Portal](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Verwalten von Near Real-Time Metric Alerts
Nach der Erstellung von Near Real-Time Metric Alerts können Sie die Warnungen mithilfe der Schritte in [Verwalten von Warnungen im Azure-Portal](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal) verwalten.

## <a name="payload-schema"></a>Nutzlast und Schema

Der POST-Vorgang enthält für alle Near Real-Time Metric Alerts die folgende JSON-Nutzlast und das folgende Schema:

```json
{
    "WebhookName": "Alert1510875839452",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "condition": {
                "metricName": "Percentage CPU",
                "metricUnit": "Percent",
                "metricValue": "17.7654545454545",
                "threshold": "1",
                "windowSize": "10",
                "timeAggregation": "Average",
                "operator": "GreaterThan"
            },
            "resourceName": "ContosoVM1",
            "resourceType": "microsoft.compute/virtualmachines",
            "resourceRegion": "westus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/automationtest/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "timestamp": "2017-11-16T23:54:03.9517451Z",
            "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/microsoft.insights/alertrules/VMMetricAlert1",
            "name": "VMMetricAlert1",
            "description": "A metric alert for the VM Win2012R2",
            "conditionType": "Metric",
            "subscriptionId": "00000000-0000-0000-0000-000000000000",
            "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "resourceGroupName": "ContosoVM"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    },
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "azure-insights/0.9",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [neue Oberfläche „Warnungen (Vorschau)“](monitoring-overview-unified-alerts.md).
* Erfahren Sie mehr über [Protokollwarnungen in Azure-Warnungen (Vorschau)](monitor-alerts-unified-log.md).
* Erfahren Sie mehr über [Warnungen in Azure](monitoring-overview-alerts.md).