---
title: "Überwachen von Hadoop-Clustern in HDInsight mithilfe der Ambari-API - Azure | Microsoft Docs"
description: "Verwenden Sie die Apache-Ambari-APIs zum Erstellen, verwalten und Überwachen von Hadoop-Clustern. Verbergen die Komplexität der Hadoop, intuitive Operator Tools und APIs."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
editor: cgronlun
manager: jhubbard
ms.assetid: 052135b3-d497-4acc-92ff-71cee49356ff
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: b6fc2098027690eb76b69b1427f0e9541b8a7a69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Überwachen von Hadoop-Clustern in HDInsight mithilfe der Ambari-API
Informationen Sie zum Überwachen von HDInsight-Clustern mit Ambari-APIs.

> [!NOTE]
> Die Informationen in diesem Artikel sind in erster Linie für Windows-basierten HDInsight-Cluster, die eine schreibgeschützte Version der Ambari REST-API bereitstellen. Linux-basierten Clustern finden Sie unter [verwalten Hadoop-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md).
> 
> 

## <a name="what-is-ambari"></a>Was ist Ambari?
[Apache Ambari] [ ambari-home] dient zum Bereitstellen, verwalten und Überwachen von Apache Hadoop-Clustern. Sie enthält eine intuitive Tools Operator und einen robusten Satz von APIs, die die Komplexität der Hadoop, vereinfachen den Vorgang der Cluster ausblenden. Weitere Informationen zu den APIs finden Sie unter [Ambari-API-Referenz][ambari-api-reference]. 

HDInsight unterstützt derzeit nur die Ambari Überwachungsfunktion. Ambari-API 1.0 wird von der HDInsight-Version 3.0 und 2.1-Clustern unterstützt. Dieser Artikel umfasst den Zugriff auf Ambari-APIs auf HDInsight-Version 3.1 und 2.1-Clustern. Der Hauptunterschied zwischen den beiden ist, dass einige der Komponenten mit der Einführung neuer Funktionen (z. B. den Verlauf Auftragsserver) geändert wurden. 

**Voraussetzungen**

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie die folgenden Elemente:

* **Eine Arbeitsstation mit Azure PowerShell**.
* (Optional) [cURL][curl]. Um es zu installieren, finden Sie unter [cURL-Versionen und Downloads][curl-download].
  
  > [!NOTE]
  > Beim Verwenden der cURL-Befehl in Windows verwenden doppelte Anführungszeichen statt einzelne Anführungszeichen für die Optionswerte.
  > 
  > 
* **Azure HDInsight-Clusters**. Anleitungen zur Clusterbereitstellung finden Sie unter [erste Schritte mit HDInsight] [ hdinsight-get-started] oder [bereitstellen HDInsight-Clustern][hdinsight-provision]. Sie benötigen die folgenden Daten im Lernprogramm zu durchlaufen:
  
  | Clustereigenschaft | Azure PowerShell-Variablenname | Wert | Beschreibung |
  | --- | --- | --- | --- |
  |   HDInsight-Clusternamen |$clusterName | |Der Name des HDInsight-Clusters. |
  |   Clusterbenutzername |$clusterUsername | |Cluster-Benutzername angegeben, wenn der Cluster erstellt wurde. |
  |   Clusterkennwort |$clusterPassword | |Cluster-Benutzerkennwort. |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a>Schnelleinstieg
Es gibt mehrere Möglichkeiten zum Ambari HDInsight-Cluster überwachen.

**Verwenden von Azure PowerShell**

Das folgende Azure PowerShell-Skript ruft die MapReduce-Auftragsinformationen Tracker *in einem 3.5 HDInsight-Cluster.*  Der Hauptunterschied besteht darin, dass wir diese Details aus den YARN-Dienst (anstelle von MapReduce) pull.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Das folgende PowerShell-Skript ruft die MapReduce-Auftragsinformationen Tracker *in einem Cluster HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Die Ausgabe lautet:

![Jobtracker-Ausgabe][img-jobtracker-output]

**CURL verwenden**

Im folgenden Beispiel wird die Clusterinformationen cURL mit:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Die Ausgabe lautet:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**Für die Version 10/8/2014**:

Bei Verwendung des Endpunkts Ambari "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", die *Host_name* Feld gibt den vollqualifizierten Domänennamen (FQDN) des Knotens anstelle des Hostnamens zurück. Vor der Version 10/8/2014 wird in diesem Beispiel einfach zurückgegeben "**headnode0**". Nach der Veröffentlichung 10/8/2014 erhalten Sie den FQDN "**headnode0. {} ClusterDNS} .azurehdinsight .net**", wie im vorherigen Beispiel gezeigt. Diese Änderung war erforderlich, um Szenarien zu ermöglichen, in denen mehrere Clustertypen (z. B. HBase und Hadoop) in ein virtuelles Netzwerk (VNET) bereitgestellt werden kann. Dies geschieht z. B. bei Verwendung von HBase als Back-End-Plattform für Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari Überwachung APIs
Die folgende Tabelle enthält einige der am häufigsten verwendeten Ambari API-Aufrufe zu überwachen. Weitere Informationen zur API finden Sie unter [Ambari-API-Referenz][ambari-api-reference].

| Monitor-API-Aufruf | URI | Beschreibung |
| --- | --- | --- |
| Abrufen von Clustern |`/api/v1/clusters` | |
| Abrufen von Clusterinformationen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |Cluster, Services, hosts |
| Abrufen von Diensten |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |Diensten gehören: Hdfs Mapreduce |
| Abrufen von Informationen für Dienste. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| Dienstkomponenten abrufen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |HDFS: – Namenode, DatanodeMapReduce: Jobtracker; "tasktracker" |
| Abrufen von Informationen der Komponente. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |ServiceComponentInfo, Host-Komponenten, Metriken |
| Abrufen von hosts |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |headnode0 workernode0 |
| Abrufen von Host-Informationen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| Abrufen von Host-Komponenten |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |– Namenode, resourcemanager |
| Abrufen von Host Komponente Informationen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |HostRoles, Komponente, Host, Metriken |
| Abrufen von Konfigurationen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |Config-Typen: Core-Standort, Hdfs-Standort, Mapred-Standort, Hive-Standort |
| Abrufen von Konfigurationsinformationen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |Config-Typen: Core-Standort, Hdfs-Standort, Mapred-Standort, Hive-Standort |

## <a name="next-steps"></a>Nächste Schritte
Jetzt haben Sie gelernt, wie Ambari Überwachung API-Aufrufe verwendet wird. Weitere Informationen finden Sie unter:

* [Verwalten von HDInsight-Cluster mit der Azure-portal][hdinsight-admin-portal]
* [Verwalten von HDInsight-Cluster mit Azure PowerShell][hdinsight-admin-powershell]
* [Verwalten von HDInsight-Cluster mithilfe des Befehlszeilen-Schnittstelle][hdinsight-admin-cli]
* [HDInsight-Dokumentation][hdinsight-documentation]
* [Erste Schritte mit HDInsight][hdinsight-get-started]

[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
