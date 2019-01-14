---
title: Programmgesteuertes Zugreifen auf Hadoop YARN-Anwendungsprotokolle – Azure
description: Greifen Sie programmgesteuert auf Anwendungsprotokolle auf einem Hadoop-Cluster in HDInsight zu.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 7326cf6a1153d5dc1f7e5f910a376a21b05db606
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725439"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-windows-based-hdinsight"></a>Zugreifen auf Apache Hadoop YARN-Anwendungsprotokolle in Windows-basiertem HDInsight
Dieses Dokument erläutert den Zugriff auf Protokolle für [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)-Anwendungen, die in einem Windows-basierten Apache Hadoop-Cluster in Azure HDInsight abgeschlossen wurden.

> [!IMPORTANT]  
> Die Informationen in diesem Dokument gelten nur für Windows-basierte HDInsight-Cluster. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement). Informationen zum Zugreifen auf YARN-Protokolle in Linux-basierten HDInsight-Clustern finden Sie unter [Zugreifen auf Apache Hadoop YARN-Anwendungsprotokolle in Linux-basierten Apache Hadoop-Clustern in HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md).


### <a name="prerequisites"></a>Voraussetzungen
* Ein Windows-basierter HDInsight-Cluster  Weitere Informationen finden Sie unter [Erstellen Windows-basierter Apache Hadoop-Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN Timeline Server
Der <a href="https://hadoop.apache.org/docs/r2.4.1/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Apache Hadoop YARN Timeline Server</a> bietet mittels zweier Schnittstellen allgemeine Informationen zu abgeschlossenen Anwendungen sowie frameworkspezifische Anwendungsinformationen. Dies gilt insbesondere in folgenden Fällen:

* Speicherung und Abruf allgemeiner Anwendungsinformationen zu HDInsight-Clustern sind für Versionen ab 3.1.1.374 aktiviert.
* Die frameworkspezifische Anwendungsinformationskomponente des Timeline Servers steht für HDInsight-Cluster derzeit nicht zur Verfügung.

Zu den allgemeinen Informationen zu Anwendungen zählen die folgenden Datenarten:

* Die Anwendungs-ID, ein eindeutiger Bezeichner einer Anwendung
* Der Benutzer, der die Anwendung gestartet hat
* Informationen zu den erfolgten Versuchen, die Anwendung abzuschließen
* Die bei Anwendungsversuchen verwendeten Container

In Ihren HDInsight-Clustern werden diese Informationen vom Azure Resource Manager gespeichert. Die Informationen werden im Verlaufsspeicher im Standardspeicher des Clusters gespeichert. Diese allgemeinen Daten zu abgeschlossenen Anwendungen können über eine REST-API abgerufen werden:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>YARN-Anwendungen und -Protokolle
YARN unterstützt mehrere Programmierungsmodelle, indem die Ressourcenverwaltung von der Zeitplanung/Überwachung von Anwendungen getrennt wird. YARN verwendet einen globalen *ResourceManager* (RM), workerknotenbezogene *NodeManager* (NMs) und anwendungsbezogene *ApplicationMaster* (AMs). Der anwendungsbezogene AM handelt Ressourcen (CPU, Arbeitsspeicher, Datenträger, Netzwerk) für die Ausführung Ihrer Anwendung mit dem RM aus. Der RM arbeitet mit NMs zusammen, um diese Ressourcen zu gewähren, die als *Container* zugewiesen werden. Der AM ist zuständig für die Nachverfolgung des Status der Container, die ihm vom RM zugewiesen wurden. Je nach Art der Anwendung kann diese viele Container benötigen.

* Jede Anwendung kann aus mehreren *Anwendungsversuchen* bestehen. 
* Container werden einem bestimmten Versuch einer Anwendung zugewiesen. 
* Ein Container stellt den Kontext für eine grundlegende Aufgabeneinheit bereit. 
* Aufgaben, die im Kontext eines Containers erledigt werden, erfolgen auf dem einzelnen Workerknoten, dem der Container zugeordnet wurde. 

Weitere Informationen finden Sie unter [Apache Hadoop YARN – Konzepte][YARN-concepts].

Anwendungsprotokolle (und dazugehörige Containerprotokolle) sind für das Beheben von Problemen bei Hadoop-Anwendungen besonders wichtig. YARN bietet mit seinem Feature [Log Aggregation][log-aggregation] ein nützliches Gerüst für das Sammeln, Zusammenführen und Speichern von Anwendungsprotokollen. Durch "Log Aggregation" lässt sich der Zugriff auf Anwendungsprotokolle besser steuern, denn Protokolle für alle Container auf einem Arbeitsknoten werden zusammengeführt und als zentrale Protokolldatei pro Workerknoten im Standarddateisystem gespeichert, nachdem eine Anwendung abgeschlossen wurde. Ihre Anwendung mag Hunderte oder Tausende von Containern verwenden, doch Protokolle für alle auf einem einzelnen Workerknoten vorhandenen Container werden zu einer zentralen Datei zusammengeführt, was in einer Datei pro Workerknoten mündet, die von Ihrer Anwendung genutzt werden. Log Aggregation ist für HDInsight-Cluster (ab Version 3.0) standardmäßig aktiviert. Zusammengeführte Protokolle befinden sich im Standardcontainer Ihres Clusters am folgenden Speicherort:

    wasb:///app-logs/<user>/logs/<applicationId>

An diesem Speicherort ist *user* der Name des Benutzers, der die Anwendung gestartet hat. *applicationId* ist der eindeutige Bezeichner einer Anwendung gemäß der Zuweisung durch den YARN Resource Manager (RM).

Die zusammengeführten Protokolle sind nicht unmittelbar lesbar, da sie in [TFile][T-file] geschrieben werden, einem nach Container indiziertem [Binärformat][binary-format]. YARN bietet CLI-Tools zum Ausgeben dieser Protokolle mit unformatiertem Text für Anwendungen und Container von Interesse. Sie können diese Protokolle als unformatierten Text anzeigen, indem Sie einen der folgenden YARN-Befehle direkt auf die Clusterknoten anwenden (nachdem über RDP eine Verbindung hergestellt wurde):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>YARN-ResourceManager-Benutzeroberfläche
Die YARN-ResourceManager-Benutzeroberfläche wird auf dem Hauptknoten des Clusters ausgeführt. Der Zugriff darauf erfolgt über das Dashboard im Azure-Portal:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im linken Menü auf **Durchsuchen**, dann auf **HDInsight-Cluster** und danach auf einen Windows-basierten Cluster, auf dessen YARN-Anwendungsprotokolle Sie zugreifen möchten.
3. Klicken Sie im oberen Menü auf **Dashboard**. Eine auf einer neuen Browserregisterkarte geöffnete Seite namens **HDInsight-Abfragekonsole** wird angezeigt.
4. Klicken Sie in der **HDInsight-Abfragekonsole** auf **Yarn-Benutzeroberfläche**.

[YARN-timeline-server]:https://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
