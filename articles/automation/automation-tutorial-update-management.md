---
title: Verwalten von Updates und Patches für Ihre virtuellen Azure Windows-Computer
description: Dieser Artikel enthält eine Übersicht über die Verwendung der Updateverwaltung von Azure Automation zum Verwalten von Updates und Patches für Ihre Azure Windows-VMs.
services: automation
author: zjalexander
ms.service: automation
ms.component: update-management
ms.topic: tutorial
ms.date: 02/28/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 84ec2a5852e6aaeb4b9fe6ef11924209d03fb54b
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
---
# <a name="manage-windows-updates-with-azure-automation"></a>Verwalten von Windows-Updates mit Azure Automation

Mithilfe der Updateverwaltung können Sie Updates und Patches für Ihre virtuellen Computer verwalten.
In diesem Tutorial wird beschrieben, wie Sie den Status der verfügbaren Updates schnell bewerten, die Installation der erforderlichen Updates planen, die Bereitstellungsergebnisse prüfen und eine Warnung erstellen, um sich zu vergewissern, ob die Updates erfolgreich angewendet werden.

Informationen zu den Preisen finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/) unter „Updateverwaltung“.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Integrieren einer VM für die Updateverwaltung
> * Anzeigen einer Updatebewertung
> * Konfigurieren von Warnungen
> * Planen einer Updatebereitstellung
> * Anzeigen der Ergebnisse einer Bereitstellung

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie noch kein Abonnement besitzen, können Sie [Ihre monatliche Azure-Gutschrift für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* Ein [Automation-Konto](automation-offering-get-started.md) für die Watcher- und Aktionsrunbooks und den Watchertask.
* Einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md), der integriert werden soll.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

Für dieses Tutorial müssen Sie zuerst die Updateverwaltung auf Ihrer VM aktivieren.

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Virtuelle Computer**, und wählen Sie in der Liste eine VM aus.
2. Klicken Sie auf der VM-Seite im Abschnitt **Vorgänge** auf **Updateverwaltung**. Die Seite **Updateverwaltung aktivieren** wird geöffnet.

Eine Überprüfung wird ausgeführt, um festzustellen, ob die Updateverwaltung für diesen virtuellen Computer aktiviert ist. Diese Überprüfung umfasst Prüfungen auf einen Log Analytics-Arbeitsbereich und ein verknüpftes Automation-Konto, und es wird auch verifiziert, ob die Lösung für die Updateverwaltung im Arbeitsbereich vorhanden ist.

Mit einem [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich werden Daten gesammelt, die von Features und Diensten wie der Updateverwaltung generiert werden. Der Arbeitsbereich ist ein zentraler Ort zum Überprüfen und Analysieren von Daten aus mehreren Quellen.

Der Überprüfungsprozess prüft auch, ob der virtuelle Computer mit dem Microsoft Monitoring Agent (MMA) und Automation Hybrid Runbook Worker bereitgestellt wird.
Dieser Agent wird verwendet, um mit Azure Automation zu kommunizieren und Informationen zum Updatestatus abzurufen. Für den Agent muss Port 443 geöffnet sein, um mit dem Azure Automation-Dienst zu kommunizieren und Updates herunterzuladen.

Wenn beim Onboarding festgestellt wird, dass eine der folgenden Voraussetzungen fehlt, wird sie automatisch hinzugefügt:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich
* [Automation-Konto](./automation-offering-get-started.md)
* Ein [Hybrid Runbook Worker](./automation-hybrid-runbook-worker.md) ist auf dem virtuellen Computer aktiviert.

Der Bildschirm **Updateverwaltung** wird geöffnet. Konfigurieren Sie den gewünschten Standort, den Log Analytics-Arbeitsbereich und das Automation-Konto, und klicken Sie auf **Aktivieren**. Wenn die Felder ausgegraut sind, bedeutet dies, dass eine andere Automatisierungslösung für die VM aktiviert ist und derselbe Arbeitsbereich und dasselbe Automation-Konto verwendet werden müssen.

![Fenster zum Aktivieren der Lösung für die Updateverwaltung](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Das Aktivieren der Lösung kann einige Minuten dauern. Während dieses Zeitraums sollten Sie das Browserfenster nicht schließen.
Nachdem die Lösung aktiviert wurde, werden Informationen zu fehlenden Updates auf dem virtuellen Computer an Log Analytics übermittelt.
Es kann zwischen 30 Minuten und 6 Stunden dauern, bis die Daten für die Analyse verfügbar sind.

## <a name="view-update-assessment"></a>Anzeigen der Updatebewertung

Sobald die **Updateverwaltung** aktiviert ist, wird der Bildschirm **Updateverwaltung** angezeigt.
Falls Updates fehlen, wird auf der Registerkarte **Fehlende Updates** eine Liste mit den entsprechenden Updates angezeigt.

Wählen Sie den **INFORMATIONSLINK** des jeweiligen Updates aus, um den Supportartikel für das Update in einem neuen Fenster zu öffnen. Er enthält wichtige Informationen zum Update.

![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Wenn Sie im Update auf eine andere Stelle klicken, wird das Fenster **Protokollsuche** für das ausgewählte Update geöffnet. Die Abfrage für die Protokollsuche ist für das jeweilige Update vordefiniert. Sie können diese Abfrage ändern oder Ihre eigene Abfrage erstellen, um ausführliche Informationen zu den Updates anzuzeigen, die in Ihrer Umgebung bereitgestellt wurden bzw. fehlen.

![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerting"></a>Konfigurieren von Warnungen

In diesem Schritt konfigurieren Sie eine Warnung, um darüber informiert zu werden, dass Updates erfolgreich bereitgestellt wurden. Die von Ihnen erstellte Warnung basiert auf einer Log Analytics-Abfrage. Sie können beliebige weitere benutzerdefinierte Abfragen für zusätzliche Warnungen schreiben, um viele verschiedene Szenarien abzudecken. Navigieren Sie im Azure-Portal zu **Überwachen**, und klicken Sie dann auf **Warnung erstellen**. Die Seite **Regel erstellen** wird geöffnet.

Klicken Sie unter **1. Warnungsbedingung definieren** auf **+ Ziel auswählen**. Wählen Sie unter **Nach Ressourcentyp filtern** die Option **Log Analytics**. Wählen Sie Ihren Log Analytics-Arbeitsbereich aus, und klicken Sie auf **Fertig**.

![Erstellen einer Warnung](./media/automation-tutorial-update-management/create-alert.png)

Klicken Sie auf die Schaltfläche **+ Kriterien hinzufügen**, um die Seite **Signallogik konfigurieren** zu öffnen. Wählen Sie in der Tabelle die Option **Benutzerdefinierte Protokollsuche**. Geben Sie im Textfeld **Suchabfrage** die folgende Abfrage ein. Mit dieser Abfrage werden die Computer und der Name der Updateausführung zurückgegeben, die im angegebenen Zeitraum durchgeführt wurde.

```loganalytics
UpdateRunProgress
| where InstallationStatus == 'Succeeded'
| where TimeGenerated > now(-10m)
| summarize by UpdateRunName, Computer
```

Geben Sie **1** als **Schwellenwert** für die Warnungslogik ein. Klicken Sie abschließend auf **Fertig**.

![Konfigurieren der Signallogik](./media/automation-tutorial-update-management/signal-logic.png)

Geben Sie unter **2. Warnungsdetails definieren** einen Anzeigenamen und eine Beschreibung für die Warnung an. Legen Sie **Schweregrad** auf **Information (Schweregrad 2)** fest, da die Warnung für eine erfolgreiche Ausführung bestimmt ist.

![Konfigurieren der Signallogik](./media/automation-tutorial-update-management/define-alert-details.png)

Klicken Sie unter **3. Aktionsgruppe definieren** auf **+ Neue Aktionsgruppe**. Eine Aktionsgruppe ist eine Gruppe mit Aktionen, die Sie übergreifend für mehrere Warnungen verwenden können. Dies können beispielsweise E-Mail-Benachrichtigungen, Runbooks, Webhooks und vieles mehr sein. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../monitoring-and-diagnostics/monitoring-action-groups.md).

Geben Sie im Feld **Name der Aktionsgruppe** einen Anzeigenamen und einen Kurznamen an. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

Geben Sie der Aktion unter **Aktionen** einen Anzeigenamen wie **E-Mail-Benachrichtigungen**, und wählen Sie unter **AKTIONSTYP** die Option **E-Mail/SMS/Push/Sprachanruf**. Wählen Sie unter **DETAILS** die Option **Details bearbeiten**.

Vergeben Sie auf der Seite **E-Mail/SMS/Push/Sprachanruf** einen Namen. Aktivieren Sie das Kontrollkästchen **E-Mail**, und geben Sie eine gültige E-Mail-Adresse ein.

![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-tutorial-update-management/configure-email-action-group.png)

Klicken Sie auf der Seite **E-Mail/SMS/Push/Sprachanruf** auf **OK**, um sie zu schließen, und anschließend erneut auf **OK**, um die Seite **Aktionsgruppe hinzuzufügen** zu schließen.

Sie können den Betreff der gesendeten E-Mail anpassen, indem Sie auf der Seite **Regel erstellen** unter **Aktionen anpassen** auf **E-Mail-Betreff** klicken. Klicken Sie auf **Warnungsregel erstellen**, wenn Sie fertig sind. Es wird eine Regel erstellt, bei der Sie eine Warnung erhalten, wenn die Bereitstellung eines Updates erfolgreich war. Außerdem wird angegeben, welche Computer Teil der durchgeführten Updatebereitstellung waren.

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Nachdem Sie die Warnungen konfiguriert haben, können Sie eine Bereitstellung zum Installieren von Updates planen, die Ihrem Releasezeitplan und Wartungsfenster entspricht.
Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen.
Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

> [!WARNING]
> Falls für Updates ein Neustart erforderlich ist, wird der virtuelle Computer automatisch neu gestartet.

Planen Sie eine neue Updatebereitstellung für die VM, indem Sie zurück zu **Updateverwaltung** navigieren und oben auf dem Bildschirm die Option **Updatebereitstellung planen** wählen.

Geben Sie auf dem Bildschirm **Neue Updatebereitstellung** die folgenden Informationen ein:

* **Name:** Geben Sie einen eindeutigen Namen für die Updatebereitstellung an.

* **Betriebssystem:** Wählen Sie das Betriebssystem, das als Ziel für die Updatebereitstellung dienen soll.

* **Updateklassifizierung:** Wählen Sie die Softwareklassen aus, die in die Updatebereitstellung eingeschlossen werden sollen. Lassen Sie für dieses Tutorial alle Typen ausgewählt.

  Es gibt die folgenden Klassifizierungstypen:

   |Betriebssystem  |Typ  |
   |---------|---------|
   |Windows     | Kritische Updates</br>Sicherheitsupdates</br>Updaterollups</br>Feature Packs</br>Service Packs</br>Definitionsupdates</br>Tools</br>Aktualisierungen        |
   |Linux     | Kritische Updates und Sicherheitsupdates</br>Andere Updates       |

   Eine Beschreibung der Klassifizierungstypen finden Sie unter [Updateklassifizierungen](automation-update-management.md#update-classifications).

* **Zeitplaneinstellungen:** Dient zum Öffnen der Seite mit den Zeitplaneinstellungen. Der Standard-Startzeitpunkt liegt 30 Minuten nach der aktuellen Uhrzeit. Er kann frei gewählt werden, solange er mindestens 10 Minuten in der Zukunft liegt.

   Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten.
   Wählen Sie unter **Wiederholung** die Option **Einmal**. Übernehmen Sie den Standardwert von einem Tag, und klicken Sie auf **OK**. Ein Zeitplan für Wiederholung wird eingerichtet.

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

Nachdem die Updatebereitstellung erfolgreich abgeschlossen wurde, wird eine E-Mail gesendet (in etwa wie in der folgenden Abbildung), um den Erfolg der Bereitstellung anzugeben.

![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Integrieren einer VM für die Updateverwaltung
> * Anzeigen einer Updatebewertung
> * Konfigurieren von Warnungen
> * Planen einer Updatebereitstellung
> * Anzeigen der Ergebnisse einer Bereitstellung

Fahren Sie mit der Übersicht über die Lösung für die Updateverwaltung fort.

> [!div class="nextstepaction"]
> [Lösung für die Updateverwaltung](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)
