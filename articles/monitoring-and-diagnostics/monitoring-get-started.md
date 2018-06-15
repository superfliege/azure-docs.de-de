---
title: Erste Schritte mit Azure Monitor
description: Starten Sie mit Azure Monitor, um Einblicke in die Vorgänge Ihrer Ressourcen zu erhalten, und ergreifen Sie basierend auf diesen Daten ggf. entsprechende Maßnahmen.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/25/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: 70807db256f72b77bb29db3f6f59474a892f2939
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263360"
---
# <a name="get-started-with-azure-monitor"></a>Erste Schritte mit Azure Monitor
Azure Monitor ist der Plattformdienst, mit dem Sie Ihre Azure-Ressourcen an einem zentralen Ort verwalten können. Mit Azure Monitor können Sie Metriken und Protokolle aus den Ressourcen in Azure visualisieren, abfragen, weiterleiten und archivieren und andere Maßnahmen ergreifen. Sie können im Azure-Portal, mit [Monitor PowerShell-Cmdlets](insights-powershell-samples.md), in einer [plattformübergreifenden Befehlszeilenschnittstelle](insights-cli-samples.md) oder in [Azure Monitor-REST-APIs](https://msdn.microsoft.com/library/dn931943.aspx) mit diesen Daten arbeiten. In diesem Artikel werden einige wichtige Komponenten von Azure Monitor beschrieben, indem zu Demonstrationszwecken das Portal verwendet wird.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
1. Navigieren Sie im Portal zu **Alle Dienste**, und suchen Sie die Option **Monitor**. Klicken Sie auf das Sternsymbol, um diese Option zu Ihrer Favoritenliste hinzuzufügen. So ist sie über die linke Navigationsleiste immer schnell zu erreichen.

    ![Azure Monitor in der Dienstliste](./media/monitoring-get-started/monitor-more-services.png)
2. Klicken Sie auf die Option **Monitor**, um die Seite **Monitor** zu öffnen. Auf dieser Seite sind alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammengefasst. Zunächst wird der Abschnitt **Übersicht** geöffnet. Die Übersicht zeigt ein Rollup sämtlicher Überwachungswarnungen, Fehler und Empfehlungen im Zusammenhang mit der Dienstintegrität, die sich auf die Ressourcen in Ihrem Abonnement beziehen.  

    ![Monitor-Navigation](./media/monitoring-get-started/monitor-blade-nav.png)

    Azure Monitor bietet drei grundlegende Kategorien von Überwachungsdaten: das **Aktivitätsprotokoll**, **Metriken** und **Diagnoseprotokolle**.
3. Klicken Sie auf **Aktivitätsprotokoll** , um sicherzustellen, dass der Abschnitt „Aktivitätsprotokoll“ angezeigt wird.

    Im [**Aktivitätsprotokoll**](monitoring-overview-activity-logs.md) werden alle Vorgänge beschrieben, die in den Ressourcen in Ihrem Abonnement durchgeführt wurden. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Erstell-, Aktualisierungs- und Löschvorgänge ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Das Aktivitätsprotokoll enthält beispielsweise Informationen darüber, wann und von wem eine Web-App beendet wurde. Aktivitätsprotokollereignisse werden auf der Plattform gespeichert und sind 90 Tage verfügbar.

    ![Aktivitätsprotokoll](./media/monitoring-get-started/monitor-act-log-blade.png)

    Sie können Abfragen für allgemeine Filter erstellen und speichern und dann die wichtigsten Abfragen an ein Portaldashboard anheften. So können Sie jederzeit sehen, ob Ereignisse aufgetreten sind, die Ihre Kriterien erfüllen.
4. Filtern Sie die Ansicht für eine bestimmte Ressourcengruppe nach der letzten Woche, und klicken Sie dann auf die Schaltfläche **Speichern** . Geben Sie Ihrer Abfrage einen Namen.

    ![Aktivitätsprotokollabfrage speichern](./media/monitoring-get-started/monitor-act-log-save.png)
5. Klicken Sie jetzt auf die Schaltfläche **Anheften** .

    ![Aktivitätsprotokoll anheften](./media/monitoring-get-started/monitor-act-log-pin.png)

    Die meisten Ansichten, die in dieser exemplarischen Vorgehensweise erläutert werden, können an ein Dashboard angeheftet werden. So können Sie eine einzige Informationsquelle für operative Daten von Ihren Diensten erstellen.
6. Wechseln Sie zurück zum Dashboard. Sie sehen nun, dass die Abfrage (und die Anzahl von Ergebnissen) auf Ihrem Dashboard angezeigt wird. Dies ist hilfreich, wenn Sie schnell wichtige Aktionen anzeigen möchten, die für Ihr Abonnement kürzlich aufgetreten sind, z.B., wenn eine neue Rolle zugewiesen oder eine VM gelöscht wurde.

    ![An das Dashboard angeheftete Aktivitätsprotokolle](./media/monitoring-get-started/monitor-act-log-db.png)
7. Kehren Sie zurück zur Kachel **Überwachen**, und klicken Sie auf den Abschnitt **Metriken**. Zunächst müssen Sie durch Filtern und Auswählen der Optionen mit der Dropdownliste oben auf der Seite eine Ressource auswählen.

    ![Ressourcen für Metriken filtern](./media/monitoring-get-started/monitor-met-filter.png)

    [**Metriken**](monitoring-overview-metrics.md) werden von allen Azure-Ressourcen ausgegeben. In dieser Ansicht sind alle Metriken in einer zentralen Konsole zusammengefasst, damit Sie sich leicht einen Überblick über die Leistung Ihrer Ressourcen verschaffen können. Sehen Sie sich darüber hinaus auch unsere [brandneue Oberfläche zur Erstellung von Diagrammen mit Metriken](https://aka.ms/azuremonitor/new-metrics-charts) an, indem Sie auf die Registerkarte **Metriken (Vorschau)** klicken.
8. Sobald Sie eine Ressource ausgewählt haben, werden alle verfügbaren Metriken links auf der Seite angezeigt. Sie können mehrere Metriken auf einmal in einem Diagramm darstellen, indem Sie die gewünschten Metriken auswählen und den Diagrammtyp und Zeitraum ändern. Sie können auch alle Metrikwarnungen anzeigen, die für diese Ressource eingerichtet wurden.

    ![Blatt "Metrik"](./media/monitoring-get-started/monitor-metric-blade.png)

   > [!NOTE]
   > Einige Metriken sind nur verfügbar, wenn Sie [Application Insights](../application-insights/app-insights-overview.md) bzw. die Windows- oder Linux-Azure-Diagnose-Erweiterung für Ihre Ressource aktivieren.
   >
   >

9. Wenn Sie mit dem Diagramm zufrieden sind, können Sie es über die Schaltfläche **Anheften** an Ihr Dashboard anheften.
10. Kehren Sie zu **Monitor** zurück, und klicken Sie auf **Diagnoseprotokolle**.

    ![Blatt „Diagnoseprotokolle“](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Diagnoseprotokolle**](monitoring-overview-of-diagnostic-logs.md) sind *von* einer Ressource ausgegebene Protokolle, in denen Daten zum Betrieb dieser bestimmten Ressource erfasst werden. Regelzähler für Netzwerksicherheitsgruppen und Workflowprotokolle für Logik-Apps sind Beispiele für solche Diagnoseprotokolle. Diese Protokolle können in einem Speicherkonto gespeichert, an einen Event Hub gestreamt oder an [Log Analytics](../log-analytics/log-analytics-overview.md) gesendet werden. Log Analytics ist das Operational Intelligence-Produkt von Microsoft für erweiterte Suchen und Warnungen.

    Um zu überprüfen, ob die entsprechenden Diagnoseprotokolle aktiviert sind, können Sie im Portal eine Liste aller Ressourcen in Ihrem Abonnement anzeigen und filtern.
    > [!NOTE]
    > Das Senden mehrdimensionaler Metriken über die Diagnoseeinstellungen wird derzeit nicht unterstützt. Metriken mit Dimensionen werden als vereinfachte eindimensionale Metriken exportiert und dimensionswertübergreifend aggregiert.
    >
    > *Beispiel:* Die Metrik „Eingehende Nachrichten“ eines Event Hubs kann auf einer warteschlangenspezifischen Ebene untersucht und in einem Diagramm dargestellt werden. Wenn Sie die Metrik allerdings über die Diagnoseeinstellungen exportieren, umfasst die Darstellung alle eingehenden Nachrichten für alle Warteschlangen im Event Hub.
    >
    >

11. Klicken Sie auf der Seite „Diagnoseprotokolle“ auf eine Ressource. Wenn Diagnoseprotokolle in einem Speicherkonto gespeichert sind, sehen Sie nun eine Liste mit stündlich erstellten Protokollen, die Sie direkt herunterladen können.

    ![Diagnoseprotokolle für eine Ressource](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Sie können auch auf **Diagnoseeinstellungen** klicken, wodurch Sie die Archivierung in einem Speicherkonto, das Streamen an Event Hubs oder das Senden an einen Log Analytics-Arbeitsbereich einrichten oder die Einstellungen dafür ändern können.

    ![Aktivieren von Diagnoseprotokollen](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Wenn Sie Diagnoseprotokolle in Log Analytics eingerichtet haben, können Sie diese dann im Portal im Abschnitt **Protokollsuche** suchen.
12. Navigieren Sie auf der Seite „Monitor“ zum Abschnitt **Warnungen (klassisch)**.

    ![Blatt „Warnungen“ für öffentliche Vorschau](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Hier können Sie alle [**klassischen Warnungen**](monitoring-overview-alerts.md) für Ihre Azure-Ressourcen verwalten. Dies beinhaltet auch Warnungen für Metriken, Aktivitätsprotokollereignisse sowie Webtests (Speicherorte) und proaktive Diagnosen in Application Insights. Warnungen werden mit Aktionsgruppen verknüpft. Mit [Aktionsgruppen](monitoring-action-groups.md) können Benutzer benachrichtigt oder bestimmte Aktionen ausgeführt werden, wenn eine Warnung ausgelöst wird.

13. Klicken Sie auf **Metrikwarnungen hinzufügen** , um eine Warnung zu erstellen.

    ![Metrikwarnungen hinzufügen](./media/monitoring-get-started/monitor-alerts-add.png)

    Sie können dann eine Warnung an Ihr Dashboard anheften, um den Status jederzeit einfach einsehen zu können.

    Azure Monitor bietet nun auch [**neuere Warnungen**](https://aka.ms/azuremonitor/near-real-time-alerts), die im Minutentakt ausgewertet werden können.

14. Auch der Abschnitt „Überwachen“ enthält Links zu [Application Insights](../application-insights/app-insights-overview.md)-Anwendungen und [Log Analytics](../log-analytics/log-analytics-overview.md)-Verwaltungslösungen. Diese Microsoft-Produkte sind eng in Azure Monitor integriert.
15. Falls Sie Application Insights oder Log Analytics nicht verwenden, ist es gut möglich, dass eine Partnerschaft zwischen Azure Monitor und Ihrem aktuellen Produktanbieter für Überwachung, Protokollierung und Warnungen besteht. Eine vollständige Liste unserer Partner sowie Anleitungen zur Integration finden Sie auf unserer [Partner-Website](monitoring-partners.md) .

Wenn Sie diese Schritte befolgen und alle wichtigen Kacheln an ein Dashboard anheften, können Sie umfassende Ansichten Ihrer Anwendung und Infrastruktur erstellen. Hier ein Beispiel:

![Azure Monitor-Dashboard](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Nächste Schritte
* In der [Übersicht über alle Azure-Überwachungstools](monitoring-overview.md) erfahren Sie, wie Azure Monitor mit ihnen arbeitet.
