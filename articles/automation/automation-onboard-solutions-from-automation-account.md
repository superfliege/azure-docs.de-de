---
title: Erfahren Sie, wie Sie Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand in Azure Automation integrieren
description: Erfahren Sie, wie Sie einen virtuellen Azure-Computer mit Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand integrieren, die Bestandteil von Azure Automation sind
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 0174e2a3c0b14c52b5750e343932a5df39d18976
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833367"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Integrieren von Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand

Azure Automation stellt Lösungen zum Verwalten der Sicherheitsupdates für das Betriebssystem, zum Nachverfolgen von Änderungen und für den Bestand bereit, der auf Ihren Computern installiert ist. Es gibt mehrere Möglichkeiten, Computer zu integrieren. Sie können die Lösung [über einen virtuellen Computer](automation-onboard-solutions-from-vm.md), [durch Durchsuchen mehrerer Computer](automation-onboard-solutions-from-browse.md), über Ihr Automation-Konto oder per [Runbook](automation-onboard-solutions.md) integrieren. Dieser Artikel behandelt das Integrieren dieser Lösungen über Ihr Automation-Konto.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Anmelden bei Azure unter https://portal.azure.com

## <a name="enable-solutions"></a>Aktivieren von Lösungen

Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie unter **KONFIGURATIONSVERWALTUNG** entweder **Bestand** oder **Änderungsnachverfolgung** aus.

Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um die Lösung zu aktivieren. Es dauert ungefähr 15 Minuten, bis die Lösung aktiviert ist.

![Integrieren der Bestandslösung](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

Die Lösung für Änderungsnachverfolgung und Bestand bietet die Möglichkeit zum Nachverfolgen von [Änderungen](automation-vm-change-tracking.md) und [Bestand](automation-vm-inventory.md) in Ihren virtuellen Computern. In diesem Schritt aktivieren Sie die Lösung auf einem virtuellen Computer.

Wenn Sie die Benachrichtigung erhalten haben, dass die Lösung für Änderungsnachverfolgung und Bestand integriert wurde, klicken Sie unter **KONFIGURATIONSVERWALTUNG** auf **Updateverwaltung**.

Mithilfe der Lösung zur Updateverwaltung können Sie Updates und Patches für Ihre Azure-Windows-VMs verwalten. Sie können den Status der verfügbaren Updates bewerten, die Installation der erforderlichen Updates planen und Bereitstellungsergebnisse überprüfen, um sicherzustellen, dass Updates erfolgreich auf die VM angewendet wurden. Mit dieser Aktion wird die Lösung für Ihren virtuellen Computer aktiviert.

Wählen Sie unter **UPDATEVERWALTUNG** die Option **Updateverwaltung** aus. Der ausgewählte Log Analytics-Arbeitsbereich ist derselbe, der im vorherigen Schritt verwendet wurde. Klicken Sie auf **Aktivieren**, um die Lösung für die Updateverwaltung zu integrieren. Es dauert ungefähr 15 Minuten, bis die Lösung aktiviert ist.

![Integrieren der Updatelösung](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Bereichskonfiguration

Jede Lösung verwendet eine Bereichskonfiguration innerhalb des Arbeitsbereichs, um die Computer zu erreichen, welche die Lösung erhalten sollen. Die Bereichskonfiguration besteht aus einer Gruppe von mindestens einem gespeicherten Suchvorgang, der zum Begrenzen des Bereichs der Lösung auf bestimmte Computer verwendet wird. Wählen Sie in Ihrem Automation-Konto unter **ZUGEHÖRIGE RESSOURCEN** die Option **Arbeitsbereich** aus, um auf die Bereichskonfigurationen zuzugreifen. Wählen dann im Arbeitsbereich unter **ARBEITSBEREICHSDATENQUELLEN** die Option **Bereichskonfigurationen** aus.

Falls der ausgewählte Arbeitsbereich noch nicht die Lösungen „Updateverwaltung“ und „Änderungsnachverfolgung“ enthält, werden die folgenden Bereichskonfigurationen erstellt:

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Wenn der ausgewählte Arbeitsbereich bereits die Lösung enthält, wird sie nicht erneut bereitgestellt, und die Bereichskonfiguration wird ihm nicht hinzugefügt.

## <a name="saved-searches"></a>Gespeicherte Suchvorgänge

Wenn ein Computer zu den Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand hinzugefügt wird, werden diese Lösungen einem der beiden gespeicherten Suchvorgänge in Ihrem Arbeitsbereich hinzugefügt. Diese gespeicherten Suchvorgänge sind Abfragen, welche die Computer enthalten, die die Zielgruppe für diese Lösungen bilden.

Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie **Gespeicherte Suchvorgänge** unter **Allgemein** aus. Die beiden gespeicherten Suchvorgänge, die von diesen Lösungen verwendet werden, können Sie in der folgenden Tabelle anzeigen:

|NAME     |Category (Kategorie)  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Aktualisierungen        | Updates__MicrosoftDefaultComputerGroup         |

Wählen Sie einen der gespeicherten Suchvorgänge aus, um die zum Füllen der Gruppe verwendete Abfrage anzuzeigen. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse:

![Gespeicherte Suchvorgänge](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Integrieren von Azure-VMs

Wählen Sie in Ihrem Automation-Konto unter **KONFIGURATIONSVERWALTUNG** entweder **Bestand** oder **Änderungsnachverfolgung** bzw. unter **UPDATEVERWALTUNG** die Option **Updateverwaltung** aus.

Klicken Sie auf **+ Azure-VMs hinzufügen**, und wählen Sie mindestens eine VM aus der Liste aus. Virtuelle Computer, die nicht aktiviert werden können, sind ausgegraut und können nicht ausgewählt werden. Klicken Sie auf der Seite **Updateverwaltung aktivieren** auf **Aktivieren**. Dadurch werden die ausgewählten virtuellen Computer der Computergruppe des gespeicherten Suchvorgangs für die Lösung hinzugefügt.

![Aktivieren von Azure-VMs](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>Integrieren eines Nicht-Azure-Computers

Computer, die nicht in Azure enthalten sind, müssen manuell hinzugefügt werden. Wählen Sie in Ihrem Automation-Konto unter **KONFIGURATIONSVERWALTUNG** entweder **Bestand** oder **Änderungsnachverfolgung** bzw. unter **UPDATEVERWALTUNG** die Option **Updateverwaltung** aus.

Klicken Sie auf **Nicht-Azure-Computer hinzufügen**. Dadurch wird ein neues Browserfenster mit den [Anweisungen zum Installieren und Konfigurieren von Microsoft Monitoring Agent auf dem Computer](../log-analytics/log-analytics-concept-hybrid.md) geöffnet, sodass der Computer mit der Berichterstellung für die Lösung beginnen kann. Wenn Sie einen Computer integrieren, der aktuell von System Center Operations Manager verwaltet wird, ist kein neuer Agent erforderlich. Die Arbeitsbereichsinformationen werden in den vorhandenen Agent eingegeben.

## <a name="onboard-machines-in-the-workspace"></a>Integrierte Computer im Arbeitsbereich

Manuell installierte Computer oder Computer, die bereits Ihrem Arbeitsbereich angehören, müssen Azure Automation hinzugefügt werden, damit die Lösung aktiviert wird. Wählen Sie in Ihrem Automation-Konto unter **KONFIGURATIONSVERWALTUNG** entweder **Bestand** oder **Änderungsnachverfolgung** bzw. unter **UPDATEVERWALTUNG** die Option **Updateverwaltung** aus.

Wählen Sie **Computer verwalten** aus. Dadurch wird die Seite **Computer verwalten** geöffnet. Auf dieser Seite können Sie die Lösung für eine ausgewählte Computergruppe, für alle verfügbaren Computer oder für alle aktuellen und zukünftigen Computer aktivieren.

![Gespeicherte Suchvorgänge](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>Alle verfügbaren Computer

Um die Lösung für alle verfügbaren Computer zu aktivieren, wählen Sie **Auf allen verfügbaren Computern aktivieren** aus. Dadurch wird das Steuerelement für das einzelne Hinzufügen von Computern deaktiviert. Mit dieser Aufgabe werden alle Namen der Computer hinzugefügt, die Berichte für den Arbeitsbereich und die Computergruppe der gespeicherten Suchabfrage erstellen.

### <a name="all-available-and-future-machines"></a>Alle verfügbaren und zukünftigen Computer

Um die Lösung für alle verfügbaren und alle zukünftigen Computer zu aktivieren, wählen Sie **Auf allen verfügbaren und zukünftigen Computern aktivieren** aus. Mit dieser Option werden die gespeicherte Suchvorgänge und Bereichskonfigurationen aus dem Arbeitsbereich gelöscht. Daraufhin wird die Lösung für alle Azure- und Nicht-Azure-Computer geöffnet, die Berichte für den Arbeitsbereich erstellen.

### <a name="selected-machines"></a>Ausgewählte Computer

Um die Lösung für einen oder mehrere Computer zu aktivieren, wählen Sie **Auf ausgewählten Computern aktivieren** aus, und klicken Sie neben jedem Computer, den Sie der Lösung hinzufügen möchten, auf **Hinzufügen**. Mit dieser Aufgabe werden die Namen der ausgewählten Computer der Computergruppe der gespeicherten Suchabfrage für die Lösung hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit den Tutorials fort, um mehr über die Verwendung der Lösungen zu erfahren.

* [Tutorial: Verwalten von Updates für den virtuellen Computer](automation-tutorial-update-management.md)

* [Tutorial: Identifizieren von Software auf einem virtuellen Computer](automation-tutorial-installed-software.md)

* [Tutorial: Problembehandlung bei Änderungen auf einem virtuellen Computer](automation-tutorial-troubleshoot-changes.md)