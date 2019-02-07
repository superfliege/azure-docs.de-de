---
title: Fehler „RequestDisallowedByPolicy“ bei Azure-Ressourcenrichtlinien | Microsoft-Dokumentation
description: Beschreibt die Ursache des Fehlers „RequestDisallowedByPolicy“.
services: azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 2d55d5958568deef5ce350c9678e55e2e37a2ff8
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496396"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Fehler „RequestDisallowedByPolicy“ bei Azure-Ressourcenrichtlinien

Dieser Artikel beschreibt die Ursache des Fehlers „RequestDisallowedByPolicy“ und bietet eine Lösung für diesen Fehler.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="symptom"></a>Symptom

Während der Bereitstellung erhalten Sie ggf. den Fehler **RequestDisallowedByPolicy**, der die Erstellung der Ressourcen verhindert. Das folgende Beispiel zeigt den Fehler:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Problembehandlung

Verwenden Sie zum Abrufen von Details zur Richtlinie, die die Bereitstellung blockiert hat, eine der folgenden Methoden:

### <a name="powershell"></a>PowerShell

Geben Sie in PowerShell diese Richtlinienkennung als `Id`-Parameter an, um Details zur Richtlinie abzurufen, die Ihre Bereitstellung blockiert.

```PowerShell
(Get-AzPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Geben Sie in der Azure CLI den Namen der Richtliniendefinition an:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Lösung

Ihre Abonnementadministratoren können zu Sicherheits- oder Compliancezwecken Richtlinien zuweisen, mit denen eingeschränkt wird, wie Ressourcen bereitgestellt werden. Beispielsweise kann Ihr Abonnement über eine Richtlinie verfügen, mit der die Erstellung von öffentlichen IP-Adressen, Netzwerksicherheitsgruppen, benutzerdefinierten Routen oder Routingtabellen verhindert wird. Die Fehlermeldung im Abschnitt **Symptome** enthält den Namen der Richtlinie.
Sehen Sie sich zum Beheben dieses Problems die Ressourcenrichtlinien an, und ermitteln Sie, wie Sie Ressourcen bereitstellen, die diese Richtlinien einhalten.

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Was ist Azure Policy?](../azure-policy/azure-policy-introduction.md)
- [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../azure-policy/create-manage-policy.md)
