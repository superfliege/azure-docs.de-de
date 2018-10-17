---
title: Einfache Azure Resource Graph-Abfragen
description: Verwenden Sie Azure Resource Graph, um einige einfache Abfragen auszuführen.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: bcc6c6e9d38948cb4be09c9f52049790ccd5ac7e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227026"
---
# <a name="starter-resource-graph-queries"></a>Einfache Resource Graph-Abfragen

Der erste Schritt zum Verstehen von Abfragen mit Azure Resource Graph sind Grundkenntnisse der [Abfragesprache](../concepts/query-language.md). Wenn Sie nicht bereits mit dem [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md) vertraut sind, sollten Sie sich über die Grundlagen informieren, um zu verstehen, wie Sie die Anforderungen für die Ressourcen, die Sie suchen, zusammenstellen müssen.

Wir behandeln die folgenden einfachen Abfragen:

> [!div class="checklist"]
> - [Anzahl der Azure-Ressourcen](#count-resources)
> - [Auflisten von Ressourcen nach Namen sortiert](#list-resources)
> - [Anzeigen aller virtuellen Computer in absteigender Folge nach Namen sortiert](#show-vms)
> - [Anzeigen der ersten fünf virtuellen Computer nach Name und BS-Typ](#show-sorted)
> - [Anzahl von virtuellen Computern nach Betriebssystemtyp](#count-os)
> - [Anzeigen von Ressourcen, die Speicher enthalten](#show-storage)
> - [Liste der öffentlichen IP-Adressen](#list-publicip)
> - [Anzahl von Ressourcen, für die IP-Adressen konfiguriert sind, nach Abonnement](#count-resources-by-ip)
> - [Auflisten von Ressourcen mit einem bestimmten Tagwert](#list-tag)
> - [Auflisten aller Speicherkonten mit einem bestimmten Tagwert](#list-specific-tag)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

## <a name="language-support"></a>Sprachunterstützung

Azure CLI (über eine Erweiterung) und Azure PowerShell (über ein Modul) unterstützen Azure Resource Graph. Überprüfen Sie vor dem Ausführen der folgenden Abfragen, ob Ihre Umgebung bereit ist. Unter [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) und [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) werden die Schritte zum Installieren und Überprüfen der Shellumgebung Ihrer Wahl beschrieben.

## <a name="count-resources"></a>Anzahl der Azure-Ressourcen

Diese Abfrage gibt die Anzahl der Azure-Ressourcen zurück, die in den Abonnements vorhanden sind, auf die Sie zugreifen können. Sie ist auch eine gute Abfrage, um sicherzustellen, dass für die Shell Ihrer Wahl die entsprechenden Azure Resource Graph-Komponenten installiert und in funktionsfähigem Zustand sind.

```Query
summarize count()
```

```azurecli-interactive
az graph query -q "summarize count()"
```

```powershell
Search-AzureRmGraph -Query "summarize count()"
```

## <a name="list-resources"></a>Auflisten von Ressourcen nach Namen sortiert

Ohne Einschränkung auf einen Ressourcentyp oder bestimmte übereinstimmende Eigenschaften gibt diese Abfrage nur **Namen**, **Typ** und **Speicherort** der Azure-Ressourcen zurück, verwendet aber `order by`, um sie nach der Eigenschaft **Name** in aufsteigender (`asc`) Reihenfolge zu sortieren.

```Query
project name, type, location
| order by name asc
```

```azurecli-interactive
az graph query -q "project name, type, location | order by name asc"
```

```powershell
Search-AzureRmGraph -Query "project name, type, location | order by name asc"
```

## <a name="show-vms"></a>Anzeigen aller virtuellen Computer in absteigender Folge nach Namen sortiert

Anstatt alle Azure-Ressourcen abzurufen, um eine Liste der virtuellen Computer zu erhalten (die `Microsoft.Compute/virtualMachines` sind), können wir die Eigenschaft **Typ** in den Ergebnissen abgleichen.
Ähnelt der vorherigen Abfrage, `desc` ändert `order by` in absteigend. `=~` in der Typübereinstimmung weist Resource Graph an, die Groß-/Kleinschreibung nicht zu berücksichtigen.

```Query
project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

```azurecli-interactive
az graph query -q "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

```powershell
Search-AzureRmGraph -Query "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

## <a name="show-sorted"></a>Anzeigen der ersten fünf virtuellen Computer nach Name und BS-Typ

Diese Abfrage verwendet `limit`, um nur fünf übereinstimmende Datensätze abzurufen, die nach Namen sortiert werden. Der Typ der Azure-Ressource ist `Microsoft.Compute/virtualMachines`. `project` teilt Azure Ressource Graph mit, welche Eigenschaften einbezogen werden sollen.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="count-os"></a>Anzahl von virtuellen Computern nach Betriebssystemtyp

Auf der vorherigen Abfrage aufbauend schränken wir immer noch nach Azure-Ressourcen vom Typ `Microsoft.Compute/virtualMachines` ein, jedoch nicht mehr die Anzahl der zurückgegebenen Datensätze.
Stattdessen haben wir `summarize` und `count()` verwendet, um festzulegen, wie Werte nach Eigenschaft gruppiert und aggregiert werden sollen, in diesem Beispiel `properties.storageProfile.osDisk.osType`. Ein Beispiel, wie diese Zeichenfolge im vollständigen Objekt aussieht, finden Sie unter [Untersuchen virtueller Computer – VM-Ermittlung](../concepts/explore-resources.md#virtual-machine-discovery).

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

Eine andere Möglichkeit zum Schreiben derselben Abfrage ist, eine Eigenschaft zu erweitern (`extend`) und ihr einen temporären Namen für die Verwendung in der Abfrage zu geben, in diesem Fall **os**. **os** wird dann wie im vorherigen Beispiel von `summarize` und `count()` verwendet.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

> [!NOTE]
> Denken Sie daran, dass `=~` zwar Übereinstimmungen ohne Berücksichtigung der Groß-/Kleinschreibung ermöglicht, die Verwendung von Eigenschaften (z.B. **properties.storageProfile.osDisk.osType**) in der Abfrage jedoch die richtige Groß-/Kleinschreibung erfordert. Wenn die Eigenschaft in falscher Groß-/Kleinschreibung vorliegt, kann trotzdem ein Wert zurückgegeben werden, aber die Gruppierung oder Zusammenfassung wäre falsch.

## <a name="show-storage"></a>Anzeigen von Ressourcen, die Speicher enthalten

Anstatt explizit den Typ zu definieren, mit dem Übereinstimmung vorliegen muss, findet diese Beispielabfrage jede Azure-Ressource, die das Wort **storage** enthält (`contains`).

```Query
where type contains 'storage' | distinct type
```

```azurecli-interactive
az graph query -q "where type contains 'storage' | distinct type"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'storage' | distinct type"
```

## <a name="list-publicip"></a>Liste der öffentlichen IP-Adressen

Ähnlich wie bei der vorherigen Abfrage wird jeder Typ gefunden, der das Wort **publicIPAddresses** enthält. Diese Abfrage baut auf diesem Muster auf, um Ergebnisse auszuschließen, in denen **properties.ipAddress** NULL ist, um nur **properties.ipAddress** zurückzugeben und die Ergebnisse auf die obersten 100 zu begrenzen (`limit`). Je nach Ihrer ausgewählten Shell müssen Sie Anführungszeichen möglicherweise mit Escapezeichen versehen.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| project properties.ipAddress
| limit 100
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

## <a name="count-resources-by-ip"></a>Anzahl von Ressourcen, für die IP-Adressen konfiguriert sind, nach Abonnement

Mithilfe der vorherigen Beispielabfrage und Hinzufügen von `summarize` und `count()` können wir eine Liste nach Abonnement von Ressourcen mit konfigurierten IP-Adressen abrufen.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| summarize count () by subscriptionId
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

## <a name="list-tag"></a>Auflisten von Ressourcen mit einem bestimmten Tagwert

Wir können die Ergebnisse nach anderen Eigenschaften als dem Azure-Ressourcentyp einschränken, z.B. einem Tag. In diesem Beispiel filtern wir mit dem Tagnamen **Environment** mit dem Wert **Internal** nach Azure-Ressourcen.

```Query
where tags.environment=~'internal'
| project name
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name"
```

```powershell
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name"
```

Wenn es erforderlich wäre, auch anzugeben, welche Tags die Ressource hätte, und wie deren Werte lauten, könnte dieses Beispiel durch Hinzufügen der Eigenschaft **tags** zum `project`-Schlüsselwort erweitert werden.

```Query
where tags.environment=~'internal'
| project name, tags
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name, tags"
```

```powershell
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name, tags"
```

## <a name="list-specific-tag"></a>Auflisten aller Speicherkonten mit einem bestimmten Tagwert

Durch Kombination der Filterfunktion des vorherigen Beispiels mit der Filterung nach Azure-Ressourcentyp nach **type**-Eigenschaft können wir unsere Suche nach bestimmten Typen von Azure-Ressourcen mit einem bestimmten Tagnamen und Wert beschränken.

```Query
where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> Dieses Beispiel verwendet `==` für den Abgleich statt der `=~`-Bedingung. Bei `==` wird die Groß-/Kleinschreibung bei der Übereinstimmung beachtet.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Abfragesprache](../concepts/query-language.md)
- Lernen Sie, [Ressourcen zu untersuchen](../concepts/explore-resources.md)
- Siehe Beispiele der [erweiterten Abfragen](advanced.md)