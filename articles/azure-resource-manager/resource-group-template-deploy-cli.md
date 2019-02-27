---
title: Bereitstellen von Ressourcen mit Azure-CLI und Vorlagen | Microsoft Docs
description: Verwenden Sie Azure Resource Manager und Azure-CLI, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2019
ms.author: tomfitz
ms.openlocfilehash: 0c298ba2074a57bd182b23e5fd9bc6b68ee496ad
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56300448"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI

In diesem Artikel wird erläutert, wie Ihre Ressourcen mithilfe der Azure CLI und Azure Resource Manager-Vorlagen in Azure bereitgestellt werden. Wenn Sie nicht mit den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen vertraut sind, informieren Sie sich unter [Übersicht über den Azure Resource Manager](resource-group-overview.md).  

Die Resource Manager-Vorlage, die Sie bereitstellen, kann entweder eine lokale Datei auf Ihrem Computer oder eine externe Datei sein, die sich in einem Repository wie GitHub befindet. Die Vorlage, die Sie in diesem Artikel bereitstellen, ist als [Speicherkontovorlage in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json) verfügbar.

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Wenn die Azure CLI nicht installiert ist, können Sie [Cloud Shell](#deploy-template-from-cloud-shell) verwenden.

## <a name="deploy-local-template"></a>Bereitstellen einer lokalen Vorlage

Beim Bereitstellen von Ressourcen in Azure gehen Sie folgendermaßen vor:

1. Anmelden bei Ihrem Azure-Konto
2. Erstellen Sie eine Ressourcengruppe, die als Container für die bereitgestellten Ressourcen fungiert. Der Name einer Ressourcengruppe darf nur alphanumerische Zeichen, Punkte, Unterstriche, Bindestriche und Klammern enthalten. Der Name kann bis zu 90 Zeichen umfassen. Der Name darf nicht mit einem Punkt enden.
3. Stellen Sie für die Ressourcengruppe die Vorlage bereit, die die zu erstellenden Ressourcen definiert.

Eine Vorlage kann Parameter enthalten, mit denen Sie die Bereitstellung anpassen können. Beispielsweise können Sie Werte angeben, die einer bestimmten Umgebung (z.B. Entwicklung, Test und Produktion) angepasst sind. Die Beispielvorlage definiert einen Parameter für die Speicherkonto-SKU. 

Im folgenden Beispiel wird eine Ressourcengruppe erstellt und eine Vorlage von Ihrem lokalen Computer aus bereitgestellt:

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Nachricht mit dem Ergebnis angezeigt:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Bereitstellen einer externen Vorlage

Anstatt Resource Manager-Vorlagen auf dem lokalen Computer zu speichern, könnten Sie sie vorzugsweise an einem externen Speicherort speichern. Sie können Vorlagen in einem Quellcodeverwaltungs-Repository (z.B. GitHub) speichern. Für den gemeinsamen Zugriff in Ihrer Organisation können Sie sie auch in einem Azure-Speicherkonto speichern.

Verwenden Sie zum Bereitstellen einer externen Vorlage den **template-uri**-Parameter. Verwenden Sie den URI im Beispiel, um die Beispielvorlage aus GitHub bereitzustellen.
   
```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

Das obige Beispiel erfordert einen URI mit öffentlichem Zugriff für die Vorlage, was in den meisten Szenarien funktioniert, da die Vorlage keine vertraulichen Daten enthalten sollte. Wenn Sie vertrauliche Daten (z.B. ein Administratorkennwort) angeben müssen, übergeben Sie diesen Wert als sicheren Parameter. Wenn Sie jedoch keinen öffentlichen Zugriff auf Ihre Vorlage wünschen, können Sie sie schützen, indem Sie sie in einem privaten Speichercontainer speichern. Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token (Shared Access Signature) erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-cli-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Geben Sie in der Cloud Shell-Instanz folgende Befehle ein:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az group deployment create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="deploy-to-more-than-one-resource-group-or-subscription"></a>Bereitstellung für mehrere Ressourcengruppen oder Abonnements

In der Regel stellen Sie alle Ressourcen in der Vorlage als einzelne Ressourcengruppe bereit. Es gibt jedoch Szenarien, bei denen Sie eine Reihe von Ressourcen zwar gemeinsam, aber in verschiedenen Ressourcengruppen oder Abonnements bereitstellen möchten. Sie können nur fünf Ressourcengruppen in einer Bereitstellung bereitstellen. Weitere Informationen finden Sie unter [Bereitstellen von Azure-Ressourcen für mehrere Abonnements oder Ressourcengruppen](resource-manager-cross-resource-group-deployment.md).

## <a name="redeploy-when-deployment-fails"></a>Erneute Bereitstellung bei Bereitstellungsfehlern

Wenn eine Bereitstellung fehlschlägt, können Sie automatisch eine frühere, erfolgreiche Bereitstellung aus Ihrem Bereitstellungsverlauf bereitstellen. Zum Angeben einer erneuten Bereitstellung verwenden Sie den Parameter `--rollback-on-error` im Bereitstellungsbefehl.

Zur Verwendung dieser Option müssen die Bereitstellungen eindeutige Namen aufweisen, damit sie im Verlauf identifiziert werden können. Wenn die Bereitstellungen keine eindeutigen Namen aufweisen, wird die vorherige erfolgreich ausgeführte Bereitstellung im Verlauf möglicherweise durch die aktuelle fehlerhafte Bereitstellung überschrieben. Diese Option kann nur für Bereitstellungen auf Stammebene verwendet werden. Bereitstellungen aus einer geschachtelten Vorlage können nicht erneut bereitgestellt werden.

Um die letzte erfolgreiche Bereitstellung erneut bereitzustellen, fügen Sie den Parameter `--rollback-on-error` als Flag hinzu.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error
```

Um eine bestimmte Bereitstellung erneut bereitzustellen, verwenden den Parameter `--rollback-on-error` und geben den Namen der Bereitstellung an.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment02 \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error ExampleDeployment01
```

Die angegebene Bereitstellung muss erfolgreich ausgeführt worden sein.

## <a name="parameters"></a>Parameter

Zum Übergeben von Parameterwerten können Sie entweder Inlineparameter oder eine Parameterdatei verwenden. In den obigen Beispielen in diesem Artikel werden Inlineparameter verwendet.

### <a name="inline-parameters"></a>Inlineparameter

Wenn Sie Inlineparameter übergeben möchten, geben Sie die Werte in `parameters` an. Wenn Sie beispielsweise eine Zeichenfolge und ein Array an eine Vorlage in einer Bash-Shell übergeben möchten, verwenden Sie Folgendes:

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Sie können auch den Inhalt einer Datei abrufen und als Inlineparameter übergeben.

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Das Abrufen eines Parameterwerts aus einer Datei ist praktisch, wenn Sie Konfigurationswerte angeben müssen. Sie können beispielsweise [cloud-init-Werte für einen virtuellen Linux-Computer](../virtual-machines/linux/using-cloud-init.md) angeben.

Das Format „arrayContent.json“ sieht wie folgt aus:

```json
[
    "value1",
    "value2"
]
```

### <a name="parameter-files"></a>Parameterdateien

Anstatt Parameter als Inlinewerte in Ihrem Skript zu übergeben, ist es wohl einfacher, eine JSON-Datei zu verwenden, die die Parameterwerte enthält. Die Parameterdatei muss eine lokale Datei sein. Externe Parameterdateien werden mit der Azure-Befehlszeilenschnittstelle nicht unterstützt.

Die Parameterdatei muss im folgenden Format vorliegen:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Beachten Sie, dass der Parameterabschnitt einen Parameternamen enthält, der dem in Ihrer Vorlage definierten Parameter (storageAccountType) entspricht. Die Parameterdatei enthält einen Wert für den Parameter. Dieser Wert wird der Vorlage automatisch während der Bereitstellung übergeben. Sie können mehrere Parameterdateien erstellen und dann die entsprechende Parameterdatei für das Szenario übergeben. 

Kopieren Sie das obige Beispiel, und speichern Sie es unter dem Dateinamen `storage.parameters.json`.

Um eine lokale Parameterdatei zu übergeben, verwenden Sie `@` zur Angabe einer lokalen Datei mit dem Namen „storage.parameters.json“ anzugeben.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

### <a name="parameter-precedence"></a>Parameterrangfolge

Sie können Inlineparameter und eine lokale Parameterdatei im selben Bereitstellungsvorgang verwenden. Sie können beispielsweise einige Werte in der lokalen Parameterdatei angeben und weitere Werte während der Bereitstellung inline hinzufügen. Wenn Sie Werte für einen Parameter sowohl in der lokalen Parameterdatei als auch inline bereitstellen, haben die Inlinewerte Vorrang.

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters @demotemplate.parameters.json \
  --parameters exampleArray=@arrtest.json
```


## <a name="test-a-template-deployment"></a>Testen einer Vorlagenbereitstellung

Verwenden Sie [az group deployment validate](/cli/azure/group/deployment#az-group-deployment-validate), um die Vorlage und Parameterwerte zu testen, ohne dabei Ressourcen bereitzustellen. 

```azurecli-interactive
az group deployment validate \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Wenn keine Fehler erkannt werden, gibt der Befehl Informationen über die Testbereitstellung zurück. Beachten Sie insbesondere, dass der **error**-Wert NULL ist.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Wenn ein Fehler erkannt wird, gibt der Befehl eine Fehlermeldung zurück. Beispielsweise wird beim Übergeben eines falschen Werts für die Speicherkonto-SKU der folgende Fehler zurückgegeben:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Wenn Ihre Vorlage einen Syntaxfehler aufweist, gibt der Befehl einen Fehler zurück, der angibt, dass die Vorlage nicht analysiert werden konnte. Die Meldung gibt Zeilennummer und Position des Analysefehlers an.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

## <a name="next-steps"></a>Nächste Schritte
* In den Beispielen dieses Artikels werden Ressourcen für eine Ressourcengruppe in Ihrem Standardabonnement bereitgestellt. Wenn Sie ein anderes Abonnement verwenden möchten, lesen Sie [Manage multiple Azure subscriptions](/cli/azure/manage-azure-subscriptions-azure-cli) (Verwalten mehrerer Azure-Abonnements).
* Wenn Sie angeben möchten, wie Ressourcen behandelt werden sollen, die in der Ressourcengruppe enthalten sind, aber nicht in der Vorlage definiert wurden, lesen Sie die Informationen unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).
* Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-cli-sas-token.md).
* Informationen zum sicheren Rollout Ihres Diensts in mehreren Regionen finden Sie im Artikel zum [Azure-Bereitstellungs-Manager](deployment-manager-overview.md).
