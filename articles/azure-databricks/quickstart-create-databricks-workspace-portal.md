---
title: 'Schnellstart: Ausführen eines Spark-Auftrags in Azure Databricks mit dem Azure-Portal | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal einen Azure Databricks-Arbeitsbereich und einen Apache Spark-Cluster erstellen und einen Spark-Auftrag ausführen.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/09/2018
ms.author: nitinme
ms.custom: mvc
ms.openlocfilehash: 6a0e69e1ab2c52586b447d8a249b52b09bbff705
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Schnellstart: Ausführen eines Spark-Auftrags in Azure Databricks mit dem Azure-Portal

In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Azure Databricks-Arbeitsbereich und in diesem Arbeitsbereich einen Apache Spark-Cluster erstellen. Außerdem lernen Sie, wie Sie einen Spark-Auftrag für den Databricks-Cluster ausführen. Weitere Informationen zu Azure Databricks finden Sie unter [Was ist Azure Databricks?](what-is-azure-databricks.md)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-databricks-workspace"></a>Erstellen eines Databricks-Arbeitsbereichs

In diesem Abschnitt erstellen Sie einen Azure Databricks-Arbeitsbereich über das Azure-Portal. 

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**, und klicken Sie anschließend auf **Daten + Analysen** > **Azure Databricks (Vorschauversion)**. 

    ![Databricks im Azure-Portal](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Databricks im Azure-Portal")

2. Klicken Sie unter **Azure Databricks (Vorschauversion)** auf **Erstellen**.

3. Geben Sie für den **Azure Databricks-Dienst** folgende Werte an:

    ![Erstellen eines Azure Databricks-Arbeitsbereichs](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "Erstellen eines Azure Databricks-Arbeitsbereichs")

    * Geben Sie unter **Arbeitsbereichsname** einen Namen für Ihren Databricks-Arbeitsbereich an.
    * Wählen Sie in der Dropdownliste unter **Abonnement** Ihr Azure-Abonnement aus.
    * Geben Sie für **Ressourcengruppe** an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene verwenden möchten. Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Weitere Informationen finden Sie in der [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
    * Wählen Sie unter **Standort** die Option **USA, Osten 2** aus. Informationen zu weiteren verfügbaren Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).
    * Wählen Sie für **Tarif** die Option **Standard** oder **Premium** aus. Weitere Informationen zu diesen Tarifen, finden Sie unter [Azure Databricks – Preise](https://azure.microsoft.com/pricing/details/databricks/).

    Klicken Sie auf **Create**.

4. Die Kontoerstellung dauert einige Minuten. Während der Erstellung des Kontos wird im Portal auf der rechten Seite die Kachel **Submitting deployment for Azure Databricks** (Bereitstellung für Azure Databricks wird übermittelt) angezeigt. Möglicherweise müssen Sie im Dashboard nach rechts scrollen, um die Kachel zu sehen. Am oberen Bildschirmrand wird auch eine Statusanzeige angezeigt. Sie können den Status beider Bereiche beobachten.

    ![Kachel zur Bereitstellung von Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-deployment-tile.png "Kachel zur Bereitstellung von Databricks")

## <a name="create-a-spark-cluster-in-databricks"></a>Erstellen eines Spark-Clusters in Databricks

1. Navigieren Sie im Azure-Portal zum erstellten Databricks-Arbeitsbereich, und klicken Sie auf **Launch Workspace** (Arbeitsbereich starten).

2. Sie werden zum Azure Databricks-Portal weitergeleitet. Klicken Sie im Portal auf **Cluster**.

    ![Databricks in Azure](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Databricks in Azure")

3. Geben Sie auf der Seite **Neuer Cluster** die erforderlichen Werte an, um einen Cluster zu erstellen.

    ![Erstellen eines Databricks-Spark-Clusters in Azure](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "Erstellen eines Databricks-Spark-Clusters in Azure")

    * Geben Sie einen Namen für den Cluster ein.
    * Erstellen Sie im Rahmen dieses Artikels einen Cluster mit der Runtime **4.0**. 
    * Aktivieren Sie das Kontrollkästchen **Terminate after ____ minutes of inactivity** (Nach ___ Minuten Inaktivität beenden). Geben Sie an, nach wie vielen Minuten der Cluster beendet werden soll, wenn er nicht verwendet wird.
    * Behalten Sie alle anderen Standardwerte bei. 
    * Klicken Sie auf **Cluster erstellen**. Sobald der Cluster ausgeführt wird, können Sie Notizbücher an den Cluster anfügen und Spark-Aufträge ausführen.

Weitere Informationen zum Erstellen von Clustern in Azure Databricks finden Sie unter [Creating Clusters](https://docs.azuredatabricks.net/user-guide/clusters/create.html) (Erstellen von Clustern).

## <a name="run-a-spark-sql-job"></a>Ausführen eines Spark SQL-Auftrags

Bevor Sie mit diesem Abschnitt beginnen, müssen folgende Schritte ausgeführt werden:

* [Erstellen Sie ein Azure-Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account). 
* Laden Sie eine JSON-Beispieldatei von [GitHub](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) herunter. 
* Laden Sie die JSON-Beispieldatei in das Azure-Speicherkonto hoch, das Sie erstellt haben. Dateien können mit dem [Microsoft Azure Storage-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) hochgeladen werden.

Gehen Sie wie folgt vor, um ein Notizbuch in Databricks zu erstellen, das Notizbuch zum Lesen von Daten aus einem Azure Blob Storage-Konto zu konfigurieren und anschließend einen Spark SQL-Auftrag für die Daten auszuführen.

1. Klicken Sie im linken Bereich auf **Arbeitsbereich**. Klicken Sie im Dropdownmenü **Arbeitsbereich** auf **Erstellen** > **Notizbuch**.

    ![Erstellen eines Notizbuchs in Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "Erstellen eines Notizbuchs in Databricks")

2. Geben Sie im Dialogfeld **Notizbuch erstellen** einen Namen ein, wählen Sie **Scala** als Sprache aus, und wählen Sie den zuvor erstellten Spark-Cluster aus.

    ![Erstellen eines Notizbuchs in Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "Erstellen eines Notizbuchs in Databricks")

    Klicken Sie auf **Create**.

3. In diesem Schritt ordnen Sie das Azure Storage-Konto dem Databricks-Spark-Cluster zu. Dafür stehen zwei Methoden zur Verfügung: Binden Sie das Azure Storage-Konto in das Databricks-Dateisystem (Databricks Filesystem, DBFS) ein, oder greifen Sie direkt über die erstellte Anwendung auf das Azure Storage-Konto zu.  

    > [!IMPORTANT]
    >In diesem Artikel wird der **Ansatz zum Einbinden des Speichers mit DBFS** verwendet. Bei diesem Ansatz wird sichergestellt, dass der eingebundene Speicher dem Clusterdateisystem selbst zugewiesen wird. Daher kann jede Anwendung, die auf den Cluster zugreift, auch den zugeordneten Speicher nutzen. Der Ansatz für Direktzugriff ist auf die Anwendung beschränkt, über die Sie den Zugriff konfigurieren.
    >
    > Um die Einbindung zu verwenden, müssen Sie einen Spark-Cluster mit der Databricks-Runtimeversion **4.0** erstellen. Diese Version haben Sie auch in diesem Artikel ausgewählt.

    Ersetzen Sie im folgenden Codeausschnitt `{YOUR CONTAINER NAME}`, `{YOUR STORAGE ACCOUNT NAME}` und `{YOUR STORAGE ACCOUNT ACCESS KEY}` durch die entsprechenden Werte für Ihr Azure Storage-Konto. Fügen Sie den Codeausschnitt in eine leere Zelle des Notizbuchs ein, und drücken Sie UMSCHALT+EINGABE, um die Codezelle auszuführen.

    * **Einbinden des Speicherkontos mit DBFS (empfohlen)**. In diesem Codeausschnitt wird der Pfad des Azure Storage-Konto in `/mnt/mypath` eingebunden. Daher muss zukünftig beim Zugreifen auf das Azure Storage-Konto nicht der vollständige Pfad angegeben werden. Sie können nur `/mnt/mypath` verwenden.

          dbutils.fs.mount(
            source = "wasbs://{YOUR CONTAINER NAME}@{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net/",
            mountPoint = "/mnt/mypath",
            extraConfigs = Map("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net" -> "{YOUR STORAGE ACCOUNT ACCESS KEY}"))

    * **Direktes Zugreifen auf das Speicherkonto**

          spark.conf.set("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net", "{YOUR STORAGE ACCOUNT ACCESS KEY}")

    Eine Anleitung zum Abrufen des Speicherkontoschlüssels finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

    > [!NOTE]
    > Sie können auch Azure Data Lake Store mit einem Spark-Cluster in Azure Databricks verwenden. Eine entsprechende Anleitung finden Sie unter [Azure Data Lake Store](https://go.microsoft.com/fwlink/?linkid=864084).

4. Führen Sie eine SQL-Anweisung aus, um eine temporäre Tabelle mit Daten aus der JSON-Beispieldatendatei **small_radio_json.json** zu erstellen. Ersetzen Sie die Platzhalterwerte im folgenden Codeausschnitt durch den Namen Ihres Containers bzw. Ihres Speicherkontos. Fügen Sie den Codeausschnitt in eine Codezelle des Notizbuchs ein, und drücken Sie UMSCHALT+EINGABE. In dem Codeausschnitt gibt `path` den Speicherort der JSON-Datei an, die Sie in Ihr Azure-Speicherkonto hochgeladen haben.

    ```sql
    %sql 
    DROP TABLE IF EXISTS radio_sample_data
    CREATE TABLE radio_sample_data
    USING json
    OPTIONS (
     path "/mnt/mypath/small_radio_json.json"
    )
    ```

    Nach erfolgreicher Ausführung des Befehls liegen alle Daten aus der JSON-Datei als Tabelle im Databricks-Cluster vor.

    Der Befehl `%sql` ermöglicht die Ausführung von SQL-Code über das Notizbuch, auch wenn es sich um ein Notizbuch eines anderen Typs handelt. Weitere Informationen finden Sie unter [Mixing languages in a notebook](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook) (Mischen von Sprachen in einem Notizbuch).

5. Zum besseren Verständnis der ausgeführten Abfrage sehen wir uns eine Momentaufnahme der JSON-Beispieldaten an. Fügen Sie den folgenden Codeausschnitt in die Codezelle ein, und drücken Sie **UMSCHALT+EINGABE**.

    ```sql
    %sql 
    SELECT * from radio_sample_data
    ```

6. Sie erhalten eine tabellarische Ausgabe wie im folgenden Screenshot, der allerdings nur einen Teil der Spalten zeigt:

    ![JSON-Beispieldaten](./media/quickstart-create-databricks-workspace-portal/databricks-sample-csv-data.png "JSON-Beispieldaten")

    Neben verschiedenen anderen Details enthalten die Beispieldaten das Geschlecht der Zielgruppe eines Radiokanals (Spalte **gender**) und die Information, ob die jeweilige Person ein kostenloses oder ein kostenpflichtiges Abonnement nutzt (Spalte **level**).

7. Sie können nun eine visuelle Darstellung dieser Daten erstellen, die zeigt, wie viele Benutzer des jeweiligen Geschlechts über kostenlose Konten verfügen und bei wie vielen es sich um zahlende Abonnenten handelt. Klicken Sie im unteren Bereich der tabellarischen Ausgabe auf das Symbol **Balkendiagramm** und anschließend auf **Zeichnungsoptionen**.

    ![Erstellen eines Balkendiagramms](./media/quickstart-create-databricks-workspace-portal/create-plots-databricks-notebook.png "Erstellen eines Balkendiagramms")

8. Platzieren Sie per Drag & Drop Werte in **Customize Plot** (Zeichnung anpassen), wie im folgenden Screenshot zu sehen.

    ![Anpassen des Balkendiagramms](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-customize-plot.png "Anpassen des Balkendiagramms")

    * Legen Sie **Schlüssel** auf **gender** fest.
    * Legen Sie **Series groupings** (Reihengruppierungen) auf **level** fest.
    * Legen Sie **Werte** auf **level** fest.
    * Legen Sie **Aggregation** auf **ANZAHL** fest.

    Klicken Sie auf **Anwenden**.

9. Die Ausgabe zeigt die visuelle Darstellung wie im folgenden Screenshot dargestellt:

     ![Anpassen des Balkendiagramms](./media/quickstart-create-databricks-workspace-portal/databricks-sql-query-output-bar-chart.png "Anpassen des Balkendiagramms")

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie beim Erstellen des Spark-Clusters das Kontrollkästchen **Terminate after __ minutes of inactivity** (Nach __ Minuten Inaktivität beenden) aktiviert haben, wird der Cluster automatisch beendet, wenn er für den angegebenen Zeitraum inaktiv war.

Andernfalls muss der Cluster manuell beendet werden. Klicken Sie hierzu im linken Bereich des Azure Databricks-Arbeitsbereichs auf **Cluster**. Bewegen Sie zum Beenden des Clusters den Cursor unter der Spalte **Aktionen** auf die Auslassungspunkte, und klicken Sie auf das Symbol **Beenden**.

![Beenden des Databricks-Clusters](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "Beenden des Databricks-Clusters")

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einen Spark-Cluster in Azure Databricks erstellt und einen Spark-Auftrag mit Daten in Azure Storage ausgeführt. Unter [Spark Data Sources](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) (Spark-Datenquellen) erfahren Sie, wie Sie Daten aus anderen Datenquellen in Azure Databricks importieren. Im nächsten Artikel erfahren Sie, wie Sie Azure Data Lake Store mit Azure Databricks verwenden.

> [!div class="nextstepaction"]
>[Azure Data Lake Store](https://go.microsoft.com/fwlink/?linkid=864084)
