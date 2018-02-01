---
title: "Azure Service Fabric: Einrichten der Überwachung mit OMS Log Analytics | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie die OMS für die Visualisierung und Analyse von Ereignissen einrichten, um Ihre Azure Service Fabric-Cluster zu überwachen."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2017
ms.author: dekapur
ms.openlocfilehash: 53b06c5a1395f34c96d4011366835a920d5c670b
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2018
---
# <a name="set-up-oms-log-analytics-for-a-cluster"></a>Einrichten von OMS Log Analytics für einen Cluster

Sie können einen OMS-Arbeitsbereich über den Azure Resource Manager, über PowerShell oder über den Azure Marketplace einrichten. Wenn Sie eine aktualisierte Resource Manager-Vorlage von Ihrer Bereitstellung verwalten, verwenden Sie zukünftig die gleiche Vorlage für die Einrichtung Ihrer OMS-Umgebung. Die Bereitstellung über den Marketplace ist einfacher, wenn Sie bereits einen Cluster mit aktivierter Diagnose bereitgestellt haben. Falls Sie im Konto, für das Sie die OMS bereitstellen, keinen Zugriff auf Abonnementebene besitzen, verwenden Sie PowerShell, oder führen Sie die Bereitstellung über die Resource Manager-Vorlage durch.

> [!NOTE]
> Die Diagnose muss für den Cluster aktiviert sein, um Ereignisse auf Cluster-/Plattformebene anzeigen und die OMS für die erfolgreiche Überwachung Ihres Clusters einrichten zu können.

## <a name="deploying-oms-using-azure-marketplace"></a>Bereitstellen der OMS mithilfe des Azure Marketplace

Wenn Sie einen OMS-Arbeitsbereich lieber nach dem Bereitstellen eines Clusters hinzufügen möchten, navigieren Sie im Portal zum Azure Marketplace, und suchen Sie nach *Service Fabric-Analyse*.

1. Klicken Sie im Navigationsmenü auf der linken Seite auf **Neu**. 

2. Suchen Sie nach *Service Fabric-Analyse*. Klicken Sie auf die angezeigte Ressource.

3. Klicken Sie auf **Erstellen**.

    ![OMS – SF-Analyse im Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Klicken Sie im Fenster für die Erstellung der Service Fabric-Analyse für das Feld *OMS-Arbeitsbereich* auf **Arbeitsbereich auswählen** und dann auf **Einen neuen Arbeitsbereich erstellen**. Nehmen Sie die erforderlichen Eingaben vor. Einzige Anforderung hier ist, dass für den Service Fabric-Cluster und den OMS-Arbeitsbereich dasselbe Abonnement verwendet werden muss. Nachdem die Eingaben überprüft wurden, wird Ihr OMS-Arbeitsbereich bereitgestellt. Dieser Vorgang sollte nur wenige Minuten dauern.

5. Wenn Sie fertig sind, klicken Sie erneut auf **Erstellen** am unteren Rand des Fensters zur Erstellung der Service Fabric-Analyse. Stellen Sie sicher, dass der neue Arbeitsbereich unter *OMS-Arbeitsbereich* angezeigt wird. Dadurch wird die Lösung dem Arbeitsbereich hinzugefügt, den Sie gerade erstellt haben.

Wenn Sie Windows verwenden, fahren Sie mit den folgenden Schritten zum Einbinden der OMS in das Speicherkonto fort, in dem Ihre Clusterereignisse gespeichert werden. Die ordnungsgemäße Aktivierung dieser Benutzeroberfläche für Linux-Cluster ist noch in Bearbeitung. Zwischenzeitlich können Sie mit dem Hinzufügen des OMS-Agents zu Ihrem Cluster fortfahren.  

1. Der Arbeitsbereich muss aber trotzdem mit den Diagnosedaten aus Ihrem Cluster verknüpft werden. Navigieren Sie zur Ressourcengruppe, in der Sie die Lösung der Service Fabric-Analyse erstellt haben. Daraufhin sollte ein *ServiceFabric(\<nameOfOMSWorkspace\>)* angezeigt werden. Klicken Sie auf die Lösung, um zur Übersichtsseite zu gelangen. Von dort aus können Sie die Lösungseinstellungen und Arbeitsbereicheinstellungen ändern und zum OMS-Portal navigieren.

2. Klicken Sie im linken Navigationsmenü auf **Speicherkontoprotokolle** unter *Arbeitsbereichsdatenquellen*.

3. Klicken Sie auf der Seite *Speicherkontoprotokolle* ganz oben auf **Hinzufügen**, um die Protokolle Ihres Clusters zum Arbeitsbereich hinzuzufügen.

4. Klicken Sie auf **Speicherkonto**, um das geeignete in Ihrem Cluster erstellte Konto hinzuzufügen. Wenn Sie den Standardnamen verwendet haben, heißt das Speicherkonto *sfdg\<resourceGroupName\>*. Sie können dies ebenso bestätigen, indem Sie die Azure Resource Manager-Vorlage prüfen, die zum Bereitstellen Ihres Clusters verwendet wurde, indem Sie den für den `applicationDiagnosticsStorageAccountName` verwendeten Wert überprüfen. Falls der Name nicht angezeigt wird, müssen Sie unter Umständen nach unten scrollen und auf **Weitere laden** klicken. Klicken Sie auf den richtigen Speicherkontonamen, um ihn auszuwählen.

5. Geben Sie als Nächstes den *Datentyp* an. Dieser sollte auf **Service Fabric-Ereignisse** festgelegt werden.

6. Die *Quelle* sollte automatisch auf *WADServiceFabric\*EventTable* festgelegt werden.

7. Klicken Sie auf **OK**, um Ihren Arbeitsbereich mit den Protokollen Ihres Clusters zu verbinden.

    ![Hinzufügen von Speicherkontoprotokollen zu OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Das Konto sollte jetzt als Teil Ihrer *Speicherkontoprotokolle* in den Datenquellen Ihres Arbeitsbereichs angezeigt werden.

Somit haben Sie nun die Service Fabric-Analyse-Lösung in einem OMS Log Analytics-Arbeitsbereich hinzugefügt, der jetzt vollständig mit der Plattform Ihres Clusters sowie der Anwendungsprotokolltabelle verbunden ist. Sie können dem Arbeitsbereich zusätzliche Quellen auf die gleiche Weise hinzufügen.


## <a name="deploying-oms-using-a-resource-manager-template"></a>Bereitstellen der OMS mithilfe einer Resource Manager-Vorlage

Beim Bereitstellen eines Clusters mithilfe einer Resource Manager-Vorlage sollte die Vorlage auch einen neuen OMS-Arbeitsbereich erstellen, diesem die Service Fabric-Lösung hinzufügen und den Arbeitsbereich so konfigurieren, dass Daten aus den entsprechenden Speichertabellen gelesen werden.

Eine nach Bedarf anpassbare Beispielvorlage finden Sie [hier](https://azure.microsoft.com/resources/templates/service-fabric-oms/). Weitere Vorlagen mit verschiedene Einrichtungsoptionen für OMS-Arbeitsbereiche finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

Hauptänderungen:

1. Fügen Sie `omsWorkspaceName` und `omsRegion` zu Ihren Parametern hinzu. Hierzu muss den in der Datei *template.json* definierten Parametern der folgende Codeausschnitt hinzugefügt werden. Die Standardwerte können nach Bedarf geändert werden. Fügen Sie die beiden neuen Parameter auch der Datei *parameters.json* hinzu, um ihre Werte für die Ressourcenbereitstellung zu definieren:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
    ```

    Die `omsRegion`-Werte müssen einem bestimmten Satz der Werte entsprechen. Dieser sollte sich möglichst nah an der Bereitstellung Ihres Clusters befinden.

2. Wenn Sie Anwendungsprotokolle an die OMS senden, vergewissern Sie sich, dass `applicationDiagnosticsStorageAccountType` und `applicationDiagnosticsStorageAccountName` als Parameter in Ihrer Vorlage enthalten sind. Fügen Sie sie andernfalls wie hier gezeigt dem Variablenabschnitt hinzu, und bearbeiten Sie die Werte nach Bedarf. Wenn Sie möchten, können Sie sie auch als Parameter einfügen. Verwenden Sie dabei das oben angegebene Format.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Fügen Sie die Service Fabric-OMS-Lösung den Variablen Ihrer Vorlage hinzu:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Fügen Sie am Ende des Ressourcenabschnitts (nach der Deklaration der Service Fabric-Clusterressource) Folgendes hinzu:

    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', parameters('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "id": "[resourceId('Microsoft.OperationsManagement/solutions/', variables('solution'))]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
    ```
    
    > [!NOTE]
    > Wenn Sie `applicationDiagnosticsStorageAccountName` als Variable hinzugefügt haben, ändern Sie jeden Verweis darauf in `variables('applicationDiagnosticsStorageAccountName')` anstelle von `parameters('applicationDiagnosticsStorageAccountName')`.

5. Stellen Sie die Vorlage als Resource Manager-Upgrade für Ihren Cluster bereit. Verwenden Sie hierzu die `New-AzureRmResourceGroupDeployment`-API im AzureRM-PowerShell-Modul. Beispielbefehl:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Azure Resource Manager erkennt, dass es sich hierbei um ein Update für eine bereits vorhandene Ressource handelt. Daher werden nur die Änderungen zwischen der Vorlage für die vorhandene Bereitstellung und der neu bereitgestellten Vorlage verarbeitet.

## <a name="deploying-oms-using-azure-powershell"></a>Bereitstellen der OMS mithilfe von Azure PowerShell

Sie können Ihre OMS Log Analytics-Ressource auch über PowerShell bereitstellen. Dies wird mithilfe des Befehls `New-AzureRmOperationalInsightsWorkspace` erreicht. Stellen Sie hierfür sicher, dass Sie [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1) installiert haben. Erstellen Sie mithilfe dieses Skripts einen neuen OMS Log Analytics-Arbeitsbereich, und fügen Sie diesem die Service Fabric-Lösung hinzu: 

```ps

$SubscriptionName = "<Name of your subscription>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<OMS Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

Wenn Sie diesen Vorgang abgeschlossen haben und es sich bei Ihrem Cluster um einen Windows-Cluster handelt, führen Sie die Schritte im obigen Abschnitt durch, um OMS Log Analytics in das entsprechende Speicherkonto einzubinden.

Mithilfe von PowerShell können Sie auch andere Lösungen hinzufügen oder andere Änderungen am OMS-Arbeitsbereich vornehmen. Weitere Informationen hierzu finden Sie unter [Verwalten von Log Analytics mit PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Nächste Schritte
* [Stellen Sie den OMS-Agent auf Ihren Knoten bereit](service-fabric-diagnostics-oms-agent.md), um Leistungsindikatoren zu erfassen und Docker-Statistiken und -Protokolle für Ihre Container zu sammeln.
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
* [Erstellen benutzerdefinierter Ansichten in Log Analytics mithilfe des Ansicht-Designers](../log-analytics/log-analytics-view-designer.md)
