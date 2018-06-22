---
title: Erfahren Sie, wie Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand für mehrere VMs in Azure Automation integriert werden
description: Erfahren Sie, wie Sie einen virtuellen Azure-Computer mit Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand integrieren, die Bestandteil von Azure Automation sind
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 0a624d850b8c3260acb24cb17566090e8ad0043e
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233936"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Aktivieren von Lösungen für die Updateverwaltung, Änderungsnachverfolgung und den Bestand für mehrere VMs

Azure Automation stellt Lösungen zum Verwalten der Sicherheitsupdates für das Betriebssystem, zum Nachverfolgen von Änderungen und für den Bestand bereit, der auf Ihren Computern installiert ist. Es gibt mehrere Möglichkeiten, Computer zu integrieren. Sie können die Lösung [über einen virtuellen Computer](automation-onboard-solutions-from-vm.md), über Ihr [Automation-Konto](automation-onboard-solutions-from-automation-account.md), durch Durchsuchen mehrerer Computer oder per [Runbook](automation-onboard-solutions.md) integrieren. Dieser Artikel behandelt das Integrieren dieser Lösungen durch Durchsuchen virtueller Computer in Azure.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Anmelden bei Azure unter https://portal.azure.com

## <a name="enable-solutions"></a>Aktivieren von Lösungen

Navigieren Sie im Azure-Portal zu **Virtuelle Computer**.

Wählen Sie über die Kontrollkästchen die virtuellen Computer aus, die Sie in Änderungsnachverfolgung und Bestand oder Updateverwaltung integrieren möchten. Die Integration kann für bis zu drei verschiedene Ressourcengruppen gleichzeitig durchgeführt werden.

![Liste der VMs](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Verwenden Sie die Filtersteuerelemente, um die Liste der virtuellen Computer zu ändern, und klicken Sie anschließend auf das oberste Kontrollkästchen, um alle virtuellen Computer in der Liste auszuwählen.

Klicken Sie in der Befehlsleiste auf **Dienste**, und wählen Sie **Änderungsnachverfolgung**, **Bestand** oder **Updateverwaltung**.

> [!NOTE]
> **Änderungsnachverfolgung** und **Bestand** verwenden dieselbe Lösung. Ist die eine Option aktiviert, ist die andere ebenfalls aktiviert.

Die folgende Abbildung zeigt die Updateverwaltung. Änderungsnachverfolgung und Bestand haben das gleiche Layout und Verhalten.

Die Liste der virtuellen Computer wird gefiltert, um nur die virtuellen Computer anzuzeigen, die sich in demselben Abonnement und Speicherort befinden. Wenn sich Ihr virtueller Computer in mehr als drei Ressourcengruppen befindet, werden die ersten drei Ressourcengruppen ausgewählt.

Verwenden Sie die Filtersteuerelemente, um virtuelle Computer aus verschiedenen Abonnements, Speicherorten und Ressourcengruppen auszuwählen.

![Integrieren der Updateverwaltungslösung](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Schauen Sie sich die Auswahlmöglichkeiten für den Log Analytics-Arbeitsbereich und das Automation-Konto an. Standardmäßig sind ein neuer Arbeitsbereich und ein neues Automation-Konto ausgewählt. Wenn Sie einen vorhandenen Log Analytics-Arbeitsbereich und ein vorhandenes Automation-Konto nutzen möchten, klicken Sie auf **Ändern**, und wählen Sie sie auf der Seite **Konfiguration** aus. Klicken Sie anschließend auf **Speichern**.

![Auswählen von Arbeitsbereich und Konto](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Deaktivieren Sie das Kontrollkästchen neben jedem virtuellen Computern, den Sie nicht aktivieren möchten. Die Auswahl für virtuelle Computer, die nicht aktiviert werden können, ist bereits aufgehoben.

Klicken Sie auf **Aktivieren**, um die Lösung zu aktivieren. Es dauert ungefähr 15 Minuten, bis die Lösung aktiviert ist.

## <a name="troubleshooting"></a>Problembehandlung

Beim Integrieren mehrerer Computer, werden einige Computer möglicherweise mit **Aktivieren nicht möglich** angezeigt. Es gibt verschiedene Gründe, warum einige Computer nicht aktiviert werden können. Die folgenden Abschnitte zeigen mögliche Gründe für den Status**Aktivieren nicht möglich** auf einem virtuellen Computer beim Versuch einer Integration.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>VM berichtet an einen anderen Arbeitsbereich: '\<workspaceName\>'.  Ändern der Konfiguration für die Verwendung zum Aktivieren

**Ursache**: Dieser Fehler zeigt an, dass der virtuelle Computer, den Sie integrieren möchten, an einen anderen Arbeitsbereich berichtet.

**Lösung**: Klicken Sie auf **Als Konfiguration verwenden**, um das angestrebte Automation-Konto und den angestrebten Log Analytics-Arbeitsbereich zu ändern.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>VM berichtet an einen Arbeitsbereich, der in diesem Abonnement nicht verfügbar ist.

**Ursache**: Der Arbeitsbereich, an den der virtuelle Computer berichtet:

* Befindet sich in einem anderen Abonnement oder
* Ist nicht mehr vorhanden oder
* Befindet sich in einer Ressourcengruppe, für die Sie keine Zugriffsberechtigungen haben

**Lösung**: Suchen Sie das Automation-Konto, das dem Arbeitsbereich zugeordnet ist, an den der virtuellen Computer berichtet, und integrieren Sie den virtuellen Computer, indem Sie die Bereichskonfiguration ändern.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>VM-Betriebssystemversion oder -Verteilung wird nicht unterstützt.

**Ursache:** Die Lösung wird nicht für alle Linux-Verteilungen oder alle Versionen von Windows unterstützt.

**Lösung:** Lesen Sie die [Liste der unterstützten Clients](automation-update-management.md#clients) für die Lösung.

### <a name="classic-vms-cannot-be-enabled"></a>Klassische VMs können nicht aktiviert werden.

**Ursache**: Virtuelle Computer, die das klassische Bereitstellungsmodell verwenden, werden nicht unterstützt.

**Lösung**: Migrieren Sie den virtuellen Computer zum Resource Manager-Bereitstellungsmodell. Informationen zur Vorgehensweise finden Sie in [Migrieren klassischer Bereitstellungsmodellressourcen](../virtual-machines/windows/migration-classic-resource-manager-overview.md)

### <a name="vm-is-stopped-deallocated"></a>Die VM wurde beendet. (Zuordnung aufgehoben)

**Ursache**: Der virtuelle Computer ist nicht im Status **Wird ausgeführt**.

**Lösung**: Um einen virtuellen Computer in eine Lösung zu integrieren, muss dieser ausgeführt werden. Klicken Sie auf den Inlinelink**VM starten**, um den virtuellen Computer zu starten, ohne die Seite zu verlassen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Lösung für Ihren virtuellen Computer aktiviert wurde, lesen Sie den Übersichtsartikel zur Updateverwaltung, um zu erfahren, wie Sie die Updatebewertung für Ihren Computer anzeigen können.

> [!div class="nextstepaction"]
> [Updateverwaltung – Anzeigen der Updatebewertung](./automation-update-management.md#viewing-update-assessments)

Zusätzliche Tutorials zu den Lösungen und ihrer Verwendung:

* [Tutorial: Verwalten von Updates für den virtuellen Computer](automation-tutorial-update-management.md)

* [Tutorial: Identifizieren von Software auf einem virtuellen Computer](automation-tutorial-installed-software.md)

* [Tutorial: Problembehandlung bei Änderungen auf einem virtuellen Computer](automation-tutorial-troubleshoot-changes.md)