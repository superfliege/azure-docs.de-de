---
title: Integrieren von Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand von einem virtuellen Azure-Computer
description: Erfahren Sie, wie Sie einen virtuellen Azure-Computer mit Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand integrieren, die Bestandteil von Azure Automation sind.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: f270b2ccea51e83bc6475051b8667bf73d7fd717
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221511"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Integrieren von Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand von einem virtuellen Azure-Computer

Azure Automation stellt Lösungen zum Verwalten der Sicherheitsupdates für das Betriebssystem, zum Nachverfolgen von Änderungen und für den Bestand bereit, der auf Ihren Computern installiert ist. Computer können auf unterschiedliche Weise integriert werden. Sie können die Lösung über einen virtuellen Computer, [über Ihr Automation-Konto](automation-onboard-solutions-from-automation-account.md), [durch Durchsuchen mehrerer Computer](automation-onboard-solutions-from-browse.md) oder per [Runbook](automation-onboard-solutions.md) integrieren. Dieser Artikel behandelt das Integrieren dieser Lösungen von einem virtuellen Azure-Computer.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="enable-the-solutions"></a>Aktivieren der Lösungen

Navigieren Sie zu einem vorhandenen virtuellen Computer. Wählen Sie unter **VORGÄNGE** **Updateverwaltung**, **Bestand** oder **Änderungsnachverfolgung** aus.

Um die Lösung nur für die VM zu aktivieren, stellen sicher, dass **Für diesen virtuellen Computer aktivieren** ausgewählt ist. Um mehrere Computer in die Lösung zu integrieren, wählen Sie **Für virtuelle Computer in diesem Abonnement aktivieren** und dann **Klicken, um zu aktivierende Computer auszuwählen** aus. Wie Sie mehrere Computer gleichzeitig integrieren, erfahren Sie unter [Integrieren von Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand](automation-onboard-solutions-from-automation-account.md).

Wählen Sie den Azure Log Analytics-Arbeitsbereich sowie das Automation-Konto und dann **Aktivieren** aus, um die Lösung zu aktivieren. Es dauert ungefähr 15 Minuten, bis die Lösung aktiviert ist.

![Integrieren der Updateverwaltungslösung](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Navigieren Sie zu den anderen Lösungen, und wählen Sie **Aktivieren** aus. Die Dropdownlisten für Log Analytics und Automation-Konto sind deaktiviert, da diese Lösungen denselben Arbeitsbereich und dasselbe Automation-Konto verwenden wie die zuvor aktivierte Lösung.

> [!NOTE]
> **Änderungsnachverfolgung** und **Bestand** verwenden dieselbe Lösung. Wenn eine dieser Lösungen aktiviert ist, ist die andere auch aktiviert.

## <a name="scope-configuration"></a>Bereichskonfiguration

Jede Lösung verwendet eine Bereichskonfiguration innerhalb des Arbeitsbereichs, um die Computer zu erreichen, welche die Lösung erhalten sollen. Die Bereichskonfiguration besteht aus einer Gruppe von mindestens einem gespeicherten Suchvorgang, der zum Begrenzen des Bereichs der Lösung auf bestimmte Computer verwendet wird. Wählen Sie in Ihrem Automation-Konto unter **ZUGEHÖRIGE RESSOURCEN** die Option **Arbeitsbereich** aus, um auf die Bereichskonfigurationen zuzugreifen. Wählen Sie im Arbeitsbereich unter **ARBEITSBEREICHSDATENQUELLEN** die Option **Bereichskonfigurationen** aus.

Falls der ausgewählte Arbeitsbereich nicht die Lösungen „Updateverwaltung“ und „Änderungsnachverfolgung“ enthält, werden die folgenden Bereichskonfigurationen erstellt:

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Verfügt der ausgewählte Arbeitsbereich bereits über die Lösung, wird die Lösung nicht erneut bereitgestellt, und die Bereichskonfiguration wird nicht hinzugefügt.

Wählen Sie die Auslassungspunkte (**...**) bei einer Konfiguration und dann **Bearbeiten** aus. Wählen Sie im Bereich **Bereichskonfiguration bearbeiten** die Option **Computergruppen auswählen** aus. Im Bereich **Computergruppen** werden die gespeicherten Suchvorgänge angezeigt, die zum Erstellen der Bereichskonfiguration verwendet werden.

## <a name="saved-searches"></a>Gespeicherte Suchvorgänge

Wird ein Computer den Lösungen für Updateverwaltung, Änderungsnachverfolgung oder Bestand hinzugefügt, wird er auch einem der beiden gespeicherten Suchvorgänge in Ihrem Arbeitsbereich hinzugefügt. Die gespeicherten Suchvorgänge sind Abfragen, welche die Computer enthalten, die die Zielgruppe dieser Lösungen sind.

Wechseln Sie zu Ihrem Arbeitsbereich. Wählen Sie unter **Allgemein** die Option **Gespeicherte Suchvorgänge** aus. Die beiden von diesen Lösungen verwendeten gespeicherten Suchvorgänge werden in der folgenden Tabelle angezeigt:

|NAME     |Category (Kategorie)  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Aktualisierungen        | Updates__MicrosoftDefaultComputerGroup         |

Wählen Sie einen der gespeicherten Suchvorgänge aus, um die zum Füllen der Gruppe verwendete Abfrage anzuzeigen. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse:

![Gespeicherte Suchvorgänge](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit den Tutorials fort, um mehr über die Verwendung der Lösungen zu erfahren:

* [Tutorial: Verwalten von Updates für den virtuellen Computer](automation-tutorial-update-management.md)
* [Tutorial: Identifizieren von Software auf einem virtuellen Computer](automation-tutorial-installed-software.md)
* [Tutorial: Problembehandlung bei Änderungen auf einem virtuellen Computer](automation-tutorial-troubleshoot-changes.md)
