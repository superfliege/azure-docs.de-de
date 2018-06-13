---
title: Installieren einer veröffentlichten Anwendung – Datameer – Azure HDInsight | Microsoft-Dokumentation
description: Installieren und verwenden Sie die Hadoop-Drittanbieteranwendung Datameer.
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
ms.openlocfilehash: 5008056ae2274d058706649f286b91b71feadc27
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31401308"
---
# <a name="install-published-application---datameer"></a>Installieren einer veröffentlichten Anwendung – Datameer

In diesem Artikel erfahren Sie, wie Sie die veröffentlichte Hadoop-Anwendung [Datameer](https://www.datameer.com/) in Azure HDInsight installieren und ausführen. Eine Übersicht der HDInsight-Anwendungsplattform und eine Liste mit den verfügbaren veröffentlichten Anwendungen unabhängiger Softwarehersteller (Independent Software Vendors, ISVs) finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern](hdinsight-apps-install-applications.md). Eine Anleitung zur Installation Ihrer eigenen Anwendung finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).

## <a name="about-datameer"></a>Informationen zu Datameer

Datameer ist eine native Anwendung für die Hadoop-Plattform. Sie erweitert die vorhandenen Azure HDInsight-Funktionen und bietet eine schnelle Integration, Vorbereitung und Analyse strukturierter und unstrukturierter Daten. Datameer kann auf über 70 Quellen und Formate zugreifen: strukturiert, halbstrukturiert und unstrukturiert. Sie können Daten direkt hochladen oder über ihre eindeutigen Datenverknüpfungen bedarfsgesteuert abfragen. Die Self-Service-Funktionen und die aus der Tabellenkalkulation bekannte Benutzeroberfläche von Datameer verringern die Komplexität von Big Data-Technologie und sorgen dafür, dass sich Benutzer schnell zurechtfinden. Die Tabellenkalkulationsoberfläche ermöglicht die einfache Eingabe deklarativer Formeln, die dann in optimierte Hadoop-Aufträge umgewandelt werden. Mit Datameer und Ihren BI- und Excel-Kenntnissen können Sie in kürzester Zeit Hadoop in der Cloud verwenden. Weitere Informationen finden Sie in der [Datameer-Dokumentation](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).

## <a name="prerequisites"></a>Voraussetzungen

Um diese App in einem neuen HDInsight-Cluster oder in einem vorhandenen Cluster zu installieren, müssen Sie über die folgende Konfiguration verfügen:

* Clusterebene: Standard
* Clustertyp: Hadoop
* Clusterversion: 3.4

## <a name="install-the-datameer-published-application"></a>Installieren der veröffentlichten Anwendung Datameer

Eine ausführliche Installationsanleitung für diese und andere verfügbare ISV-Anwendungen finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](hdinsight-apps-install-applications.md).

## <a name="launch-datameer"></a>Starten von Datameer

1. Nach der Installation können Sie Datameer im Azure-Portal von Ihrem Cluster aus starten. Navigieren Sie hierzu zum Bereich **Einstellungen**, und klicken Sie unter der Kategorie **Allgemein** auf **Anwendungen**. Im Bereich „Installierte Apps“ werden die installierten Anwendungen aufgeführt.

    ![Installierte Datameer-App](./media/hdinsight-apps-install-datameer/datameer-app.png)

2. Wenn Sie auf „Datameer“ klicken, werden ein Link zur Webseite und der SSH-Endpunktpfad angezeigt. Klicken Sie auf den Link zur WEBSEITE.

3. Beim ersten Start stehen zwei Lizenzoptionen zur Auswahl: eine kostenlose 14-tägige Testversion und eine Option zum Aktivieren einer vorhandenen Lizenz.

    ![Lizenzoptionen](./media/hdinsight-apps-install-datameer/license.png)

4. Nachdem Sie die gewünschte Lizenzoption ausgewählt haben, wird ein Anmeldeformular angezeigt. Geben Sie die Standardanmeldeinformationen ein, die vor dem Anmeldeformular angezeigt wurden. Akzeptieren Sie nach der Anmeldung die Softwarevereinbarung, um den Vorgang fortzusetzen.

    ![Anmeldung](./media/hdinsight-apps-install-datameer/login.png)

In den folgenden Schritten wird eine Hello World-Demonstration gezeigt.

1. [Laden Sie die Beispiel-CSV-Datei herunter.](https://datameer.box.com/s/wzzw27za3agic4yjj8zrn6vfrph0ppnf)

2. Klicken Sie am oberen Rand des Datameer-Dashboards auf das Symbol **+** und anschließend auf **Dateiupload**.

    ![Dateiupload](./media/hdinsight-apps-install-datameer/upload.png)

3. Navigieren Sie im Uploaddialogfeld zu der heruntergeladenen Datei **Hello World.csv**, und wählen Sie sie aus. Vergewissern Sie sich, dass der **Dateityp** auf „CSV/TSV“ festgelegt ist. Klicken Sie auf **Weiter**. Klicken Sie so oft auf **Weiter**, bis Sie das Ende des Assistenten erreichen.

    ![Dateiupload](./media/hdinsight-apps-install-datameer/upload-browse.png)

4. Nennen Sie die Datei **Hello World**, und platzieren Sie sie in einem neuen Ordner. Benennen Sie den neuen Ordner in „Demo“ um. Wählen Sie **Speichern**aus.

    ![Speichern](./media/hdinsight-apps-install-datameer/save.png)

5. Klicken Sie erneut auf das Symbol **+** und anschließend auf **Arbeitsmappe**, um eine neue Arbeitsmappe für die Daten zu erstellen.

    ![Hinzufügen einer Arbeitsmappe](./media/hdinsight-apps-install-datameer/add-workbook.png)

6. Erweitern Sie den Ordner **Data** > **FileUploads** und anschließend den Ordner **Demo**, den Sie beim Speichern der Datei „Hello World“ erstellt haben. Wählen Sie in der Dateiliste die Datei **Hello World** aus, und klicken Sie auf **Daten hinzufügen**.

    ![Speichern](./media/hdinsight-apps-install-datameer/select-file.png)

7. Die Daten werden in eine Tabellenkalkulationsoberfläche geladen. Wenn Sie eine Teilmenge der Daten auswählen möchten, klicken Sie auf der Symbolleiste auf die Schaltfläche **Filter**.

    ![Filterschaltfläche](./media/hdinsight-apps-install-datameer/filter-button.png)

8. Wählen Sie im Filterdialogfeld die Spalte **City** und den Operator **gleich** aus, und geben Sie in das Filtertextfeld die Zeichenfolge **Chicago** ein. Aktivieren Sie das Kontrollkästchen **Create filter in new sheet** (Filter in neuem Arbeitsblatt erstellen), und klicken Sie anschließend auf **Filter erstellen**.

    ![Anwenden eines Filters](./media/hdinsight-apps-install-datameer/apply-filter.png)

9. Speichern Sie die Arbeitsmappe durch Klicken auf **Datei** > **Speichern**. Geben Sie einen Namen an (beispielsweise „Hello World Workbook“).

10. Ihnen stehen verschiedene Optionen für Art und Zeitpunkt der Arbeitsmappenausführung zur Verfügung. Behalten Sie für diese Optionen vorerst die Standardwerte bei, aktivieren Sie das Kontrollkästchen **Start calculation process immediately after save** (Berechnung unmittelbar nach dem Speichern starten), und klicken Sie auf **Speichern**.

    ![Speichern der Arbeitsmappe](./media/hdinsight-apps-install-datameer/save-workbook.png)

11. Datameer verfügt über leistungsstarke Visualisierungstools. Erstellen Sie zum Anzeigen der Daten eine Infografik. Klicken Sie am oberen Rand des Dashboards auf das Symbol **+** und anschließend auf **Infographic** (Infografik).

    ![Hinzufügen einer Infografik](./media/hdinsight-apps-install-datameer/infographic-button.png)

12. Ziehen Sie ein Balkendiagramm-Widget aus der Widgetliste auf der linken Seite in die Mitte, wie in Schritt 1 des folgenden Diagramms dargestellt. Navigieren Sie als Nächstes im Datenbrowser auf der rechten Seite durch den Datenordner, und erweitern Sie Ihre Arbeitsmappe und anschließend das Arbeitsblatt, das Sie mit dem Filter hinzugefügt haben (Schritt 2). Ziehen Sie die Spalte **Name** über den oberen Bereich des Balkendiagramms, und platzieren Sie sie am Ziel **Bezeichnung**, um die Namensspalte der Arbeitsmappe als Bezeichnungsfeld des Diagramms festzulegen (Schritt 3).

    ![Infografik](./media/hdinsight-apps-install-datameer/infographic.png)

13. Ziehen Sie die Spalte **Age** in das Diagrammfeld **Daten**, um das Alter als Y-Achse des Diagramms festzulegen.

    ![Infografik](./media/hdinsight-apps-install-datameer/infographic-age.png)

Glückwunsch! Sie haben eine Visualisierung Ihrer Daten erstellt, ohne Code schreiben zu müssen. Nun können Sie sich mit Varianten und weiteren Visualisierungen beschäftigen.

## <a name="next-steps"></a>Nächste Schritte

* [Datameer-Dokumentation](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft)
* [Installieren von benutzerdefinierten Hadoop-Anwendungen in Azure HDInsight:](hdinsight-apps-install-custom-applications.md) Hier erfahren Sie, wie Sie eine nicht veröffentlichte HDInsight-Anwendung in HDInsight bereitstellen.
* [Veröffentlichen von HDInsight-Anwendungen](hdinsight-apps-publish-applications.md): Hier erfahren Sie, wie Sie benutzerdefinierte HDInsight-Anwendungen im Azure Marketplace veröffentlichen.
* [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx)(MSDN: Installieren einer HDInsight-Anwendung): Hier erfahren Sie, wie Sie HDInsight-Anwendungen definieren.
* [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md): Hier erfahren Sie, wie Sie mithilfe einer Skriptaktion zusätzliche Anwendungen installieren.
* [Verwenden leerer Edgeknoten in Hadoop-Clustern in HDInsight](hdinsight-apps-use-edge-node.md): Hier erfahren Sie, wie Sie einen leeren Edgeknoten zum Zugreifen auf HDInsight-Cluster und zum Testen und Hosten von HDInsight-Anwendungen verwenden.
