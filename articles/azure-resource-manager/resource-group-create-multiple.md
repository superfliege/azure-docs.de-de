---
title: Bereitstellen mehrerer Instanzen von Azure-Ressourcen | Microsoft-Dokumentation
description: Verwenden Sie den copy-Vorgang und Arrays in einer Azure-Ressourcen-Manager-Vorlage, um sie beim Bereitstellen von Ressourcen mehrere Male zu durchlaufen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2017
ms.author: tomfitz
ms.openlocfilehash: e19833cb58f37f5f8b83d5558d74255583137684
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Bereitstellen mehrerer Instanzen einer Ressource oder Eigenschaft in Azure Resource Manager-Vorlagen
In diesem Artikel erfahren Sie, wie Sie eine Ressource bedingt bereitstellen und wie Sie die Azure Resource Manager-Vorlage durchlaufen, um mehrere Instanzen einer Ressource zu erstellen.

## <a name="conditionally-deploy-resource"></a>Bedingtes Bereitstellen von Ressourcen

Wenn Sie sich während der Bereitstellung entscheiden müssen, eine oder keine der Instanzen einer Ressource zu erstellen, verwenden Sie das `condition`-Element. Der Wert für dieses Element wird mit „true“ oder „false“ aufgelöst. Wenn der Wert „true“ ist, wird die Ressource bereitgestellt. Ist der Wert „false“, wird die Ressource nicht bereitgestellt. Verwenden Sie beispielsweise Folgendes, um anzugeben, ob ein neues Speicherkonto bereitgestellt wird oder ob ein vorhandenes Speicherkonto verwendet wird:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

## <a name="resource-iteration"></a>Ressourceniteration
Wenn Sie sich während der Bereitstellung entscheiden müssen, eine oder mehrere Instanzen einer Ressource zu erstellen, fügen Sie dem Ressourcentyp ein `copy`-Element hinzu. Im copy-Element geben Sie die Anzahl von Iterationen und einen Namen für diese Schleife an. Der count-Wert muss eine positive ganze Zahl sein und darf 800 nicht überschreiten. 

Die Ressource zum mehrfachen Erstellen übernimmt das folgende Format:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

Beachten Sie, dass der Name der einzelnen Ressourcen die `copyIndex()`-Funktion enthält, die die aktuelle Iteration in der Schleife zurückgibt. `copyIndex()` ist nullbasiert. Im folgenden Beispiel werden die unten aufgeführten Namen erstellt:

```json
"name": "[concat('storage', copyIndex())]",
```

Namen:

* storage0
* storage1
* storage2

Zum Versetzen des Indexwerts können Sie einen Wert in der copyIndex()-Funktion übergeben. Die Anzahl von durchzuführenden Durchläufen wird weiterhin im copy-Element angegeben, aber der Wert von copyIndex wird um den angegebenen Wert versetzt. Im folgenden Beispiel werden die unten aufgeführten Namen erstellt:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Namen:

* storage1
* storage2
* storage3

Der „copy“-Vorgang ist besonders bei Verwendung von Arrays hilfreich, weil Sie jedes Element im Array durchlaufen können. Verwenden Sie die Funktion `length` für das Array, um die Anzahl von Iterationen anzugeben, und `copyIndex`, um den aktuellen Index im Array abzurufen. Im folgenden Beispiel werden die unten aufgeführten Namen erstellt:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

Namen:

* storagecontoso
* storagefabrikam
* storagecoho

Resource Manager erstellt die Ressourcen standardmäßig gleichzeitig. Aus diesem Grund ist die Reihenfolge, in der sie erstellt werden, nicht garantiert. Es ist aber möglicherweise sinnvoll, anzugeben, dass die Ressource sequenziell bereitgestellt werden. Wenn Sie z.B. eine Produktionsumgebung aktualisieren, möchten Sie die Updates möglicherweise staffeln, sodass nur eine bestimmte Anzahl von Ressourcen gleichzeitig aktualisiert wird.

Legen Sie zum seriellen Bereitstellen mehrerer Instanzen einer Ressource `mode` auf **serial** und `batchSize` auf die Anzahl der Instanzen fest, die zu einem Zeitpunkt bereitgestellt werden sollen. Im seriellen Modus erstellt Resource Manager eine Abhängigkeit von früheren Instanzen in der Schleife, sodass ein Batch erst dann gestartet wird, wenn das vorherige Batch abgeschlossen wurde.

Zum seriellen Bereitstellen von zwei Speicherkonten gleichzeitig verwenden Sie beispielsweise:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            }
        }
    ],
    "outputs": {}
}
``` 

Die mode-Eigenschaft akzeptiert auch **parallel**, wobei es sich um den Standardwert handelt.

## <a name="property-iteration"></a>Iteration von Eigenschaften

Wenn Sie mehrere Werte für eine Eigenschaft einer Ressource erstellen möchten, fügen Sie im properties-Element ein `copy`-Array hinzu. Dieses Array enthält Objekte, und jedes Objekt weist die folgenden Eigenschaften auf:

* „name“ – der Name der Eigenschaft, für die mehrere Werte erstellt werden sollen
* „count“ – die Anzahl der zu erstellenden Werte
* „input“ – ein Objekt, das die Werte enthält, die der Eigenschaft zugewiesen werden sollen  

Im folgenden Beispiel wird veranschaulicht, wie `copy` auf die dataDisks-Eigenschaft auf einem virtuellen Computer angewendet wird:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Beachten Sie Folgendes: Bei Verwendung von `copyIndex` in einer Eigenschaften-Iteration müssen Sie den Namen der Iteration angeben. Bei Verwendung mit einer Ressourcen-Iteration muss der Name nicht angegeben werden.

Ressourcen-Manager erweitert das `copy`-Array während der Bereitstellung. Der Name des Arrays wird zum Namen der Eigenschaft. Die Eingabewerte werden zu den Eigenschaften des Objekts. Die bereitgestellte Vorlage sieht wie folgt aus:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Sie können die Ressourcen- und die Eigenschaften-Iteration zusammen verwenden. Verweisen Sie anhand des Namens auf die Eigenschaften-Iteration.

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

## <a name="variable-iteration"></a>Variableniteration

Um mehrere Instanzen einer Variable zu erstellen, verwenden Sie das `copy`-Element im Abschnitt „variables“. Sie können mehrere Instanzen von Objekten mit den verknüpften Werten erstellen und anschließend diese Werte Instanzen der Ressource zuweisen. Sie können copy zum Erstellen eines Objekts mit einer array-Eigenschaft oder einem Array verwenden. Im folgenden Beispiel sind beide Ansätze dargestellt:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        }
      ]
    },
    "copy": [
      {
        "name": "disks-top-level-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('disks-top-level-array')]"
        }
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleArray": {
      "value": "[variables('disks-top-level-array')]",
      "type" : "array"
    }
  }
}
```

## <a name="depend-on-resources-in-a-loop"></a>Abhängigkeit von Ressourcen in einer Schleife
Sie geben an, dass eine Ressource nach einer anderen Ressource bereitgestellt wird, indem Sie das `dependsOn`-Element verwenden. Um eine Ressource bereitzustellen, die von der Sammlung von Ressourcen in einer Schleife abhängt, geben Sie den Namen der Kopierschleife im dependsOn-Element an. Das folgende Beispiel zeigt, wie drei Speicherkonten vor dem Bereitstellen des virtuellen Computers bereitgestellt werden. Die vollständige Definition des virtuellen Computers ist dabei nicht angegeben. Beachten Sie, dass „name“ für das copy-Element auf `storagecopy` und auch das dependsOn-Element für die virtuellen Computer auf `storagecopy` festgelegt ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="iteration-for-a-child-resource"></a>Iteration für eine untergeordnete Ressource
Für eine untergeordnete Ressource kann keine Kopierschleife verwendet werden. Um mehrere Instanzen einer Ressource zu erstellen, die Sie in der Regel als innerhalb einer anderen Ressource geschachtelt definieren, müssen Sie diese Ressource stattdessen als Ressource oberster Ebene erstellen. Sie definieren die Beziehung zur übergeordneten Ressource mithilfe der Eigenschaften „type“ und „name“.

Angenommen, Sie definieren ein Dataset als untergeordnete Ressource innerhalb einer Data Factory.

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

Um mehrere Instanzen von Datasets zu erstellen, verschieben Sie sie außerhalb der Data Factory. Das Dataset muss sich auf der gleichen Ebene wie die Data Factory befinden, ist aber immer noch eine untergeordnete Ressource der Data Factory. Sie erhalten die Beziehung zwischen Dataset und Data Factory mithilfe der Eigenschaften „type“ und „name“ bei. Da „type“ nicht mehr von seiner Position in der Vorlage abgeleitet werden kann, müssen Sie den vollqualifizierten Typ im folgenden Format angeben: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Um eine Über-/Unterordnungsbeziehung mit einer Instanz der Data Factory herzustellen, geben Sie einen Namen für das Dataset an, das den Namen der übergeordneten Ressource enthält. Verwenden Sie das folgende Format: `{parent-resource-name}/{child-resource-name}`.  

Das folgende Beispiel zeigt die Implementierung:

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="example-templates"></a>Beispielvorlagen

Die folgenden Beispiele zeigen allgemeine Szenarien für das Erstellen mehrerer Ressourcen oder Eigenschaften.

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
|[Speicher kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Stellt mehrere Speicherkonten mit einer Indexnummer im Namen bereit |
|[Speicher seriell kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Stellt mehrere Speicherkonten nacheinander bereit. Der Name enthält die Indexnummer. |
|[Speicher mit Array kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Stellt mehrere Speicherkonten bereit. Der Name enthält einen Wert aus einem Array. |
|[VM mit neuem oder vorhandenem VNET, Speicher und öffentlicher IP-Adresse](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) |Stellt bedingt neue oder vorhandene Ressourcen mit einem virtuellen Computer bereit |
|[VM-Bereitstellung mit einer variablen Anzahl von Datenträgern](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Stellt mehrere Datenträger mit einem virtuellen Computer bereit |
|[Variablen kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Veranschaulicht die verschiedenen Methoden zum Durchlaufen von Variablen |
|[Mehrere Sicherheitsregeln](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Stellt mehrere Sicherheitsregeln in einer Netzwerksicherheitsgruppe bereit. Die Sicherheitsregeln werden aus einem Parameter generiert. |

## <a name="next-steps"></a>Nächste Schritte
* Informationen zu den Abschnitten einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).

