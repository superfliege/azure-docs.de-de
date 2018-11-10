---
title: Erstellen eines Namespace und einer Consumergruppe für Azure Event Hubs anhand einer Vorlage | Microsoft-Dokumentation
description: Erstellen eines Event Hubs-Namespace mit einem Event Hub und einer Consumergruppe mithilfe von Azure Resource Manager-Vorlagen
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/16/2018
ms.author: shvija
ms.openlocfilehash: 584696303bfbaed07f416fb0b3febbcf59d05b35
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085738"
---
# <a name="quickstart-create-an-event-hub-using-azure-resource-manager-template"></a>Schnellstart: Erstellen eines Event Hubs anhand einer Azure Resource Manager-Vorlage
Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und unter [Event Hubs-Features im Überblick](event-hubs-features.md).

In dieser Schnellstartanleitung erstellen Sie einen Event Hub mithilfe einer Azure Resource Manager-Vorlage. Sie verwenden eine Azure Resource Manager-Vorlage, um einen Namespace vom Typ [Event Hubs](event-hubs-what-is-event-hubs.md) mit einem Event Hub und einer Consumergruppe zu erstellen. Dieser Artikel zeigt Ihnen, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen. Informationen zum Erstellen von Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen][Authoring Azure Resource Manager templates].


> [!NOTE]
> Die vollständige [Event Hub- und Consumergruppenvorlage][Event Hub and consumer group template] finden Sie auf GitHub. Mit dieser Vorlage wurde zusätzlich zu einem Event Hub-Namespace und einem Event Hub eine Consumergruppe erstellt. Die neuesten Vorlagen finden Sie, indem Sie im Katalog [Azure-Schnellstartvorlagen][Azure Quickstart Templates] nach „Event Hubs“ suchen.

## <a name="prerequisites"></a>Voraussetzungen
Für diese Schnellstartanleitung benötigen Sie ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.

Wenn Sie die Resource Manager-Vorlage mithilfe von **Azure PowerShell** bereitstellen möchten, [installieren Sie Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

Wenn Sie die Resource Manager-Vorlage über die **Azure-Befehlszeilenschnittstelle** bereitstellen möchten, [installieren Sie die Azure-Befehlszeilenschnittstelle]( /cli/azure/install-azure-cli).

## <a name="create-the-resource-manager-template-json"></a>Erstellen der JSON-Datei für die Resource Manager-Vorlage
Erstellen Sie die JSON-Datei „MyEventHub.json“ mit folgendem Inhalt, und speichern Sie sie in einem Ordner (z.B unter „C:\EventHubsQuickstarts\ResourceManagerTemplate“).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventhub-namespace-name": {
            "type": "String"
        },
        "eventhub_name": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.EventHub/namespaces",
            "sku": {
                "name": "Standard",
                "tier": "Standard",
                "capacity": 1
            },
            "name": "[parameters('eventhub-namespace-name')]",
            "apiVersion": "2017-04-01",
            "location": "East US",
            "tags": {},
            "scale": null,
            "properties": {
                "isAutoInflateEnabled": false,
                "maximumThroughputUnits": 0
            },
            "dependsOn": []
        },
        {
            "type": "Microsoft.EventHub/namespaces/eventhubs",
            "name": "[concat(parameters('eventhub-namespace-name'), '/', parameters('eventhub_name'))]",
            "apiVersion": "2017-04-01",
            "location": "East US",
            "scale": null,
            "properties": {
                "messageRetentionInDays": 7,
                "partitionCount": 1,
                "status": "Active"
            },
            "dependsOn": [
                "[resourceId('Microsoft.EventHub/namespaces', parameters('eventhub-namespace-name'))]"
            ]
        }
    ]
}
```

## <a name="create-the-parameters-json"></a>Erstellen der JSON-Parameter
Erstellen Sie die JSON-Datei „MyEventHub-Parameters.json“ mit Parametern für die Azure Resource Manager-Vorlage. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
        "eventhub-namespace-name": {
            "value": "<specify a name for the event hub namespace>"
        },
        "eventhub_name": {
            "value": "<Specify a name for the event hub in the namespace>"
        }
  }
}
```



## <a name="use-azure-powershell-to-deploy-the-template"></a>Bereitstellen der Vorlage mit Azure PowerShell

### <a name="sign-in-to-azure"></a>Anmelden bei Azure
1. Starten von Azure PowerShell

2. Führen Sie den folgenden Befehl aus, um sich bei Azure anzumelden:

   ```azurepowershell
   Login-AzureRmAccount
   ```
3. Führen Sie die folgenden Befehle aus, um den aktuellen Abonnementkontext festzulegen:

   ```azurepowershell
   Select-AzureRmSubscription -SubscriptionName "<YourSubscriptionName>" 
   ```

### <a name="provision-resources"></a>Bereitstellen von Ressourcen
Wechseln Sie zum Bereitstellen der Ressourcen mit Azure PowerShell zum Ordner „C:\EventHubsQuickStart\ARM\“, und führen Sie die folgenden Befehle aus:

> [!IMPORTANT]
> Geben Sie vor dem Ausführen der Befehle einen Namen für die Azure-Ressourcengruppe als Wert für „$resourceGroupName“ an. 

```azurepowershell
$resourceGroupName = "<Specify a name for the Azure resource group>"

# Create an Azure resource group
New-AzureRmResourceGroup $resourceGroupName -location 'East US'

# Deploy the Resource Manager template. Specify the names of deployment itself, resource group, JSON file for the template, JSON file for parameters
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName $resourceGroupName -TemplateFile MyEventHub.json -TemplateParameterFile MyEventHub-Parameters.json
```

## <a name="use-azure-cli-to-deploy-the-template"></a>Bereitstellen der Vorlage über die Azure-Befehlszeilenschnittstelle

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Die folgenden Schritte sind nicht erforderlich, wenn Sie Befehle in Cloud Shell ausführen. Bei lokaler Ausführung der CLI führen Sie die folgenden Schritte aus, um sich an Azure anzumelden und Ihr aktuelles Abonnement festzulegen:

Führen Sie den folgenden Befehl aus, um sich bei Azure anzumelden:

```azurecli
az login
```

Legen Sie den aktuellen Abonnementkontext fest. Ersetzen Sie `MyAzureSub` durch den Namen des Azure-Abonnements, das Sie verwenden möchten:

```azurecli
az account set --subscription <Name of your Azure subscription>
``` 

### <a name="provision-resources"></a>Bereitstellen von Ressourcen
Wechseln Sie zum Bereitstellen der Ressourcen über die Azure-Befehlszeilenschnittstelle zum Ordner „C:\EventHubsQuickStart\ARM\“, und führen Sie die folgenden Befehle aus:

> [!IMPORTANT]
> Geben Sie im Befehl „az group create“ einen Namen für die Azure-Ressourcengruppe an. .

```azurecli
# Create an Azure resource group
az group create --name <YourResourceGroupName> --location eastus

# # Deploy the Resource Manager template. Specify the names of resource group, deployment, JSON file for the template, JSON file for parameters
az group deployment create --name <Specify a name for the deployment> --resource-group <YourResourceGroupName> --template-file MyEventHub.json --parameters @MyEventHub-Parameters.json
```

Glückwunsch! Sie haben die Azure Resource Manager-Vorlage verwendet, um einen Event Hubs-Namespace und einen darin enthaltenen Event Hub zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie den Event Hubs-Namespace erstellt und Beispielanwendungen verwendet, um Ereignisse von Ihrem Event Hub zu senden und zu empfangen. Schritt-für-Schritt-Anleitungen zum Senden von Ereignissen an oder Empfangen von Ereignissen von einem Event Hub finden Sie in den folgenden Tutorials: 

- **Senden von Ereignissen an einen Event Hub**: [.NET Standard](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [Node.js](event-hubs-node-get-started-send.md), [Go](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
- **Empfangen von Ereignissen von einem Event Hub**: [.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [Node.js](event-hubs-node-get-started-receive.md), [Go](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
