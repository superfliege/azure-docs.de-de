---
title: Variablen von Azure Resource Manager-Vorlagen | Microsoft Docs
description: Beschreibt, wie Variablen in Azure Resource Manager-Vorlagen mithilfe deklarativer JSON-Syntax definiert werden.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: tomfitz
ms.openlocfilehash: 08728a3c0b4d4578939004e2d1b1ee2d30a682ab
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="variables-section-of-azure-resource-manager-templates"></a>Abschnitt „Variables“ von Azure Resource Manager-Vorlagen
Im Abschnitt „variables“ erstellen Sie Werte, die in der ganzen Vorlage verwendet werden können. Sie müssen nicht unbedingt variables definieren, aber häufig bewirken sie eine Vereinfachung Ihrer Vorlage, indem komplexe Ausdrücke reduziert werden.

## <a name="define-and-use-a-variable"></a>Definieren und Verwenden einer Variablen

Im folgenden Beispiel wird eine Variablendefinition gezeigt. Dabei wird ein Zeichenfolgenwert für einen Speicherkontonamen erstellt. Es werden mehrere Vorlagenfunktionen verwendet, um einen Parameterwert abzurufen und zu einer eindeutigen Zeichenfolge zu verketten.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Sie verwenden die Variable, wenn Sie die Ressource definieren.

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    ...
```

## <a name="available-definitions"></a>Verfügbare Definitionen

Im vorherige Beispiel wurde eine Möglichkeit zum Definieren einer Variablen gezeigt. Sie können auch eine der folgenden Definitionen verwenden:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    },
    "<variable-object-name>": {
        "copy": [
            {
                "name": "<name-of-array-property>",
                "count": <number-of-iterations>,
                "input": {
                    <properties-to-repeat>
                }
            }
        ]
    },
    "copy": [
        {
            "name": "<variable-array-name>",
            "count": <number-of-iterations>,
            "input": {
                <properties-to-repeat>
            }
        }
    ]
}
```

## <a name="configuration-variables"></a>Konfigurationsvariablen

Sie können komplexe JSON-Typen verwenden, um verwandte Werte für eine Umgebung zu definieren. 

```json
"variables": {
    "environmentSettings": {
        "test": {
            "instanceSize": "Small",
            "instanceCount": 1
        },
        "prod": {
            "instanceSize": "Large",
            "instanceCount": 4
        }
    }
},
```

Unter „Parameters“ erstellen Sie einen Wert, der angibt, welche Konfigurationswerte verwendet werden sollen.

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
```

Sie rufen die aktuellen Einstellungen folgendermaßen ab:

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="use-copy-element-in-variable-definition"></a>Verwenden von „copy element“ in der Variablendefinition

Sie können die **copy**-Syntax zum Erstellen einer Variablen mit einem Array mehrerer Elemente verwenden. Sie geben eine Zahl für die Anzahl von Elementen an. Jedes Element enthält die Eigenschaften im **input**-Objekt. Sie können „copy“ entweder innerhalb einer Variablen oder zum Erstellen der Variablen verwenden. Wenn Sie eine Variable definieren und **copy** in dieser Variablen verwenden, erstellen Sie ein Objekt, das eine Array-Eigenschaft besitzt. Wenn Sie **copy** auf der obersten Ebene verwenden und darin mindestens eine Variable definieren, erstellen Sie mindestens ein Array. Im folgenden Beispiel sind beide Ansätze dargestellt:

```json
"variables": {
    "disk-array-on-object": {
        "copy": [
            {
                "name": "disks",
                "count": 3,
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
            "count": 3,
            "input": {
                "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('disks-top-level-array')]"
            }
        }
    ]
},
```

Die Variable **disk-array-on-object** enthält das folgende Objekt mit einem Array namens **disks**:

```json
{
  "disks": [
    {
      "name": "myDataDisk1",
      "diskSizeGB": "1",
      "diskIndex": 0
    },
    {
      "name": "myDataDisk2",
      "diskSizeGB": "1",
      "diskIndex": 1
    },
    {
      "name": "myDataDisk3",
      "diskSizeGB": "1",
      "diskIndex": 2
    }
  ]
}
```

Die Variable **disks-top-level-array** enthält das folgende Array:

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  }
]
```

Sie können auch mehrere Objekte angeben, wenn Sie Variablen durch Kopieren erstellen. Im folgenden Beispiel werden zwei Arrays als Variablen definiert. Ein Array namens **disks-top-level-array** mit fünf Elementen. Ein anderes Array namens **a-different-array** mit drei Elementen.

```json
"variables": {
    "copy": [
        {
            "name": "disks-top-level-array",
            "count": 5,
            "input": {
                "name": "[concat('oneDataDisk', copyIndex('disks-top-level-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('disks-top-level-array')]"
            }
        },
        {
            "name": "a-different-array",
            "count": 3,
            "input": {
                "name": "[concat('twoDataDisk', copyIndex('a-different-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('a-different-array')]"
            }
        }
    ]
},
```

Diese Vorgehensweise funktioniert gut, wenn Sie Parameterwerte annehmen und sicherstellen müssen, dass sie das richtige Format für einen Vorlagenwert aufweisen. Im folgenden Beispiel werden Parameterwerte für die Verwendung beim Definieren von Sicherheitsregeln formatiert:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "securityRules": {
      "type": "array"
    }
  },
  "variables": {
    "copy": [
      {
        "name": "securityRules",
        "count": "[length(parameters('securityRules'))]",
        "input": {
          "name": "[parameters('securityRules')[copyIndex('securityRules')].name]",
          "properties": {
            "description": "[parameters('securityRules')[copyIndex('securityRules')].description]",
            "priority": "[parameters('securityRules')[copyIndex('securityRules')].priority]",
            "protocol": "[parameters('securityRules')[copyIndex('securityRules')].protocol]",
            "sourcePortRange": "[parameters('securityRules')[copyIndex('securityRules')].sourcePortRange]",
            "destinationPortRange": "[parameters('securityRules')[copyIndex('securityRules')].destinationPortRange]",
            "sourceAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].sourceAddressPrefix]",
            "destinationAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].destinationAddressPrefix]",
            "access": "[parameters('securityRules')[copyIndex('securityRules')].access]",
            "direction": "[parameters('securityRules')[copyIndex('securityRules')].direction]"
          }
        }
      }
    ]
  },
  "resources": [
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "NSG1",
      "location": "[resourceGroup().location]",
      "properties": {
        "securityRules": "[variables('securityRules')]"
      }
    }
  ],
  "outputs": {}
}
```

## <a name="recommendations"></a>Empfehlungen
Die folgenden Informationen können bei der Verwendung von Variablen hilfreich sein:

* Verwenden Sie Variablen für Werte, die Sie mehr als einmal in einer Vorlage verwenden müssen. Wenn ein Wert nur einmal verwendet wird, erleichtert ein hartcodierter Wert das Lesen Ihrer Vorlage.
* Im Abschnitt **variables** können Sie die [reference](resource-group-template-functions-resource.md#reference)-Funktion nicht nutzen. Die **reference**-Funktion leitet ihren Wert aus dem Laufzeitstatus der Ressource ab. Variablen werden jedoch während der ersten Analyse der Vorlage aufgelöst. Erstellen Sie Werte, die die **reference**-Funktion direkt in den Abschnitten **resources** oder **outputs** der Vorlage benötigen.
* Beziehen Sie Variablen für Ressourcennamen ein, die eindeutig sein müssen.

## <a name="example-templates"></a>Beispielvorlagen

Diese Beispielvorlagen veranschaulichen einige Szenarien für die Verwendung von Variablen. Stellen Sie sie bereit, um zu testen, wie Variablen in verschiedenen Szenarien verarbeitet werden. 

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
| [Variablendefinitionen](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Zeigt die verschiedenen Typen von Variablen. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Variablenwerte und gibt diese Werte zurück. |
| [Konfigurationsvariable](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Zeigt die Verwendung einer Variablen, die Konfigurationswerte definiert. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Variablenwerte und gibt diese Werte zurück. |
| [Netzwerksicherheitsregeln](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) und [Parameterdatei](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Generiert ein Array im richtigen Format zum Zuweisen von Sicherheitsregeln zu einer Netzwerksicherheitsgruppe. |


## <a name="next-steps"></a>Nächste Schritte
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Informationen zum Zusammenführen mehrerer Vorlagen während der Bereitstellung finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).
* Möglicherweise müssen Sie Ressourcen verwenden, die in einer anderen Ressourcengruppe enthalten sind. Dieses Szenario ist bei der Arbeit mit Speicherkonten oder virtuellen Netzwerken üblich, die in mehreren Ressourcengruppen gemeinsam verwendet werden. Weitere Informationen finden Sie unter der [resourceId-Funktion](resource-group-template-functions-resource.md#resourceid).