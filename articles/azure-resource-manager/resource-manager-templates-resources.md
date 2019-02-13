---
title: Azure Resource Manager-Vorlagenressourcen | Microsoft Docs
description: Beschreibt den Abschnitt „Resources“ der Azure Resource Manager-Vorlagen mithilfe deklarativer JSON-Syntax.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2019
ms.author: tomfitz
ms.openlocfilehash: 01aacf8815ce4150eb1c243d4337f52c4e0b03e9
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697047"
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Abschnitt „Resources“ von Azure Resource Manager-Vorlagen

Im Ressourcenabschnitt definieren Sie die Ressourcen, die bereitgestellt oder aktualisiert werden. Dieser Abschnitt kann komplizierter werden, da Sie die bereitgestellten Typen verstehen müssen, um die richtigen Werte angeben zu können.

## <a name="available-properties"></a>Verfügbare Eigenschaften

Sie definieren Ressourcen mit der folgenden Struktur:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
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
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
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
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| condition | Nein  | Boolescher Wert, der angibt, ob die Ressource während dieser Bereitstellung bereitgestellt wird. Wenn der Wert `true` lautet, wird die Ressource während der Bereitstellung erstellt. Wenn der Wert `false` lautet, wird die Ressource für diese Bereitstellung ausgelassen. |
| apiVersion |Ja |Version der REST-API zum Erstellen der Ressource. |
| type |Ja |Der Typ der Ressource. Dieser Wert ist eine Kombination aus dem Namespace des Ressourcenanbieters und dem Ressourcentyp (z.B. **Microsoft.Storage/storageAccounts**). |
| name |Ja |Der Name der Ressource. Der Name muss die Einschränkungen für URI-Komponenten laut Definition in RFC3986 erfüllen. Darüber hinaus überprüfen Azure-Dienste, die externen Parteien den Ressourcennamen verfügbar machen, den Namen, um sicherzustellen, dass es sich nicht um einen Versuch handelt, eine andere Identität vorzutäuschen. |
| location |Variabel |Unterstützte Standorte der angegebenen Ressource Wählen Sie einen der verfügbaren Standorte. In der Regel ist es jedoch sinnvoll, einen in der Nähe der Benutzer zu wählen. Normalerweise ist es auch sinnvoll, Ressourcen, die miteinander interagieren, in der gleichen Region zu platzieren. Die meisten Ressourcentypen benötigen einen Speicherort, andere Typen (z.B. eine Rollenzuordnung) jedoch nicht. |
| tags |Nein  |Markierungen, die der Ressource zugeordnet sind Verwenden Sie Tags zum logischen Organisieren der Ressourcen in Ihrem Abonnement. |
| Kommentare |Nein  |Ihre Notizen zur Dokumentierung der Ressourcen in Ihrer Vorlage. Weitere Informationen finden Sie unter [Kommentare in Vorlagen](resource-group-authoring-templates.md#comments). |
| copy |Nein  |Wenn mehr als eine Instanz erforderlich ist, die Anzahl der zu erstellenden Ressourcen. Der Standardmodus ist parallel. Geben Sie den seriellen Modus an, wenn nicht alle Ressourcen gleichzeitig bereitgestellt werden sollen. Weitere Informationen finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nein  |Ressourcen, die bereitgestellt werden müssen, bevor diese Ressource bereitgestellt wird. Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der richtigen Reihenfolge bereit. Wenn Ressourcen nicht voneinander abhängig sind, werden sie parallel bereitgestellt. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen oder eindeutigen Ressourcenbezeichnern sein. Es werden nur Ressourcen aufgelistet, die in dieser Vorlage bereitgestellt werden. Ressourcen, die nicht in dieser Vorlage definiert sind, müssen bereits vorhanden sein. Vermeiden Sie das Hinzufügen unnötiger Abhängigkeiten, da diese die Bereitstellung verlangsamen und Ringabhängigkeiten schaffen können. Tipps für das Festlegen von Abhängigkeiten finden Sie unter [Definieren von Abhängigkeiten in Azure Resource Manager-Vorlagen](resource-group-define-dependencies.md). |
| Eigenschaften |Nein  |Ressourcenspezifische Konfigurationseinstellungen. Die Werte für die Eigenschaften sind mit den Werten identisch, die Sie im Anforderungstext für den REST-API-Vorgang (PUT-Methode) angegeben haben, um die Ressource zu erstellen. Sie können auch ein Kopierarray angeben, um mehrere Instanzen einer Eigenschaft zu erstellen. |
| sku | Nein  | Einige Ressourcen lassen Werte zu, die die bereitzustellende SKU definieren. Beispielsweise können Sie den Typ der Redundanz für ein Speicherkonto angeben. |
| kind | Nein  | Einige Ressourcen lassen einen Wert zu, der den Typ der Ressource definiert, die Sie bereitstellen. Beispielsweise können Sie den Typ der zu erstellenden Cosmos DB angeben. |
| Tarif | Nein  | Einige Ressourcen lassen Werte zu, die den bereitzustellenden Tarif definieren. Beispielsweise können Sie das Marketplace-Image für einen virtuellen Computer angeben. | 
| ressourcen |Nein  |Untergeordnete Ressourcen, die von der definierten Ressource abhängig sind. Stellen Sie nur Ressourcentypen bereit, die laut Schema der übergeordneten Ressource zulässig sind. Der vollqualifizierte Typ der untergeordneten Ressource enthält den übergeordneten Ressourcentyp, z.B. **Microsoft.Web/sites/extensions**. Eine Abhängigkeit von der übergeordneten Ressource ist nicht impliziert. Sie müssen diese Abhängigkeit explizit definieren. |

## <a name="condition"></a>Bedingung

Wenn Sie sich während der Bereitstellung entscheiden müssen, ob Sie eine Ressource erstellen möchten, verwenden Sie das `condition`-Element. Der Wert für dieses Element wird mit „true“ oder „false“ aufgelöst. Wenn der Wert TRUE ist, wird die Ressource erstellt. Wenn der Wert FALSE ist, wird die Ressource nicht erstellt. Der Wert kann nur auf die gesamte Ressource angewandt werden.

Dieser Wert wird üblicherweise verwendet, wenn Sie eine neue Ressource erstellen oder eine bereits vorhandene verwenden möchten. Verwenden Sie beispielsweise Folgendes, um anzugeben, ob ein neues Speicherkonto bereitgestellt wird oder ob ein vorhandenes Speicherkonto verwendet wird:

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

Eine vollständige Beispielvorlage, die das `condition`-Element verwendet, finden Sie unter [VM mit einem neuen oder vorhandenen virtuellen Netzwerk, Speicher und einer neuen oder vorhandenen öffentlichen IP-Adresse](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

## <a name="resource-specific-values"></a>Ressourcenspezifische Werte

Die Werte der **apiVersion**-, **type**- und **properties**-Elemente unterscheiden sich bei den einzelnen Ressourcentypen. Die **sku**-, **kind**- und **plan**-Elemente sind für einige Ressourcentypen verfügbar, aber nicht für alle. Informationen zum Ermitteln der Werte für diese Eigenschaften finden Sie in der [Vorlagenreferenz](/azure/templates/).

## <a name="resource-names"></a>Ressourcennamen

Im Allgemeinen arbeiten Sie in Resource Manager mit drei Arten von Ressourcennamen:

* Ressourcennamen, die eindeutig sein müssen.
* Ressourcennamen, die nicht eindeutig sein müssen. Sie können jedoch einen Namen angeben, der das Identifizieren der Ressource vereinfacht.
* Ressourcennamen, die allgemein sein können.

### <a name="unique-resource-names"></a>Eindeutige Ressourcennamen

Geben Sie einen eindeutigen Ressourcennamen für alle Ressourcentypen an, die einen Datenzugriffsendpunkt haben. Es folgen allgemeine Ressourcentypen, für die ein eindeutiger Name erforderlich ist:

* Azure Storage<sup>1</sup> 
* Web-Apps-Funktion von Azure App Service
* SQL Server
* Azure Key Vault
* Azure Cache for Redis
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup>Für Namen von Speicherkonten gilt darüber hinaus Folgendes: Kleinbuchstaben, max. 24 Zeichen, keine Bindestriche.

Wenn Sie den Namen festlegen, können Sie entweder manuell einen eindeutigen Namen erstellen oder die [uniqueString()](resource-group-template-functions-string.md#uniquestring)-Funktion zum Generieren eines Namens verwenden. Sie können auch ein Präfix oder Suffix zum **uniqueString**-Ergebnis hinzufügen. Wenn Sie den eindeutigen Namen ändern, können Sie den Ressourcentyp einfacher aus dem Namen ableiten. Beispielsweise können Sie einen eindeutigen Namen für ein Speicherkonto mit der folgenden Variablen generieren:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Ressourcennamen zur Identifizierung
Einige Ressourcentypen möchten Sie vielleicht benennen, ihre Namen müssen jedoch nicht eindeutig sein. Für diese Ressourcentypen können Sie einen Namen festlegen, der sowohl den Ressourcenkontext als auch den Ressourcentyp angibt.

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

### <a name="generic-resource-names"></a>Allgemeine Ressourcennamen
Für Ressourcentypen, auf die hauptsächlich über eine andere Ressource zugegriffen wird, können Sie einen allgemeinen Namen wählen, der in der Vorlage hartcodiert ist. Beispielsweise können Sie einen allgemeinen Standardnamen für Firewallregeln auf einer SQL Server-Instanz festlegen:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Standort
Beim Bereitstellen einer Vorlage müssen Sie einen Speicherort für jede Ressource angeben. An verschiedenen Speicherorten werden unterschiedliche Ressourcentypen unterstützt. Informationen zum Abrufen der unterstützten Speicherorte für einen Ressourcentyp finden Sie unter [Azure-Ressourcenanbieter und -typen](resource-manager-supported-services.md).

Verwenden Sie einen Parameter zur Angabe des Standorts für Ressourcen, und legen den Standardwert auf `resourceGroup().location` fest.

Das folgende Beispiel zeigt ein Speicherkonto, das an einem als Parameter angegebenen Speicherort bereitgestellt wird:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

Wenn Sie den Speicherort in Ihrer Vorlage hartcodieren müssen, geben Sie den Namen einer der unterstützten Regionen an. Das folgende Beispiel zeigt ein Speicherkonto, das immer in „USA, Norden-Mitte“ bereitgestellt wird:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="tags"></a>Tags
[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

### <a name="add-tags-to-your-template"></a>Hinzufügen von Tags zu Ihrer Vorlage

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Untergeordnete Ressourcen

Innerhalb einiger Ressourcen können Sie auch ein Array untergeordneter Ressourcen definieren. Untergeordnete Ressourcen sind Ressourcen, die nur im Kontext einer anderen Ressource verfügbar sind. Beispielsweise kann eine SQL-Datenbank nicht ohne einen SQL-Server vorhanden sein – also ist die Datenbank ein untergeordnetes Element des Servers. Sie können die Datenbank in der Definition für den Server definieren.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Der Typ wird bei der Schachtelung auf `databases` festgelegt, der vollständige Ressourcentyp lautet jedoch `Microsoft.Sql/servers/databases`. `Microsoft.Sql/servers/` wird nicht angegeben, da dieser Teil vom übergeordneten Ressourcentyp übernommen wird. Der Name der untergeordneten Ressource ist auf `exampledatabase` festgelegt, der vollständige Name enthält jedoch auch den Namen der übergeordneten Ressource. `exampleserver` wird nicht angegeben, da dieser Teil von der übergeordneten Ressource übernommen wird.

Der Typ der untergeordneten Ressource weist folgendes Format auf: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Der Name der untergeordneten Ressource weist folgendes Format auf: `{parent-resource-name}/{child-resource-name}`

Sie müssen die Datenbank aber nicht auf dem Server definieren. Sie können die untergeordnete Ressource auf der obersten Ebene definieren. So können Sie vorgehen, wenn die übergeordnete Ressource nicht in derselben Vorlage bereitgestellt wird oder wenn Sie mithilfe von `copy` mehrere untergeordnete Ressourcen erstellen möchten. In diesem Fall müssen Sie den vollständigen Ressourcentyp angeben und den Namen der übergeordneten Ressource in den Namen der untergeordneten Ressource einfügen.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Wenn Sie einen vollqualifizierten Verweis auf eine Ressource erstellen, ist die Reihenfolge für die Kombination von Segmenten von Typ und Name nicht einfach eine Verkettung beider Werte. Verwenden Sie stattdessen nach dem Namespace eine Folge von *Typ-Name*-Paaren, beginnend mit dem am wenigsten spezifischen bis zum spezifischsten:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Beispiel: 

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` ist richtig `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` ist nicht richtig



## <a name="next-steps"></a>Nächste Schritte
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Empfehlungen zum Erstellen von Vorlagen finden Sie unter [Azure Resource Manager template best practices (Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen)](template-best-practices.md).
* Möglicherweise müssen Sie Ressourcen verwenden, die in einer anderen Ressourcengruppe enthalten sind. Dieses Szenario ist bei der Arbeit mit Speicherkonten oder virtuellen Netzwerken üblich, die in mehreren Ressourcengruppen gemeinsam verwendet werden. Weitere Informationen finden Sie unter der [resourceId-Funktion](resource-group-template-functions-resource.md#resourceid).
* Informationen zu Einschränkungen für Ressourcennamen finden Sie unter [Empfohlene Benennungskonventionen für Azure-Ressourcen](../guidance/guidance-naming-conventions.md).