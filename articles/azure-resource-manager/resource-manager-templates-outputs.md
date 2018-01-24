---
title: Ausgaben von Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie mit deklarativer JSON-Syntax Ausgaben für Azure Resource Manager-Vorlagen definieren."
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
ms.date: 12/14/2017
ms.author: tomfitz
ms.openlocfilehash: 485a3eb5c5d04d1540482245d088c48645704465
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2017
---
# <a name="outputs-section-in-azure-resource-manager-templates"></a>Ausgabenabschnitt in Azure Resource Manager-Vorlagen
Im Ausgabenabschnitt legen Sie Werte fest, die von der Bereitstellung zurückgegeben werden. Sie könnten z. B. den URI für den Zugriff auf eine bereitgestellte Ressource zurückgeben.

## <a name="define-and-use-output-values"></a>Definieren und Verwenden von Ausgabewerten

Das folgende Beispiel zeigt die Vorgehensweise zum Zurückgeben der Ressourcen-ID für eine öffentliche IP-Adresse:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Nach der Bereitstellung können Sie den Wert per Skript abrufen. Verwenden Sie für PowerShell Folgendes:

```powershell
(Get-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

Mit der Funktion [reference](resource-group-template-functions-resource.md#reference) können Sie den Ausgabewert aus einer verknüpften Vorlage abrufen. Um einen Ausgabewert aus einer verknüpften Vorlage abzurufen, rufen Sie den Eigenschaftswert mit einer der folgenden ähnlichen Syntax ab: `"[reference('<name-of-deployment>').outputs.<property-name>.value]"`.

So können Sie beispielsweise die IP-Adresse auf einen Lastenausgleich festlegen, indem Sie einen Wert aus einer verknüpften Vorlage abrufen.

```json
"publicIPAddress": {
    "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

## <a name="available-properties"></a>Verfügbare Eigenschaften

Das folgende Beispiel zeigt die Struktur einer Ausgabedefinition:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Elementname | Erforderlich | Beschreibung |
|:--- |:--- |:--- |
| outputName |Ja |Name des Ausgabewerts. Es muss sich um einen gültigen JavaScript-Bezeichner handeln. |
| type |Ja |Der Typ des Ausgabewerts. Ausgabewerte unterstützen dieselben Typen wie Vorlagen-Eingabeparameter. |
| value |Ja |Vorlagensprachausdruck, der ausgewertet und als Ausgabewert zurückgegeben wird. |

## <a name="recommendations"></a>Empfehlungen

Wenn Sie öffentliche IP-Adressen mithilfe einer Vorlage erstellen, sollte diese einen Ausgabenabschnitt enthalten, der Details zur IP-Adresse und den vollständig qualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) zurückgibt. Mit Ausgabewerten können Sie nach der Bereitstellung ganz einfach Details zu öffentlichen IP-Adressen und FQDNs abrufen.

```json
"outputs": {
    "fqdn": {
        "value": "[reference(parameters('publicIPAddresses_name')).dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]",
        "type": "string"
    }
}
```

## <a name="example-templates"></a>Beispielvorlagen


|Vorlage  |Beschreibung  |
|---------|---------|
|[Variablen kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Erstellt komplexe Variablen und gibt die entsprechenden Werte aus. Stellt keine Ressourcen bereit. |
|[Öffentliche IP-Adresse](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Erstellt eine öffentliche IP-Adresse und gibt die Ressourcen-ID aus. |
|[Load Balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Stellt eine Verknüpfung mit der vorherigen Vorlage her. Verwendet beim Erstellen des Lastenausgleichs die Ressourcen-ID in der Ausgabe. |


## <a name="next-steps"></a>Nächste Schritte
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Informationen zum Zusammenführen mehrerer Vorlagen während der Bereitstellung finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).
* Möglicherweise müssen Sie Ressourcen verwenden, die in einer anderen Ressourcengruppe enthalten sind. Dieses Szenario ist bei der Arbeit mit Speicherkonten oder virtuellen Netzwerken üblich, die in mehreren Ressourcengruppen gemeinsam verwendet werden. Weitere Informationen finden Sie unter der [resourceId-Funktion](resource-group-template-functions-resource.md#resourceid).