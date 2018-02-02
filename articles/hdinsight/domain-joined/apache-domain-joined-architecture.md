---
title: "In die Domäne eingebundene Azure HDInsight-Architektur | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie für HDInsight eine Einbindung in die Domäne planen."
services: hdinsight
documentationcenter: 
author: bhanupr
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/14/2017
ms.author: bprakash
ms.openlocfilehash: 5285199d22528ed6b9fa3b7dbc85e382e7b28569
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planen von in die Azure-Domäne eingebundenen Hadoop-Clustern in HDInsight

Der standardmäßige HDInsight-Cluster ist ein Einzelbenutzercluster. Dieser ist für die meisten Unternehmen geeignet, in denen kleinere Anwendungsteams Big Data-Workloads erstellen. Mit zunehmender Beliebtheit von Hadoop führten viele Unternehmen die Umstellung auf ein Modell durch, bei dem Cluster von IT-Teams verwaltet werden und mehrere Anwendungsteams Cluster gemeinsam nutzen. Cluster für mehrere Benutzer zählen daher zu den am meisten geforderten Funktionen in Azure HDInsight.

Anstatt die Authentifizierung und Autorisierung für mehrere Benutzer selbst zu erstellen, wird für HDInsight der beliebteste Identitätsanbieter verwendet: Active Directory (AD). Die leistungsstarken Sicherheitsfunktionen in AD können zur Verwaltung der Autorisierung für mehrere Benutzer in HDInsight verwendet werden. Wenn Sie HDInsight in AD integrieren, können Sie unter Verwendung Ihrer AD-Anmeldeinformationen mit den Clustern kommunizieren. Die VMs in HDInsight sind in Ihre AD-Domäne eingebunden. Auf diese Weise kann HDInsight einen AD-Benutzer einem lokalen Hadoop-Benutzer zuordnen, sodass alle in HDInsight ausgeführten Dienste (Ambari, Hive-Server, Ranger, Spark Thrift-Server usw.) für den authentifizierten Benutzer nahtlos funktionieren. Administratoren können dann mithilfe von Apache Ranger starke Autorisierungsrichtlinien erstellen, um eine rollenbasierte Zugriffssteuerung für Ressourcen in HDInsight zu ermöglichen.


## <a name="integrate-hdinsight-with-active-directory"></a>Integrieren von HDInsight in Active Directory

Bei der Integration von HDInsight in Active Directory werden die HDInsight-Clusterknoten in eine AD-Domäne eingebunden. Die Kerberos-Sicherheit ist für die Hadoop-Komponenten auf dem Cluster konfiguriert. Für jede der Hadoop-Komponenten wird in Active Directory ein Dienstprinzipal erstellt. Zudem wird ein entsprechender Computerprinzipal für jeden Computer erstellt, die mit der Domäne verknüpft ist. Diese Dienst- und Computerprinzipale können Ihr Active Directory-Verzeichnis mit unwichtigen Elementen füllen. Daher ist es erforderlich, eine Organisationseinheit (OE) in Active Directory bereitzustellen, in der diese Prinzipale platziert werden. 

Zusammengefasst benötigen Sie zum Einrichten einer Umgebung demnach Folgendes:

- Einen Active Directory-Domänencontroller mit konfiguriertem LDAPS
- Verbindung vom virtuellen HDInsight-Netzwerk zu Ihrem Active Directory-Domänencontroller
- Eine in Active Directory erstellte Organisationseinheit
- Ein Dienstkonto, dem folgende Berechtigungen zugewiesen sind:

    - Erstellen von Dienstprinzipalen in der OE
    - Hinzufügen von Computern zur Domäne und Erstellen von Computerprinzipalen in der OE

Der folgende Screenshot zeigt eine in contoso.com erstellte OE. Einige der Dienst- und Computerprinzipale sind ebenfalls im Screenshot zu sehen.

![In die Domäne eingebundene HDInsight-Cluster (OE)](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png)zu erstellen und zu verwalten.

### <a name="two-ways-of-bringing-your-own-active-directory-domain-controllers"></a>Zwei Möglichkeiten für den Einsatz von eigenen Active Directory-Domänencontrollern

Es gibt zwei Möglichkeiten, wie Sie mit Active Directory-Domänencontrollern in die Domäne eingebundene HDInsight-Cluster erstellen können. 

- **Azure Active Directory Domain Services**: Dieser Dienst stellt eine verwaltete Active Directory-Domäne bereit, die vollständig mit Windows Server Active Directory kompatibel ist. Microsoft übernimmt die Verwaltung, das Patchen und das Überwachen der AD-Domäne. Sie können Ihren Cluster bereitstellen, ohne sich Sorgen um die Verwaltung von Domänencontrollern zu machen. Benutzer, Gruppen und Kennwörter werden über Azure Active Directory synchronisiert, sodass Benutzer sich beim Cluster mit ihren Unternehmensanmeldeinformationen anmelden können. Weitere Informationen finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).

- **Active Directory auf Azure IaaS-VMs**: Bei dieser Option übernehmen Sie die Bereitstellung und Verwaltung Ihrer eigenen Windows Server Active Directory-Domäne auf Azure IaaS-VMs. Weitere Informationen finden Sie unter [Konfigurieren einer in die Domäne eingebundenen Sandboxumgebung](./apache-domain-joined-configure.md).

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](apache-domain-joined-configure.md).
* Informationen zum Verwalten von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwalten von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](apache-domain-joined-manage.md).
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung (Vorschau)](apache-domain-joined-run-hive.md).
* Informationen zum Ausführen von Hive-Abfragen mit SSH für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
