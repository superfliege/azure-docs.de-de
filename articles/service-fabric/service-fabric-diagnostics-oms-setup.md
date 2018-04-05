---
title: 'Azure Service Fabric: Einrichten der Überwachung mit OMS Log Analytics | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie die Operations Management Suite für die Visualisierung und Analyse von Ereignissen einrichten, um Ihre Azure Service Fabric-Cluster zu überwachen.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/30/2018
ms.author: dekapur; srrengar
ms.openlocfilehash: 2589efa1808a394f2e32b842efa2ee70809da232
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="set-up-operations-management-suite-log-analytics-for-a-cluster"></a>Einrichten von Operations Management Suite Log Analytics für einen Cluster

Sie können einen Operations Management Suite-Arbeitsbereich (OMS-Arbeitsbereich) über Azure Resource Manager, PowerShell oder den Azure Marketplace einrichten. Wenn Sie eine aktualisierte Resource Manager-Vorlage von Ihrer Bereitstellung verwalten, verwenden Sie zukünftig die gleiche Vorlage für die Einrichtung Ihrer OMS-Umgebung. Die Bereitstellung über den Marketplace ist einfacher, wenn Sie bereits einen Cluster mit aktivierter Diagnose bereitgestellt haben. Wenn Sie in dem Konto, für das Sie die OMS bereitstellen, keinen Zugriff auf Abonnementebene haben, führen Sie die Bereitstellung über PowerShell oder über die Resource Manager-Vorlage durch.

> [!NOTE]
> Zum Einrichten der OMS zur Überwachung des Clusters muss die Diagnose aktiviert sein, um Ereignisse auf Cluster- oder auf Plattformebene anzeigen zu können.

## <a name="deploy-oms-by-using-azure-marketplace"></a>Bereitstellen der OMS mithilfe des Azure Marketplace

Wenn Sie einen OMS-Arbeitsbereich nach dem Bereitstellen eines Clusters hinzufügen möchten, navigieren Sie im Portal zum Azure Marketplace, und suchen Sie nach **Service Fabric-Analyse**:

1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Neu** aus. 

2. Suchen Sie nach **Service Fabric-Analyse**. Wählen Sie die angezeigte Ressource aus.

3. Klicken Sie auf **Erstellen**.

    ![OMS – SF-Analyse im Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Wählen Sie im Fenster für die Erstellung der Service Fabric-Analyse im Feld **OMS-Arbeitsbereich** die Option **Arbeitsbereich auswählen** und dann **Neuen Arbeitsbereich erstellen** aus. Nehmen Sie die erforderlichen Eingaben vor. Einzige Anforderung hierbei ist, dass für den Service Fabric-Cluster und den OMS-Arbeitsbereich dasselbe Abonnement verwendet werden muss. Nachdem die Eingaben überprüft wurden, wird Ihr OMS-Arbeitsbereich bereitgestellt. Der Vorgang dauert nur wenige Minuten.

5. Wählen Sie anschließend unten im Fenster zur Erstellung der Service Fabric-Analyse erneut **Erstellen** aus. Stellen Sie sicher, dass der neue Arbeitsbereich unter **OMS-Arbeitsbereich** angezeigt wird. Mit dieser Aktion wird die Lösung dem erstellten Arbeitsbereich hinzugefügt.

Wenn Sie Windows verwenden, fahren Sie mit den folgenden Schritten zum Verknüpfen der OMS mit dem Speicherkonto fort, in dem die Clusterereignisse gespeichert werden. 

>[!NOTE]
>Für Linux-Cluster ist dies noch nicht verfügbar. 

### <a name="connect-the-oms-workspace-to-your-cluster"></a>Verbinden des OMS-Arbeitsbereichs mit Ihrem Cluster 

1. Der Arbeitsbereich muss mit den Diagnosedaten aus Ihrem Cluster verknüpft werden. Navigieren Sie zu der Ressourcengruppe, in der Sie die Lösung der Service Fabric-Analyse erstellt haben. Wählen Sie **ServiceFabric\<OMS-Arbeitsbereichsname\>** aus, und wechseln Sie zur zugehörigen Übersichtsseite. Dort können Sie Lösungseinstellungen und Arbeitsbereichseinstellungen ändern und auf das OMS-Portal zugreifen.

2. Wählen Sie im linken Navigationsmenü unter **Speicherkontoprotokolle** die Option **Arbeitsbereichsdatenquellen** aus.

3. Wählen Sie auf der Seite **Speicherkontoprotokolle** ganz oben die Option **Hinzufügen** aus, um dem Arbeitsbereich die Protokolle Ihres Clusters hinzuzufügen.

4. Wählen Sie **Speicherkonto** aus, um das geeignete in Ihrem Cluster erstellte Konto hinzuzufügen. Wenn Sie den Standardnamen verwendet haben, heißt das Speicherkonto **sfdg\<Ressourcengruppenname\>**. Sie können dies auch mithilfe der Azure Resource Manager-Vorlage überprüfen, die zum Bereitstellen des Clusters verwendet wurde, indem Sie den für **applicationDiagnosticsStorageAccountName** verwendeten Wert prüfen. Wenn der Name nicht angezeigt wird, scrollen Sie nach unten, und wählen Sie **Weitere laden** aus. Wählen Sie den Speicherkontonamen aus.

5. Geben Sie den Datentyp an. Legen Sie ihn auf **Service Fabric-Ereignisse** fest.

6. Vergewissern Sie sich, dass die Quelle automatisch auf **WADServiceFabric\*EventTable** festgelegt wird.

7. Wählen Sie **OK** aus, um den Arbeitsbereich mit den Protokollen Ihres Clusters zu verbinden.

    ![Hinzufügen von Speicherkontoprotokollen zu OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Das Konto wird jetzt als Teil Ihrer Speicherkontoprotokolle in den Datenquellen Ihres Arbeitsbereichs angezeigt.

Damit haben Sie die Service Fabric-Analyse-Lösung einem OMS Log Analytics-Arbeitsbereich hinzugefügt, der jetzt vollständig mit der Plattform Ihres Clusters sowie der Anwendungsprotokolltabelle verbunden ist. Sie können dem Arbeitsbereich zusätzliche Quellen auf die gleiche Weise hinzufügen.


## <a name="deploy-oms-by-using-a-resource-manager-template"></a>Bereitstellen der OMS mithilfe einer Resource Manager-Vorlage

Beim Bereitstellen eines Clusters mithilfe einer Resource Manager-Vorlage wird mit der Vorlage ein neuer OMS-Arbeitsbereich erstellt, diesem die Service Fabric-Lösung hinzugefügt und der Arbeitsbereich so konfiguriert, dass Daten aus den entsprechenden Speichertabellen gelesen werden.

Sie können [diese Beispielvorlage](https://github.com/krnese/azure-quickstart-templates/tree/master/service-fabric-oms) verwenden und entsprechend Ihren Anforderungen ändern.

Nehmen Sie folgende Änderungen vor:
1. Fügen Sie Ihren Parametern `omsWorkspaceName` und `omsRegion` hinzu, indem Sie den in der Datei *template.json* definierten Parametern den folgenden Codeausschnitt hinzufügen. Die Standardwerte können nach Bedarf geändert werden. Fügen Sie die beiden neuen Parameter auch in der Datei *parameters.json* hinzu, um ihre Werte für die Ressourcenbereitstellung zu definieren:
    
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

    Die `omsRegion`-Werte müssen einem bestimmten Satz der Werte entsprechen. Wählen Sie denjenigen aus, der sich möglichst nah an der Bereitstellung Ihres Clusters befindet.

2. Wenn Sie Anwendungsprotokolle an die OMS senden, vergewissern Sie sich zunächst, dass `applicationDiagnosticsStorageAccountType` und `applicationDiagnosticsStorageAccountName` als Parameter in der Vorlage enthalten sind. Wenn dies nicht der Fall ist, fügen Sie sie dem Abschnitt „variables“ hinzu, und bearbeiten Sie die Werte nach Bedarf. Sie können sie auch als Parameter einfügen. Verwenden Sie dabei das oben angegebene Format.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Fügen Sie die Service Fabric-OMS-Lösung den Variablen Ihrer Vorlage hinzu:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Fügen Sie am Ende des Abschnitts „resources“ (nach der Deklaration der Service Fabric-Clusterressource) Folgendes hinzu:

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

5. Stellen Sie die Vorlage als Resource Manager-Upgrade für Ihren Cluster bereit. Verwenden Sie dazu die `New-AzureRmResourceGroupDeployment`-API im AzureRM-PowerShell-Modul. Beispielbefehl:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Azure Resource Manager erkennt, dass es sich bei diesem Befehl um ein Update für eine vorhandene Ressource handelt. Daher werden nur die Änderungen zwischen der Vorlage für die vorhandene Bereitstellung und der neu bereitgestellten Vorlage verarbeitet.

## <a name="deploy-oms-by-using-azure-powershell"></a>Bereitstellen der OMS mithilfe von Azure PowerShell

Sie können Ihre OMS Log Analytics-Ressource mit dem Befehl `New-AzureRmOperationalInsightsWorkspace` auch über PowerShell bereitstellen. Stellen Sie hierfür sicher, dass [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1) installiert ist. Erstellen Sie mithilfe dieses Skripts einen neuen OMS Log Analytics-Arbeitsbereich, und fügen Sie diesem die Service Fabric-Lösung hinzu: 

```PowerShell

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

Führen Sie anschließend die Schritte im vorherigen Abschnitt aus, um OMS Log Analytics mit dem entsprechenden Speicherkonto zu verknüpfen.

Mithilfe von PowerShell können Sie zudem weitere Lösungen hinzufügen oder andere Änderungen am OMS-Arbeitsbereich vornehmen. Weitere Informationen finden Sie unter [Verwalten von Log Analytics mit PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Nächste Schritte
* [Stellen Sie den OMS-Agent auf Ihren Knoten bereit](service-fabric-diagnostics-oms-agent.md), um Leistungsindikatoren zu erfassen und Docker-Statistiken und -Protokolle für Ihre Container zu sammeln.
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
* [Erstellen benutzerdefinierter Ansichten in Log Analytics mithilfe des Ansicht-Designers](../log-analytics/log-analytics-view-designer.md)
