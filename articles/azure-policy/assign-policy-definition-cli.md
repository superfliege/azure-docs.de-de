---
title: Verwenden der Azure-Befehlszeilenschnittstelle für die Erstellung einer Richtlinienzuweisung zum Identifizieren nicht kompatibler Ressourcen in Ihrer Azure-Umgebung | Microsoft-Dokumentation
description: Erstellen Sie mithilfe von PowerShell eine Azure Policy-Zuweisung zum Identifizieren nicht kompatibler Ressourcen.
services: azure-policy
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/03/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 9c0abc96b074f6226ae8153237c5e96b8b0dec0b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Erstellen einer Richtlinienzuweisung zum Identifizieren nicht kompatibler Ressourcen in Ihrer Azure-Umgebung mit der Azure-Befehlszeilenschnittstelle

Zum Verständnis der Konformität in Azure müssen Sie zunächst wissen, wie Sie den Status Ihrer Ressourcen ermitteln. Diese Schnellstartanleitung führt Sie schrittweise durch die Erstellung einer Richtlinienzuweisung zur Identifizierung von virtuellen Computern, die keine verwalteten Datenträger verwenden.

Am Ende dieses Prozesses können Sie erfolgreich virtuelle Computer identifizieren, die keine verwalteten Datenträger verwenden. Sie sind mit der Richtlinienzuweisung *nicht konform*.

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Anleitung wird die Azure-Befehlszeilenschnittstelle verwendet, um eine Richtlinienzuweisung zu erstellen und nicht konforme Ressourcen in Ihrer Azure-Umgebung zu identifizieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Für diese Schnellstartanleitung ist es erforderlich, Version 2.0.4 oder höher der Azure CLI auszuführen, um die CLI lokal zu installieren und zu verwenden. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="prerequisites"></a>Voraussetzungen

Registrieren Sie den Ressourcenanbieter Policy Insights mithilfe der Azure-Befehlszeilenschnittstelle. Durch die Registrierung des Ressourcenanbieters wird sichergestellt, dass das Abonnement mit ihm verwendet werden kann. Um einen Ressourcenanbieter zu registrieren, benötigen Sie die Berechtigungen zum Ausführen des Vorgangs „Aktion registrieren“ für den Ressourcenanbieter. Dieser Vorgang ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten. Führen Sie den folgenden Befehl aus, um den Ressourcenanbieter zu registrieren:

```
az provider register –-namespace 'Microsoft.PolicyInsights'
```
Weitere Informationen zum Registrieren und Anzeigen von Ressourcenanbietern finden Sie unter [Ressourcenanbieter und -typen](../azure-resource-manager/resource-manager-supported-services.md).


## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

In dieser Schnellstartanleitung erstellen Sie eine Richtlinienzuweisung und weisen die Definition „Audit Virtual Machines without Managed Disks“ (Virtuelle Computer ohne verwaltete Datenträger überwachen) zu. Mit dieser Richtliniendefinition werden Ressourcen identifiziert, die die in der Richtliniendefinition festgelegten Bedingungen nicht erfüllen.

Führen Sie den folgenden Befehl aus, um eine Richtlinienzuweisung zu erstellen:

```
az policy assignment create --name 'Audit Virtual Machines without Managed Disks Assignment' --scope '<scope>' --policy '<policy definition ID>' --sku 'standard'
```

In dem Befehl werden folgende Informationen verwendet:

- **Name:** Der Anzeigename für die Richtlinienzuweisung. Verwenden Sie in diesem Fall *Audit Virtual Machines without Managed Disks Assignment* (Zuweisung für die Überwachung virtueller Computer ohne verwaltete Datenträger).
- **Richtlinie:** Die Richtliniendefinitions-ID, auf deren Grundlage Sie die Zuweisung erstellen. In diesem Fall ist es die Richtliniendefinition: *Audit Virtual Machines without Managed Disks* (Virtuelle Computer ohne verwaltete Datenträger überwachen). Führen Sie den folgenden Befehl aus, um die Richtliniendefinitions-ID abzurufen: `az policy definition show --name 'Audit Virtual Machines without Managed Disks Assignment'`
- **Bereich:** Ein Bereich bestimmt, für welche Ressourcen oder Ressourcengruppe die Richtlinienzuweisung erzwungen wird. Er kann von einem Abonnement bis zu Ressourcengruppen reichen. Ersetzen Sie &lt;scope&gt; durch den Namen Ihrer Ressourcengruppe.
- **SKU:** Dieser Befehl erstellt eine Richtlinienzuweisung mit dem Standard-Tarif. Der Standard-Tarif ermöglicht eine skalierbare Verwaltung, Compliancebewertung und Problembehandlung. Weitere Details zu Tarifen finden Sie auf der [Preisseite für Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy).


## <a name="identify-non-compliant-resources"></a>Identifizieren nicht konformer Ressourcen

Führen Sie zum Anzeigen der Ressourcen, die unter dieser Zuordnung nicht konform sind, die folgenden Befehle aus, um die Richtlinienzuweisungs-ID abzurufen:

```
$policyAssignment = Get-AzureRmPolicyAssignment | where {$_.properties.displayName -eq "Audit Virtual Machines without Managed Disks"}
```

```
$policyAssignment.PolicyAssignmentId
```

Weitere Informationen zu Richtlinienzuweisungs-IDs finden Sie unter [Get-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment).

Führen Sie als Nächstes den folgenden Befehl aus, um die Ressourcen-IDs der nicht konformen Ressourcen abzurufen, die in einer JSON-Datei ausgegeben werden:

```
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

Ihre Ergebnisse sollten in etwa wie im folgenden Beispiel aussehen:

```
{
"@odata.context":"https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
"@odata.count": 3,
"value": [
{
    "@odata.id": null,
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
      "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachineId>"
    },
    {
      "@odata.id": null,
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
      "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine2Id>"
         },
{
      "@odata.id": null,
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
      "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3ID>"
         }

]
}

```

Die Ergebnisse sind vergleichbar mit dem, was üblicherweise in der Azure-Portalansicht unter **Nicht konforme Ressourcen** zu sehen ist.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Anleitungen in dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie mit weiteren Tutorials fortfahren möchten, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, löschen Sie die erstellte Zuweisung mithilfe des folgenden Befehls:

```azurecli
az policy assignment delete –name Audit Virtual Machines without Managed Disks Assignment --scope /subscriptions/ <subscriptionID> / <resourceGroupName>
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine Richtliniendefinition zum Identifizieren nicht kompatibler Ressourcen in Ihrer Azure-Umgebung zugewiesen.

Wenn Sie mehr über das Zuweisen von Richtlinien erfahren und sicherstellen möchten, dass **zukünftige** von Ihnen erstellte Ressourcen konform sind, fahren Sie mit dem folgenden Tutorial fort:

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Richtlinien](./create-manage-policy.md)
