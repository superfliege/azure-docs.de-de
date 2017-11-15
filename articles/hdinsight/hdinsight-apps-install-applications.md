---
title: Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Hadoop-Anwendungen von Drittanbietern in Azure HDInsight installieren.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/06/2017
ms.author: jgao
ms.openlocfilehash: b23e62d3ae0fa3468a8a9a5608eb3d316852f086
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight

Erfahren Sie, wie Sie Hadoop-Anwendungen von Drittanbietern in Azure HDInsight installieren. Eine Anleitung zur Installation Ihrer eigenen Anwendung finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).

Eine HDInsight-Anwendung kann von Benutzern in einem HDInsight-Cluster installiert werden. Diese Anwendungen können von Microsoft oder von unabhängigen Softwareanbietern (Independent Software Vendors, ISVs) bezogen oder aber selbst entwickelt werden.  

Die folgende Liste zeigt die veröffentlichten Anwendungen:

* **AtScale Intelligence Platform** verwandelt Ihren HDInsight-Cluster in einen OLAP-Server mit horizontaler Hochskalierung. Die Anwendung ermöglicht Ihnen das Abfragen von Milliarden von Datenzeilen. Dabei werden interaktiv die BI-Tools von Microsoft Excel, Power BI, Tableau-Software oder QlikView verwendet.
* **Cask CDAP für HDInsight** stellt die erste einheitliche Integrationsplattform für Big Data bereit, die die Zeit bis zur Einführung in der Produktion für Datenanwendungen und Data Lakes um 80 % reduziert. Diese Anwendung unterstützt nur Standard HBase 3.4-Cluster.
* **DATAIKU DDS on HDInsight** erlaubt Datenexperten, Prototypen für hochspezifische Dienste zu entwickeln und diese Dienste anschließend zu erstellen und bereitzustellen, um Rohdaten in wirkungsvolle Unternehmensprognosen zu verwandeln.
* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) bietet Analysten eine Möglichkeit, Big-Data-Ergebnisse interaktiv zu ermitteln, zu analysieren und zu visualisieren. Binden Sie zusätzliche Datenquellen auf einfache Weise ein, um neue Beziehungen zu ermitteln und die benötigten Antworten schnell zu erhalten.
* **H2O Artificial Intelligence für HDInsight (Beta):** H2O Sparkling Water unterstützt die folgenden verteilten Algorithmen: GLM, Naïve Bayes, Distributed Random Forest, Gradient Boosting Machine, Deep Neural Networks, Deep Learning, K-Means, PCA, Generalized Low Rank Models, Anomalieerkennung und Autoencoder.
* **Kyligence Analyseplattform:** Kyligence Analytics Platform (KAP) ist ein Datawarehouse für Unternehmen, das von Apache Kylin und Apache Hadoop unterstützt wird. Es ermöglicht Abfragelatenzen unter einer Sekunde in riesigen Datensätzen und vereinfacht die Datenanalyse für Geschäftsbenutzer und Analytiker. 
* **Self-Service Data Prep von Paxata**
* **SnapLogic Hadooplex:** Mit SnapLogic Hadooplex unter HDInsight können Kunden geschäftliche Erkenntnisse schneller gewinnen, indem Self-Service-Dateneingabe und -vorbereitung aus nahezu jeder Quelle für die Microsoft Azure-Cloudplaftform bereitgestellt werden.
* **Spark Job Server für KNIME Spark Executor:** Spark Job Server für KNIME Spark Executor wird zum Verbinden der KNIME Analytics-Plattform mit HDInsight-Clustern verwendet.
* **Streamsets Data Collector für HDInsight** stellt eine voll ausgestattete integrierte Entwicklungsumgebung (IDE) bereit, mit der Sie n:n-Erfassungspipelines entwerfen, testen, bereitstellen und verwalten können. Diese Pipelines mischen Strom- und Batchdaten und enthalten eine Reihe von datenstrominternen Transformationen – ohne benutzerdefinierten Code schreiben zu müssen. 
* **WANdisco Fusion HDI-App** ermöglicht dauerhafte konsistente Konnektivität mit Daten, während diese sich ändern, und zwar unabhängig vom Speicherort. Sie bietet Ihnen jederzeit und an jedem Ort Zugriff auf Ihre Daten – ohne Ausfallzeiten oder Unterbrechungen.

Für die Anleitungen in diesem Artikel wird das Azure-Portal verwendet. Sie können die Azure Resource Manager-Vorlage auch aus dem Portal exportieren oder eine Kopie der Resource Manager-Vorlage von Anbietern erhalten und Azure PowerShell und die Azure CLI zum Bereitstellen der Vorlage verwenden.  Weitere Informationen finden Sie unter [Erstellen von Hadoop-Clustern in HDInsight mit Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie HDInsight-Anwendungen in einem vorhandenen HDInsight-Cluster installieren möchten, benötigen Sie einen HDInsight-Cluster. Informationen zum Erstellen eines solchen Clusters finden Sie unter [Erstellen von Clustern](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). Im Zuge der HDInsight-Clustererstellung können Sie auch HDInsight-Anwendungen installieren.

## <a name="install-applications-to-existing-clusters"></a>Installieren von Anwendungen für vorhandene Cluster
Das folgende Verfahren veranschaulicht, wie Sie HDInsight-Anwendungen für einen vorhandenen HDInsight-Cluster installieren.

**So installieren Sie eine HDInsight-Anwendung**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **HDInsight-Cluster** .
3. Klicken Sie auf einen HDInsight-Cluster.  Falls Sie noch keinen Cluster besitzen, müssen Sie zuerst einen erstellen.  Weitere Informationen finden Sie unter [Erstellen von Clustern](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Klicken Sie in der Kategorie **Konfigurationen** auf **Anwendungen**. Sie sehen eine Liste mit installierten Anwendungen. Sollte „Anwendungen“ nicht angezeigt werden, sind für diese Version des HDInsight-Clusters keine Anwendungen vorhanden.
   
    ![HDInsight-Anwendungen – Portal-Menü](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Klicken Sie im Menü auf **Hinzufügen**. 
   
    ![HDInsight-Anwendungen – Installierte Apps](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Eine Liste mit den vorhandenen HDInsight-Anwendungen wird angezeigt.
   
    ![HDInsight-Anwendungen – Verfügbare Anwendungen](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Klicken Sie auf eine Anwendung, stimmen Sie den rechtlichen Bedingungen zu, und klicken Sie dann auf **Auswählen**.

Sie können den Status der Installation über die Portalbenachrichtigungen verfolgen. (Klicken Sie oben im Portal auf das Glockensymbol.) Nach der Installation wird die Anwendung in der Liste „Installierte Apps“ angezeigt.

## <a name="install-applications-during-cluster-creation"></a>Installieren von Anwendungen während der Clustererstellung
Sie haben die Möglichkeit, die HDInsight-Anwendung beim Erstellen eines Clusters zu installieren. Während des Prozesses werden HDInsight-Anwendungen installiert, nachdem der Cluster erstellt wurde und sich im ausgeführten Zustand befindet. Um während der Erstellung des Clusters Anwendungen über das Azure-Portal zu installieren, verwenden Sie die Option „--Custom--“ anstelle der Standardoption „--Quick create--“.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Auflisten der installierten HDInsight-Apps und Eigenschaften
Im Portal wird eine Liste mit den installierten HDInsight-Anwendungen für einen Cluster und den Eigenschaften der einzelnen installierten Anwendungen angezeigt.

**So listen Sie HDInsight-Anwendungen und Eigenschaften auf**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **HDInsight-Cluster** . 
3. Klicken Sie auf einen HDInsight-Cluster.
4. Klicken Sie in **Einstellungen** unter der Kategorie **Allgemein** auf **Anwendungen**. Die installierten Apps werden auf der rechten Seite aufgeführt. 
   
    ![HDInsight-Anwendungen – Installierte Apps](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Klicken Sie auf eine installierte Anwendung, um die Eigenschaft anzuzeigen. Die Eigenschaft zeigt Folgendes:
   
   * App-Name: Name der Anwendung.
   * Status: Status der Anwendung. 
   * Webseite: die URL der Webanwendung, die Sie auf dem Edgeknoten bereitgestellt haben. Die Anmeldeinformationen sind mit den HTTP-Benutzeranmeldeinformationen identisch, die Sie für den Cluster konfiguriert haben.
   * HTTP-Endpunkt: Die Anmeldeinformationen sind mit den HTTP-Benutzeranmeldeinformationen identisch, die Sie für den Cluster konfiguriert haben. 
   * SSH-Endpunkt: Sie können SSH verwenden, um eine Verbindung mit dem Edgeknoten herzustellen. Die SSH-Anmeldeinformationen sind mit den SSH-Benutzeranmeldeinformationen identisch, die Sie für den Cluster konfiguriert haben. Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
6. Klicken Sie zum Löschen einer Anwendung mit der rechten Maustaste darauf, und klicken Sie dann im Kontextmenü auf **Löschen**.

## <a name="connect-to-the-edge-node"></a>Herstellen einer Verbindung mit dem Edgeknoten
Sie können per HTTP und SSH eine Verbindung mit dem Edgeknoten herstellen. Sie finden die Endpunktinformationen im [Portal](#list-installed-hdinsight-apps-and-properties). Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

Die HTTP-Endpunkt-Anmeldeinformationen sind die HTTP-Benutzeranmeldeinformationen, die Sie für den HDInsight-Cluster konfiguriert haben. Die SSH-Endpunkt-Anmeldeinformationen sind die SSH-Anmeldeinformationen, die Sie für den HDInsight-Cluster konfiguriert haben.

## <a name="troubleshoot"></a>Problembehandlung
Weitere Informationen finden Sie unter [Behandeln von Installationsproblemen](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Nächste Schritte
* [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md): Hier erfahren Sie, wie Sie eine nicht veröffentlichte HDInsight-Anwendung in HDInsight bereitstellen.
* [Veröffentlichen von HDInsight-Anwendungen](hdinsight-apps-publish-applications.md): Hier erfahren Sie, wie Sie benutzerdefinierte HDInsight-Anwendungen im Azure Marketplace veröffentlichen.
* [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx)(MSDN: Installieren einer HDInsight-Anwendung): Hier erfahren Sie, wie Sie HDInsight-Anwendungen definieren.
* [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md): Hier erfahren Sie, wie Sie mithilfe der Skriptaktion zusätzliche Anwendungen installieren.
* [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight mit Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Hier erfahren Sie, wie Sie Resource Manager-Vorlagen für die Erstellung von HDInsight-Clustern aufrufen.
* [Verwenden leerer Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md): Erfahren Sie, wie Sie einen leeren Edgeknoten zum Zugreifen auf HDInsight-Cluster, Testen von HDInsight-Anwendungen und Hosten von HDInsight-Anwendungen verwenden.

