---
title: Hadoop Oozie-Workflows in mit der Domäne verknüpften HDInsight-Clustern
description: Verwenden von Hadoop Oozie im mit der Domäne verknüpften, Linux-basierten HDInsight-Enterprise-Sicherheitspaket. Erfahren Sie, wie Sie einen Oozie-Workflow definieren und einen Oozie-Auftrag übermitteln können.
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: omidm
ms.openlocfilehash: 928f6adbb348683a110f7da9b20efaae998290ca
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972212"
---
#<a name="run-apache-oozie-in-domain-joined-hdinsight-hadoop-clusters"></a>Ausführen von Apache Oozie in mit der Domäne verknüpften HDInsight-Hadoop-Clustern
Oozie ist ein Workflow- und Koordinationssystem zur Verwaltung von Hadoop-Aufträgen. Oozie ist in den Hadoop-Stack integriert und unterstützt die folgenden Aufträge:
- Apache MapReduce
- Apache Pig
- Apache Hive
- Apache Sqoop

Sie können Oozie auch dazu verwenden, bestimmte Aufträge für ein System zu planen, beispielsweise Java-Programme oder Shellskripts.

##<a name="prerequisites"></a>Voraussetzungen:
- Ein mit der Domäne verknüpfter HDInsight-Hadoop-Cluster. Weitere Informationen finden Sie unter [Configure Domain-joined HDInsight clusters (Preview)](./apache-domain-joined-configure-using-azure-adds.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern [Vorschau]).

    > [!NOTE]
    > Detaillierte Anweisungen zum Verwenden von Oozie auf nicht mit der Domäne verknüpften Clustern finden Sie [hier](../hdinsight-use-oozie-linux-mac.md)

##<a name="connecting-to-domain-joined-cluster"></a>Herstellen einer Verbindung mit einem in die Domäne eingebundenen Cluster
Weitere Informationen zu SSH finden Sie unter [Authentication: Domain-joined HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) (Authentifizierung: HDInsight mit Domänenverknüpfung).
- Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her:
     ```bash
    ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
    ```
Um zu überprüfen, ob die Kerberos-Authentifizierung erfolgreich war, verwenden Sie den `klist`-Befehl. Falls nicht, verwenden Sie `kinit`, um die Kerberos-Authentifizierung einzuleiten.

- Melden Sie sich beim HDInsight-Gateway an, um das oAuth-Token zu registrieren, das für den Zugriff auf den Azure Data Lake Store (ADLS) erforderlich ist
     ```bash
     curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
     ```

    Der Statusantwortcode _200 OK_ zeigt die erfolgreiche Registrierung an. Überprüfen Sie Benutzername und Kennwort, wenn eine Unberechtigt-Antwort (401) empfangen wird.

## <a name="define-the-workflow"></a>Definieren des Workflows
Oozie-Workflowdefinitionen sind in der Sprache der Hadoop-Prozessdefinition (hPDL) geschrieben. Dabei handelt es sich um eine Sprache der XML-Prozessdefinition. Führen Sie zum Definieren des Workflows die folgenden Schritte aus:

-   Einrichten des Arbeitsbereichs des Domänenbenutzers:
 ```bash
hdfs dfs -mkdir /user/<DomainUser>
cd /home/<DomainUserPath>
cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
tar -xvf oozie-examples.tar.gz
hdfs dfs -put examples /user/<DomainUser>/
 ```
Ersetzen Sie `DomainUser` durch den Namen des Domänenbenutzers. Ersetzen Sie `DomainUserPath` durch den Pfad des Stammverzeichnisses für den Domänenbenutzer. Ersetzen Sie `ClusterVersion` durch Ihre Cluster-HDP-Version.

-   Verwenden Sie die folgende Anweisung zum Erstellen und Bearbeiten einer neuen Datei:
 ```bash
nano workflow.xml
 ```

- Nachdem der Nano-Editor geöffnet wurde, verwenden Sie den folgenden XML-Code als Inhalt der Datei:
 ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <workflow-app xmlns="uri:oozie:workflow:0.4" name="map-reduce-wf">
       <credentials>
          <credential name="metastore_token" type="hcat">
             <property>
                <name>hcat.metastore.uri</name>
                <value>thrift://hn0-<clustername>.<Domain>.com:9083</value>
             </property>
             <property>
                <name>hcat.metastore.principal</name>
                <value>hive/_HOST@<Domain>.COM</value>
             </property>
          </credential>
          <credential name="hs2-creds" type="hive2">
             <property>
                <name>hive2.server.principal</name>
                <value>${jdbcPrincipal}</value>
             </property>
             <property>
                <name>hive2.jdbc.url</name>
                <value>${jdbcURL}</value>
             </property>
          </credential>
       </credentials>
       <start to="mr-test" />
       <action name="mr-test">
          <map-reduce>
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <prepare>
                <delete path="${nameNode}/user/${wf:user()}/examples/output-data/mrresult" />
             </prepare>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
                <property>
                   <name>mapred.mapper.class</name>
                   <value>org.apache.oozie.example.SampleMapper</value>
                </property>
                <property>
                   <name>mapred.reducer.class</name>
                   <value>org.apache.oozie.example.SampleReducer</value>
                </property>
                <property>
                   <name>mapred.map.tasks</name>
                   <value>1</value>
                </property>
                <property>
                   <name>mapred.input.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/input-data/text</value>
                </property>
                <property>
                   <name>mapred.output.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/output-data/mrresult</value>
                </property>
             </configuration>
          </map-reduce>
          <ok to="myHive2" />
          <error to="fail" />
       </action>
       <action name="myHive2" cred="hs2-creds">
          <hive2 xmlns="uri:oozie:hive2-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <jdbc-url>${jdbcURL}</jdbc-url>
             <script>${hiveScript2}</script>
             <param>hiveOutputDirectory2=${hiveOutputDirectory2}</param>
          </hive2>
          <ok to="myHive" />
          <error to="fail" />
       </action>
       <action name="myHive" cred="metastore_token">
          <hive xmlns="uri:oozie:hive-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
             </configuration>
             <script>${hiveScript1}</script>
             <param>hiveOutputDirectory1=${hiveOutputDirectory1}</param>
          </hive>
          <ok to="end" />
          <error to="fail" />
       </action>
       <kill name="fail">
          <message>Oozie job failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
       </kill>
       <end name="end" />
    </workflow-app>
 ```
Ersetzen Sie `clustername` durch den Namen des Clusters. 

Um die Datei zu speichern, drücken Sie STRG + X, geben Sie `Y` ein, und drücken Sie auf die **EINGABETASTE**.

Der Workflow ist in zwei Teile gegliedert:
*   Anmeldeinformationsabschnitt: Der Anmeldeinformationsabschnitt nimmt die Anmeldeinformationen auf, die für die Authentifizierung von Oozie-Aktionen verwendet werden:

    In diesem Beispiel wird die Authentifizierung für Hive-Aktionen verwendet. Weitere Informationen finden Sie [hier]( https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html).

    Der Anmeldeinformationsdienst ermöglicht es Oozie-Aktionen, mithilfe von Identitätswechsel für den Zugriff auf Hadoop-Dienste den Benutzer zu verkörpern.

*   Aktionsabschnitt: Hier gibt es drei Aktionen – map-reduce (Zuordnen-Reduzieren), die Hive-Server 2-Aktion und die Hive Server-1-Aktion:

    map-reduce führt ein Beispiel aus dem Oozie-Paket für map-reduce aus, das die aggregierte Wortanzahl ausgibt.

    Die Hive-Server 2- und Hive-Server 1-Aktionen führen eine einfache Abfrage in der mit HDInsight gelieferten Hivesample-Tabelle aus.

    Die Hive-Aktionen verwenden die im Anmeldeinformationsabschnitt definierten Anmeldeinformationen für die Authentifizierung mithilfe des Schlüsselworts `cred` im Aktionselement

- Kopieren Sie mit folgendem Befehl die Datei `workflow.xml` nach `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`:
     ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
     ```

    Ersetzen Sie `domainuser` durch Ihren Benutzernamen für die Domäne.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Definieren der Eigenschaftendatei für den Oozie-Auftrag

1.  Verwenden Sie die folgende Anweisung zum Erstellen und Bearbeiten einer neuen Datei für Auftragseigenschaften:
     ```bash
    nano job.properties
     ```

2.   Nachdem der Nano-Editor geöffnet wurde, verwenden Sie den folgenden XML-Code als Inhalt der Datei:

    ```bash
        nameNode=adl://home
        jobTracker=headnodehost:8050
        queueName=default
        examplesRoot=examples
        oozie.wf.application.path=${nameNode}/user/[domainuser]/examples/apps/map-reduce/workflow.xml
        hiveScript1=${nameNode}/user/${user.name}/countrowshive1.hql
        hiveScript2=${nameNode}/user/${user.name}/countrowshive2.hql
        oozie.use.system.libpath=true
        user.name=[domainuser]
        jdbcPrincipal=hive/hn0-<ClusterShortName>.<Domain>.com@<Domain>.COM
        jdbcURL=[jdbcurlvalue]
        hiveOutputDirectory1=${nameNode}/user/${user.name}/hiveresult1
        hiveOutputDirectory2=${nameNode}/user/${user.name}/hiveresult2
    ```
    

   * Ersetzen Sie `domainuser` durch Ihren Benutzernamen für die Domäne.
   * Ersetzen Sie `ClusterShortName` durch den ShortName für den Cluster. Wenn der Clustername https://sechadoopcontoso.azurehdisnight.net ist, besteht der `clustershortname` aus den ersten sechs Buchstaben für den Cluster: sechad
   * Ersetzen Sie `jdbcurlvalue` durch die JDBC-URL aus der Hive-Konfiguration. Beispiel: jdbc:hive2://headnodehost:10001/;transportMode=http.
    
   * Um die Datei zu speichern, drücken Sie STRG + X, geben Sie `Y` ein, und drücken Sie auf die **EINGABETASTE**.

   * Diese Eigenschaftendatei muss bei der Ausführung von Oozie-Aufträgen lokal vorhanden sein

## <a name="creating-custom-hive-scripts-for-the-oozie-job"></a>Erstellen benutzerdefinierter Hive-Skripts für den Oozie-Auftrag
Die zwei Hive-Skripts für Hive-Server 1 und Hive-Server 2 können in folgender Weise erstellt werden:
-   Datei für Hive-Server 1:
1.  Verwenden Sie die folgende Anweisung, um eine Datei für eine Hive-Server 1-Aktion zu erstellen und zu bearbeiten:
    ```bash
    nano countrowshive1.hql
    ```

2.  Erstellen Sie das Skript.
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemake from hivesampletable limit 2;
    ```

3.  Speichern Sie die Datei in Hdfs.
    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

-   Datei für Hive-Server 2:
1.  Verwenden Sie die folgende Anweisung, um eine Datei für die Hive-Server 2-Aktion zu erstellen und zu bearbeiten:
    ```bash
    nano countrowshive2.hql
    ```

2.  Erstellen Sie das Skript.
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3.  Speichern Sie die Datei in Hdfs.
    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submission-of-oozie-jobs"></a>Übermittlung von Oozie-Aufträgen
Das Übermitteln von Oozie-Aufträgen für in die Domäne eingebundene Cluster erfolgt ähnlich wie das Übermitteln von Oozie-Aufträgen an nicht mit einer Domäne verknüpfte Cluster.

Weitere Informationen finden Sie unter [Übermitteln und Verwalten des Auftrags](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-oozie-job-submission"></a>Ergebnisse der Oozie-Auftragsübermittlung
Da Oozie-Aufträge im Auftrag des Benutzers ausgeführt werden, werden die Aufträge sowohl in Yarn- als auch in Ranger-Überwachungsprotokollen als vom imitierten Benutzer ausgeführt angezeigt. Die CLI-Ausgabe des Oozie-Auftrags sieht wie unten dargestellt aus:


```bash
    Job ID : 0000015-180626011240801-oozie-oozi-W
    ------------------------------------------------------------------------------------------------
    Workflow Name : map-reduce-wf
    App Path      : adl://home/user/alicetest/examples/apps/map-reduce/wf.xml
    Status        : SUCCEEDED
    Run           : 0
    User          : alicetest
    Group         : -
    Created       : 2018-06-26 19:25 GMT
    Started       : 2018-06-26 19:25 GMT
    Last Modified : 2018-06-26 19:30 GMT
    Ended         : 2018-06-26 19:30 GMT
    CoordAction ID: -
    
    Actions
    ------------------------------------------------------------------------------------------------
    ID                      Status  Ext ID          ExtStatus   ErrCode
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@:start:    OK  -           OK      -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@mr-test    OK  job_1529975666160_0051  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive2    OK  job_1529975666160_0053  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive OK  job_1529975666160_0055  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@end    OK  -           OK      -
    -----------------------------------------------------------------------------------------------
```

*    Die Ranger-Überwachungsprotokolle für Hive-Server 2-Aktionen zeigen die Ausführung durch Oozie im Auftrag des Benutzers. Die Ranger- und Yarn-Ansicht ist nur für den Clusteradministrator verfügbar.

## <a name="configuration-of-user-authorization-in-oozie"></a>Konfiguration der Benutzerautorisierung in Oozie
Oozie verfügt seinerseits über eine Konfiguration zur Benutzerautorisierung, die das Beenden oder zwangsweise Beenden von Aufträgen anderer Benutzer durch Benutzer blockieren kann. Diese wird durch Festlegen von `oozie.service.AuthorizationService.security.enabled` auf `true` aktiviert. 

Weitere Details hierzu finden Sie in der Oozie-Dokumentation im Abschnitt [User Authorization Configuration]( https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html) (Konfiguration der Benutzerautorisierung):

Für Komponenten wie Hive-Server 1, für die das Ranger-Plug-In nicht verfügbar ist bzw. nicht unterstützt wird, ist nur die grob abgestufte HDFS-Autorisierung möglich. Eine fein abgestufte Autorisierung ist nur mithilfe von Ranger-Plug-Ins möglich.

## <a name="oozie-web-ui"></a>Oozie-Webbenutzeroberfläche
Die Oozie-Webbenutzeroberfläche bietet eine webbasierte Anzeige des Status von Oozie-Aufträgen im Cluster. Auf in eine Domäne eingebundenen Clustern müssen dies ausführen:

1. Fügen Sie einen [Edgeknoten](../hdinsight-apps-use-edge-node.md) hinzu, und aktivieren Sie [SSH Kerberos-Authentifizierung](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. Führen Sie die unter [Oozie-Web-UI](../hdinsight-use-oozie-linux-mac.md) beschriebenen Schritte aus, um das Tunneln von SSH zum Edgeknoten zu aktivieren und auf das Web-UI zuzugreifen.

## <a name="next-steps"></a>Nächste Schritte
* [Verwenden von Oozie mit Hadoop zum Definieren und Ausführen eines Workflows in Linux-basiertem Azure HDInsight](../hdinsight-use-oozie-linux-mac.md)
* [Verwenden des zeitbasierten Oozie-Koordinators](../hdinsight-use-oozie-coordinator-time.md)
* [Ausführen von Hive-Abfragen per SSH für in eine Domäne eingebundene HDInsight-Cluster](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
