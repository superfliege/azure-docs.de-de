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
ms.openlocfilehash: bb9c19bb489873d1a2175f4a85f7654a3bf099b8
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2017
---
# <a name="manage-updates-for-multiple-machines"></a>Verwalten von Updates für mehrere Computer

Sie können die Updateverwaltung verwenden, um Updates und Patches für Ihre Windows- und Linux-Computer zu verwalten. Über Ihr [Azure Automation](automation-offering-get-started.md)-Konto können Sie:

- Virtuelle Computer integrieren
- Den Status verfügbarer Updates bewerten
- Die Installation erforderlicher Updates planen
- Bereitstellungsergebnisse überprüfen, um sicherzustellen, dass Updates erfolgreich auf alle virtuellen Computer angewendet wurden, für die die Updateverwaltung aktiviert ist

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden der Updateverwaltung benötigen Sie Folgendes:

* Ein ausführendes Azure Automation-Konto. Anweisungen zur Erstellung finden Sie unter [Erste Schritte mit Azure Automation](automation-offering-get-started.md).

* Einen virtuellen Computer bzw. einen Computer, auf dem eines der unterstützten Betriebssysteme installiert ist.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die Updateverwaltung wird für die unten angegebenen Betriebssysteme unterstützt.

### <a name="windows"></a>Windows

* Windows Server 2008 und höher und Updatebereitstellungen für Windows Server 2008 R2 SP1 und höher. Server Core- und Nano Server-Installationsoptionen werden nicht unterstützt.

  Damit die Bereitstellung von Updates für Windows Server 2008 R2 SP1 unterstützt wird, sind .NET Framework 4.5 und Windows Management Framework 5.0 oder höher erforderlich.

* Windows-Clientbetriebssysteme werden nicht unterstützt.

Windows-Agents müssen entweder für die Kommunikation mit einem WSUS-Server (Windows Server Update Services) konfiguriert sein oder über Zugriff auf Microsoft-Update verfügen.

> [!NOTE]
> System Center Configuration Manager kann den Windows-Agent nicht gleichzeitig verwalten.
>

### <a name="linux"></a>Linux

* CentOS 6 (x86/x64) und 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) und 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)  
* Ubuntu 12.04 LTS und höher (x86/x64)   

> [!NOTE]  
> Damit unter Ubuntu keine Updates außerhalb der Wartungsfenster angewendet werden, konfigurieren Sie das „Unattended-Upgrade“-Paket erneut, um automatische Updates zu deaktivieren. Weitere Informationen finden Sie im [Thema zu automatischen Updates im Ubuntu-Serverhandbuch](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.

Diese Lösung unterstützt keinen OMS-Agent für Linux, der zur Berichterstattung an mehrere Operations Management Suite-Arbeitsbereiche konfiguriert ist.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Aktivieren der Updateverwaltung für virtuelle Azure-Computer

1. Öffnen Sie das Automation-Konto im Azure-Portal.
2. Wählen Sie im linken Bereich **Updateverwaltung** aus.
3. Wählen Sie oben im Fenster **Azure-VM hinzufügen** aus.
   ![Registerkarte „Azure-VM hinzufügen“](./media/manage-update-multi/update-onboard-vm.png)
4. Wählen Sie einen virtuellen Computer aus, der integriert werden soll. Das Dialogfeld **Updateverwaltung aktivieren** wird geöffnet.
5. Wählen Sie **Aktivieren** aus.

   ![Dialogfeld „Updateverwaltung aktivieren“](./media/manage-update-multi/update-enable.png)

Die Updateverwaltung ist für Ihren virtuellen Computer aktiviert.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Aktivieren der Updateverwaltung für VMs und Computer, die nicht Azure unterstehen

Eine Anleitung zur Aktivierung der Updateverwaltung für Windows-VMs und -Computer, die nicht Azure unterstehen, finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](../log-analytics/log-analytics-windows-agents.md).

Anweisungen zum Aktivieren der Updateverwaltung für Azure-fremde Linux-VMs und -Computer finden Sie unter [Verbinden Ihrer Linux-Computer mit Log Analytics](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-an-update-assessment"></a>Anzeigen einer Updatebewertung

Nachdem die Updateverwaltung aktiviert wurde, wird das Dialogfeld **Updateverwaltung** angezeigt. Auf der Registerkarte **Fehlende Updates** wird eine Liste der fehlenden Updates angezeigt.

## <a name="collect-data"></a>Sammeln von Daten

Mit Agents, die auf VMs und Computern installiert sind, werden Daten zu Updates gesammelt und an die Azure-Updateverwaltung gesendet.

### <a name="supported-agents"></a>Unterstützte Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von dieser Lösung unterstützt werden:

| Verbundene Quelle | Unterstützt | Beschreibung |
| --- | --- | --- |
| Windows-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates aus Windows-Agents und initiiert die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe. |
| Azure-Speicherkonto |Nein |Azure-Speicher enthält keine Informationen zu Systemupdates. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Für jeden verwalteten Windows-Computer wird zweimal pro Tag ein Scanvorgang ausgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn ja, wird ein Konformitätsscan initiiert. Für jeden verwalteten Linux-Computer wird alle drei Stunden ein Scanvorgang ausgeführt.

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Planen Sie zum Installieren von Updates eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht.
Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

Um eine neue Updatebereitstellung für einen oder mehrere virtuelle Computer zu planen, klicken Sie oben im Dialogfeld **Updateverwaltung** auf **Updatebereitstellung planen**. Geben Sie im Bereich **Neue Updatebereitstellung** die folgenden Informationen ein:

* **Name:** Geben Sie einen eindeutigen Namen zur Identifizierung der Updatebereitstellung an.
* **Betriebssystemtyp:** Wählen Sie Windows oder Linux aus.
* **Zu aktualisierende Computer:** Wählen Sie die virtuellen Computer aus, die Sie aktualisieren möchten.

  ![Bereich „Neue Updatebereitstellung“](./media/manage-update-multi/update-select-computers.png)

* **Updateklassifizierung:** Wählen Sie die Softwaretypen aus, die in die Updatebereitstellung eingeschlossen werden. Es gibt die folgenden Klassifizierungstypen:
  * Kritische Updates
  * Sicherheitsupdates
  * Updaterollups
  * Feature Packs
  * Service Packs
  * Definitionsupdates
  * Tools
  * Aktualisierungen
* **Zeitplaneinstellungen**: Sie können das Standarddatum und die Standarduhrzeit (30 Minuten nach der aktuellen Zeit) übernehmen. Alternativ können Sie eine andere Zeit angeben.
   Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten. Wählen Sie zum Einrichten eines sich wiederholenden Zeitplans unter **Wiederholung** die Option **Serie** aus.

   ![Dialogfeld „Zeitplaneinstellungen“](./media/manage-update-multi/update-set-schedule.png)

* **Wartungsfenster (Minuten):** Geben Sie den Zeitraum an, in dem die Updatebereitstellung stattfinden soll. Mit dieser Einstellung können Sie sicherstellen, dass Änderungen in den von Ihnen festgelegten Wartungsfenstern ausgeführt werden.

Nachdem Sie die Konfiguration des Zeitplans abgeschlossen haben, klicken Sie auf die Schaltfläche **Erstellen**, um zum Statusdashboard zurückzukehren. Die Tabelle **Geplant** zeigt den von Ihnen erstellten Bereitstellungszeitplan.

> [!WARNING]
> Für Updates, die einen Neustart erfordern, wird der virtuelle Computer automatisch neu gestartet.

## <a name="view-results-of-an-update-deployment"></a>Anzeigen der Ergebnisse einer Updatebereitstellung

Nach dem Start der geplanten Bereitstellung können Sie den Status der Bereitstellung auf der Registerkarte **Updatebereitstellungen** des Bildschirms **Updateverwaltung** einsehen.
Wenn die Bereitstellung derzeit ausgeführt wird, lautet der Status **In Bearbeitung**. Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, ändert sich der Status in **Erfolgreich**.
Wenn bei einem oder mehreren Updates in der Bereitstellung ein Fehler auftritt, wird der Status **Der Vorgang ist teilweise fehlgeschlagen** angezeigt.

![Status der Updatebereitstellung](./media/manage-update-multi/update-view-results.png)

Klicken Sie auf die abgeschlossene Bereitstellung, um das Dashboard für eine Updatebereitstellung anzuzeigen.

Im Bereich **Updateergebnisse** werden die Gesamtzahl von Updates und die Ergebnisse der Bereitstellung auf dem virtuellen Computer angezeigt.
Die Tabelle rechts enthält eine detaillierte Aufschlüsselung der einzelnen Updates und die Installationsergebnisse. Bei den Installationsergebnissen kann es sich um einen der folgenden Werte handeln:

* Kein Versuch erfolgt: Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
* Erfolgreich: Das Update wurde erfolgreich ausgeführt.
* Fehler: Beim Update ist ein Fehler aufgetreten.

Klicken Sie auf **Alle Protokolle**, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Klicken Sie auf die Kachel **Ausgabe**, um den Auftragsdatenstrom des Runbooks anzuzeigen, das die Updatebereitstellung auf der Ziel-VM verwaltet.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern bei der Bereitstellung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Updateverwaltung – einschließlich der Protokolle, Ausgabe und Fehler – finden Sie unter [Lösung für die Updateverwaltung in OMS](../operations-management-suite/oms-solution-update-management.md).

