---
title: Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung
description: In diesem Artikel erfahren Sie, wie Sie eine Richtliniendefinition zur Identifizierung nicht konformer Ressourcen erstellen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 9b1cfd762d6f050994688d1cd6afd91f0e43bfc3
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195776"
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung

Zum Verständnis der Konformität in Azure müssen Sie zunächst wissen, wie Sie den Status Ihrer Ressourcen ermitteln. Diese Schnellstartanleitung führt Sie schrittweise durch die Erstellung einer Richtlinienzuweisung zur Identifizierung von virtuellen Computern, die keine verwalteten Datenträger verwenden.

Am Ende dieses Prozesses haben Sie erfolgreich die virtuellen Computer identifiziert, die keine verwalteten Datenträger verwenden. Sie sind mit der Richtlinienzuweisung *nicht konform*.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

In dieser Schnellstartanleitung erstellen Sie eine Richtlinienzuweisung und weisen die Richtliniendefinition *Audit Virtual Machines without Managed Disks* (Virtuelle Computer ohne verwaltete Datenträger überwachen) zu.

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Richtlinie** suchen und die entsprechende Option auswählen.

   ![Suchen nach „Policy“](media/assign-policy-definition/search-policy.png)

2. Wählen Sie links auf der Seite „Azure Policy“ die Option **Zuweisungen**. Eine Zuweisung ist eine zugewiesene Richtlinie, die innerhalb eines bestimmten Bereichs angewendet werden soll.
3. Wählen Sie im oberen Bereich der Seite **Richtlinien – Zuweisungen** die Option **Richtlinie zuweisen**.

   ![Zuweisen einer Richtliniendefinition](media/assign-policy-definition/select-assign-policy.png)

4. Wählen Sie auf der Seite **Richtlinie zuweisen** den **Bereich** aus, indem Sie auf die Auslassungspunkte klicken und ein Abonnement (erforderlich) und eine Ressourcengruppe (optional) auswählen. Ein Bereich bestimmt, für welche Ressourcen oder Ressourcengruppe die Richtlinienzuweisung erzwungen wird.  Klicken Sie dann unten im Abschnitt der Seite **Bereich** auf **Auswählen**.

   Dieses Beispiel verwendet das **Contoso-Abonnement**. Ihr Abonnement wird sich davon unterscheiden.

5. Wenn Sie eine oder mehrere Ressourcengruppen (wenn Sie nur ein Abonnement erfasst haben) oder bestimmte Ressourcen innerhalb einer Ressourcengruppe (bei der Bereichswahl) ausschließen möchten, können Sie **Ausschlüsse** von der Richtlinienzuweisung konfigurieren. Lassen Sie dieses Feld im Moment leer.

6. Wählen Sie die **Richtliniendefinition** mit den Auslassungspunkten, um die Liste der verfügbaren Definitionen zu öffnen. Azure Policy umfasst bereits integrierte Richtliniendefinitionen, die Sie verwenden können. Es sind viele integrierte Richtliniendefinitionen verfügbar, wie zum Beispiel:

   - Enforce tag and its value (Tag und zugehörigen Wert erzwingen)
   - Apply tag and its value (Tag und zugehörigen Wert anwenden)
   - Require SQL Server version 12.0 (SQL Server-Version 12.0 fordern)

    Eine vollständige Liste der verfügbaren integrierten Richtlinien finden Sie unter [Richtlinienvorlagen](json-samples.md).

7. Durchsuchen Sie die Liste der Richtliniendefinitionen nach der Definition *Überwachen Sie die virtuellen Computer, die nicht verwaltete Datenträger verwenden*. Klicken Sie auf diese Richtlinie und anschließend auf **Auswählen**.

   ![Suchen der korrekten Richtliniendefinition](media/assign-policy-definition/select-available-definition.png)

8. Der **Zuweisungsname** wird automatisch mit dem ausgewählten Richtliniennamen gefüllt, kann aber geändert werden. In diesem Fall wird *Überwachen Sie die virtuellen Computer, die nicht verwaltete Datenträger verwenden* übernommen. Geben Sie ggf. auch eine **Beschreibung** ein. Die Beschreibung enthält Details zu dieser Richtlinienzuweisung.

9. Klicken Sie auf **Zuweisen**.

Sie können nun nicht konforme Ressourcen identifizieren, um den Konformitätszustand Ihrer Umgebung nachzuvollziehen.

## <a name="identify-non-compliant-resources"></a>Identifizieren nicht konformer Ressourcen

Wählen Sie links auf der Seite „Azure Policy“ **Konformität**, und suchen Sie die von Ihnen erstellte Richtlinienzuweisung **Überwachen Sie die virtuellen Computer, die nicht verwaltete Datenträger verwenden**.

![Richtlinienkonformität](media/assign-policy-definition/policy-compliance.png)

Falls Ressourcen vorhanden sind, die mit dieser neuen Zuweisung nicht konform sind, werden diese unter **Nicht konforme Ressourcen** angezeigt.

Wenn eine Bedingung für einige Ihrer vorhandenen Ressourcen als erfüllt ausgewertet wird, werden diese Ressourcen als nicht mit der Richtlinie konform markiert. Die folgende Tabelle gibt Aufschluss über das Zusammenspiel zwischen den verschiedenen Richtlinienauswirkungen, der Bedingungsauswertung und dem resultierenden Konformitätszustand. Die Auswertungslogik wird zwar im Azure-Portal nicht angezeigt, Sie sehen aber die Ergebnisse für den Konformitätszustand. Das Ergebnis für den Konformitätszustand ist entweder „konform“ oder „nicht konform“.

| **Ressourcenzustand** | **Auswirkung** | **Richtlinienauswertung** | **Konformitätszustand** |
| --- | --- | --- | --- |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Nicht konform |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Konform |
| Neu | Audit, AuditIfNotExist\* | True | Nicht konform |
| Neu | Audit, AuditIfNotExist\* | False | Konform |

\* Für die Auswirkungen „Append“, „DeployIfNotExist“ und „AuditIfNotExist“ muss die IF-Anweisung auf TRUE festgelegt sein. Für die Auswirkungen muss die Existenzbedingung außerdem auf FALSE festgelegt sein, damit sie nicht konform sind. Bei TRUE löst die IF-Bedingung die Auswertung der Existenzbedingung für die zugehörigen Ressourcen aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Leitfäden in dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie mit den nachfolgenden Tutorials fortfahren möchten, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

1. Wählen Sie links auf der Seite „Azure Policy“ **Konformität** (oder **Zuweisungen**), und suchen Sie die von Ihnen erstellte Richtlinienzuweisung **Überwachen Sie die virtuellen Computer, die nicht verwaltete Datenträger verwenden**.

2. Klicken Sie mit der rechten Maustaste auf die Richtlinienzuweisung **Überwachen Sie die virtuellen Computer, die nicht verwaltete Datenträger verwenden**, und wählen Sie **Zuweisung löschen**.

   ![Löschen einer Zuweisung](media/assign-policy-definition/delete-assignment.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einem Bereich eine Richtliniendefinition zugewiesen und deren Konformitätsbericht ausgewertet. Die Richtliniendefinition stellt sicher, dass alle Ressourcen im Bereich konform sind, und identifiziert die Ressourcen, die nicht konform sind.

Weitere Informationen zum Zuweisen von Richtlinien, die die Konformität **zukünftig** erstellter Ressourcen sicherstellen, finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Richtlinien](create-manage-policy.md)