---
title: "Installieren einer veröffentlichten Anwendung – StreamSets Data Collector – Azure HDInsight | Microsoft-Dokumentation"
description: Installieren und verwenden Sie die Hadoop-Drittanbieteranwendung StreamSets Data Collector.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 90775452c58457ae8ecc73687a375606474158f5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="install-published-application---streamsets-data-collector"></a>Installieren einer veröffentlichten Anwendung – StreamSets Data Collector

In diesem Artikel erfahren Sie, wie Sie die veröffentlichte Hadoop-Anwendung [StreamSets Data Collector for HDInsight](https://streamsets.com/) in Azure HDInsight installieren und ausführen. Eine Übersicht über die HDInsight-Anwendungsplattform und eine Liste mit den verfügbaren veröffentlichten Anwendungen unabhängiger Softwarehersteller (Independent Software Vendors, ISVs) finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern](hdinsight-apps-install-applications.md). Eine Anleitung zur Installation Ihrer eigenen Anwendung finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).

## <a name="about-streamsets-data-collector"></a>Informationen zu StreamSets Data Collector

StreamSets Data Collector wird zusätzlich zur Azure HDInsight-Anwendung bereitgestellt. StreamSets Data Collector bietet eine umfassende integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) zum Entwerfen, Testen, Bereitstellen und Verwalten von n:n-Erfassungspipelines. Diese Pipelines eignen sich sowohl für Datenstrom- als auch für Batchdaten und enthalten eine Reihe von datenstrominternen Transformationen, ohne dass Sie dazu benutzerdefinierten Code schreiben müssen.

Mit StreamSets Data Collector können Sie Datenflüsse mit zahlreichen Big Data-Komponenten wie HDFS, Kafka, Solr, Hive, HBase und Kudu erstellen. Sobald StreamSets Data Collector auf einem Edgeserver oder in Ihrem Hadoop-Cluster ausgeführt wird, können Sie sowohl Datenanomalien als auch Datenflussvorgänge in Echtzeit überwachen. Diese Überwachung umfasst schwellenwertbasierte Warnungen, eine Anomalienerkennung sowie eine automatische Korrektur von Fehlerdatensätzen.

StreamSets Data Collector ist dafür konzipiert, jede Phase einer Pipeline logisch zu isolieren, sodass Sie neue geschäftliche Anforderungen durch Implementierung neuer Prozessoren und Connectors ohne Programmieraufwand und mit minimaler Downtime erfüllen können.

### <a name="streamsets-resource-links"></a>Ressourcenlinks für StreamSets

* [Dokumentation](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html)
* [Blog](https://streamsets.com/blog/)
* [Lernprogramme](https://github.com/streamsets/tutorials)
* [Supportforum für Entwickler](https://groups.google.com/a/streamsets.com/forum/#!forum/sdc-user)
* [Slack: Öffentlicher StreamSets-Kanal](https://streamsetters.slack.com/)
* [Quellcode](https://github.com/streamsets)

## <a name="prerequisites"></a>Voraussetzungen

Um diese App in einem neuen HDInsight-Cluster oder in einem vorhandenen Cluster zu installieren, müssen Sie über die folgende Konfiguration verfügen:

* Clustertarif: Standard oder Premium
* Clusterversion: ab 3.5

## <a name="install-the-streamsets-data-collector-published-application"></a>Installieren der veröffentlichten Anwendung StreamSets Data Collector

Eine ausführliche Installationsanleitung für diese und andere verfügbare ISV-Anwendungen finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](hdinsight-apps-install-applications.md).

## <a name="launch-streamsets-data-collector"></a>Starten von StreamSets Data Collector

1. Nach der Installation können Sie StreamSets über Ihren Cluster im Azure-Portal starten. Navigieren Sie hierzu zum Bereich **Einstellungen**, und klicken Sie unter der Kategorie **Allgemein** auf **Anwendungen**. Im Bereich „Installierte Apps“ werden die installierten Anwendungen aufgeführt.

    ![Installierte StreamSets-App](./media/hdinsight-apps-install-streamsets/streamsets.png)

2. Wenn Sie auf „StreamSets Data Collector“ klicken, werden ein Link zur Webseite und der SSH-Endpunktpfad angezeigt. Klicken Sie auf den Link zur WEBSEITE.

3. Melden Sie sich im Anmeldedialogfeld mit folgenden Anmeldeinformationen an: `admin` und `admin`.

4. Klicken Sie auf der Seite mit den ersten Schritten auf **Create New Pipeline** (Neue Pipeline erstellen).

    ![Erstellen einer neuen Pipeline](./media/hdinsight-apps-install-streamsets/get-started.png)

5. Geben Sie im Fenster für die neue Pipeline einen Namen („Hello World“) und ggf. eine Beschreibung ein, und klicken Sie anschließend auf **Speichern**.

6. Die Data Collector-Konsole wird geöffnet. Im Eigenschaftenbereich werden Eigenschaften der Pipeline angezeigt.
 
    ![Data Collector-Konsole](./media/hdinsight-apps-install-streamsets/pipeline-canvas.png)

7. Nun sind Sie bereit für das [StreamSets-Tutorial](https://streamsets.com/documentation/datacollector/latest/help/#Tutorial/Tutorial-title.html). Das Tutorial enthält eine ausführliche Anleitung zur Erstellung Ihrer ersten Pipeline.

## <a name="next-steps"></a>Nächste Schritte

* [StreamSets Data Collector-Dokumentation](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html#concept_htw_ghg_jq)
* [Installieren von benutzerdefinierten Hadoop-Anwendungen in Azure HDInsight:](hdinsight-apps-install-custom-applications.md) Hier erfahren Sie, wie Sie eine nicht veröffentlichte HDInsight-Anwendung in HDInsight bereitstellen.
* [Veröffentlichen von HDInsight-Anwendungen](hdinsight-apps-publish-applications.md): Hier erfahren Sie, wie Sie benutzerdefinierte HDInsight-Anwendungen im Azure Marketplace veröffentlichen.
* [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx)(MSDN: Installieren einer HDInsight-Anwendung): Hier erfahren Sie, wie Sie HDInsight-Anwendungen definieren.
* [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen:](hdinsight-hadoop-customize-cluster-linux.md) Hier erfahren Sie, wie Sie mithilfe einer Skriptaktion zusätzliche Anwendungen installieren.
* [Verwenden leerer Edgeknoten in Hadoop-Clustern in HDInsight:](hdinsight-apps-use-edge-node.md) Hier erfahren Sie, wie Sie einen leeren Edgeknoten zum Zugreifen auf HDInsight-Cluster und zum Testen und Hosten von HDInsight-Anwendungen verwenden.
