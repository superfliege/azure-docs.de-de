---
title: Spark BI mit Datenvisualisierungstools unter Azure HDInsight | Microsoft-Dokumentation
description: "Verwenden von Datenvisualisierungstools für Analysen mithilfe von Apache Spark BI in HDInsight-Clustern"
keywords: Apache Spark Bi, Spark Bi, Spark-Datenvisualisierung, Spark-Business Intelligence
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 074415ba50ecdb1799093a3ead3bdd22fd02cc15
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI mit Datenvisualisierungstools unter Azure HDInsight

Erfahren Sie, wie Sie mit [Microsoft Power BI](http://powerbi.microsoft.com) und [Tableau](http://www.tableau.com) Daten in einem Apache Spark-Cluster unter Azure HDInsight visualisieren.

## <a name="prerequisites"></a>Voraussetzungen

* **Abschließen des Artikels [Ausführen interaktiver Abfragen auf Spark-Clustern in HDInsight](./apache-spark-load-data-run-query.md)**
* **Power BI:** [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) und [Power BI-Testabonnement](https://app.powerbi.com/signupredirect?pbi_source=web) (optional)
* **Tableau:** [Tableau Desktop](http://www.tableau.com/products/desktop) und [ODBC-Treiber für Microsoft Spark](http://go.microsoft.com/fwlink/?LinkId=616229)


## <a name="hivetable"></a>Überprüfen der Daten

Das im [vorherigen Tutorial](apache-spark-load-data-run-query.md) erstellte Jupyter Notebook enthält Code zum Erstellen einer `hvac`-Tabelle. Diese Tabelle basiert auf der CSV-Datei, die in allen HDInsight Spark-Clustern unter **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv** verfügbar ist. Gehen Sie folgendermaßen vor, um die Daten zu überprüfen.

1. Fügen Sie im Jupyter Notebook den folgenden Code ein, und drücken Sie **UMSCHALT+EINGABETASTE**. Der Code überprüft das Vorhandensein der Tabellen.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Die Ausgabe sieht wie folgt aus:

    ![Anzeigen von Tabellen in Spark](./media/apache-spark-use-bi-tools/show-tables.png)

    Wenn Sie das Notebook vor dem Starten dieses Tutorials geschlossen haben, wird `hvactemptable` bereinigt und ist daher nicht in der Ausgabe enthalten.
    Nur Hive-Tabellen, die im Metastore gespeichert werden (angegeben durch **False** in der Spalte **isTemporary**), sind für die BI-Tools zugänglich. In diesem Tutorial stellen Sie eine Verbindung mit der erstellten Tabelle **hvac** her.

2. Fügen Sie den folgenden Code in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABETASTE**. Der Code überprüft die Daten in der Tabelle.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    Die Ausgabe sieht wie folgt aus:

    ![Anzeigen von Zeilen aus der hvac-Tabelle in Spark](./media/apache-spark-use-bi-tools/select-limit.png)

3. Klicken Sie hierzu im Menü **Datei** des Notebooks auf **Close and Halt** (Schließen und Anhalten). Fahren Sie das Notebook herunter, um die Ressourcen freizugeben. 















## <a name="powerbi"></a>Verwenden von Power BI

In diesem Abschnitt verwenden Sie Power BI, um Visualisierungen, Berichte und Dashboards aus den Daten im Spark-Cluster zu erstellen. 

### <a name="create-a-report-in-power-bi-desktop"></a>Erstellen eines Berichts in Power BI Desktop
Die ersten Schritte bei der Verwendung von Spark bestehen darin, eine Verbindung mit dem Cluster in Power BI Desktop herzustellen, Daten aus dem Cluster zu laden und eine grundlegende Visualisierung auf der Grundlage dieser Daten zu erstellen.

> [!NOTE]
> Der in diesem Artikel vorgestellte Connector befindet sich derzeit in der Vorschau. Wenn Sie Feedback abgeben möchten, ist dies über die [Power BI-Communitywebsite](https://community.powerbi.com/) und [Power BI Ideas](https://ideas.powerbi.com/forums/265200-power-bi-ideas) möglich.

1. Öffnen Sie [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).
1. Klicken Sie auf der Registerkarte **Start** auf **Daten abrufen** und dann auf **Mehr**.

    ![Abrufen von Daten in Power BI Desktop aus HDInsight Apache Spark](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Abrufen von Daten in Power BI aus Apache Spark BI")


2. Geben Sie `Spark` im Suchfeld **Azure HDInsight Spark (Beta)** ein, und klicken Sie dann auf **Verbinden**.

    ![Einlesen von Daten in Power BI aus Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Einlesen von Daten in Power BI aus Apache Spark BI")

3. Geben Sie Ihre Cluster-URL (im Format `mysparkcluster.azurehdinsight.net`) ein, wählen Sie **DirectQuery** aus, und klicken Sie dann auf **OK**.

    Sie können mit Spark beide Datenkonnektivitätsmodi verwenden. Wenn Sie „DirectQuery“ verwenden, werden Änderungen in Berichten ohne Aktualisierung des gesamten Datasets wiedergegeben. Wenn Sie Daten importieren, müssen Sie das Dataset aktualisieren, um die Änderungen zu sehen. Weitere Informationen dazu, wie und wann Sie „DirectQuery“ verwenden, finden Sie unter [Verwenden von DirectQuery mit Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. Geben Sie die Kontoinformationen für die HDInsight-Anmeldung ein, und klicken Sie auf **Verbinden**. Der Standardkontoname lautet *admin*.

5. Wählen Sie die Tabelle `hvac` aus, warten Sie, um eine Vorschau der Daten anzuzeigen, und klicken Sie dann auf **Laden**.

    ![Benutzername und Kennwort des Spark-Clusters](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Benutzername und Kennwort des Spark-Clusters")

    Power BI Desktop verfügt nun über alle Informationen, die zum Herstellen einer Verbindung mit dem Spark-Cluster und zum Laden von Daten aus der Tabelle `hvac` erforderlich sind. Die Tabelle und ihre Spalten werden im Bereich **Felder** angezeigt.  Der folgende Screenshot zeigt dies:

6. Visualisieren Sie die Abweichung zwischen Zieltemperatur und Ist-Temperatur für jedes Gebäude: 

    1. Wählen Sie im Bereich **VISUALISIERUNGEN** die Option **Flächendiagramm** aus. 
    2. Ziehen Sie das Feld **BuildingID** unter **Achse** und die Felder **ActualTemp** und **TargetTemp** unter **Wert**.

        ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

        Das Diagramm sieht wie folgt aus:

        ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

        Standardmäßig werden in der Visualisierung die Summen für **ActualTemp** und **TargetTemp** angezeigt. Klicken Sie auf den Pfeil nach unten neben **ActualTemp** und **TargetTemp** im Bereich „Visualisierungen“. Sie sehen, dass **Summe** ausgewählt ist.

    3. Klicken Sie auf den Pfeil nach unten neben **ActualTemp** und **TargetTemp** im Bereich „Visualisierungen“, wählen Sie **Durchschnitt** aus, um den Durchschnittswert zwischen tatsächlicher und Zieltemperatur für jedes Gebäude zu erhalten.

        ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

        Ihre Datenvisualisierung sollte ähnlich dem Screenshot aussehen. Bewegen Sie den Cursor über die Visualisierung, um QuickInfos mit relevanten Daten abzurufen.

        ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

7. Klicken Sie auf **Datei** und dann auf **Speichern**, und geben Sie den Namen `BuildingTemperature.pbix` für die Datei ein. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Veröffentlichen des Berichts im Power BI-Dienst (optional)

Mit dem Power BI-Dienst können Sie Berichte und Dashboards in Ihrer Organisation freigeben. In diesem Abschnitt veröffentlichen Sie zunächst das DataSet und den Bericht. Dann heften Sie den Bericht an ein Dashboard an. Dashboards werden üblicherweise dazu verwendet, sich auf eine Teilmenge der Daten in einem Bericht zu konzentrieren. Ihr Bericht enthält zwar nur eine Visualisierung, es ist aber dennoch hilfreich, die Schritte durchzugehen.

1. Öffnen Sie Power BI Desktop.
2. Klicken Sie auf der Registerkarte **Start** auf **Veröffentlichen**.

    ![Veröffentlichen über Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Veröffentlichen über Power BI Desktop")

2. Wählen Sie einen Arbeitsbereich für die Veröffentlichung des Datasets und des Berichts aus, und klicken Sie dann auf **Auswählen**. In der folgenden Abbildung wird die Standardoption **My Workspace** (Mein Arbeitsbereich) ausgewählt.

    ![Auswählen eines Arbeitsbereichs für die Veröffentlichung des Datasets und des Berichts](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "Auswählen eines Arbeitsbereichs für die Veröffentlichung des Datasets und des Berichts") 

3. Nachdem die Veröffentlichung abgeschlossen ist, klicken Sie auf **Open „BuildingTemperature.pbix“ in Power BI** („BuildingTemperature.pbix“ in Power BI öffnen).

    ![Veröffentlichung erfolgreich, zur Eingabe von Anmeldeinformationen klicken](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "Veröffentlichung erfolgreich, zur Eingabe von Anmeldeinformationen klicken") 

4. Klicken Sie im Power BI-Dienst auf **Anmeldeinformationen eingeben**.

    ![Eingeben von Anmeldeinformationen im Power BI-Dienst](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Eingeben von Anmeldeinformationen im Power BI-Dienst")

5. Klicken Sie auf **Anmeldeinformationen bearbeiten**.

    ![Bearbeiten von Anmeldeinformationen im Power BI-Dienst](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Bearbeiten von Anmeldeinformationen im Power BI-Dienst")

6. Geben Sie die Kontoinformationen für die HDInsight-Anmeldung ein, und klicken Sie auf **Anmelden**. Der Standardkontoname lautet *admin*.

    ![Anmelden beim Spark-Cluster](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Anmelden beim Spark-Cluster")

7. Navigieren Sie im linken Bereich zu **Arbeitsbereiche** > **Mein Arbeitsbereich** > **BERICHTE**, und klicken Sie auf **BuildingTemperature**.

    ![Bericht unter „Berichte“ im linken Bereich](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "Bericht unter „Berichte“ im linken Bereich")

    Im linken Bereich sollte unter **DATASETS** außerdem **BuildingTemperature** aufgeführt sein.

    Das in Power BI Desktop erstellte visuelle Element ist nun im Power BI-Dienst verfügbar. 

8. Zeigen Sie mit der Maus auf die Visualisierung, und klicken Sie dann auf das Symbol zum Anheften in der rechten oberen Ecke.

    ![Bericht im Power BI-Dienst](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Bericht im Power BI-Dienst")

9. Wählen Sie „Neues Dashboard“, geben Sie den Namen `Building temperature` ein, und klicken Sie dann auf **Anheften**.

    ![An neues Dashboard anheften](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "An neues Dashboard anheften")

10. Klicken Sie im Bericht auf **Zum Dashboard wechseln**. 

Ihr visuelles Element wird an das Dashboard angeheftet. Sie können weitere visuelle Elemente zum Bericht hinzufügen und sie ans gleiche Dashboard anheften. Weitere Informationen zu Berichten und Dashboards finden Sie unter [Berichte in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/) und [Dashboards im Power BI-Dienst](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="tableau"></a>Verwenden von Tableau Desktop 

> [!NOTE]
> Dieser Abschnitt gilt nur für Spark 1.5.2-Cluster, die in Azure HDInsight erstellt wurden.
>
>

1. Installieren Sie [Tableau Desktop](http://www.tableau.com/products/desktop) auf dem Computer, auf dem Sie dieses Apache Spark BI-Tutorial durchführen.

2. Stellen Sie sicher, dass auf dem Computer auch der Microsoft Spark ODBC-Treiber installiert ist. Sie können den Treiber [hier](http://go.microsoft.com/fwlink/?LinkId=616229)installieren.

1. Starten Sie Tableau Desktop. Klicken Sie im linken Bereich in der Liste mit den verfügbaren Servern auf **Spark SQL**. Wenn Spark-SQL nicht standardmäßig im linken Bereich aufgeführt ist, finden Sie es durch Klicken auf **Mehr Server**.
2. Geben Sie im Dialogfeld für die Spark-SQL-Verbindung die Werte wie im Screenshot dargestellt ein, und klicken Sie dann auf **OK**.

    ![Herstellen einer Verbindung mit einem Cluster für Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Herstellen einer Verbindung mit einem Cluster für Apache Spark BI")

    In der Dropdownliste für die Authentifizierung wird der **Microsoft Azure HDInsight-Dienst** nur dann als Option aufgeführt, wenn Sie auf dem Computer den [Microsoft Spark-ODBC-Treiber](http://go.microsoft.com/fwlink/?LinkId=616229) installiert haben.
3. Klicken Sie auf dem nächsten Bildschirm in der Dropdownliste **Schema** auf das Symbol **Suchen** und dann auf **Standard**.

    ![Suchen eines Schemas für Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Suchen eines Schemas für Apache Spark BI")
4. Klicken Sie für das Feld **Tabelle** erneut auf das Symbol **Suchen**, um alle Hive-Tabellen aufzulisten, die im Cluster verfügbar sind. Daraufhin sollte die **hvac** -Tabelle angezeigt werden, die Sie zuvor mit dem Notebook erstellt haben.

    ![Suchen einer Tabelle für Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Suchen einer Tabelle für Apache Spark BI")
5. Legen Sie die Tabelle im oberen Feld auf der rechten Seite ab. Tableau importiert die Daten und zeigt das Schema wie in der Hervorhebung mit dem roten Kasten an.

    ![Hinzufügen von Tabellen zu Tableau für Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Hinzufügen von Tabellen zu Tableau für Apache Spark BI")
6. Klicken Sie unten links auf die Registerkarte **Blatt1** . Erstellen Sie eine Visualisierung, in der die durchschnittlichen Ziel- und Ist-Temperaturen für alle Gebäude und jedes Datum angezeigt werden. Ziehen Sie **Datum** und **Building ID** (Gebäude-ID) auf **Spalten** und **Actual Temp**/**Target Temp** (Tatsächliche Temperatur/Zieltemperatur) auf **Zeilen**. Wählen Sie unter **Markierungen** die Option **Bereich** aus, um eine Bereichskarte für die Spark-Datenvisualisierung zu verwenden.

     ![Hinzufügen von Feldern für die Spark-Datenvisualisierung](./media/apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Hinzufügen von Feldern für die Spark-Datenvisualisierung")
7. Standardmäßig werden die Temperaturfelder als Aggregatwerte angezeigt. Wenn Sie stattdessen die durchschnittlichen Temperaturen anzeigen möchten, können Sie die Dropdownliste verwenden, wie im nachstehenden Screenshot dargestellt:

    ![Ermitteln der Durchschnittstemperatur für die Spark-Datenvisualisierung](./media/apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Ermitteln der Durchschnittstemperatur für die Spark-Datenvisualisierung")

8. Sie können auch eine Temperaturkarte auf die andere Temperaturkarte legen, um ein besseres Gefühl für den Unterschied zwischen Ziel- und Ist-Temperaturen zu erhalten. Bewegen Sie den Mauszeiger in die Ecke der unteren Bereichskarte, bis das Griffsymbol mit einem roten Kreis markiert wird. Ziehen Sie die Karte in die andere Karte, die darüber liegt, und lassen Sie die Maustaste los, wenn die Form im roten Rechteck hervorgehoben wird.

    ![Zusammenführen von Karten für die Spark-Datenvisualisierung](./media/apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Zusammenführen von Karten für die Spark-Datenvisualisierung")

     Die Visualisierung der Daten sollte sich wie im Screenshot dargestellt ändern:

    ![Tableau-Ausgabe für die Spark-Datenvisualisierung](./media/apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau-Ausgabe für die Spark-Datenvisualisierung")
9. Klicken Sie auf **Speichern** , um das Arbeitsblatt zu speichern. Sie können Dashboards erstellen und ein oder mehrere Blätter hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

Bis hier her haben Sie erfahren, wie Sie ein Cluster und Spark-Datenrahmen zum Abfrage von Daten erstellen können und wie Sie dann auf diese Daten von BI-Tools aus zugreifen. Es stehen Ihnen nun Anweisungen zur Verfügung, wie Sie Clusterressourcen verwalten und Aufträge debuggen können, die ein einem Cluster von HDInsight Spark ausgeführt werden.

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)

