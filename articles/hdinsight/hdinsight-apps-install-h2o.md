---
title: Installieren einer veröffentlichten Anwendung – H2O Sparkling Water – Azure HDInsight | Microsoft-Dokumentation
description: Installieren und verwenden Sie die Hadoop-Drittanbieteranwendung H2O Sparkling Water.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: e3c80fe824d87c15a710b133c8e6cddf4ee0e096
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856554"
---
# <a name="install-published-application---h2o-sparkling-water"></a>Installieren einer veröffentlichten Anwendung – H2O Sparkling Water

In diesem Artikel erfahren Sie, wie Sie die veröffentlichte Hadoop-Anwendung [H20 Sparkling Water](http://www.h2o.ai/) in Azure HDInsight installieren und ausführen. Eine Übersicht der HDInsight-Anwendungsplattform und eine Liste mit den verfügbaren veröffentlichten Anwendungen unabhängiger Softwarehersteller (Independent Software Vendors, ISVs) finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern](hdinsight-apps-install-applications.md). Eine Anleitung zur Installation Ihrer eigenen Anwendung finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).

## <a name="about-h2o-sparkling-water"></a>Grundlegendes zu H2O Sparkling Water

H2O Sparkling Water ist eine Open Source-basierte, vollständig verteilte In-Memory-Machine Learning-Plattform mit linearer Skalierbarkeit. Mit H2O Sparkling Water können Sie die schnellen, skalierbaren Machine Learning-Algorithmen von H2O mit den Funktionen von Spark kombinieren. Mit Sparkling Water könne Benutzer Berechnungen aus Scala, R und Python mithilfe der H2O Flow-Benutzeroberfläche durchführen.

H2O Sparkling Water bietet Folgendes:

* **Einfach zu bedienende Weboberfläche und vertraute Schnittstellen**: Die intuitive, webbasierte Flow-Benutzeroberfläche von H2O oder Programmierumgebungen wie z.B. R, Python, Java, Scala, JSON und die H2O-APIs ermöglichen eine schnelle Einrichtung für einen sofortigen Einstieg.
* **Datenunabhängige Unterstützung für alle gängigen Datenbank- und Dateitypen**: Erkunden und modellieren Sie auf einfache Weise Big Data aus Microsoft Excel, R Studio, Tableau u.v.a. Stellen Sie aus HDFS, S3, SQL und NoSQL-Datenquellen eine Verbindung mit Ihren Daten her.
* **Hochgradig skalierbare Big Data-Bereinigung und -Analyse**: H2O Big Joins sind 7-mal schneller als data.table-Operationen in R, und es kann eine lineare Skalierung auf bis zu 10 Milliarden × 10 Milliarden Zeilenjoins durchgeführt werden.
* **Datenbewertung in Echtzeit**: Stellen Sie unter Verwendung von einfachen Java-Objekten (Plain-Old Java Objects, POJO), modelloptimierten Java-Objekten (MOJO) oder der H2O-REST-API Modelle umgehend in der Produktion bereit.

### <a name="resource-links"></a>Ressourcenlinks

* [H2O.ai-Entwicklungsroadmap](http://jira.h2o.ai/)
* [H2O.ai-Startseite](http://www.h2o.ai/)
* [H2O.ai-Dokumentation](http://docs.h2o.ai/)
* [H2O.ai-Support](https://support.h2o.ai/)
* [H2O.ai-Open Source-Codebasis](https://github.com/h2oai/)

## <a name="prerequisites"></a>Voraussetzungen

Um diese App in einem neuen HDInsight-Cluster oder in einem vorhandenen Cluster zu installieren, müssen Sie über die folgende Konfiguration verfügen:

* Clustertarif(e): Standard oder Premium
* Clustertyp: Spark
* Clusterversion(en): 3.5 oder 3.6

## <a name="install-the-h2o-sparkling-water-published-application"></a>Installieren der veröffentlichten Anwendung H2O Sparkling Water

Eine ausführliche Installationsanleitung für diese und andere verfügbare ISV-Anwendungen finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](hdinsight-apps-install-applications.md).

## <a name="launch-h2o-sparkling-water"></a>Starten von H2O Sparkling Water

1. Nach der Installation können Sie damit beginnen, H2O Sparkling Water (h2o-sparklingwater) aus Ihrem Cluster im Azure-Portal zu verwenden, indem Sie Jupyter Notebook (`https://<ClusterName>.azurehdinsight.net/jupyter`) öffnen. Sie gelangen auch zu Jupyter, indem Sie im Portal in Ihrem Clusterbereich das **Clusterdashboard** auswählen und dann auf **Jupyter Notebook** klicken. Sie werden zum Eingeben Ihrer Anmeldeinformationen aufgefordert. Geben Sie die Hadoop-Anmeldeinformationen für Ihren Cluster ein, die bei der Clustererstellung angegeben wurden.

2. In Jupyter werden drei Ordner angezeigt: „H2O-PySparkling-Examples“, „PySpark Examples“ und „Scala Examples“. Wählen Sie den Ordner **H2O-PySparkling-Examples** aus.

    ![Jupyter Notebook-Startseite](./media/hdinsight-apps-install-h2o/jupyter-home.png)

3. Der erste Schritt beim Erstellen eines neuen Notebooks besteht darin, die Spark-Umgebung zu konfigurieren. Diese Informationen sind im Beispiel **Sentiment_analysis_with_Sparkling_Water** enthalten. Stellen Sie beim Konfigurieren der Spark-Umgebung sicher, dass Sie die richtige JAR-Datei verwenden, und geben Sie die IP-Adresse aus der Ausgabe in der ersten Zelle an.

    ![Jupyter Notebook-Startseite](./media/hdinsight-apps-install-h2o/spark-config.png)

4. Starten Sie den H2O-Cluster.

    ![Cluster starten](./media/hdinsight-apps-install-h2o/start-cluster.png)

5. Nachdem der H2O-Cluster eingerichtet wurde und ausgeführt wird, öffnen Sie H2O Flow, indem Sie zu **`https://<ClusterName>-h2o.apps.azurehdinsight.net:443`** wechseln.

    > [!NOTE]
    > Wenn Sie H2O nicht öffnen können, löschen Sie Ihren Browsercache. Falls H2O weiterhin nicht geöffnet werde kann, weist Ihr Cluster möglicherweise nicht genügend Ressourcen auf. Erhöhen Sie in Ihrem Clusterbereich die Anzahl von Workerknoten unterhalb von **Scale cluster** (Cluster skalieren).

    ![H2O Flow-Dashboard](./media/hdinsight-apps-install-h2o/h2o-flow.png)

6. Wählen Sie das Beispiel **Million_Songs.flow** im Menü auf der rechten Seite aus. Wenn eine Warnung angezeigt wird, klicken Sie auf **Load Notebook** (Notebook laden). Diese Demonstration ist so konzipiert, dass sie einige Minuten mit echten Daten ausgeführt wird. Das Ziel besteht darin, mithilfe einer binären Klassifizierung vorherzusagen, ob der Titel vor oder nach 2004 veröffentlicht wurde.

    ![„Million_Songs.flow“ auswählen](./media/hdinsight-apps-install-h2o/million-songs.png)

7. Suchen Sie den Pfad, der **milsongs-cls-train.csv.gz** enthält, und ersetzen Sie den gesamten Pfad durch **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-train.csv.gz**.

8. Suchen Sie den Pfad, der **milsongs-cls-test.csv.gz** enthält, und ersetzen Sie den gesamten Pfad durch **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-test.csv.gz**.

9. Um alle Anweisungen innerhalb der Notebook-Zellen auszuführen, wählen Sie auf der Symbolleiste die Schaltfläche **Run all** (Alle ausführen) aus.

    ![Run all (Alle ausführen)](./media/hdinsight-apps-install-h2o/run-all.png)

10. Nach einigen Minuten sollte eine Ausgabe ähnlich der folgenden angezeigt werden.

    ![Output](./media/hdinsight-apps-install-h2o/output.png)

Das ist alles! Sie haben innerhalb von wenigen Minuten künstliche Intelligenz in Spark genutzt. Sie können jetzt weitere Beispiele in H2O Flow erkunden, mit denen unterschiedliche Arten von Machine Learning-Algorithmen veranschaulicht werden.

## <a name="next-steps"></a>Nächste Schritte

* [H2O-Dokumentation](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html)
* [Installieren von benutzerdefinierten Hadoop-Anwendungen in Azure HDInsight:](hdinsight-apps-install-custom-applications.md) Hier erfahren Sie, wie Sie eine nicht veröffentlichte HDInsight-Anwendung in HDInsight bereitstellen.
* [Veröffentlichen von HDInsight-Anwendungen](hdinsight-apps-publish-applications.md): Hier erfahren Sie, wie Sie benutzerdefinierte HDInsight-Anwendungen im Azure Marketplace veröffentlichen.
* [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx)(MSDN: Installieren einer HDInsight-Anwendung): Hier erfahren Sie, wie Sie HDInsight-Anwendungen definieren.
* [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md): Hier erfahren Sie, wie Sie mithilfe einer Skriptaktion zusätzliche Anwendungen installieren.
* [Verwenden leerer Edgeknoten in Hadoop-Clustern in HDInsight](hdinsight-apps-use-edge-node.md): Hier erfahren Sie, wie Sie einen leeren Edgeknoten zum Zugreifen auf HDInsight-Cluster und zum Testen und Hosten von HDInsight-Anwendungen verwenden.
