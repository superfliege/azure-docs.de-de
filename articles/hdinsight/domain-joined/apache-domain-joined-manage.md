---
title: "Verwalten von in die Domäne eingebundenen HDInsight-Clustern – Azure | Microsoft-Dokumentation"
description: "Informationen zum Verwalten von in die Domäne eingebundenen HDInsight-Clustern"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2018
ms.author: saurinsh
ms.openlocfilehash: 6a43ea602052b9b3338567571075742adc5a3ca0
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="manage-domain-joined-hdinsight-clusters"></a>Verwalten von in die Domäne eingebundenen HDInsight-Clustern
Hier erhalten Sie Informationen zu den Benutzern und den Rollen in HDInsight mit Domänenverknüpfung sowie zum Verwalten von in die Domäne eingebundenen HDInsight-Clustern.

## <a name="access-the-clusters-with-enterprise-security-package"></a>Greifen Sie mit dem Enterprise Security Package auf die Cluster zu.

Das Enterprise Security Package (bisher als HDInsight Premium bezeichnet) ermöglicht den Zugriff auf den Cluster durch mehrere Benutzer, wobei die Authentifizierung mit Active Directory und die Autorisierung mit Apache Ranger und Storage-ACLs (ADLS-ACLs) durchgeführt wird. Mit der Autorisierung wird für sichere Grenzen zwischen mehreren Benutzern gesorgt, und nur berechtigte Benutzer haben basierend auf den Autorisierungsrichtlinien Zugriff auf die Daten.

Die Sicherheit und die Benutzerisolation sind für einen HDInsight-Cluster mit Enterprise Security Package wichtig. Zur Erfüllung dieser Anforderungen ist der SSH-Zugriff auf den Cluster mit Enterprise Security Package blockiert. In der folgenden Tabelle sind die empfohlenen Zugriffsmethoden für die einzelnen Clustertypen aufgeführt:

|Workload|Szenario|Zugriffsmethode|
|--------|--------|-------------|
|Hadoop|Hive – Interaktive Aufträge/Abfragen |<ul><li>[Beeline](#beeline)</li><li>[Struktur anzeigen](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio-Tools](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Interaktive Aufträge/Abfragen, PySpark interaktiv|<ul><li>[Beeline](#beeline)</li><li>[Zeppelin mit Livy](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Struktur anzeigen](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio-Tools](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Batchszenarien – Spark-Submit, PySpark|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
|Interaktive Abfrage (LLAP)|Interactive|<ul><li>[Beeline](#beeline)</li><li>[Struktur anzeigen](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio-Tools](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Beliebig|Installieren der benutzerdefinierten Anwendung|<ul><li>[Skriptaktionen](../hdinsight-hadoop-customize-cluster-linux.md)</li></ul>|


Aus Sicht der Sicherheit ist die Verwendung der Standard-APIs hilfreich. Außerdem kommen Sie in den Genuss der folgenden Vorteile:

1.  **Verwaltung**: Sie können Ihren Code verwalten und Aufträge automatisieren, indem Sie Standard-APIs nutzen, z.B. Livy, HS2 usw.
2.  **Überwachung**: Mit SSH kann nicht überwacht werden, welche Benutzer per SSH auf den Cluster zugegriffen haben. Dies ist nicht der Fall, wenn Aufträge über Standardendpunkte erstellt werden, da sie dann im Kontext des Benutzers ausgeführt werden. 



### <a name="beeline"></a>Verwenden von Beeline 
Installieren Sie Beeline auf Ihrem Computer, und stellen Sie eine Verbindung über das öffentliche Internet her, indem Sie die folgenden Parameter verwenden: 

```
- Connection string: -u 'jdbc:hive2://&lt;clustername&gt;.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'
- Cluster login name: -n admin
- Cluster login password -p 'password'
```

Wenn Sie Beeline lokal installiert haben und eine Verbindung über ein virtuelles Azure-Netzwerk herstellen, verwenden Sie die folgenden Parameter: 

```
- Connection string: -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Verwenden Sie die Informationen im Dokument „Verwalten von HDInsight mithilfe der Ambari-REST-API“, um den vollqualifizierten Domänennamen eines Hauptknotens zu ermitteln.














## <a name="users-of-domain-joined-hdinsight-clusters"></a>Benutzer von in die Domäne eingebundenen HDInsight-Clustern
Ein HDInsight-Cluster, der nicht in die Domäne eingebunden ist, verfügt über zwei Benutzerkonten, die während der Clustererstellung erstellt werden:

* **Ambari-Administrator:** Dieses Konto wird auch als *Hadoop-Benutzer* oder *HTTP-Benutzer* bezeichnet. Dieses Konto kann für die Anmeldung bei Ambari unter https://&lt;Clustername>.azurehdinsight.net verwendet werden. Es kann auch zum Ausführen von Abfragen für Ambari-Ansichten, zum Ausführen von Aufträgen über externe Tools (z.B. PowerShell, Templeton, Visual Studio) und für die Authentifizierung mit dem Hive ODBC-Treiber und BI-Tools (z.B. Excel, Power BI oder Tableau) verwendet werden.
* **SSH-Benutzer:** Dieses Konto kann mit SSH und zum Ausführen von sudo-Befehlen verwendet werden. Es verfügt über Rootberechtigungen für die virtuellen Linux-Computer.

Ein in die Domäne eingebundener HDInsight-Cluster verfügt neben Ambari-Administrator und SSH-Benutzer über drei neue Benutzer.

* **Ranger-Administrator:** Dieses Konto ist das lokale Apache Ranger-Administratorkonto. Es handelt sich um keinen Active Directory-Domänenbenutzer. Dieses Konto kann verwendet werden, um Richtlinien einzurichten und andere Benutzer als Administratoren oder delegierte Administratoren festzulegen (sodass diese Benutzer Richtlinien verwalten können). Der Benutzername lautet standardmäßig *admin*, und das Kennwort ist identisch mit dem Ambari-Administratorkennwort. Das Kennwort kann auf der Seite „Settings“ (Einstellungen) in Ranger aktualisiert werden.
* **Domänenbenutzer des Clusteradministrators:** Dieses Konto ist ein Active Directory-Domänenbenutzer, der als Hadoop-Clusteradministrator (einschließlich Ambari und Ranger) festgelegt ist. Sie müssen die Anmeldeinformationen dieses Benutzers während der Clustererstellung angeben. Dieser Benutzer verfügt über die folgenden Berechtigungen:

  * Einbinden von Computern in die Domäne und Platzieren dieser Computer in der Organisationseinheit, die Sie während der Clustererstellung angeben.
  * Erstellen von Dienstprinzipalen in der Organisationseinheit, die Sie während der Clustererstellung angeben.
  * Erstellen von Reverse-DNS-Einträgen.

    Beachten Sie, dass die anderen AD-Benutzer auch über diese Berechtigungen verfügen.

    Es gibt einige Endpunkte innerhalb des Clusters (z.B. Templeton), die nicht von Ranger verwaltet werden und daher nicht sicher sind. Diese Endpunkte sind für alle Benutzer außer dem Domänenbenutzer des Clusteradministrators gesperrt.
* **Normal:** Während der Clustererstellung können Sie mehrere Active Directory-Gruppen angeben. Die Benutzer in diesen Gruppen werden mit Ranger und Ambari synchronisiert. Diese Benutzer sind Domänenbenutzer und haben nur Zugriff auf über Ranger verwaltete Endpunkte (z.B. Hiveserver2). Alle RBAC-Richtlinien und die Überwachung gelten für diese Benutzer.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Rollen von in die Domäne eingebundenen HDInsight-Clustern
In die Domäne eingebundene HDInsight-Cluster weisen die folgenden Rollen auf:

* Clusteradministrator
* Clusteroperator
* Dienstadministrator
* Dienstoperator
* Clusterbenutzer

**So zeigen Sie die Berechtigungen dieser Rollen an**

1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Roles** (Rollen).
3. Klicken Sie auf das blaue Fragezeichen, um die Berechtigungen anzuzeigen:

    ![Berechtigungen für Rollen von in die Domäne eingebundenen HDInsight-Clustern](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Öffnen der Ambari-Verwaltungsoberfläche

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Öffnen Sie Ihren HDInsight-Cluster. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klicken Sie im Hauptmenü auf **Dashboard**, um Ambari zu öffnen.
4. Melden Sie sich mit dem Domänenbenutzernamen und dem Kennwort des Clusteradministrators bei Ambari an.
5. Klicken Sie oben rechts auf das Dropdownmenü **Admin** und dann auf **Manage Ambari** (Ambari verwalten).

    ![HDInsight mit Domänenverknüpfung – Verwalten von Ambari](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Die Benutzeroberfläche sieht wie folgt aus:

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Auflisten der über Active Directory synchronisierten Domänenbenutzer
1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Benutzer**. Es werden alle Benutzer angezeigt, die über Active Directory mit dem HDInsight-Cluster synchronisiert wurden.

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche – Auflisten der Benutzer](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Auflisten der über Active Directory synchronisierten Domänengruppen
1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Groups** (Gruppen). Es werden alle Gruppen angezeigt, die über Active Directory mit dem HDInsight-Cluster synchronisiert wurden.

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche – Auflisten der Gruppen](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Konfigurieren von Berechtigungen für Hive-Ansichten
1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Views** (Ansichten).
3. Klicken Sie auf **HIVE**, um die Details anzuzeigen.

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche – Hive-Ansichten](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klicken Sie auf den Link **Hive View** (Hive-Ansicht), um Hive-Ansichten zu konfigurieren.
5. Scrollen Sie nach unten zum Abschnitt **Permissions** (Berechtigungen).

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche – Hive-Ansichten – Konfigurieren von Berechtigungen](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klicken Sie auf **Add User** (Benutzer hinzufügen) oder **Add Group** (Gruppe hinzufügen), und geben Sie dann die Benutzer oder Gruppen an, die Hive-Ansichten verwenden können.

## <a name="configure-users-for-the-roles"></a>Konfigurieren von Benutzern für die Rollen
 Eine Aufstellung der Rollen und der zugehörigen Berechtigungen finden Sie unter [Rollen von in die Domäne eingebundenen HDInsight-Clustern](#roles-of-domain---joined-hdinsight-clusters).

1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Roles** (Rollen).
3. Klicken Sie auf **Add User** (Benutzer hinzufügen) oder **Add Group** (Gruppe hinzufügen), um Benutzer und Gruppen zu verschiedenen Rollen zuzuweisen.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](apache-domain-joined-configure.md).
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](apache-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Ausführen von Hive-Abfragen per SSH für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
