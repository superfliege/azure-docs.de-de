---
title: "Verwalten von Updates für mehrere virtuelle Azure-Computer | Microsoft-Dokumentation"
description: "In diesem Thema wird beschrieben, wie Sie Updates für virtuelle Computer verwalten."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2017
ms.author: magoedte;eslesar
ms.openlocfilehash: f97b28d1588e959728163f7ab16d2550a79f610e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="manage-updates-for-multiple-machines"></a>Verwalten von Updates für mehrere Computer

Mithilfe der Updateverwaltung können Sie Updates und Patches für Ihre Windows- und Linux-Computer verwalten.
In Ihrem [Azure Automation](automation-offering-get-started.md)-Konto können Sie Computer schnell integrieren, den Status verfügbarer Updates bewerten, die Installation der erforderlichen Updates planen und Bereitstellungsergebnisse überprüfen. So können Sie sicherstellen, dass Updates erfolgreich auf alle virtuellen Computer angewendet wurden, auf denen die Updateverwaltung aktiviert ist.

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden der Updateverwaltung benötigen Sie Folgendes:

* Ein Azure Automation-Konto. Eine Anleitung zum Erstellen eines ausführenden Azure Automation-Kontos finden Sie unter [Erste Schritte mit Azure Automation](automation-offering-get-started.md).

* Einen virtuellen Computer bzw. einen Computer, auf dem eines der unterstützten Betriebssysteme installiert ist.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die Updateverwaltung wird für die unten angegebenen Betriebssysteme unterstützt.

### <a name="windows"></a>Windows

* Windows Server 2008 und höher und Updatebereitstellungen für Windows Server 2008 R2 SP1 und höher.  Server Core- und Nano Server-Installationsoptionen werden nicht unterstützt.

    > [!NOTE]
    > Damit die Bereitstellung von Updates für Windows Server 2008 R2 SP1 unterstützt wird, sind .NET Framework 4.5 und WMF 5.0 oder höher erforderlich.
    > 
* Windows-Clientbetriebssysteme werden nicht unterstützt.

Windows-Agents müssen entweder für die Kommunikation mit einem WSUS-Server (Windows Server Update Services) konfiguriert sein oder über Zugriff auf Microsoft-Update verfügen.

> [!NOTE]
> Der Windows-Agent kann nicht gleichzeitig mit System Center Configuration Manager verwaltet werden.
>

### <a name="linux"></a>Linux

* CentOS 6 (x86/x64) und 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) und 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)  
* Ubuntu 12.04 LTS und höher (x86/x64)   

> [!NOTE]  
> Damit unter Ubuntu keine Updates außerhalb der Wartungsfenster angewendet werden, konfigurieren Sie das „Unattended-Upgrade“-Paket erneut, um automatische Updates zu deaktivieren. Informationen zu dieser Konfiguration finden Sie im [Thema zu automatischen Updates im Ubuntu-Serverhandbuch](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.

> [!NOTE]
> Ein OMS-Agent für Linux, der für das Melden an mehrere OMS-Arbeitsbereiche konfiguriert ist, wird für diese Lösung nicht unterstützt.  
>

## <a name="enable-update-management-for-azure-virtual-machines"></a>Aktivieren der Updateverwaltung für virtuelle Azure-Computer

1. Öffnen Sie das Automation-Konto im Azure-Portal.
2. Wählen Sie auf der linken Seite des Bildschirms **Updateverwaltung** aus.
3. Klicken Sie am oberen Bildschirmrand auf **Azure-VM hinzufügen**.
    ![Integrieren von VMs](./media/manage-update-multi/update-onboard-vm.png)
4. Wählen Sie einen virtuellen Computer aus, der integriert werden soll. Der Bildschirm **Updateverwaltung aktivieren** wird geöffnet.
5. Klicken Sie auf **Aktivieren**.

   ![Aktivieren der Updateverwaltung](./media/manage-update-multi/update-enable.png)

Die Updateverwaltung ist für Ihren virtuellen Computer aktiviert.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Aktivieren der Updateverwaltung für VMs und Computer, die nicht Azure unterstehen

Eine Anleitung zur Aktivierung der Updateverwaltung für Windows-VMs und -Computer, die nicht Azure unterstehen, finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](../log-analytics/log-analytics-windows-agents.md).

Eine Anleitung zur Aktivierung der Updateverwaltung für Linux-VMs und -Computer, die nicht Azure unterstehen, finden Sie unter [Verbinden Ihrer Linux-Computer mit Operations Management Suite (OMS)](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-update-assessment"></a>Anzeigen der Updatebewertung

Sobald die **Updateverwaltung** aktiviert ist, wird der Bildschirm **Updateverwaltung** angezeigt. Auf der Registerkarte **Fehlende Updates** wird eine Liste der fehlenden Updates angezeigt.

## <a name="data-collection"></a>Datensammlung

Mit Agents, die auf VMs und Computern installiert sind, werden Daten zu Updates gesammelt und an die Azure-Updateverwaltung gesendet.

### <a name="supported-agents"></a>Unterstützte Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Unterstützt | Beschreibung |
| --- | --- | --- |
| Windows-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates aus Windows-Agents und initiiert die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe. |
| Azure-Speicherkonto |Nein |Azure-Speicher enthält keine Informationen zu Systemupdates. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Für jeden verwalteten Windows-Computer wird zweimal pro Tag ein Scanvorgang durchgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn ja, wird ein Konformitätsscan initiiert.  Für jeden verwalteten Linux-Computer wird alle drei Stunden ein Scanvorgang durchgeführt.

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

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

Ausführliche Informationen zu den Protokollen, zur Ausgabe und zu den Fehlerinformationen finden Sie unter [Lösung für die Updateverwaltung in OMS](../operations-management-suite/oms-solution-update-management.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Updateverwaltung finden Sie unter [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md).
