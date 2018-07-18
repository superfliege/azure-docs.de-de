---
title: Verwenden von Hive mit Hadoop für Analysen von Websiteprotokollen – Azure HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Website-Protokolle mit Hive in HDInsight analysieren können. Sie verwenden eine Protokolldatei als Eingabe für eine HDInsight-Tabelle und verwenden anschließend HiveQL, um die Daten abzufragen.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 53a0560d3bc5a52069d5829b9c3bd353e0c37ef3
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31398156"
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
* Sie benötigen den [Microsoft Hive ODBC-Treiber](http://www.microsoft.com/download/details.aspx?id=40886) , um die Daten aus Hive in Excel zu importieren.

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
