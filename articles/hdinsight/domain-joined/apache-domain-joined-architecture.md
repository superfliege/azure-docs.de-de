---
title: In die Domäne eingebundene Azure HDInsight-Architektur | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie für HDInsight eine Einbindung in die Domäne planen.
services: hdinsight
documentationcenter: ''
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: omidm
ms.openlocfilehash: 20d6dbad6fa1914c8b12f47bb48f6efba3895887
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
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

### <a name="the-way-of-bringing-your-own-active-directory-domain-controllers"></a>Möglichkeit für den Einsatz von eigenen Active Directory-Domänencontrollern

- **Azure Active Directory Domain Services**: Dieser Dienst stellt eine verwaltete Active Directory-Domäne bereit, die vollständig mit Windows Server Active Directory kompatibel ist. Microsoft übernimmt die Verwaltung, das Patchen und das Überwachen der AD-Domäne. Sie können Ihren Cluster bereitstellen, ohne sich Sorgen um die Verwaltung von Domänencontrollern zu machen. Benutzer, Gruppen und Kennwörter werden über Azure Active Directory synchronisiert, sodass Benutzer sich beim Cluster mit ihren Unternehmensanmeldeinformationen anmelden können. Weitere Informationen finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Verwalten von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwalten von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](apache-domain-joined-manage.md).
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung (Vorschau)](apache-domain-joined-run-hive.md).
* Informationen zum Ausführen von Hive-Abfragen mit SSH für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
