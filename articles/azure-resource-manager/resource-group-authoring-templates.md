---
title: 'Azure Resource Manager-Vorlagen: Struktur und Syntax | Microsoft-Dokumentation'
description: Beschreibt die Struktur und die Eigenschaften der Azure Resource Manager-Vorlagen mithilfe deklarativer JSON-Syntax.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2018
ms.author: tomfitz
ms.openlocfilehash: f1ce47874b759748f4a2e2ce1fb438b394443058
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "36334797"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen
In diesem Artikel wird die Struktur einer Azure Resource Manager-Vorlage beschrieben. Er zeigt die verschiedenen Abschnitte einer Vorlage und die Eigenschaften, die in diesen Abschnitten verfügbar sind. Die Vorlage besteht aus JSON-Code und Ausdrücken, mit denen Sie Werte für Ihre Bereitstellung erstellen können. Ein ausführliches Tutorial zum Erstellen einer Vorlage finden Sie unter [Erstellen Ihrer ersten Azure Resource Manager-Vorlage](resource-manager-create-first-template.md).

## <a name="template-format"></a>Vorlagenformat
In der einfachsten Struktur weist eine Vorlage die folgenden Elemente auf:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "functions": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| $schema |Ja |Speicherort der JSON-Schemadatei, die die Version der Vorlagensprache beschreibt. Verwenden Sie die im vorherigen Beispiel gezeigte URL. |
| contentVersion |Ja |Version der Vorlage (z. B. 1.0.0.0). Sie können einen beliebigen Wert für dieses Element resources. Mit diesem Wert können Sie wichtige Änderungen in der Vorlage dokumentieren. Bei der Bereitstellung von Ressourcen mithilfe der Vorlage kann mit diesem Wert sichergestellt werden, dass die richtige Vorlage verwendet wird. |
| Parameter |Nein  |Werte, die bei der Bereitstellung angegeben werden, um die Bereitstellung der Ressourcen anpassen. |
| variables |Nein  |Werte, die als JSON-Fragmente in der Vorlage verwendet werden, um Vorlagensprachausdrücke zu vereinfachen. |
| functions |Nein  |Benutzerdefinierte Funktionen, die in der Vorlage verfügbar sind. |
| resources |Ja |Ressourcentypen, die in einer Ressourcengruppe bereitgestellt oder aktualisiert werden. |
| outputs |Nein  |Werte, die nach der Bereitstellung zurückgegeben werden. |

Jedes Element weist Eigenschaften auf, die Sie festlegen können. Das folgende Beispiel zeigt die vollständige Syntax für eine Vorlage:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
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
    },
    "variables": {
        "<variable-name>": "<variable-value>",
        "<variable-object-name>": {
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
    },
    "functions": [
      {
        "namespace": "<namespace-for-your-function>",
        "members": {
          "<function-name>": {
            "parameters": [
              {
                "name": "<parameter-name>",
                "type": "<type-of-parameter-value>"
              }
            ],
            "output": {
              "type": "<type-of-output-value>",
              "value": "<function-expression>"
            }
          }
        }
      }
    ],
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

In diesem Artikel werden die Abschnitte der Vorlage ausführlicher beschrieben.

## <a name="syntax"></a>Syntax
Die grundlegende Syntax der Vorlage ist JSON. Allerdings erweitern Ausdrücke und Funktionen die JSON-Werte, die in der Vorlage zur Verfügung stehen.  Ausdrücke werden innerhalb von JSON-Zeichenfolgenliteralen geschrieben, deren erstes und letztes Zeichen jeweils eine Klammer ist: `[` und `]`. Der Wert des Ausdrucks wird bei der Bereitstellung der Vorlage ausgewertet. Trotz der Schreibweise als Zeichenfolgenliteral kann das Ergebnis der Auswertung des Ausdrucks ein anderer JSON-Typ sein, z.B. ein Array oder Integer. Dies hängt vom jeweiligen Ausdruck ab.  Wenn eine Literalzeichenfolge mit einer Klammer `[` beginnen, aber nicht als Ausdruck interpretiert werden soll, fügen Sie am Anfang der Zeichenfolge eine weitere Klammer ein: `[[`.

In der Regel verwenden Sie Ausdrücke mit Funktionen, um Vorgänge zum Konfigurieren der Bereitstellung durchzuführen. Genau wie in JavaScript haben Funktionsaufrufe das Format `functionName(arg1,arg2,arg3)`. Auf Eigenschaften verweisen Sie mithilfe der Operatoren Punkt und [Index].

Das folgende Beispiel zeigt, wie Sie verschiedene Funktionen beim Erstellen eines Werts verwenden:

```json
"variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
}
```

Unter [Funktionen von Azure-Ressourcen-Manager-Vorlagen](resource-group-template-functions.md)finden Sie eine vollständige Liste der Vorlagenfunktionen. 

## <a name="parameters"></a>Parameter
Im Abschnitt „Parameter“ der Vorlage geben Sie an, welche Werte Sie beim Bereitstellen der Ressourcen eingeben können. Mit diesen Parameterwerten können Sie die Bereitstellung anpassen, indem Sie Werte resources, die für eine bestimmte Umgebung (z. B. Entwicklung, Testing oder Produktion) maßgeschneidert sind. Sie müssen in der Vorlage nicht unbedingt Parameter angeben, aber ohne Parameter stellt Ihre Vorlage immer die gleichen Ressourcen mit den gleichen Namen, Speicherorten und Eigenschaften bereit.

Das folgende Beispiel zeigt eine einfache Parameterdefinition:

```json
"parameters": {
  "siteNamePrefix": {
    "type": "string",
    "metadata": {
      "description": "The name prefix of the web app that you wish to create."
    }
  },
},
```

Informationen zum Definieren von Parametern finden Sie im [Abschnitt „Parameters“ von Azure Resource Manager-Vorlagen](resource-manager-templates-parameters.md).

## <a name="variables"></a>Variables
Im Abschnitt „variables“ erstellen Sie Werte, die in der ganzen Vorlage verwendet werden können. Sie müssen nicht unbedingt Variablen definieren, aber häufig bewirken sie eine Vereinfachung Ihrer Vorlage, indem komplexe Ausdrücke reduziert werden.

Im folgenden Beispiel wird eine einfache Variablendefinition gezeigt:

```json
"variables": {
  "webSiteName": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
},
```

Informationen zum Definieren von Variablen finden Sie im [Abschnitt „Variables“ von Azure Resource Manager-Vorlagen](resource-manager-templates-variables.md).

## <a name="functions"></a>Functions

In Ihrer Vorlage können Sie Ihre eigenen Funktionen erstellen. Diese Funktionen stehen dann zur Verwendung in der Vorlage zur Verfügung. Normalerweise definieren Sie komplexe Ausdrücke, die in der Vorlage nicht wiederholt werden sollen. Sie erstellen die benutzerdefinierten Funktionen aus Ausdrücken und [Funktionen](resource-group-template-functions.md), die in Vorlagen unterstützt werden.

Beim Definieren einer benutzerdefinierten Funktion gelten einige Einschränkungen:

* Die Funktion kann nicht auf Variablen zugreifen.
* Die Funktion kann keine anderen benutzerdefinierten Funktionen aufrufen.
* Die Funktion kann nicht die [Referenzfunktion](resource-group-template-functions-resource.md#reference) verwenden.
* Für die Parameter der Funktion können keine Standardwerte verwendet werden.

Für benutzerdefinierte Funktionen müssen Namespacewerte verwendet werden, um Namenskonflikte mit Vorlagenfunktionen zu vermeiden. Im folgenden Beispiel wird eine Funktion veranschaulicht, die einen Speicherkontonamen zurückgibt:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

Sie rufen die Funktion wie folgt auf:

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "location": "South Central US",
    "tags": {},
    "properties": {}
  }
]
```

## <a name="resources"></a>angeben
Im Ressourcenabschnitt definieren Sie die Ressourcen, die bereitgestellt oder aktualisiert werden. Dieser Abschnitt kann komplizierter werden, da Sie die bereitgestellten Typen verstehen müssen, um die richtigen Werte angeben zu können.

```json
"resources": [
  {
    "apiVersion": "2016-08-01",
    "name": "[variables('webSiteName')]",
    "type": "Microsoft.Web/sites",
    "location": "[resourceGroup().location]",
    "properties": {
      "serverFarmId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Web/serverFarms/<plan-name>"
    }
  }
],
```

Weitere Informationen finden Sie im [Abschnitt „Resources“ von Azure Resource Manager-Vorlagen](resource-manager-templates-resources.md).

## <a name="outputs"></a>Ausgaben
Im Ausgabenabschnitt legen Sie Werte fest, die von der Bereitstellung zurückgegeben werden. Sie könnten z. B. den URI für den Zugriff auf eine bereitgestellte Ressource zurückgeben.

```json
"outputs": {
  "newHostName": {
    "type": "string",
    "value": "[reference(variables('webSiteName')).defaultHostName]"
  }
}
```

Weitere Informationen finden Sie im [Abschnitt „Outputs“ von Azure Resource Manager-Vorlagen](resource-manager-templates-outputs.md).

## <a name="template-limits"></a>Vorlagengrenzwerte

Begrenzen Sie die Größe der Vorlage auf 1 MB und die jeder Parameterdatei auf 64 KB. Die 1-MB-Beschränkung gilt für den endgültigen Status der Vorlage, nachdem sie durch iterative Ressourcendefinitionen und Werte für variables und Parameter erweitert wurde. 

Außerdem gelten folgenden Beschränkungen:

* 256 Parameter
* 256 Variablen
* 800 Ressourcen (einschließlich copy-Anzahl)
* 64 Ausgabewerte
* 24.576 Zeichen in einem Vorlagenausdruck

Sie können einige Vorlagengrenzwerte überschreiten, indem Sie eine geschachtelte Vorlage verwenden. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md). Um die Anzahl von Parametern, Variablen oder Ausgaben zu reduzieren, können Sie mehrere Werte in einem Objekt kombinieren. Weitere Informationen finden Sie unter [Objekte als Parameter](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Nächste Schritte
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Informationen zum Zusammenführen mehrerer Vorlagen während der Bereitstellung finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).
* Möglicherweise müssen Sie Ressourcen verwenden, die in einer anderen Ressourcengruppe enthalten sind. Dieses Szenario ist bei der Arbeit mit Speicherkonten oder virtuellen Netzwerken üblich, die in mehreren Ressourcengruppen gemeinsam verwendet werden. Weitere Informationen finden Sie unter der [resourceId-Funktion](resource-group-template-functions-resource.md#resourceid).
