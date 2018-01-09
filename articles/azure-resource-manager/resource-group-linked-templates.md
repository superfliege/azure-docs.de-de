---
title: "Verknüpfen von Vorlagen für die Azure-Bereitstellung | Microsoft-Dokumentation"
description: "Beschreibt, wie verknüpfte Vorlagen in einer Azure-Ressourcen-Manager-Vorlage zum Erstellen einer modularen Vorlagenprojektmappe verwendet werden. Zeigt, wie Parameterwerte übergeben, eine Parameterdatei festgelegt und URLs dynamisch erstellt werden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: tomfitz
ms.openlocfilehash: 78e5749369de1dd9865f61baefd70e6ce4bde31d
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen

Um Ihre Lösung bereitzustellen, können Sie entweder eine Einzelvorlage oder eine Hauptvorlage mit mehreren verknüpften Vorlagen verwenden. Bei kleinen bis mittelgroßen Lösungen lässt sich eine Einzelvorlage einfacher verstehen und verwalten. Sie können alle Ressourcen und Werte in einer einzelnen Datei anzeigen. In erweiterten Szenarien können Sie mithilfe verknüpfter Vorlagen die Lösung in Zielkomponenten unterteilen und Vorlagen wiederverwenden.

Bei Verwendung verknüpfter Vorlagen erstellen Sie eine Hauptvorlage, in der während der Bereitstellung Parameterwerte empfangen werden. Die Hauptvorlage enthält alle verknüpften Vorlagen und übergibt bei Bedarf Werte an diese Vorlagen.

![Verknüpfte Vorlagen](./media/resource-group-linked-templates/nestedTemplateDesign.png)

## <a name="link-to-a-template"></a>Verknüpfen mit einer Vorlage

Zum Verknüpfen mit einer anderen Vorlage fügen Sie der Hauptvorlage eine **Bereitstellungsressource** hinzu.

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <inline-template-or-external-template>
      }
  }
]
```

Die Eigenschaften, die Sie für die Bereitstellungsressource angeben, variieren je nachdem, ob Sie eine externe Vorlage verknüpfen oder eine Inlinevorlage in die Hauptvorlage einbetten.

### <a name="inline-template"></a>Inlinevorlage

Verwenden Sie zum Einbetten der verknüpften Vorlage die Eigenschaft **Vorlage**, und fügen Sie die Vorlage ein.

```json
"resources": [
  {
    "apiVersion": "2017-05-10",
    "name": "nestedTemplate",
    "type": "Microsoft.Resources/deployments",
    "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
              "accountType": "Standard_LRS"
            }
          }
        ]
      },
      "parameters": {}
    }
  }
]
```

### <a name="external-template-and-external-parameters"></a>Externe Vorlage und externe Parameter

Um eine externe Vorlage und Parameterdatei zu verknüpfen, verwenden Sie **templateLink** und **parametersLink**. Beim Verknüpfen mit einer Vorlage muss der Resource Manager-Dienst auf die verknüpfte Vorlage zugreifen können. Sie können keine lokale Datei und keine Datei angeben, die nur in Ihrem lokalen Netzwerk verfügbar ist. Sie können nur einen URI-Wert bereitstellen, der entweder **http** oder **https** enthält. Eine Option besteht darin, die verknüpfte Vorlage in einem Speicherkonto zu platzieren und den URI für dieses Element zu verwenden.

```json
"resources": [
  {
     "apiVersion": "2017-05-10",
     "name": "linkedTemplate",
     "type": "Microsoft.Resources/deployments",
     "properties": {
       "mode": "incremental",
       "templateLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       },
       "parametersLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.parameters.json",
          "contentVersion":"1.0.0.0"
       }
     }
  }
]
```

### <a name="external-template-and-inline-parameters"></a>Externe Vorlage und Inlineparameter

Alternativ können Sie den Parameter inline bereitstellen. Um einen Wert von der Hauptvorlage an die verknüpfte Vorlage zu übergeben, verwenden Sie **parameters**.

```json
"resources": [
  {
     "apiVersion": "2017-05-10",
     "name": "linkedTemplate",
     "type": "Microsoft.Resources/deployments",
     "properties": {
       "mode": "incremental",
       "templateLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       },
       "parameters": {
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"}
        }
     }
  }
]
```

## <a name="using-variables-to-link-templates"></a>Verwenden von Variablen für das Verknüpfen von Vorlagen

Die vorherigen Beispiele zeigen hartcodierte URL-Werte für die Vorlagenlinks. Dieser Ansatz funktioniert u. U. für eine einfache Vorlage, aber er funktioniert nicht gut bei der Arbeit mit einer großen Anzahl von modularen Vorlagen. Stattdessen können Sie eine statische Variable erstellen, die eine Basis-URL für die Hauptvorlage speichert, und dann aus dieser Basis-URL dynamisch URLs für die verknüpften Vorlagen erstellen. Der Vorteil dieses Ansatzes besteht darin, dass Sie die Vorlage problemlos verschieben oder verzweigen können, da Sie nur die statische Variable in der Hauptvorlage ändern müssen. Die Hauptvorlage übergibt die richtigen URIs an die zerlegten Vorlagen.

Das folgende Beispiel zeigt, wie Sie eine Basis-URL verwenden können, um zwei URLs für verknüpfte Vorlagen (**sharedTemplateUrl** und **vmTemplate**) zu erstellen.

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Sie können auch [deployment()](resource-group-template-functions-deployment.md#deployment) verwenden, um die Basis-URL für die aktuelle Vorlage zu erhalten. Mit dieser können Sie die URL für die anderen Vorlagen am gleichen Speicherort abrufen. Diese Vorgehensweise ist hilfreich, wenn sich der Speicherort der Vorlage ändert (möglicherweise aufgrund einer Versionsverwaltung) oder wenn Sie es vermeiden möchten, URLs in der Vorlagendatei fest programmieren zu müssen.

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="get-values-from-linked-template"></a>Abrufen von Werten aus einer verknüpften Vorlage

Um einen Ausgabewert aus einer verknüpften Vorlage abzurufen, rufen Sie den Eigenschaftswert mit einer der folgenden ähnlichen Syntax ab: `"[reference('<name-of-deployment>').outputs.<property-name>.value]"`.

In den folgenden Beispielen wird veranschaulicht, wie Sie auf eine verknüpfte Vorlage verweisen und einen Ausgabewert abrufen. Die verknüpfte Vorlage gibt eine einfache Nachricht zurück.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {
        "greetingMessage": {
            "value": "Hello World",
            "type" : "string"
        }
    }
}
```

Die übergeordnete Vorlage stellt die verknüpfte Vorlage bereit und ruft den zurückgegebenen Wert ab. Beachten Sie, dass sie durch den Namen auf die Bereitstellungsressource verweist und den Namen der von der verknüpften Vorlage zurückgegebenen Eigenschaft verwendet.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[uri(deployment().properties.templateLink.uri, 'helloworld.json')]",
                    "contentVersion": "1.0.0.0"
                }
            }
        }
    ],
    "outputs": {
        "messageFromLinkedTemplate": {
            "type": "string",
            "value": "[reference('linkedTemplate').outputs.greetingMessage.value]"
        }
    }
}
```

Wie andere Ressourcentypen können Sie Abhängigkeiten zwischen der verknüpften Vorlage und anderen Ressourcen festlegen. Wenn also andere Ressourcen einen Ausgabewert aus der verknüpften Vorlage benötigen, können Sie sicherstellen, dass die verknüpfte Vorlage vor ihnen bereitgestellt wird. Wenn andererseits die verknüpfte Vorlage von anderen Ressourcen abhängig ist, können Sie sicherstellen, dass andere Ressourcen vor der verknüpften Vorlage bereitgestellt werden.

Im folgenden Beispiel ist eine Vorlage dargestellt, die eine öffentliche IP-Adresse bereitstellt und die Ressourcen-ID zurückgibt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_name": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddresses_name')]",
            "apiVersion": "2017-06-01",
            "location": "eastus",
            "properties": {
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "resourceID": {
            "type": "string",
            "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
        }
    }
}
```

Um beim Bereitstellen eines Lastenausgleichs die öffentliche IP-Adresse aus der vorherigen Vorlage zu verwenden, verknüpfen Sie die Vorlage, und fügen Sie eine Abhängigkeit von der Bereitstellungsressource hinzu. Die öffentliche IP-Adresse des Lastenausgleichs wird auf den Ausgabewert von der verknüpften Vorlage festgelegt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "loadBalancers_name": {
            "defaultValue": "mylb",
            "type": "string"
        },
        "publicIPAddresses_name": {
            "defaultValue": "myip",
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "name": "[parameters('loadBalancers_name')]",
            "apiVersion": "2017-06-01",
            "location": "eastus",
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LoadBalancerFrontEnd",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[reference('linkedTemplate').outputs.resourceID.value]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [],
                "loadBalancingRules": [],
                "probes": [],
                "inboundNatRules": [],
                "outboundNatRules": [],
                "inboundNatPools": []
            },
            "dependsOn": [
                "linkedTemplate"
            ]
        },
        {
            "apiVersion": "2017-05-10",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[uri(deployment().properties.templateLink.uri, 'publicip.json')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters":{
                    "publicIPAddresses_name":{"value": "[parameters('publicIPAddresses_name')]"}
                }
            }
        }
    ]
}
```

## <a name="linked-templates-in-deployment-history"></a>Verknüpfte Vorlagen im Bereitstellungsverlauf

Der Resource Manager verarbeitet jede verknüpfte Vorlage als separate Bereitstellung im Bereitstellungsverlauf. Daher wird eine übergeordnete Vorlage mit drei verknüpften Vorlagen im Bereitstellungsverlauf wie folgt angezeigt:

![Bereitstellungsverlauf](./media/resource-group-linked-templates/deployment-history.png)

Über diese separaten Einträge im Verlauf können Sie nach der Bereitstellung Ausgabewerte abrufen. Mit der folgenden Vorlage wird eine öffentliche IP-Adresse erstellt und die IP-Adresse ausgegeben:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_name": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddresses_name')]",
            "apiVersion": "2017-06-01",
            "location": "southcentralus",
            "properties": {
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "dnsSettings": {
                    "domainNameLabel": "[concat(parameters('publicIPAddresses_name'), uniqueString(resourceGroup().id))]"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "returnedIPAddress": {
            "type": "string",
            "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]"
        }
    }
}
```

Die folgende Vorlage wird mit der vorherigen Vorlage verknüpft. Es werden drei öffentliche IP-Adressen erstellt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "[concat('linkedTemplate', copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "Incremental",
              "templateLink": {
                "uri": "[uri(deployment().properties.templateLink.uri, 'static-public-ip.json')]",
                "contentVersion": "1.0.0.0"
              },
              "parameters":{
                  "publicIPAddresses_name":{"value": "[concat('myip-', copyIndex())]"}
              }
            },
            "copy": {
                "count": 3,
                "name": "ip-loop"
            }
        }
    ]
}
```

Nach der Bereitstellung können Sie die Ausgabewerte mit dem folgenden PowerShell-Skript abrufen:

```powershell
$loopCount = 3
for ($i = 0; $i -lt $loopCount; $i++)
{
    $name = 'linkedTemplate' + $i;
    $deployment = Get-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -Name $name
    Write-Output "deployment $($deployment.DeploymentName) returned $($deployment.Outputs.returnedIPAddress.value)"
}
```

Oder mit folgendem Skript für die Azure-Azure-Befehlszeilenschnittstelle:

```azurecli
for i in 0 1 2;
do
    name="linkedTemplate$i";
    deployment=$(az group deployment show -g examplegroup -n $name);
    ip=$(echo $deployment | jq .properties.outputs.returnedIPAddress.value);
    echo "deployment $name returned $ip";
done
```

## <a name="securing-an-external-template"></a>Sichern einer externen Vorlage

Obwohl die verknüpfte Vorlage extern verfügbar sein muss, muss sie der Öffentlichkeit nicht allgemein zur Verfügung stehen. Sie können Ihre Vorlage einem privaten Speicherkonto hinzufügen, auf das nur der Speicherkontobesitzer Zugriff hat. Anschließend erstellen Sie ein SAS-Token (Shared Access Signature), um den Zugriff während der Bereitstellung zu ermöglichen. Sie fügen dieses SAS-Token dem URI für die verknüpfte Vorlage hinzu. Obwohl das Token als sichere Zeichenfolge übergeben wird, wird der URI der verknüpften Vorlage samt SAS-Token in den Bereitstellungsvorgängen protokolliert. Legen Sie ein Ablaufdatum für das Token fest, um den Zugriff zu beschränken.

Für die Parameterdatei kann auch die Einschränkung gelten, dass der Zugriff nur mithilfe eines SAS-Tokens möglich ist.

Im folgenden Beispiel wird veranschaulicht, wie ein SAS-Token beim Verknüpfen mit einer Vorlage übergeben wird:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
  }
}
```

In PowerShell rufen Sie ein Token für den Container ab und stellen die Vorlagen mit folgendem Code bereit:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

In der Azure-Befehlszeilenschnittstelle rufen Sie ein Token für den Container ab und stellen die Vorlagen mit folgendem Code bereit:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="example-templates"></a>Beispielvorlagen

Die folgenden Beispiele zeigen gängige Nutzungsszenarien für verknüpften Vorlagen.

|Hauptvorlage  |Verknüpfte Vorlage |Beschreibung  |
|---------|---------| ---------|
|[Hello World](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworldparent.json) |[Verknüpfte Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworld.json) | Gibt eine Zeichenfolge aus der verknüpften Vorlage zurück. |
|[Lastenausgleich mit öffentlicher IP-Adresse](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) |[Verknüpfte Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) |Gibt die öffentliche IP-Adresse aus der verknüpften Vorlage zurück und legt diesen Wert im Lastenausgleichsmodul fest. |
|[Mehrere IP-Adressen](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json) | [Verknüpfte Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip.json) |Erstellt mehrere öffentliche IP-Adressen in der verknüpften Vorlage.  |

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Definieren der Bereitstellungsreihenfolge Ihrer Ressourcen finden Sie unter [Definieren von Abhängigkeiten in Azure Resource Manager-Vorlagen](resource-group-define-dependencies.md).
* Informationen, wie Sie eine Ressource definieren und von dieser viele Instanzen erstellen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](resource-group-create-multiple.md).
* Schritte zum Einrichten einer Vorlage in einem Speicherkonto und zum Generieren eines SAS-Tokens finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure PowerShell](resource-group-template-deploy.md) oder [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure-CLI](resource-group-template-deploy-cli.md).