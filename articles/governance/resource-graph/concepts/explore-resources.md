---
title: Untersuchen von Azure-Ressourcen
description: Hier erfahren Sie, wie Sie mit der Abfragesprache Resource Graph Ihre Ressourcen untersuchen und herausfinden, wie diese miteinander vernetzt sind.
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 3174e74dc3fb8c56279c0c9708a67d99ae19724a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795969"
---
# <a name="explore-your-azure-resources-with-resource-graph"></a>Untersuchen Ihrer Azure-Ressourcen mit Resource Graph

Mit Azure Resource Graph können Sie Ihre Azure-Ressourcen schnell und bedarfsgerecht erkunden und ermitteln. Resource Graph wurde speziell für schnelle Reaktionen entwickelt und bietet eine gute Möglichkeit, eine Umgebung sowie die Eigenschaften kennenzulernen, die die Azure-Ressourcen definieren.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="explore-virtual-machines"></a>Erkunden von virtuellen Computern

Ein virtueller Computer ist eine typische Ressource in Azure. Als Ressourcentyp verfügen virtuelle Computer über viele Eigenschaften, die abgefragt werden können. Anhand der einzelnen Eigenschaften kann nach der jeweils gewünschten Ressource gefiltert oder gesucht werden.

### <a name="virtual-machine-discovery"></a>Ermittlung von virtuellen Computern

Beginnen wir zunächst mit einer einfachen Abfrage zum Abrufen eines virtuellen Computers in unserer Umgebung, und untersuchen wir anschließend die zurückgegebenen Eigenschaften.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | limit 1"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | limit 1" | ConvertTo-Json -Depth 100
```

> [!NOTE]
> Das Azure PowerShell-Cmdlet `Search-AzGraph` gibt standardmäßig **PSCustomObject** zurück. Damit die Ausgabe dem von der Azure-Befehlszeilenschnittstelle zurückgegebenen Ergebnis entspricht, wird das `ConvertTo-Json`-Cmdlet verwendet. Der Standardwert für **Depth** ist _2_. Bei einem Wert von _100_ sollten alle zurückgegebenen Ebenen konvertiert werden.

Die JSON-Ergebnisse sind etwa wie im folgenden Beispiel strukturiert:

```json
[
  {
    "aliases": {
      "Microsoft.Compute/imageId": null,
      "Microsoft.Compute/imageOffer": "WindowsServer",
      "Microsoft.Compute/imagePublisher": "MicrosoftWindowsServer",
      "Microsoft.Compute/imageSku": "2016-Datacenter",
      "Microsoft.Compute/imageVersion": "latest",
      "Microsoft.Compute/licenseType": null,
      "Microsoft.Compute/virtualMachines/availabilitySet.id": null,
      "Microsoft.Compute/virtualMachines/diagnosticsProfile.bootDiagnostics": null,
      "Microsoft.Compute/virtualMachines/diagnosticsProfile.bootDiagnostics.enabled": null,
      "Microsoft.Compute/virtualMachines/diagnosticsProfile.bootDiagnostics.storageUri": null,
      "Microsoft.Compute/virtualMachines/imageOffer": "WindowsServer",
      "Microsoft.Compute/virtualMachines/imagePublisher": "MicrosoftWindowsServer",
      "Microsoft.Compute/virtualMachines/imageSku": "2016-Datacenter",
      "Microsoft.Compute/virtualMachines/imageVersion": "latest",
      "Microsoft.Compute/virtualMachines/networkInterfaces[*].id": [
        "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Network/networkInterfaces/contosovm1535"
      ],
      "Microsoft.Compute/virtualMachines/osDisk.Uri": null,
      "Microsoft.Compute/virtualMachines/osProfile.adminPassword": null,
      "Microsoft.Compute/virtualMachines/osProfile.adminUsername": "localAdmin",
      "Microsoft.Compute/virtualMachines/osProfile.linuxConfiguration": null,
      "Microsoft.Compute/virtualMachines/osProfile.linuxConfiguration.disablePasswordAuthentication": null,
      "Microsoft.Compute/virtualMachines/osProfile.windowsConfiguration": {
        "enableAutomaticUpdates": true,
        "provisionVMAgent": true
      },
      "Microsoft.Compute/virtualMachines/osProfile.windowsConfiguration.enableAutomaticUpdates": true,
      "Microsoft.Compute/virtualMachines/osProfile.windowsConfiguration.provisionVMAgent": true,
      "Microsoft.Compute/virtualMachines/sku.name": "Standard_B2s",
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].caching": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].createOption": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].diskSizeGB": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].image.uri": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].lun": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].managedDisk.id": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].managedDisk.storageAccountType": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].name": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].vhd.uri": [],
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.caching": "ReadWrite",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.createOption": "FromImage",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.encryptionSettings": null,
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.encryptionSettings.enabled": null,
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.id": "/subscriptions/<subscriptionId>/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType": "Premium_LRS",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.osType": "Windows",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.vhd": null,
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.vhd.uri": null
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1",
    "plan": {},
    "properties": {
      "hardwareProfile": {
        "vmSize": "Standard_B2s"
      },
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "/subscriptions/<subscriptionId>/MyResourceGroup/providers/Microsoft.Network/networkInterfaces/contosovm1535",
            "resourceGroup": "MyResourceGroup"
          }
        ]
      },
      "osProfile": {
        "adminUsername": "localAdmin",
        "computerName": "ContosoVM1",
        "secrets": [],
        "windowsConfiguration": {
          "enableAutomaticUpdates": true,
          "provisionVMAgent": true
        }
      },
      "provisioningState": "Succeeded",
      "storageProfile": {
        "dataDisks": [],
        "imageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2016-Datacenter",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": 127,
          "managedDisk": {
            "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
            "resourceGroup": "MyResourceGroup",
            "storageAccountType": "Premium_LRS"
          },
          "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
          "osType": "Windows"
        }
      },
      "vmId": "bbb9b451-6dc7-4117-bec5-c971eb1118c6"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {},
    "subscriptionId": "<subscriptionId>",
    "tags": {},
    "type": "microsoft.compute/virtualmachines"
  }
]
```

Die ersten Eigenschaften unter **aliases** enthalten eine Reihe von verwandten Eigenschaftswerten. Weitere Informationen zu Aliasen sowie dazu, welche Aliase verfügbar sind, finden Sie unter [Azure Policy definition structure - Aliases](../../policy/concepts/definition-structure.md#aliases) (Struktur von Azure Policy-Definitionen – Azure). Aliase werden in erster Linie von Azure Policy zum Verwalten der Compliance mit Organisationsregeln und Governance verwendet.

Die anderen Eigenschaften liefern uns zusätzliche Informationen zur VM-Ressource selbst sowie zu SKU, BS, Datenträger, Tags und zur Ressourcengruppe sowie zum Abonnement, der bzw. dem die Ressource angehört.

### <a name="virtual-machines-by-location"></a>Virtuelle Computer nach Standort

Ausgehend von dem, was wir über die VM-Ressource erfahren haben, zählen wir anhand der Eigenschaft **location** alle virtuellen Computer nach Standort. Wir aktualisieren die Abfrage, indem wir den Grenzwert entfernen und die Anzahl der Speicherortwerte summieren.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by location
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

Die JSON-Ergebnisse sind etwa wie im folgenden Beispiel strukturiert:

```json
[
  {
    "count_": 386,
    "location": "eastus"
  },
  {
    "count_": 215,
    "location": "southcentralus"
  },
  {
    "count_": 59,
    "location": "westus"
  }
]
```

Wir sehen nun, wie viele virtuelle Computer sich in den einzelnen Azure-Regionen befinden.

### <a name="virtual-machines-by-sku"></a>Virtuelle Computer nach SKU

Kehren wir zu den ursprünglichen Eigenschaften des virtuellen Computers zurück und suchen nun alle virtuellen Computer mit der SKU-Größe **Standard_B2s**. Anhand des JSON-Ergebnisse können wir erkennen, dass dieser Wert in **properties.hardwareprofile.vmsize** gespeichert ist. Also aktualisieren wir die Abfrage so, dass alle virtuellen Computer mit dieser Größe gesucht und nur die Namen der virtuellen Computer und die jeweiligen Regionen zurückgegeben werden.

```Query
where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s
| project name, resourceGroup"
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

### <a name="virtual-machines-connected-to-premium-managed-disks"></a>Mit Managed Disks Premium vernetzte virtuelle Computer

Wenn wir die Details von Managed Disks Premium abrufen möchten, die diesen virtuellen **Standard_B2s**-Computern zugeordnet sind, können wir die Abfrage erweitern, sodass die Ressourcen-IDs dieser verwalteten Datenträger zurückgegeben werden.

```Query
where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s'
| extend disk = properties.storageProfile.osDisk.managedDisk
| where disk.storageAccountType == 'Premium_LRS'
| project disk.id
```

> [!NOTE]
> Alternativ könnten wir die SKU auch mithilfe der **aliases**-Eigenschaft **Microsoft.Compute/virtualMachines/sku.name** abrufen.

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

```azurepowershell-interactive
  Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

Das Ergebnis ist eine Liste mit Datenträger-IDs.

### <a name="managed-disk-discovery"></a>Ermittlung von verwalteten Datenträgern

Mit dem ersten Datensatz aus der vorherigen Abfrage untersuchen wir die Eigenschaften für den verwalteten Datenträger, der dem ersten virtuellen Computer zugeordnet wurde. Für die aktualisierte Abfrage wird die Datenträger-ID verwendet und der Typ geändert.

Beispielausgabe aus der vorherigen Abfrage:

```json
[
  {
    "disk_id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166"
  }
]
```

```Query
where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'
```

Woher wussten wir vor dem Ausführen der Abfrage, dass für **type** jetzt **Microsoft.Compute/disks** festgelegt werden muss?
Wenn Sie sich die vollständige ID ansehen, erkennen Sie, dass **/providers/Microsoft.Compute/disks/** Teil der Zeichenfolge ist. Dieses Zeichenfolgenfragment ist ein Hinweis darauf, nach welchem Typ gesucht werden muss. Alternativ kann auch der Grenzwert nach Typ entfernt und stattdessen nur nach dem ID-Feld gesucht werden. Da die ID eindeutig ist, wird nur ein Datensatz zurückgegeben, und dessen **type**-Eigenschaft enthält diese Information.

> [!NOTE]
> Damit dieses Beispiel funktioniert, müssen Sie das ID-Feld durch ein Ergebnis aus Ihrer Umgebung ersetzen.

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

Die JSON-Ergebnisse sind etwa wie im folgenden Beispiel strukturiert:

```json
[
  {
    "aliases": {
      "Microsoft.Compute/disks/sku.name": "Premium_LRS",
      "Microsoft.Compute/imageId": null,
      "Microsoft.Compute/imageOffer": null,
      "Microsoft.Compute/imagePublisher": null,
      "Microsoft.Compute/imageSku": null,
      "Microsoft.Compute/imageVersion": null
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "plan": {},
    "properties": {
      "creationData": {
        "createOption": "Empty"
      },
      "diskSizeGB": 127,
      "diskState": "ActiveSAS",
      "provisioningState": "Succeeded",
      "timeCreated": "2018-09-14T12:17:32.2570000Z"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {
      "name": "Premium_LRS",
      "tier": "Premium"
    },
    "subscriptionId": "<subscriptionId>",
    "tags": {
      "environment": "prod"
    },
    "type": "microsoft.compute/disks"
  }
]
```

## <a name="explore-virtual-machines-to-find-public-ip-addresses"></a>Erkunden von virtuellen Computern zum Suchen nach öffentlichen IP-Adressen

Mit diesen Azure CLI-Abfragen werden zuerst alle Netzwerkschnittstellenressourcen gefunden und gespeichert, die mit virtuellen Computern verbunden sind. Anschließend wird die Liste mit den Netzwerkschnittstellen genutzt, um nach den einzelnen IP-Adressressourcen zu suchen, bei denen es sich um eine öffentliche IP-Adresse handelt. Diese Werte werden dann gespeichert. Abschließend wird eine Liste mit den öffentlichen IP-Adressen bereitgestellt.

```azurecli-interactive
# Use Resource Graph to get all NICs and store in the 'nic' variable
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project nic = tostring(properties['networkProfile']['networkInterfaces'][0]['id']) | where isnotempty(nic) | distinct nic | limit 20" --output table | tail -n +3 > nics.txt

# Review the output of the query stored in 'nics.txt'
cat nics.txt
```

Verwenden Sie die Datei `nics.txt` in der nächsten Abfrage zum Abrufen der Details der entsprechenden Netzwerkschnittstellenressourcen, denen eine öffentliche IP-Adresse zugeordnet ist.

```azurecli-interactive
# Use Resource Graph with the 'nics.txt' file to get all related public IP addresses and store in 'publicIp.txt' file
az graph query -q="where type =~ 'Microsoft.Network/networkInterfaces' | where id in ('$(awk -vORS="','" '{print $0}' nics.txt | sed 's/,$//')') | project publicIp = tostring(properties['ipConfigurations'][0]['properties']['publicIPAddress']['id']) | where isnotempty(publicIp) | distinct publicIp" --output table | tail -n +3 > ips.txt

# Review the output of the query stored in 'ips.txt'
cat ips.txt
```

Und zuletzt verwenden wir die in `ips.txt` gespeicherte Liste der öffentlichen IP-Adressressourcen zum Abrufen der tatsächlichen öffentlichen IP-Adresse und zeigen diese an.

```azurecli-interactive
# Use Resource Graph with the 'ips.txt' file to get the IP address of the public IP address resources
az graph query -q="where type =~ 'Microsoft.Network/publicIPAddresses' | where id in ('$(awk -vORS="','" '{print $0}' ips.txt | sed 's/,$//')') | project ip = tostring(properties['ipAddress']) | where isnotempty(ip) | distinct ip" --output table
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Abfragesprache](query-language.md)
- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).