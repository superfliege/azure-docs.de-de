---
title: Erstellen von Metrikwarnungen für Protokolle in Azure Monitor
description: Tutorial zur Erstellung von Metrikwarnungen in Echtzeit für gängige Log Analytics-Daten.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: abfe7f0b39463a842b1eb167661af7da90bc9eca
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980847"
---
# <a name="create-metric-alerts-for-logs-in-azure-monitor"></a>Erstellen von Metrikwarnungen für Protokolle in Azure Monitor  

## <a name="overview"></a>Übersicht
Azure Monitor unterstützt den [Metrikwarnungstyp](monitoring-near-real-time-metric-alerts.md). Dies hat Vorteile gegenüber den [klassischen Warnungen](insights-alerts-portal.md). Metriken stehen für [eine umfangreiche Liste von Azure-Diensten](monitoring-supported-metrics.md) zur Verfügung. In diesem Artikel wird die Verwendung einer Teilmenge erläutert, d. h. für eine Ressource – `Microsoft.OperationalInsights/workspaces`. 

Sie können Metrikwarnungen für gängige Log Analytics-Protokolle verwenden, die als Metriken als Teil von Metriken aus Protokollen extrahiert wurden, einschließlich Ressourcen in Azure oder lokal. Die unterstützten Log Analytics-Lösungen sind nachfolgend aufgeführt:
- [Leistungsindikatoren](../log-analytics/log-analytics-data-sources-performance-counters.md) für Windows- und Linux-Computer
- [Heartbeat-Datensätze für Agent-Integritätsdiagnose](../operations-management-suite/oms-solution-agenthealth.md)
- Datensätze der [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md)
- [Ereignisdaten](../log-analytics/log-analytics-data-sources-windows-events.md)protokolle
 
Es gibt viele Vorteile für die Verwendung von **Metrikwarnungen für Protokolle** gegenüber abfragebasierten [Protokollwarnungen](alert-log.md) in Azure. Einige von ihnen sind unten aufgeführt:
- Metrikwarnungen bieten Überwachungsfunktionen in nahezu Echtzeit und Metrikwarnungen für Protokolle geben Daten aus der Protokollquelle an, um dasselbe sicherzustellen.
- Metrikwarnungen sind zustandsbehaftet. Es erfolgt nur eine Benachrichtigung, wenn ein Alarm ausgelöst wird und eine Benachrichtigung, wenn der Alarm aufgehoben wird. Dies steht im Gegensatz zu Protokollwarnungen, die zustandslos sind und in jedem Intervall ausgelöst werden, wenn die Bedingung der Warnung erfüllt ist.
- Metrikwarnungen für Protokolle bieten mehrere Dimensionen, die es ermöglichen, bestimmte Werte wie Computer, Betriebssystemtyp usw. einfacher zu filtern, ohne dass eine verfassende Abfrage bei der Analyse erforderlich ist.

> [!NOTE]
> Bestimmte Metriken und/oder Dimensionen werden nur angezeigt, wenn im gewählten Zeitraum dafür Daten vorhanden sind. Diese Metriken sind für Kunden mit Azure Log Analytics-Arbeitsbereichen verfügbar.

## <a name="metrics-and-dimensions-supported-for-logs"></a>Für Protokolle unterstützte Metriken und Dimensionen
 Metrikwarnungen unterstützen Warnungen für Metriken mit Dimensionen. Mithilfe von Dimensionen können Sie die Metrik nach der richtigen Ebene filtern. Die vollständige Liste der unterstützten Metriken für Protokolle aus [Log Analytics-Arbeitsbereichen](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces) ist aufgelistet (für unterstützte Lösungen).

> [!NOTE]
> Um unterstützte Metriken anzuzeigen, die über [Azure Monitor – Metriken](monitoring-metric-charts.md) aus dem Log Analytics-Arbeitsbereich extrahiert werden können, muss für diese Metrik eine Metrikwarnung für Protokolle erstellt werden. Die in der Metrikwarnung für Protokolle gewählten Dimensionen werden nur für die Untersuchung über „Azure Monitor – Metriken“ angezeigt.

# <a name="creating-metric-alert-for-log-analytics"></a>Erstellen von Metrikwarnungen für Log Analytics
Metrikdaten aus gängigen Protokollen werden vor der Verarbeitung in Log Analytics an „Azure Monitor – Metriken“ weitergeleitet. Auf diese Weise können Benutzer die Funktionen der Metrikplattform sowie die Metrikwarnung nutzen, einschließlich der Möglichkeit, Warnungen mit einer Häufigkeit von bis zu einer Minute zu erhalten. Nachfolgend sind die Möglichkeiten aufgeführt, eine Metrikwarnung für Protokolle zu erstellen.

## <a name="prerequisites-for-metric-alert-for-logs"></a>Voraussetzungen für Metrikwarnungen für Protokolle
Bevor die Metrik für Protokolle, die über Log Analytics-Daten erfasst wurden, funktioniert, muss Folgendes eingerichtet und verfügbar sein:
1. **Aktiver Log Analytics-Arbeitsbereich**: Es muss ein gültiger und aktiver Log Analytics-Arbeitsbereich vorhanden sein. Weitere Informationen finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../log-analytics/log-analytics-quick-create-workspace.md).
2. **Agent ist für Log Analytics-Arbeitsbereich konfiguriert**: Der Agent muss für Azure VMs (und/oder) lokale VMs konfiguriert werden, um Daten in den Log Analytics-Arbeitsbereich zu senden, der in einem früheren Schritt verwendet wurde. Weitere Informationen finden Sie unter [Log Analytics – Übersicht über Agents](../monitoring/monitoring-overview-azure-agents.md).
3. **Unterstützte Log Analytics-Lösungen sind installiert**: Die Log Analytics-Lösung sollte konfiguriert sein und Daten in den Log Analytics-Arbeitsbereich senden – unterstützte Lösungen sind [Leistungsindikatoren für Windows und Linux](../log-analytics/log-analytics-data-sources-performance-counters.md), [Heartbeat-Datensätze für Agent-Integritätsdiagnose](../operations-management-suite/oms-solution-agenthealth.md), [Update-Management und [Ereignisdaten](../log-analytics/log-analytics-data-sources-windows-events.md).
4. **Zum Senden von Protokollen konfigurierte Log Analytics-Lösungen**: Die Log Analytis-Lösung sollte die erforderlichen Protokolle/Daten entsprechend der für [Log Analytics-Arbeitsbereiche unterstützten Metriken](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces) aktiviert haben. Der Zähler *% Verfügbarer Arbeitsspeicher* muss z. B. zuerst in der Lösung [Leistungsindikatoren](../log-analytics/log-analytics-data-sources-performance-counters.md) konfiguriert sein.

## <a name="configuring-metric-alert-for-logs"></a>Konfigurieren der Metrikwarnung für Protokolle
 Metrikwarnungen können über das Azure-Portal, Resource Manager-Vorlagen, REST-API, PowerShell und Azure CLI erstellt und verwaltet werden. Da Metrikwarnungen für Protokolle eine Variante von Metrikwarnungen sind, können nach Erfüllung der Voraussetzungen Metrikwarnungen für Protokolle für den angegebenen Log Analytics-Arbeitsbereich erstellt werden. Alle Merkmale und Funktionalitäten von [Metrikwarnungen](monitoring-near-real-time-metric-alerts.md) gelten auch für Metrikwarnungen für Protokolle; einschließlich Nutzlastschema, anwendbaren Kontingentgrenzen und fakturiertem Preis.

Schritt-für-Schritt-Anleitungen und Beispiele finden Sie unter [Erstellen und Verwalten von Metrikwarnungen](https://aka.ms/createmetricalert). Befolgen Sie insbesondere für Metrikwarnungen für Protokolle die Anweisungen zur Verwaltung von Metrikwarnungen und stellen Sie Folgendes sicher:
- Ziel der Metrikwarnung ist ein gültiger *Log Analytics-Arbeitsbereich*.
- Das für die Metrikwarnung für den ausgewählten *Log Analytics-Arbeitsbereich* gewählte Signal besitzt den Typ **Metrik**.
- Filter für bestimmte Bedingungen oder Ressourcen unter Verwendung von Dimensionsfiltern; Metriken für Protokolle sind mehrdimensional.
- Bei der Konfiguration der *Signallogik* kann eine einzelne Warnung erstellt werden, um mehrere Werte der Dimension (z.B. Computer) zu erfassen.
- Wenn **nicht** das Azure-Portal zum Erstellen einer Metrikwarnung für den ausgewählten *Log Analytics-Arbeitsbereich* verwendet wird, dann muss der Benutzer zunächst manuell eine explizite Regel zum Konvertieren von Protokolldaten in eine Metrik mit [Azure Monitor – Geplante Abfrageregeln](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules
) erstellen.

> [!NOTE]
> Bei der Erstellung von Metrikwarnungen für den Log Analytics-Arbeitsbereich über das Azure-Portal – entsprechende Regel für die Konvertierung von Protokolldaten in Metrik über [Azure Monitor – Geplante Abfrageregeln](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules) wird automatisch im Hintergrund erstellt, *ohne dass ein Benutzereingriff oder eine Aktion erforderlich ist*. Für Metrikwarnungen zum Erstellen von Protokollen mit anderen Mitteln als dem Azure-Portal finden Sie weitere Informationen im Abschnitt [Ressourcenvorlage für Metrikwarnungen für Protokolle](#resource-template-for-metric-alerts-for-logs) über Beispielmittel zum Erstellen eines auf „ScheduledQueryRule“ basierenden Protokolls zur Metrikkonvertierungsregel vor der Erstellung von Metrikwarnungen, andernfalls gibt es keine Daten für den Metrikwarnung für erstellte Protokolle.

## <a name="resource-template-for-metric-alerts-for-logs"></a>Ressourcenvorlage für Metrikwarnungen für Protokolle
Wie bereits erwähnt, ist der Prozess zum Erstellen von Metrikwarnungen aus Protokollen zweigliedrig:
1. Erstellen einer Regel zum Extrahieren von Metriken aus unterstützten Protokollen mithilfe der scheduledQueryRule-API
2. Erstellen einer Metrikwarnung für die aus dem Protokoll (in Schritt 1) und dem Log Analytics-Arbeitsbereich als Zielressource extrahierte Metrik.

Um dasselbe zu erreichen, kann die untenstehende Azure Resource Manager-Beispielvorlage verwendet werden, wobei die Erstellung einer Metrikwarnung von der erfolgreichen Erstellung der Regel für die Extraktion von Metriken aus Protokollen über „scheduledQueryRule“ abhängt.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log convertion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterThan",
            "allowedValues": [
                "Equals",
                "NotEquals",
                "GreaterThan",
                "GreaterThanOrEqual",
                "LessThan",
                "LessThanOrEqual"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "threshold": {
            "type": "string",
            "defaultValue": "0",
            "metadata": {
                "description": "The threshold value at which the alert is activated."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resouceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resouceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.SingleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],   
                            "operator": "[parameters('operator')]",
                            "threshold" : "[parameters('threshold')]",
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"                
                    }
                ]
            }
        }
    ]
}

```
Angenommen, das obige JSON wird als „metricfromLogsAlert.json“ gespeichert, dann kann es mit einer JSON-Parameterdatei für die ressourcenvorlagenbasierte Erstellung gekoppelt werden. Eine JSON-Beispieldatei ist nachfolgend aufgeführt:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule" 
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterThan" 
        },
        "threshold":{
            "value": "1"
        },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }    
}
```
Angenommen, die obige Parameterdatei wird als „metricfromLogsAlert.parameters.json“ gespeichert; dann kann eine Metrikwarnung für Protokolle erstellt werden, die eine [Ressourcenvorlage für die Erstellung im Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md) verwendet. 

Alternativ kann auch der folgende Azure PowerShell-Befehl verwendet werden:
```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlert.json TemplateParameterFile metricfromLogsAlert.parameters.json
```

Oder verwenden Sie die Bereitstellung von Ressourcenvorlagen mithilfe der Azure CLI:
```CLI
az group deployment create --resource-group myRG --template-file metricfromLogsAlert.json --parameters @metricfromLogsAlert.parameters.json
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [Metrikwarnungen](https://aka.ms/createmetricalert).
* Erfahren Sie mehr über [Protokollwarnungen in Azure](monitor-alerts-unified-log.md).
* Erfahren Sie mehr über [Warnungen in Azure](monitoring-overview-alerts.md).
