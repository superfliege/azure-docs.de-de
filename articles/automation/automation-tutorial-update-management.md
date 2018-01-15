---
title: "Verwalten von Updates und Patches für Ihre Azure Windows-VMs | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Übersicht über die Verwendung der Updateverwaltung von Azure Automation zum Verwalten von Updates und Patches für Ihre Azure Windows-VMs."
services: automation
documentationcenter: 
author: zjalexander
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: ae53832d28af8a36e3a1dea67f6680906630a399
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2018
---
# <a name="manage-windows-updates-with-azure-automation"></a>Verwalten von Windows-Updates mit Azure Automation

Mithilfe der Updateverwaltung können Sie Updates und Patches für Ihre virtuellen Computer verwalten.
In diesem Tutorial wird beschrieben, wie Sie den Status der verfügbaren Updates schnell bewerten, die Installation der erforderlichen Updates planen und die Bereitstellungsergebnisse prüfen können, um sicherzustellen, dass die Updates erfolgreich angewendet werden.

Informationen zu den Preisen finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/) unter „Updateverwaltung“.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Integrieren einer VM für die Updateverwaltung
> * Anzeigen einer Updatebewertung
> * Planen einer Updatebereitstellung
> * Anzeigen der Ergebnisse einer Bereitstellung

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* Ein [Automation-Konto](automation-offering-get-started.md) für die Watcher- und Aktionsrunbooks und den Watchertask.
* Einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md), der integriert werden soll.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

Für dieses Tutorial müssen Sie zuerst die Updateverwaltung für Ihre VM aktivieren. Dieser Schritt ist nicht erforderlich, falls Sie zuvor eine andere Automatisierungslösung für eine VM aktiviert haben.

1. Wählen Sie im Menü auf der linken Seite die Option **Virtuelle Computer**, und wählen Sie in der Liste eine VM aus.
2. Klicken Sie im Menü auf der linken Seite unter dem Abschnitt **Vorgänge** auf **Updateverwaltung**. Die Seite **Updateverwaltung aktivieren** wird geöffnet.

Eine Überprüfung wird ausgeführt, um festzustellen, ob die Updateverwaltung für diesen virtuellen Computer aktiviert ist.
Die Überprüfung umfasst Prüfungen für einen Log Analytics-Arbeitsbereich und ein verknüpftes Automation-Konto, und ob die Lösung im Arbeitsbereich vorhanden ist.

Mit einem [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich werden Daten gesammelt, die von Features und Diensten wie der Updateverwaltung generiert werden.
Der Arbeitsbereich ist ein zentraler Ort zum Überprüfen und Analysieren von Daten aus mehreren Quellen.
Um weitere Aktionen auf virtuellen Computern auszuführen, die Updates erfordern, können Sie mit Azure Automation Runbooks für virtuelle Computer ausführen, z.B. um Updates herunterzuladen und anzuwenden.

Der Überprüfungsprozess prüft auch, ob der virtuelle Computer mit dem Microsoft Monitoring Agent (MMA) und Hybrid Worker bereitgestellt wird.
Dieser Agent wird verwendet, um mit dem virtuellen Computer zu kommunizieren und Informationen zum Updatestatus abzurufen.
Der Überprüfungsprozess prüft auch, ob der virtuelle Computer mit dem Microsoft Monitoring Agent (MMA) und Automation Hybrid Runbook Worker bereitgestellt wird.

Wenn diese Voraussetzungen nicht erfüllt sind, wird ein Banner angezeigt, über das Sie die Lösung aktivieren können.

![Integriertes Banner zur Konfiguration der Updateverwaltung](./media/automation-tutorial-update-management/manageupdates-onboard-solution-banner.png)

Klicken Sie auf das Banner, um die Lösung zu aktivieren.
Wenn bei der Überprüfung festgestellt wird, dass eine der folgenden Voraussetzungen fehlt, wird sie automatisch hinzugefügt:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich
* [Automation](./automation-offering-get-started.md)
* Ein [Hybrid Runbook Worker](./automation-hybrid-runbook-worker.md) ist auf dem virtuellen Computer aktiviert.

Der Bildschirm **Updateverwaltung** wird geöffnet. Konfigurieren Sie den gewünschten Standort, den Log Analytics-Arbeitsbereich und das Automation-Konto, und klicken Sie auf **Aktivieren**. Wenn die Felder ausgegraut sind, bedeutet dies, dass eine andere Automatisierungslösung für die VM aktiviert ist und derselbe Arbeitsbereich und dasselbe Automation-Konto verwendet werden müssen.

![Fenster zum Aktivieren der Lösung für die Updateverwaltung](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Das Aktivieren der Lösung kann bis zu 15 Minuten dauern. Während dieses Zeitraums sollten Sie das Browserfenster nicht schließen.
Nachdem die Lösung aktiviert wurde, werden Informationen zu fehlenden Updates auf dem virtuellen Computer an Log Analytics übermittelt.
Es kann zwischen 30 Minuten und 6 Stunden dauern, bis die Daten für die Analyse verfügbar sind.

## <a name="view-update-assessment"></a>Anzeigen der Updatebewertung

Sobald die **Updateverwaltung** aktiviert ist, wird der Bildschirm **Updateverwaltung** angezeigt.
Falls Updates fehlen, wird auf der Registerkarte **Fehlende Updates** eine Liste mit den entsprechenden Updates angezeigt.

Wählen Sie den **INFORMATIONSLINK** des jeweiligen Updates aus, um den Supportartikel für das Update in einem neuen Fenster zu öffnen. Er enthält wichtige Informationen zum Update.

![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Wenn Sie im Update auf eine andere Stelle klicken, wird das Fenster **Protokollsuche** für das ausgewählte Update geöffnet. Die Abfrage für die Protokollsuche ist für das jeweilige Update vordefiniert. Sie können diese Abfrage ändern oder Ihre eigene Abfrage erstellen, um ausführliche Informationen zu den Updates anzuzeigen, die in Ihrer Umgebung bereitgestellt wurden bzw. fehlen.

![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/logsearch.png)

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Sie wissen jetzt, dass für Ihre VM Updates fehlen. Planen Sie zum Installieren von Updates eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht.
Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen.
Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

> [!WARNING]
> Falls für Updates ein Neustart erforderlich ist, wird der virtuelle Computer automatisch neu gestartet.

Planen Sie eine neue Updatebereitstellung für die VM, indem Sie zurück zu **Updateverwaltung** navigieren und oben auf dem Bildschirm die Option **Updatebereitstellung planen** wählen.

Geben Sie auf dem Bildschirm **Neue Updatebereitstellung** die folgenden Informationen ein:

* **Name:** Geben Sie einen eindeutigen Namen für die Updatebereitstellung an.
* **Updateklassifizierung:** Wählen Sie die Softwareklassen aus, die in die Updatebereitstellung eingeschlossen werden sollen. Lassen Sie für dieses Tutorial alle Typen ausgewählt.

  Es gibt die folgenden Klassifizierungstypen:

  * Kritische Updates
  * Sicherheitsupdates
  * Updaterollups
  * Feature Packs
  * Service Packs
  * Definitionsupdates
  * Tools
  * Aktualisierungen

* **Zeitplaneinstellungen:** Legen Sie fest, dass zukünftig ein Zeitraum von fünf Minuten verwendet werden soll. Sie können auch die Standardeinstellung übernehmen (30 Minuten nach dem aktuellen Zeitpunkt).
Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten.
Wählen Sie unter **Wiederholung** die Option **Wiederholung**. Übernehmen Sie den Standardwert von einem Tag, und klicken Sie auf **OK**. Ein Zeitplan für Wiederholung wird eingerichtet.

* **Wartungsfenster (Minuten):** Übernehmen Sie hierfür den Standardwert. Sie können den Zeitraum angeben, in dem die Updatebereitstellung stattfinden soll. Mit dieser Einstellung können Sie sicherstellen, dass Änderungen in den von Ihnen festgelegten Wartungsfenstern ausgeführt werden.

![Bildschirm für Updatezeitplan-Einstellungen](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Klicken Sie auf die Schaltfläche **Erstellen**, nachdem Sie die Konfiguration des Zeitplans abgeschlossen haben. Das Statusdashboard wird wieder angezeigt. Wählen Sie **Geplante Updatebereitstellungen**, um den Bereitstellungszeitplan anzuzeigen.

## <a name="view-results-of-an-update-deployment"></a>Anzeigen der Ergebnisse einer Updatebereitstellung

Nach dem Start der geplanten Bereitstellung sehen Sie den Status der Bereitstellung auf der Registerkarte **Updatebereitstellungen** auf dem Bildschirm **Updateverwaltung**.
Der Status **In Bearbeitung** wird angezeigt, wenn gerade eine Bearbeitung durchgeführt wird.
Nach erfolgreichem Abschluss ändert sich der Status in **Erfolgreich**.
Wenn bei einem oder mehreren Updates in der Bereitstellung Fehler auftreten, wird der Status **Der Vorgang ist teilweise fehlgeschlagen** angezeigt.
Klicken Sie auf die abgeschlossene Updatebereitstellung, um das Dashboard für diese Updatebereitstellung anzuzeigen.

![Updatebereitstellungs-Statusdashboard für bestimmte Bereitstellung](./media/automation-tutorial-update-management/manageupdates-view-results.png)

Auf der Kachel **Updateergebnisse** werden eine Zusammenfassung der Gesamtzahl der Updates und der Ergebnisse der Bereitstellung auf dem virtuellen Computer angegeben.
Die Tabelle rechts enthält eine detaillierte Aufschlüsselung der einzelnen Updates und die Installationsergebnisse.
In der folgenden Liste sind die verfügbaren Werte aufgeführt:

* **Kein Versuch erfolgt:** Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
* **Erfolgreich:** Das Update wurde erfolgreich ausgeführt.
* **Fehler:** Bei der Aktualisierung ist ein Fehler aufgetreten.

Klicken Sie auf **Alle Protokolle**, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Klicken Sie auf die Kachel **Ausgabe**, um den Auftragsdatenstrom des Runbooks anzuzeigen, dass für die Verwaltung der Updatebereitstellung auf der Ziel-VM verantwortlich ist.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern aus der Bereitstellung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Integrieren einer VM für die Updateverwaltung
> * Anzeigen einer Updatebewertung
> * Planen einer Updatebereitstellung
> * Anzeigen der Ergebnisse einer Bereitstellung

Fahren Sie mit der Übersicht über die Lösung für die Updateverwaltung fort.

> [!div class="nextstepaction"]
> [Lösung für die Updateverwaltung](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)