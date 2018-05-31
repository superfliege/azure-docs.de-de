---
title: Erstellen einer Protokollwarnung anhand einer Resource Manager-Vorlage | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Protokollwarnung mit einer Azure Resource Manager-Vorlage und -API erstellen.
author: msvijayn
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
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 14a2560d91fd0f8dcc729b32c7155c4b74aa8aa1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204157"
---
# <a name="create-a-log-alert-with-a-resource-manager-template"></a>Erstellen einer Protokollwarnung anhand einer Resource Manager-Vorlage
In diesem Artikel wird beschrieben, wie Sie [Protokollwarnungen](monitor-alerts-unified-log.md) programmgesteuert und skaliert in Azure verwalten können, indem Sie eine [Azure Resource Manager-Vorlage](..//azure-resource-manager/resource-group-authoring-templates.md) über [Azure Powershell](../azure-resource-manager/resource-group-template-deploy.md) und [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) verwenden. Derzeit unterstützen Azure-Warnungen Protokollwarnungen zu Abfragen von [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) und [Azure Application Insights](../application-insights/app-insights-analytics-tour.md).

## <a name="managing-log-alert-on-log-analytics"></a>Verwalten von Protokollwarnungen in Log Analytics
Protokollwarnungen für [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) sind in die [neue Azure-Umgebung für Warnungen](monitoring-overview-unified-alerts.md) integriert, während sie weiterhin in den Log Analytics-APIs ausgeführt werden und mit dem früher zum Verwalten von [Warnungen im OMS-Portal](..//log-analytics/log-analytics-alerts-creating.md) verwendeten Schema kompatibel sind.

> [!NOTE]
> Ab 14. Mai 2018 werden alle Warnungen in einem Arbeitsbereich automatisch auf Azure ausgedehnt. Ein Benutzer kann die Ausdehnung von Warnungen auf Azure bereits vor dem 14. Mai 2018 freiwillig initiieren. Weitere Informationen finden Sie unter [Erweitern von Warnungen in Azure aus OMS](monitoring-alerts-extend.md). 

### <a name="using-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen
Protokollwarnungen für Log Analytics werden anhand von Warnungsregeln erstellt, die in regelmäßigen Abständen eine gespeicherte Suche ausführen. Wenn die Ergebnisse der Abfrage mit den angegebenen Kriterien übereinstimmen, wird eine Warnung erstellt und mindestens eine Aktion ausgeführt. 

Die Ressourcenvorlagen für die [gespeicherte Suche von Log Analytics](../log-analytics/log-analytics-log-searches.md) und [Log Analytics-Warnungen](../log-analytics/log-analytics-alerts.md) stehen im Log Analytics-Abschnitt der Dokumentation zur Verfügung. Erfahren Sie mehr über, [Hinzufügen gespeicherter Suchen und Warnungen von Log Analytics](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md). Hier finden Sie anschauliche Beispiele sowie die Schemadetails.

### <a name="using-resource-template-via-apipowershell"></a>Verwenden der Ressourcenvorlage über API/Powershell
Die REST-API für Log Analytics-Warnungen ist RESTful. Der Zugriff darauf erfolgt über die Azure Resource Manager-REST-API. Auf die API kann daher über die PowerShell-Befehlszeile zugegriffen werden. Die Suchergebnisse werden Ihnen im JSON-Format ausgegeben, und Sie können die Ergebnisse programmgesteuert auf viele verschiedene Arten verwenden.

Erfahren Sie mehr über das [Erstellen und Verwalten von Warnungsregeln in Log Analytics mit REST-API](../log-analytics/log-analytics-api-alerts.md). Hier finden Sie auch Beispiele für den Zugriff auf die API aus Powershell.

## <a name="managing-log-alert-on-application-insights"></a>Verwalten von Protokollwarnungen in Application Insights
Protokollwarnungen für Azure Application Insights wurden als Teil der neuen Azure-Warnungen unter Azure Monitor eingeführt. Daher werden sie unter der Azure Monitor-API als die REST-Vorgangsgruppe [Scheduled Query Rules](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) (geplante Abfrageregeln) ausgeführt.

### <a name="using-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen
Protokollwarnungen für Application Insights-Ressourcen sind vom Typ `Microsoft.Insights/scheduledQueryRules/`. Weitere Informationen zu diesem Ressourcentyp finden Sie unter [Azure Monitor: Referenz für die Scheduled Query Rules-API](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/).

Im folgenden finden Sie die Struktur für die auf der [Scheduled Query Rules-Erstellung](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/createorupdate) basierende Ressourcenvorlage mit einem Stichprobendataset als Variablen.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
    "variables": {
    "alertLocation": "southcentralus",
    "alertName": "samplelogalert",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
    "alertDesription": "Sample log search alert",
    "alertStatus": "true",
    "alertSource":{
        "Query":"requests",
        "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
        "Type":"ResultCount"
         },
     "alertSchedule":{
         "Frequency": 15,
         "Time": 60
         },
     "alertActions":{
         "SeverityLevel": "4"
         },
      "alertTrigger":{
        "Operator":"GreaterThan",
        "Threshold":"1"
         },
       "actionGrp":{
        "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
        "Subject": "Customized Email Header",
        "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"           
         }
  },
  "resources":[ {
    "name":"[variables('alertName')]",
    "type":"Microsoft.Insights/scheduledQueryRules",
    "apiVersion": "2018-04-16",
    "location": "[variables('alertLocation')]",
    "tags":{"[variables('alertTag')]": "Resource"},
    "properties":{
       "description": "[variables('alertDesription')]",
       "enabled": "[variables('alertStatus')]",
       "source": {
           "query": "[variables('alertSource').Query]",
           "dataSourceId": "[variables('alertSource').SourceId]",
           "queryType":"[variables('alertSource').Type]"
       },
      "schedule":{
           "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
           "timeWindowInMinutes": "[variables('alertSchedule').Time]"    
       },
      "action":{
           "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
           "severity":"[variables('alertActions').SeverityLevel]",
           "aznsAction":{
               "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
               "emailSubject":"[variables('actionGrp').Subject]",
               "customWebhookPayload":"[variables('actionGrp').Webhook]"
           },
       "trigger":{
               "thresholdOperator":"[variables('alertTrigger').Operator]",
               "threshold":"[variables('alertTrigger').Threshold]"
           }
       }
     }
   }
 ]
}
```
> [!IMPORTANT]
> Das Tag-Feld mit einem ausgeblendetem Link zur Zielressource ist bei Verwendung des API-Aufrufs von [Scheduled Query Rules](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) oder der Ressourcenvorlage zwingend erforderlich. 

Die Json-Beispiel oben kann im Rahmen dieser exemplarischen Vorgehensweise z. B. als „sampleScheduledQueryRule.json“ gespeichert und mithilfe von [Azure Resource Manager im Azure-Portal](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) bereitgestellt werden.

### <a name="using-resource-template-via-clipowershell"></a>Verwenden der Ressourcenvorlage über CLI/Powershell
Azure Monitor – Scheduled Query Rule-API ist eine REST-API und vollständig kompatibel mit der Azure Resource Manager-REST-API. Daher kann es über Powershell mit dem Resource Manager-Cmdlet und der Azure CLI verwendet werden.

Nachfolgend wird die Verwendung über das PowerShell-Cmdlet von Azure Resource Manager für die weiter oben dargestellte Ressourcenvorlage (sampleScheduledQueryRule.json) veranschaulicht:
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Nachfolgend wird die Verwendung über den Azure Resource Manager-Befehl in der Azure CLI für die weiter oben dargestellte Ressourcenvorlage (sampleScheduledQueryRule.json) veranschaulicht:

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Bei erfolgreicher Ausführung wird 201 zurückgegeben, wenn eine neue Warnungsregel erstellt wurde, bzw. 200, wenn eine vorhandene Warnungsregel geändert wurde.


## <a name="next-steps"></a>Nächste Schritte
* Machen Sie sich mit [Webhookaktionen für Protokollwarnungen](monitor-alerts-unified-log-webhook.md) vertraut.
* Informieren Sie sich über die neuen [Azure-Warnungen](monitoring-overview-unified-alerts.md).
* Weitere Informationen zu [Application Insights](../application-insights/app-insights-analytics.md)
* Erfahren Sie mehr über [Log Analytics](../log-analytics/log-analytics-overview.md).   
