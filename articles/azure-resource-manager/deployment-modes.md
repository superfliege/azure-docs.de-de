---
title: Azure Resource Manager-Bereitstellungsmodi | Microsoft-Dokumentation
description: Beschreibt das Festlegen, ob für Azure Resource Manager eine vollständige oder inkrementelle Bereitstellung verwendet wird.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2018
ms.author: tomfitz
ms.openlocfilehash: 4d1b27c9b1694f987ea7461c16899f3e5ecb84d2
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140992"
---
# <a name="azure-resource-manager-deployment-modes"></a>Azure Resource Manager-Bereitstellungsmodi
Bei der Bereitstellung Ihrer Ressourcen geben Sie an, dass es sich bei der Bereitstellung entweder um ein inkrementelles Update oder um ein vollständiges Update handelt.  Der Hauptunterschied zwischen diesen beiden Modi besteht darin, wie Resource Manager vorhandene Ressourcen in der Ressourcengruppe behandelt, die nicht in der Vorlage enthalten sind. Der Standardmodus ist inkrementell.

## <a name="incremental-and-complete-deployments"></a>Inkrementelle und vollständige Bereitstellungen
Beim Bereitstellen von Ressourcen:

* Im vollständigen-Modus **löscht** Resource Manager Ressourcen, die in der Ressourcengruppe vorhanden, aber nicht in der Vorlage angegeben sind. 
* Im inkrementellen Modus lässt Resource Manager Ressourcen **unverändert**, die in der Ressourcengruppe vorhanden, aber nicht in der Vorlage angegeben sind.

In beiden Modi versucht Resource Manager, alle in der Vorlage angegebenen Ressourcen zu erstellen. Wenn die Ressource bereits in der Ressourcengruppe vorhanden ist und ihre Einstellungen unverändert sind, führt der Vorgang zu keiner Änderung. Wenn Sie die Eigenschaftswerte für eine Ressource ändern, wird die Ressource mit diesen neuen Werten aktualisiert. Falls Sie versuchen, den Speicherort oder Typ einer vorhandenen Ressource zu aktualisieren, tritt bei der Bereitstellung ein Fehler auf. Stellen Sie stattdessen eine neue Ressource mit dem gewünschten Speicherort oder Typ bereit.

Wenn Sie eine Ressource im inkrementellen Modus erneut bereitstellen, sollten Sie alle Eigenschaftswerte für die Ressource angeben – nicht nur diejenigen, die Sie aktualisieren. Falls Sie bestimmte Eigenschaften nicht angeben, interpretiert Resource Manager das Update als Überschreibung dieser Werte.

## <a name="example-result"></a>Beispielergebnis

Im folgenden Szenario wird der Unterschied zwischen dem inkrementellen und dem vollständigen Modus veranschaulicht:

Die **Ressourcengruppe** enthält Folgendes:

* Ressource A
* Ressource B
* Ressource C

Die **Vorlage** enthält Folgendes:

* Ressource A
* Ressource B
* Ressource D

Bei der Bereitstellung im **inkrementellen** Modus enthält die Ressourcengruppe:

* Ressource A
* Ressource B
* Ressource C
* Ressource D

Bei der Bereitstellung im **vollständigen** Modus wird Ressource C gelöscht. Die Ressourcengruppe enthält:

* Ressource A
* Ressource B
* Ressource D

## <a name="set-deployment-mode"></a>Festlegen des Bereitstellungsmodus

Verwenden Sie zum Festlegen des Bereitstellungsmodus mit PowerShell den `Mode`-Parameter.

```powershell
New-AzureRmResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json 
```

Verwenden Sie zum Festlegen des Bereitstellungsmodus mit der Azure CLI den `mode`-Parameter.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Bei Verwendung einer [verknüpften oder geschachtelten Vorlage](resource-group-linked-templates.md) müssen Sie `mode`-Eigenschaft auf `Incremental` festlegen. Nur Vorlagen auf Stammebene unterstützen den vollständigen Bereitstellungsmodus.

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen von Vorlagen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).
* Informationen zum Anzeigen der Vorgänge für einen Ressourcenanbieter finden Sie unter [Azure-REST-API](/rest/api/).

