---
title: Überwachen von Logik-App-Ausführungen mit Log Analytics – Azure Logic Apps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Log Analytics Erkenntnisse und Debuggingdaten zu Logik-App-Ausführungen erhalten, die bei der Problembehandlung und Diagnose hilfreich sind.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/11/2018
ms.openlocfilehash: 177c361734a88acab5fc10d6b460645be82bf437
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457137"
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-log-analytics"></a>Überwachen und Auswerten von Logik-App-Ausführungen mit Log Analytics

Sie können beim Erstellen einer Logik-App gleichzeitig Log Analytics aktivieren, um die Überwachung durchzuführen und umfassendere Debuginformationen zu erhalten. Log Analytics ermöglicht die Diagnoseprotokollierung und Überwachung für Ihre Logik-App-Ausführungen über das Azure-Portal. Wenn Sie die Lösung für die Logik-App-Verwaltung hinzufügen, erhalten Sie den aggregierten Status für Ihre Logik-App-Ausführungen und spezifische Details wie Status, Ausführungszeit, Status der erneuten Übermittlung und Korrelations-IDs.

In diesem Artikel wird veranschaulicht, wie Sie Log Analytics aktivieren, um Laufzeitereignisse und Daten für Ihre Logik-App-Ausführung anzuzeigen.

 > [!TIP]
 > Führen Sie diese Schritte zum [Aktivieren der Diagnoseprotokollierung und Senden von Logik-App-Laufzeitdaten an Log Analytics](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics) aus, um Ihre vorhandenen Logik-Apps zu überwachen.

## <a name="requirements"></a>Requirements (Anforderungen)

Sie müssen über einen Log Analytics-Arbeitsbereich verfügen, bevor Sie beginnen. Erfahren Sie mehr über das [Erstellen eines Log Analytics-Arbeitsbereichs](../log-analytics/log-analytics-quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Aktivieren der Diagnoseprotokollierung beim Erstellen von Logik-Apps

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) eine Logik-App. Wählen Sie **Ressource erstellen** > **Integration** > **Logik-App** aus.

   ![Erstellen einer Logik-App](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

1. Führen Sie unter **Logik-App erstellen** wie gezeigt die folgenden Schritte aus:

   1. Geben Sie einen Namen für Ihre Logik-App an, und wählen Sie Ihr Azure-Abonnement aus. 

   1. Erstellen Sie eine Azure-Ressourcengruppe, oder wählen Sie sie aus.

   1. Legen Sie **Log Analytics** auf **Ein** fest. 

   1. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den Arbeitsbereich aus, an den Sie Daten für Ihre Logik-App-Ausführungen senden möchten. 

      ![Erstellen einer Logik-App](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Nach Abschluss dieses Schritts erstellt Azure Ihre Logik-App, die jetzt Ihrem Log Analytics-Arbeitsbereich zugeordnet ist. 
      Mit diesem Schritt wird auch die Lösung für die Logik-App-Verwaltung automatisch in Ihrem Arbeitsbereich installiert.

   1. Wenn Sie fertig sind, wählen Sie **Erstellen** aus.

1. Fahren Sie mit [diesen Schritten](#view-logic-app-runs-oms) fort, um Ihre Logik-App-Ausführungen anzuzeigen.

## <a name="install-the-logic-apps-management-solution"></a>Installieren der Lösung für die Logik-App-Verwaltung

Überspringen Sie diesen Schritt, falls Sie Log Analytics bereits beim Erstellen Ihrer Logik-App aktiviert haben. Sie haben die Lösung für die Logik-App-Verwaltung bereits installiert.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Alle Dienste**. Geben Sie im Suchfeld „log analytics“ als Filter ein, und wählen Sie **Log Analytics** aus.

   ![Auswählen von „Log Analytics“](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. Wählen Sie unter **Log Analytics** Ihren Log Analytics-Arbeitsbereich aus. 

   ![Auswählen Ihres Log Analytics-Arbeitsbereichs](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. Wählen Sie unter **Überwachungslösungen konfigurieren** die Option **Lösungen anzeigen** aus.

   ![Auswählen von „Lösungen anzeigen“](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. Wählen Sie auf der Seite „Übersicht“ die Option **Hinzufügen** aus, um die Liste **Verwaltungslösungen** zu öffnen. Wählen Sie in dieser Liste die Option **Logic Apps-Verwaltung** aus. 

   ![Auswählen von „Logic Apps-Verwaltung“](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

   Wenn Sie die Lösung nicht finden können, wählen Sie am unteren Rand der Liste **Weitere laden** aus, bis die Lösung angezeigt wird.

1. Wählen Sie **Erstellen** aus, um die Lösung zu installieren.

   ![Auswählen von „Hinzufügen“ für „Logic Apps Management“ (Logik-App-Verwaltung)](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-runs-in-log-analytics-workspace"></a>Anzeigen von Logik-App-Ausführungen im Log Analytics-Arbeitsbereich

1. Navigieren Sie zu Ihrem Log Analytics-Arbeitsbereich, und öffnen Sie die Seite „Übersicht“, um Anzahl und Status für Ihre Logik-App-Ausführungen anzuzeigen. 

   Details zu Ihren Logik-App-Ausführungen finden Sie auf der Kachel **Logic Apps-Verwaltung**. Wählen Sie die Kachel **Logic Apps Management** (Logik-App-Verwaltung), um eine Zusammenfassung mit weiteren Details zu Ihren Logik-App-Ausführungen anzuzeigen. 

   ![Übersichtskachel mit Anzahl und Status der Logik-App-Ausführungen](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   Hier sind Ihre Logik-App-Ausführungen nach Name oder Ausführungsstatus gruppiert. 
   Auf dieser Seite finden Sie auch Details zu Fehlern in Aktionen oder Triggern für die Logik-App-Ausführungen.

   ![Statuszusammenfassung für Ihre Logik-App-Ausführungen](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
1. Wählen Sie die Zeile für eine Logik-App oder einen Status aus, um alle Ausführungen für eine bestimmte Logik-App bzw. einen Status anzuzeigen.

   Hier ist ein Beispiel angegeben, bei dem alle Ausführungen für eine bestimmte Logik-App aufgeführt werden:

   ![Anzeigen von Ausführungen für eine Logik-App oder einen Status](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Diese Seite enthält diese erweiterten Optionen:

   * **Nachverfolgte Eigenschaften:**

     In dieser Spalte werden nachverfolgte Eigenschaften für die Logik-App nach Aktionen gruppiert angezeigt. Wählen Sie zum Anzeigen der überwachten Eigenschaften **Ansicht** aus. 
     Sie können die nachverfolgten Eigenschaften mit dem Spaltenfilter durchsuchen.
   
     ![Anzeigen der überwachten Eigenschaften für eine Logik-App](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Alle neu hinzugefügten überwachten Eigenschaften werden möglicherweise erst nach 10 bis 15 Minuten erstmalig angezeigt. Erfahren Sie mehr über das [Hinzufügen überwachter Eigenschaften zu Ihrer Logik-App](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Erneut übermitteln:** Sie können eine oder mehrere Logik-App-Ausführungen erneut übermitteln, die fehlgeschlagen sind, erfolgreich waren oder noch ausgeführt werden. Aktivieren Sie die Kontrollkästchen für die Ausführungen, die Sie erneut übermitteln möchten, und wählen Sie **Erneut übermitteln** aus. 

     ![Erneutes Übermitteln von Logik-App-Ausführungen](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Zum Filtern dieser Ergebnisse können Sie sowohl die clientseitige als auch die serverseitige Filterung durchführen.

   * **Clientseitiger Filter:** Wählen Sie für jede Spalte die gewünschten Filter aus. Beispiel:

     ![Beispiel für Spaltenfilter](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Serverseitiger Filter:** Verwenden Sie das Steuerelement für die Bereichsauswahl oben auf der Seite, um ein bestimmtes Zeitfenster auszuwählen oder die Anzahl von angezeigten Ausführungen zu begrenzen. Standardmäßig werden nur jeweils 1.000 Datensätze angezeigt.
   
     ![Ändern des Zeitfensters](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
1. Um alle Aktionen und deren Details für eine bestimmte Ausführung anzuzeigen, wählen Sie eine Zeile für eine Logik-App-Ausführung aus.

   Hier ist ein Beispiel mit allen Aktionen für eine bestimmte Logik-App-Ausführung dargestellt:

   ![Anzeigen von Aktionen für eine Logik-App-Ausführung](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
1. Um auf einer Ergebnisseite die Abfrage hinter den Ergebnissen anzuzeigen oder um alle Ergebnisse anzuzeigen, wählen Sie **Alle anzeigen** aus. Damit wird die Seite der Protokollsuche geöffnet.
   
   ![„Alle anzeigen“ auf Ergebnisseiten](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   Auf der Seite „Protokollsuche“:

   * Um die Abfrageergebnisse in einer Tabelle anzuzeigen, wählen Sie **Tabelle** aus.

   * Sie können die Abfragezeichenfolge in der Suchleiste bearbeiten, um die Abfrage zu ändern. 
   Wählen Sie **Erweiterte Analyse**, falls Sie mehr Optionen benötigen.

     ![Anzeigen von Aktionen und Details für eine Logik-App-Ausführung](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     Auf der Azure Log Analytics-Seite können Sie Abfragen aktualisieren und die Ergebnisse aus der Tabelle anzeigen. Für diese Abfrage wird die [Kusto-Abfragesprache](https://aka.ms/LogAnalyticsLanguageReference) verwendet. Sie können diese Sprache bearbeiten, wenn Sie unterschiedliche Ergebnisse anzeigen möchten. 

     ![Azure Log Analytics – Abfrageansicht](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen von B2B-Nachrichten](../logic-apps/logic-apps-monitor-b2b-message.md)