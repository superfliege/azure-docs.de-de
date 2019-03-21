---
title: Überwachen von Hadoop-Clustern in HDInsight mit der Ambari-API – Azure
description: Verwenden Sie die Apache Ambari-APIs für die Erstellung, Verwaltung und Überwachung von Hadoop-Clustern. Die intuitiven Operatortools und APIs verbergen die Komplexität von Hadoop.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/07/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: ff6601042c82cef2b0101833117f17aca8b463dc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58223256"
---
# <a name="monitor-apache-hadoop-clusters-in-hdinsight-using-the-apache-ambari-api"></a>Überwachen von Apache Hadoop-Clustern in HDInsight mit der Apache Ambari-API
Erfahren Sie, wie HDInsight-Cluster mithilfe der Apache Ambari-APIs überwacht werden.

> [!NOTE]  
> Die Informationen in diesem Artikel betreffen in erster Linie Windows-basierte HDInsight-Cluster, die eine schreibgeschützte Version der Ambari REST-API bereitstellen. Informationen zu Linux-basierten Clustern finden Sie unter [Verwalten von Apache Hadoop-Clustern mit Apache Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Was ist Ambari?
[Apache Ambari][ambari-home] dient zur Bereitstellung, Verwaltung und Überwachung von Apache Hadoop-Clustern. Es umfasst eine Sammlung intuitiver Operatortools und eine Reihe robuster APIs, welche die Komplexität von Hadoop verbergen und den Betrieb von Clustern vereinfachen. Weitere Informationen zu den APIs finden Sie in der [Ambari-API-Referenz][ambari-api-reference]. 

HDInsight unterstützt derzeit nur das Ambari-Überwachungsfeature. Ambari API 1.0 wird von HDInsight-Clustern der Versionen 3.0 und 2.1 unterstützt. Dieser Artikel behandelt den Zugriff auf Ambari-API auf HDInsight-Clustern der Versionen 3.1 und 2.1. Die beiden Versionen unterscheiden sich hauptsächlich darin, dass sich bestimmte Komponenten mit der Einführung neuer Funktionen geändert haben (z. B. der Auftragsverlauf-Server). 

**Voraussetzungen**

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Eine Arbeitsstation mit Azure PowerShell**.
* (Optional) [cURL][curl]. Informationen zur Installation finden Sie unter [cURL-Releases und Downloads][curl-download].
  
  > [!NOTE]  
  > Wenn Sie den cURL-Befehl in Windows verwenden, geben Sie die Optionswerte in doppelten statt einfachen Anführungszeichen ein.

* **Ein Azure HDInsight-Cluster**. Anweisungen zur Bereitstellung von Clustern finden Sie unter [Erste Schritte mit HDInsight][hdinsight-get-started] und unter [Bereitstellen von HDInsight-Clustern][hdinsight-provision]. Sie benötigen die folgenden Daten, um das Lernprogramm durchzuarbeiten:
  
  | Clustereigenschaft | Azure PowerShell-Variablenname | Wert | BESCHREIBUNG |
  | --- | --- | --- | --- |
  |   HDInsight-Clustername |$clusterName | |Der Name des HDInsight-Clusters. |
  |   Cluster-Benutzername |$clusterUsername | |Der Clustername, der beim Erstellen des Clusters angegeben wurde. |
  |   Cluster-Kennwort |$clusterPassword | |Das Benutzerkennwort für den Cluster. |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a>Schnelleinstieg
Es gibt verschiedene Möglichkeiten, Ambari zur Überwachung von HDInsight-Clustern einzusetzen.

**Verwenden von Azure PowerShell**

Es folgt ein Azure PowerShell-Skript zum Abrufen der MapReduce-Auftragsverfolgungsinformationen *in einem HDInsight 3.5-Cluster.*   Der wichtigste Unterschied besteht darin, dass wir diese Informationen vom YARN-Dienst abrufen (statt von MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Es folgt ein PowerShell-Skript zum Abrufen der MapReduce-Auftragsverfolgungsinformationen *in einem HDInsight 2.1-Cluster*.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Die Ausgabe ist:

![JobTracker-Ausgabe][img-jobtracker-output]

**Verwenden von cURL**

Im folgenden Beispiel werden Clusterinformationen mithilfe von cURL abgerufen:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Die Ausgabe ist:

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

**Hinweis für die Version vom 10.8.2014**:

Wenn der Ambari-Endpunkt „https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}“ verwendet wird, gibt das Feld *host_name* den vollqualifizierten Domänennamen (FQDN) des Knotens statt des Hostnamens zurück. Vor der Version vom 10.8.2014 hat dieses Beispiel lediglich **headnode0** zurückgegeben. Seit dem 10.8.2014 wird der FQDN **headnode0.{ClusterDNS}.azurehdinsight.net** zurückgegeben, wie im obigen Beispiel gezeigt. Diese Änderung erleichtert Szenarien, in denen mehrere Clustertypen wie z. B. HBase und Hadoop in einem einzigen virtuellen Netzwerk (VNET) bereitgestellt werden. Dies ist z. B. der Fall, wenn HBase als Back-End-Plattform für Hadoop verwendet wird.

## <a name="ambari-monitoring-apis"></a>Ambari-Überwachungs-APIs
In der folgenden Tabelle sind einige der am häufigsten verwendeten Ambari-Überwachungs-API-Aufrufe aufgeführt. Weitere Informationen zur API finden Sie in der [Apache Ambari-API-Referenz][ambari-api-reference].

| Überwachungs-API-Aufruf | URI | BESCHREIBUNG |
| --- | --- | --- |
| Cluster abrufen |`/api/v1/clusters` | |
| Clusterinformationen abrufen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |Cluster, Dienste, Hosts |
| Dienste abrufen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |Services include: hdfs, mapreduce |
| Dienstinformationen abrufen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| Dienstkomponenten abrufen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker |
| Komponenteninformationen abrufen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |ServiceComponentInfo, Hostkomponenten, Metriken |
| Hosts abrufen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |headnode0, workernode0 |
| Hostinformationen abrufen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| Hostkomponenten abrufen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |namenode, resourcemanager |
| Host-Komponenteninformationen abrufen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |HostRoles, Komponente, Host, Metriken |
| Konfigurationen abrufen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |Config types: core-site, hdfs-site, mapred-site, hive-site |
| Konfigurationsinformationen abrufen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |Config types: core-site, hdfs-site, mapred-site, hive-site |

## <a name="next-steps"></a>Nächste Schritte
Sie haben erfahren, wie Apache Ambari-Überwachungs-API-Aufrufe verwendet werden. Weitere Informationen finden Sie unter:

* [Verwalten von Apache Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-portal-linux.md)
* [Verwalten von HDInsight-Clustern mit Azure PowerShell][hdinsight-admin-powershell]
* [Verwalten von HDInsight-Clustern mit der Befehlszeilenschnittstelle][hdinsight-admin-cli]
* [HDInsight-Dokumentation][hdinsight-documentation]
* [Erste Schritte mit HDInsight][hdinsight-get-started]

[ambari-home]: https://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: https://curl.haxx.se
[curl-download]: https://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: https://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
