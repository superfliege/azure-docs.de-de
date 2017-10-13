---
title: "Verwalten von Updates für mehrere virtuelle Azure-Computer | Microsoft-Dokumentation"
description: Informationen zum Integrieren virtueller Azure-Computer zum Verwalten von Updates.
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.openlocfilehash: 89bf87f27fdf276068cba261fc6ae1660307e0b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-updates-for-multiple-azure-virtual-machines"></a>Verwalten von Updates für mehrere virtuelle Azure-Computer

Mithilfe der Updateverwaltung können Sie Updates und Patches für Ihre virtuellen Azure-Computer verwalten.
In Ihrem [Azure Automation](automation-offering-get-started.md)-Konto können Sie virtuelle Computer schnell integrieren, den Status verfügbarer Updates bewerten, die Installation der erforderlichen Updates planen und Bereitstellungsergebnisse überprüfen. Sie können so sicherstellen, dass Updates erfolgreich auf alle virtuellen Computer angewendet wurden, auf denen die Updateverwaltung aktiviert ist.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Schritte in dieser Anleitung ausführen können, brauchen Sie Folgendes:

* Ein Azure Automation-Konto. Informationen zum Erstellen eines ausführenden Azure Automation-Kontos finden Sie unter [Azure Run As Account](automation-sec-configure-azure-runas-account.md)(Ausführendes Azure-Konto).
* Einen virtuellen Computer gemäß dem Azure Resource Manager-Modell (nicht klassisch). Eine Anleitung zum Erstellen einer VM finden Sie unter [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="enable-update-management-for-azure-virtual-machines"></a>Aktivieren der Updateverwaltung für virtuelle Azure-Computer

1. Öffnen Sie das Automation-Konto im Azure-Portal.
2. Wählen Sie auf der linken Seite des Bildschirms **Updateverwaltung** aus.
3. Klicken Sie am oberen Bildschirmrand auf **Azure-VM hinzufügen**.
    ![Integrieren von VMs](./media/manage-update-multi/update-onboard-vm.png)
4. Wählen Sie einen virtuellen Computer aus, der integriert werden soll. Der Bildschirm **Updateverwaltung aktivieren** wird geöffnet.
5. Klicken Sie auf **Aktivieren**.

   ![Aktivieren der Updateverwaltung](./media/manage-update-multi/update-enable.png)

Die Updateverwaltung ist für Ihren virtuellen Computer aktiviert.

## <a name="view-update-assessment"></a>Anzeigen der Updatebewertung

Sobald die **Updateverwaltung** aktiviert ist, wird der Bildschirm **Updateverwaltung** angezeigt. Auf der Registerkarte **Fehlende Updates** wird eine Liste der fehlenden Updates angezeigt.

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Planen Sie zum Installieren von Updates eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht.
Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

Um eine neue Updatebereitstellung für einen oder mehrere virtuelle Computer zu planen, klicken Sie am oberen Rand des Bildschirms **Updateverwaltung** auf **Updatebereitstellung planen**. Geben Sie auf dem Bildschirm **Neue Updatebereitstellung** die folgenden Informationen ein:

* **Name:** Geben Sie einen eindeutigen Namen zur Identifizierung der Updatebereitstellung an.
* **Betriebssystemtyp:** Wählen Sie Windows oder Linux.
* **Zu aktualisierende Computer:** Wählen Sie die virtuellen Computer aus, die Sie aktualisieren möchten.

  ![Auswählen der zu aktualisierenden Computer](./media/manage-update-multi/update-select-computers.png)

* **Updateklassifizierung:** Wählen Sie die Softwaretypen aus, die in die Updatebereitstellung eingeschlossen werden sollen. Es gibt die folgenden Klassifizierungstypen:
  * Kritische Updates
  * Sicherheitsupdates
  * Updaterollups
  * Feature Packs
  * Service Packs
  * Definitionsupdates
  * Tools
  * Aktualisierungen
* **Zeitplaneinstellungen**: Sie können entweder Standarddatum und Standarduhrzeit (30 Minuten nach der aktuellen Zeit) akzeptieren oder einen anderen Zeitpunkt angeben.
   Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten. Klicken Sie unter „Wiederholung“ auf die Option „Wiederholt“, um einen sich wiederholenden Zeitplan einzurichten.

   ![Bildschirm für Updatezeitplan-Einstellungen](./media/manage-update-multi/update-set-schedule.png)

* **Wartungsfenster (Minuten):** Geben Sie den Zeitraum an, in dem die Updatebereitstellung stattfinden soll.  So wird sichergestellt, dass Änderungen in dem von Ihnen festgelegten Wartungsfenster ausgeführt werden.

Nachdem Sie die Konfiguration des Zeitplans abgeschlossen haben, klicken Sie auf die Schaltfläche **Erstellen**. Sie kehren damit zum Statusdashboard zurück.
Beachten Sie, dass die Tabelle **Geplant** den von Ihnen erstellten Bereitstellungszeitplan anzeigt.

> [!WARNING]
> Für Updates, die einen Neustart erfordern, wird der virtuelle Computer automatisch neu gestartet.

## <a name="view-results-of-an-update-deployment"></a>Anzeigen der Ergebnisse einer Updatebereitstellung

Nach dem Start der geplanten Bereitstellung sehen Sie den Status der Bereitstellung auf der Registerkarte **Updatebereitstellungen** auf dem Bildschirm **Updateverwaltung**.
Wenn sie aktuell ausgeführt wird, wird der Status **Vorgang wird ausgeführt** angezeigt. Nach erfolgreichem Abschluss ändert sich der Status in **Erfolgreich**.
Wenn bei einem oder mehreren Updates in der Bereitstellung ein Fehler auftritt, wird der Status **Der Vorgang ist teilweise fehlgeschlagen** angezeigt.

![Aktualisieren des Bereitstellungsstatus ](./media/manage-update-multi/update-view-results.png)

Klicken Sie auf die abgeschlossene Updatebereitstellung, um das Dashboard für diese Updatebereitstellung anzuzeigen.

Die Kachel **Updateergebnisse** enthält eine Zusammenfassung der Gesamtzahl der Updates und die Ergebnisse der Bereitstellung auf dem virtuellen Computer.
Die Tabelle rechts enthält eine detaillierte Analyse der einzelnen Updates und die Installationsergebnisse, wobei jeweils einer der folgenden Werte infrage kommt:

* Kein Versuch erfolgt: Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
* Erfolgreich: Das Update wurde erfolgreich ausgeführt.
* Fehler: Bei der Aktualisierung ist ein Fehler aufgetreten.

Klicken Sie auf **Alle Protokolle**, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Klicken Sie auf die Kachel **Ausgabe**, um den Auftragsdatenstrom des Runbooks anzuzeigen, dass für die Verwaltung der Updatebereitstellung auf der Ziel-VM verantwortlich ist.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern aus der Bereitstellung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Updateverwaltung finden Sie unter [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md).