---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: a528fad10144ec733a3db5340ef12dee5ce5411c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553544"
---
| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Ressourcen pro [Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md#resource-groups), pro Ressourcentyp |800 |Variiert je nach Ressourcentyp |
| Bereitstellungen pro Ressourcenverlauf im Bereitstellungsverlauf |800 |800 |
| Ressourcen pro Bereitstellung |800 |800 |
| Managementsperren pro eindeutigem Bereich |20 |20 |
| Anzahl Tags pro Ressource oder Ressourcengruppe |15 |15 |
| Tagschlüssellänge |512 |512 |
| Tagwertlänge |256 |256 |


#### <a name="template-limits"></a>Vorlagengrenzwerte

| Wert | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Parameter |256 |256 |
| Variables |256 |256 |
| Ressourcen einschließlich Kopienzahl |800 |800 |
| Ausgaben |64 |64 |
| Vorlagenausdruck |24.576 Zeichen |24.576 Zeichen |
| Ressourcen in exportierten Vorlagen |200 |200 | 
| Vorlagengröße |1 MB |1 MB |
| Parameterdateigröße |64 KB |64 KB |

Sie können einige Vorlagengrenzwerte überschreiten, indem Sie eine geschachtelte Vorlage verwenden. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](../articles/azure-resource-manager/resource-group-linked-templates.md). Um die Anzahl von Parametern, Variablen oder Ausgaben zu reduzieren, können Sie mehrere Werte in einem Objekt kombinieren. Weitere Informationen finden Sie unter [Objekte als Parameter](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

Wenn der Grenzwert von 800 Bereitstellungen pro Ressourcengruppe erreicht ist, löschen Sie nicht mehr benötigte Bereitstellungen aus dem Verlauf. Bei Verwendung der Azure CLI können Sie mit [az group deployment delete](/cli/azure/group/deployment) Einträge aus dem Verlauf löschen. Sie können auch [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) in PowerShell nutzen. Das Löschen eines Eintrags aus dem Bereitstellungsverlaufs wirkt sich nicht auf die bereitgestellten Ressourcen aus. 