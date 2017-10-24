---
title: Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung mithilfe von PowerShell | Microsoft-Dokumentation
description: Erstellen Sie mithilfe von PowerShell eine Azure Policy-Zuweisung zum Identifizieren nicht konformer Ressourcen.
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 10/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 3f9ef7886af20845eddc4c1e71d60911e4b22eca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung mithilfe von PowerShell

Zum Verständnis der Konformität in Azure müssen Sie zunächst wissen, wo Sie mit Ihren aktuellen Ressourcen stehen. Diese Schnellstartanleitung führt Sie schrittweise durch die Erstellung einer Richtlinienzuweisung zur Identifizierung nicht konformer Ressourcen mit der Richtliniendefinition *Require SQL Server version 12.0* (SQL Server-Version 12.0 fordern). Am Ende dieses Prozesses haben Sie erfolgreich identifiziert, welche Server eine andere Version besitzen und somit nicht konform sind.

PowerShell dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In diesem Leitfaden erfahren Sie, wie Sie mithilfe von PowerShell eine Richtlinienzuweisung zur Identifizierung nicht konformer Ressourcen in Ihrer Azure-Umgebung erstellen.

Für diesen Leitfaden benötigen Sie mindestens Version 4.0 des Azure PowerShell-Moduls. Führen Sie  ```Get-Module -ListAvailable AzureRM```  aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

Stellen Sie vor Beginn sicher, dass die aktuelle Version von PowerShell installiert ist. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="opt-in-to-azure-policy"></a>Registrieren für Azure Policy

Azure Policy ist jetzt in der eingeschränkten Vorschau verfügbar und kann erst nach vorheriger Registrierung verwendet werden.

1. Navigieren Sie zu Azure Policy (https://aka.ms/getpolicy), und klicken Sie im linken Bereich auf **Registrieren**.

   ![Suchen nach „Policy“](media/assign-policy-definition/sign-up.png)

2. Registrieren Sie sich für Azure Policy, indem Sie in der Abonnementliste**** die gewünschten Abonnements auswählen. Klicken Sie anschließend auf **Register** (Registrieren).

   ![Registrieren für die Verwendung von Azure Policy](media/assign-policy-definition/preview-opt-in.png)

   Bei hoher Nachfrage kann es mehrere Tage dauern, bis wir Ihre Registrierungsanforderung akzeptieren. Wenn Ihre Anforderung akzeptiert wurde, werden Sie per E-Mail darüber informiert, dass Sie den Dienst nun verwenden können.

## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

In dieser Schnellstartanleitung erstellen wir eine Richtlinienzuweisung und weisen die Definition *Require SQL Server version 12.0* (SQL Server-Version 12.0 fordern) zu. Mit dieser Richtliniendefinition werden Ressourcen identifiziert, die die in der Richtliniendefinition festgelegten Bedingungen nicht erfüllen.

Führen Sie die folgenden Schritte aus, um eine neue Richtlinienzuweisung zu erstellen.

Führen Sie den folgenden Befehl aus, um alle Richtliniendefinitionen anzuzeigen und die Richtliniendefinition zu suchen, die Sie zuweisen möchten:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

Azure Policy umfasst bereits integrierte Richtliniendefinitionen, die Sie verwenden können. Ihnen werden integrierte Richtliniendefinitionen wie die folgenden angezeigt:

- Enforce tag and its value (Tag und zugehörigen Wert erzwingen)
- Apply tag and its value (Tag und zugehörigen Wert anwenden)
- Require SQL Server version 12.0 (SQL Server-Version 12.0 fordern)

Wenden Sie als Nächstes mithilfe des Cmdlets `New-AzureRmPolicyAssignment` die Richtliniendefinition auf den gewünschten Bereich an.

In diesem Tutorial geben wir für den Befehl folgende Informationen an:
- Den Anzeigenamen**** für die Richtlinienzuweisung. In diesem Beispiel verwenden wir „Require SQL Server version 12.0 Assignment“ (Zuweisung der SQL Server-Version 12.0 erforderlich).
- **Richtlinie**: Die Richtliniendefinition, auf deren Grundlage Sie die Zuweisung erstellen. In diesem Beispiel verwenden wir die Richtliniendefinition *Require SQL Server version 12.0* (SQL Server-Version 12.0 fordern).
- **Bereich**: Ein Bereich bestimmt, für welche Ressourcen oder Ressourcengruppe die Richtlinienzuweisung erzwungen wird. Er kann von einem Abonnement bis zu Ressourcengruppen reichen. In diesem Beispiel weisen wir die Richtliniendefinition der Ressourcengruppe **FabrikamOMS** zu.
- **$definition**: Geben Sie die Ressourcen-ID der Richtliniendefinition an. In unserem Beispiel verwenden wir die ID für die Richtliniendefinition: *Require SQL Server version 12.0* (SQL Server-Version 12.0 fordern).

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Require SQL Server version 12.0 Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Sie können nun nicht konforme Ressourcen identifizieren, um den Konformitätszustand Ihrer Umgebung nachzuvollziehen.

## <a name="identify-non-compliant-resources"></a>Identifizieren nicht konformer Ressourcen

1. Navigieren Sie zurück zur Startseite von Azure Policy.
2. Klicken Sie im linken Bereich auf **Konformität**, und suchen Sie nach der von Ihnen erstellten **Richtlinienzuweisung**.

   ![Richtlinienkonformität](media/assign-policy-definition/policy-compliance.png)

   Falls Ressourcen vorhanden sind, die mit dieser neuen Zuweisung nicht konform sind, werden diese wie oben dargestellt auf der Registerkarte **Non-compliant resources** (Nicht konforme Ressourcen) angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Leitfäden in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Wenn Sie mit weiteren Tutorials fortfahren möchten, überspringen Sie die Bereinigung der Ressourcen, die im Rahmen dieser Schnellstartanleitung erstellt wurden. Wenn Sie nicht fortfahren möchten, löschen Sie die erstellte Zuweisung mit dem folgenden Befehl:

```powershell
Remove-AzureRmPolicyAssignment -Name “Require SQL Server version 12.0 Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Richtliniendefinition zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung zugewiesen.

Weitere Informationen zum Zuweisen von Richtlinien, die die Konformität **zukünftig** erstellter Ressourcen sicherstellen, finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Richtlinien](./create-manage-policy.md)