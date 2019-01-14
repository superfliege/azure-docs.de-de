---
title: Installieren einer veröffentlichten Anwendung – Datameer – Azure HDInsight
description: Installieren und verwenden Sie die Hadoop-Drittanbieteranwendung Datameer.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 715e536d7356a4e37f512027a23236b1fd37cbac
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2018
ms.locfileid: "53651298"
---
# <a name="install-published-application---cask-data-application-platform-cdap"></a>Installieren einer veröffentlichten Anwendung – Cask Data Application Platform (CDAP)

In diesem Artikel erfahren Sie, wie Sie die veröffentlichte [Apache Hadoop](https://hadoop.apache.org/)-Anwendung [CDAP](https://cask.co/products/cdap/) in Azure HDInsight installieren und ausführen. Eine Übersicht über die HDInsight-Anwendungsplattform und eine Liste mit den verfügbaren veröffentlichten Anwendungen unabhängiger Softwarehersteller (Independent Software Vendors, ISVs) finden Sie unter [Installieren von Apache Hadoop-Anwendungen von Drittanbietern](hdinsight-apps-install-applications.md). Eine Anleitung zur Installation Ihrer eigenen Anwendung finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).

## <a name="about-cdap"></a>Informationen zu CDAP

Die Anwendungsentwicklung in Hadoop kann eine Herausforderung sein.  Es gibt eine große (und wachsende) Anzahl von Hadoop-Technologieerweiterungen, deren Integration sich als recht zeitaufwendig erweisen kann. Für die Überwachung des Datenflusses und die Erfassung von Metriken muss ggf. eine separate Lösung erstellt werden.

### <a name="how-does-cdap-help"></a>Welchen Nutzen hat CDAP?

Cask Data Application Platform (CDAP) ist eine Integrationsplattform für Big Data. Mit CDAP können Sie sich auf die Anwendungserstellung konzentrieren, anstatt sich ausführlich mit der zugrunde liegenden Infrastruktur zu beschäftigen.

CDAP verwendet allgemeine Konzepte und Abstraktionen, mit denen Entwickler vertraut sind. Diese Abstraktionen blenden die Komplexität interner Systeme aus und tragen zur Wiederverwendbarkeit von Lösungen bei.

Eine CDAP-Erweiterung namens [Cask Hydrator](https://cask.co/products/hydrator/) bietet eine Benutzeroberfläche für die Entwicklung und Verwaltung von Datenpipelines. Eine Datenpipeline besteht aus verschiedenen Plug-Ins, die Aufgaben wie Datenerfassung, -transformation und -analyse sowie Vorgänge nach der Ausführung durchführen.

Jedes CDAP-Plug-In besitzt eine klar definierte Schnittstelle, sodass zur Evaluierung verschiedener Technologien lediglich ein Plug-In durch ein anderes ersetzt werden muss und keinerlei Änderungen am Rest der Anwendung erforderlich sind.

Die *Pipelines* von CDAP stellen den allgemeinen Datenfluss in Ihrer Anwendung bildlich dar. Die Visualisierung zeigt den End-to-End-Datenfluss von der Erfassung über die Datentransformationen und -analysen bis zum externen Datenspeicher.

Im folgenden Beispiel einer Datenpipeline werden Twitter-Daten in Echtzeit erfasst und anschließend einige Tweets auf der Grundlage vordefinierter Kriterien herausgefiltert. Die Datenpipeline transformiert unformatierte Tweet-Daten und projiziert sie in ein besser lesbares Format. Anschließend transformiert sie die Tweets gemäß einer Reihe von Werten und schreibt die Ergebnisse in einen HBase-Speicher.

![CDAP-Pipeline](./media/hdinsight-apps-install-cask/pipeline.png)

Diese End-to-End-Pipeline wurde über die **Cask Hydrator-Benutzeroberfläche** erstellt. Die Verbindungen zwischen den einzelnen Phasen wurden dabei mithilfe der Plug-In-Schnittstelle und der Drag&Drop-Funktion hergestellt. Die Funktionen der einzelnen Plug-Ins können unabhängig isoliert und geändert werden. Mit CDAP können Sie in wenigen Stunden ähnliche Pipelines erstellen und überprüfen. In der herkömmlichen Hadoop-Umgebung kann die Erstellung solcher Lösungen mehrere Tage dauern.

Mit der CDAP-Erweiterung [Cask Tracker](https://cask.co/products/tracker/) können Daten auf ihrem Weg durch die Anwendung visuell nachverfolgt werden. Cask Tracker erweitert das System mit *Datengovernance* für eine formale Verwaltung von Datenressourcen in der gesamten Anwendung. Sie können die Herkunft jedes Datenpunkts nachvollziehen, relevante Metriken erfassen und den Weg von Daten während des gesamten Prozesses überwachen.

Die folgende Abbildung zeigt den Datenfluss in der obigen Pipeline:

![CDAP-Tracker](./media/hdinsight-apps-install-cask/tracker.png)

## <a name="prerequisites"></a>Voraussetzungen

Um diese App in einem neuen HDInsight-Cluster oder in einem vorhandenen Cluster zu installieren, müssen Sie über die folgende Konfiguration verfügen:

* Clustertarif: Standard
* Clustertyp: hbase
* Clusterversion: 3.4, 3.5

## <a name="install-the-cdap-published-application"></a>Installieren der veröffentlichten Anwendung CDAP

Eine ausführliche Installationsanleitung für diese und andere verfügbare ISV-Anwendungen finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](hdinsight-apps-install-applications.md).

## <a name="launch-cdap"></a>Starten von CDAP

1. Starten Sie CDAP nach der Installation über Ihren Cluster im Azure-Portal. Navigieren Sie hierzu zum Bereich **Einstellungen**, und klicken Sie unter der Kategorie **Allgemein** auf **Anwendungen**. Im Bereich „Installierte Apps“ werden alle installierten Anwendungen aufgeführt.

    ![Installierte CDAP-App](./media/hdinsight-apps-install-cask/cdap-app.png)

2. Wenn Sie auf „CDAP“ klicken, werden ein Link zur Webseite und zum HTTP-Endpunkt sowie der SSH-Endpunktpfad angezeigt. Klicken Sie auf den Link zur WEBSEITE.

3. Geben Sie die Ihre Clusteradministrator-Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.

    ![Authentifizierung](./media/hdinsight-apps-install-cask/auth.png)

4. Nach der Anmeldung wird die Startseite der grafischen Benutzeroberfläche von Cask CDAP angezeigt.

    ![Startseite der grafischen Benutzeroberfläche von Cask](./media/hdinsight-apps-install-cask/gui.png)

5. Klicken Sie zum Erkunden der CDAP-Oberfläche am oberen Seitenrand auf den Menülink **Cask Market** (Cask-Markt).

    ![Link zum Cask-Markt](./media/hdinsight-apps-install-cask/cask-market.png)

6. Wählen Sie in der Liste die Option **Access Log Sample** aus.

    ![Access Log Sample](./media/hdinsight-apps-install-cask/market-log-sample.png)

7. Klicken Sie zur Bestätigung auf **Load** (Laden).

    ![Klicken auf „Load“ (Laden)](./media/hdinsight-apps-install-cask/market-load.png)

8. Eine Ansicht der enthaltenen Beispieldaten wird angezeigt. Klicken Sie auf **Next** (Weiter).

    ![Access Log Sample – Anzeigen der Daten](./media/hdinsight-apps-install-cask/market-view-data.png)

9. Wählen Sie als Zieltyp die Option **Stream** aus, geben Sie einen Zielnamen ein, und klicken Sie anschließend auf **Finish** (Fertig stellen).

    ![Access Log Sample – Auswählen des Ziels](./media/hdinsight-apps-install-cask/market-destination.png)

10. Klicken Sie nach dem Laden des Datenpakets auf **View Stream Details** (Streamdetails anzeigen).

    ![Datenpaket erfolgreich hochgeladen](./media/hdinsight-apps-install-cask/market-view-details.png)

11. Klicken Sie auf der Seite mit den Zugriffsprotokolldetails auf der Registerkarte „Usage“ (Verwendung) auf **Enable** (Aktivieren).

    ![Access Log Sample – Geladen – Aktivieren von Metadaten](./media/hdinsight-apps-install-cask/log-loaded.png)

12. Nach der Aktivierung von Metadaten wird ein Diagramm mit Überwachungsmeldungsinformationen angezeigt.

    ![Access Log Sample – Metadaten aktiviert](./media/hdinsight-apps-install-cask/log-metadata.png)

13. Klicken Sie zum Erkunden der Protokolldaten am oberen Seitenrand auf das Symbol **Erkunden**.

    ![Access Log Sample – Erkunden](./media/hdinsight-apps-install-cask/log-explore.png)

14. Eine SQL-Beispielabfrage wird angezeigt. Passen Sie diese Abfrage nach Bedarf an, und klicken Sie anschließend auf **Execute** (Ausführen).

    ![Access Log Sample – Erkunden des Datasets mithilfe einer Abfrage](./media/hdinsight-apps-install-cask/log-query.png)

15. Klicken Sie nach Abschluss der Abfrage in der Spalte mit den Aktionen auf das Symbol **Anzeigen**.

    ![Access Log Sample – Anzeigen der abgeschlossenen Abfrage](./media/hdinsight-apps-install-cask/log-query-view.png)

16. Die Abfrageergebnisse werden angezeigt.

    ![Access Log Sample – Abfrageergebnisse](./media/hdinsight-apps-install-cask/log-query-results.png)

## <a name="next-steps"></a>Nächste Schritte

* [Cask-Dokumentation](https://cask.co/resources/documentation/)
* [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md): Hier erfahren Sie, wie Sie eine nicht veröffentlichte HDInsight-Anwendung in HDInsight bereitstellen.
* [Veröffentlichen von HDInsight-Anwendungen](hdinsight-apps-publish-applications.md): Hier erfahren Sie, wie Sie benutzerdefinierte HDInsight-Anwendungen im Azure Marketplace veröffentlichen.
* [MSDN: Installieren einer HDInsight-Anwendung](https://msdn.microsoft.com/library/mt706515.aspx): Hier erfahren Sie, wie Sie HDInsight-Anwendungen definieren.
* [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md): Hier erfahren Sie, wie Sie mithilfe einer Skriptaktion zusätzliche Anwendungen installieren.
* [Verwenden leerer Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md): Hier erfahren Sie, wie Sie einen leeren Edgeknoten zum Zugreifen auf HDInsight-Cluster und zum Testen und Hosten von HDInsight-Anwendungen verwenden.
