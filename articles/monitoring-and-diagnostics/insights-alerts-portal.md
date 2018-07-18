---
title: Verwenden des Azure-Portals zum Erstellen klassischer Warnungen für Azure-Dienste | Microsoft-Dokumentation
description: E-Mails oder Benachrichtigungen bzw. URLs (Webhooks) von Websites oder Automatisierung werden ausgelöst bzw. aufgerufen, wenn die von Ihnen angegebenen Bedingungen erfüllt sind.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/23/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: d0e5512eb3f963898ded6d7155f8b75cfb6ef911
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287428"
---
# <a name="use-the-azure-portal-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>Verwenden des Azure-Portals zum Erstellen klassischer Metrikwarnungen in Azure Monitor für Azure-Dienste 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](insights-alerts-command-line-interface.md)
>

> [!NOTE]
> In diesem Artikel erfahren Sie, wie Sie ältere klassische Metrikwarnungen erstellen. Azure Monitor unterstützt jetzt [neuere Metrikwarnungen](monitoring-near-real-time-metric-alerts.md). 


In diesem Artikel erfahren Sie, wie Sie klassische Azure-Metrikwarnungen über das Azure-Portal einrichten können. 

Möglicherweise erhalten Sie auf Metriken basierende Warnungen für Ihre Azure-Dienste, oder Sie können Warnungen für Ereignisse empfangen, die in Azure auftreten können.

* **Metrikwerte**: Die Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert passiert. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch dann, wenn diese Bedingung nicht mehr erfüllt wird.    

* **Aktivitätsprotokollereignisse**: Eine Warnung kann für *jedes* Ereignis ausgelöst werden, oder dann, wenn bestimmte Ereignisse auftreten. Erfahren Sie mehr über [Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts.md).

Sie können konfigurieren, dass bei einer klassischen Metrikwarnung Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren
* Senden von E-Mails an weitere von Ihnen angegebene Adressen.
* Aufrufen eines Webhooks
* Starten der Ausführung eines Azure-Runbooks (nur über das Azure-Portal).

Mit folgenden Mitteln können Sie klassische Metrikwarnregeln konfigurieren und zugehörige Informationen abrufen: 

* [Azure-Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure-CLI](insights-alerts-command-line-interface.md)
* [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Erstellen einer Warnungsregel anhand einer Metrik mit dem Azure-Portal
1. Suchen Sie im [Portal](https://portal.azure.com/) die Ressource, die Sie überwachen möchten, und wählen Sie sie aus.

2. Wählen Sie im Abschnitt **ÜBERWACHUNG** die Option **Warnungen (klassisch)** aus. Text und Symbol können je nach Ressource geringfügig variieren. Wenn Sie **Warnungen (klassisch)** hier nicht finden, befindet sich die Option möglicherweise unter **Warnungen** oder **Warnungsregeln**.

    ![Überwachung](./media/insights-alerts-portal/AlertRulesButton.png)

3. Wählen Sie den Befehl **Metrikwarnung hinzufügen (klassisch)** aus, und füllen Sie die Felder aus.

    ![Warnung hinzufügen](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. Geben Sie Ihrer Warnungsregel einen **Namen**. Wählen Sie dann eine **Beschreibung** aus, die auch in Benachrichtigungs-E-Mails angezeigt wird.

5. Wählen Sie die **Metrik** aus, die Sie überwachen möchten. Wählen Sie dann eine **Bedingung** und einen **Schwellenwert** für die Metrik aus. Wählen Sie auch den **Zeitraum** der Metrikregel aus, der erfüllt sein muss, ehe die Warnung ausgelöst wird. Wenn Sie z.B. den Zeitraum „In den letzten 5 Minuten“ auswählen und die Warnung nach einer CPU-Auslastung von über 80% sucht, wird die Warnung ausgelöst, wenn die CPU-Auslastung 5 Minuten durchgängig über 80% lag. Nach Auslösen des ersten Triggers erfolgt ein erneutes Auslösen, wenn die CPU-Auslastung 5 Minuten unter 80% bleibt. Die Messung der CPU-Metrik erfolgt minütlich.

6. Wählen Sie **E-Mail-Besitzer...** aus, wenn Sie möchten, dass Administratoren und Co-Administratoren E-Mail-Benachrichtigungen empfangen, wenn die Warnung ausgelöst wird.

7. Wenn Sie möchten, dass bei Auslösen der Warnung Benachrichtigungen an weitere E-Mail-Adressen gesendet werden, fügen Sie diese dem Feld **Weitere Administrator-E-Mail(s)** hinzu. Trennen Sie mehrere E-Mail-Nachrichten in folgendem Format durch Semikolons: *email@contoso.com; email2@contoso.com*

8. Fügen Sie einen gültigen URI in das Feld **Webhook** ein, wenn dieser bei Auslösen der Warnung aufgerufen werden soll.

9. Wenn Sie Azure Automation verwenden, können Sie ein Runbook auswählen, das ausgeführt werden soll, sobald die Warnung ausgelöst wird.

10. Wählen Sie **OK** , um die Warnung zu erstellen.   

Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="manage-your-alerts"></a>Verwalten Ihrer Warnungen
Nachdem Sie eine Warnung erstellt haben, können Sie sie auswählen und eine der folgenden Aufgaben ausführen:

* Anzeigen eines Diagramms, das den Schwellenwert der Metrik und die tatsächlichen Werte vom Vortag zeigt.
* bearbeiten oder löschen.
* sie **deaktivieren** oder **aktivieren**, wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über die Azure-Überwachung](monitoring-overview.md), einschließlich der Typen von Informationen, die Sie sammeln und überwachen können.
* Informieren Sie sich ausführlicher über die [neueren Metrikwarnungen](monitoring-near-real-time-metric-alerts.md).
* Erfahren Sie mehr über das [Konfigurieren von Webhooks in Warnungen](insights-webhooks-alerts.md).
* Erfahren Sie mehr über das [Konfigurieren von Warnungen zu Aktivitätsprotokollereignissen](monitoring-activity-log-alerts.md).
* Erfahren Sie mehr zu [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Verschaffen Sie sich einen [Überblick über Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md), um detaillierte Hochfrequenzmetriken für Ihren Dienst zu erfassen.
* Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](insights-how-to-customize-monitoring.md), um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
