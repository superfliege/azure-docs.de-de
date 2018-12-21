---
title: 'Beispiel: Überwachen der transparenten Datenverschlüsselung für SQL-Datenbank'
description: Mit dieser Beispielrichtlinie wird überwacht, wenn die transparente Datenverschlüsselung für SQL-Datenbank nicht aktiviert ist.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 10/29/2018
ms.author: dacoulte
ms.openlocfilehash: 36eb62dfa7ae45b70f5907241c66f2286f9ce760
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310622"
---
# <a name="audit-sql-database-encryption"></a>Überwachen der SQL-Datenbankverschlüsselung

Mit dieser integrierten Richtlinie wird überwacht, wenn die transparente Datenverschlüsselung für SQL-Datenbank nicht aktiviert ist.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Vorlagenbeispiel

[!code-json[main](../../../../policy-templates/samples/SQL/audit-sql-db-tde-status/azurepolicy.json "Audit TDE for SQL Database")]

Sie können diese Vorlage über das [Azure-Portal](#deploy-with-the-portal), mit [PowerShell](#deploy-with-powershell) oder mit der [Azure CLI](#deploy-with-azure-cli) bereitstellen. Verwenden Sie zum Abrufen der integrierten Richtlinie die ID `17k78e20-9358-41c9-923c-fb736d382a12`.

## <a name="deploy-with-the-portal"></a>Bereitstellen über das Portal

Wählen Sie beim Zuweisen einer Richtlinie in den verfügbaren integrierten Definitionen die Option **Transparent Data Encryption-Status überwachen** aus.

## <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/17k78e20-9358-41c9-923c-fb736d382a12

New-AzureRmPolicyAssignment -name "SQL TDE Audit" -PolicyDefinition $definition -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>Bereinigen der PowerShell-Bereitstellung

Führen Sie den folgenden Befehl aus, um die Richtlinienzuweisung zu entfernen.

```azurepowershell-interactive
Remove-AzureRmPolicyAssignment -Name "SQL TDE Audit" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "SQL TDE Audit" --policy 17k78e20-9358-41c9-923c-fb736d382a12
```

### <a name="clean-up-azure-cli-deployment"></a>Bereinigen der Azure CLI-Bereitstellung

Führen Sie den folgenden Befehl aus, um die Richtlinienzuweisung zu entfernen.

```azurecli-interactive
az policy assignment delete --name "SQL TDE Audit" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- Unter [Azure Policy-Beispiele](index.md) finden Sie weitere Beispiele.