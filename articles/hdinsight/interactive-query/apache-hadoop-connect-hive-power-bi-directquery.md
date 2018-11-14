---
title: Visualisieren von Interactive Query-Hive-Daten mit Power BI in Azure HDInsight
description: Erfahren Sie, wie Sie Microsoft Power BI verwenden, um Interactive Query-Hive-Daten von Azure HDInsight zu visualisieren.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 46b5b3436a33c3f83d85cfb02028759d53d214af
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245272"
---
# <a name="visualize-interactive-query-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Visualisieren von Interactive Query-Hive-Daten mit Power BI mithilfe direkter Abfragen in Azure HDInsight

In diesem Artikel wird beschrieben, wie Sie Microsoft Power BI mithilfe von direkten Abfragen mit Azure HDInsight Interactive Query-Clustern verbinden und die Apache Hive-Daten visualisieren. In dem angegebenen Beispiel werden Daten aus einer Hive-Tabelle namens „hivesampletable“ in Power BI geladen. Die Hive-Tabelle „hivesampletable“ enthält einige Mobiltelefon-Nutzungsdaten. Anschließend zeichnen Sie die Nutzungsdaten auf eine Weltkarte:

![HDInsight Power BI Kartenbericht](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Sie können den [Hive-ODBC-Treiber](../hadoop/apache-hadoop-connect-hive-power-bi.md) verwenden, um Importe über den generischen ODBC-Connector in Power BI Desktop durchzuführen. Aufgrund der fehlenden Interaktivität der Hive-Abfrage-Engine wird die Verwendung für BI-Workloads jedoch nicht empfohlen. Aus Leistungsgründen sind der [interaktive HDInsight-Abfrageconnector](./apache-hadoop-connect-hive-power-bi-directquery.md) und der [HDInsight Spark-Connector](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) eine bessere Wahl.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie diesen Artikel durchgehen können, benötigen Sie Folgendes:

* **HDInsight-Cluster**. Bei dem Cluster kann es sich entweder um einen HDInsight-Cluster mit Hive oder um einen neu veröffentlichten Interactive Query-Cluster handeln. Informationen zum Erstellen von Clustern finden Sie unter [Cluster erstellen](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Sie können eine Kopie aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331) herunterladen.

## <a name="load-data-from-hdinsight"></a>Laden von Daten aus HDInsight

Die Hive-Tabelle „hivesampletable“ ist in allen HDInsight-Clustern enthalten.

1. Melden Sie sich bei Power BI Desktop an.

2. Klicken Sie auf die Registerkarte **Start**, im Menüband **Externe Daten** auf **Daten abrufen** und dann auf **Mehr...**.

    ![HDInsight Power BI Daten öffnen](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
    
3. Geben Sie im Bereich **Daten abrufen** in das Suchfeld **hdinsight** ein. Wenn **HDInsight Interactive Query (Beta)** nicht angezeigt wird, müssen Sie Power BI Desktop auf die neueste Version aktualisieren.

4. Klicken Sie auf **HDInsight Interactive Query (Beta)** und dann auf **Verbinden**.

5. Klicken Sie auf **Weiter**, um das Warnungsdialogfeld **Connector (Vorschau)** zu schließen.

6. Wählen Sie in **HDInsight Interactive Query** die folgenden Informationen aus, oder geben Sie sie ein:

    - **Server:** Geben Sie den Namen des Interactive Query-Clusters ein, z.B. *myiqcluster.azurehdinsight.net*.

    - **Datenbank:** Geben Sie für dieses Tutorial **Standard** ein.
    
    - **Datenverbindungsmodus:** Wählen Sie für dieses Tutorial **DirectQuery** aus.

    ![HDInsight Interactive Query – Verbindung mit Power BI DirectQuery](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. Klicken Sie auf **OK**.

8. Geben Sie die Anmeldeinformationen des HTTP-Benutzers ein, und klicken Sie dann auf **OK**. Der Standardbenutzername lautet **admin**.

9. Wählen Sie im linken Bereich **hivesampletable** aus, und klicken Sie dann auf **Laden**.

    ![HDInsight Interactive Query – „hivesampletable“ in Power BI](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>Visualisieren von Daten auf einer Karte

Fahren Sie ab der vorherigen Prozedur fort.

1. Wählen Sie im Bereich „Visualisierungen“ **Karte** aus.  Dabei handelt es sich um ein Globussymbol.

    ![HDInsight Power BI passt Bericht an](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
    
2. Wählen Sie im Bereich „Felder“ **Land** und **devicemake** aus. Ihnen werden die auf die Karte gezeichneten Daten angezeigt.

3. Erweitern Sie die Karte.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Daten aus HDInsight mithilfe von Power BI visualisieren.  Weitere Informationen zur Datenvisualisierung finden Sie in den folgenden Artikeln:

* [Visualisieren von Hive-Daten mit Microsoft Power BI mithilfe von ODBC in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md) 
* [Verwenden von Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md)
* [Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Verbinden von Excel mit Hadoop mithilfe von Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Connect to Azure HDInsight and run Hive queries using Data Lake Tools for Visual Studio (Verbinden mit Azure HDInsight und Ausführen von Hive-Abfragen mithilfe von Data Lake-Tools für Visual Studio)](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Verwenden von Azure HDInsight-Tools für Visual Studio Code](../hdinsight-for-vscode.md)
* [Upload data to HDInsight (Hochladen von Daten in HDInsight)](./../hdinsight-upload-data.md).
