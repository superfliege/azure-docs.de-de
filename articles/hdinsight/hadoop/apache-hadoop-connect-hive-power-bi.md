---
title: Visualisieren von Big Data mit Power BI in Azure HDInsight
description: Erfahren Sie, wie Sie Microsoft Power BI verwenden, um von Azure HDInsight verarbeitete Hive-Daten zu visualisieren.
keywords: hdinsight, hadoop, hive, interactive query, interactive hive, LLAP, odbc
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: d3459a9905719be8849b9f6bb79f500121ca8d8d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597319"
---
# <a name="visualize-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Visualisieren von Hive-Daten mit Microsoft Power BI mithilfe von ODBC in Azure HDInsight

Erfahren Sie, wie Sie Microsoft Power BI mithilfe von ODBC mit Azure HDInsight verbinden und die Hive-Daten visualisieren. 

>[!IMPORTANT]
> Sie können den Hive-ODBC-Treiber verwenden, um Importe über den generischen ODBC-Connector in Power BI Desktop durchzuführen. Aufgrund der fehlenden Interaktivität der Hive-Abfrage-Engine wird die Verwendung für BI-Workloads jedoch nicht empfohlen. Aus Leistungsgründen sind der [interaktive HDInsight-Abfrageconnector](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) und der [HDInsight Spark-Connector](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) eine bessere Wahl.

In diesem Tutorial laden Sie die Daten aus einer „hivesampletable“-Hive-Tabelle in Power BI. Die Hive-Tabelle enthält einige Mobiltelefon-Nutzungsdaten. Anschließend zeichnen Sie die Nutzungsdaten auf eine Weltkarte:

![HDInsight Power BI Kartenbericht](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

Die Informationen gelten auch für den neuen Clustertyp [Interactive Query](../interactive-query/apache-interactive-query-get-started.md). Weitere Informationen zum Herstellen einer Verbindung mit HDInsight Interactive Query mithilfe von direkten Abfragen finden Sie unter [Visualisieren von Interactive Query-Hive-Daten mit Power BI mithilfe direkter Abfragen in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).



## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie diesen Artikel durchgehen können, benötigen Sie Folgendes:

* **HDInsight-Cluster**. Bei dem Cluster kann es sich entweder um einen HDInsight-Cluster mit Hive oder um einen neu veröffentlichten Interactive Query-Cluster handeln. Informationen zum Erstellen von Clustern finden Sie unter [Cluster erstellen](apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Sie können eine Kopie aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331) herunterladen.

## <a name="create-hive-odbc-data-source"></a>Erstellen einer Hive ODBC-Datenquelle

Informationen finden Sie unter [Erstellen einer Hive ODBC-Datenquelle](apache-hadoop-connect-excel-hive-odbc-driver.md#create-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Laden von Daten aus HDInsight

Die Hive-Tabelle „hivesampletable“ ist in allen HDInsight-Clustern enthalten.

1. Melden Sie sich bei Power BI Desktop an.
2. Klicken Sie auf die Registerkarte **Start**, klicken Sie im Menüband **Externe Daten** auf **Daten abrufen**, und wählen Sie dann **Mehr** aus.

    ![HDInsight Power BI Daten öffnen](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)
3. Klicken Sie im Bereich **Daten abrufen** links auf **Other** (Sonstige), klicken Sie rechts auf **ODBC**, und klicken Sie dann unten auf **Verbinden**.
4. Wählen Sie im Bereich **Aus ODBC** den im letzten Abschnitt erstellten Datenquellennamen aus, und klicken Sie dann auf **OK**.
5. Erweitern Sie im Bereich **Navigator** **ODBC > HIVE > Standard**, wählen Sie **hivesampletable** aus, und klicken Sie auf **Laden**.

## <a name="visualize-data"></a>Visualisieren von Daten

Fahren Sie ab der vorherigen Prozedur fort.

1. Wählen Sie im Bereich „Visualisierungen“ **Karte** aus.  Dabei handelt es sich um ein Globussymbol.

    ![HDInsight Power BI passt Bericht an](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Wählen Sie im Bereich „Felder“ **Land** und **devicemake** aus. Ihnen werden die auf die Karte gezeichneten Daten angezeigt.
3. Erweitern Sie die Karte.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Daten aus HDInsight mithilfe von Power BI visualisieren.  Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verwenden von Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md)
* [Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Verbinden von Excel mit Hadoop mithilfe von Power Query](apache-hadoop-connect-excel-power-query.md).
* [Connect to Azure HDInsight and run Hive queries using Data Lake Tools for Visual Studio (Verbinden mit Azure HDInsight und Ausführen von Hive-Abfragen mithilfe von Data Lake-Tools für Visual Studio)](apache-hadoop-visual-studio-tools-get-started.md).
* [Verwenden von Azure HDInsight-Tools für Visual Studio Code](../hdinsight-for-vscode.md)
* [Upload data to HDInsight (Hochladen von Daten in HDInsight)](./../hdinsight-upload-data.md).
