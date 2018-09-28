---
title: Aktualisieren einer vorhandenen Azure Blueprint-Zuweisung
description: Hier werden die Mechanismen beim Aktualisieren einer vorhandenen Zuweisung in Azure Blueprint erläutert.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: ecac0fb21a6691874d5e8db49eadd7114d41845f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956199"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Aktualisieren einer vorhandenen Blaupausenzuweisung

Wenn eine Blaupause zugewiesen ist, kann die Zuweisung aktualisiert werden. Es gibt verschiedene Gründe für das Aktualisieren einer vorhandenen Zuweisung:

- Hinzufügen oder Entfernen einer [Ressourcensperre](../concepts/resource-locking.md)
- Ändern des Wertes von [dynamischen Parametern](../concepts/parameters.md#dynamic-parameters)
- Aktualisieren der Zuweisung auf eine neuere **veröffentlichte** Version der Blaupause

## <a name="updating-assignments"></a>Aktualisieren von Zuweisungen

1. Starten Sie den Azure Blueprint-Dienst im Azure-Portal. Klicken Sie dazu auf **Alle Dienste**, und suchen Sie im linken Bereich nach **Richtlinie**. Klicken Sie auf der Seite **Richtlinie** auf **Blaupausen**.

1. Wählen Sie auf der Seite links **Zugewiesene Blaupausen** aus.

1. Klicken Sie in der Liste der Blaupausen auf die Blaupausenzuweisung und anschließend auf die Schaltfläche **Zuweisung aktualisieren**, ODER klicken Sie mit der rechten Maustaste auf die Blaupausenzuweisung, und wählen Sie **Zuweisung aktualisieren** aus.

   ![Zuweisung aktualisieren](../media/update-existing-assignments/update-assignment.png)

1. Die Seite **Blaupause zuweisen** wird geladen, und alle Werte der ursprünglichen Zuweisung werden automatisch eingetragen. Sie können die **Version der Blaupausendefinition**, den Zustand von **Zuweisung sperren** sowie die dynamischen Parameter für die Blaupausendefinition ändern. Klicken Sie nach dem Vornehmen der Änderungen auf **Zuweisen**.

1. Überprüfen Sie auf der Detailseite für die aktualisierte Zuweisung den neuen Status. In diesem Beispiel wurde der Zuweisung **Wird gesperrt** hinzugefügt.

   ![Aktualisierte Zuweisung – Gesperrt](../media/update-existing-assignments/updated-assignment.png)

1. Einzelheiten zu anderen **Zuweisungsvorgängen** finden Sie in der Dropdownliste. Die Tabelle **Verwaltete Ressourcen** wird mit dem ausgewählten Zuweisungsvorgang aktualisiert.

   ![Zuweisungsvorgänge](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Regeln für das Aktualisieren von Zuweisungen

Bei der Bereitstellung der aktualisierten Zuweisungen werden einige wichtige Regeln befolgt. Diese Regeln bestimmen, was je nach angeforderter Änderung und dem Typ der bereitgestellten oder aktualisierten Artefaktressource mit einer vorhandenen Ressource geschieht.

- Rollenzuweisungen
  - Wird die Rolle oder der Rollenempfänger (Benutzer, Gruppe oder App) geändert, wird eine neue Rollenzuweisung erstellt. Die zuvor bereitgestellte Rollenzuweisung wird beibehalten.
- Richtlinienzuweisungen
  - Werden die Parameter der Richtlinienzuweisung geändert, wird die vorhandene Zuweisung aktualisiert.
  - Wird die Definition der Richtlinienzuweisung geändert, wird eine neue Richtlinienzuweisung erstellt. Die zuvor bereitgestellte Richtlinienzuweisung wird beibehalten.
  - Wird das Artefakt der Richtlinienzuweisung aus der Blaupause entfernt, wird die zuvor bereitgestellte Richtlinienzuweisung beibehalten.
- Azure-Ressourcen-Manager-Vorlagen
  - Die Vorlage wird über den Ressourcen-Manager als **PUT** verarbeitet. Da dies für jeden Ressourcentyp anders behandelt wird, sehen Sie die Dokumentation der einzelnen eingeschlossenen Ressourcen ein, um die Auswirkung dieser von Blueprint ausgeführten Aktion zu bestimmen.

## <a name="possible-errors-on-updating-assignments"></a>Mögliche Fehler beim Aktualisieren von Zuweisungen

Beim Aktualisieren von Zuweisungen können Änderungen vorgenommen werden, die bei der Ausführung eine Unterbrechung bewirken. Ein Beispiel hierfür ist das Ändern des Speicherorts einer Ressourcengruppe, nachdem diese bereits bereitgestellt wurde. Jede von [Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) unterstützte Änderung kann vorgenommen werden. Jede Änderung, die bei Azure Resource Manager einen Fehler verursacht, bewirkt jedoch ebenso das Fehlschlagen der Zuweisung.

Es gibt keine Beschränkung in Bezug darauf, wie oft eine Zuweisung aktualisiert werden kann. Wenn also ein Fehler aufgrund eines fehlerhaften Parameters, eines bereits vorhandenen Objekts oder einer durch Azure Resource Manager nicht zugelassenen Änderung auftritt, ermitteln Sie den Fehler, und aktualisieren Sie die Zuweisung ein weiteres Mal.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Lebensdauer von Blaupausen](../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../concepts/sequencing-order.md) anpassen.
- Informieren Sie sich über die Verwendung der [Blaupausen-Ressourcensperre](../concepts/resource-locking.md).
- Beheben Sie anhand der [allgemeinen Problembehandlung](../troubleshoot/general.md) Probleme während der Zuweisung einer Blaupause.
