---
title: Verwalten von Updates und Patches für Ihre virtuellen Azure Windows-Computer
description: Dieser Artikel enthält eine Übersicht über die Verwendung der Updateverwaltung von Azure Automation zum Verwalten von Updates und Patches für Ihre virtuellen Azure Windows-Computer.
services: automation
author: zjalexander
ms.service: automation
ms.component: update-management
ms.topic: tutorial
ms.date: 09/18/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 6046781f59b64dcec4769686a2acd710c7b68965
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987306"
---
# <a name="manage-windows-updates-by-using-azure-automation"></a>Verwalten von Windows-Updates mithilfe von Azure Automation

Sie können die Updateverwaltungslösung verwenden, um Updates und Patches für Ihre virtuellen Computer zu verwalten. In diesem Tutorial wird beschrieben, wie Sie den Status der verfügbaren Updates schnell bewerten, die Installation der erforderlichen Updates planen, die Bereitstellungsergebnisse prüfen und eine Warnung erstellen, um sich zu vergewissern, dass die Updates erfolgreich angewendet werden.

Informationen zu den Preisen finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/) unter „Updateverwaltung“.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Integrieren eines virtuellen Computers für die Updateverwaltung
> * Anzeigen einer Updatebewertung
> * Konfigurieren von Warnungen
> * Planen einer Updatebereitstellung
> * Anzeigen der Ergebnisse einer Bereitstellung

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie noch kein Abonnement besitzen, können Sie [Ihre monatliche Azure-Gutschrift für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* Ein [Azure Automation-Konto](automation-offering-get-started.md) für die Watcher- und Aktionsrunbooks und den Watchertask
* Einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md), der integriert werden soll.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

Aktivieren Sie für dieses Tutorial zuerst die Updateverwaltung auf Ihrem virtuellen Computer:

1. Wählen Sie im Azure-Portal im Menü Bereich die Option **Virtuelle Computer**. Wählen Sie einen virtuellen Computer aus der Liste aus.
2. Wählen Sie auf der Seite der virtuellen Computer unter **VORGÄNGE** die Option **Updateverwaltung**. Der Bereich **Updateverwaltung aktivieren** wird geöffnet.

Eine Überprüfung wird ausgeführt, um festzustellen, ob die Updateverwaltung für diesen virtuellen Computer aktiviert ist. Dabei wird auf einen Azure Log Analytics-Arbeitsbereich und ein verknüpftes Automation-Konto geprüft und überprüft, ob die Lösung für die Updateverwaltung im Arbeitsbereich vorhanden ist.

Mit einem [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich werden Daten gesammelt, die von Features und Diensten wie der Updateverwaltung generiert werden. Der Arbeitsbereich ist ein zentraler Ort zum Überprüfen und Analysieren von Daten aus mehreren Quellen.

Bei der Überprüfung wird auch geprüft, ob der virtuelle Computer mit Microsoft Monitoring Agent (MMA) und Automation Hybrid Runbook Worker bereitgestellt wird. Dieser Agent wird verwendet, um mit Azure Automation zu kommunizieren und Informationen zum Updatestatus abzurufen. Für den Agent muss Port 443 geöffnet sein, um mit dem Azure Automation-Dienst zu kommunizieren und Updates herunterzuladen.

Wenn beim Onboarding festgestellt wird, dass eine der folgenden Voraussetzungen fehlt, wird sie automatisch hinzugefügt:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich
* [Automation-Konto](./automation-offering-get-started.md)
* [Hybrid Runbook Worker](./automation-hybrid-runbook-worker.md) (auf dem virtuellen Computer aktiviert)

Legen Sie unter **Updateverwaltung** den Standort, den Log Analytics-Arbeitsbereich und das Automation-Konto fest, die verwendet werden sollen. Wählen Sie anschließend **Aktivieren** aus. Sind diese Optionen nicht verfügbar, bedeutet das, dass eine andere Automatisierungslösung für den virtuellen Computer aktiviert ist. In diesem Fall müssen der gleiche Arbeitsbereich und das gleiche Automation-Konto verwendet werden.

![Fenster zum Aktivieren der Lösung für die Updateverwaltung](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Das Aktivieren der Lösung kann einige Minuten dauern. Schließen Sie das Browserfenster während dieses Zeitraums nicht. Nachdem die Lösung aktiviert wurde, werden Informationen zu fehlenden Updates auf dem virtuellen Computer an Log Analytics übermittelt. Es kann zwischen 30 Minuten und 6 Stunden dauern, bis die Daten für die Analyse verfügbar sind.

## <a name="view-update-assessment"></a>Anzeigen der Updatebewertung

Sobald die Updateverwaltung aktiviert ist, wird der Bereich **Updateverwaltung** angezeigt. Falls Updates fehlen, wird auf der Registerkarte **Fehlende Updates** eine Liste mit den entsprechenden Updates angezeigt.

Klicken Sie unter **INFORMATIONSLINK** auf den Updatelink, um den Supportartikel für das Update in einem neuen Fenster zu öffnen. Dieses Fenster enthält wichtige Informationen zum Update.

![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Klicken Sie im Update auf eine andere Stelle, um den Bereich **Protokollsuche** für das ausgewählte Update zu öffnen. Die Abfrage für die Protokollsuche ist für das jeweilige Update vordefiniert. Sie können diese Abfrage ändern oder Ihre eigene Abfrage erstellen, um ausführliche Informationen zu den Updates anzuzeigen, die in Ihrer Umgebung bereitgestellt wurden bzw. fehlen.

![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Konfigurieren von Warnungen

In diesem Schritt erfahren Sie, wie Sie eine Warnung einrichten, die Sie über die erfolgreiche Bereitstellung von Updates informiert. Dazu können Sie entweder eine Log Analytics-Abfrage verwenden oder das Masterrunbook für die Updateverwaltung auf nicht erfolgreiche Bereitstellungen überwachen.

### <a name="alert-conditions"></a>Warnungsbedingungen

Für jeden Warnungstyp müssen verschiedene Warnungsbedingungen definiert werden.

#### <a name="log-analytics-query-alert"></a>Log Analytics-Abfragewarnung

Für erfolgreiche Bereitstellungen können Sie eine auf einer Log Analytics-Abfrage basierende Warnung erstellen. Bei nicht erfolgreichen Bereitstellungen können Sie die Schritte für [Runbookwarnungen](#runbook-alert) ausführen, um benachrichtigt zu werden, wenn beim Masterrunbook, das Updatebereitstellungen orchestriert, ein Fehler auftritt. Sie können eine benutzerdefinierte Abfrage für zusätzliche Warnungen schreiben, um viele verschiedene Szenarien abzudecken.

Navigieren Sie im Azure-Portal zu **Überwachen**, und klicken Sie dann auf **Warnung erstellen**.

Klicken Sie unter **1. Warnungsbedingung definieren** auf **Ziel auswählen**. Wählen Sie unter **Nach Ressourcentyp filtern** die Option **Log Analytics**. Wählen Sie Ihren Log Analytics-Arbeitsbereich aus, und klicken Sie dann auf **Fertig**.

![Erstellen einer Warnung](./media/automation-tutorial-update-management/create-alert.png)

Klicken Sie auf **Kriterien hinzufügen**.

Klicken Sie unter **Signallogik konfigurieren** in der Tabelle auf **Benutzerdefinierte Protokollsuche**. Geben Sie im Textfeld **Suchabfrage** die folgende Abfrage ein:

```loganalytics
UpdateRunProgress
| where InstallationStatus == 'Succeeded'
| where TimeGenerated > now(-10m)
| summarize by UpdateRunName, Computer
```
Mit dieser Abfrage werden die Computer und der Name der Updateausführung zurückgegeben, die im angegebenen Zeitraum durchgeführt wurde.

Geben Sie unter **Warnungslogik** für **Schwellenwert** den Wert **1** ein. Klicken Sie auf **Fertig**, wenn Sie fertig sind.

![Konfigurieren der Signallogik](./media/automation-tutorial-update-management/signal-logic.png)

#### <a name="runbook-alert"></a>Runbookwarnung

Bei nicht erfolgreichen Bereitstellungen muss auf die Fehlerhaftigkeit des Masterrunbooks aufmerksam gemacht werden.
Navigieren Sie im Azure-Portal zu **Überwachen**, und klicken Sie dann auf **Warnung erstellen**.

Klicken Sie unter **1. Warnungsbedingung definieren** auf **Ziel auswählen**. Wählen Sie unter **Nach Ressourcentyp filtern** die Option **Automation-Konten** aus. Wählen Sie Ihr Automation-Konto aus, und klicken Sie auf **Fertig**.

Klicken Sie neben **Runbookname** auf das Pluszeichen (**\+**), und geben Sie als benutzerdefinierten Namen **Patch-MicrosoftOMSComputers** ein. Wählen Sie als **Status** die Option **Fehler**, oder klicken Sie auf das Pluszeichen (**\+**), um **Fehler** einzugeben.

![Konfigurieren der Signallogik für Runbooks](./media/automation-tutorial-update-management/signal-logic-runbook.png)

Geben Sie unter **Warnungslogik** für **Schwellenwert** den Wert **1** ein. Klicken Sie auf **Fertig**, wenn Sie fertig sind.

### <a name="alert-details"></a>Warnungsdetails

Wählen Sie unter **2. Warnungsdetails definieren** einen Namen und eine Beschreibung für die Warnung ein. Legen Sie **Schweregrad** für eine erfolgreiche Ausführung auf **Information (Schweregrad 2)** bzw. für eine Ausführung mit Fehler auf **Information (Schweregrad 1)** fest.

![Konfigurieren der Signallogik](./media/automation-tutorial-update-management/define-alert-details.png)

Klicken Sie unter **3. Aktionsgruppe definieren** auf **Neue Aktionsgruppe**. Eine Aktionsgruppe ist eine Gruppe mit Aktionen, die Sie übergreifend für mehrere Warnungen verwenden können. Dies können beispielsweise E-Mail-Benachrichtigungen, Runbooks, Webhooks und vieles mehr sein. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../monitoring-and-diagnostics/monitoring-action-groups.md).

Geben Sie im Feld **Name der Aktionsgruppe** einen Namen für die Warnung und darunter einen Kurznamen ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

Geben Sie unter **Aktionen** einen Namen für die Aktion ein, beispielsweise **E-Mail-Benachrichtigungen**. Wählen Sie unter **AKTIONSTYP** die Option **E-Mail/SMS/Push/Sprachanruf** aus. Wählen Sie unter **DETAILS** die Option **Details bearbeiten**.

Geben Sie auf der Seite **E-Mail/SMS/Push/Sprachanruf** einen Namen ein. Aktivieren Sie das Kontrollkästchen **E-Mail**, und geben Sie eine gültige E-Mail-Adresse ein.

![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-tutorial-update-management/configure-email-action-group.png)

Klicken Sie im Bereich **E-Mail/SMS/Push/Sprachanruf** auf **OK**. Klicken Sie im Bereich **Aktionsgruppe hinzufügen** auf **OK**.

Wenn Sie den Betreff der Warnungs-E-Mail anpassen möchten, klicken Sie unter **Regel erstellen** und **Aktionen anpassen** auf **E-Mail-Betreff**. Klicken Sie abschließend auf **Warnungsregel erstellen**. Die Warnung informiert Sie, wenn die Bereitstellung eines Updates erfolgreich war. Außerdem wird angegeben, welche Computer Teil der durchgeführten Updatebereitstellung waren.

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Planen Sie als Nächstes eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht, um Updates zu installieren. Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

Um eine neue Updatebereitstellung für den virtuellen Computer zu planen, klicken Sie auf **Updateverwaltung** und dann auf **Updatebereitstellung planen**.

Geben Sie unter **Neue Updatebereitstellung** die folgenden Informationen ein:

* **Name:** Geben Sie einen eindeutigen Namen für die Updatebereitstellung ein.

* **Betriebssystem:** Wählen Sie das Betriebssystem aus, das als Ziel für die Updatebereitstellung dienen soll.

* **Zu aktualisierende Gruppen (Vorschau)**: Definieren Sie eine Abfrage basierend auf einer Kombination aus Abonnement, Ressourcengruppen, Standorten und Tags, um eine dynamische Gruppe von Azure-VMs zu erstellen, die in Ihre Bereitstellung eingeschlossen werden sollen. Weitere Informationen finden Sie unter [Dynamische Gruppen](automation-update-management.md#using-dynamic-groups).

* **Zu aktualisierende Computer:** Wählen Sie eine gespeicherte Suche oder eine importierte Gruppe aus, oder wählen Sie im Dropdownmenü „Computer“ und dann einzelne Computer aus. Bei Auswahl von **Computer** wird die Bereitschaft des Computers in der Spalte **BEREITSCHAFT DES UPDATE-AGENTS** angezeigt. Weitere Informationen zu den verschiedenen Methoden zum Erstellen von Computergruppen in Log Analytics finden Sie unter [Computergruppen in Log Analytics](../log-analytics/log-analytics-computer-groups.md).

* **Updateklassifizierung:** Wählen Sie die Softwareklassen aus, die in die Updatebereitstellung eingeschlossen werden sollen. Lassen Sie für dieses Tutorial alle Typen ausgewählt.

  Es gibt die folgenden Klassifizierungstypen:

   |Betriebssystem  |Typ  |
   |---------|---------|
   |Windows     | Kritische Updates</br>Sicherheitsupdates</br>Updaterollups</br>Feature Packs</br>Service Packs</br>Definitionsupdates</br>Tools</br>Aktualisierungen        |
   |Linux     | Kritische Updates und Sicherheitsupdates</br>Andere Updates       |

   Eine Beschreibung der Klassifizierungstypen finden Sie unter [Updateklassifizierungen](automation-update-management.md#update-classifications).

* **Einzuschließende/auszuschließende Updates**: Öffnet die Seite **Einschließen/ausschließen**. Updates, die eingeschlossen oder ausgeschlossen werden sollen, befinden sich auf verschiedenen Registerkarten. Weitere Informationen zur Vorgehensweise beim Einschließen finden Sie unter [Verhalten beim Einschließen](automation-update-management.md#inclusion-behavior).

* **Zeitplaneinstellungen:** Der Bereich **Zeitplaneinstellungen** wird geöffnet. Der Standard-Startzeitpunkt liegt 30 Minuten nach der aktuellen Uhrzeit. Als Startzeit können Sie einen beliebigen Wert festlegen, er muss jedoch mindestens 10 Minuten in der Zukunft liegen.

   Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten. Wählen Sie unter **Wiederholung** die Option **Einmal**. Übernehmen Sie den Standardwert „1 Tag“, und wählen Sie **OK**. Ein Zeitplan für Wiederholung wird eingerichtet.

* **Vor und nach dem Vorgang auszuführende Skripts**: Wählen Sie die Skripts aus, die vor und nach Ihrer Bereitstellung ausgeführt werden sollen. Weitere Informationen finden Sie unter [Verwalten von Pre- und Post-Skripts](pre-post-scripts.md).
* **Wartungsfenster (Minuten):** Übernehmen Sie hierfür den Standardwert. Sie können das Zeitfenster angeben, in dem die Updatebereitstellung stattfinden soll. Mit dieser Einstellung können Sie sicherstellen, dass Änderungen in den von Ihnen festgelegten Wartungsfenstern ausgeführt werden.

* **Neustartoptionen:** Mit dieser Einstellung wird festgelegt, wie Neustarts behandelt werden sollen. Die verfügbaren Optionen lauten wie folgt:
  * Neu starten bei Bedarf (Standard)
  * Immer neu starten
  * Nie neu starten
  * Nur neu starten – Updates werden nicht installiert

Klicken Sie auf **Erstellen**, wenn die Konfiguration des Zeitplans abgeschlossen ist.

![Bereich für Updatezeitplan-Einstellungen](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Das Statusdashboard wird wieder angezeigt. Wählen Sie **Geplante Updatebereitstellungen**, um den Bereitstellungszeitplan anzuzeigen.

> [!NOTE]
> Die Updateverwaltung unterstützt die Bereitstellung von Erstanbieterupdates sowie Vorabdownloads von Patches. Hierzu sind Änderungen an den Systemen erforderlich, die gepatcht werden. Informationen zum Konfigurieren der entsprechenden Einstellungen für Ihre Systeme finden Sie im [Abschnitt zu Erstanbieterpatches und Vorabdownloads](automation-update-management.md#firstparty-predownload).

## <a name="view-results-of-an-update-deployment"></a>Anzeigen der Ergebnisse einer Updatebereitstellung

Nach dem Start der geplanten Bereitstellung sehen Sie den Status der Bereitstellung auf der Registerkarte **Updatebereitstellungen** unter **Updateverwaltung**. Der Status lautet **In Bearbeitung**, wenn die Bereitstellung derzeit ausgeführt wird. Wenn die Bereitstellung erfolgreich abgeschlossen wird, ändert sich der Status in **Erfolgreich**. Wenn bei einem oder mehreren Updates in der Bereitstellung Fehler auftreten, wird der Status **Der Vorgang ist teilweise fehlgeschlagen** angezeigt.

Klicken Sie auf die abgeschlossene Updatebereitstellung, um das Dashboard für diese Updatebereitstellung anzuzeigen.

![Updatebereitstellungs-Statusdashboard für eine bestimmte Bereitstellung](./media/automation-tutorial-update-management/manageupdates-view-results.png)

Unter **Updateergebnisse** werden eine Zusammenfassung der Gesamtzahl von Updates und der Ergebnisse der Bereitstellung auf dem virtuellen Computer angegeben. Die Tabelle rechts enthält eine detaillierte Aufschlüsselung der einzelnen Updates und die Installationsergebnisse.

In der folgenden Liste sind die verfügbaren Werte aufgeführt:

* **Kein Versuch erfolgt**: Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
* **Erfolgreich**: Das Update wurde erfolgreich ausgeführt.
* **Fehler**: Beim Update ist ein Fehler aufgetreten.

Klicken Sie auf **Alle Protokolle**, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Klicken Sie auf **Ausgabe**, um den Auftragsdatenstrom des Runbooks anzuzeigen, das für die Verwaltung der Updatebereitstellung auf dem virtuellen Zielcomputer verantwortlich ist.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern bei der Bereitstellung anzuzeigen.

Nachdem die Updatebereitstellung erfolgreich abgeschlossen wurde, wird eine E-Mail (in etwa wie im folgenden Beispiel) gesendet, um den Erfolg der Bereitstellung anzugeben:

![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Integrieren eines virtuellen Computers für die Updateverwaltung
> * Anzeigen einer Updatebewertung
> * Konfigurieren von Warnungen
> * Planen einer Updatebereitstellung
> * Anzeigen der Ergebnisse einer Bereitstellung

Fahren Sie mit der Übersicht über die Lösung für die Updateverwaltung fort.

> [!div class="nextstepaction"]
> [Lösung für die Updateverwaltung](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)
