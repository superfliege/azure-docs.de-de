---
title: 'Tutorial: Analysieren von Apache Spark-Daten mithilfe von Power BI in Azure HDInsight '
description: Verwenden von Microsoft Power BI zum Visualisieren von Apache Spark-Daten, die in HDInsight-Clustern gespeichert sind
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/07/2018
ms.openlocfilehash: ed7beeadc0a550a28d1f936702aabeb45823b677
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64699604"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>Tutorial: Analysieren von Apache Spark-Daten mithilfe von Power BI in HDInsight 

Hier erfahren Sie, wie Sie mit [Microsoft Power BI](https://powerbi.microsoft.com/) Daten in einem [Apache Spark](https://spark.apache.org/)-Cluster in [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) visualisieren.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Visualisieren von Spark-Daten mithilfe von Power BI

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* **Lesen Sie den Artikel [Tutorial: Laden von Daten und Ausführen von Abfragen auf einem Apache Spark-Cluster in Azure HDInsight](./apache-spark-load-data-run-query.md)**.
* **Power BI**: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) und [Power BI-Testabonnement](https://app.powerbi.com/signupredirect?pbi_source=web) (optional)


## <a name="verify-the-data"></a>Überprüfen der Daten

Das im [vorherigen Tutorial](apache-spark-load-data-run-query.md) erstellte [Jupyter Notebook](https://jupyter.org/) enthält Code zum Erstellen einer `hvac`-Tabelle. Diese Tabelle basiert auf der CSV-Datei, die in allen HDInsight Spark-Clustern unter **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv** verfügbar ist. Gehen Sie folgendermaßen vor, um die Daten zu überprüfen.

1. Fügen Sie im Jupyter Notebook den folgenden Code ein, und drücken Sie **UMSCHALT+EINGABETASTE**. Der Code überprüft das Vorhandensein der Tabellen.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Die Ausgabe sieht wie folgt aus:

    ![Anzeigen von Tabellen in Spark](./media/apache-spark-use-bi-tools/show-tables.png)

    Wenn Sie das Notebook vor dem Starten dieses Tutorials geschlossen haben, wird `hvactemptable` bereinigt und ist daher nicht in der Ausgabe enthalten.  Nur Hive-Tabellen, die im Metastore gespeichert werden (angegeben durch **False** in der Spalte **isTemporary**), sind für die BI-Tools zugänglich. In diesem Tutorial stellen Sie eine Verbindung mit der erstellten Tabelle **hvac** her.

2. Fügen Sie den folgenden Code in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABETASTE**. Der Code überprüft die Daten in der Tabelle.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    Die Ausgabe sieht wie folgt aus:

    ![Anzeigen von Zeilen aus der hvac-Tabelle in Spark](./media/apache-spark-use-bi-tools/select-limit.png)

3. Klicken Sie hierzu im Menü **Datei** des Notebooks auf **Close and Halt** (Schließen und Anhalten). Fahren Sie das Notebook herunter, um die Ressourcen freizugeben. 

## <a name="visualize-the-data"></a>Visualisieren der Daten

In diesem Abschnitt verwenden Sie Power BI, um Visualisierungen, Berichte und Dashboards aus den Daten im Spark-Cluster zu erstellen. 

### <a name="create-a-report-in-power-bi-desktop"></a>Erstellen eines Berichts in Power BI Desktop
Die ersten Schritte bei der Verwendung von Spark bestehen darin, eine Verbindung mit dem Cluster in Power BI Desktop herzustellen, Daten aus dem Cluster zu laden und eine grundlegende Visualisierung auf der Grundlage dieser Daten zu erstellen.

> [!NOTE]  
> Der in diesem Artikel vorgestellte Connector befindet sich derzeit in der Vorschau. Wenn Sie Feedback abgeben möchten, ist dies über die [Power BI-Communitywebsite](https://community.powerbi.com/) und [Power BI Ideas](https://ideas.powerbi.com/forums/265200-power-bi-ideas) möglich.

1. Öffnen Sie [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).
1. Klicken Sie auf der Registerkarte **Start** auf **Daten abrufen** und dann auf **Mehr**.

    ![Abrufen von Daten in Power BI Desktop aus HDInsight Apache Spark](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Abrufen von Daten in Power BI aus Apache Spark BI")


2. Geben Sie ins Suchfeld `Spark` ein, wählen Sie **Azure HDInsight Spark** aus, und klicken Sie dann auf **Verbinden**.

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

        ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

        Standardmäßig werden in der Visualisierung die Summen für **ActualTemp** und **TargetTemp** angezeigt. Klicken Sie auf den Pfeil nach unten neben **ActualTemp** und **TargetTemp** im Bereich „Visualisierungen“. Sie sehen, dass **Summe** ausgewählt ist.

    3. Klicken Sie auf den Pfeil nach unten neben **ActualTemp** und **TargetTemp** im Bereich „Visualisierungen“, wählen Sie **Durchschnitt** aus, um den Durchschnittswert zwischen tatsächlicher und Zieltemperatur für jedes Gebäude zu erhalten.

        ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

        Ihre Datenvisualisierung sollte ähnlich dem Screenshot aussehen. Bewegen Sie den Cursor über die Visualisierung, um QuickInfos mit relevanten Daten abzurufen.

        ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

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

Ihr visuelles Element wird an das Dashboard angeheftet. Sie können weitere visuelle Elemente zum Bericht hinzufügen und sie ans gleiche Dashboard anheften. Weitere Informationen zu Berichten und Dashboards finden Sie unter [Berichte in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/) und [Einführung in Dashboards für Power BI-Designer](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

<!--
## <a name="tableau"></a>Use Tableau Desktop 

> [!NOTE]
> This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.
>
>

1. Install [Tableau Desktop](https://www.tableau.com/products/desktop) on the computer where you are running this Apache Spark BI tutorial.

2. Make sure that computer also has Microsoft Spark ODBC driver installed. You can install the driver from [here](https://go.microsoft.com/fwlink/?LinkId=616229).

1. Launch Tableau Desktop. In the left pane, from the list of server to connect to, click **Spark SQL**. If Spark SQL is not listed by default in the left pane, you can find it by click **More Servers**.
2. In the Spark SQL connection dialog box, provide the values as shown in the screenshot, and then click **OK**.

    ![Connect to a cluster for Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect to a cluster for Apache Spark BI")

    The authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed the [Microsoft Spark ODBC Driver](https://go.microsoft.com/fwlink/?LinkId=616229) on the computer.
3. On the next screen, from the **Schema** drop-down, click the **Find** icon, and then click **default**.

    ![Find schema for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Find schema for Apache Spark BI")
4. For the **Table** field, click the **Find** icon again to list all the Hive tables available in the cluster. You should see the **hvac** table you created earlier using the notebook.

    ![Find table for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Find table for Apache Spark BI")
5. Drag and drop the table to the top box on the right. Tableau imports the data and displays the schema as highlighted by the red box.

    ![Add tables to Tableau for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Add tables to Tableau for Apache Spark BI")
6. Click the **Sheet1** tab at the bottom left. Make a visualization that shows the average target and actual temperatures for all buildings for each date. Drag **Date** and **Building ID** to **Columns** and **Actual Temp**/**Target Temp** to **Rows**. Under **Marks**, select **Area** to use an area map for Spark data visualization.

     ![Add fields for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Add fields for Spark data visualization")
7. By default, the temperature fields are shown as aggregate. If you want to show the average temperatures instead, you can do so from the drop-down, as shown in the following screenshot:

    ![Take average of temperature for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Take average of temperature for Spark data visualization")

8. You can also super-impose one temperature map over the other to get a better feel of difference between target and actual temperatures. Move the mouse to the corner of the lower area map until you see the handle shape highlighted in a red circle. Drag the map to the other map on the top and release the mouse when you see the shape highlighted in red rectangle.

    ![Merge maps for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge maps for Spark data visualization")

     Your data visualization should change as shown in the screenshot:

    ![Tableau output for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau output for Spark data visualization")
9. Click **Save** to save the worksheet. You can create dashboards and add one or more sheets to it.
-->

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

- Visualisieren von Apache Spark-Daten mithilfe von Power BI

Fahren Sie mit dem nächsten Artikel fort, um festzustellen, wie die Daten, die Sie in Spark registriert haben, in ein BI-Analyse-Tool wie Power BI gezogen werden können. 
> [!div class="nextstepaction"]
> [Tutorial: Verarbeiten von Tweets mit Azure Event Hubs und Spark in HDInsight](apache-spark-eventhub-streaming.md)

