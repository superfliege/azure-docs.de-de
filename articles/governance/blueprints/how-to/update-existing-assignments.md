---
title: Aktualisieren einer vorhandenen Zuweisung
description: Hier werden die Mechanismen beim Aktualisieren einer vorhandenen Zuweisung in Azure Blueprint erläutert.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 2ada0ccc1286677300c005da4fa0e92be94a3ca8
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314825"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Aktualisieren einer vorhandenen Blaupausenzuweisung

Wenn eine Blaupause zugewiesen ist, kann die Zuweisung aktualisiert werden. Es gibt verschiedene Gründe für das Aktualisieren einer vorhandenen Zuweisung:

- Hinzufügen oder Entfernen einer [Ressourcensperre](../concepts/resource-locking.md)
- Ändern des Wertes von [dynamischen Parametern](../concepts/parameters.md#dynamic-parameters)
- Aktualisieren der Zuweisung auf eine neuere **veröffentlichte** Version der Blaupause

## <a name="updating-assignments"></a>Aktualisieren von Zuweisungen

1. Klicken Sie auf **Alle Dienste**, suchen Sie im linken Bereich nach der Option **Richtlinie**, und wählen Sie sie aus. Klicken Sie auf der Seite **Richtlinie** auf **Blaupausen**.

1. Wählen Sie auf der Seite links **Zugewiesene Blaupausen** aus.

1. Klicken Sie in der Liste der Blaupausen auf die Blaupausenzuweisung. Klicken Sie anschließend auf die Schaltfläche **Zuweisung aktualisieren**, ODER klicken Sie mit der rechten Maustaste auf die Blaupausenzuweisung, und wählen Sie **Zuweisung aktualisieren** aus.

   ![Zuweisung aktualisieren](../media/update-existing-assignments/update-assignment.png)

1. Die Seite **Blaupause zuweisen** wird geladen, und alle Werte der ursprünglichen Zuweisung werden automatisch eingetragen. Sie können die **Version der Blaupausendefinition**, den Zustand von **Zuweisung sperren** sowie die dynamischen Parameter für die Blaupausendefinition ändern. Klicken Sie nach dem Vornehmen der Änderungen auf **Zuweisen**.

1. Überprüfen Sie auf der Detailseite für die aktualisierte Zuweisung den neuen Status. In diesem Beispiel wurde der Zuweisung **Wird gesperrt** hinzugefügt.

   ![Aktualisierte Zuweisung – Gesperrt](../media/update-existing-assignments/updated-assignment.png)

1. Einzelheiten zu anderen **Zuweisungsvorgängen** finden Sie in der Dropdownliste. Die Tabelle **Verwaltete Ressourcen** wird mit dem ausgewählten Zuweisungsvorgang aktualisiert.

   ![Zuweisungsvorgänge](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Regeln für das Aktualisieren von Zuweisungen

Bei der Bereitstellung der aktualisierten Zuweisungen werden einige wichtige Regeln befolgt. Diese Regeln legen fest, was mit bereits bereitgestellten Ressourcen geschieht. Die angeforderte Änderung und der Typ der bereitgestellten oder aktualisierten Artefaktressource bestimmen, welche Maßnahmen ergriffen werden.

- Rollenzuweisungen
  - Wird die Rolle oder der Rollenempfänger (Benutzer, Gruppe oder App) geändert, wird eine neue Rollenzuweisung erstellt. Zuvor bereitgestellte Rollenzuweisungen werden beibehalten.
- Richtlinienzuweisungen
  - Werden die Parameter der Richtlinienzuweisung geändert, wird die vorhandene Zuweisung aktualisiert.
  - Wird die Definition der Richtlinienzuweisung geändert, wird eine neue Richtlinienzuweisung erstellt. Zuvor bereitgestellte Richtlinienzuweisungen werden beibehalten.
  - Wird das Artefakt der Richtlinienzuweisung aus der Blaupause entfernt, werden bereitgestellte Richtlinienzuweisung beibehalten.
- Azure-Ressourcen-Manager-Vorlagen
  - Die Vorlage wird über den Ressourcen-Manager als **PUT** verarbeitet. Da diese Aktion für jeden Ressourcentyp anders behandelt wird, sehen Sie sich die Dokumentation der einzelnen eingeschlossenen Ressourcen an, um die Auswirkung dieser von Blaupausen ausgeführten Aktion zu bestimmen.

## <a name="possible-errors-on-updating-assignments"></a>Mögliche Fehler beim Aktualisieren von Zuweisungen

Beim Aktualisieren von Zuweisungen können Änderungen vorgenommen werden, die bei der Ausführung eine Unterbrechung bewirken. Ein Beispiel hierfür ist das Ändern des Speicherorts einer Ressourcengruppe, nachdem diese bereits bereitgestellt wurde. Jede von [Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) unterstützte Änderung kann vorgenommen werden. Jede Änderung, die bei Azure Resource Manager einen Fehler verursacht, bewirkt jedoch ebenso das Fehlschlagen der Zuweisung.

Es gibt keine Beschränkung in Bezug darauf, wie oft eine Zuweisung aktualisiert werden kann. Wenn ein Fehler auftritt, identifizieren Sie ihn, und aktualisieren Sie die Zuweisung erneut.  Beispielszenarien für Fehler:

- Falscher Parameter
- Ein bereits vorhandenes Objekt
- Eine Änderung, die von Azure Resource Manager nicht unterstützt wird

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../concepts/sequencing-order.md) anpassen können.
- Informieren Sie sich über die Verwendung der [Blaupausen-Ressourcensperre](../concepts/resource-locking.md).
- Beheben Sie anhand der [allgemeinen Problembehandlung](../troubleshoot/general.md) Probleme während der Zuweisung einer Blaupause.
