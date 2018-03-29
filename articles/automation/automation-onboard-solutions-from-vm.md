---
title: Erfahren Sie, wie Sie Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand von einem virtuellen Azure-Computer integrieren
description: Erfahren Sie, wie Sie einen virtuellen Azure-Computer mit Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand integrieren, die Bestandteil von Azure Automation sind
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 8713447386e116b3f1ecb2754d46cc8d88d00b39
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Integrieren von Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand von einem virtuellen Azure-Computer

Azure Automation stellt Lösungen zum Verwalten der Sicherheitsupdates für das Betriebssystem, zum Nachverfolgen von Änderungen und für den Bestand bereit, der auf Ihren Computern installiert ist. Es gibt mehrere Möglichkeiten, Computer zu integrieren; Sie können die Lösung von einem virtuellen Computer, [über Ihr Automation-Konto](automation-onboard-solutions-from-automation-account.md) oder per [Runbook](automation-onboard-solutions.md) integrieren. Dieser Artikel behandelt das Integrieren dieser Lösungen von einem virtuellen Azure-Computer.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Anmelden bei Azure unter https://portal.azure.com

## <a name="enable-the-solutions"></a>Aktivieren der Lösungen

Navigieren Sie zu einem vorhandenen virtuellen Computer, und wählen Sie unter **VORGÄNGE** entweder **Updateverwaltung**, **Bestand** oder **Änderungsnachverfolgung** aus.

Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um die Lösung zu aktivieren. Es dauert ungefähr 15 Minuten, bis die Lösung aktiviert ist.

![Integrieren der Updatelösung](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Navigieren Sie zu den anderen Lösungen, und klicken Sie auf **Aktivieren**. Die Dropdownfelder für Log Analytics und Automation-Konto sind dabei deaktiviert, da auch die anderen Lösungen den Arbeitsbereich und das Automation-Konto verwenden, die Sie bei der zuvor aktivierten Lösung angegeben haben.

![Integrieren der Updatelösung](media/automation-onboard-solutions-from-vm/onboard-solutions2.png)

> [!NOTE]
> **Änderungsnachverfolgung** und **Bestand** verwenden dieselbe Lösung. Ist die eine Option aktiviert, ist die andere ebenfalls aktiviert.

## <a name="scope-configuration"></a>Bereichskonfiguration

Jede Lösung verwendet eine Bereichskonfiguration innerhalb des Arbeitsbereichs, um die Computer zu erreichen, welche die Lösung erhalten sollen. Die Bereichskonfiguration besteht aus einer Gruppe von mindestens einem gespeicherten Suchvorgang, der zum Begrenzen des Bereichs der Lösung auf bestimmte Computer verwendet wird. Wenn Sie auf die Bereichskonfigurationen zugreifen möchten, wählen Sie in Ihrem Automation-Konto unter **VERWANDTE RESSOURCEN** die Option **Arbeitsbereich** und dann im Arbeitsbereich unter **Arbeitsbereichsdatenquellen** die **Bereichskonfigurationen** aus.

**MicrosoftDefaultScopeConfig-ChangeTracking** und **MicrosoftDefaultScopeConfig-Updates** sind die beiden Bereichskonfigurationen, die standardmäßig erstellt werden.

Klicken Sie auf die Auslassungspunkte (...) bei einer Konfiguration, und wählen Sie **Bearbeiten** aus. Wählen Sie auf der Seite **Bereichskonfiguration bearbeiten** die Option **Computergruppen auswählen** aus, um die Seite **Computergruppen** zu öffnen. Auf dieser Seite werden die gespeicherten Suchvorgänge angezeigt, die zum Erstellen der Bereichskonfiguration verwendet werden.

## <a name="saved-searches"></a>Gespeicherte Suchvorgänge

Wenn ein Computer zu den Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand hinzugefügt wird, werden diese Lösungen einem der beiden gespeicherten Suchvorgänge in Ihrem Arbeitsbereich hinzugefügt. Diese gespeicherten Suchvorgänge sind Abfragen, welche die Computer enthalten, die die Zielgruppe für diese Lösungen bilden.

Navigieren Sie zu Ihrem Arbeitsbereich, und wählen Sie **Gespeicherte Suchvorgänge** unter **Allgemein** aus. Die beiden gespeicherten Suchvorgänge, die von diesen Lösungen verwendet werden, können Sie in der folgenden Tabelle anzeigen:

|NAME     |Category (Kategorie)  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Aktualisierungen        | Updates__MicrosoftDefaultComputerGroup         |

Wählen Sie einen der gespeicherten Suchvorgänge aus, um die zum Füllen der Gruppe verwendete Abfrage anzuzeigen. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse.

![Gespeicherte Suchvorgänge](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit den Tutorials fort, um mehr über die Verwendung der Lösungen zu erfahren.

* [Tutorial: Verwalten von Updates für den virtuellen Computer](automation-tutorial-update-management.md)

* [Tutorial: Identifizieren von Software auf einem virtuellen Computer](automation-tutorial-installed-software.md)

* [Tutorial: Problembehandlung bei Änderungen auf einem virtuellen Computer](automation-tutorial-troubleshoot-changes.md)
