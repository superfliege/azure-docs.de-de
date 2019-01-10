---
title: Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor
description: Verwenden Sie Azure Monitor, um Protokollwarnungsregeln in Azure zu erstellen, anzuzeigen und zu verwalten.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 1516410952dfdfef0a0ef12c450040d1e3fb8a27
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54103762"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor  

## <a name="overview"></a>Übersicht
In diesem Artikel wird erläutert, wie Sie im Azure-Portal mithilfe der Oberfläche „Warnungen“ Protokollwarnungen einrichten. Die Definition einer Warnungsregel setzt sich aus drei Teilen zusammen:
- Ziel: Eine bestimmte zu überwachende Azure-Ressource
- Kriterien: Eine bestimmte Bedingung oder Logik, die beim Vorkommen in einem Signal eine Aktion auslösen soll
- Aktion: Eine bestimmte Form der Kontaktaufnahme, die an einen Empfänger einer Benachrichtigung (z. B. E-Mail, SMS oder Webhook usw.) gesendet wird.

Der Begriff **Protokollwarnungen** beschreibt Warnungen basierend auf [Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) oder [Application Insights](../../azure-monitor/app/analytics.md), bei denen das Signal eine benutzerdefinierte Abfrage darstellt. Erfahren Sie mehr über Funktionen, Terminologie und Typen von [Protokollwarnungen – Übersicht](../../azure-monitor/platform/alerts-unified-log.md).

> [!NOTE]
> Gängige Protokolldaten aus [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) sind nun auch auf der Metrikplattform in Azure Monitor verfügbar. Eine Detailansicht finden Sie unter [Metrikwarnung für Protokolle](../../azure-monitor/platform/alerts-metric-logs.md).

## <a name="managing-log-alerts-from-the-azure-portal"></a>Verwalten von Protokollwarnungen über das Azure-Portal

Als nächstes wird die schrittweise Anleitung zur Verwendung von Protokollwarnungen über die Benutzeroberfläche des Azure-Portals eingehend erläutert.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Erstellen einer Protokollwarnungsregel mit dem Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com/) die Option **Monitor** und im Abschnitt „MONITOR“ dann **Warnungen** aus.  
    ![Überwachung](media/alerts-log/AlertsPreviewMenu.png)

1. Wählen Sie die Schaltfläche **Neue Warnungsregel** aus, um eine neue Warnung in Azure zu erstellen.
    ![Warnung hinzufügen](media/alerts-log/AlertsPreviewOption.png)

1. Der Abschnitt „Warnung erstellen“ wird mit den drei Teilen angezeigt, die aus Folgendem bestehen: *Warnungsbedingung definieren*, *Warnungsdetails definieren* und *Aktionsgruppe definieren*.

    ![Erstellen einer Regel](media/alerts-log/AlertsPreviewAdd.png)

1.  Definieren Sie die Warnungsbedingung, indem Sie auf den Link **Ressource auswählen** klicken und dann durch Auswahl einer Ressource das Ziel angeben. Filtern Sie, indem Sie die Optionen _Abonnement_, _Ressourcentyp_ und schließlich die erforderliche Option _Ressource_ auswählen. 

    >[!NOTE]

    > Überprüfen Sie beim Erstellen einer Protokollwarnung das für die ausgewählte Ressource verfügbare **Protokoll**signal, bevor Sie den Vorgang fortsetzen.
    ![Auswählen einer Ressource](media/alerts-log/Alert-SelectResourceLog.png)

 
1. *Protokollwarnungen*: Stellen Sie sicher, dass als **Ressourcentyp** eine Analysequelle wie *Log Analytics* oder *Application Insights* und der Signaltyp **Protokoll** ausgewählt sind, und klicken Sie nach Auswahl der entsprechenden **Ressource** auf *Fertig*. Klicken Sie als Nächstes auf die Schaltfläche **Kriterien hinzufügen**, um die Liste mit den für die Ressource verfügbaren Signaloptionen und die Option **Benutzerdefinierte Protokollsuche** für den ausgewählten Protokollüberwachungsdienst (z.B. *Log Analytics* oder *Application Insights*) aus der Signalliste anzuzeigen.

   ![Auswählen einer Ressource – benutzerdefinierte Protokollsuche](media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Über die Listen von „Warnungen“ kann eine Analyseabfrage als Signaltyp **Protokoll (gespeicherte Abfrage)** importiert werden. Dies ist in der obigen Abbildung dargestellt. Benutzer können Ihre Abfrage so in Analytics verfeinern und zur zukünftigen Nutzung in Warnungen speichern. Weitere Informationen zum Speichern von Abfragen finden Sie unter [Suchen von Daten mit Protokollsuchen in Log Analytics](../../azure-monitor/log-query/log-query-overview.md) oder [Was ist Log Analytics?](../../azure-monitor/log-query/log-query-overview.md). 

1.  *Protokollwarnungen*: Wenn Sie diese Option ausgewählt haben, können im Feld **Suchabfrage** Abfragen für Warnungen angegeben werden. Ist die Abfragesyntax falsch, wird eine Fehlermeldung in ROT angezeigt. Wenn die Abfragesyntax korrekt ist, werden für die angegebene Abfrage Referenzverlaufsdaten als Diagramm angezeigt. Dabei besteht die Möglichkeit, das Zeitfenster von den letzten sechs Stunden bis zur letzten Woche anzupassen.

 ![Konfigurieren einer Warnungsregel](media/alerts-log/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Die Visualisierung von Verlaufsdaten kann nur angezeigt werden, wenn die Abfrageergebnisse Details in Bezug auf die Zeit enthalten. Wenn Ihre Abfrage zusammengefasste Daten oder bestimmte Spaltenwerte zurückgibt, werden diese als einzelner Plot angezeigt.

    >  Wenn Sie als Typ für die Metrikmessung Protokollwarnungen mit Application Insights verwenden, können Sie angeben, nach welcher Variablen die Daten gruppiert werden sollen. Verwenden Sie dazu die Option **Aggregieren auf**, wie hier zu sehen:

    ![Option „Aggregieren auf“](media/alerts-log/aggregate-on.png)

1.  *Protokollwarnungen*: Ist die Visualisierung eingerichtet, kann die **Warnungslogik** aus den angezeigten Optionen „Bedingung“, „Aggregation“ und schließlich „Schwellenwert“ ausgewählt werden. Geben Sie schließlich über die Option **Zeitraum** den Zeitraum für die Bewertung der angegebenen Bedingung in der Logik an. Legen Sie durch Auswahl der **Häufigkeit** außerdem fest, wie oft die Warnung ausgeführt werden soll.
**Protokollwarnungen** können auf Folgendem basieren:
   - *Anzahl von Datensätzen*: Eine Warnung wird erstellt, wenn die Anzahl der von der Abfrage zurückgegebenen Datensätze entweder größer als oder kleiner als der Wert ist, den Sie angeben.
   - *Metrische Maßeinheit*: Eine Warnung wird erstellt, wenn die einzelnen *aggregierten Werte* in den Ergebnissen den angegebenen Schwellenwert überschreiten und *nach dem ausgewählten Wert gruppiert* werden. Die Anzahl von Sicherheitsverletzungen für eine Warnung ist die Anzahl der Häufigkeit, mit der im ausgewählten Zeitraum der Schwellenwert überschritten wird. Sie können „Sicherheitsverletzungen gesamt“ für eine beliebige Kombination aus Verletzungen in den Ergebnissen angeben oder „Aufeinanderfolgende Sicherheitsverletzungen“, um vorauszusetzen, dass die Sicherheitsverletzungen in aufeinanderfolgenden Stichproben auftreten müssen. Erfahren Sie mehr über [Protokollwarnungen und die zugehörigen Typen](../../azure-monitor/platform/alerts-unified-log.md).


1. Legen Sie im zweiten Schritt einen Namen für ihre Warnung im Feld **Name der Warnungsregel** fest, und geben Sie eine **Beschreibung** mit Einzelheiten zur Warnung sowie den **Schweregrad** über die angegebenen Optionen an. Diese Details werden in allen Warnungs-E-Mails bzw. Benachrichtigungen wiederverwendet oder mithilfe von Push von Azure Monitor übertragen. Darüber hinaus kann der Benutzer durch Umschalten der Option **Regel beim Erstellen aktivieren** festlegen, dass die Warnungsregel bei der Erstellung sofort aktiviert werden soll.

    Nur bei **Protokollwarnungen** sind einige zusätzliche Funktionen in den Warnungsdetails verfügbar:

    - **Warnungen unterdrücken**: Wenn Sie die Unterdrückung für die Warnungsregel aktivieren, werden Aktionen für die Regel nach dem Erstellen einer neuen Warnung für einen vorher festgelegten Zeitraum deaktiviert. Die Regel wird weiter ausgeführt und erstellt Warnungsdatensätze, wenn die Kriterien erfüllt sind. Dies erspart Ihnen die Zeit, das Problem zu beheben, ohne doppelte Aktionen durchzuführen.

        ![Unterdrücken von Warnungen für Protokollwarnungen](media/alerts-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Geben Sie einen größeren Wert für das Unterdrücken von Warnungen als für die Häufigkeit der Warnung an, um sicherzustellen, dass Benachrichtigungen ohne Überlappung beendet werden.

1. Legen Sie im dritten und letzten Schritt bei Bedarf fest, dass eine **Aktionsgruppe** für die Warnungsregel ausgelöst werden muss, wenn die Warnungsbedingung erfüllt ist. Sie können eine vorhandene Aktionsgruppe mit einer Warnung auswählen oder eine neue Aktionsgruppe erstellen. Entsprechend der ausgewählten Aktionsgruppe werden beim Auslösen einer Warnung u.a. folgende Vorgänge in Azure ausgeführt: Senden von E-Mails, Senden von SMS, Aufrufen von Webhooks, Wiederherstellen mit Azure Runbooks und Übertragen von Inhalten an das ITSM-Tool mithilfe von Push. Erfahren Sie mehr über [Aktionsgruppen](action-groups.md).

    > [!NOTE]
    > Informationen zu den Einschränkungen für Runbook-Nutzlasten, die für Protokollwarnungen über Azure-Aktionsgruppen ausgelöst werden, finden Sie unter [Azure-Abonnementdienstgrenzen](../../azure-subscription-service-limits.md).

    Für **Protokollwarnungen** sind einige zusätzliche Funktionen zum Überschreiben der Standardaktionen verfügbar:

    - **E-Mail-Benachrichtigung**: Überschreibt den *E-Mail-Betreff* in der E-Mail, die über die Aktionsgruppe gesendet wurde, wenn mindestens eine E-Mail-Aktion in der genannten Aktionsgruppe vorhanden ist. Der Text der E-Mail kann nicht geändert werden, und dieses Feld ist **nicht** für E-Mail-Adressen vorgesehen.
    - **Benutzerdefinierte JSON-Nutzlast einschließen**: Diese Option überschreibt den von Aktionsgruppen verwendeten JSON-Webhook, wenn mindestens eine Webhookaktion in der genannten Aktionsgruppe vorhanden ist. Der Benutzer kann das JSON-Format angeben, das für alle in zugewiesenen Aktionsgruppen konfigurierte Webhooks verwendet werden soll. Weitere Informationen zu Webhookformaten finden Sie unter [Webhookaktionen für Protokollwarnungsregeln](../../azure-monitor/platform/alerts-log-webhook.md). Die Option „Webhook anzeigen“ bietet die Möglichkeit, das Format anhand von JSON-Beispieldaten zu überprüfen.

        ![Aktionsüberschreibungen für Protokollwarnungen](media/alerts-log/AlertsPreviewOverrideLog.png)


1. Wenn alle Felder gültig sind und mit einem grünen Häkchen versehen sind, kann auf die Schaltfläche **Warnungsregel erstellen** geklickt werden, wodurch eine Warnung in Azure Monitor unter „Warnungen“ erstellt wird. Alle Warnungen können über das Dashboard „Warnungen“ angezeigt werden.

    ![Regelerstellung](media/alerts-log/AlertsPreviewCreate.png)

    Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

Benutzer können ihre Analyseabfrage auch auf der Seite [Logs Analytics im Azure-Portal](../../azure-monitor/log-query/portals.md#log-analytics-page
) abschließen und dann über die Schaltfläche „Warnung festlegen“ eine Warnung erstellen. Anschließend können sie den Anweisungen ab Schritt 6 im obigen Tutorial folgen.

 ![Log Analytics – Warnung festlegen](media/alerts-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Anzeigen und Verwalten von Warnungen im Azure-Portal

1. Wählen Sie im [Portal](https://portal.azure.com/) die Option **Monitor** und im Abschnitt „MONITOR“ dann **Warnungen** aus.  

1. Das **Dashboard „Warnungen“** wird angezeigt, wobei alle Azure-Warnungen (einschließlich Protokollwarnungen) in einem einzelnen Board angezeigt werden; einschließlich jeder Instanz seit der Auslösung der Protokollwarnungsregel. Weitere Informationen finden Sie unter [Warnungsverwaltung](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Protokollwarnungsregeln bestehen aus einer benutzerdefinierten, abfragebasierten Logik, die von Benutzern bereitgestellt wird und somit keinen aufgelösten Zustand aufweisen. Daher wird sie jedes Mal ausgelöst, wenn die in der Protokollwarnungsregel festgelegten Bedingungen erfüllt sind. 


1. Wählen Sie in der oberen Leiste die Schaltfläche **Regeln verwalten** aus, um zum Abschnitt „Regelverwaltung“ zu navigieren, wobei alle erstellten Warnungsregeln sowie deaktivierte Warnungen aufgeführt sind.
    ![Verwalten von Warnungsregeln](media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Verwalten von Protokollwarnungen mithilfe von Azure-Ressourcenvorlagen
Derzeit können Protokollwarnungen mit zwei verschiedenen Ressourcenvorlagen erstellt werden, je nachdem, auf welcher Analyseplattform die Warnung basieren soll (d. h. Log Analytics oder Application Insights).

Daher finden Sie im folgenden Abschnitt Details zur Verwendung der Ressourcenvorlage für Protokollwarnungen für jede Analyseplattform.

### <a name="azure-resource-template-for-log-analytics"></a>Azure-Ressourcenvorlage für Log Analytics
Protokollwarnungen für Log Analytics werden anhand von Warnungsregeln erstellt, die in regelmäßigen Abständen eine gespeicherte Suche ausführen. Wenn die Ergebnisse der Abfrage mit den angegebenen Kriterien übereinstimmen, wird eine Warnung erstellt und mindestens eine Aktion ausgeführt. 

Die Ressourcenvorlagen für die gespeicherte Suche von Log Analytics und Log Analytics-Warnungen stehen im Log Analytics-Abschnitt der Dokumentation zur Verfügung. Erfahren Sie mehr unter [Hinzufügen gespeicherter Suchen und Warnungen von Log Analytics](../../azure-monitor/insights/solutions-resources-searches-alerts.md). Hier finden Sie anschauliche Beispiele sowie die Schemadetails.

### <a name="azure-resource-template-for-application-insights"></a>Azure-Ressourcenvorlage für Application Insights
Protokollwarnungen für Application Insights-Ressourcen sind vom Typ `Microsoft.Insights/scheduledQueryRules/`. Weitere Informationen zu diesem Ressourcentyp finden Sie unter [Azure Monitor: Referenz für die Scheduled Query Rules-API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

Im folgenden finden Sie die Struktur für die auf der [Scheduled Query Rules-Erstellung](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) basierende Ressourcenvorlage mit einem Stichprobendataset als Variablen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
> Das Tag-Feld mit einem ausgeblendetem Link zur Zielressource ist bei Verwendung des API-Aufrufs von [Scheduled Query Rules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) oder der Ressourcenvorlage zwingend erforderlich. 

Die Json-Beispiel oben kann im Rahmen dieser exemplarischen Vorgehensweise z. B. als „sampleScheduledQueryRule.json“ gespeichert und mithilfe von [Azure Resource Manager im Azure-Portal](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) bereitgestellt werden.


## <a name="managing-log-alerts-using-powershell-cli-or-api"></a>Verwalten von Protokollwarnungen mit PowerShell, CLI oder API
Derzeit können Protokollwarnungen mit zwei verschiedenen APIs erstellt werden, die mit Resource Manager kompatibel sind, je nachdem, auf welcher Analyseplattform die Warnung basieren soll (d. h. Log Analytics oder Application Insights).

Daher finden Sie im folgenden Abschnitt Details zur Verwendung der API über PowerShell oder CLI für Protokollwarnungen für die einzelnen Analyseplattformen.

### <a name="powershell-cli-or-api-for-log-analytics"></a>PowerShell, CLI oder API für Log Analytics
Die REST-API für Log Analytics-Warnungen ist RESTful. Der Zugriff darauf erfolgt über die Azure Resource Manager-REST-API. Auf die API kann daher über die PowerShell-Befehlszeile zugegriffen werden. Die Suchergebnisse werden Ihnen im JSON-Format ausgegeben, und Sie können die Ergebnisse programmgesteuert auf viele verschiedene Arten verwenden.

Erfahren Sie mehr über das [Erstellen und Verwalten von Warnungsregeln in Log Analytics mit REST-API](../../azure-monitor/platform/api-alerts.md). Hier finden Sie auch Beispiele für den Zugriff auf die API aus Powershell.

### <a name="powershell-cli-or-api-for-application-insights"></a>PowerShell, CLI oder API für Application Insights
[Azure Monitor – API für geplante Abfrageregeln](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) ist eine REST-API und vollständig kompatibel mit der Azure Resource Manager-REST-API. Daher kann es über Powershell mit dem Resource Manager-Cmdlet und der Azure CLI verwendet werden.

Nachfolgend wird die Verwendung über das PowerShell-Cmdlet von Azure Resource Manager für die weiter oben im [Abschnitt „Ressourcenvorlage“](#azure-resource-template-for-application-insights) dargestellte Ressourcenvorlage (sampleScheduledQueryRule.json) veranschaulicht:
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Nachfolgend wird die Verwendung über den Azure Resource Manager-Befehl in der Azure CLI für die weiter oben im [Abschnitt „Ressourcenvorlage“](#azure-resource-template-for-application-insights) dargestellte Ressourcenvorlage (sampleScheduledQueryRule.json) veranschaulicht:

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Bei erfolgreicher Ausführung wird 201 zurückgegeben, wenn eine neue Warnungsregel erstellt wurde, bzw. 200, wenn eine vorhandene Warnungsregel geändert wurde.


  
## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Protokollwarnungen in Azure-Warnungen](../../azure-monitor/platform/alerts-unified-log.md).
* Machen Sie sich mit [Webhookaktionen für Protokollwarnungen](../../azure-monitor/platform/alerts-log-webhook.md) vertraut.
* Weitere Informationen zu [Application Insights](../../azure-monitor/app/analytics.md)
* Erfahren Sie mehr über [Log Analytics](../../azure-monitor/log-query/log-query-overview.md). 

