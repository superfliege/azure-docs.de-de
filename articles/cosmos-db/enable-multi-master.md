---
title: Aktivieren von Multimaster für Azure Cosmos DB-Konten
description: Dieser Artikel beschreibt das Aktivieren von Multimasterunterstützung beim Erstellen eines Azure Cosmos DB-Kontos mit dem Azure-Portal, PowerShell, der CLI oder einer Azure Resource Manager-Vorlage.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 71579e28f389d91498ef5f37c5d43dc9d5140234
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963218"
---
# <a name="enable-multi-master-for-azure-cosmos-db-accounts"></a>Aktivieren von Multimaster für Azure Cosmos DB-Konten

Multimasterunterstützung wird beim Erstellen eines Azure Cosmos DB-Kontos aktiviert. Ein Azure Cosmos DB-Konto kann mit dem Azure-Portal, PowerShell, der CLI oder einer Azure Resource Manager-Vorlage erstellt werden.

> [!IMPORTANT]
> Derzeit kann Multimasterunterstützung nur für neue Azure Cosmos DB-Konten aktiviert werden. Für vorhandene Azure Cosmos DB-Konten ist das Feature nicht verfügbar. Wir arbeiten daran, Unterstützung für vorhandene Konten bereitzustellen, und werden die Verfügbarkeit dieser Unterstützung bekanntgeben.

Nach dem Erstellen eines Azure Cosmos DB-Kontos mit Multimasterunterstützung können Sie Datenbanken und Container erstellen, Dokumente hochladen und Richtlinien zur Konfliktlösung erstellen. Informationen zur Konfliktlösung in Multimaster sowie Codebeispiele finden Sie im Artikel zu [Multimaster-Codebeispielen](multi-master-conflict-resolution.md#code-samples).

## <a name="enable-multi-master-using-azure-portal"></a>Aktivieren von Multimaster mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)

2. Wählen Sie **Ressource erstellen > Datenbanken > Azure Cosmos DB**.

3. Geben Sie auf der Seite **Neues Konto** die folgenden Einstellungen für ein neues Azure Cosmos DB-Konto ein:

   |**Einstellung**  |**Empfohlener Wert** |**Beschreibung**|
   |---------|---------|---------|
   |Abonnement   | {Ihr Abonnement}  |Wählen Sie das für dieses Azure Cosmos DB-Konto zu verwendende Azure-Abonnement aus.  |
   |Ressourcengruppe  |   {Name Ihrer Ressourcengruppe}    |  Wählen Sie eine vorhandene Ressourcengruppe oder die Option  **Neu erstellen** aus, und geben Sie dann einen neuen Ressourcengruppennamen für Ihr Konto ein. |
   |Kontoname | {Name Ihres Kontos}   |  Geben Sie einen eindeutigen Namen ein, der Ihr Azure Cosmos DB-Konto identifiziert.        |
   |API  |   Beliebig   |  Wählen Sie einen API-Typ aus.   |
   |Standort  | Beliebige Region auswählen   | Wählen Sie den geografischen Standort, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll, aus. Sie können eine beliebige Region auswählen, da sich dieses Konto in mehreren Regionen befinden wird.  |
   |Georedundanz aktivieren   |  Aktivieren  |  Wählen Sie diese Einstellung, um die unten ausgewählte Multimaster-Instanz zu aktivieren.   |
   |Multimaster aktivieren | Aktivieren  | Wählen Sie diese Einstellung zum Aktivieren von Multimaster für dieses Konto. |


## <a name="using-multi-master-in-sdks"></a>Verwenden von Multimaster in SDKs

Mit einem Multimaster-fähigen Konto können Sie innerhalb Ihrer Anwendungen Multimasterfunktionen nutzen, indem Sie wie unten gezeigt „ConnectionPolicy“ verwenden.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
{
   ConnectionMode = ConnectionMode.Direct,
   ConnectionProtocol = Protocol.Tcp,
   UseMultipleWriteLocations = true,
};
policy.PreferredLocations.Add("West US");
policy.PreferredLocations.Add("North Europe");
policy.PreferredLocations.Add("Southeast Asia");
```

## <a name="enable-multi-master-using-powershell"></a>Aktivieren von Multimaster mithilfe von PowerShell

Sie können auch ein Multimaster-fähiges Cosmos DB-Konto erstellen, indem Sie den `enableMultipleWriteLocations` Parameter auf „true“ festlegen. Zum Erstellen eines Azure Cosmos DB-Konto mit Multimaster-Aktivierung öffnen Sie ein PowerShell-Fenster und führen das folgende Skript aus:

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = "<ip-range-filter>"

$consistencyPolicy = @{"defaultConsistencyLevel"="Session";
                       "maxIntervalInSeconds"= "10";
                       "maxStalenessPrefix"="200"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

## <a name="enable-multi-master-using-cli"></a>Aktivieren von Multimaster mithilfe der CLI

Sie können Multimaster aktivieren, indem Sie den Parameter „enable-multiple-write-locations“ auf „true“ festlegen. Zum Erstellen eines Azure Cosmos DB-Konto mit Multimaster-Aktivierung öffnen Sie die Azure CLI oder Cloud Shell und führen den folgenden Befehl aus:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --default-consistency-level "Session" \
   --enable-automatic-failover "true" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="enable-multi-master-using-resource-manager-template"></a>Aktivieren von Multimaster mithilfe einer Resource Manager-Vorlage

Der folgende JSON-Code ist eine Resource Manager-Beispielvorlage, die Sie zum Bereitstellen eines Azure Cosmos DB-Kontos verwenden können. Weitere Informationen zum Resource Manager-Vorlagenformat sowie zur Syntax finden Sie in der [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)-Dokumentation. In dieser Vorlage ist der folgende Schlüsselparameter zu beachten: „enableMultipleWriteLocations“: true.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Multimasterunterstützung für Azure Cosmos DB-Konten aktiviert wird. Sehen Sie sich als Nächstes folgende Ressourcen an:

* [Verwenden von Multimaster mit Open Source-NoSQL-Datenbanken](multi-master-oss-nosql.md)

* [Grundlegendes zur Konfliktauflösung in Azure Cosmos DB](multi-master-conflict-resolution.md)
