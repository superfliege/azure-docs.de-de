---
title: 'Erstellen von Abfragen für B2B-Nachrichten in Log Analytics: Azure Logic Apps | Microsoft-Dokumentation'
description: Erstellen von Abfragen zum Nachverfolgen von AS2-, X12- und EDIFACT-Nachrichten mit Log Analytics für Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: baccd255fc2812eae0de3a98dfcef3dcbc7e1b46
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124269"
---
# <a name="create-queries-for-tracking-as2-x12-and-edifact-messages-in-log-analytics-for-azure-logic-apps"></a>Erstellen von Abfragen zum Nachverfolgen von AS2-, X12- und EDIFACT-Nachrichten mit Log Analytics für Azure Logic Apps

Sie können Abfragen erstellen, die Aktionen auf der Grundlage bestimmter Kriterien filtern, um die AS2-, X12- oder EDIFACT-Nachrichten zu finden, die Sie mithilfe von [Azure Log Analytics](../log-analytics/log-analytics-overview.md) nachverfolgen. So können Sie beispielsweise anhand einer bestimmten Austauschkontrollnummer nach Nachrichten suchen.

## <a name="requirements"></a>Anforderungen

* Eine Logik-App, für die die Diagnoseprotokollierung eingerichtet ist. Informieren Sie sich über das [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) und das [Einrichten der Protokollierung für diese Logik-App](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Ein Integrationskonto, für das die Überwachung und Protokollierung eingerichtet ist. Informieren Sie sich über das [Erstellen eines Integrationskontos](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) und das [Einrichten der Überwachung und Protokollierung für dieses Konto](../logic-apps/logic-apps-monitor-b2b-message.md).

* [Veröffentlichen Sie Diagnosedaten in Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md), und [richten Sie die Nachrichtennachverfolgung in Log Analytics ein](../logic-apps/logic-apps-track-b2b-messages-omsportal.md), sofern noch nicht geschehen.

> [!NOTE]
> Wenn auch die anderen Anforderungen erfüllt sind, verfügen Sie über einen Arbeitsbereich in Log Analytics. Es empfiehlt sich, für die Nachverfolgung der B2B-Kommunikation in Log Analytics den gleichen Arbeitsbereich zu verwenden. 
>  
> Falls Sie keinen Log Analytics-Arbeitsbereich besitzen, lesen Sie die Informationen zum [Erstellen eines Log Analytics-Arbeitsbereichs](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="create-message-queries-with-filters-in-log-analytics"></a>Erstellen von Nachrichtenabfragen mit Filtern in Log Analytics

In diesem Beispiel erfahren Sie, wie Sie anhand der Austauschkontrollnummer nach Nachrichten suchen.

> [!TIP] 
> Wenn Ihnen der Name Ihres Log Analytics-Arbeitsbereichs bekannt ist, navigieren Sie zur Startseite Ihres Arbeitsbereichs (`https://{your-workspace-name}.portal.mms.microsoft.com`), und fahren Sie mit Schritt 4 fort. Beginnen Sie andernfalls mit Schritt 1.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Alle Dienste** aus. Suchen Sie nach „Log Analytics“, und wählen Sie die Option **Log Analytics** aus, wie hier zu sehen:

   ![Suchen nach Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Wählen Sie unter **Log Analytics** Ihren Log Analytics-Arbeitsbereich aus.

   ![Auswählen Ihres Log Analytics-Arbeitsbereichs](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Wählen Sie unter **Verwaltung** die Option **Protokollsuche** aus.

   ![Auswählen von „Protokollsuche“](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/azure-portal-page.png)

4. Geben Sie im Suchfeld ein Feld an, nach dem Sie suchen möchten, und drücken Sie die **EINGABETASTE**. Wenn Sie mit der Eingabe beginnen, werden in Log Analytics mögliche Übereinstimmungen und verfügbare Vorgänge angezeigt. Informieren Sie sich über das [Suchen nach Daten in Log Analytics](../log-analytics/log-analytics-log-searches.md).

   In diesem Beispiel wird nach Ereignissen mit **Type=AzureDiagnostics** gesucht.

   ![Beginnen mit der Eingabe einer Abfragezeichenfolge](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

5. Wählen Sie auf der linken Leiste den gewünschten Zeitrahmen aus. Wenn Sie der Abfrage einen Filter hinzufügen möchten, wählen Sie **+Hinzufügen** aus.

   ![Hinzufügen eines Filters zur Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

6. Geben Sie unter **Filter hinzufügen** den Namen des gewünschten Filters ein. Wählen Sie den Filter aus, und wählen Sie **+Hinzufügen** aus.

   Zur Ermittlung der Austauschkontrollnummer wird in diesem Beispiel nach dem Wort „interchange“ (Austausch) gesucht und **event_record_messageProperties_interchangeControlNumber_s** als Filter ausgewählt.

   ![Auswählen des Filters](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

7. Wählen Sie auf der linken Leiste den gewünschten Filterwert und anschließend **Übernehmen** aus.

   In diesem Beispiel wird die Austauschkontrollnummer für die gewünschten Nachrichten ausgewählt.

   ![Auswählen des Filterwerts](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

8. Kehren Sie zur Abfrageerstellung zurück. Ihre Abfrage wurde mit dem ausgewählten Filterereignis und -wert aktualisiert. Die vorherigen Ergebnisse werden nun ebenfalls gefiltert.

    ![Rückkehr zur Abfrage mit gefilterten Ergebnissen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Speichern der Abfrage zur späteren Verwendung

1. Wählen Sie auf der Seite **Protokollsuche** für Ihre Abfrage die Option **Speichern** aus. Benennen Sie die Abfrage, wählen Sie eine Kategorie aus, und wählen Sie anschließend **Speichern** aus.

   ![Benennen der Abfrage und Auswählen einer Kategorie](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. Wählen Sie zum Anzeigen Ihrer Abfrage die Option **Favoriten** aus.

   ![Auswählen von „Favoriten“](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Wählen Sie unter **Gespeicherte Suchvorgänge** Ihre Abfrage aus, um die entsprechenden Ergebnisse anzuzeigen. Sie können die Abfrage bearbeiten, um andere Ergebnisse zu erhalten.

   ![Auswählen Ihrer Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-log-analytics"></a>Suchen und Ausführen gespeicherter Abfragen in Log Analytics

1. Öffnen Sie die Startseite Ihres Log Analytics-Arbeitsbereichs (`https://{your-workspace-name}.portal.mms.microsoft.com`), und klicken Sie auf **Protokollsuche**.

   ![Klicken auf „Protokollsuche“ auf der Log Analytics-Startseite](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   Oder

   ![Auswählen von „Protokollsuche“ im Menü](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Wählen Sie auf der Startseite der **Protokollsuche** die Option **Favoriten** aus.

   ![Auswählen von „Favoriten“](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Wählen Sie unter **Gespeicherte Suchvorgänge** Ihre Abfrage aus, um die entsprechenden Ergebnisse anzuzeigen. Sie können die Abfrage bearbeiten, um andere Ergebnisse zu erhalten.

   ![Auswählen Ihrer Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Nächste Schritte

* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Benutzerdefinierte Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)