---
title: Near Real-Time Metric Alerts in Azure Monitor | Microsoft Docs
description: "Near Real-Time Metric Alerts ermöglichen Ihnen die minütliche Überwachung von Azure-Ressourcemetriken."
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
ms.openlocfilehash: d3e88a98e0ba93a630d131c25ca4dd5cb16f1b1a
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Near Real-Time Metric Alerts (Preview)
Azure Monitor unterstützt jetzt einen neuen Typ von Metrikwarnungen, die als „Near Real-Time Metric Alerts (Preview)“ bezeichnet werden. Dieses Feature ist zurzeit als öffentliche Preview verfügbar.
Diese Warnungen unterscheiden sich in verschiedener Hinsicht von regulären Metrikwarnungen.

- **Verbesserte Latenz**: Near Real-Time Metric Alerts können Änderungen an Metrikwerten innerhalb einer Minute überwachen.
- **Mehr Kontrolle über Metrikbedingungen**: Near Real-Time Metric Alerts ermöglichen Benutzern, umfangreichere Warnungsregeln zu definieren. Die Warnungen unterstützen nun die Überwachung der maximalen, minimalen und durchschnittlichen Werte sowie der Gesamtwerte der Metriken.
- **Kombinierte Überwachung mehrerer Metriken**: Near Real-Time Metric Alerts können mehrere Metriken (zurzeit zwei Metriken) mit einer einzigen Regel überwachen. Die Warnung wird ausgelöst, wenn beide Metriken ihre jeweiligen Schwellenwerte für den angegebenen Zeitraum verletzen.
- **Modulares Benachrichtigungssystem**: Near Real-Time Metric Alerts verwenden [Aktionsgruppen](monitoring-action-groups.md). Diese Funktion ermöglicht Benutzern das Erstellen von Aktionen auf modulare Weise sowie die Wiederverwendung für viele Warnungsregeln.

> [!NOTE]
> Das Feature Real-Time Metric Alerts ist zurzeit als öffentliche Preview verfügbar. Die Funktionen und die Benutzeroberfläche können jederzeit geändert werden.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Für welche Ressourcen kann ich Real-Time Metric Alerts erstellen?
Vollständige Liste der Ressourcentypen, die von Real-Time Metric Alerts unterstützt werden:

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

## <a name="near-real-time-metric-alerts-on-metrics-with-dimensions"></a>Metrikwarnungen nahezu in Echtzeit für Metriken mit Dimensionen
Metrikwarnungen nahezu in Echtzeit unterstützen Warnungen für Metriken mit Dimensionen. Dimensionen stellen eine Möglichkeit dar, um die Metrik nach der richtigen Ebene zu filtern. Metrikwarnungen nahezu in Echtzeit für Metriken mit Dimensionen werden für folgende Ressourcentypen unterstützt.

* Microsoft.ApiManagement/service
* Microsoft.Storage/storageAccounts (nur für Speicherkonten in Regionen der USA unterstützt)
* Microsoft.Storage/storageAccounts/services (nur für Speicherkonten in Regionen der USA unterstützt)


## <a name="create-a-near-real-time-metric-alert"></a>Erstellen eines Real-Time Metric Alerts
Zurzeit können Real-Time Metric Alerts nur über das Azure Portal erstellt werden. Unterstützung für die Konfiguration von Near Real-Time Metric Alerts über PowerShell, die Befehlszeilenschnittstelle (CLI) und die REST-API von Azure Monitor wird bald verfügbar sein.

Die Oberfläche zum Erstellen von Warnungen für Metrikwarnungen nahezu in Echtzeit wurde zur Oberfläche **Warnungen (Vorschauversion)** migriert. Obwohl auf der aktuellen Seite „Warnungen“ **Metrikwarnung nahezu in Echtzeit hinzufügen** angezeigt wird, werden Sie zur neuen Oberfläche umgeleitet.

Sie können anhand der [hier](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal) beschriebenen Schritte eine Metrikwarnung in Echtzeit erstellen.

## <a name="managing-near-real-time-metric-alerts"></a>Verwalten von Near Real-Time Metric Alerts
Sobald Sie eine **Metrikwarnung nahezu in Echtzeit** erstellt haben, kann diese anhand der [hier](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal) beschriebenen Schritte verwaltet werden.

## <a name="payload-schema"></a>Nutzlast und Schema

Der POST-Vorgang enthält für alle Metrikwarnungen nahezu in Echtzeit die folgende JSON-Nutzlast und das folgende Schema:

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

* [Erfahren Sie mehr über die neue Oberfläche „Warnungen (Vorschauversion)“.](monitoring-overview-unified-alerts.md)
* [Erfahren Sie mehr über Protokollwarnungen in Azure-Warnungen (Vorschauversion).](monitor-alerts-unified-log.md)
* [Erfahren Sie mehr über Warnungen in Azure.](monitoring-overview-alerts.md)