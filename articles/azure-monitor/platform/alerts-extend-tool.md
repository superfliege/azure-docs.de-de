---
title: Erweitern von Warnungen von Log Analytics auf Azure
description: Dieser Artikel beschreibt die Tools und die API zum Erweitern von Warnungen aus Log Analytics auf Azure-Warnungen.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: dc8c1733f506870765523b17c1fc3e283ff9cbdb
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423274"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Erweitern von Warnungen aus Log Analytics in Azure-Warnungen
Die Warnfunktion in Azure Log Analytics wird durch Azure-Warnungen ersetzt. Im Rahmen dieses Übergangs werden Warnungen, die Sie ursprünglich in Log Analytics konfiguriert haben, auf Azure erweitert. Wenn Sie nicht warten möchten, bis diese automatisch in Azure verschoben werden, können Sie den Prozess auslösen:

- Manuell über das Operations Management Suite-Portal 
- Programmgesteuert mithilfe der AlertsVersion-API  

> [!NOTE]
> Ab dem 14. Mai 2018 erweitert Microsoft in Public Cloud-Instanzen von Log Analytics erstellte Warnungen automatisch in einer wiederholten Serie bis zum vollständigen Abschluss auf Azure-Warnungen. Wenn beim Erstellen von [Aktionsgruppen](../../azure-monitor/platform/action-groups.md) irgendwelche Probleme auftreten, verwenden Sie [diese Wartungsschritte](alerts-extend-tool.md#troubleshooting), um Aktionsgruppen automatisch erstellen zu lassen. Sie können diese Schritte bis zum 5. Juli 2018 verwenden. *Gilt nicht für Azure Government- und Sovereign Cloud-Benutzer von Log Analytics*. 

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Option 1: Initiieren über das Operations Management Suite-Portal
Die folgenden Schritte beschreiben, wie Sie Warnungen für den Arbeitsbereich über das Operations Management Suite-Portal erweitern.  

1. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
2. Wählen Sie im Log Analytics-Abonnementbereich einen Arbeitsbereich aus, und wählen Sie dann die Kachel **OMS-Portal** aus.
![Screenshot des Log Analytics-Abonnementbereichs mit hervorgehobener Kachel für das OMS-Portal](media/alerts-extend-tool/azure-portal-01.png) 
3. Nachdem Sie zum Operations Management Suite-Portal weitergeleitet wurden, klicken Sie auf das Symbol **Einstellungen**.
![Screenshot des Operations Management Suite-Portals mit hervorgehobenem Symbol „Einstellungen“](media/alerts-extend-tool/oms-portal-settings-option.png) 
4. Klicken Sie auf der Seite **Einstellungen** auf **Warnungen**.  
5. Wählen Sie **Auf Azure erweitern**.
![Screenshot der Seite „Einstellungen“ im Operations Management Suite-Portal mit hervorgehobener Option „Auf Azure erweitern“](media/alerts-extend-tool/ExtendInto.png)
6. Ein Assistent mit drei Schritten wird im Bereich **Warnungen** angezeigt. Lesen Sie die Übersicht, und wählen Sie **Weiter**.
![Screenshot von Schritt 1 des Assistenten](media/alerts-extend-tool/ExtendStep1.png)  
7. Im zweiten Schritt wird eine Zusammenfassung der vorgeschlagenen Änderungen mit den entsprechenden [Aktionsgruppen](../../azure-monitor/platform/action-groups.md) für die Warnungen angezeigt. Wenn für mehrere Warnungen ähnliche Aktionen angezeigt werden, schlägt der Assistent vor, allen eine einzige Aktionsgruppe zuzuordnen.  Die Namenskonvention lautet wie folgt: *Arbeitsbereichsname_AG_#Nummer*. Zum Fortsetzen des Vorgangs klicken Sie auf **Weiter**.
![Screenshot von Schritt 2 des Assistenten](media/alerts-extend-tool/ExtendStep2.png)  
8. Wählen Sie im letzten Schritt des Assistenten **Fertig stellen**, und bestätigen Sie bei Aufforderung den Start des Vorgangs. Optional können Sie eine E-Mail-Adresse angeben, damit Sie benachrichtigt werden, wenn der Prozess abgeschlossen ist und alle Warnungen erfolgreich in Azure-Warnungen verschoben wurden.
![Screenshot von Schritt 3 des Assistenten](media/alerts-extend-tool/ExtendStep3.png)

Wenn der Assistent beendet ist, wird die Option zum Erweitern von Warnungen auf Azure auf der Seite **Warnungseinstellungen** nicht mehr angezeigt. Im Hintergrund werden Ihre Warnungen in Azure verschoben, und dieser Vorgang kann eine Weile dauern. Während des Vorgangs können Sie über das Operations Management Suite-Portal keine Änderungen an Warnungen vornehmen. Den aktuellen Status können Sie dem Banner am oberen Rand des Portals entnehmen. Wenn Sie zuvor eine E-Mail-Adresse angegeben haben, erhalten eine E-Mail, wenn der Vorgang erfolgreich abgeschlossen wurde.  


Warnungen werden weiterhin im Operations Management Suite-Portal aufgeführt, auch nachdem sie erfolgreich in Azure verschoben wurden.
![Screenshot der Seite mit Warnungseinstellungen im Operations Management Suite-Portal](media/alerts-extend-tool/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Option 2: Verwenden der AlertsVersion-API
Sie können die AlertsVersion-API von Log Analytics zum Erweitern von Warnungen aus Log Analytics in Azure-Warnungen auf jedem beliebigen Client verwenden, der eine REST-API aufrufen kann. Die API kann in PowerShell über das Open Source-Befehlszeilentool [ARMClient](https://github.com/projectkudu/ARMClient) aufgerufen werden. Die Ergebnisse können im JSON-Format ausgegeben werden.  

Zum Verwenden der API erstellen Sie zunächst eine GET-Anforderung. Diese bewertet die vorgeschlagenen Änderungen und gibt eine Zusammenfassung zurück, bevor Sie die Erweiterung auf Azure tatsächlich mithilfe einer POST-Anforderung durchführen. In den Ergebnissen werden Ihre Warnungen und eine vorgeschlagene Liste von [Aktionsgruppen](../../azure-monitor/platform/action-groups.md) im JSON-Format aufgeführt. Wenn für mehrere Warnungen ähnliche Aktionen angezeigt werden, schlägt der Dienst vor, allen eine einzige Aktionsgruppe zuzuordnen. Die Namenskonvention lautet wie folgt: *Arbeitsbereichsname_AG_#Nummer*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Wenn die GET-Anforderung erfolgreich ist, wird ein HTTP-Statuscode 200 zusammen mit einer Liste der Warnungen und vorgeschlagenen Aktionsgruppen in den JSON-Daten zurückgegeben. Hier sehen Sie eine Beispielantwort:

```json
{
    "version": 1,
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "https://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}

```
Wenn für den angegebenen Arbeitsbereich keine Warnregeln definiert sind, geben die JSON-Daten Folgendes zurück:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Wenn alle Warnungen im angegebenen Arbeitsbereich bereits auf Azure erweitert wurden, lautet die Antwort auf die GET-Anforderung folgendermaßen:

```json
{
    "version": 2
}
```

Um die Migration von Warnungen zu Azure zu starten, initiieren Sie eine POST-Antwort. Die POST-Antwort bestätigt Ihre Absicht und Ihr Einverständnis dafür, Warnungen aus Log Analytics auf Azure-Warnungen zu erweitern. Die Aktivität wird zeitlich geplant, und die Warnungen werden wie angegeben und basierend auf den Ergebnissen der zuvor durchgeführten GET-Antwort verarbeitet. Optional können Sie eine Liste von E-Mail-Adressen angeben, an die Log Analytics einen Bericht sendet, wenn der geplante Hintergrundprozess zur Migration der Warnungen erfolgreich abgeschlossen wurde. Sie können das folgende Anforderungsbeispiel verwenden:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Das Ergebnis der Migration von Warnungen zu Azure-Warnungen variiert basierend auf der Zusammenfassung, die von der GET-Antwort bereitgestellt wurde. Wenn sie geplant werden, können Warnungen in Log Analytics vorübergehend nicht im Operations Management Suite-Portal bearbeitet werden. Sie können jedoch neue Warnungen erstellen. 

Wenn die POST-Anforderung erfolgreich ist, werden ein HTTP 200 OK-Status sowie die folgende Antwort zurückgegeben:

```json
{
    "version": 2
}
```

Diese Antwort gibt an, dass die Warnungen erfolgreich auf Azure-Warnungen erweitert wurden. Die Versionseigenschaft dient nur zur Überprüfung, ob Warnungen auf Azure erweitert wurden, und steht nicht im Zusammenhang mit der [Log Analytics-API für die Suche](../../azure-monitor/platform/api-alerts.md). Wenn die Warnungen erfolgreich auf Azure erweitert wurden, erhalten alle E-Mail-Adressen, die mit der POST-Anforderung angegeben wurden, einen Bericht. Wenn für alle Warnungen im angegebenen Arbeitsbereich bereits eine Erweiterung geplant wurde, erhalten Sie als Antwort auf Ihre POST-Anforderung einen Statuscode 403 mit dem Hinweis, dass der Versuch unzulässig sei. Um Fehlermeldungen anzuzeigen oder zu ermitteln, ob der Prozess abgestürzt ist, können Sie eine GET-Anforderung senden. Wenn eine Fehlermeldung vorliegt, wird sie zusammen mit den Zusammenfassungsinformationen zurückgegeben.

```json
{
    "version": 1,
    "message": "OMS was unable to extend your alerts into Azure, Error: The subscription is not registered to use the namespace 'microsoft.insights'. OMS will schedule extending your alerts, once remediation steps illustrated in the troubleshooting guide are done.",
    "recipients": [
       "john.doe@email.com",
       "jane.doe@email.com"
     ],
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "https://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}              

```


## <a name="option-3-use-a-custom-powershell-script"></a>Option 3: Verwenden eines benutzerdefinierten PowerShell-Skripts
 Wenn Microsoft Ihre Warnungen nicht erfolgreich aus dem Operations Management Suite-Portal auf Azure erweitern konnte, können Sie dies bis zum 5. Juli 2018 manuell durchführen. In den vorherigen beiden Abschnitten wurden die beiden Optionen für die manuelle Erweiterung behandelt.

Nach dem 5. Juli 2018 werden alle Warnungen aus dem Operations Management Suite-Portal auf Azure erweitert. Für Benutzer, die die [erforderlichen vorgeschlagenen Wiederherstellungsschritte](#troubleshooting) nicht durchgeführt haben, werden Warnungen ohne das Auslösen von Aktionen oder Benachrichtigungen ausgeführt, weil keine [Aktionsgruppen](../../azure-monitor/platform/action-groups.md) zugeordnet wurden. 

Um [Aktionsgruppen](../../azure-monitor/platform/action-groups.md) für Warnungen manuell in Log Analytics zu erstellen, verwenden Sie das folgende Beispielskript:
```PowerShell
########## Input Parameters Begin ###########


$subscriptionId = ""
$resourceGroup = ""
$workspaceName = "" 


########## Input Parameters End ###########

armclient login

try
{
    $workspace = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/"$workspaceName"?api-version=2015-03-20 | ConvertFrom-Json
    $workspaceId = $workspace.properties.customerId
    $resourceLocation = $workspace.location
}
catch
{
    "Please enter valid input parameters i.e. Subscription Id, Resource Group and Workspace Name !!"
    exit
}

# Get Extend Summary of the Alerts
"`nGetting Extend Summary of Alerts for the workspace...`n"
try
{

    $value = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview

    "Extend preview summary"
    "=========================`n"

    $value

    $result = $value | ConvertFrom-Json
}
catch
{

    $ErrorMessage = $_.Exception.Message
    "Error occurred while fetching/parsing Extend summary: $ErrorMessage"
    exit 
}

if ($result.version -eq 2)
{
    "`nThe alerts in this workspace have already been extended to Azure."
    exit
}

$in = Read-Host -Prompt "`nDo you want to continue extending the alerts to Azure? (Y/N)"

if ($in.ToLower() -ne "y")
{
    exit
} 


# Check for resource provider registration
try
{
    $val = armclient get subscriptions/$subscriptionId/providers/microsoft.insights/?api-version=2017-05-10 | ConvertFrom-Json
    if ($val.registrationState -eq "NotRegistered")
    {
        $val = armclient post subscriptions/$subscriptionId/providers/microsoft.insights/register/?api-version=2017-05-10
    }
}
catch
{
    "`nThe user does not have required access to register the resource provider. Please try with user having Contributor/Owner role in the subscription"
    exit
}

$actionGroupsMap = @{}
try
{
    "`nCreating new action groups for alerts extension...`n"
    foreach ($actionGroup in $result.migrationSummary.actionGroups)
    {
        $actionGroupName = $actionGroup.actionGroupName
        $actions = $actionGroup.actions
        if ($actionGroupsMap.ContainsKey($actionGroupName))
        {
            continue
        } 
        
        # Create action group payload
        $shortName = $actionGroupName.Substring($actionGroupName.LastIndexOf("AG_"))
        $properties = @{"groupShortName"= $shortName; "enabled" = $true}
        $emailReceivers = New-Object Object[] $actions.emailIds.Count
        $webhookReceivers = New-Object Object[] $actions.webhookActions.Count
        
        $count = 0
        foreach ($email in $actions.emailIds)
        {
            $emailReceivers[$count] = @{"name" = "Email$($count+1)"; "emailAddress" = "$email"}
            $count++
        }

        $count = 0
        foreach ($webhook in $actions.webhookActions)
        {
            $webhookReceivers[$count] = @{"name" = "$($webhook.name)"; "serviceUri" = "$($webhook.serviceUri)"}
            $count++
        }

        $itsmAction = $actions.itsmAction
        if ($itsmAction.connectionId -ne $null)
        {
            $val = @{
            "name" = "ITSM"
            "workspaceId" = "$subscriptionId|$workspaceId"
            "connectionId" = "$($itsmAction.connectionId)"
            "ticketConfiguration" = $itsmAction.templateInfo
            "region" = "$resourceLocation"
            }
            $properties["itsmReceivers"] = @($val)  
        }

        $properties["emailReceivers"] = @($emailReceivers)
        $properties["webhookReceivers"] = @($webhookReceivers)
        $armPayload = @{"properties" = $properties; "location" = "Global"} | ConvertTo-Json -Compress -Depth 4

    
        # Azure Resource Manager call to create action group
        $response = $armPayload | armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroupName/?api-version=2017-04-01

        "Created Action Group with name $actionGroupName" 
        $actionGroupsMap[$actionGroupName] = $actionGroup.actionGroupResourceId.ToLower()
        $index++
    }

    "`nSuccessfully created all action groups!!"
}
catch
{
    $ErrorMessage = $_.Exception.Message

    #Delete all action groups in case of failure
    "`nDeleting newly created action groups if any as some error happened..."
    
    foreach ($actionGroup in $actionGroupsMap.Keys)
    {
        $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
    }

    "`nError: $ErrorMessage"
    "`nExiting..."
    exit
}

# Update all alerts configuration to the new version
"`nExtending OMS alerts to Azure...`n"

try
{
    $index = 1
    foreach ($alert in $result.migrationSummary.alerts)
    {
        $uri = $alert.alertId + "?api-version=2015-03-20"
        $config = armclient get $uri | ConvertFrom-Json
        $aznsNotification = @{
            "GroupIds" = @($actionGroupsMap[$alert.actionGroupName])
        }
        if ($alert.customWebhookPayload)
        {
            $aznsNotification.Add("CustomWebhookPayload", $alert.customWebhookPayload)
        }
        if ($alert.customEmailSubject)
        {
            $aznsNotification.Add("CustomEmailSubject", $alert.customEmailSubject)
        }      

        # Update alert version
        $config.properties.Version = 2

        $config.properties | Add-Member -MemberType NoteProperty -Name "AzNsNotification" -Value $aznsNotification
        $payload = $config | ConvertTo-Json -Depth 4
        $response = $payload | armclient put $uri
    
        "Extended alert with name $($alert.alertName)"
        $index++
    }
}
catch
{
    $ErrorMessage = $_.Exception.Message   
    if ($index -eq 1)
    {
        "`nDeleting all newly created action groups as no alerts got extended..."
        foreach ($actionGroup in $actionGroupsMap.Keys)
        {
            $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
        }
        "`nDeleted all action groups."  
    }
    
    "`nError: $ErrorMessage"
    "`nPlease resolve the issue and try extending again!!"
    "`nExiting..."
    exit
}

"`nSuccessfully extended all OMS alerts to Azure!!" 

# Update version of workspace to indicate extension
"`nUpdating alert version information in OMS workspace..." 

$response = armclient post "/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview&iversion=2"

"`nExtension complete!!"
```


### <a name="about-the-custom-powershell-script"></a>Informationen zum benutzerdefinierten PowerShell-Skript 
Im Folgenden finden Sie wichtige Informationen zur Verwendung des Skripts:
- Eine Voraussetzung dafür ist die Installation von [ARMClient](https://github.com/projectkudu/ARMClient), ein Open Source-Befehlszeilentool, das den Aufruf der Azure Resource Manager-API vereinfacht.
- Um das Skript auszuführen, muss Ihr Azure-Abonnement eine Rolle „Mitwirkender“ oder „Besitzer“ aufweisen.
- Sie müssen die folgenden Parameter angeben:
    - $subscriptionId: Die Azure-Abonnement-ID, die dem Log Analytics-Arbeitsbereich der Operations Management Suite zugeordnet ist.
    - $resourceGroup: Die Azure-Ressourcengruppe für den Log Analytics-Arbeitsbereich der Operations Management Suite.
    - $workspaceName: Der Name des Log Analytics-Arbeitsbereichs der Operations Management Suite.

### <a name="output-of-the-custom-powershell-script"></a>Ausgabe des benutzerdefinierten PowerShell-Skripts
Das Skript ist sehr detailliert und gibt während der Ausführung die einzelnen Schritte aus: 
- Es zeigt eine Zusammenfassung mit den Informationen zu den vorhandenen Log Analytics-Warnungen im Arbeitsbereich der Operations Management Suite. Die Zusammenfassung enthält auch Informationen zu den Azure-Aktionsgruppen, die für die ihnen zugeordneten Aktionen erstellt werden sollen. 
- Sie werden aufgefordert, mit der Erweiterung fortzufahren oder den Vorgang nach dem Anzeigen der Zusammenfassung zu beenden.
- Wenn Sie die Erweiterung fortsetzen, werden neue Azure-Aktionsgruppen erstellt, denen alle vorhandenen Warnungen zugeordnet werden. 
- Das Skript wird mit der Meldung „Erweiterung abgeschlossen!“ beendet. Bei zwischenzeitlichen Fehlern zeigt das Skript nachfolgende Fehler an.

## <a name="troubleshooting"></a>Problembehandlung 
Bei der Erweiterung von Warnungen können Probleme verhindern, dass das System die erforderlichen [Aktionsgruppen](../../azure-monitor/platform/action-groups.md) erstellt. In solchen Fällen wird Ihnen eine Fehlermeldung in einem Banner im Abschnitt **Warnung** des Operations Management Suite-Portals oder im an die API gerichteten GET-Aufruf angezeigt.

> [!IMPORTANT]
> Wenn die auf der Azure Public Cloud basierenden Log Analytics-Benutzer die folgenden Wiederherstellungsschritte nicht vor dem 5. Juli 2018 durchführen, werden Warnungen in Azure ausgeführt, lösen aber keine Aktion oder Benachrichtigung aus. Um Benachrichtigungen für Warnungen zu erhalten, müssen Sie [Aktionsgruppen](../../azure-monitor/platform/action-groups.md) manuell bearbeiten und hinzufügen oder das oben beschriebene [benutzerdefinierte PowerShell-Skript](#option-3---using-custom-powershell-script) verwenden.

Hier werden die Wiederherstellungsschritte für die einzelnen Fehler aufgeführt:
- **Error: Scope Lock is present at subscription/resource group level for write operations** (Fehler: Bereichssperre auf Abonnement-/Ressourcengruppenebene für Schreibvorgänge):   ![Screenshot der Seite „Warnungseinstellungen“ im Operations Management Suite-Portal mit hervorgehobener Fehlermeldung zur Bereichssperre](media/alerts-extend-tool/ErrorScopeLock.png)

    Wenn die Bereichssperre aktiviert ist, schränkt dieses Feature neue Änderungen in dem Abonnement bzw. in der Ressourcengruppe ein, das bzw. die den Log Analytics-Arbeitsbereich (Operations Management Suite) enthält. Das System kann Warnungen nicht auf Azure erweitern und nicht die erforderlichen Aktionsgruppen erstellen.
    
    Um das Problem zu lösen, löschen Sie die *ReadOnly*-Sperre aus dem Abonnement oder der Ressourcengruppe, das bzw. die den Arbeitsbereich enthält. Verwenden Sie hierzu das Azure-Portal, PowerShell, die Azure CLI oder die API. Weitere Informationen finden Sie unter [Verwenden von Ressourcensperren](../../azure-resource-manager/resource-group-lock-resources.md). 
    
    Wenn Sie den Fehler mithilfe der in diesem Artikel beschriebenen Schritte beheben, erweitert Operations Management Suite Ihre Warnungen während der für den nächsten Tag geplanten Ausführung auf Azure. Sie müssen keine weiteren Aktionen durchführen oder starten.

- **Error: Policy is present at subscription/resource group level** (Fehler: Richtlinie auf Abonnement-/Ressourcengruppenebene):   ![Screenshot der Seite „Warnungseinstellungen“ im Operations Management Suite-Portal mit hervorgehobener Fehlermeldung zu Richtlinien](media/alerts-extend-tool/ErrorPolicy.png)

    Wenn [Azure Policy](../../governance/policy/overview.md) angewendet wird, werden neue Ressourcen in einem Abonnement oder in einer Ressourcengruppe eingeschränkt, das bzw. die den Log Analytics-Arbeitsbereich (Operations Management Suite) enthält. Das System kann Warnungen nicht auf Azure erweitern und nicht die erforderlichen Aktionsgruppen erstellen.
    
    Beheben Sie das Problem durch Bearbeiten der Richtlinie, die den Fehler *[RequestDisallowedByPolicy](../../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* verursacht. Dieser Fehler verhindert die Erstellung neuer Ressourcen für das Abonnement oder die Ressourcengruppe, das bzw. die den Arbeitsbereich enthält. Verwenden Sie hierzu das Azure-Portal, PowerShell, die Azure CLI oder die API. Sie können Aktionen überwachen, um die entsprechende Richtlinie zu finden, die den Fehler verursacht. Weitere Informationen finden Sie unter [Anzeigen von Aktivitätsprotokollen, um Aktionen an Ressourcen zu überwachen](../../azure-resource-manager/resource-group-audit.md). 
    
    Wenn Sie den Fehler mithilfe der in diesem Artikel beschriebenen Schritte beheben, erweitert Operations Management Suite Ihre Warnungen während der für den nächsten Tag geplanten Ausführung auf Azure. Sie müssen keine weiteren Aktionen durchführen oder starten.


## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die neue [Benutzeroberfläche „Azure-Warnungen“](../../azure-monitor/platform/alerts-overview.md).
* Erfahren Sie mehr über [Protokollwarnungen in Azure-Warnungen](alerts-unified-log.md).

