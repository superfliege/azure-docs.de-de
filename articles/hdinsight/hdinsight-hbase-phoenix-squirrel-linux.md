---
title: Verwenden von Apache Phoenix und SQLLine mit HBase in Azure HDInsight | Microsoft-Dokumentation
description: "Informationen zum Verwenden von Apache Phoenix in HDInsight. Darüber hinaus erhalten Sie Informationen zum Installieren und Einrichten von SQLLine für die Verbindung mit einem HBase-Cluster in HDInsight auf Ihrem Computer."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: cda0f33b-a2e8-494c-972f-ae0bb482b818
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/22/2017
ms.author: jgao
ms.openlocfilehash: 41bfef95059b7c5343d952b4fe372e3baae6f9d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Verwenden von Apache Phoenix mit Linux-basierten HBase-Clustern in HDInsight
Erfahren Sie, wie Sie [Apache Phoenix](http://phoenix.apache.org/) in Azure HDInsight und wie Sie SQLLine verwenden können. Weitere Informationen zu Phoenix finden Sie unter [Phoenix innerhalb von 15 Minuten oder weniger](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Eine Beschreibung der Phoenix-Grammatik finden Sie unter [Phoenix Grammar](http://phoenix.apache.org/language/index.html) (Phoenix-Grammatik).

> [!NOTE]
> Versionshinweise für Phoenix finden Sie in HDInsight unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Verwendung von SQLLine
[SQLLine](http://sqlline.sourceforge.net/) ist ein Befehlszeilenprogramm zum Ausführen von SQL.

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie SQLLine verwenden können, benötigen Sie folgende Elemente:

* **Ein HBase-Cluster in HDInsight**. Eine Beschreibung der Erstellung finden Sie unter [Erste Schritte mit Apache HBase in HDInsight](./hdinsight-hbase-tutorial-get-started.md).

Beim Herstellen der Verbindung mit einem HBase-Cluster müssen Sie die Verbindung mit einer der Zookeeper-VMs herstellen. Jeder HDInsight-Cluster verfügt über drei Zookeeper-VMs.

**So rufen Sie den Hostnamen des Zookeepers ab**

1. Öffnen Sie Ambari, indem Sie zu **https://\<Clustername\>.azurehdinsight.net** navigieren.
2. Geben Sie zum Anmelden den HTTP-Benutzernamen (Clusternamen) und das Kennwort ein.
3. Wählen Sie im Menü auf der linken Seite die Option **Zookeeper** aus. Drei **Zookeeper Server**-Instanzen sind aufgeführt.
4. Wählen Sie eine der **Zookeeper Server**-Instanzen aus. Suchen Sie im Bereich **Summary** (Zusammenfassung) den **Hostname** (Hostnamen). Er entspricht *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Verwendung von SQLLine**

1. Die Verbindungsherstellung mit dem Cluster erfolgt über SSH. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Führen Sie in SSH die folgenden Befehle aus, um SQLLine auszuführen:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ZOOKEEPER SERVER FQDN>:2181:/hbase-unsecure
3. Um eine HBase-Tabelle zu erstellen und Daten einzufügen, führen Sie die folgenden Befehle aus:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Weitere Informationen finden Sie im [SQLLine-Handbuch](http://sqlline.sourceforge.net/#manual) sowie unter [Phoenix Grammar](http://phoenix.apache.org/language/index.html) (Phoenix-Grammatik).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie Apache Phoenix in HDInsight verwenden. Weitere Informationen finden Sie in diesen Artikeln:

* [Übersicht über HDInsight HBase][hdinsight-hbase-overview].
  HBase ist eine Open Source-NoSQL-Datenbank von Apache, die auf Hadoop basiert und wahlfreien Zugriff und starke Konsistenz für große Mengen unstrukturierter und teilstrukturierter Daten bietet.
* [Erstellen von HBase-Clustern in HDInsight in Azure Virtual Network][hdinsight-hbase-provision-vnet].
  Mit der Integration in virtuelle Netzwerke können HBase-Cluster im selben virtuellen Netzwerk bereitgestellt werden wie Ihre Anwendungen, sodass Anwendungen direkt mit HBase kommunizieren können.
* [Konfigurieren der HBase-Replikation in HDInsight](hdinsight-hbase-replication.md). Erfahren Sie, wie Sie HBase-Replikation zwischen zwei Azure-Rechenzentren einrichten.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
