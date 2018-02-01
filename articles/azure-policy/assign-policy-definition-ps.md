---
title: Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung mithilfe von PowerShell | Microsoft-Dokumentation
description: Erstellen Sie mithilfe von PowerShell eine Azure Policy-Zuweisung zum Identifizieren nicht konformer Ressourcen.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 1/17/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 67c779b96dab088d810d22ad3053ade106aec56a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung mithilfe von PowerShell

Zum Verständnis der Konformität in Azure müssen Sie zunächst wissen, wie Sie den Status Ihrer Ressourcen ermitteln. Diese Schnellstartanleitung führt Sie schrittweise durch die Erstellung einer Richtlinienzuweisung zur Identifizierung von virtuellen Computern, die keine verwalteten Datenträger verwenden.

Am Ende dieses Prozesses haben Sie erfolgreich die virtuellen Computer identifiziert, die keine verwalteten Datenträger verwenden. Sie sind mit der Richtlinienzuweisung *nicht konform*.

PowerShell dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In diesem Leitfaden erfahren Sie, wie Sie mithilfe von PowerShell eine Richtlinienzuweisung zur Identifizierung nicht konformer Ressourcen in Ihrer Azure-Umgebung erstellen.

Für diesen Leitfaden benötigen Sie mindestens Version 4.0 des Azure PowerShell-Moduls. Führen Sie  `Get-Module -ListAvailable AzureRM`  aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

Stellen Sie vor Beginn sicher, dass die aktuelle Version von PowerShell installiert ist. Ausführliche Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.


## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

In dieser Schnellstartanleitung erstellen Sie eine Richtlinienzuweisung und weisen die Definition *Audit Virtual Machines without Managed Disks* (Virtuelle Computer ohne verwaltete Datenträger überwachen) zu. Mit dieser Richtliniendefinition werden Ressourcen identifiziert, die die in der Richtliniendefinition festgelegten Bedingungen nicht erfüllen.

Führen Sie die folgenden Schritte aus, um eine neue Richtlinienzuweisung zu erstellen.

1. Registrieren Sie den Ressourcenanbieter „Policy Insights“, um sicherzustellen, dass Ihr Abonnement für den Ressourcenanbieter funktioniert. Um einen Ressourcenanbieter zu registrieren, benötigen Sie die Berechtigungen zum Ausführen des Vorgangs „Aktion registrieren“ für den Ressourcenanbieter. Dieser Vorgang ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten.

    Führen Sie den folgenden Befehl aus, um den Ressourcenanbieter zu registrieren:

    ```
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
```

    Sie können die Registrierung eines Ressourcenanbieters nicht aufheben, wenn in Ihrem Abonnement Ressourcentypen dieses Ressourcenanbieters vorhanden sind.

    Weitere Informationen zum Registrieren und Anzeigen von Ressourcenanbietern finden Sie unter [Ressourcenanbieter und -typen](../azure-resource-manager/resource-manager-supported-services.md).

2. Führen Sie nach dem Registrieren des Ressourcenanbieters den folgenden Befehl aus, um alle Richtliniendefinitionen anzuzeigen und diejenige zu ermitteln, die Sie zuweisen möchten:

    ```powershell
$definition = Get-AzureRmPolicyDefinition
```

    Azure Policy umfasst bereits integrierte Richtliniendefinitionen, die Sie verwenden können. Ihnen werden integrierte Richtliniendefinitionen wie die folgenden angezeigt:

    - Enforce tag and its value (Tag und zugehörigen Wert erzwingen)
    - Apply tag and its value (Tag und zugehörigen Wert anwenden)
    - Require SQL Server version 12.0 (SQL Server-Version 12.0 fordern)

3. Wenden Sie als Nächstes mithilfe des Cmdlets `New-AzureRmPolicyAssignment` die Richtliniendefinition auf den gewünschten Bereich an.

Verwenden Sie für den Befehl in diesem Tutorial die folgenden Informationen:

- **Anzeigename** für die Richtlinienzuweisung. Verwenden Sie in diesem Fall „Audit Virtual Machines without Managed Disks“ (Virtuelle Computer ohne verwaltete Datenträger überwachen).
- **Richtlinie**: Die Richtliniendefinition, auf deren Grundlage Sie die Zuweisung erstellen. In diesem Fall ist es die Richtliniendefinition: *Audit Virtual Machines without Managed Disks* (Virtuelle Computer ohne verwaltete Datenträger überwachen).
- **Bereich**: Ein Bereich bestimmt, für welche Ressourcen oder Ressourcengruppe die Richtlinienzuweisung erzwungen wird. Er kann von einem Abonnement bis zu Ressourcengruppen reichen. In diesem Beispiel weisen Sie die Richtliniendefinition der Ressourcengruppe **FabrikamOMS** zu.
- **$definition**: Geben Sie die Ressourcen-ID der Richtliniendefinition an. In diesem Beispiel verwenden Sie die ID für die Richtliniendefinition: *Audit Virtual Machines without Managed Disks* (Virtuelle Computer ohne verwaltete Datenträger überwachen).

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Sie können nun nicht konforme Ressourcen identifizieren, um den Konformitätszustand Ihrer Umgebung nachzuvollziehen.

## <a name="identify-non-compliant-resources"></a>Identifizieren nicht konformer Ressourcen

1. Navigieren Sie zurück zur Startseite von Azure Policy.
2. Klicken Sie im linken Bereich auf **Konformität**, und suchen Sie nach der von Ihnen erstellten **Richtlinienzuweisung**.

   ![Richtlinienkonformität](media/assign-policy-definition/policy-compliance.png)

   Falls Ressourcen vorhanden sind, die mit dieser neuen Zuweisung nicht konform sind, werden diese unter der Registerkarte **Nicht konforme Ressourcen** angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Anleitungen in dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie mit den nachfolgenden Tutorials fortfahren möchten, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Wenn Sie nicht fortfahren möchten, löschen Sie die erstellte Zuweisung mit dem folgenden Befehl:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine Richtliniendefinition zum Identifizieren nicht kompatibler Ressourcen in Ihrer Azure-Umgebung zugewiesen.

Weitere Informationen zum Zuweisen von Richtlinien, die die Konformität **zukünftig** erstellter Ressourcen sicherstellen, finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Richtlinien](./create-manage-policy.md)
