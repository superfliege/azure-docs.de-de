---
title: Erweiterte Azure Resource Graph-Abfragen
description: Verwenden Sie Azure Resource Graph, um einige erweiterte Abfragen auszuführen.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: a9281bc333b3edd12501a6813f12b9e7ad1bf59f
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219266"
---
# <a name="advanced-resource-graph-queries"></a>Erweiterte Resource Graph-Abfragen

Der erste Schritt zum Verstehen von Abfragen mit Azure Resource Graph sind Grundkenntnisse der [Abfragesprache](../concepts/query-language.md). Wenn Sie nicht bereits mit dem [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md) vertraut sind, sollten Sie sich über die Grundlagen informieren, um zu verstehen, wie Sie die Anforderungen für die Ressourcen, die Sie suchen, zusammenstellen müssen.

Wir behandeln die folgenden erweiterten Abfragen:

> [!div class="checklist"]
> - [Abrufen von VMSS-Kapazität und -Größe](#vmss-capacity)
> - [Auflisten aller Tagnamen](#list-all-tags)
> - [Einem regulären Ausdruck entsprechende VMs](#vm-regex)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

## <a name="language-support"></a>Sprachunterstützung

Azure CLI (über eine Erweiterung) und Azure PowerShell (über ein Modul) unterstützen Azure Resource Graph. Überprüfen Sie vor dem Ausführen der folgenden Abfragen, ob Ihre Umgebung bereit ist. Unter [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) und [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) werden die Schritte zum Installieren und Überprüfen der Shellumgebung Ihrer Wahl beschrieben.

## <a name="vmss-capacity"></a>Abrufen von VMSS-Kapazität und -Größe

Diese Abfrage sucht nach Ressourcen für VM-Skalierungsgruppen (VMSS, Virtual Machine Scale Sets) und ruft verschiedene Details einschließlich der Größe des virtuellen Computers und der Kapazität der Skalierungsgruppe ab. Mit diesen Informationen wandelt die `toint()`-Funktion die Kapazität in eine Zahl um, damit sie sortiert werden kann. So werden auch die Rückgabewerte in benutzerdefiniert benannte Eigenschaften umbenannt.

```Query
where type=~ 'microsoft.compute/virtualmachinescalesets'
| where name contains 'contoso'
| project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name
| order by Capacity desc
```

```azurecli-interactive
az graph query -q "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

```powershell
Search-AzureRmGraph -Query "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

## <a name="list-all-tags"></a>Auflisten aller Tagnamen

Diese Abfrage beginnt mit dem Tag und erstellt ein JSON-Objekt, das alle eindeutigen Tagnamen und ihre entsprechenden Typen auflistet.

```Query
project tags
| summarize buildschema(tags)
```

```azurecli-interactive
az graph query -q "project tags | summarize buildschema(tags)"
```

```powershell
Search-AzureRmGraph -Query "project tags | summarize buildschema(tags)"
```

## <a name="vm-regex"></a>Einem regulären Ausdruck entsprechende VMs

Diese Abfrage sucht nach virtuellen Computern, die einem [regulären Ausdruck](/dotnet/standard/base-types/regular-expression-language-quick-reference) (als _RegEx_ bekannt) entsprechen.
Mit **matches regex @** wird der reguläre Ausdruck für den Abgleich definiert: **^Contoso(.*) [0-9] + $**. Diese RegEx-Definition wird wie folgt erläutert:

- `^` – Die Übereinstimmung muss am Anfang der Zeichenfolge beginnen.
- `Contoso` – Die Kernzeichenfolge, nach deren Übereinstimmung wir suchen (Groß-/Kleinschreibung).
- `(.*)` – Eine Übereinstimmung mit einem Teilausdruck:
  - `.` – stimmt mit einem beliebigen einzelnen Zeichen (außer eines Zeilenumbruchs) überein.
  - `*` – stimmt mit dem vorhergehenden Element null Mal oder mehrfach überein.
- `[0-9]` – Übereinstimmung von Zeichengruppe für die Ziffern 0 bis 9.
- `+` – stimmt mit dem vorhergehenden Element ein Mal oder mehrfach überein.
- `$` – Übereinstimmung des vorhergehenden Elements muss am Ende der Zeichenfolge auftreten.

Nach dem Namensabgleich stellt die Abfrage den Namen dar und sortiert aufsteigend nach Name.

```Query
where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$'
| project name
| order by name asc
```

```azurecli-interactive
az graph query -q "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

## <a name="next-steps"></a>Nächste Schritte

- Siehe Beispiele der [einfachen Abfragen](starter.md)
- Erfahren Sie mehr über die [Abfragesprache](../concepts/query-language.md)
- Lernen Sie, [Ressourcen zu untersuchen](../concepts/explore-resources.md)