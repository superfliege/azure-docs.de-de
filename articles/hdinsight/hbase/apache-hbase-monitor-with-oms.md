---
title: "Überwachen von HBase mit der Operations Management Suite (OMS) – Azure HDInsight HBase | Microsoft-Dokumentation"
description: "Verwenden Sie die OMS und Azure Log Analytics zur Überwachung von HDInsight HBase-Clustern."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: f78d570cfa8b040cd7673a5e14e6a992511f60bb
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="monitor-hbase-with-operations-management-suite-oms"></a>Überwachen von HBase mit der Operations Management Suite (OMS)

Bei der HDInsight HBase-Überwachung wird Azure Log Analytics verwendet, um HDInsight HBase-Leistungsmetriken Ihrer HDInsight-Clusterknoten zu erfassen. Die Überwachung bietet HBase-spezifische Visualisierungen und Dashboards sowie Tools zum Durchsuchen der Metriken und ermöglicht das Erstellen benutzerdefinierter Überwachungsregeln und -benachrichtigungen. Sie können die Metriken für mehrere HDInsight HBase-Cluster und über mehrere Azure-Abonnements hinweg überwachen.

Log Analytics ist ein Dienst in [Operations Management Suite (OMS)](../../operations-management-suite/operations-management-suite-overview.md), der Ihre cloudbasierten und lokalen Umgebungen überwacht, um die Verfügbarkeit und Leistung sicherzustellen. Log Analytics sammelt Daten, die von Ressourcen in Ihren cloudbasierten und lokalen Umgebungen sowie von anderen Überwachungstools generiert werden, um Analysen für mehrere Quellen zu ermöglichen.

[Log Analytics-Verwaltungslösungen](../../log-analytics/log-analytics-add-solutions.md) erweitern den Funktionsumfang der OMS und stellen zusätzliche Daten und Analysetools bereit. Bei den Log Analytics-Verwaltungslösungen handelt es sich um eine Sammlung von Logik-, Visualisierungs- und Datenerfassungsregeln für einen bestimmten Bereich. Eine Lösung kann auch neue Datensatztypen definieren, die erfasst und mit der Protokollsuche oder mithilfe neuer Features der Benutzeroberfläche analysiert werden können.

[Insight & Analytics](https://azure.microsoft.com/pricing/details/insight-analytics/) basiert auf der Log Analytics-Plattform. Sie können entweder die Log Analytics-Funktionen nutzen (in diesem Fall basiert die Abrechnung auf den vom Dienst erfassten GB) oder Ihren Arbeitsbereich auf den Tarif „Insight & Analytics“ umstellen (in diesem Fall basiert die Abrechnung auf den vom Dienst verwalteten Knoten). Insight & Analytics bietet im Vergleich zu Log Analytics mehr Funktionen. Die HBase-Überwachungslösung ist mit Log Analytics oder mit Insight & Analytics erhältlich.

Wenn Sie eine HDInsight HBase-Überwachungslösung bereitstellen, erstellen Sie einen OMS-Arbeitsbereich. Jeder Arbeitsbereich ist eine eigene Log Analytics-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen. Sie können in Ihrem Abonnement mehrere Arbeitsbereiche erstellen, um mehrere Umgebungen zu unterstützen (beispielsweise eine Produktions- und eine Testumgebung).

## <a name="provision-hdinsight-hbase-monitoring"></a>Bereitstellen der HDInsight HBase-Überwachung

1. Melden Sie sich unter Verwendung Ihres Azure-Abonnements beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie unter **Marketplace** im Bereich **Neu** auf **Überwachung und Verwaltung**.
3. Klicken Sie im Bereich **Überwachung und Verwaltung** auf **Alle anzeigen**.

    ![Bereich „Überwachung und Verwaltung“](./media/apache-hbase-monitor-with-oms/monitoring-management-blade.png)  

4. Suchen Sie in der Liste nach **Verwaltungslösungen**. Klicken Sie rechts neben **Verwaltungslösungen** auf **Mehr**.

    ![Bereich „Überwachung und Verwaltung“](./media/apache-hbase-monitor-with-oms/management-solutions.png) 

5. Wählen Sie im Bereich **Verwaltungslösungen** die Verwaltungslösung „HDInsight HBase-Überwachung“ aus, um sie einem Arbeitsbereich hinzuzufügen.

    ![Bereich „Verwaltungslösungen“](./media/apache-hbase-monitor-with-oms/hbase-solution.png)  
6. Überprüfen Sie im Bereich „Verwaltungslösung“ die Informationen zur Verwaltungslösung, und klicken Sie anschließend auf **Erstellen**. 
7. Wählen Sie im Bereich *Name der Verwaltungslösung* einen vorhandenen Arbeitsbereich aus, um ihn der Verwaltungslösung zuzuordnen, oder erstellen Sie einen neuen OMS-Arbeitsbereich, und wählen Sie ihn aus.
8. Ändern Sie ggf. die Arbeitsbereichseinstellungen für das Azure-Abonnement, die Ressourcengruppe und den Standort. 
    ![Lösungsarbeitsbereich](./media/apache-hbase-monitor-with-oms/solution-workspace.png)  
9. Klicken Sie auf **Erstellen**.  
10. Um die neue Verwaltungslösung im dazugehörigen Arbeitsbereich zu verwenden, navigieren Sie zu **Log Analytics** > ***Name des Arbeitsbereichs*** > **Lösungen**. Die Liste enthält einen Eintrag für Ihre Verwaltungslösung. Wählen Sie den Eintrag aus, um zu der Lösung zu navigieren.

    ![Log Analytics-Lösungen](./media/apache-hbase-monitor-with-oms/log-analytics-solutions.png)  

11. Der Bereich für Ihre HDInsight HBase-Überwachungslösung wird angezeigt.

    ![Bereich der HDInsight HBase-Lösung](./media/apache-hbase-monitor-with-oms/hdinsight-hbase-solution.png) 

12. Auf den Zusammenfassungskacheln werden keine Daten angezeigt, da Sie Ihren HDInsight HBase-Cluster noch nicht für das Senden von Daten an Log Analytics konfiguriert haben.

## <a name="connect-hdinsight-hbase-cluster-to-log-analytics"></a>Herstellen einer Verbindung zwischen HDInsight HBase-Cluster und Log Analytics

Um die von der HDInsight HBase-Überwachung bereitgestellten Tools verwenden zu können, müssen Sie Ihren Cluster so konfigurieren, dass er die Metriken vom entsprechenden Regionsserver sowie von den Haupt- und ZooKeeper-Knoten an Log Analytics überträgt. Hierzu wird eine Skriptaktion für Ihren HDInsight HBase-Cluster ausgeführt.

### <a name="get-oms-workspace-id-and-workspace-key"></a>Abrufen von OMS-Arbeitsbereichs-ID und Arbeitsbereichsschlüssel

Sie benötigen die OMS-Arbeitsbereichs-ID und den Arbeitsbereichsschlüssel, damit sich die Knoten in Ihrem Cluster bei Log Analytics authentifizieren können. Diese Werte erhalten Sie wie folgt:

1. Klicken Sie im Azure-Portal im Bereich Ihrer HBase-Überwachung auf „Übersicht“.

    ![Bereich der HBase-Überwachungslösung](./media/apache-hbase-monitor-with-oms/hdinsight-hbase-solution.png) 

2. Klicken Sie auf „OMS-Portal“, um das OMS-Portal auf einer neuen Browserregisterkarte oder in einem neuen Fenster zu öffnen.

    ![Klicken auf „OMS-Portal“](./media/apache-hbase-monitor-with-oms/select-oms-portal.png) 

3. Klicken Sie auf der Startseite des OMS-Portals auf „Einstellungen“.

    ![OMS-Portal](./media/apache-hbase-monitor-with-oms/oms-portal-settings.png) 

4. Klicken Sie auf „Verbundene Quelle“ > „Linux-Server“.

    ![Klicken auf „Verbundene Quelle“ > „Linux-Server“](./media/apache-hbase-monitor-with-oms/select-linux-servers.png) 

5. Notieren Sie sich die Arbeitsbereichs-ID und den Primärschlüssel. Diese werden für die Skriptaktion benötigt.

### <a name="run-the-script-action"></a>Ausführen der Skriptaktion

Um die Datensammlung für Ihren HDInsight HBase-Cluster zu aktivieren, führen Sie eine Skriptaktion für alle Knoten im Cluster aus:

1. Navigieren Sie im Azure-Portal zum Bereich für Ihren HDInsight HBase-Cluster.
2. Klicken Sie auf **Skriptaktionen**.

    ![Skriptaktionen](./media/apache-hbase-monitor-with-oms/script-actions.png) 

3. Klicken Sie auf **Neue übermitteln**.

    ![Neue übermitteln](./media/apache-hbase-monitor-with-oms/script-actions-submit-new.png)  

4. Legen Sie in der Aktion **Skript übermitteln** den Skripttyp auf `"- Custom"` fest.
5. Geben Sie einen Namen für dieses Skript an.
6. Fügen Sie für **Bash-Skript-URI** den folgenden URI ein:

        https://raw.githubusercontent.com/hdinsight/HDInsightOMS/master/monitoring/script2.sh 

7. Wählen Sie unter **Knotentypen** alle drei Optionen (**Hauptknoten**, **Region** und **ZooKeeper**) aus.
8. Geben Sie im Textfeld **Parameter** Ihre Arbeitsbereichs-ID und Ihren Arbeitsbereichsschlüssel ein. Setzen Sie die Werte dabei jeweils in Anführungszeichen, und trennen Sie sie durch ein Leerzeichen.

        "<Workspace ID>" "<Workspace Key>"

9. Aktivieren Sie das Kontrollkästchen **Speichern Sie diese Skriptaktion...**, um die Aktion erneut auszuführen, wenn dem Cluster neue Knoten hinzugefügt werden.

    ![Einstellungen für die Skriptaktion](./media/apache-hbase-monitor-with-oms/submit-script-action.png)  

10. Klicken Sie auf **Erstellen**.
11. Die Ausführung der Skriptaktion dauert einige Minuten. Sie können den Status im Bereich für Skriptaktionen überwachen.

    ![Ausgeführte Skriptaktion](./media/apache-hbase-monitor-with-oms/script-action-running.png)  

12. Nach Abschluss der Skriptaktion wird in der Liste ein grünes Häkchen neben dem Skriptnamen angezeigt.

    ![Abgeschlossene Skriptaktion](./media/apache-hbase-monitor-with-oms/script-action-done.png)  

## <a name="view-the-hdinsight-hbase-monitoring-solution"></a>Anzeigen der HDInsight HBase-Überwachungslösung

Einige Minuten nach Abschluss der Skriptaktion sollten in der Überwachungslösung Daten angezeigt werden.

1. Navigieren Sie im Azure-Portal zum Bereich Ihrer HDInsight HBase-Lösung.
2. Klicken Sie auf die Registerkarte **Übersicht**.
3. Unter **Zusammenfassung** befindet sich eine Kachel mit der Anzahl überwachter Regionsserver.

    ![Kachel mit der Überwachungszusammenfassung](./media/apache-hbase-monitor-with-oms/monitoring-summary-tile.png)  

4. Klicken Sie auf die Kachel mit der Regionsserveranzahl. Das Hauptdashboard wird angezeigt.
5. Über dieses Dashboard können Sie auf Statistiken zu den Regionen, auf die Anzahl verwendeter Write-Ahead-Protokolle (Write-Ahead Logs, WALs), auf eine Sammlung von Log Analytics-Suchen (beispielsweise nach Regionsserverprotokollen, Phoenix-Protokollen und Ausnahmen) sowie auf eine Sammlung beliebter Diagramme zur Visualisierung relevanter Metriken zugreifen. 

    ![Hauptdashboard](./media/apache-hbase-monitor-with-oms/main-dashboard.png)  

6. Wenn Sie auf eine dieser Optionen klicken, können Sie ein Drilldown in die Ansicht „Protokollsuche“ durchführen und dort die Abfrage optimieren sowie eine detailliertere Datenanalyse vornehmen.

## <a name="next-steps"></a>Nächste Schritte

* [Arbeiten mit Warnungsregeln in Log Analytics](../../log-analytics/log-analytics-alerts-creating.md)
* [Suchen von Daten mit Protokollsuchen in Log Analytics](../../log-analytics/log-analytics-log-searches.md)
