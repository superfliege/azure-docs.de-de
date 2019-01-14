---
title: Abschnitt „Parameter“ der Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: Beschreibt den Abschnitt „Parameters“ der Azure Resource Manager-Vorlagen mithilfe deklarativer JSON-Syntax.
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
ms.date: 12/18/2018
ms.author: tomfitz
ms.openlocfilehash: fd6fcff6ac556abe3b2d34c7e8b1b0290208f5b0
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53722141"
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Abschnitt „Parameters“ von Azure Resource Manager-Vorlagen
Im Abschnitt „Parameter“ der Vorlage geben Sie an, welche Werte Sie beim Bereitstellen der Ressourcen eingeben können. Mit diesen Parameterwerten können Sie die Bereitstellung anpassen, indem Sie Werte resources, die für eine bestimmte Umgebung (z. B. Entwicklung, Testing oder Produktion) maßgeschneidert sind. Sie müssen in der Vorlage nicht unbedingt Parameter angeben, aber ohne Parameter stellt Ihre Vorlage immer die gleichen Ressourcen mit den gleichen Namen, Speicherorten und Eigenschaften bereit.

Die Anzahl der Parameter in einer Vorlage ist auf 256 beschränkt. Sie können die Anzahl der Parameter verringern, indem Sie Objekte verwenden, die mehrere Eigenschaften enthalten, wie in diesem Artikel beschrieben wird.

## <a name="define-and-use-a-parameter"></a>Definieren und Verwenden eines Parameters

Das folgende Beispiel zeigt eine einfache Parameterdefinition. Dabei wird der Name des Parameters definiert, und es wird angegeben, dass er einen Zeichenfolgenwert annimmt. Der Parameter akzeptiert nur Werte, die für seine beabsichtigte Verwendung sinnvoll sind. Er gibt einen Standardwert an, wenn während der Bereitstellung kein Wert angegeben wird. Schließlich enthält den Parameter eine Beschreibung seiner Syntax. 

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }   
}
```

In der Vorlage verweisen Sie mithilfe der folgenden Syntax auf den Wert für den Parameter:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>Verfügbare Eigenschaften

Im vorherige Beispiel wurden nur einige der Eigenschaften gezeigt, die Sie im Abschnitt „Parameter“ verwenden können. Die folgenden Eigenschaften sind verfügbar:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| parameterName |JA |Der Name des Parameters. Es muss sich um einen gültigen JavaScript-Bezeichner handeln. |
| type |JA |Der Typ des Parameterwerts. Die zulässigen Typen und Werte sind **string**, **securestring**, **int**, **bool**, **object**, **secureObject** und **array**. |
| defaultValue |Nein  |Der Standardwert für den Parameter, wenn kein Wert für den Parameter angegeben wird. |
| allowedValues |Nein  |Ein Array der zulässigen Werte für den Parameter, um sicherzustellen, dass der richtige Wert angegeben wird. |
| minValue |Nein  |Der Mindestwert für Parameter vom Typ "int", einschließlich des angegebenen Werts. |
| maxValue |Nein  |Der Höchstwert für Parameter vom Typ "int", einschließlich des angegebenen Werts. |
| minLength |Nein  |Die Mindestlänge der Parameter „string“, „securestring“ und „array“, einschließlich des angegebenen Werts. |
| maxLength |Nein  |Die Höchstlänge der Parameter „string“, „securestring“ und „array“, einschließlich des angegebenen Werts. |
| Beschreibung |Nein  |Beschreibung des Parameters, der Benutzern im Portal angezeigt wird. |

## <a name="template-functions-with-parameters"></a>Vorlagenfunktionen mit Parametern

Wenn Sie den Standardwert für einen Parameter angeben, können Sie die meisten Vorlagenfunktionen verwenden. Sie können einen anderen Parameterwert verwenden, um einen Standardwert zu erstellen. Die folgende Vorlage veranschaulicht die Verwendung von Funktionen im Standardwert:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Sie können die `reference`-Funktion nicht im Abschnitt „Parameter“ verwenden. Parameter werden vor der Bereitstellung ausgewertet, daher kann die `reference`-Funktion nicht den Laufzeitstatus einer Ressource abrufen. 

## <a name="objects-as-parameters"></a>Objekte als Parameter

Es kann einfacher sein, in Beziehung stehende Werte zu organisieren, indem sie als Objekt übergeben werden. Diese Vorgehensweise verringert auch die Anzahl der Parameter in der Vorlage.

Definieren Sie den Parameter in Ihrer Vorlage, und geben Sie ein JSON-Objekt an, anstatt einen einzelnen Wert während der Bereitstellung zu übergeben. 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Verweisen Sie anschließend auf die Untereigenschaften des Parameters, indem Sie den Punktoperator verwenden.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>Beispielvorlagen

Diese Beispielvorlagen veranschaulichen einige Szenarien für die Verwendung von Parametern. Stellen Sie sie bereit, um zu testen, wie Parameter in verschiedenen Szenarien verarbeitet werden.

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
|[Parameter mit Funktionen für Standardwerte](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Zeigt, wie Vorlagenfunktionen verwendet werden, wenn Sie Standardwerte für Parameter definieren. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Parameterwerte und gibt diese Werte zurück. |
|[Parameterobjekt](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Veranschaulicht die Verwendung eines Objekts für einen Parameter. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Parameterwerte und gibt diese Werte zurück. |

## <a name="next-steps"></a>Nächste Schritte

* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Informationen zum Eingeben der Parameterwerte während der Bereitstellung finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy.md). 
* Empfehlungen zum Erstellen von Vorlagen finden Sie unter [Azure Resource Manager template best practices (Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen)](template-best-practices.md).
* Informationen zur Verwendung eines Parameterobjekts finden Sie unter [Verwenden eines Objekts als Parameter in einer Azure Resource Manager-Vorlage](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).
