---
title: Installieren einer veröffentlichten Anwendung – Dataiku DSS – Azure HDInsight | Microsoft-Dokumentation
description: Installieren und verwenden Sie die Hadoop-Drittanbieteranwendung Dataiku DSS.
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
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 835f433e0bf79e8bc4d9b30963196f65bc53cd0e
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="install-published-application---dataiku-dds"></a>Installieren einer veröffentlichten Anwendung – Dataiku DSS

In diesem Artikel wird beschrieben, wie Sie die veröffentlichte Hadoop-Anwendung [Dataiku DSS](https://www.dataiku.com/) in Azure HDInsight installieren und ausführen. Eine Übersicht der HDInsight-Anwendungsplattform und eine Liste mit den verfügbaren veröffentlichten Anwendungen unabhängiger Softwarehersteller (Independent Software Vendors, ISVs) finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern](hdinsight-apps-install-applications.md). Eine Anleitung zur Installation Ihrer eigenen Anwendung finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).

## <a name="about-dataiku-dss"></a>Grundlegendes zu Dataiku DSS

[Data Science Studio (DSS)](https://www.dataiku.com/dss/features/connectivity/) von Daitaku ist eine Data Science-Plattform für die Zusammenarbeit, die Data Scientists das Entwickeln und Bereitstellen von Analyselösungen ermöglicht. Durch die Bereitstellung von DSS als HDInsight-Anwendung können Sie Big Data-Lösungen auf der Grundlage von Data Science entwickeln und auf Unternehmensniveau ausführen.

Mithilfe von DSS können Sie eine umfassende Analyselösung implementieren, angefangen bei Datenerfassung, -vorbereitung und -verarbeitung. Eine DSS-Lösung kann außerdem Trainingsfunktionen umfassen und Machine Learning-Modelle, Visualisierung und eine anschließende Operationalisierung anwenden.

Sie können DSS mithilfe von Hadoop- oder Spark-Clustern in HDInsight installieren. Hierbei können Sie auswählen, ob die Installation von DSS auf vorhandenen Clustern oder bei der Erstellung neuer Cluster erfolgen soll. DSS unterstützt außerdem Azure Blob Storage als Connector zum Lesen von Daten.

Sie können mithilfe von DSS Projekte erstellen, die anschließend MapReduce- oder Spark-Aufträge generieren. Diese Auftrage werden als reguläre MapReduce- oder Spark-Aufträge in HDInsight ausgeführt, sodass Sie den Cluster nach Bedarf skalieren können.

## <a name="prerequisites"></a>Voraussetzungen

Um diese App in einem neuen HDInsight-Cluster oder in einem vorhandenen Cluster zu installieren, müssen Sie über die folgende Konfiguration verfügen:

* Clustertarif(e): Standard, Premium
* Clustertyp(en): Hadoop, Spark
* Clusterversion(en): 3.4, 3.5

## <a name="install-the-dataiku-dss-published-application"></a>Installieren der veröffentlichten Anwendung Dataiku DSS

Eine ausführliche Installationsanleitung für diese und andere verfügbare ISV-Anwendungen finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](hdinsight-apps-install-applications.md).

## <a name="launch-dataiku-dss"></a>Starten von Dataiku DSS

1. Nach der Installation können Sie DSS aus Ihrem Cluster im Azure-Portal starten, indem Sie zum Bereich **Einstellungen** wechseln und dann unterhalb der Kategorie **Allgemein** auf **Anwendungen** klicken. Im Bereich „Installierte Apps“ werden alle installierten Anwendungen aufgeführt.

    ![Installierte Dataiku DSS-App](./media/hdinsight-apps-install-dataiku/app.png)

2. Wenn Sie DSS in HDInsight auswählen, werden ein Link zur Webseite und der SSH-Endpunktpfad angezeigt. Wählen Sie den Link zur Webseite aus.

3. Beim ersten Start wird ein Formular angezeigt, über das Sie entweder kostenfrei ein neues Dataiku-Konto erstellen oder sich bei einem vorhandenen Konto anmelden können. Sie haben außerdem die Option, eine 2-Wochen-Testversion der [Enterprise Edition](https://www.dataiku.com/dss/editions/) zu starten. An diesem Punkt haben Sie die Möglichkeit, mit der Eingabe eines Lizenzschlüssels für die Enterprise Edition fortzufahren oder die Community Edition zu verwenden.

4. Nachdem Sie die gewünschte Lizenzoption ausgewählt haben, wird ein Anmeldeformular angezeigt. Geben Sie die Standardanmeldeinformationen ein, die vor dem Anmeldeformular angezeigt wurden.

Die nachfolgend aufgeführten Schritte dienen zur Veranschaulichung.

1. [Laden Sie die CSV-Datei mit Beispielbestellungen herunter](https://doc.dataiku.com/tutorials/data/101/haiku_shirt_sales.csv).

2. Wählen Sie im DSS-Dashboard im Menü links den Link **+** (Neues Projekt) aus, um ein neues Projekt zu erstellen.

    ![Link für neues Projekt](./media/hdinsight-apps-install-dataiku/new-project.png)

3. Geben Sie im Formular „New Project“ (Neues Projekt) einen Wert für **Name** ein. Der **Project Key** (Projektschlüssel) wird automatisch mit einem vorgeschlagenen Wert aufgefüllt. Geben Sie in diesem Fall „Orders“ (Bestellungen) ein. Klicken Sie auf **CREATE** (Erstellen).

    ![Formular für neues Projekt](./media/hdinsight-apps-install-dataiku/new-project-form.png)

4. Wählen Sie auf der Seite für Ihr neues Projekt **+ IMPORT YOUR FIRST DATASET** (Erstes Dataset importieren) aus.

    ![Dateiupload](./media/hdinsight-apps-install-dataiku/import-dataset.png)

5. Wählen Sie unterhalb der Datasetliste **Files** (Dateien) die Einstellung **Upload your Files** (Dateien hochladen) aus. Ein Dialogfeld für den Upload wird geöffnet. Klicken Sie auf „Add a file“ (Datei hinzufügen), wählen Sie die Datei `haiku_shirt_sales.csv` aus, die Sie heruntergeladen haben, und überprüfen Sie sie.

6. Die Datei wird nach DSS hochgeladen. Überprüfen Sie, ob DSS das CSV-Format richtig erkannt hat, indem Sie auf die Schaltfläche für die Vorschau klicken.

    ![Dateiupload](./media/hdinsight-apps-install-dataiku/preview.png)

7. Der Import ist fast vollständig. Die CSV-Datei verwendet Tabstopps als Trennzeichen. Sie können sehen, dass die Daten in tabellarischer Form vorliegen, wobei die Spalten Merkmale enthalten und die Zeilen die beobachteten Werte repräsentieren. Der einzige Fehler ist offensichtlich, dass die Datei eine Leerzeile zwischen der Überschrift und den Daten enthält. Um diesen Fehler zu korrigieren, geben Sie `1` in das Feld **Skip next lines** (Nächste Zeilen überspringen) ein.

    ![Speichern](./media/hdinsight-apps-install-dataiku/skip-lines.png)

8. Geben Sie dem neuen Dataset einen Namen. Geben Sie **haiku_shirt_sales** im Feld oben auf dem Bildschirm ein, und wählen Sie dann die Schaltfläche **Create** (Erstellen) aus.

9. Sie sehen eine tabellarische Ansicht Ihrer Daten, in der Sie mit einer Untersuchung beginnen können. Bei jeder Spalte sollten Sie sehen, dass Dataiku Science Studio einen Datentyp (in _blau_) erkannt hat – in diesem Fall Text, eine Zahl oder Daten (nicht analysiert). Ein Messwert zeigt das Verhältnis der Spalte an, für die die Werte nicht dem Typ entsprechen (rot) oder fehlen (leer). In diesem Beispieldataset umfasst die Spalte „department“ (Abteilung) sowohl leere Werte als auch ungültige Daten.

    ![Tabellarische Ansicht](./media/hdinsight-apps-install-dataiku/viewing-dataset.png)

## <a name="data-manipulation"></a>Datenbearbeitung

Reale Daten sind fast immer unübersichtlich und nur selten ordentlich paketiert. Für das Bereinigen von Daten sind üblicherweise zahlreiche Skripts und die damit verbundene Geschäftslogik erforderlich. Dataiku DSS bietet ein dediziertes Tool namens **Lab** mit dem diese Aufgabe benutzerfreundlicher wird.

1. Klicken Sie oben rechts auf **Lab**.

    ![Schaltfläche „Lab“](./media/hdinsight-apps-install-dataiku/lab-button.png)

2. Das Lab-Fenster wird geöffnet. Im Lab-Fenster können Sie iterativ in Ihrem Dataset arbeiten, um es genauer zu untersuchen. In diesem Tutorial wird der Aspekt der visuellen Analyse demonstriert. Wählen Sie unterhalb von „Visual Analysis“ (Visuelle Analyse) die Schaltfläche **Neu** aus. Sie werden aufgefordert, einen Namen für Ihre Analyse einzugeben. Behalten Sie vorerst den Standardnamen bei, und klicken Sie auf **CREATE** (Erstellen).

    ![Lab erstellen](./media/hdinsight-apps-install-dataiku/create-lab.png)

3. Wählen Sie oben rechts auf der Seite die Schaltfläche **Quick columns stats** (Schnelle Spaltenstatistik) aus.

    ![Schnelle Spaltenstatistik](./media/hdinsight-apps-install-dataiku/quick-column-stats.png)

4. Unterhalb des Bereichs **Columns quick view** (Schnellanzeige für Spalten) werden Statistiken für Datentypen und -werte in zeitachsenbasierten Diagrammen angezeigt.

    ![Schnellanzeige für Spalten](./media/hdinsight-apps-install-dataiku/columns-quick-view.png)

Sie können DSS jetzt mithilfe der Beispieldaten erkunden. Sie können die Daten bereinigen und damit arbeiten sowie neue Visualisierungen erstellen.

Ausführlichere Tutorials finden Sie unter [Learn Dataiku DSS](https://www.dataiku.com/learn/) (in englischer Sprache).

## <a name="next-steps"></a>Nächste Schritte

* [Dokumentation zu Dataiku DSS](https://doc.dataiku.com/dss/latest/).
* [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md): Hier erfahren Sie, wie Sie eine nicht veröffentlichte HDInsight-Anwendung in HDInsight bereitstellen.
* [Veröffentlichen von HDInsight-Anwendungen](hdinsight-apps-publish-applications.md): Hier erfahren Sie, wie Sie benutzerdefinierte HDInsight-Anwendungen im Azure Marketplace veröffentlichen.
* [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx)(MSDN: Installieren einer HDInsight-Anwendung): Hier erfahren Sie, wie Sie HDInsight-Anwendungen definieren.
* [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md): Hier erfahren Sie, wie Sie mithilfe einer Skriptaktion zusätzliche Anwendungen installieren.
* [Verwenden leerer Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md): Erfahren Sie, wie Sie einen leeren Edgeknoten zum Zugreifen auf HDInsight-Cluster und zum Testen und Hosten von HDInsight-Anwendungen verwenden.
