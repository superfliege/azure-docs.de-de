---
title: Kennzeichnen von Azure-Ressourcen für die logische Organisation | Microsoft-Dokumentation
description: Zeigt, wie Sie Tags zum Organisieren von Azure-Ressourcen für die Abrechnung und Verwaltung anwenden können.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: tomfitz
ms.openlocfilehash: 8c828bb49548adfdb02ed6fb1611eb405ebf4ff2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34602923"
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Verwenden von Tags zum Organisieren von Azure-Ressourcen

[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="powershell"></a>PowerShell

Die Beispiele in diesem Artikel erfordern Version 6.0 oder höher von Azure PowerShell. Wenn Sie keine Version 6.0 oder höher haben, [aktualisieren Sie Ihre Version](/powershell/azure/install-azurerm-ps).

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressourcengruppe* anzuzeigen:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Das Skript gibt das folgende Format zurück:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einer angegebenen Ressourcen-ID* anzuzeigen:

```powershell
(Get-AzureRmResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

Oder verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einem angegebenen Namen und einer Ressourcengruppe* anzuzeigen:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Verwenden Sie Folgendes, um *Ressourcengruppen mit einem bestimmten Tag* abzurufen:

```powershell
(Get-AzureRmResourceGroup -Tag @{ Dept="Finance" }).ResourceGroupName
```

Verwenden Sie Folgendes, um *Ressourcen mit einem bestimmten Tag* abzurufen:

```powershell
(Get-AzureRmResource -Tag @{ Dept="Finance"}).Name
```

Verwenden Sie Folgendes, um *Ressourcen mit einem bestimmten Tagnamen* abzurufen:

```powershell
(Get-AzureRmResource -TagName Dept).Name
```

Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Daher müssen Sie Ihre Vorgehensweise darauf abstimmen, ob für die Ressource oder Ressourcengruppe bereits Tags vorhanden sind.

Verwenden Sie Folgendes, um einer *Ressourcengruppe ohne vorhandene Tags* Tags hinzuzufügen:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Rufen Sie die vorhandenen Tags ab, fügen Sie das neue Tag hinzu, und wenden Sie die Tags wieder an, um einer *Ressourcengruppe mit vorhandenen Tags* Tags hinzuzufügen:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

Verwenden Sie Folgendes, um einer *Ressource ohne vorhandene Tags* Tags hinzuzufügen:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

Verwenden Sie Folgendes, um einer *Ressource mit vorhandenen Tags* Tags hinzuzufügen:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.Tags.Add("Status", "Approved") 
Set-AzureRmResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für die Ressourcen nicht beizubehalten*:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups)
{
    Get-AzureRmResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für Ressourcen beizubehalten, die keine Duplikate sind*:

```powershell
$group = Get-AzureRmResourceGroup "examplegroup"
if ($group.Tags -ne $null) {
    $resources = Get-AzureRmResource -ResourceGroupName $group.ResourceGroupName
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzureRmResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Durch Übergeben einer leeren Hashtabelle können Sie alle Tags löschen:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressourcengruppe* anzuzeigen:

```azurecli
az group show -n examplegroup --query tags
```

Das Skript gibt das folgende Format zurück:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

Oder verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einem angegebenen Namen, Typen und einer Ressourcengruppe* anzuzeigen:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Beim Durchlaufen einer Ressourcensammlung können Sie die Ressourcen nach Ressourcen-ID anzeigen. Ein vollständiges Beispiel finden Sie weiter unten in diesem Artikel. Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einer angegebenen Ressourcen-ID* anzuzeigen:

```azurecli
az resource show --id <resource-id> --query tags
```

Verwenden Sie `az group list`, um Ressourcengruppen mit einem bestimmten Tag abzurufen:

```azurecli
az group list --tag Dept=IT
```

Zum Abrufen aller Ressourcen mit einem bestimmten Tag und Wert verwenden Sie `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Daher müssen Sie Ihre Vorgehensweise darauf abstimmen, ob für die Ressource oder Ressourcengruppe bereits Tags vorhanden sind.

Verwenden Sie Folgendes, um einer *Ressourcengruppe ohne vorhandene Tags* Tags hinzuzufügen:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

Verwenden Sie Folgendes, um einer *Ressource ohne vorhandene Tags* Tags hinzuzufügen:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Zum Hinzufügen von Tags zu einer Ressource, die bereits Tags aufweist, rufen Sie die vorhandenen Tags ab, formatieren den Wert neu und wenden die vorhandenen und neuen Tags erneut an: 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für die Ressourcen nicht beizubehalten*:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    az resource tag --tags $t --id $resid
  done
done
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für Ressourcen beizubehalten*:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done
done
```

## <a name="templates"></a>Vorlagen

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>REST-API

Das Azure-Portal und PowerShell verwenden im Hintergrund die [Resource Manager-REST-API](https://docs.microsoft.com/rest/api/resources/). Wenn Sie das Tagging in eine andere Umgebung integrieren müssen, können Sie Tags mit **GET** für die Ressourcen-ID abrufen und die Tags mit einem **PATCH**-Aufruf aktualisieren.

## <a name="tags-and-billing"></a>Tags und Abrechnung

Abrechnungsdaten können mithilfe von Tags gruppiert werden. Beispiel: Wenn Sie mehrere virtuelle Computer für verschiedene Organisationen betreiben, gruppieren Sie die Nutzung mithilfe von Tags nach Kostenstelle. Mit Tags können Sie auch Kosten nach Runtimeumgebung kategorisieren, beispielsweise zur Abrechnung der Nutzung virtueller Computer in der Produktionsumgebung.

Informationen zu Tags können Sie über die [Azure-Ressourcennutzungs- und RateCard-APIs](../billing/billing-usage-rate-card-overview.md) oder aus der Nutzungsdatei im CSV-Format abrufen. Sie laden die Nutzungsdatei aus dem [Azure-Kontoportal](https://account.windowsazure.com/) oder dem [EA-Portal](https://ea.azure.com) herunter. Weitere Informationen zum programmgesteuerten Zugriff auf Abrechnungsinformationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](../billing/billing-usage-rate-card-overview.md). Hinweise zu REST-API-Vorgängen finden Sie unter [Azure Billing REST API Reference (Preview)](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)(in englischer Sprache).

Wenn Sie die CSV-Nutzungsdatei für Dienste herunterladen, die die Verwendung von Tags für die Abrechnung unterstützen, sind die Tags in der Spalte **Tags** enthalten. Weitere Informationen finden Sie unter [Informationen zu Ihrer Rechnung für Microsoft Azure](../billing/billing-understand-your-bill.md).

![Anzeigen von Tags in der Abrechnung](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Nächste Schritte

* Sie können mithilfe benutzerdefinierter Richtlinien Einschränkungen und Konventionen für Ihr Abonnement festlegen. Bei einer von Ihnen definierten Richtlinie kann es erforderlich sein, dass alle Ressourcen über einen Wert für ein bestimmtes Tag verfügen. Weitere Informationen finden Sie unter [Was ist Azure Policy?](../azure-policy/azure-policy-introduction.md)
* Eine Einführung zur Verwendung von Azure PowerShell beim Bereitstellen von Ressourcen finden Sie unter [Verwenden von Windows PowerShell mit dem Azure Resource Manager](powershell-azure-resource-manager.md).
* Eine Einführung zur Verwendung der Azure CLI beim Bereitstellen von Ressourcen finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit dem Azure Resource Manager](xplat-cli-azure-resource-manager.md).
* Eine Einführung zum Verwenden des Portals finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](resource-group-portal.md).  
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).
