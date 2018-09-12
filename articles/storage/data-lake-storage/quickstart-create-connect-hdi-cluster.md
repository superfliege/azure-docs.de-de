---
title: Einrichten von Clustern für Hadoop, Spark, Kafka, HBase oder R Server – Azure HDInsight
description: Richten Sie Hadoop-, Kafka-, Spark-, HBase-, R Server- oder Storm-Cluster für HDInsight über einen Browser, die Azure-Befehlszeilenschnittstelle, Azure PowerShell, REST oder ein SDK ein.
keywords: Hadoop-Cluster einrichten, Kafka-Cluster einrichten, Spark-Cluster einrichten, was ist ein Hadoop-Cluster?
services: storage
author: jamesbak
tags: azure-portal
ms.component: data-lake-storage-gen2
ms.service: storage
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: 8bfe069ba03aec2476d437f25ebb00f8a934c845
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44025552"
---
# <a name="quickstart-set-up-clusters-in-hdinsight"></a>Schnellstart: Einrichten von Hadoop-Clustern in HDInsight

In diesem Schnellstart erfahren Sie, wie Sie Cluster in HDInsight mit Hadoop, Spark, Kafka, Interactive Query, HBase, R Server oder Storm einrichten und konfigurieren. Sie erfahren auch, wie Sie Cluster anpassen, sie in eine Domäne einbinden und mit [Azure Data Lake Storage Gen2 Preview](introduction.md) verbinden.

Ein Hadoop-Cluster besteht aus mehreren virtuellen Computern (Knoten), die zur verteilten Verarbeitung von Aufgaben verwendet werden. Azure HDInsight verwaltet die Implementierungsdetails der Installation und Konfiguration einzelner Knoten, sodass Sie nur allgemeine Konfigurationsinformationen bereitstellen müssen.

> [!IMPORTANT]
>Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden. Erfahren Sie, wie Sie [einen Cluster löschen](../../hdinsight/hdinsight-delete-cluster.md).

Data Lake Storage wird in diesem Schnellstart als Datenschicht verwendet. Mit dem Dienst „Hierarchischer Namespace“ und dem [Hadoop-Treiber](abfs-driver.md) ist Data Lake Storage für die verteilte Verarbeitung und Analyse optimiert. In Data Lake Storage gespeicherten Daten bleiben auch nach dem Löschen eines HDInsight-Clusters erhalten.

## <a name="cluster-setup-methods"></a>Methoden für die Clustereinrichtung

Die folgende Tabelle zeigt die verschiedenen Methoden, die Sie zum Einrichten eines HDInsight-Clusters verwenden können.

| Verfahren zur Clustererstellung | Webbrowser | Befehlszeile | REST-API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure-Portal](../../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](../../hdinsight/hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI (Version 1.0)](../../hdinsight/hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](../../hdinsight/hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](../../hdinsight/hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [Azure-Ressourcen-Manager-Vorlagen](../../hdinsight/hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Schnellerfassung: grundlegende Clustereinrichtung

Dieser Artikel enthält die Schritte im [Azure-Portal](https://portal.azure.com) zum Erstellen eines HDInsight-Clusters mithilfe der *Schnellerfassung* oder *benutzerdefiniert*. 

![HDInsight-Erstellungsoptionen – benutzerdefinierte Schnellerfassung](media/quickstart-create-connect-hdi-cluster/hdinsight-creation-options.png)

Befolgen Sie die Anweisungen auf dem Bildschirm, um eine grundlegenden Clustereinrichtung durchzuführen. Sie finden im Folgenden folgende Informationen:

* [Ressourcengruppenname](#resource-group-name)
* [Clustertypen und Konfiguration](#cluster-types) 
* [Clusteranmeldung und SSH-Benutzername](#cluster-login-and-ssh-user-name)
* [Location](#location)

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight 3.3](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="resource-group-name"></a>Ressourcengruppenname

Mit [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) können Sie mit den Ressourcen in Ihrer Anwendung als Gruppe arbeiten. Dies wird als „Azure-Ressourcengruppe“ bezeichnet. Sie können alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen, aktualisieren, überwachen oder löschen.

## <a name="cluster-types"></a> Clustertypen und Konfiguration
Azure HDInsight bietet derzeit die folgenden Typen von Clustern mit je einer Reihe von Komponenten, um bestimmte Funktionen bereitzustellen.

> [!IMPORTANT]
> HDInsight-Cluster sind jeweils für einzelne Workloads oder Technologien in verschiedenen Typen verfügbar. Es ist keine unterstützte Methode zum Erstellen eines Clusters vorhanden, bei der mehrere Typen kombiniert werden, z.B. Storm und HBase in einem Cluster. Wenn für Ihre Lösung Technologien erforderlich sind, die auf mehrere HDInsight-Clustertypen verteilt sind, können Sie die erforderlichen Clustertypen über ein [virtuelles Azure-Netzwerk](https://docs.microsoft.com/azure/virtual-network) miteinander verbinden. 
>
>

| Clustertyp | Funktionalität |
| --- | --- |
| [Hadoop](../../hdinsight/hadoop/apache-hadoop-introduction.md) |Batch-Abfragen und -analysen gespeicherter Daten |
| [HBase](../../hdinsight/hbase/apache-hbase-overview.md) |Verarbeitung großen Mengen von schemalosen NoSQL-Daten |
| [Interactive Query](../../hdinsight/interactive-query/apache-interactive-query-get-started.md) |Interaktive und schnellere Hive-Abfragen durch speicherinternes Caching |
| [Kafka](../../hdinsight/kafka/apache-kafka-introduction.md) | Dies ist eine verteilte Open-Source-Streamingplattform, die zum Erstellen von Datenpipelines und Anwendungen mit Echtzeitstreaming verwendet werden kann. |
| [R Server](../../hdinsight/r-server/r-server-overview.md) |Verschiedene Funktionen für Big Data-Statistiken, Vorhersagemodellierung und Machine Learning |
| [Spark](../../hdinsight/spark/apache-spark-overview.md) |Arbeitsspeicherinterne Verarbeitung, interaktive Abfragen, Microbatch-Datenstromverarbeitung |
| [Storm](../../hdinsight/storm/apache-storm-overview.md) |Ereignisverarbeitung in Echtzeit |

### <a name="hdinsight-version"></a>HDInsight-Version

Wählen Sie die Version von HDInsight für diesen Cluster aus. Weitere Informationen finden Sie unter [Unterstützte HDInsight-Versionen](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="enterprise-security-package"></a>Sicherheitspaket für Unternehmen

Für Cluster der Typen Hadoop, Spark und Interactive Query können Sie das **Sicherheitspaket für Unternehmen** aktivieren. Dieses Paket bietet die Möglichkeit, mithilfe von Apache Ranger und der Integration in Azure Active Directory eine sicherere Clustereinrichtung zu erreichen. Weitere Informationen finden Sie unter [Sicherheitspaket für Unternehmen in Azure HDInsight](../../hdinsight/domain-joined/apache-domain-joined-introduction.md).

![HDInsight-Erstellungsoptionen – Sicherheitspaket für Unternehmen auswählen](./media/quickstart-create-connect-hdi-cluster/hdinsight-creation-enterprise-security-package.png)

Weitere Informationen zum Erstellen eines in eine Domäne eingebundenen HDInsight-Clusters finden Sie unter [Erstellen einer in eine Domäne eingebundenen HDInsight-Sandboxumgebung](../../hdinsight/domain-joined/apache-domain-joined-configure.md).

## <a name="cluster-login-and-ssh-user-name"></a>Clusteranmeldung und SSH-Benutzername

Während der Clustererstellung ermöglichen die HDInsight-Cluster Ihnen das Konfigurieren von zwei Benutzerkonten:

* HTTP-Benutzer: Der Standardbenutzername ist *admin*. Für ihn gilt im Azure-Portal die Standardkonfiguration. Er wird auch als „Clusterbenutzer“ bezeichnet.
* SSH-Benutzer (Linux-Cluster): Dient dem Herstellen einer Verbindung mit dem Cluster über SSH. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Mit dem Sicherheitspaket für Unternehmen können Sie HDInsight mit Active Directory und Apache Ranger integrieren. Mithilfe des Enterprise-Sicherheitspakets können mehrere Benutzer erstellt werden.

## <a name="location"></a>Orte (Regionen) für Cluster und Speicher

Sie müssen den Clusterstandort nicht explizit angeben, denn der Cluster befindet sich am selben Ort wie der Standardspeicher. Um eine Liste der unterstützten Regionen zu erhalten, klicken Sie in der Dropdownliste **Region** auf [HDInsight-Preise](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Speicherendpunkte für Cluster

Obwohl eine lokale Installation von Hadoop das Hadoop Distributed File System (HDFS) für die Speicherung im Cluster verwendet, nutzen Sie in der Cloud Speicherendpunkte, die mit dem Cluster verbunden sind. HDInsight-Cluster verwenden entweder [Data Lake Storage Gen2](abfs-driver.md) oder [Blobs in Azure Storage](../../hdinsight/hdinsight-hadoop-use-blob-storage.md). Durch Verwendung von Azure Storage oder Data Lake Storage können Sie die für Berechnungen verwendeten HDInsight-Cluster sicher löschen und gleichzeitig Ihre Daten beibehalten.

> [!WARNING]
> Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.

Während der Konfiguration geben Sie als Standardspeicherendpunkt Data Lake Storage an. Der Standardspeicher enthält Anwendungs- und Systemprotokolle. Optional können Sie zusätzliche verknüpfte Azure Data Lake Storage-Konten angeben, auf die der Cluster zugreifen kann. Der HDInsight-Cluster und die abhängigen Speicherkonten müssen sich an demselben Azure-Standort befinden.

![Clusterspeichereinstellungen: HDFS-kompatible Speicherendpunkte](media/quickstart-create-connect-hdi-cluster/hdinsight-cluster-creation-storage2.png)

> [!IMPORTANT]
> Vergessen Sie nicht, den **Zugriff auf Data Lake Store zu deaktivieren**. Diese Einstellungen beziehen sich auf die alte *Data Lake Store*-Funktionalität und müssen deaktiviert werden, damit *Data Lake Storage*-Funktionen einwandfrei arbeiten.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="optional-metastores"></a>Optionale Metastores
Sie können optionale Hive- oder Oozie Metastores erstellen. Allerdings unterstützen nicht alle Clustertypen Metastores, und Azure SQL Data Warehouse ist nicht mit Metastores kompatibel. 

Weitere Informationen finden Sie unter [Verwenden von externen Metadatenspeichern in Azure HDInsight](../../hdinsight/hdinsight-use-external-metadata-stores.md).

> [!IMPORTANT]
> Verwenden Sie beim Erstellen eines benutzerdefinierten Metastores keinen Datenbanknamen, der Gedankenstriche, Bindestriche oder Leerzeichen enthält. Dies kann dazu führen, dass der Clustererstellungsprozess fehlschlägt.

### <a name="use-hiveoozie-metastore"></a>Hive-Metastore

Verwenden Sie einen benutzerdefinierten Metastore, wenn Sie nach dem Löschen des HDInsight-Clusters Ihre Hive-Tabellen beibehalten möchten. Sie können diesen Metastore anschließend an einen anderen HDInsight-Cluster anfügen.

Ein HDInsight-Metastore, der für eine HDInsight-Clusterversion erstellt wurde, kann nicht über verschiedene HDInsight-Clusterversionen freigegeben werden. Eine Liste mit den HDInsight-Versionen finden Sie unter [Unterstützte HDInsight-Versionen](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie-Metastore

Verwenden Sie zur Erhöhung der Leistung bei Verwendung von Oozie einen benutzerdefinierten Metastore. Ein Metastore kann auch Zugriff auf Oozie-Auftragsdaten bieten, nachdem Sie Ihren Cluster gelöscht haben. 

> [!IMPORTANT]
> Ein benutzerdefinierter Oozie-Metastore kann nicht wiederverwendet werden. Wenn Sie einen benutzerdefinierten Oozie-Metastore verwenden möchten, müssen Sie beim Erstellen des HDInsight-Clusters eine leere Azure SQL-Datenbank bereitstellen.

## <a name="configure-cluster-size"></a>Konfigurieren der Clustergröße

Die Verwendung der Knoten wird so lange abgerechnet, wie der Cluster vorhanden ist. Die Abrechnung beginnt, sobald ein Cluster erstellt wurde, und sie endet, wenn der Cluster gelöscht wird. Bei Clustern ist kein Aufheben der Zuweisung oder ein Anhalten möglich.


### <a name="number-of-nodes-for-each-cluster-type"></a>Anzahl der Knoten für jeden Clustertyp
Jeder Clustertyp verfügt über eine eigene Anzahl von Knoten, Terminologie für Knoten und eine VM-Standardgröße. In der folgenden Tabelle ist die Anzahl von Knoten für jeden Knotentyp jeweils in Klammern angegeben.

| Typ | Nodes | Diagramm |
| --- | --- | --- |
| Hadoop |Hauptknoten (2), Datenknoten (1+) |![HDInsight-Hadoop-Clusterknoten](media/quickstart-create-connect-hdi-cluster/hdinsight-hadoop-cluster-type-nodes.png) |
| hbase |Hauptserver (2), Regionsserver (1+), Master-/Zookeeper-Knoten (3) |![HDInsight-HBase-Clusterknoten](media/quickstart-create-connect-hdi-cluster/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus-Knoten (2), Supervisor-Server (1+), Zookeeper-Knoten (3) |![HDInsight-Storm-Clusterknoten](media/quickstart-create-connect-hdi-cluster/hdinsight-storm-cluster-type-setup.png) |
| Spark |Hauptknoten (2), Workerknoten (1+), Zookeeper-Knoten (3) (kostenlos für Zookeeper-VM-Größe A1) |![HDInsight-Spark-Clusterknoten](media/quickstart-create-connect-hdi-cluster/hdinsight-spark-cluster-type-setup.png) |

Weitere Informationen finden Sie unter [Standardknotenkonfiguration und VM-Größen für Cluster](../../hdinsight/hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) in „Welche Hadoop-Komponenten und -Versionen gibt es in HDInsight?“.

Die Kosten von HDInsight-Clustern ergeben sich aus der Anzahl der Knoten und aus der Größe der virtuellen Computer für die Knoten. 

Unterschiedliche Clustertypen weisen verschiedene Knotentypen, eine unterschiedliche Anzahl von Knoten sowie verschiedene Knotengrößen auf:
* Hadoop-Standardclustertyp: 
    * Zwei *Hauptknoten*  
    * Vier *Datenknoten*
* Storm-Standardclustertyp: 
    * Zwei *Nimbusknoten*
    * Drei *Zookeeper-Knoten*
    * Vier *Supervisorknoten* 

Wenn Sie HDInsight gerade testen, sollten Sie nur einen Datenknoten verwenden. Weitere Informationen zu den Preisen von HDInsight finden Sie unter [HDInsight – Preise](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> Die Begrenzung der Clustergröße variiert je nach Azure-Abonnement. Wenden Sie sich an den [Azure-Abrechnungssupport](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request), um diese Begrenzung zu erhöhen.
>

Wenn Sie das Azure-Portal zum Konfigurieren des Clusters verwenden, ist die Knotengröße über das Blatt **Knotentarife** verfügbar. Außerdem werden im Portal die Kosten angezeigt, die den unterschiedlichen Knotengrößen zugeordnet sind. 

![VM-Größen für HDInsight-Knoten](media/quickstart-create-connect-hdi-cluster/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Größen virtueller Computer 
Wählen Sie bei der Bereitstellung von Clustern die Computeressourcen basierend auf der Lösung aus, die Sie bereitstellen möchten. Für HDInsight-Cluster werden die folgenden virtuellen Computer verwendet:
* Virtuelle Computer der Serien A und D1–4: [Linux-VM-Größen für allgemeine Zwecke](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* Virtueller Computer der Serien D11–14: [Arbeitsspeicheroptimierte Linux-VM-Größen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Wenn Sie wissen möchten, welchen Wert Sie beim Erstellen eines Clusters mithilfe unterschiedlicher SDKs oder von Azure PowerShell als VM-Größe angeben sollten, lesen Sie unter [VM-Größen für HDInsight-Cluster](../../cloud-services/cloud-services-sizes-specs.md#size-tables) nach. Verwenden Sie im verknüpften Artikel in den Tabellen den Wert in der Spalte **Größe**.

> [!IMPORTANT]
> Wenn Sie mehr als 32 Workerknoten benötigen, müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen.
>
>

Weitere Informationen finden Sie unter [Größen für virtuelle Computer](../../virtual-machines/windows/sizes.md). Informationen zu den Preisen der unterschiedlichen Größen finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Benutzerdefinierte Clustereinrichtung
Die benutzerdefinierte Clustereinrichtung basiert auf den Einstellungen der Schnellerfassung mit den folgenden zusätzlichen Optionen:
- [HDInsight-Anwendungen](#hdinsight-applications)
- [Clustergröße](#cluster-size)
- Erweiterte Einstellungen
  - [Skriptaktionen](#customize-clusters-using-script-action)
  - [Virtuelles Netzwerk](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>Installieren von HDInsight-Anwendungen in Clustern

Eine HDInsight-Anwendung kann von Benutzern in einem Linux-basierten HDInsight-Cluster installiert werden. Sie können Anwendungen von Microsoft, Anwendungen von Drittanbietern oder selbst entwickelte Anwendungen verwenden. Weitere Informationen finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](../../hdinsight/hdinsight-apps-install-applications.md).

Die meisten HDInsight-Anwendungen werden auf einem leeren Edgeknoten installiert.  Ein leerer Edgeknoten ist ein virtueller Linux-Computer, auf dem die gleichen Clienttools installiert und konfiguriert sind wie im Hauptknoten. Sie können den Edgeknoten zum Zugreifen auf den Cluster sowie zum Testen und Hosten Ihrer Clientanwendungen verwenden. Weitere Informationen finden Sie unter [Use empty edge nodes in HDInsight](../../hdinsight/hdinsight-apps-use-edge-node.md)(Verwenden leerer Edgeknoten in HDInsight).

## <a name="advanced-settings-script-actions"></a>Erweiterte Einstellungen: Skriptaktionen

Sie können zusätzliche Komponenten installieren oder die Clusterkonfiguration mithilfe von Skripts während der Erstellung anpassen. Diese Skripts werden mithilfe der Konfigurationsoption **Skriptaktion**aufgerufen, die vom Azure-Verwaltungsportal, von HDInsight Windows PowerShell-Cmdlets oder dem HDInsight .NET SDK verwendet werden kann. Weitere Informationen finden Sie unter [Anpassen eines HDInsight-Clusters mithilfe von Skriptaktionen](../../hdinsight/hdinsight-hadoop-customize-cluster-linux.md).

Einige systemeigene Java-Komponenten wie Mahout und Cascading können auf dem Cluster als JAR-Dateien (Java Archive) ausgeführt werden. Diese JAR-Dateien können an Azure Storage oder Azure Data Lake Storage verteilt und mit den Verfahren zur Übermittlung von Hadoop-Aufträgen an HDInsight-Cluster gesendet werden. Weitere Informationen finden Sie unter [Programmgesteuerte Übermittlung von Hadoop-Aufträgen](../../hdinsight/hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]
> Wenn bei der Bereitstellung von JAR-Dateien für HDInsight-Cluster oder beim Aufrufen von JAR-Dateien für HDInsight-Cluster Probleme auftreten, wenden Sie sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/).
>
> Cascading wird von HDInsight nicht unterstützt, und es steht kein Microsoft-Support dafür zur Verfügung. Listen der unterstützten Komponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Clusterversionen](../../hdinsight/hdinsight-component-versioning.md).
>
>

Es kann vorkommen, dass Sie die folgenden Konfigurationsdateien während des Erstellungsprozesses bearbeiten möchten:

* clusterIdentity.xml
* core-site.xml
* gateway.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](../../hdinsight/hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Erweiterte Einstellungen: Erweitern von Cluster mit einem virtuellen Netzwerk
Wenn für Ihre Lösung Technologien erforderlich sind, die auf mehrere HDInsight-Clustertypen verteilt sind, können Sie die erforderlichen Clustertypen über ein [virtuelles Azure-Netzwerk](https://docs.microsoft.com/azure/virtual-network) miteinander verbinden. Durch diese Konfiguration können die Cluster und der gesamte Code, den Sie dafür bereitstellen, direkt miteinander kommunizieren.

Weitere Informationen zur Verwendung eines virtuellen Azure-Netzwerks finden Sie unter [Erweitern von HDInsight mit virtuellen Azure-Netzwerken](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md).

Ein Beispiel für die Verwendung von zwei Clustertypen in einem virtuellen Azure-Netzwerk finden Sie unter [Verwenden von strukturiertem Spark-Streaming mit Kafka](../../hdinsight/hdinsight-apache-kafka-spark-structured-streaming.md). Weitere Informationen zur Verwendung von HDInsight mit einem virtuellen Netzwerk, einschließlich spezifischer Konfigurationsanforderungen für das virtuelle Netzwerk, finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Beheben von Problemen mit der Zugriffssteuerung

Falls beim Erstellen von HDInsight-Clustern Probleme auftreten, sehen Sie sich die [Voraussetzungen für die Zugriffssteuerung](../../hdinsight/hdinsight-administer-use-portal-linux.md#create-clusters) an.

## <a name="next-steps"></a>Nächste Schritte

- [ABFS-Hadoop-Dateisystemtreiber für Azure Data Lake Storage Gen2](abfs-driver.md)
- [Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight](tutorial-extract-transform-load-hive.md)
- [Was sind HDInsight, das Hadoop-Ökosystem und Hadoop-Cluster?](../../hdinsight/hadoop/apache-hadoop-introduction.md)
- [Erste Schritte mit Hadoop in HDInsight](../../hdinsight/hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Verwenden eines Windows-Computers mit Hadoop in HDInsight](../../hdinsight/hdinsight-hadoop-windows-tools.md)
