---
title: Verwenden von Hive mit Hadoop für Analysen von Websiteprotokollen – Azure HDInsight
description: Erfahren Sie, wie Sie Website-Protokolle mit Hive in HDInsight analysieren können. Sie verwenden eine Protokolldatei als Eingabe für eine HDInsight-Tabelle und verwenden anschließend HiveQL, um die Daten abzufragen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/17/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: c0a15bacd3aaf97a3caa54ee8bf70a9c4cf54663
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51233595"
---
# <a name="use-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>Verwenden von Hive in Windows-basierten HDInsight-Clustern zum Analysieren von Website-Protokollen
Erfahren Sie, wie Sie Website-Protokolle mit HiveQL in HDInsight analysieren können. Mit der Analyse von Websiteprotokollen können Sie Ihr Publikum nach ähnlichen Aktivitäten gruppieren, Seitenbesucher nach demografischen Daten kategorisieren, die angezeigten Inhalte und die Ursprungsseiten ermitteln und vieles mehr.

> [!IMPORTANT]
> Die Schritte in diesem Dokument funktionieren nur mit einem Windows-basierten HDInsight-Cluster. HDInsight ist unter Windows nur für HDInsight-Versionen vor 3.4 verfügbar. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

In diesem Beispiel analysieren Sie Websiteprotokolle in einem HDInsight-Cluster, um Einblicke in die Häufigkeit von Besuchen der Website von externen Websites pro Tag zu erhalten. Sie generieren zudem eine Zusammenfassung der Websitefehler, die bei den Benutzern auftreten. Folgendes wird vermittelt:

* Herstellen einer Verbindung mit einer Azure Blob Storage-Instanz, die Protokolldateien von Websites enthält
* Erstellen von HIVE-Tabellen zum Abfragen dieser Protokolle
* Erstellen von HIVE-Abfragen zum Analysieren der Daten
* Verwenden von Microsoft Excel zum Herstellen einer Verbindung mit HDInsight (über eine offene Datenbankverbindung (ODBC)), um die analysierten Daten abzurufen

![HDI.Samples.Website.Log.Analysis](./media/apache-hive-analyze-website-log/hdinsight-weblogs-sample.png)

## <a name="prerequisites"></a>Voraussetzungen
* Sie müssen einen Hadoop-Cluster unter Azure HDInsight bereitgestellt haben. Anweisungen finden Sie unter [Bereitstellen eines HDInsight-Clusters](../hdinsight-hadoop-provision-linux-clusters.md).
* Microsoft Excel 2013 oder Excel 2010 muss installiert sein.
* Sie benötigen den [Microsoft Hive ODBC-Treiber](https://www.microsoft.com/download/details.aspx?id=40886) , um die Daten aus Hive in Excel zu importieren.

## <a name="to-run-the-sample"></a>Ausführen des Beispiels
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) im Startmenü (sofern Sie den Cluster dort angeheftet haben) auf die Kachel des Clusters, für den Sie das Beispiel ausführen möchten.
2. Klicken Sie auf dem Blatt für den Cluster unter **Quicklinks** auf **Clusterdashboard** und auf dem Blatt **Clusterdashboard** auf **HDInsight-Cluster-Dashboard**. Alternativ können Sie das Dashboard direkt über die folgende URL öffnen:

         https://<clustername>.azurehdinsight.net

    Geben Sie den Benutzernamen und das Kennwort des Administrators ein, den Sie bei der Clusterbereitstellung verwendet haben, wenn Sie dazu aufgefordert werden.
3. Klicken Sie auf der Webseite, die sich daraufhin öffnet, auf die Registerkarte **Getting Started Gallery** (Erste Schritte mit dem Katalog) und anschließend in der Kategorie **Solutions with Sample Data** (Lösungen mit Beispieldaten) auf das Beispiel für die **Websiteprotokollanalyse**.
4. Folgen Sie den Anweisungen auf der Webseite, um das Beispiel abzuschließen.

## <a name="next-steps"></a>Nächste Schritte
Versuchen Sie es mit dem folgenden Beispiel: [Analysieren von Sensordaten mit Hive mit HDInsight](apache-hive-analyze-sensor-data.md).

[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md
