---
title: Skalieren von Clustergrößen – Azure HDInsight
description: Skalieren Sie einen HDInsight-Cluster gemäß Ihrer Workload.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/02/2018
ms.author: ashish
ms.openlocfilehash: 3e664fc83fde937b26a4726f997da4c0cb4d8f8a
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407880"
---
# <a name="scale-hdinsight-clusters"></a>Skalieren von HDInsight-Clustern

HDInsight bietet Flexibilität, indem Sie die Anzahl der Workerknoten in Ihren Clustern zentral hoch- und herunterskalieren können. So können Sie einen Cluster nach den Geschäftsstunden oder am Wochenende verkleinern und während der Spitzenbelastungen erweitern.

Wenn Sie beispielsweise einmal täglich oder monatlich eine Batchverarbeitung durchführen, kann der HDInsight-Cluster ein paar Minuten vor diesem geplanten Ereignis zentral hochskaliert werden, damit ausreichend Arbeitsspeicher und CPU-Rechenleistung zur Verfügung stehen. Sie können die Skalierung mit dem PowerShell-Cmdlet [`Set–AzureRmHDInsightClusterSize`](hdinsight-administer-use-powershell.md#scale-clusters) automatisieren.  Später, wenn nach der Verarbeitung die Nutzung wieder sinkt, können Sie den HDInsight-Cluster auf weniger Workerknoten zentral herunterskalieren.

* So skalieren Sie Ihren Cluster über [PowerShell](hdinsight-administer-use-powershell.md):

    ```powershell
    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    ```
    
* So skalieren Sie Ihren Cluster über die [klassische Azure CLI](hdinsight-administer-use-command-line.md):

    ```
    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
    ```

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]
    
* Um Ihren Cluster über das [Azure-Portal](https://portal.azure.com) zu skalieren, öffnen Sie Ihren HDInsight-Clusterbereich, wählen Sie im linken Menü **Cluster skalieren**, geben Sie dann im Fenster „Cluster skalieren“ die Anzahl der Workerknoten ein, und wählen Sie „Speichern“ aus.

    ![Skalieren von Clustern](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Mit jeder dieser Methoden können Sie Ihren HDInsight-Cluster innerhalb von Minuten zentral hoch- oder herunterskalieren.

## <a name="scaling-impacts-on-running-jobs"></a>Auswirkungen der Skalierung auf die Ausführung von Aufträgen

Wenn Sie Ihrem ausgeführten HDInsight-Cluster Knoten **hinzufügen**, sind ausstehende oder ausgeführte Aufträge nicht betroffen. Darüber hinaus können neue Aufträge sicher übermittelt werden, während der Skalierungsprozess ausgeführt wird. Wenn bei der Skalierung aus beliebigem Grund ein Fehler auftritt, wird der Fehler ordnungsgemäß behandelt, sodass der Cluster funktionsfähig bleibt.

Wenn Sie jedoch Knoten **entfernen**, um Ihren Cluster zentral herunter zu skalieren, tritt bei ausstehenden oder ausgeführten Aufträgen ein Fehler auf, wenn die Skalierung abgeschlossen ist. Der Grund für diesen Fehler ist, dass einige Dienste während des Prozesses neu gestartet werden.

Um dieses Problem zu beheben, können Sie warten, bis die Aufträge abgeschlossen sind, bevor Sie Ihren Cluster zentral herunterskalieren, die Aufträge manuell beenden oder sie nach Abschluss der Skalierung erneut übermitteln.

Um eine Liste der ausstehenden und ausgeführten Aufträge anzuzeigen, können Sie mit folgenden Schritten die YARN ResourceManager-Benutzeroberfläche verwenden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im linken Menü **Durchsuchen** und **HDInsight-Cluster** aus, und wählen Sie dann Ihren Cluster aus.
3. Wählen Sie in Ihrem HDInsight-Clusterbereich **Dashboard** im oberen Menü aus, um die Ambari-Benutzeroberfläche zu öffnen. Geben Sie Ihre Clusteranmeldeinformationen ein.
4. Klicken Sie in der Liste mit den Diensten im linken Menü auf **YARN**. Wählen Sie auf der Seite „YARN“ **Quick Links** aus, zeigen Sie auf den aktiven Hauptknoten, und klicken Sie auf **ResourceManager UI** (Ressourcen-Manager-Benutzeroberfläche).

    ![ResourceManager UI](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

Sie können direkt mit `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster` auf die ResourceManager UI zugreifen.

Eine Liste der Aufträge wird zusammen mit ihren aktuellen Status angezeigt. Im Screenshot wird derzeit ein Auftrag ausgeführt:

![ResourceManager UI-Anwendungen](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Um die ausgeführte Anwendung manuell zu beenden, führen Sie den folgenden Befehl der SSH-Shell aus:

```bash
yarn application -kill <application_id>
```

Beispiel: 

```bash
yarn application -kill "application_1499348398273_0003"
```

## <a name="rebalancing-an-apache-hbase-cluster"></a>Erstellen eines Apache HBase-Clusters

Regionsserver werden innerhalb weniger Minuten nach Abschluss des Skalierungsvorgangs automatisch ausgeglichen. Um Regionsserver manuell auszugleichen, führen Sie folgende Schritte aus:

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Starten Sie die HBase-Shell:

        hbase shell

3. Um die Regionsserver manuell auszugleichen, führen Sie folgenden Befehl aus:

        balancer

## <a name="scale-down-implications"></a>Auswirkungen des zentralen Herunterskalierens

Wie bereits erwähnt, werden alle ausstehenden oder ausgeführten Aufträge nach dem Abschluss des zentralen Herunterskalierens beendet. Es können jedoch andere potenzielle Auswirkungen des zentralen Herunterskalierens auftreten.

## <a name="hdinsight-name-node-stays-in-safe-mode-after-scaling-down"></a>Der HDInsight-Namensknoten bleibt nach dem zentralen Herunterskalieren im abgesicherten Modus

![Skalieren von Clustern](./media/hdinsight-scaling-best-practices/scale-cluster.png)

Wenn Sie, wie in der vorherigen Abbildung dargestellt, Ihren Cluster auf das Minimum von einem Workerknoten verkleinern, kann Apache HDFS im abgesicherten Modus hängen bleiben, wenn Workerknoten aufgrund des Patchens oder unmittelbar nach der Skalierung neu gestartet werden.

Die primäre Ursache ist, dass Hive ein paar `scratchdir`-Dateien verwendet und standardmäßig drei Replikate jedes Blocks erwartet, aber es ist nur ein Replikat möglich, wenn Sie auf das Minimum von einem Workerknoten zentral herunterskalieren. Daher sind die Dateien im `scratchdir` *unterrepliziert*. Dies kann HDFS veranlassen, nach dem Neustart der Dienste nach dem Skalierungsvorgang im abgesicherten Modus zu bleiben.

Wenn versucht wird, zentral herunter zu skalieren, verlässt sich HDInsight darauf, dass die Apache Ambari-Verwaltungsoberflächen zuerst die zusätzlichen unerwünschten Workerknoten außer Betrieb setzen, sodass deren HDFS-Blöcke auf andere Workerknoten repliziert werden, die online sind, und dann den Cluster sicher zentral herunterskalieren. HDFS wechselt während des Wartungsfensters in einen abgesicherten Modus, und es wird erwartet, dass HDFS diesen nach dem Skalieren verlässt. Genau an diesem Punkt kann HDFS im abgesicherten Modus hängen bleiben.

HDFS ist mit der `dfs.replication`-Einstellung 3 konfiguriert. Daher sind die Blöcke der Scratchdateien immer dann unterrepliziert, wenn weniger als drei Workerknoten online sind, da nicht die erwarteten drei Kopien jedes Dateiblocks verfügbar sind.

Sie können einen Befehl ausführen, um HDFS aus dem abgesicherten Modus zu holen. Wenn Sie z.B. wissen, dass der abgesicherte Modus nur aktiviert ist, weil die temporären Dateien unterrepliziert sind, können Sie den abgesicherten Modus ohne Bedenken verlassen. Der Grund ist, dass die unterreplizierten Dateien temporäre Hive-Scratchdateien sind.

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

Nach dem Verlassen des abgesicherten Modus können Sie die temporären Dateien manuell entfernen oder darauf warten, dass Hive sie letztendlich automatisch bereinigt.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Beispiel für Fehler bei aktiviertem abgesichertem Modus

* H070 Hive-Sitzung kann nicht geöffnet werden. org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **Verzeichnis kann nicht erstellen werden:** /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. **Namensknoten befindet sich im abgesicherten Modus**. Die gemeldeten 75 Blöcke benötigen weitere 12 Blöcke, um den Schwellenwert 0,9900 von insgesamt 87 Blöcken zu erreichen. Die Anzahl 10 der aktiven Datenknoten hat die minimale Anzahl 0 erreicht. Der abgesicherte Modus wird automatisch deaktiviert, sobald die Schwellenwerte erreicht sind.

* H100 Anweisung zum Anzeigen der Datenbanken kann nicht übermittelt werden: org.apache.thrift.transport.TTransportException: org.apache.http.conn.HttpHostConnectException: Fehler beim Herstellen der Verbindung mit hn0-clustername.servername.internal.cloudapp.net:10001 [hn0-clustername.servername. internal.cloudapp.net/1.1.1.1]: **Verbindung verweigert**

* H020 Verbindung mit hn0-hdisrv.servername.bx.internal.cloudapp.net:10001: org.apache.thrift.transport.TTransportException konnte nicht hergestellt werden: HTTP-Verbindung mit http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/ konnte nicht erstellt werden. org.apache.http.conn.HttpHostConnectException: Fehler beim Herstellen der Verbindung mit hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28]: Verbindung verweigert: org.apache.thrift.transport.TTransportException: HTTP-Verbindung mit http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/ konnte nicht erstellt werden. org.apache.http.conn.HttpHostConnectException: Fehler beim Herstellen der Verbindung mit hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28]: **Verbindung verweigert**

* Aus den Hive-Protokollen: WARN [main]: server.HiveServer2 (HiveServer2.java:startHiveServer2(442)) – Fehler beim Starten von HiveServer2 bei Versuch 21, java.lang.RuntimeException wird in 60 Sekunden erneut versucht: Fehler beim Anwenden der Autorisierungsrichtlinie auf die Hive-Konfiguration: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **Verzeichnis kann nicht erstellen werden:** /tmp/hive/hive/70a42b8a-9437-466e-acbe-da90b1614374. **Namensknoten befindet sich im abgesicherten Modus**.
    Die gemeldeten 0 Blöcke benötigen weitere 9 Blöcke, um den Schwellenwert 0,9900 von insgesamt 9 Blöcken zu erreichen.
    Die Anzahl 10 der aktiven Datenknoten hat die minimale Anzahl 0 erreicht. **Der abgesicherte Modus wird automatisch deaktiviert, sobald die Schwellenwerte erreicht sind**.
    bei org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1324)

Sie können die Namensknotenprotokolle aus dem `/var/log/hadoop/hdfs/`-Ordner überprüfen, die nahe am Zeitpunkt der Clusterskalierung liegen, um festzustellen, wann der Wechsel in den abgesicherten Modus erfolgte. Die Protokolldateien werden mit `Hadoop-hdfs-namenode-hn0-clustername.*` benannt.

Die Grundursache für die vorherigen Fehler ist, dass Hive während der Ausführung von Abfragen von temporären Dateien in HDFS abhängig ist. Wenn HDFS in den abgesicherten Modus wechselt, kann Hive keine Abfragen ausführen, weil Hive nicht in HDFS schreiben kann. Die temporären Dateien in HDFS befinden sich auf dem lokalen Laufwerk, das in einzelne Workerknoten-VMs eingebunden ist, und werden mit mindestens drei Replikaten zwischen anderen Workerknoten repliziert.

Der `hive.exec.scratchdir`-Parameter in Hive ist innerhalb von `/etc/hive/conf/hive-site.xml` konfiguriert:

```xml
<property>
    <name>hive.exec.scratchdir</name>
    <value>hdfs://mycluster/tmp/hive</value>
</property>
```

### <a name="view-the-health-and-state-of-your-hdfs-file-system"></a>Anzeigen von Integrität und Status Ihres HDFS-Dateisystems

Sie können einen Statusbericht zu jedem Namensknoten anzeigen, um festzustellen, ob Knoten sich im abgesicherten Modus befinden. Um den Bericht anzuzeigen, stellen Sie mit SSH eine Verbindung zu jedem Hauptknoten her, und führen Sie den folgenden Befehl aus:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode get
```

![Deaktivierter abgesicherter Modus](./media/hdinsight-scaling-best-practices/safe-mode-off.png)

> [!NOTE]  
> Der `-D`-Schalter ist erforderlich, da das Standarddateisystem in HDInsight entweder Azure Storage oder Azure Data Lake Store ist. `-D` gibt an, dass die Befehle im lokalen HDFS-Dateisystem ausgeführt werden.

Als Nächstes können Sie einen Bericht anzeigen, der die Details des HDFS-Status enthält:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -report
```

In einem fehlerfreien Cluster, in dem alle Blöcke im erwarteten Ausmaß repliziert werden, ergibt dieser Befehl Folgendes:

![Deaktivierter abgesicherter Modus](./media/hdinsight-scaling-best-practices/report.png)

HDFS unterstützt den `fsck`-Befehl zur Überprüfung auf Inkonsistenzen bei verschiedenen Dateien, z.B. fehlende Blöcke bei einer Datei oder unterreplizierte Blöcke. So führen Sie den `fsck`-Befehl für die `scratchdir`-Dateien (temporäre Scratch-Datenträgerdateien) aus:

```
hdfs fsck -D 'fs.default.name=hdfs://mycluster/' /tmp/hive/hive
```

Bei Ausführung in einem fehlerfreien HDFS-Dateisystem ohne unterreplizierte Blöcke sehen Sie eine Ausgabe ähnlich der folgenden:

```
Connecting to namenode via http://hn0-scalin.name.bx.internal.cloudapp.net:30070/fsck?ugi=sshuser&path=%2Ftmp%2Fhive%2Fhive
FSCK started by sshuser (auth:SIMPLE) from /10.0.0.21 for path /tmp/hive/hive at Thu Jul 06 20:07:01 UTC 2017
..Status: HEALTHY
 Total size:    53 B
 Total dirs:    5
 Total files:   2
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2 (avg. block size 26 B)
 Minimally replicated blocks:   2 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          4
 Number of racks:               1
FSCK ended at Thu Jul 06 20:07:01 UTC 2017 in 3 milliseconds


The filesystem under path '/tmp/hive/hive' is HEALTHY
```

Wenn der `fsck`-Befehl im Gegensatz dazu auf einem HDFS-Dateisystem mit einigen unterreplizierten Blöcke ausgeführt wird, ähnelt die Ausgabe der folgenden:

```
Connecting to namenode via http://hn0-scalin.name.bx.internal.cloudapp.net:30070/fsck?ugi=sshuser&path=%2Ftmp%2Fhive%2Fhive
FSCK started by sshuser (auth:SIMPLE) from /10.0.0.21 for path /tmp/hive/hive at Thu Jul 06 20:13:58 UTC 2017
.
/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info:  Under replicated BP-1867508080-10.0.0.21-1499348422953:blk_1073741826_1002. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s) and 0 decommissioning replica(s).
.
/tmp/hive/hive/e7c03964-ff3a-4ee1-aa3c-90637a1f4591/inuse.info: CORRUPT blockpool BP-1867508080-10.0.0.21-1499348422953 block blk_1073741825

/tmp/hive/hive/e7c03964-ff3a-4ee1-aa3c-90637a1f4591/inuse.info: MISSING 1 blocks of total size 26 B.Status: CORRUPT
 Total size:    53 B
 Total dirs:    5
 Total files:   2
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2 (avg. block size 26 B)
  ********************************
  UNDER MIN REPL'D BLOCKS:      1 (50.0 %)
  dfs.namenode.replication.min: 1
  CORRUPT FILES:        1
  MISSING BLOCKS:       1
  MISSING SIZE:         26 B
  CORRUPT BLOCKS:       1
  ********************************
 Minimally replicated blocks:   1 (50.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       1 (50.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     0.5
 Corrupt blocks:                1
 Missing replicas:              2 (33.333332 %)
 Number of data-nodes:          1
 Number of racks:               1
FSCK ended at Thu Jul 06 20:13:58 UTC 2017 in 28 milliseconds


The filesystem under path '/tmp/hive/hive' is CORRUPT
```

Sie können den HDFS-Status auch in der Ambari-Benutzeroberfläche anzeigen, indem Sie auf der linken Seite den **HDFS**-Dienst auswählen, oder mit `https://<HDInsightClusterName>.azurehdinsight.net/#/main/services/HDFS/summary`.

![Ambari-HDFS-Status](./media/hdinsight-scaling-best-practices/ambari-hdfs.png)

Möglicherweise werden auch einer oder mehrere kritische Fehler bei den aktiven oder Standby-NameNodes angezeigt. Wählen Sie zum Anzeigen der Integrität der NameNode-Blöcke den NameNode-Link neben der Warnung aus.

![Integrität der NameNode-Blöcke](./media/hdinsight-scaling-best-practices/ambari-hdfs-crit.png)

Um die Scratchdateien zu bereinigen, wodurch die Blockreplikationsfehler entfernt werden, stellen Sie mit SSH eine Verbindung zu jedem Hauptknoten her, und führen Sie den folgenden Befehl aus:

```
hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
```

> [!NOTE]  
> Mit diesem Befehl kann Hive unterbrochen werden, falls noch einige Aufträge ausgeführt werden.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode-due-to-under-replicated-blocks"></a>Gewusst wie: Verhindern, dass HDInsight aufgrund unterreplizierter Blöcke im abgesicherten Modus hängen bleibt

Es gibt mehrere Möglichkeiten, zu verhindern, dass HDInsight im abgesicherten Modus hängen bleibt:

* Beenden Sie alle Hive-Aufträge, bevor Sie HDInsight zentral herunterskalieren. Alternativ können Sie das zentrale Herunterskalieren planen, um Konflikte mit der Ausführung von Hive-Aufträgen zu verhindern.
* Bereinigen Sie vor dem zentralen Herunterskalieren manuell die `tmp`-Scratchverzeichnisdateien von Hive in HDFS.
* Skalieren Sie HDInsight nur auf minimal drei Workerknoten zentral herunter. Vermeiden Sie es, auf nur einen Workerknoten herunter zu gehen.
* Führen Sie bei Bedarf den Befehl zum Verlassen des abgesicherten Modus aus.

In den folgenden Abschnitten werden diese Optionen beschrieben.

#### <a name="stop-all-hive-jobs"></a>Beenden aller Hive-Aufträge

Beenden Sie alle Hive-Aufträge, bevor Sie einen Workerknoten zentral herunterskalieren. Wenn Ihre Workload geplant ist, führen Sie Ihre zentrale Herunterskalierung nach Abschluss der Hive-Arbeit durch.

Dadurch minimieren Sie ggf. die Anzahl der Scratchdateien im tmp-Ordner.

#### <a name="manually-clean-up-hives-scratch-files"></a>Manuelles Bereinigen der Hive-Scratchdateien

Wenn Hive temporäre Dateien zurückgelassen hat, können Sie diese Dateien vor dem zentralen Herunterskalieren manuell bereinigen, um den abgesicherten Modus zu vermeiden.

1. Beenden Sie die Hive-Dienste, und achten Sie darauf, dass alle Abfragen und Aufträge abgeschlossen sind.

2. Listen Sie den Inhalt des `hdfs://mycluster/tmp/hive/`-Verzeichnisses auf, um festzustellen, ob es alle Dateien enthält:

    ```
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```
    
    Dies ist eine Beispielausgabe für den Fall, dass Dateien vorhanden sind:

    ```
    sshuser@hn0-scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Wenn Sie wissen, dass Hive diese Dateien nicht mehr benötigt, können Sie sie entfernen. Vergewissern Sie sich auf der ResourceManager UI-Seite von Yarn, dass keine Hive-Abfragen mehr ausgeführt werden.

    Beispielbefehlszeile zum Entfernen von Dateien aus HDFS:

    ```
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```
    
#### <a name="scale--hdinsight-to-three-worker-nodes"></a>Skalieren von HDInsight auf drei Workerknoten

Wenn das Hängenbleiben im abgesicherten Modus ein Dauerproblem ist, und die vorherigen Schritte keine Optionen sind, können Sie das Problem vielleicht vermeiden, indem Sie nur auf drei Workerknoten zentral herunterskalieren. Dies ist möglicherweise im Vergleich zum zentralen Herunterskalieren auf einen Knoten aus Kostengründen nicht optimal. Allerdings kann HDFS mit nur einem Workerknoten nicht garantieren, dass dem Cluster drei Replikate der Daten zur Verfügung stehen.

#### <a name="run-the-command-to-leave-safe-mode"></a>Ausführen des Befehls zum Verlassen des abgesicherten Modus

Die letzte Option ist, den seltenen Fall zu beobachten, dass HDFS in den abgesicherten Modus wechselt, und dann den Befehl zum Verlassen des abgesicherten Modus auszuführen. Nachdem Sie festgestellt haben, dass der Grund für den Wechsel von HDFS in den abgesicherten Modus ist, dass die Hive-Dateien unterrepliziert sind, führen Sie den folgenden Befehl zum Verlassen des abgesicherten Modus aus:

* HDInsight unter Linux:

    ```bash
    hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
    ```
    
* HDInsight unter Windows:

    ```bash
    hdfs dfsadmin -fs hdfs://headnodehost:9000 -safemode leave
    ```
    
## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure HDInsight](hadoop/apache-hadoop-introduction.md)
* [Skalieren von Clustern](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md)
