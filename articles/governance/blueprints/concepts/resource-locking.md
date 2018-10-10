---
title: Grundlegendes zur Ressourcensperre in Azure Blueprint
description: Es wird beschrieben, wie Sie die Optionen für das Sperren verwenden, um beim Zuweisen einer Blaupause die Ressourcen zu schützen.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 718c23b806da5058c042b961077e51ba0d4b6245
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973251"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Grundlegendes zur Ressourcensperre in Azure Blueprint

Die bedarfsabhängige Erstellung von einheitlichen Umgebungen hat nur dann wirklich Vorteile, wenn ein Mechanismus vorhanden ist, mit dem die Wahrung der Einheitlichkeit sichergestellt werden kann. In diesem Artikel wird beschrieben, wie das Sperren von Ressourcen in Azure Blueprint funktioniert.

## <a name="locking-modes-and-states"></a>Modi und Zustände von Sperren

Der Sperrmodus gilt für die Blaupausenzuweisung und verfügt nur über zwei Optionen: **Keine** und **Alle Ressourcen**. Dies wird während der Blaupausenzuweisung konfiguriert und kann nicht mehr geändert werden, nachdem die Zuweisung erfolgreich auf das Abonnement angewendet wurde.

Ressourcen, die über Artefaktdefinitionen in der dem Abonnement zugewiesenen Blaupause erstellt wurden, können drei Zustände aufweisen: **Nicht gesperrt**, **Schreibgeschützt** und **Bearbeitung/Löschen nicht möglich**. Alle Arten von Artefakten können den Zustand **Nicht gesperrt** aufweisen. Für andere Artefakte als Ressourcengruppen gilt **Schreibgeschützt**, und für Ressourcengruppen gilt **Bearbeitung/Löschen nicht möglich**. Dies stellt in Bezug auf die Verwaltung dieser Ressourcen einen wichtigen Unterschied dar.

Der Zustand **Schreibgeschützt** ist klar definiert: Die Ressource kann nicht geändert oder gelöscht werden. Beim Zustand **Bearbeitung/Löschen nicht möglich** ist es nicht so eindeutig, weil Ressourcengruppen praktisch „Container“ sind. Das Ressourcengruppenobjekt ist schreibgeschützt, aber es ist möglich, Ressourcen in der Ressourcengruppe zu erstellen, zu aktualisieren und zu löschen – solange diese nicht Teil einer Blaupausenzuweisung mit dem Sperrstatus **Schreibgeschützt** sind.

## <a name="overriding-locking-states"></a>Außerkraftsetzen von Zuständen

Es ist normalerweise zwar möglich, dass ein Benutzer mit der richtigen [rollenbasierten Zugriffssteuerung](../../../role-based-access-control/overview.md) (Role-Based Access Control, RBAC) im Abonnement, z.B. der Rolle „Besitzer“, alle Ressourcen ändern oder löschen kann. Dies ist aber nicht der Fall, wenn Azure Blueprint die Sperre im Rahmen der bereitgestellten Zuweisung anwendet. Wenn die Zuweisung mit der Option **Sperren** festgelegt wurde, kann auch der Besitzer des Abonnements die enthaltenen Ressourcen nicht ändern.

So wird die Konsistenz der definierten Blaupause sichergestellt, und die Umgebung, die damit erstellt werden soll, wird vor versehentlichen oder programmgesteuerten Lösch- oder Änderungsvorgängen geschützt.

## <a name="removing-locking-states"></a>Entfernen von Sperrzuständen

Wenn es erforderlich ist, die von einer Zuweisung erstellten Ressourcen zu löschen, ist dies nur möglich, wenn zuerst die Zuweisung entfernt wird. Wenn die Zuweisung entfernt wird, werden auch die von Blueprint erstellten Sperren entfernt. Die Ressource ist aber weiterhin vorhanden und muss dann auf die übliche Weise von einem Benutzer mit den entsprechenden Berechtigungen gelöscht werden.

## <a name="how-blueprint-locks-work"></a>Funktionsweise von Blaupausensperren

Die RBAC-Rolle `denyAssignments` wird bei der Zuweisung einer Blaupause auf Artefaktressourcen angewendet, wenn für die Zuweisung die Option **Sperren** gewählt wurde. Die Rolle wird von der verwalteten Identität der Blaupausenzuweisung hinzugefügt und kann nur von derselben verwalteten Identität aus den Artefaktressourcen entfernt werden. Hierdurch wird der Sperrmechanismus erzwungen, und Versuche, die Blaupausensperre außerhalb von Blueprint aufzuheben, werden verhindert. Das Entfernen der Rolle und der Sperre ist nur möglich, indem die Blaupausenzuweisung entfernt wird. Dies kann nur von Personen mit den entsprechenden Rechten durchgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](sequencing-order.md) anpassen.
- Erfahren Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../how-to/update-existing-assignments.md).
- Beheben Sie bei der Blaupausenzuweisung auftretende Probleme mithilfe der [allgemeinen Lösungsanleitung](../troubleshoot/general.md).