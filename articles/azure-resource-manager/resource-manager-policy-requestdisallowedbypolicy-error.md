---
title: "Fehler „RequestDisallowedByPolicy“ bei Azure-Ressourcenrichtlinien | Microsoft-Dokumentation"
description: "Beschreibt die Ursache des Fehlers „RequestDisallowedByPolicy“."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 07/12/2017
ms.author: genli
ms.openlocfilehash: 3b11dc6afac716ef391976093839547e8fd37a91
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Fehler „RequestDisallowedByPolicy“ bei Azure-Ressourcenrichtlinien

Dieser Artikel beschreibt die Ursache des Fehlers „RequestDisallowedByPolicy“ und bietet eine Lösung für diesen Fehler.

## <a name="symptom"></a>Symptom

Wenn Sie versuchen, während der Bereitstellung eine Aktion auszuführen, erhalten Sie möglicherweise den Fehler **RequestDisallowedByPolicy**, der verhindert, dass die Aktion abgeschlossen wird. Das folgende Beispiel zeigt den Fehler:

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

### <a name="method-1"></a>Methode 1

Geben Sie in PowerShell diese Richtlinienkennung als `Id`-Parameter an, um Details zur Richtlinie abzurufen, die Ihre Bereitstellung blockiert.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>Methode 2 

Geben Sie in Azure CLI 2.0 den Namen der Richtliniendefinition an: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Lösung

Für Sicherheit und Kompatibilität kann Ihre IT-Abteilung eine Ressourcenrichtlinie erzwingen, die das Erstellen von öffentlichen IP-Adressen, Netzwerksicherheitsgruppen, benutzerdefinierten Routen oder Routentabellen verhindert. Die Fehlermeldung im Abschnitt **Symptoms** zeigt eine Richtlinie mit dem Namen **regionPolicyDefinition** an. Ihre Richtlinie hat möglicherweise einen anderen Namen.
Zum Beheben dieses Problems arbeiten Sie mit Ihrer IT-Abteilung zusammen, um die Ressourcenrichtlinien zu überprüfen und zu bestimmen, wie die angeforderte Aktion in Übereinstimmung mit diesen Richtlinien ausgeführt werden kann.

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Übersicht über Ressourcenrichtlinien](resource-manager-policy.md)
- [Anzeigen der Richtlinienzuweisungen](resource-manager-policy-portal.md#view-policy-assignments)