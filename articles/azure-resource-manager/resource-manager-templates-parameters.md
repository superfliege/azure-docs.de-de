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
ms.date: 10/30/2018
ms.author: tomfitz
ms.openlocfilehash: 83ba1b94413990c0eb8dff42c49d46456a658d5a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417768"
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

## <a name="recommendations"></a>Empfehlungen
Die folgenden Informationen können bei der Verwendung von Parametern hilfreich sein:

* Verwenden Sie möglichst wenig Parameter. Verwenden Sie nach Möglichkeit eine Variable oder einen Literalwert. Verwenden Sie Parameter nur in den folgenden Szenarien:
   
   * Einstellungen, die sie an die Umgebung (SKU, Größe, Kapazität) anpassen möchten.
   * Ressourcennamen, die Sie zur leichteren Identifizierung angeben möchten
   * Werte, die Sie häufig nutzen, um andere Aufgaben zu erledigen (z.B. Benutzername des Administrators)
   * Geheimnisse (z.B. Kennwörter)
   * Anzahl oder Array von Werten, die beim Erstellen von mehr als einer Instanz eines Ressourcentyps verwendet werden.
* Verwenden Sie die camel-Schreibweise für Parameternamen.
* Geben Sie für jeden Parameter in den Metadaten eine Beschreibung an.

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Definieren Sie Standardwerte für Parameter (außer für Kennwörter und SSH-Schlüssel). Durch das Angeben eines Standardwerts wird der Parameter während der Bereitstellung optional. Der Standardwert kann eine leere Zeichenfolge sein. 
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Verwenden Sie **securestring** für alle Kennwörter und geheimen Schlüssel. Wenn Sie vertrauliche Daten an ein JSON-Objekt übergeben, verwenden Sie den Typ **secureObject**. Vorlagenparameter des Typs „securestring“ oder „secureObject“ können nach der Bereitstellung der Ressource nicht mehr gelesen werden. 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Verwenden Sie einen Parameter für die Angabe von Standorten, und nutzen Sie diesen Parameterwert nach Möglichkeit so oft wie möglich bei Ressourcen, die sich wahrscheinlich am selben Standort befinden. Durch diese Vorgehensweise werden Benutzer weniger häufig zur Angabe von Speicherortinformationen aufgefordert. Wenn ein Ressourcentyp nur von einer begrenzten Anzahl von Standorten unterstützt wird, können Sie einen gültigen Standort direkt in der Vorlage angeben oder einen weiteren Standortparameter hinzufügen. Wenn eine Organisation die zulässigen Regionen für Benutzer einschränkt, kann ein Benutzer durch den Ausdruck **resourceGroup().location** an der Bereitstellung der Vorlage gehindert werden. Beispiel: Ein Benutzer erstellt eine Ressourcengruppe in einer Region. Ein zweiter Benutzer muss Bereitstellungen für diese Ressourcengruppe durchführen, hat aber keinen Zugriff auf die Region. 
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[parameters('location')]",
         ...
     }
   ]
   ```
    
* Vermeiden Sie das Verwenden eines Parameters oder einer Variablen für die API-Version eines Ressourcentyps. Ressourceneigenschaften und -werte können je nach Versionsnummer variieren. Mithilfe von IntelliSense kann in Code-Editoren nicht das richtige Schema ermittelt werden, wenn die API-Version auf einen Parameter oder eine Variable festgelegt ist. Stattdessen sollten Sie die API-Version in der Vorlage hartcodieren.
* Vermeiden Sie das Angeben eines Parameternamens in der Vorlage, der mit einem Parameter im Bereitstellungsbefehl übereinstimmt. Resource Manager löst diesen Namenskonflikt durch Hinzufügen des Postfix-Elements **FromTemplate** zum Vorlagenparameter. Beispiel: Falls Sie einen Parameter namens **ResourceGroupName** in Ihrer Vorlage einfügen, wird ein Konflikt mit dem Parameter **ResourceGroupName** im Cmdlet [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) verursacht. Sie werden während der Bereitstellung zur Eingabe eines Werts für **ResourceGroupNameFromTemplate** aufgefordert.

## <a name="example-templates"></a>Beispielvorlagen

Diese Beispielvorlagen veranschaulichen einige Szenarien für die Verwendung von Parametern. Stellen Sie sie bereit, um zu testen, wie Parameter in verschiedenen Szenarien verarbeitet werden.

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
|[Parameter mit Funktionen für Standardwerte](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Zeigt, wie Vorlagenfunktionen verwendet werden, wenn Sie Standardwerte für Parameter definieren. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Parameterwerte und gibt diese Werte zurück. |
|[Parameterobjekt](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Veranschaulicht die Verwendung eines Objekts für einen Parameter. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Parameterwerte und gibt diese Werte zurück. |

## <a name="next-steps"></a>Nächste Schritte

* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Informationen zum Eingeben der Parameterwerte während der Bereitstellung finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy.md). 
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Informationen zur Verwendung eines Parameterobjekts finden Sie unter [Verwenden eines Objekts als Parameter in einer Azure Resource Manager-Vorlage](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).
