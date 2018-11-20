---
title: Verwalten von Datenbankkonten in Azure Cosmos DB
description: Verwalten von Datenbankkonten in Azure Cosmos DB
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 0683516d16bf1501eee83901c5171811b8c0e44d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621546"
---
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>Verwalten von Datenbankkonten in Azure Cosmos DB

In diesem Artikel erfahren Sie, wie Sie Ihr Azure Cosmos DB-Konto verwalten, um Multi-Homing einzurichten, eine Region hinzuzufügen/zu entfernen, mehrere Schreibregionen zu konfigurieren und Failoverprioritäten einzurichten. 

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

### <a id="create-database-account-via-portal"></a>Azure-Portal

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure-Befehlszeilenschnittstelle

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>Konfigurieren von Clients für Multihoming

### <a id="configure-clients-multi-homing-dotnet"></a>.NET SDK

```csharp
// Create a new Connection Policy
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to Preferred locations
// The name of the location will match what you see in the portal/etc.
policy.PreferredLocations.Add("East US");
policy.PreferredLocations.Add("North Europe");

// Pass the Connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-java-async"></a>Java Async SDK

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setPreferredLocations(Collections.singleton("West US"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-java-sync"></a>Java Sync SDK

```java
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
Collection<String> preferredLocations = new ArrayList<String>();
preferredLocations.add("Australia East");
connectionPolicy.setPreferredLocations(preferredLocations);
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy);
```

### <a id="configure-clients-multi-homing-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Set up the connection policy with your preferred regions
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["West US", "Australia East"];

// Pass that connection policy to the client
const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy
});
```

### <a id="configure-clients-multi-homing-python"></a>Python SDK

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.PreferredLocations = ['West US', 'Japan West']
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy)

```

## <a name="addremove-regions-from-your-database-account"></a>Hinzufügen/Entfernen von Regionen für Ihr Datenbankkonto

### <a id="add-remove-regions-via-portal"></a>Azure-Portal

1. Navigieren Sie zu Ihrem Azure Cosmos DB-Konto, und öffnen Sie das Menü **Daten global replizieren**.

2. Wenn Sie Regionen hinzufügen möchten, wählen Sie auf der Karte die gewünschten Regionen aus, indem Sie auf die leeren Sechsecke mit der Beschriftung **+** klicken. Alternativ können Sie zum Hinzufügen einer Region auch die Option **+ Region hinzufügen** und anschließend eine Region aus dem Dropdownmenü auswählen.

3. Wenn Sie Regionen entfernen möchten, heben Sie die Auswahl der gewünschten Regionen auf der Karte auf, indem Sie auf die blauen, mit einem Häkchen gekennzeichneten Sechsecke klicken, oder klicken Sie rechts neben der Region auf das Papierkorbsymbol (🗑).

4. Klicken Sie auf „Speichern“, um die Änderungen zu speichern.

   ![Menü „Regionen hinzufügen/entfernen“](./media/how-to-manage-database-account/add-region.png)

Im Schreibmodus mit einer einzelnen Region kann die Schreibregion nicht entfernt werden. In diesem Fall muss erst ein Failover auf eine andere Region durchgeführt werden, um die aktuelle Schreibregion löschen zu können.

Im Schreibmodus mit mehreren Regionen können Sie beliebige Regionen hinzufügen/entfernen, solange noch mindestens eine Region vorhanden ist.

### <a id="add-remove-regions-via-cli"></a>Azure-Befehlszeilenschnittstelle

```bash
# Given an account created with 1 region like so
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0'

# Add a new region by adding another region to the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0 westus=1'

# Remove a region by removing a region from the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'westus=0'
```

## <a name="configure-multiple-write-regions"></a>Konfigurieren mehrerer Schreibregionen

### <a id="configure-multiple-write-regions-portal"></a>Azure-Portal

Achten Sie beim Erstellen eines Datenbankkontos darauf, dass die Einstellung **Multi-region Writes** (Schreibvorgänge in mehreren Regionen) aktiviert ist.

![Screenshot: Azure Cosmos-Kontoerstellung](./media/how-to-manage-database-account/account-create.png)

### <a id="configure-multiple-write-regions-cli"></a>Azure-Befehlszeilenschnittstelle

```bash
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-arm"></a>Resource Manager-Vorlage

Der folgende JSON-Code ist ein Beispiel für eine Resource Manager-Vorlage. Sie können damit ein Azure Cosmos-Konto mit der Konsistenzrichtlinie „Begrenzte Veraltung“, einem maximalen Veraltungsintervall von fünf Sekunden und einer maximalen Anzahl von 100 tolerierten veralteten Anforderungen bereitstellen. Weitere Informationen zum Resource Manager-Vorlagenformat sowie zur Syntax finden Sie in der [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)-Dokumentation.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "BoundedStaleness",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
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


## <a id="manual-failover"></a>Aktivieren des manuellen Failovers für Ihr Azure Cosmos-Konto

### <a id="enable-manual-failover-via-portal"></a>Azure-Portal

1. Navigieren Sie zu Ihrem Azure Cosmos-Konto, und öffnen Sie das Menü **Daten global replizieren**.

2. Klicken Sie oben im Menü auf die Schaltfläche **Manuelles Failover**.

   ![Menü „Daten global replizieren“](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Wählen Sie im Menü **Manuelles Failover** Ihre neue Schreibregion aus, und bestätigen Sie durch Aktivieren des Kontrollkästchens, dass Sie verstehen, dass sich durch diese Option Ihre Schreibregion ändert.

4. Klicken Sie auf „OK“, um das Failover auszulösen.

   ![Portalmenü für manuelles Failover](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure-Befehlszeilenschnittstelle

```bash
# Given your account currently has regions with priority like so: 'eastus=0 westus=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=1 westus=0'
```

## <a id="automatic-failover"></a>Aktivieren des automatischen Failovers für Ihr Azure Cosmos-Konto

### <a id="enable-automatic-failover-via-portal"></a>Azure-Portal

1. Öffnen Sie in Ihrem Azure Cosmos-Konto den Bereich **Daten global replizieren**. 

2. Klicken Sie oben im Bereich auf die Schaltfläche **Automatisches Failover**.

   ![Menü „Daten global replizieren“](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Vergewissern Sie sich im Bereich **Automatisches Failover**, dass **Automatisches Failover aktivieren** auf **EIN** festgelegt ist. 

4. Klicken Sie im unteren Bereich des Menüs auf „Speichern“.

   ![Portalmenü für automatisches Failover](./media/how-to-manage-database-account/automatic-failover.png)

In diesem Menü können Sie auch Ihre Failoverprioritäten festlegen.

### <a id="enable-automatic-failover-via-cli"></a>Azure-Befehlszeilenschnittstelle

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>Festlegen von Failoverprioritäten für Ihr Azure Cosmos-Konto

### <a id="set-failover-priorities-via-portal"></a>Azure-Portal

1. Öffnen Sie in Ihrem Azure Cosmos-Konto den Bereich **Daten global replizieren**. 

2. Klicken Sie oben im Bereich auf die Schaltfläche **Automatisches Failover**.

   ![Menü „Daten global replizieren“](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Vergewissern Sie sich im Bereich **Automatisches Failover**, dass **Automatisches Failover aktivieren** auf **EIN** festgelegt ist. 

4. Sie können die Failoverpriorität ändern, indem Sie auf die Leseregionen klicken und sie mithilfe der drei Punkte ziehen, die auf der linken Seite der Zeile angezeigt werden, wenn Sie darauf zeigen. 

5. Klicken Sie im unteren Bereich des Menüs auf „Speichern“.

   ![Portalmenü für automatisches Failover](./media/how-to-manage-database-account/automatic-failover.png)

Die Leseregion kann in diesem Menü nicht geändert werden. Sie müssen ein manuelles Failover durchführen, um die Schreibregion manuell zu ändern.

### <a id="set-failover-priorities-via-cli"></a>Azure-Befehlszeilenschnittstelle

```bash
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies 'eastus=0 westus=2 southcentralus=1'
```

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Dokumente enthalten Informationen zur Behandlung von Konsistenzebenen und Datenkonflikten in Azure Cosmos DB:

* [Manage consistency](how-to-manage-consistency.md) (Verwalten der Konsistenz)
* [Behandeln von Konflikten zwischen Regionen](how-to-manage-conflicts.md)

