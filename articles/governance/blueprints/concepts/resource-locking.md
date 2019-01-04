---
title: Grundlagen von Ressourcensperren
description: Es wird beschrieben, wie Sie die Optionen für das Sperren verwenden, um beim Zuweisen einer Blaupause die Ressourcen zu schützen.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 0e272f7137967b545269a408b6e83552de532682
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309428"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Grundlegendes zur Ressourcensperre in Azure Blueprint

Die bedarfsabhängige Erstellung konsistenter Umgebungen hat nur dann wirklich Vorteile, wenn ein Mechanismus vorhanden ist, mit dem die Wahrung der Konsistenz sichergestellt werden kann. In diesem Artikel wird beschrieben, wie das Sperren von Ressourcen in Azure Blueprint funktioniert.

## <a name="locking-modes-and-states"></a>Modi und Zustände von Sperren

Der Sperrmodus gilt für die Blaupausenzuweisung und verfügt nur über zwei Optionen: **Keine** und **Alle Ressourcen**. Dieser Sperrmodus wird während der Blaupausenzuweisung konfiguriert und kann nicht mehr geändert werden, nachdem die Zuweisung erfolgreich auf das Abonnement angewendet wurde.

Für Ressourcen, die von Artefakten in einer Blaupausenzuweisung erstellt wurden, gibt es drei mögliche Zustände: **Nicht gesperrt**, **Schreibgeschützt** und **Bearbeitung/Löschen nicht möglich**. Jedes Artefakt kann sich im Zustand **Nicht gesperrt** befinden. Ressourcengruppenfremde Artefakte haben jedoch den Zustand **Schreibgeschützt** und Ressourcengruppen den Zustand **Bearbeitung/Löschen nicht möglich**. Dieser Unterschied muss bei der Verwaltung der Ressourcen berücksichtigt werden.

Der Zustand **Schreibgeschützt** ist klar definiert: Die Ressource kann nicht geändert oder gelöscht werden. Beim Zustand **Bearbeitung/Löschen nicht möglich** ist es nicht so eindeutig, weil Ressourcengruppen praktisch „Container“ sind. Das Ressourcengruppenobjekt ist zwar schreibgeschützt, innerhalb der Ressourcengruppe können jedoch Änderungen an nicht gesperrten Ressourcen vorgenommen werden.

## <a name="overriding-locking-states"></a>Außerkraftsetzen von Zuständen

In der Regel kann es Benutzern mit entsprechender [rollenbasierter Zugriffssteuerung](../../../role-based-access-control/overview.md) für das Abonnement (also beispielsweise mit der Rolle „Besitzer“) gestattet werden, Änderungs- oder Löschvorgänge für beliebige Ressourcen auszuführen. Dies ist jedoch nicht der Fall, wenn Blueprints im Rahmen einer Zuweisungsbereitstellung eine Sperre anwendet. Wenn die Zuweisung mit der Option **Sperren** festgelegt wurde, kann auch der Besitzer des Abonnements die enthaltenen Ressourcen nicht ändern.

Diese Sicherheitsmaßnahme schützt die Konsistenz der definierten Blaupause und die Umgebung, die damit erstellt werden soll, vor versehentlichen oder programmgesteuerten Lösch- oder Änderungsvorgängen.

## <a name="removing-locking-states"></a>Entfernen von Sperrzuständen

Wenn die von einer Zuweisung erstellten Ressourcen gelöscht werden müssen, muss zunächst die Zuweisung entfernt werden. Wenn die Zuweisung entfernt wird, werden auch die von Blueprint erstellten Sperren entfernt. Die Ressource bleibt jedoch zurück und muss auf herkömmliche Weise gelöscht werden.

## <a name="how-blueprint-locks-work"></a>Funktionsweise von Blaupausensperren

Die RBAC-Rolle `denyAssignments` wird bei der Zuweisung einer Blaupause auf Artefaktressourcen angewendet, wenn für die Zuweisung die Option **Sperren** gewählt wurde. Die Rolle wird von der verwalteten Identität der Blaupausenzuweisung hinzugefügt und kann nur von derselben verwalteten Identität aus den Artefaktressourcen entfernt werden. Diese Sicherheitsmaßnahme erzwingt den Sperrmechanismus und verhindert die Aufhebung der Blaupausensperre außerhalb von Blueprints. Das Entfernen der Rolle und der Sperre ist nur möglich, indem die Blaupausenzuweisung entfernt wird. Dies kann nur von Personen mit den entsprechenden Rechten durchgeführt werden.

> [!IMPORTANT]
> Azure Resource Manager speichert Details zu Rollenzuweisungen bis zu 30 Minuten lang zwischen. `denyAssignments` ist daher für Blaupausenressourcen möglicherweise nicht sofort in vollem Umfang wirksam. Während dieser Zeit ist es ggf. möglich, eine Ressource zu löschen, die eigentlich durch Blaupausensperren geschützt werden sollte.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](sequencing-order.md) anpassen können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../how-to/update-existing-assignments.md).
- Beheben Sie Probleme bei der Blaupausenzuweisung mithilfe des [allgemeinen Leitfadens zur Problembehandlung](../troubleshoot/general.md).