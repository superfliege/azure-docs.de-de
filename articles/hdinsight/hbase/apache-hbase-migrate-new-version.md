---
title: Migrieren eines HBase-Clusters zu einer neuen Version – Azure HDInsight | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie HBase-Cluster zu einer neuen Version migrieren.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 2ab0bc792777b73f878bc1728c0d8310ecf41167
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34165093"
---
# <a name="migrate-an-hbase-cluster-to-a-new-version"></a>Migrieren eines HBase-Clusters zu einer neuen Version

Auftragsbasierte Cluster wie Spark und Hadoop lassen sich sehr einfach upgraden (wie unter [Aktualisieren eines HDInsight-Clusters auf eine neuere Version](../hdinsight-upgrade-cluster.md) beschrieben):

1. Sichern Sie flüchtige (lokal gespeicherte) Daten.
2. Löschen Sie den vorhandenen Cluster.
3. Erstellen Sie einen neuen Cluster im gleichen VNet-Subnetz.
4. Importieren Sie die flüchtigen Daten.
5. Starten Sie Aufträge, und setzen Sie die Verarbeitung im neuen Cluster fort.

Für das Upgrade eines HBase-Clusters sind einige zusätzliche Schritte erforderlich. Diese werden im vorliegenden Artikel beschrieben.

> [!NOTE]
> Die Downtime beim Upgrade beträgt in der Regel nur wenige Minuten. Sie ist auf das Leeren sämtlicher In-Memory-Daten und auf die Konfiguration und den Neustart der Dienste im neuen Cluster zurückzuführen. Die Ergebnisse variieren je nach Kontenanzahl, Datenmenge und anderen Variablen.

## <a name="review-hbase-compatibility"></a>Überprüfen der HBase-Kompatibilität

Vergewissern Sie sich vor dem HBase-Upgrade, dass die HBase-Versionen im Quell- und Zielcluster kompatibel sind. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md).

> [!NOTE]
> Wir empfehlen dringend, die Kompatibilitätsmatrix im [HBase-Buch](https://hbase.apache.org/book.html#upgrading) zu prüfen.

Hier sehen Sie ein Beispiel für eine Versionskompatibilitätsmatrix, wobei „J“ für Kompatibilität und „N“ für potenzielle Inkompatibilität steht:

| Kompatibilitätstyp | Hauptversion| Nebenversion | Patch |
| --- | --- | --- | --- |
| Client-Server-Verbindungskompatibilität | N | J | J |
| Server-Server-Kompatibilität | N | J | J |
| Dateiformatkompatibilität | N | J | J |
| Client-API-Kompatibilität | N | J | J |
| Client-Binärkompatibilität | N | N | J |
| **Serverseitig eingeschränkte API-Kompatibilität** |  |  |  |
| Stable | N | J | J |
| In Entwicklung | N | N | J |
| Instabil | N | N | N |
| Abhängigkeitskompatibilität | N | J | J |
| Betriebskompatibilität | N | N | J |

> [!NOTE]
> Kritische Inkompatibilitäten sollten in den Versionshinweisen für HBase-Version beschrieben sein.

## <a name="upgrade-with-same-hbase-major-version"></a>Upgraden mit der gleichen HBase-Hauptversion

Im folgenden Szenario wird ein Upgrade von HDInsight 3.4 auf 3.6 (jeweils enthalten in Apache HBase 1.1.2) mit der gleichen HBase-Hauptversion durchgeführt. Andere Versionsupgrades laufen ähnlich ab – vorausgesetzt, es liegen keine Kompatibilitätsprobleme zwischen Quell- und Zielversion vor.

1. Vergewissern Sie sich anhand der HBase-Kompatibilitätsmatrix und der Versionshinweise, dass Ihre Anwendung mit der neuen Version kompatibel ist. Testen Sie Ihre Anwendung in einem Cluster mit der Zielversion von HDInsight und HBase.

2. [Richten Sie einen neuen HDInsight-Zielcluster ein](../hdinsight-hadoop-provision-linux-clusters.md), und verwenden Sie dabei das gleiche Speicherkonto, aber einen anderen Containernamen:

    ![Verwenden des gleichen Speicherkontos, aber mit einem anderen Container](./media/apache-hbase-migrate-new-version/same-storage-different-container.png)

3. Leeren Sie Ihren HBase-Quellcluster. Dies ist der Cluster, den Sie upgraden möchten. HBase schreibt eingehende Daten in einen In-Memory-Speicher. Dieser wird als _Memstore_ bezeichnet. Wenn der Memstore eine bestimmte Größe erreicht, wird er zur langfristigen Speicherung im Speicherkonto des Clusters auf den Datenträger geschrieben. Wenn Sie den alten Cluster löschen, werden die Memstores recycelt, wodurch ggf. Daten verloren gehen. Führen Sie das folgende Skript aus, um den Memstore für jede Tabelle manuell auf den Datenträger zu schreiben. Die neueste Version dieses Skripts finden Sie auf [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/flush_all_tables.sh) für Azure.

    ```bash
    #!/bin/bash
    
    #-------------------------------------------------------------------------------#
    # SCRIPT TO FLUSH ALL HBASE TABLES.
    #-------------------------------------------------------------------------------#
    
    LIST_OF_TABLES=/tmp/tables.txt
    HBASE_SCRIPT=/tmp/hbase_script.txt
    TARGET_HOST=$1
    
    usage ()
    {
        if [[ "$1" == "-h" ]] || [[ "$1" == "--help" ]]
        then
            cat << ...
    
    Usage: 
    
    $0 [hostname]
    
    Providing hostname is optional and not required when the script is executed within HDInsight cluster with access to 'hbase shell'.
    
    However hostname should be provided when executing the script as a script-action from HDInsight portal.
    
    For Example:
    
        1.  Executing script inside HDInsight cluster (where 'hbase shell' is 
            accessible):
    
            $0 
    
            [No need to provide hostname]
    
        2.  Executing script from HDinsight Azure portal:
    
            Provide Script URL.
    
            Provide hostname as a parameter (i.e. hn0, hn1 or wn2 etc.).
    ...
            exit
        fi
    }
    
    validate_machine ()
    {
        THIS_HOST=`hostname`
    
        if [[ ! -z "$TARGET_HOST" ]] && [[ $THIS_HOST  != $TARGET_HOST* ]]
        then
            echo "[INFO] This machine '$THIS_HOST' is not the right machine ($TARGET_HOST) to execute the script."
            exit 0
        fi
    }
    
    get_tables_list ()
    {
    hbase shell << ... > $LIST_OF_TABLES 2> /dev/null
        list
        exit
    ...
    }
    
    add_table_for_flush ()
    {
        TABLE_NAME=$1
        echo "[INFO] Adding table '$TABLE_NAME' to flush list..."
        cat << ... >> $HBASE_SCRIPT
            flush '$TABLE_NAME'
    ...
    }
    
    clean_up ()
    {
        rm -f $LIST_OF_TABLES
        rm -f $HBASE_SCRIPT
    }
    
    ########
    # MAIN #
    ########
    
    usage $1
    
    validate_machine
    
    clean_up
    
    get_tables_list
    
    START=false
    
    while read LINE 
    do 
        if [[ $LINE == TABLE ]] 
        then
            START=true
            continue
        elif [[ $LINE == *row*in*seconds ]]
        then
            break
        elif [[ $START == true ]]
        then
            add_table_for_flush $LINE
        fi
    
    done < $LIST_OF_TABLES
    
    cat $HBASE_SCRIPT
    
    hbase shell $HBASE_SCRIPT << ... 2> /dev/null
    exit
    ...
    
    ```
    
4. Beenden Sie die Erfassung im alten HBase-Cluster.
5. Führen Sie das vorherige Skript erneut aus, um sicherzustellen, dass alle aktuellen Daten im Memstore geleert wurden.
6. Melden Sie sich im alten Cluster (https://OLDCLUSTERNAME.azurehdidnsight.net)) bei Ambari an, und beenden Sie die HBase-Dienste. Wenn Sie aufgefordert werden, zu bestätigen, dass Sie die Dienste beenden möchten, aktivieren Sie das Kontrollkästchen zum Aktivieren des Wartungsmodus für HBase. Weitere Informationen zur Verbindungsherstellung mit Ambari sowie zur Verwendung finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](../hdinsight-hadoop-manage-ambari.md).

    ![Klicken Sie in Ambari auf die Registerkarte „Services“ (Dienste), im linken Bereich auf „HBase“ und anschließend unter „Service Actions“ (Dienstaktionen) auf „Stop“ (Beenden).](./media/apache-hbase-migrate-new-version/stop-hbase-services.png)

    ![Aktivieren Sie das Kontrollkästchen zum Aktivieren des Wartungsmodus für HBase, und bestätigen Sie den Vorgang.](./media/apache-hbase-migrate-new-version/turn-on-maintenance-mode.png)

7. Melden Sie sich im neuen HDInsight-Cluster bei Ambari an. Ändern Sie die HDFS-Einstellung `fs.defaultFS` so, dass sie auf den Containernamen verweist, der im ursprünglichen Cluster verwendet wurde. Diese Einstellung befindet sich unter **HDFS > Configs > Advanced > Advanced core-site** (HDFS > Konfigurationen > Erweitert > core-site (erweitert)).

    ![Klicken Sie in Ambari auf die Registerkarte „Services“ (Dienste), im linken Menü auf „HDFS“, anschließend auf die Registerkarte „Configs“ (Konfigurationen) und darunter auf die Registerkarte „Advanced“ (Erweitert).](./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png)

    ![Ändern Sie den Containernamen in Ambari.](./media/apache-hbase-migrate-new-version/change-container-name.png)

8. Speichern Sie die Änderungen.
9. Starten Sie alle erforderlichen Dienste neu, wie durch Ambari angegeben.
10. Richten Sie für Ihre Anwendung einen Verweis auf den neuen Cluster ein.

    > [!NOTE]
    > Bei einem Upgrade ändert sich der statische DNS-Name für Ihre Anwendung. Sie können in den DNS-Einstellungen Ihres Domänennamens einen CNAME-Eintrag konfigurieren, der auf den Namen des Clusters verweist, anstatt einen hartcodierten DNS-Namen zu verwenden. Eine weitere Möglichkeit ist die Verwendung einer Konfigurationsdatei für Ihre Anwendung, die Sie ohne erneute Bereitstellung aktualisieren können.

11. Starten Sie die Erfassung, um zu prüfen, ob alles wie erwartet funktioniert.
12. Wenn der neue Cluster Ihren Vorstellungen entspricht, löschen Sie den ursprünglichen Cluster.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu HBase und zum Upgraden von HDInsight-Clustern finden Sie in den folgenden Artikeln:

* [Aktualisieren eines HDInsight-Clusters auf eine neuere Version](../hdinsight-upgrade-cluster.md)
* [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](../hdinsight-hadoop-manage-ambari.md)
* [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md)
* [Optimieren von Konfigurationen mit Ambari](../hdinsight-changing-configs-via-ambari.md#hbase-optimization-with-the-ambari-web-ui)
