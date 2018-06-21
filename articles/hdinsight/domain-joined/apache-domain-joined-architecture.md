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
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: f4380f5d6ec379d5807f697294623a672bd270ae
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34715240"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planen von in die Azure-Domäne eingebundenen Hadoop-Clustern in HDInsight

Der standardmäßige HDInsight-Cluster ist ein Einzelbenutzercluster. Dieser ist für die meisten Unternehmen geeignet, in denen kleinere Anwendungsteams Big Data-Workloads erstellen. Jedem Benutzer kann bei Bedarf ein anderer Cluster reserviert werden, und er kann ihn zerstören, wenn er nicht mehr benötigt wird. Viele Unternehmen führten jedoch die Umstellung auf ein Modell durch, bei dem Cluster von IT-Teams verwaltet werden und mehrere Anwendungsteams Cluster gemeinsam nutzen. Daher ist für diese größeren Unternehmen der Zugriff auf den Cluster durch mehrere Benutzer in Azure HDInsight erforderlich.

HDInsight basiert auf dem am häufigsten verwendeten Identitätsanbieter – Active Directory (AD) in einer verwalteten Methode. Durch die Integration von HDInsight in [Azure Active Directory Domain Services (AAD-DS)](../../active-directory-domain-services/active-directory-ds-overview.md) können Sie mithilfe der Anmeldeinformationen für die Domäne auf die Cluster zugreifen. Die VMs in HDInsight sind in die von Ihnen bereitgestellte Domäne eingebunden, sodass alle in HDInsight ausgeführten Dienste (Ambari, Hive-Server, Ranger, Spark Thrift-Server usw.) für den authentifizierten Benutzer nahtlos funktionieren. Administratoren können dann mithilfe von Apache Ranger starke Autorisierungsrichtlinien erstellen, um eine rollenbasierte Zugriffssteuerung für Ressourcen im Cluster zu ermöglichen.


## <a name="integrate-hdinsight-with-active-directory"></a>Integrieren von HDInsight in Active Directory

Open-Source-Hadoop basiert auf Kerberos für die Bereitstellung von Authentifizierung und Sicherheit. Daher sind HDInsight-Clusterknoten in eine mit AAD-DS verwaltete Domäne eingebunden. Die Kerberos-Sicherheit ist für die Hadoop-Komponenten auf dem Cluster konfiguriert. Für jede der Hadoop-Komponenten wird automatisch ein Dienstprinzipal erstellt. Zudem wird ein entsprechender Computerprinzipal für jeden Computer erstellt, die mit der Domäne verknüpft ist. Um diese Dienst- und Computerprinzipale speichern zu können, muss eine Organisationseinheit (Organizational Unit, OU) in dem Domänencontroller (AAD-DS) bereitgestellt werden, in dem diese Prinzipale platziert werden. 

Zusammengefasst benötigen Sie zum Einrichten einer Umgebung Folgendes:

- Eine (vom AAD-DS verwaltete) Active Directory-Domäne
- In AAD-DS aktiviertes sicheres LDAP (LDAPS).
- Richtige Netzwerkkonnektivität zwischen HDInsight-VNET und AAD-DS-VNET für den Fall, dass Sie separate VNETs für sie auswählen. Ein virtueller Computer im HDI-VNET sollte über eine direkte Verbindung zu AAD-DS mittels VNET-Peering verfügen. Wenn HDI und AAD-DS im gleichen VNET bereitgestellt werden, wird die Konnektivität automatisch bereitgestellt, und keine weitere Aktion ist erforderlich.
- Eine [in AAD-DS erstellte Organisationseinheit](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) (Organizational Unit, OU)
- Ein Dienstkonto, dem folgende Berechtigungen zugewiesen sind:
    - Erstellen von Dienstprinzipalen in der OE
    - Hinzufügen von Computern zur Domäne und Erstellen von Computerprinzipalen in der OE.

Der folgende Screenshot zeigt eine in contoso.com erstellte OE. Einige der Dienst- und Computerprinzipale sind ebenfalls im Screenshot zu sehen.

![In die Domäne eingebundene HDInsight-Cluster (OE)](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png)zu erstellen und zu verwalten.

### <a name="different-domain-controllers-setup"></a>Anderes Domänencontrollersetup:
HDInsight unterstützt derzeit nur AAD-DS als Hauptdomänencontroller, mit dem der Cluster bezüglich der Kerberos-Authentifizierung kommuniziert. Andere komplexe AD-Setups sind jedoch auch möglich, solange AAD-DS für den HDI-Zugriff aktiviert wird.

- **[Azure Active Directory Domain Services (AAD-DS)](../../active-directory-domain-services/active-directory-ds-overview.md)**: Dieser Dienst stellt eine verwaltete Domäne bereit, die vollständig mit Windows Server Active Directory kompatibel ist. Microsoft übernimmt in einem Hochverfügbarkeitssetup (Highly Available, HA) die Verwaltung, das Patchen und das Überwachen der Domäne. Sie können Ihren Cluster bereitstellen, ohne sich Sorgen um die Verwaltung von Domänencontrollern zu machen. Benutzer, Gruppen und Kennwörter werden über Azure Active Directory (AAD) [Unidirektionale Synchronisierung von AAD zu AAD-DS] synchronisiert, sodass Benutzer sich mit denselben Unternehmensanmeldeinformationen beim Cluster anmelden können. Weitere Informationen finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).
- **Lokales AD oder AD auf virtuellen IaaS-Computern**: Wenn Sie über ein lokales AD oder andere komplexere AD-Setups für Ihre Domäne verfügen, können Sie diese Identitäten mithilfe von AD Connect mit AAD synchronisieren und dann AAD-DS auf diesem AD-Mandanten aktivieren. Da Kerberos auf Kennworthashes angewiesen ist, müssen Sie [die Kennworthashsynchronisierung auf AAD-DS aktivieren](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Wenn Sie den Verbund mit AD Federation Services (ADFS) verwenden, können Sie optional die Kennworthashsynchronisierung als Sicherung einrichten, für den Fall, dass in Ihrer ADFS-Infrastruktur ein Fehler auftritt. Weitere Informationen finden Sie unter [Implementieren der Kennworthashsynchronisierung mit der Azure AD Connect-Synchronisierung](../../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). Die alleinige Verwendung von lokalem AD oder AD auf IaaS-VMs ohne AAD und AAD-DS ist keine unterstützte Konfiguration für die HDI-Clusterdomäneneinbindung.

## <a name="next-steps"></a>Nächste Schritte
* [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern](apache-domain-joined-configure-using-azure-adds.md).
* [Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung](apache-domain-joined-run-hive.md).
* [Verwalten von in die Domäne eingebundenen HDInsight-Clustern](apache-domain-joined-manage.md). 
