---
title: Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Richtliniendefinition zur Identifizierung nicht konformer Ressourcen erstellen.
services: azure-policy
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/18/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: e5b27bdc2aef15b619022d1c08fa3e6dccaa5736
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung
Zum Verständnis der Konformität in Azure müssen Sie zunächst wissen, wie Sie den Status Ihrer Ressourcen ermitteln. Diese Schnellstartanleitung führt Sie schrittweise durch die Erstellung einer Richtlinienzuweisung zur Identifizierung von virtuellen Computern, die keine verwalteten Datenträger verwenden.

Am Ende dieses Prozesses haben Sie erfolgreich die virtuellen Computer identifiziert, die keine verwalteten Datenträger verwenden. Sie sind mit der Richtlinienzuweisung *nicht konform*.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

In dieser Schnellstartanleitung erstellen Sie eine Richtlinienzuweisung und weisen die Richtliniendefinition *Audit Virtual Machines without Managed Disks* (Virtuelle Computer ohne verwaltete Datenträger überwachen) zu.

1. Klicken Sie im linken Bereich der Seite „Azure Policy“ auf **Zuweisungen**.
2. Klicken Sie im oberen Bereich der Seite **Zuweisungen** auf **Richtlinie zuweisen**.

   ![Zuweisen einer Richtliniendefinition](media/assign-policy-definition/select-assign-policy.png)

3. Klicken Sie auf der Seite **Richtlinie zuweisen** neben dem Feld **Richtlinie** auf die Schaltfläche ![Richtliniendefinition](media/assign-policy-definition/definitions-button.png), um die Liste mit den verfügbaren Definitionen zu öffnen.

   ![Öffnen der verfügbaren Richtliniendefinitionen](media/assign-policy-definition/open-policy-definitions.png)

   Azure Policy umfasst bereits integrierte Richtliniendefinitionen, die Sie verwenden können. Ihnen werden integrierte Richtliniendefinitionen wie die folgenden angezeigt:

   - Enforce tag and its value (Tag und zugehörigen Wert erzwingen)
   - Apply tag and its value (Tag und zugehörigen Wert anwenden)
   - Require SQL Server version 12.0 (SQL Server-Version 12.0 fordern)

    Eine vollständige Liste der verfügbaren integrierten Richtlinien finden Sie unter [Richtlinienvorlagen](json-samples.md).

4. Durchsuchen Sie Ihre Richtliniendefinitionen nach der Definition *Audit VMs that do not use managed disks* (VMs überwachen, die keine verwalteten Datenträger verwenden). Klicken Sie auf diese Richtlinie und anschließend auf **Auswählen**.

   ![Suchen der korrekten Richtliniendefinition](media/assign-policy-definition/select-available-definition.png)

5. Geben Sie einen **Anzeigenamen** für die Richtlinienzuweisung an. In diesem Fall verwenden wir *Audit VMs that do not use managed disks* (VMs überwachen, die keine verwalteten Datenträger verwenden). Geben Sie ggf. auch eine **Beschreibung** ein. Die Beschreibung enthält Details dazu, wie durch die Richtlinienzuweisung alle virtuellen Computer identifiziert werden, die keine verwalteten Datenträger verwenden.
6. Ändern Sie den Tarif in **Standard**, um sicherzustellen, dass die Richtlinie auf bereits vorhandene Ressourcen angewendet wird.

   In Azure Policy stehen zwei Tarife zur Verfügung: *Free* und *Standard*. Mit dem Free-Tarif können Richtlinien nur für zukünftige Ressourcen erzwungen werden. Mit dem Standard-Tarif können Sie Richtlinien hingegen auch für bereits vorhandene Ressourcen erzwingen und Ihren Konformitätszustand besser nachvollziehen. Weitere Informationen zu Preisen finden Sie auf der [Preisseite für Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy/).

7. Wählen Sie den **Bereich** aus, auf den die Richtlinie angewendet werden soll.  Ein Bereich bestimmt, für welche Ressourcen oder Ressourcengruppe die Richtlinienzuweisung erzwungen wird. Er kann von einem Abonnement bis zu Ressourcengruppen reichen.
8. Wählen Sie das zuvor registrierte Abonnement (oder die Ressourcengruppe) aus. In diesem Beispiel wird das Abonnement **Azure Analytics Capacity Dev** verwendet. (Ihre Optionen werden sich davon unterscheiden.) Klicken Sie auf **Auswählen**.

   ![Suchen der korrekten Richtliniendefinition](media/assign-policy-definition/assign-policy.png)

9. Lassen Sie **Ausschlüsse** vorerst leer, und klicken Sie dann auf **Zuweisen**.

Sie können nun nicht konforme Ressourcen identifizieren, um den Konformitätszustand Ihrer Umgebung nachzuvollziehen.

## <a name="identify-non-compliant-resources"></a>Identifizieren nicht konformer Ressourcen

Klicken Sie im linken Bereich auf **Konformität**, und suchen Sie nach der von Ihnen erstellten Richtlinienzuweisung.

![Richtlinienkonformität](media/assign-policy-definition/policy-compliance.png)

Falls Ressourcen vorhanden sind, die mit dieser neuen Zuweisung nicht konform sind, werden diese unter **Nicht konforme Ressourcen** angezeigt.

Wenn eine Bedingung für einige Ihrer vorhandenen Ressourcen als erfüllt ausgewertet wird, werden diese Ressourcen als nicht mit der Richtlinie konform markiert. In der Beispielabbildung oben werden nicht konforme Ressourcen angezeigt. Die folgende Tabelle gibt Aufschluss über das Zusammenspiel zwischen den verschiedenen Richtlinienaktionen, der Bedingungsauswertung und dem resultierenden Konformitätszustand. Die Auswertungslogik wird zwar im Azure-Portal nicht angezeigt, Sie sehen aber die Ergebnisse für den Konformitätszustand. Das Ergebnis für den Konformitätszustand ist entweder „konform“ oder „nicht konform“.

| **Ressourcenzustand** | **Aktion** | **Richtlinienauswertung** | **Konformitätszustand** |
| --- | --- | --- | --- |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Nicht konform |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Konform |
| Neu | Audit, AuditIfNotExist\* | True | Nicht konform |
| Neu | Audit, AuditIfNotExist\* | False | Konform |

\* Für die Aktionen Append, DeployIfNotExist und AuditIfNotExist muss die IF-Anweisung auf TRUE festgelegt sein. Für die Aktionen muss die Existenzbedingung außerdem auf FALSE festgelegt sein, damit sie nicht konform sind. Bei TRUE löst die IF-Bedingung die Auswertung der Existenzbedingung für die zugehörigen Ressourcen aus.
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Leitfäden in dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie mit den nachfolgenden Tutorials fortfahren möchten, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.
1. Klicken Sie im linken Bereich auf **Zuweisungen**.
2. Suchen Sie nach der von Ihnen erstellten Zuordnung, und klicken Sie mit der rechten Maustaste darauf.

   ![Löschen einer Zuweisung](media/assign-policy-definition/delete-assignment.png)

3.  Klicken Sie auf **Zuweisung löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einem Bereich eine Richtliniendefinition zugewiesen. Die Richtliniendefinition stellt sicher, dass alle Ressourcen im Bereich konform sind, und identifiziert die Ressourcen, die nicht konform sind.

Weitere Informationen zum Zuweisen von Richtlinien, die die Konformität **zukünftig** erstellter Ressourcen sicherstellen, finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Richtlinien](./create-manage-policy.md)
