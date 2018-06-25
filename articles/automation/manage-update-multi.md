---
title: Verwalten von Updates für mehrere virtuelle Azure-Computer
description: In diesem Thema wird beschrieben, wie Sie Updates für virtuelle Computer verwalten.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 658686bec41fe1a6cfa8ca4ba6fe61d2e559297c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833718"
---
# <a name="manage-updates-for-multiple-machines"></a>Verwalten von Updates für mehrere Computer

Sie können die Updateverwaltung verwenden, um Updates und Patches für Ihre Windows- und Linux-Computer zu verwalten. Über Ihr [Azure Automation](automation-offering-get-started.md)-Konto können Sie:

- Virtuelle Computer integrieren
- Den Status verfügbarer Updates bewerten
- Die Installation erforderlicher Updates planen
- Bereitstellungsergebnisse überprüfen, um sicherzustellen, dass Updates erfolgreich auf alle virtuellen Computer angewendet wurden, für die die Updateverwaltung aktiviert ist

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden der Updateverwaltung benötigen Sie Folgendes:

- Ein ausführendes Azure Automation-Konto. Anweisungen zur Erstellung finden Sie unter [Erste Schritte mit Azure Automation](automation-offering-get-started.md).

- Einen virtuellen Computer bzw. einen Computer, auf dem eines der unterstützten Betriebssysteme installiert ist.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die Updateverwaltung wird für folgende Betriebssysteme unterstützt:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Unterstützt nur Updatebewertungen         |
|Windows Server 2008 R2 SP1 und höher     |Windows PowerShell 4.0 oder höher erforderlich ([WMF 4.0 herunterladen](https://www.microsoft.com/download/details.aspx?id=40855)).</br> Für eine höhere Zuverlässigkeit wird Windows PowerShell 5.1 ([WMF 5.1 herunterladen](https://www.microsoft.com/download/details.aspx?id=54616)) empfohlen.         |
|CentOS 6 (x86/x64) und 7 (x64)      | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|Red Hat Enterprise 6 (x86/x64) und 7 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)      |Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.         |

> [!NOTE]
> Damit unter Ubuntu keine Updates außerhalb der Wartungsfenster angewendet werden, konfigurieren Sie das „Unattended-Upgrade“-Paket erneut, um automatische Updates zu deaktivieren. Weitere Informationen finden Sie im [Thema zu automatischen Updates im Ubuntu-Serverhandbuch](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.

Diese Lösung bietet keine Unterstützung für einen OMS-Agent für Linux, der für die Berichterstattung an mehrere Log Analytics-Arbeitsbereiche konfiguriert ist.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Aktivieren der Updateverwaltung für virtuelle Azure-Computer

Öffnen Sie im Azure-Portal Ihr Automation-Konto, und wählen Sie **Updateverwaltung** aus.

Wählen Sie oben im Fenster **Azure-VM hinzufügen** aus.

![Registerkarte „Azure-VM hinzufügen“](./media/manage-update-multi/update-onboard-vm.png)

Wählen Sie einen virtuellen Computer aus, der integriert werden soll. Das Dialogfeld **Updateverwaltung aktivieren** wird geöffnet. Wählen Sie **Aktivieren** aus, um das Onboarding des virtuellen Computers durchzuführen. Nach Durchführung des Onboardings ist die Updateverwaltung für Ihren virtuellen Computer aktiviert.

![Dialogfeld „Updateverwaltung aktivieren“](./media/manage-update-multi/update-enable.png)

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Aktivieren der Updateverwaltung für VMs und Computer, die nicht Azure unterstehen

Eine Anleitung zur Aktivierung der Updateverwaltung für Windows-VMs und -Computer, die nicht Azure unterstehen, finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](../log-analytics/log-analytics-windows-agent.md).

Anweisungen zum Aktivieren der Updateverwaltung für Azure-fremde Linux-VMs und -Computer finden Sie unter [Verbinden Ihrer Linux-Computer mit Log Analytics](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Anzeigen von Computern, die an Ihr Automation-Konto angefügt sind

Nachdem Sie die Updateverwaltung für Ihre Computer aktiviert haben, können Sie die entsprechenden Informationen anzeigen, indem Sie auf **Computer** klicken. Folgende Computerinformationen sind verfügbar: *Name*, *Konformität*, *Umgebung*, *Betriebssystemtyp*, *Wichtige Updates und Sicherheitsupdates*, *Weitere Updates* und *Bereitschaft des Update-Agents*.

  ![Registerkarte „Computer“](./media/manage-update-multi/update-computers-tab.png)

Computer, für die die Updateverwaltung erst vor Kurzem aktiviert wurde, ist ggf. noch keine Bewertung vorhanden. Der Konformitätszustand dieser Computer lautet *Nicht bewertet*.  Hier ist eine Liste mit den Werten zum Konformitätszustand angegeben:

- Konform: Computer, für die keine kritischen Updates oder Sicherheitsupdates fehlen.

- Nicht konform: Computer, für die mindestens ein kritisches Update oder Sicherheitsupdate fehlt.

- Nicht bewertet: Die Daten für die Updatebewertung wurden vom Computer nicht innerhalb des erwarteten Zeitrahmens empfangen.  Für Linux-Computer gilt hierfür ein Zeitraum von drei Stunden, und für Windows-Computer ein Zeitraum von 12 Stunden.

Klicken Sie zum Anzeigen des Agent-Status auf den Link in der Spalte **BEREITSCHAFT DES UPDATE-AGENTS**. Daraufhin wird die Hybrid Worker-Seite geöffnet, auf der Status des Hybrid Worker angezeigt wird. In der folgenden Abbildung wird ein Beispiel für einen Agent gezeigt, der seit längerer Zeit nicht mit der Updateverwaltung verbunden war.

![Registerkarte „Computer“](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Anzeigen einer Updatebewertung

Nachdem die Updateverwaltung aktiviert wurde, wird das Dialogfeld **Updateverwaltung** angezeigt. Auf der Registerkarte **Fehlende Updates** wird eine Liste der fehlenden Updates angezeigt.

## <a name="collect-data"></a>Sammeln von Daten

Mit Agents, die auf VMs und Computern installiert sind, werden Daten zu Updates gesammelt und an die Azure-Updateverwaltung gesendet.

### <a name="supported-agents"></a>Unterstützte Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von dieser Lösung unterstützt werden:

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| Windows-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates aus Windows-Agents und initiiert die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe. |
| Azure-Speicherkonto |Nein  |Azure-Speicher enthält keine Informationen zu Systemupdates. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Für jeden verwalteten Windows-Computer wird zweimal pro Tag ein Scanvorgang ausgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn ja, wird ein Konformitätsscan initiiert. Für jeden verwalteten Linux-Computer wird alle drei Stunden ein Scanvorgang ausgeführt.

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Planen Sie zum Installieren von Updates eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht.
Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

Um eine neue Updatebereitstellung für einen oder mehrere virtuelle Computer zu planen, klicken Sie oben im Dialogfeld **Updateverwaltung** auf **Updatebereitstellung planen**.
Geben Sie im Bereich **Neue Updatebereitstellung** die folgenden Informationen ein:

- **Name:** Geben Sie einen eindeutigen Namen zur Identifizierung der Updatebereitstellung an.
- **Betriebssystemtyp:** Wählen Sie Windows oder Linux aus.
- **Zu aktualisierende Computer**: Wählen Sie die virtuellen Computer aus, die Sie aktualisieren möchten. Die Bereitschaft des Computers wird in der Spalte **BEREITSCHAFT DES UPDATE-AGENTS** angezeigt. Hier können Sie vor der Planung der Updatebereitstellung den Integritätsstatus des Computers sehen.

  ![Bereich „Neue Updatebereitstellung“](./media/manage-update-multi/update-select-computers.png)

- **Updateklassifizierung:** Wählen Sie die Softwaretypen aus, die in die Updatebereitstellung eingeschlossen werden. Eine Beschreibung der Klassifizierungstypen finden Sie unter [Updateklassifizierungen](automation-update-management.md#update-classifications). Es gibt die folgenden Klassifizierungstypen:
  - Kritische Updates
  - Sicherheitsupdates
  - Updaterollups
  - Feature Packs
  - Service Packs
  - Definitionsupdates
  - Tools
  - Aktualisierungen

- **Auszuschließende Updates**: Öffnet die Seite **Ausschließen**. Geben Sie die auszuschließenden KBs oder Paketnamen an.

- **Zeitplaneinstellungen**: Sie können das Standarddatum und die Standarduhrzeit (30 Minuten nach der aktuellen Zeit) übernehmen. Alternativ können Sie eine andere Zeit angeben.
   Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten. Wählen Sie zum Einrichten eines sich wiederholenden Zeitplans unter **Wiederholung** die Option **Serie** aus.

   ![Dialogfeld „Zeitplaneinstellungen“](./media/manage-update-multi/update-set-schedule.png)

- **Wartungsfenster (Minuten):** Geben Sie den Zeitraum an, in dem die Updatebereitstellung stattfinden soll. Mit dieser Einstellung können Sie sicherstellen, dass Änderungen in den von Ihnen festgelegten Wartungsfenstern ausgeführt werden.

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

- Kein Versuch erfolgt: Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
- Erfolgreich: Das Update wurde erfolgreich ausgeführt.
- Fehler: Beim Update ist ein Fehler aufgetreten.

Klicken Sie auf **Alle Protokolle**, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Klicken Sie auf die Kachel **Ausgabe**, um den Auftragsdatenstrom des Runbooks anzuzeigen, das die Updatebereitstellung auf der Ziel-VM verwaltet.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern bei der Bereitstellung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Updateverwaltung – einschließlich der Protokolle, Ausgabe und Fehler – finden Sie unter [Lösung für die Updateverwaltung in Azure](../operations-management-suite/oms-solution-update-management.md).
