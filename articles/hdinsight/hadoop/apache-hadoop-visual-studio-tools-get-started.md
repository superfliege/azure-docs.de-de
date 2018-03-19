---
title: "Herstellen einer Verbindung mit Azure HDInsight mithilfe von Data Lake Tools für Visual Studio | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Data Lake Tools für Visual Studio installieren und verwenden, um eine Verbindung mit Hadoop-Clustern in Azure HDInsight herzustellen, und anschließend Hive-Abfragen ausführen."
keywords: Hadoop-Tools, Hive-Abfrage, Visual Studio, Visual Studio Hadoop
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: afd40d75bb9c5fd3170a4da215925244994d7749
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-hive-queries"></a>Herstellen einer Verbindung mit Azure HDInsight und Ausführen von Hive-Abfragen mithilfe von Data Lake Tools für Visual Studio

Erfahren Sie, wie Sie mit Data Lake Tools für Visual Studio (auch als Azure Data Lake- und Stream Analytics-Tools für Visual Studio bezeichnet) eine Verbindung mit Hadoop-Clustern in [Azure HDInsight](../hdinsight-hadoop-introduction.md) herstellen und Hive-Abfragen übermitteln. 

Weitere Informationen zur Verwendung von HDInsight finden Sie unter [Einführung in HDInsight](../hdinsight-hadoop-introduction.md) und [Erste Schritte mit HDInsight](apache-hadoop-linux-tutorial-get-started.md). 

Weitere Informationen zum Herstellen einer Verbindung mit einem Storm-Cluster finden Sie im Artikel zum [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Sie können Data Lake Tools für Visual Studio verwenden, um auf Data Lake Analytics und HDInsight zuzugreifen. Weitere Informationen zu Data Lake Tools finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake Tools für Visual Studio](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial durchzuführen und Data Lake Tools für Visual Studio zu verwenden, benötigen Sie Folgendes:

* Ein Azure HDInsight-Cluster. Informationen zum Erstellen eines Azure HDInsight-Clusters finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](apache-hadoop-linux-tutorial-get-started.md). Zum Ausführen interaktiver Hive-Abfragen benötigen Sie einen [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md)-Cluster.
* Einen Computer, auf dem Visual Studio 2017, 2015 oder 2013 installiert ist.
    
    > [!NOTE]
    > Derzeit ist nur die englische Version von Data Lake Tools für Visual Studio verfügbar.
    > 
    > 

## <a name="install-or-update-data-lake-tools-for-visual-studio"></a>Installieren oder Aktualisieren von Azure Data Lake Tools für Visual Studio

### <a name="install-data-lake-tools"></a>Installieren von Data Lake Tools

Data Lake-Tools sind bei Visual Studio-2017 standardmäßig installiert. Für frühere Versionen von Visual Studio können Sie Data Lake Tools mit dem [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) installieren. Wählen Sie die passende Version von Data Lake Tools für Ihre Visual Studio-Version aus. 

### <a name="install-visual-studio"></a>Installieren von Visual Studio

Falls Sie Visual Studio nicht installiert haben, installieren Sie mit dem [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) die aktuellen Visual Studio Community- und Azure SDK-Versionen:

![Screenshot des Webplattform-Installers für Data Lake Tools für Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Verwenden des Webplattform-Installers zum Installieren von Data Lake Tools für Visual Studio")

### <a name="update-the-tools"></a>Aktualisieren der Tools

1. Öffnen Sie Visual Studio.
2. Klicken Sie im Menü **Extras** auf **Extensions and updates** (Erweiterungen und Updates).
3. Erweitern Sie **Updates**, und wählen Sie dann **Azure Data Lake and Stream Analytics Tools** (Azure Data Lake- und Stream Analytics-Tools) aus (sofern noch nicht installiert).

> [!NOTE]
>
> Sie können Data Lake Tools Version 2.3.0.0 oder höher nur verwenden, um eine Verbindung mit Interactive Query-Clustern herzustellen und interaktive Hive-Abfragen auszuführen.

## <a name="connect-to-azure-subscriptions"></a>Herstellen einer Verbindung mit Azure-Abonnements
Mithilfe von Data Lake Tools für Visual Studio können Sie eine Verbindung mit Ihren HDInsight-Clustern herstellen, einige einfache Verwaltungsvorgänge durchführen und Hive-Abfragen ausführen.

> [!NOTE]
> Informationen zum Herstellen einer Verbindung mit einem generischen Hadoop-Cluster finden Sie unter [Write and submit Hive queries by using Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx) (Schreiben und Übermitteln von Hive-Abfragen mithilfe von Visual Studio).
> 
> 

So stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her:

1. Öffnen Sie Visual Studio.
2. Klicken Sie im Menü **Ansicht** auf **Server-Explorer**.
3. Erweitern Sie im Server-Explorer erst **Azure** und dann **HDInsight**.
   
   > [!NOTE]
   > Das Fenster **HDInsight Task List** (HDInsight-Taskliste) sollte geöffnet sein. Falls Sie das Fenster nicht sehen, klicken Sie im Menü **Ansicht** auf **Other Windows** (Weitere Fenster), und wählen Sie dann **HDInsight Task List Window** (HDInsight-Tasklistenfenster) aus.  
   > 
   > 
4. Geben Sie die Anmeldeinformationen für Ihr Azure-Abonnement ein, und klicken Sie auf **Anmelden**. Die Authentifizierung ist nur erforderlich, wenn Sie in Visual Studio auf diesem Computer noch nie eine Verbindung mit dem Azure-Abonnement hergestellt haben.
5. Im Server-Explorer wird eine Liste der vorhandenen HDInsight-Cluster angezeigt. Wenn Sie noch keine Cluster haben, können Sie über das Azure-Portal, Azure PowerShell oder das HDInsight SDK Cluster erstellen. Weitere Informationen finden Sie unter [Erstellen von HDInsight-Clustern](../hdinsight-hadoop-provision-linux-clusters.md).
   
   ![Screenshot der Clusterliste von Data Lake Tools für Visual Studio im Server-Explorer](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Clusterliste von Data Lake Tools für Visual Studio im Server-Explorer")
6. Erweitern Sie einen HDInsight-Cluster. **Hive-Datenbanken**, das Standardspeicherkonto, verknüpfte Speicherkonten und das **Hadoop-Dienstprotokoll** werden angezeigt. Sie können die einzelnen Elemente weiter erweitern.

Nach dem Herstellen der Verbindung mit Ihrem Azure-Abonnement können Sie die folgenden Aufgaben durchführen.

So stellen Sie in Visual Studio eine Verbindung mit dem Azure-Portal her:

1. Klicken Sie im Server-Explorer auf **Azure** > **HDInsight**.
2. Klicken Sie mit der rechten Maustaste auf einen HDInsight-Cluster, und wählen Sie dann **Manage Cluster in Azure portal** (Cluster im Azure-Portal verwalten) aus.

So können Sie in Visual Studio Fragen stellen und Feedback geben:

1. Klicken Sie im Menü **Extras** auf **HDInsight**.
2. Wählen Sie **MSDN Forum** (MSDN-Forum) aus, um Fragen zu stellen. Wählen Sie **Give Feedback** (Feedback geben) aus, um Feedback zu senden.

## <a name="explore-linked-resources"></a>Erkunden der verknüpften Ressourcen
Im Server-Explorer werden das Standardspeicherkonto und alle verknüpften Speicherkonten angezeigt. Wenn Sie das Standardspeicherkonto erweitern, werden die Container im Speicherkonto angezeigt. Das Standardspeicherkonto und der Standardcontainer sind markiert. Klicken Sie mit der rechten Maustaste auf einen der Container, um seinen Inhalt anzuzeigen.

![Screenshot der Liste der verknüpften Ressourcen von Data Lake Tools für Visual Studio im Server-Explorer](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "Liste der verknüpften Ressourcen")

Nach dem Öffnen eines Containers können Sie mithilfe der folgenden Schaltflächen Blobs hochladen, löschen und herunterladen:

![Screenshot der Blobvorgänge von Data Lake Tools für Visual Studio im Server-Explorer](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "Hochladen, Löschen und Herunterladen von Blobs im Server-Explorer")

## <a name="run-interactive-hive-queries"></a>Ausführen von interaktiven Hive-Abfragen
[Apache Hive](http://hive.apache.org) ist eine Data Warehouse-Infrastruktur, die auf Hadoop basiert. Hive wird für die Datenzusammenfassung, Abfragen und die Analyse verwendet. Sie können Data Lake Tools für Visual Studio verwenden, um Hive-Abfragen in Visual Studio auszuführen. Weitere Informationen zu Hive finden Sie unter [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md).

[Interactive Query](../interactive-query/apache-interactive-query-get-started.md) verwendet [Hive unter LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) in Apache Hive 2.1. Interactive Query bietet Interaktivität für komplexe Data Warehouse-Abfragen von umfangreichen, gespeicherten Datasets. Die Ausführung von Hive-Abfragen mit Interactive Query ist im Vergleich zu herkömmlichen Hive-Batchaufträgen deutlich schneller. Weitere Informationen finden Sie unter [Ausführen von Hive-Batchaufträgen](#run-hive-batch-jobs).

> [!NOTE]
>
> Das Ausführen von interaktiven Hive-Abfragen ist nur möglich, wenn Sie eine Verbindung mit einem [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md)-Cluster herstellen.

Sie können Data Lake Tools für Visual Studio auch verwenden, um den Inhalt eines Hive-Auftrags anzuzeigen. Data Lake Tools für Visual Studio sammelt die YARN-Protokolle bestimmter Hive-Aufträge und zeigt sie an.

### <a name="view-hivesampletable"></a>Anzeigen von **hivesampletable**
Alle HDInsight-Cluster verfügen über eine Hive-Beispieltabelle namens „hivesampletable“. Die Hive-Tabelle definiert, wie Hive-Tabellen und die darin enthaltenen Zeilen aufgelistet sowie Tabellenschemas angezeigt werden.

So listen Sie Hive-Tabellen auf und zeigen Sie das Hive-Tabellenschema an:

1. Klicken Sie zum Anzeigen des Tabellenschemas im **Server-Explorer** auf **Azure** > **HDInsight**. Wählen Sie Ihren Cluster aus, und klicken Sie dann auf **Hive Databases (Hive-Datenbanken)** > **Standard** > **Hivesampletable**.
2. Klicken Sie mit der rechten Maustaste auf **hivesampletable**, und klicken Sie dann auf **Die ersten 100 Zeilen anzeigen**, um die Zeilen aufzulisten. Dies entspricht dem Ausführen der folgenden Hive-Abfrage mithilfe des Hive-ODBC-Treibers:
   
     `SELECT * FROM hivesampletable LIMIT 100`
   
   Sie können die Anzahl der Zeilen anpassen.
   
   ![Screenshot einer HDInsight Hive Visual Studio-Schemaabfrage](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Hive-Abfrageergebnisse")

### <a name="create-hive-tables"></a>Erstellen von Hive-Tabellen
Zum Erstellen einer Hive-Tabelle können Sie die grafische Benutzeroberfläche oder Hive-Abfragen verwenden. Informationen zum Verwenden von Hive-Abfragen finden Sie unter [Ausführen von Hive-Abfragen](#run.queries).

So erstellen Sie eine Hive-Tabelle:

1. Klicken Sie im **Server-Explorer** auf **Azure** > **HDInsight-Cluster**. Wählen Sie Ihren HDInsight-Cluster aus, und klicken Sie dann auf **Hive Databases** (Hive-Datenbanken).
2. Klicken Sie mit der rechten Maustaste auf **Standard**, und wählen Sie dann **Tabelle erstellen** aus.
3. Konfigurieren Sie die Tabelle.  
4. Klicken Sie auf **Tabelle erstellen**, um den Auftrag zum Erstellen der neuen Hive-Tabelle zu übermitteln.
   
    ![Screenshot des Fensters „Tabelle erstellen“ der HDInsight Visual Studio-Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Erstellen einer Hive-Tabelle")

### <a name="run.queries"></a>Überprüfen und Ausführen von Hive-Abfragen
Zum Erstellen und Ausführen von Hive-Abfragen stehen Ihnen zwei Möglichkeiten zur Auswahl:

* Erstellen von Ad-hoc-Abfragen
* Erstellen einer Hive-Anwendung

So erstellen und überprüfen Sie Ad-hoc-Abfragen und führen Sie sie aus:

1. Klicken Sie im **Server-Explorer** auf **Azure** > **HDInsight-Cluster**.
2. Klicken Sie mit der rechten Maustaste auf den Cluster, in dem Sie die Abfrage ausführen möchten, und wählen Sie dann **Write a Hive Query** (Hive-Abfrage schreiben) aus.  
3. Geben Sie die Hive-Abfragen ein. 

    Der Hive-Editor unterstützt IntelliSense. Data Lake Tools für Visual Studio unterstützt das Laden von Remotemetadaten, wenn Sie Ihr Hive-Skript bearbeiten. Wenn Sie beispielsweise **SELECT * FROM** eingeben, listet IntelliSense alle vorgeschlagenen Tabellennamen auf. Wird ein Tabellenname angegeben, listet IntelliSense die Spaltennamen auf. Die Tools unterstützen die meisten Hive-DML-Anweisungen, Unterabfragen und integrierte UDFs.
   
    ![Screenshot von Beispiel 1 für HDInsight Visual Studio-Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Screenshot von Beispiel 2 für HDInsight Visual Studio-Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > IntelliSense schlägt nur die Metadaten des Clusters vor, der auf der HDInsight-Symbolleiste ausgewählt ist.
   > 
   
4. (Optional) Klicken Sie auf **Validate Script** (Skript überprüfen), um das Skript auf Syntaxfehler zu überprüfen.
   
    ![Screenshot der lokalen Überprüfung von Data Lake Tools für Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Überprüfen von Skripts")
5. Klicken Sie auf **Submit** (Senden) oder **Submit (Advanced)** (Erweitertes Senden). Wenn Sie die Option für erweitertes Senden auswählen, konfigurieren Sie den **Auftragsnamen**, **Argumente**, **zusätzliche Konfigurationen** und das **Statusverzeichnis** für das Skript:
   
    ![Screenshot einer HDInsight Hadoop Hive-Abfrage](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Übermitteln von Abfragen")
   
    Nachdem Sie den Auftrag gesendet haben, wird das Fenster **Hive Job Summary** (Hive-Auftragszusammenfassung) angezeigt.
   
    ![Screenshot der Zusammenfassung einer HDInsight Hadoop Hive-Abfrage](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Hive-Auftragszusammenfassung")
6. Klicken Sie auf die Schaltfläche **Aktualisieren**, um den Status zu aktualisieren, bis sich der Auftragsstatus in **Abgeschlossen** ändert.
7. Klicken Sie unten auf die Links, um die **Auftragsabfrage**, die **Auftragsausgabe**, das **Auftragsprotokoll** oder das **YARN-Protokoll** anzuzeigen.

So erstellen Sie eine Hive-Lösung und führen Sie sie aus:

1. Klicken Sie im Menü **Datei** auf **Neu** und anschließend auf **Projekt**.
2. Wählen Sie im linken Bereich **HDInsight** aus. Wählen Sie im mittleren Bereich **Hive Application** (Hive-Anwendung) aus. Geben Sie die Eigenschaften ein, und klicken Sie dann auf **OK**.
   
    ![Screenshot eines neuen Hive-Projekts in den HDInsight Visual Studio-Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Erstellen von Hive-Anwendungen in Visual Studio")
3. Doppelklicken Sie im **Projektmappen-Explorer** auf **Script.hql**, um das Skript zu öffnen.
4. Klicken Sie zum Überprüfen des Hive-Skripts auf die Schaltfläche **Validate Script** (Skript überprüfen). Sie können auch im Hive-Editor mit der rechten Maustaste auf das Skript klicken und dann im Kontextmenü die Option **Validate Script** (Skript überprüfen) auswählen.

### <a name="view-hive-jobs"></a>Anzeigen von Hive-Aufträgen
Sie können Auftragsabfragen, die Auftragsausgabe, Auftragsprotokolle und YARN-Protokolle für Hive-Aufträge anzeigen. Weitere Informationen können Sie dem obigen Screenshot entnehmen.

In der neuesten Version der Tools können Sie den Inhalt Ihrer Hive-Aufträge anzeigen, indem Sie YARN-Protokolle erfassen und anzeigen. Ein YARN-Protokoll kann Ihnen bei der Untersuchung von Leistungsproblemen helfen. Weitere Informationen zur Erfassung von YARN-Protokollen durch HDInsight finden Sie unter [Zugriff auf YARN-Anwendungsprotokolle unter Windows-basiertem HDInsight](../hdinsight-hadoop-access-yarn-app-logs.md).

So zeigen Sie Hive-Aufträge an:

1. Erweitern Sie im **Server-Explorer** erst **Azure** und dann **HDInsight**.
2. Klicken Sie mit der rechten Maustaste auf einen HDInsight-Cluster, und klicken Sie dann auf **Aufträge anzeigen**. Eine Liste der für den Cluster ausgeführten Hive-Aufträge wird angezeigt.  
3. Wählen Sie einen Auftrag aus. Wählen Sie im Fenster **Hive Job Summary** (Hive-Auftragszusammenfassung) eine der folgenden Optionen aus:
    - **Job Query** (Auftragsabfrage)
    - **Auftragsausgabe**
    - **Job Log** (Auftragsprotokoll)  
    - **Yarn log** (YARN-Protokoll)
   
    ![Screenshot des Fensters „View Hive Jobs“ (Hive-Aufträge anzeigen) der HDInsight Visual Studio-Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Anzeigen von Hive-Aufträgen")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Schnellere Hive-Ausführung über HiveServer2
> [!NOTE]
> Dieses Feature funktioniert nur in einem Cluster in HDInsight Version 3.2 oder höher.
 
In Data Lake Tools für Visual Studio wurden Hive-Aufträge bisher über [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (auch bekannt als Templeton) übermittelt. Bei dieser Methode zum Übermitteln von Hive-Aufträgen hat es sehr lange gedauert, bis Auftragsdetails und Fehlerinformationen zurückgegeben wurden.

Um dieses Leistungsproblem zu beheben, kann Data Lake Tools für Visual Studio RDP/SSH umgehen und Hive-Aufträge über HiveServer2 direkt im Cluster ausführen.

Neben der Leistungsverbesserung bietet diese Methode Ihnen die Möglichkeit, Hive in Apache Tez-Diagrammen und in den Aufgabendetails anzuzeigen.

In einem Cluster in HDInsight-Version 3.2 oder höher wird eine Schaltfläche **Execute via HiveServer2** (Über HiveServer2 ausführen) angezeigt:

![Screenshot der Option „Execute via HiveServer2“ (Über HiveServer2 ausführen) in Data Lake Tools für Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "Ausführen von Hive-Abfragen mit HiveServer2")

Sie können auch die in Echtzeit zurückgestreamten Protokolle anzeigen. Zudem können Sie die Auftragsdiagramme anzeigen, wenn die Hive-Abfrage in Tez ausgeführt wird.

![Screenshot zur schnelleren Hive-Ausführung in Data Lake Tools für Visual Studio mit HiveServer2](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "Anzeigen von Auftragsdiagrammen")

### <a name="execute-queries-via-hiveserver2-vs-submit-queries-via-webhcat"></a>Ausführen von Abfragen über HiveServer2 im Vergleich zum Übermitteln von Abfragen über WebHCat

Obwohl die Ausführung von Abfragen über HiveServer2 viele Leistungsvorteile bietet, gelten bei dieser Methode doch auch verschiedene Einschränkungen. Aufgrund einiger dieser Einschränkungen eignet sich die Methode nicht zur Verwendung in einer Produktionsumgebung. 

In der folgenden Tabelle sind die Unterschiede zwischen dem Ausführen von Abfragen über HiveServer2 und dem Übermitteln von Abfragen über WebHCat aufgeführt:

|  | Ausführen über HiveServer2 | Übermitteln über WebHCat |
| --- | --- | --- |
| Ausführen von Abfragen |Der Mehraufwand in WebHCat (Start eines MapReduce-Auftrags namens „TempletonControllerJob“) entfällt. |Wenn eine Abfrage über WebHCat ausgeführt wird, startet WebHCat einen MapReduce-Auftrag, der mit zusätzlicher Wartezeit verbunden ist. |
| Zurückstreamen von Protokollen |Nahezu in Echtzeit. |Die Auftragsausführungsprotokolle sind nur verfügbar, wenn der Auftrag abgeschlossen ist. |
| Anzeige des Auftragsverlaufs |Wenn eine Abfrage über HiveServer2 ausgeführt wird, wird der Auftragsverlauf (Auftragsprotokoll, Auftragsausgabe) nicht beibehalten. Sie können die Anwendung in der YARN-Benutzeroberfläche mit begrenzten Informationen anzeigen. |Wenn eine Abfrage über WebHCat ausgeführt wird, wird der Auftragsverlauf (Auftragsprotokoll, Auftragsausgabe) beibehalten. Sie können den Auftragsverlauf in Visual Studio, mit dem HDInsight SDK oder PowerShell anzeigen. |
| Fenster schließen |Die Ausführung über HiveServer2 erfolgt *synchron*. Wenn die Fenster geschlossen werden, wird die Ausführung der Abfrage abgebrochen. |Die Übermittlung über WebHCat erfolgt *asynchron*. Sie können die Abfrage über WebHCat senden und Visual Studio dann schließen. Sie können jederzeit zurückkehren und die Ergebnisse anzeigen. |

### <a name="tez-hive-job-performance-graph"></a>Leistungsdiagramm für Tez Hive-Auftrag
In Data Lake Tools für Visual Studio können Sie Leistungsdiagramme für die Hive-Aufträge anzeigen, die vom Tez-Ausführungsmodul ausgeführt werden. Informationen zum Aktivieren von Tez finden Sie im Thema zur [Verwendung von Hive in HDInsight](hdinsight-use-hive.md). 

Nachdem Sie in Visual Studio einen Hive-Job übermittelt haben, wird nach Abschluss des Auftrags das Diagramm in Visual Studio angezeigt. Sie müssen ggf. auf die Schaltfläche **Aktualisieren** klicken, um den aktuellen Auftragsstatus anzuzeigen.

> [!NOTE]
> Dieses Feature ist nur für einen Cluster in HDInsight-Version 3.2.4.593 oder höher verfügbar. Das Feature kann nur für abgeschlossene Aufträge verwendet werden. Außerdem müssen Sie den Auftrag über WebHCat übermitteln, um dieses Feature verwenden zu können. Der folgende Bildschirm wird angezeigt, wenn Sie Ihre Abfrage über HiveServer2 ausführen: 
> 
> ![Screenshot eines Hadoop Hive Tez-Leistungsdiagramms](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "Auftragsstatus")

Um Ihnen das Verständnis der Hive-Abfrage zu erleichtern, wurde in dieser Version die Ansicht für Hive-Operatoren als neues Feature hinzugefügt. Um alle Operatoren am Scheitelpunkt anzuzeigen, doppelklicken Sie auf die Scheitelpunkte des Auftragsdiagramms. Sie können auch auf einen bestimmten Operator zeigen, um weitere Details anzuzeigen.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Aufgabenausführungsansicht für Hive unter Tez-Aufträge
In der Aufgabenausführungsansicht für Hive unter Tez-Aufträge können Sie strukturierte und visualisierte Informationen zu Hive-Aufträgen anzeigen. Sie können auch weitere Auftragsinformationen abrufen. Im Fall von Leistungsproblemen können Sie in der Ansicht weitere Details zum Problem anzeigen. Beispielsweise können Sie Informationen zur Funktion jeder Aufgabe und ausführliche Informationen zu einzelnen Aufgaben (Lese-/Schreibzugriff auf Daten, Zeitplan, Start- und Endzeit usw.) anzeigen. Mithilfe der visualisierten Informationen können Sie Auftragskonfigurationen oder die Systemarchitektur optimieren.

![Screenshot der Aufgabenausführungsansicht von Data Lake Tools für Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Aufgabenausführungsansicht")

## <a name="run-hive-batch-jobs"></a>Ausführen von Hive-Batchaufträgen
Das Testen von Hive-Skripts für einen HDInsight-Cluster (mit Ausnahme des Interactive Query-Clusters) kann zeitaufwändig sein. Der Vorgang kann mehrere Minuten in Anspruch nehmen. Data Lake Tools für Visual Studio kann Hive-Skripts ohne Verbindung mit einem aktiven Cluster lokal überprüfen. Weitere Informationen zur Ausführung von interaktiven Abfragen finden Sie unter [Ausführen von interaktiven Hive-Abfragen](#run-interactive-hive-queries).

Mithilfe von Data Lake Tools für Visual Studio können Sie auch den Inhalt des Hive-Auftrags anzeigen, indem Sie die YARN-Protokolle bestimmter Hive-Aufträge sammeln und anzeigen.

Weitere Informationen zur Ausführung von Hive-Batchaufträgen finden Sie unter [Ausführen von interaktiven Hive-Abfragen](#run-interactive-hive-queries). Die Informationen in diesem Abschnitt gelten für die Ausführung von Hive-Batchaufträgen mit längerer Ausführungsdauer.

## <a name="run-pig-scripts"></a>Ausführen von Pig-Skripts
Sie können mit Data Lake Tools für Visual Studio Pig-Skripts erstellen und an HDInsight-Cluster übermitteln. Erstellen Sie dazu zunächst anhand einer Vorlage ein Pig-Projekt. Übermitteln Sie das Skript dann an HDInsight-Cluster.

## <a name="feedback-and-known-issues"></a>Feedback und bekannte Probleme
* Derzeit werden HiveServer2-Ergebnisse als Klartext angezeigt, was nicht ideal ist. Microsoft arbeitet daran, dieses Problem zu beheben.
* Ein Problem, aufgrund dessen mit NULL-Werten gestartete Ergebnisse nicht angezeigt werden, wurde behoben. Wenden Sie sich an das Supportteam, falls die Ausführung bei Ihnen durch dieses Problem blockiert wird.
* Das von Visual Studio erstellte HQL-Skript wird abhängig von der lokalen Regionseinstellung des Benutzers codiert. Das Skript wird nicht korrekt ausgeführt, wenn Sie es als Binärdatei in einen Cluster hochladen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie das Data Lake Tools für Visual Studio-Paket verwenden, um in Visual Studio eine Verbindung mit HDInsight-Clustern herzustellen. Zudem haben Sie gelernt, wie Sie eine Hive-Abfrage ausführen. Weitere Informationen und Beispiele finden Sie in diesen Artikeln:

* [Verwenden von Hadoop Hive in HDInsight](hdinsight-use-hive.md)
* [Erste Schritte mit Hadoop in HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Übermitteln von Hadoop-Aufträgen in HDInsight](submit-apache-hadoop-jobs-programmatically.md)
* [Analysieren von Twitter-Daten mit Hadoop in HDInsight](../hdinsight-analyze-twitter-data.md)

