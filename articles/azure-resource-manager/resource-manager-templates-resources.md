---
title: 'Azure Resource Manager-Vorlagen: Struktur und Syntax | Microsoft-Dokumentation'
description: Beschreibt die Struktur und die Eigenschaften der Azure Resource Manager-Vorlagen mithilfe deklarativer JSON-Syntax.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2017
ms.author: tomfitz
ms.openlocfilehash: b5438080f71fa8f5c4f03006b75b826f1cfa576a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Abschnitt „Resources“ von Azure Resource Manager-Vorlagen

Im Ressourcenabschnitt definieren Sie die Ressourcen, die bereitgestellt oder aktualisiert werden. Dieser Abschnitt kann komplizierter werden, da Sie die Typen, die sie bereitstellen, verstehen müssen, um die richtigen Werte resources zu können.

## <a name="available-properties"></a>Verfügbare Eigenschaften

Sie definieren Ressourcen mit der folgenden Struktur:

```json
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
]
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| condition | Nein  | Boolescher Wert, der angibt, ob die Ressource bereitgestellt wird. |
| apiVersion |Ja |Version der REST-API zum Erstellen der Ressource. |
| type |Ja |Der Typ der Ressource. Dieser Wert ist eine Kombination aus dem Namespace des Ressourcenanbieters und dem Ressourcentyp (z.B. **Microsoft.Storage/storageAccounts**). |
| name |Ja |Der Name der Ressource. Der Name muss die Einschränkungen für URI-Komponenten laut Definition in RFC3986 erfüllen. Darüber hinaus überprüfen Azure-Dienste, die externen Parteien den Ressourcennamen verfügbar machen, den Namen, um sicherzustellen, dass es sich nicht um einen Versuch handelt, eine andere Identität vorzutäuschen. |
| location |Variabel |Unterstützte Standorte der angegebenen Ressource Wählen Sie einen der verfügbaren Standorte. In der Regel ist es jedoch sinnvoll, einen in der Nähe der Benutzer zu wählen. Normalerweise ist es auch sinnvoll, Ressourcen, die miteinander interagieren, in der gleichen Region zu platzieren. eine Rollenzuweisung) jedoch nicht. |
| tags |Nein  |Markierungen, die der Ressource zugeordnet sind Verwenden Sie Tags zum logischen Organisieren der Ressourcen in Ihrem Abonnement. |
| Kommentare |Nein  |Ihre Notizen zur Dokumentierung der Ressourcen in Ihrer Vorlage |
| copy |Nein  |Wenn mehr als eine Instanz erforderlich ist, die Anzahl der zu erstellenden Ressourcen. Der Standardmodus ist parallel. Geben Sie den seriellen Modus an, wenn nicht alle Ressourcen gleichzeitig bereitgestellt werden sollen. Weitere Informationen finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nein  |Ressourcen, die bereitgestellt werden müssen, bevor diese Ressource bereitgestellt wird. Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der richtigen Reihenfolge bereit. Wenn Ressourcen nicht voneinander abhängig sind, werden sie parallel bereitgestellt. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen oder eindeutigen Ressourcenbezeichnern sein. Es werden nur Ressourcen aufgelistet, die in dieser Vorlage bereitgestellt werden. Ressourcen, die nicht in dieser Vorlage definiert sind, müssen bereits vorhanden sein. Vermeiden Sie das Hinzufügen unnötiger Abhängigkeiten, da diese die Bereitstellung verlangsamen und Ringabhängigkeiten schaffen können. Tipps für das Festlegen von Abhängigkeiten finden Sie unter [Definieren von Abhängigkeiten in Azure Resource Manager-Vorlagen](resource-group-define-dependencies.md). |
| Eigenschaften |Nein  |Ressourcenspezifische Konfigurationseinstellungen. Die Werte für die Eigenschaften sind mit den Werten identisch, die Sie im Anforderungstext für den REST-API-Vorgang (PUT-Methode) angegeben haben, um die Ressource zu erstellen. Sie können auch ein Kopierarray angeben, um mehrere Instanzen einer Eigenschaft zu erstellen. |
| resources |Nein  |Untergeordnete Ressourcen, die von der definierten Ressource abhängig sind. Stellen Sie nur Ressourcentypen bereit, die laut Schema der übergeordneten Ressource zulässig sind. Der vollqualifizierte Typ der untergeordneten Ressource enthält den übergeordneten Ressourcentyp, z.B. **Microsoft.Web/sites/extensions**. Eine Abhängigkeit von der übergeordneten Ressource ist nicht impliziert. Sie müssen diese Abhängigkeit explizit definieren. |

## <a name="resource-specific-values"></a>Ressourcenspezifische Werte

Die Werte von **apiVersion**, **type** und **properties** unterscheiden sich bei den einzelnen Ressourcentypen. Informationen zum Ermitteln der Werte für diese Eigenschaften finden Sie in der [Vorlagenreferenz](/azure/templates/).

## <a name="resource-names"></a>Ressourcennamen
Im Allgemeinen arbeiten Sie in Resource Manager mit drei Arten von Ressourcennamen:

* Ressourcennamen, die eindeutig sein müssen.
* Ressourcennamen, die nicht eindeutig sein müssen. Sie können jedoch einen Namen angeben, der das Identifizieren der Ressource vereinfacht.
* Ressourcennamen, die allgemein sein können.

### <a name="unique-resource-names"></a>Eindeutige Ressourcennamen
Sie müssen einen eindeutigen Ressourcennamen für alle Ressourcentypen angeben, die einen Datenzugriffsendpunkt haben. Es folgen allgemeine Ressourcentypen, für die ein eindeutiger Name erforderlich ist:

* Azure Storage<sup>1</sup> 
* Web-Apps-Funktion von Azure App Service
* SQL Server
* Azure Key Vault
* Azure Redis Cache
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

## <a name="location"></a>Speicherort
Beim Bereitstellen einer Vorlage müssen Sie einen Speicherort für jede Ressource angeben. An verschiedenen Speicherorten werden unterschiedliche Ressourcentypen unterstützt. Um eine Liste aller für Ihr Abonnement verfügbaren Speicherorte für einen bestimmten Ressourcentyp anzuzeigen, verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle. 

Das folgende Beispiel verwendet PowerShell, um die Speicherorte für den Ressourcentyp `Microsoft.Web\sites` abzurufen:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Das folgende Beispiel verwendet Azure CLI 2.0, um die Speicherorte für den Ressourcentyp `Microsoft.Web\sites` abzurufen:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

Nachdem Sie einen unterstützten Speicherort für Ihre Ressourcen ermittelt haben, legen Sie diesen Speicherort in Ihrer Vorlage fest. Die einfachste Möglichkeit zum Festlegen dieses Werts ist es, eine Ressourcengruppe in einem Speicherort zu erstellen, der die entsprechenden Ressourcentypen unterstützt, und jeden Speicherort auf `[resourceGroup().location]` festzulegen. Sie können die Vorlage für Ressourcengruppen in verschiedenen Speicherorten bereitstellen, ohne Werte in der Vorlage oder Parameter ändern zu müssen. 

Das folgende Beispiel zeigt ein Speicherkonto, das im gleichen Speicherort bereitgestellt wurde wie die Ressourcengruppe:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
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

Wenn Sie einen vollqualifizierten Verweis auf eine Ressource erstellen, ist die Reihenfolge für die Kombination von Segmenten von Typ und Name nicht einfach eine Verkettung beider Werte.  Verwenden Sie stattdessen nach dem Namespace eine Folge von *Typ-Name*-Paaren, beginnend mit dem am wenigsten spezifischen bis zum spezifischsten:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Beispiel: 

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` ist richtig `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` ist nicht richtig

## <a name="recommendations"></a>Empfehlungen
Die folgenden Informationen können bei der Verwendung von Ressourcen hilfreich sein:

* Geben Sie in der Vorlage **Kommentare** für jede Ressource ein, damit andere Mitwirkende den Zweck der Ressource verstehen.
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Wenn Sie einen *öffentlichen Endpunkt* in Ihrer Vorlage verwenden (z.B. einen öffentlichen Azure Blob Storage-Endpunkt), dürfen Sie den Namespace *nicht hartcodieren*. Verwenden Sie die **reference**-Funktion, um den Namespace dynamisch abzurufen. Mit diesem Ansatz können Sie die Vorlage in anderen öffentlichen Namespace-Umgebungen bereitstellen, ohne den Endpunkt in der Vorlage manuell zu ändern. Legen Sie die API-Version auf die Version fest, die Sie für das Speicherkonto in der Vorlage verwenden:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Wenn das Speicherkonto in der Vorlage bereitgestellt wird, die Sie gerade erstellen, müssen Sie beim Verweisen auf die Ressource nicht den Namespace des Anbieters angeben. Das folgende Beispiel zeigt die vereinfachte Syntax:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Wenn die Vorlage andere Werte enthält, die für die Verwendung eines öffentlichen Namespace konfiguriert sind, ändern Sie diese Werte entsprechend der **reference**-Funktion. Beispiel: Sie können die **storageUri**-Eigenschaft des Diagnoseprofils des virtuellen Computers festlegen:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Sie können auch auf ein in einer anderen Ressourcengruppe vorhandenes Speicherkonto verweisen:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Weisen Sie einem virtuellen Computer nur dann öffentliche IP-Adressen zu, wenn dies für eine Anwendung erforderlich ist. Verwenden Sie zum Herstellen einer Verbindung mit einem virtuellen Computer für das Debuggen, die Verwaltung oder administrative Zwecke entweder NAT-Eingangsregeln, ein Gateway für virtuelle Netzwerke oder eine Jumpbox.
   
     Weitere Informationen zum Herstellen einer Verbindung mit virtuellen Computern finden Sie unter:
   
   * [Run Windows VMs for an N-tier application](../guidance/guidance-compute-n-tier-vm.md) (Ausführen virtueller Windows-Computer in einer Architektur mit n Ebenen in Azure)
   * [Einrichten des Zugriffs auf WinRM für virtuelle Computer in Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [Öffnen von Ports für einen virtuellen Computer in Azure mithilfe des Azure-Portals](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Öffnen von Ports und Endpunkten für einen virtuellen Computer in Azure mithilfe von PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Öffnen von Ports und Endpunkten für einen virtuellen Linux-Computer mithilfe der Azure CLI](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* Die Eigenschaft **domainNameLabel** für öffentliche IP-Adressen muss eindeutig sein. Der Wert **domainNameLabel** muss 3 bis 63 Zeichen lang sein und den Regeln des regulären Ausdrucks `^[a-z][a-z0-9-]{1,61}[a-z0-9]$` entsprechen. Da die **uniqueString**-Funktion eine Zeichenfolge mit 13 Zeichen erstellt, ist der **dnsPrefixString**-Parameter auf 50 Zeichen beschränkt:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Verwenden Sie beim Hinzufügen eines Kennworts zu einer benutzerdefinierten Skripterweiterung die **commandToExecute**-Eigenschaft in **protectedSettings**:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > Zum Sicherzustellen der Verschlüsselung von Geheimnissen, die als Parameter an virtuelle Computer und Erweiterungen übergeben werden, verwenden Sie die **protectedSettings**-Eigenschaft der entsprechenden Erweiterungen.
   > 
   > 


## <a name="next-steps"></a>Nächste Schritte
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Informationen zum Zusammenführen mehrerer Vorlagen während der Bereitstellung finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).
* Möglicherweise müssen Sie Ressourcen verwenden, die in einer anderen Ressourcengruppe enthalten sind. Dieses Szenario ist bei der Arbeit mit Speicherkonten oder virtuellen Netzwerken üblich, die in mehreren Ressourcengruppen gemeinsam verwendet werden. Weitere Informationen finden Sie unter der [resourceId-Funktion](resource-group-template-functions-resource.md#resourceid).
* Informationen zu Einschränkungen für Ressourcennamen finden Sie unter [Empfohlene Benennungskonventionen für Azure-Ressourcen](../guidance/guidance-naming-conventions.md).