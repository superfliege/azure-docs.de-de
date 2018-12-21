---
title: 'Beispiel: Richtlinieninitiative zur Abrechnung von Tags'
description: Diese Beispielrichtlinie erfordert angegebene Tagwerte für die Kostenstelle und den Produktnamen.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 09/18/2018
ms.author: dacoulte
ms.openlocfilehash: 1929cb7287460557cf815b62bd5d31ffe38ab161
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317774"
---
# <a name="billing-tags-policy-initiative"></a>Richtlinieninitiative zur Abrechnung von Tags

Diese festgelegte Richtlinie erfordert angegebene Tagwerte für die Kostenstelle und den Produktnamen. Verwendet integrierte Richtlinien, um erforderliche Tags anzuwenden und zu erzwingen. Sie geben die erforderlichen Werte für die Tags an.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Vorlagenbeispiel

[!code-json[main](../../../../policy-templates/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.json "Billing Tags Policy Initiative")]

Sie können diese Vorlage über das [Azure-Portal](#deploy-with-the-portal) oder mit [PowerShell](#deploy-with-powershell) bereitstellen.

## <a name="deploy-with-the-portal"></a>Bereitstellen über das Portal

[![In Azure bereitstellen](http://azuredeploy.net/deploybutton.png)](https://aka.ms/getpolicy)

## <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.parameters.json"

$policyset= New-AzureRmPolicySetDefinition -Name "multiple-billing-tags" -DisplayName "Billing Tags Policy Initiative" -Description "Specify cost Center tag and product name tag" -PolicyDefinition $policydefinitions -Parameter $policysetparameters

New-AzureRmPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentname> -Scope <scope>  -costCenterValue <required value for Cost Center tag> -productNameValue <required value for product Name tag>
```

### <a name="clean-up-powershell-deployment"></a>Bereinigen der PowerShell-Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="apply-tags-to-existing-resources"></a>Anwenden von Tags auf vorhandene Ressourcen

Nach dem Zuweisen der Richtlinien können Sie ein Update für alle vorhandenen Ressourcen auslösen, um die hinzugefügten Tagrichtlinien zu erzwingen. Das folgende Skript behält alle anderen Tags bei, die für die Ressourcen vorhanden waren:

```azurepowershell-interactive
$resources = Get-AzureRmResource -ResourceGroupName 'ExampleGroup'

foreach ($r in $resources) {
    try {
        Set-AzureRmResource -Tags ($a = if ($r.Tags -eq $NULL) { @{} } else {$r.Tags}) -ResourceId $r.ResourceId -Force -UsePatchSemantics
    }
    catch {
        Write-Host $r.ResourceId + "can't be updated"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Unter [Azure Policy-Beispiele](index.md) finden Sie weitere Beispiele.