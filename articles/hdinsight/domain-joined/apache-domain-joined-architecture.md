---
title: "In die Domäne eingebundene Azure HDInsight-Architektur | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie für HDInsight eine Einbindung in die Domäne planen."
services: hdinsight
documentationcenter: 
author: saurinsh
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
ms.date: 09/21/2017
ms.author: saurinsh
ms.openlocfilehash: 9deb5e4dbd925c4fdc523d8d21afbcfbf85947b8
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planen von in die Azure-Domäne eingebundenen Hadoop-Clustern in HDInsight

Bei einem herkömmlichen Hadoop-Cluster handelt es sich um einen Einzelbenutzercluster. Dieser ist für die meisten Unternehmen geeignet, in denen kleinere Anwendungsteams Big Data-Workloads erstellen. Hadoop wird immer beliebter, und viele Unternehmen führen die Umstellung auf ein Modell durch, bei dem Cluster von IT-Teams verwaltet werden und mehrere Anwendungsteams Cluster gemeinsam nutzen. Cluster für mehrere Benutzer zählen daher zu den am meisten geforderten Funktionen in Azure HDInsight.

Anstatt die Authentifizierung und Autorisierung für mehrere Benutzer selbst zu erstellen, wird für HDInsight der beliebteste Identitätsanbieter verwendet: Active Directory (AD). Die leistungsstarken Sicherheitsfunktionen in AD können zur Verwaltung der Autorisierung für mehrere Benutzer in HDInsight verwendet werden. Wenn Sie HDInsight in AD integrieren, können Sie unter Verwendung Ihrer AD-Anmeldeinformationen mit den Clustern kommunizieren. In HDInsight wird ein AD-Benutzer einem lokalen Hadoop-Benutzer zugeordnet, sodass alle Dienste, die unter HDInsight ausgeführt werden (Ambari, Hive-Server, Ranger, Spark Thrift-Server usw.), für den authentifizierten Benutzer nahtlos funktionieren.

## <a name="integrate-hdinsight-with-active-directory"></a>Integrieren von HDInsight in Active Directory

Wenn Sie HDInsight in Active Directory integrieren, werden die HDInsight-Clusterknoten in eine AD-Domäne eingebunden. In HDInsight werden Dienstprinzipale für die im Cluster ausgeführten Hadoop-Dienste erstellt und in einer angegebenen Organisationseinheit (OE) in der Domäne angeordnet. Darüber hinaus erstellt HDInsight umgekehrte DNS-Zuordnungen in der Domäne für die IP-Adresse der Knoten, die in die Domäne eingebunden sind.

Es gibt zwei Bereitstellungsoptionen für Active Directory:
* **[Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md):** Dieser Dienst stellt eine verwaltete Active Directory-Domäne bereit, die vollständig mit Windows Server Active Directory kompatibel ist. Microsoft übernimmt die Verwaltung, das Patchen und das Überwachen der AD-Domäne. Sie können Ihren Cluster bereitstellen, ohne sich Sorgen um die Verwaltung von Domänencontrollern zu machen. Benutzer, Gruppen und Kennwörter werden über Ihr Azure Active Directory synchronisiert, sodass Benutzer sich beim Cluster mithilfe ihrer Unternehmensanmeldeinformationen anmelden können.

* **Windows Server Active Directory-Domäne auf Azure IaaS-VMs:** Bei dieser Option übernehmen Sie die Bereitstellung und Verwaltung Ihrer eigenen Windows Server Active Directory-Domäne auf Azure IaaS-VMs. 

Sie können mehrere Architekturen verwenden, um diese Einrichtung umzusetzen. Sie können zwischen den folgenden Architekturen wählen.


### <a name="hdinsight-integrated-with-an-azure-ad-domain-services-managed-ad-domain"></a>HDInsight integriert in eine von Azure AD Domain Services verwaltete AD-Domäne
Sie können eine von [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS) verwaltete Domäne bereitstellen. Azure AD DS stellt eine verwaltete AD-Domäne in Azure bereit, die von Microsoft verwaltet, aktualisiert und überwacht wird. Es werden zwei Domänencontroller erstellt, um für eine Hochverfügbarkeit zu sorgen, und es sind DNS-Dienste enthalten. Sie können dann den HDInsight-Cluster in diese verwaltete Domäne integrieren. Bei dieser Bereitstellungsoption müssen Sie sich keine Sorgen über das Verwalten, Patchen, Aktualisieren und Überwachen von Domänencontrollern machen.

![Topologie mit in die Domäne eingebundenem HDInsight-Cluster](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Voraussetzungen für die Integration in Azure AD Domain Services:

* [Eine durch Azure AD Domain Services verwaltete Domäne muss bereitgestellt werden](../../active-directory-domain-services/active-directory-ds-getting-started.md).
* Eine [Organisationseinheit](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) muss erstellt werden, in der die HDInsight-Cluster-VMs und die vom Cluster verwendeten Dienstprinzipale platziert werden.
* Bei der Konfiguration von Azure AD DS müssen Sie [LDAPS](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) einrichten. Das Zertifikat zum Einrichten von LDAPS muss ein von einer öffentlichen Zertifizierungsstelle ausgestelltes Zertifikat sein (kein selbst signiertes Zertifikat).
* In der verwalteten Domäne müssen für den IP-Adressbereich des HDInsight-Subnetzes (z.B. 10.2.0.0/24 in der obigen Abbildung) Reverse-DNS-Zonen erstellt werden.
* Die Konfiguration der [Synchronisierung von Kennworthashes, die für die NTLM- und Kerberos-Authentifizierung erforderlich sind](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md), muss von Azure AD in die verwaltete Azure AD DS-Domäne geändert werden.
* Ein Dienstkonto oder ein Benutzerkonto wird benötigt. Mit diesem Konto erstellen Sie den HDInsight-Cluster. Dieses Konto muss die folgenden Berechtigungen besitzen:

    - Berechtigungen zum Erstellen von Dienstprinzipalobjekten und Computerobjekten innerhalb der Organisationseinheit
    - Berechtigungen zum Erstellen von Reverse-DNS-Proxyregeln
    - Berechtigungen zum Einbinden von Computern in die Azure AD-Domäne


### <a name="hdinsight-integrated-with-windows-server-ad-running-on-azure-iaas"></a>HDInsight integriert in Windows Server AD unter Azure IaaS

Sie können die Windows Server Active Directory Domain Services-Rolle auf einem oder mehreren virtuellen Computern in Azure bereitstellen und sie auf Domänencontroller heraufstufen. Diese Domänencontroller-VMs können mithilfe des Resource Manager-Bereitstellungsmodells im gleichen virtuellen Netzwerk wie der HDInsight-Cluster bereitgestellt werden. Wenn die Domänencontroller in einem anderen virtuellen Netzwerk bereitgestellt werden, müssen Sie diese virtuellen Netzwerke mithilfe von [VNet-zu-VNet-Peering](../../virtual-network/virtual-network-create-peering.md) verknüpfen. 

[Weitere Informationen: Bereitstellung von Windows Server Active Directory auf Azure-VMs](../../active-directory/virtual-networks-windows-server-active-directory-virtual-machines.md)

![Topologie mit in die Domäne eingebundenem HDInsight-Cluster](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> In dieser Architektur können Sie Azure Data Lake Store nicht mit dem HDInsight-Cluster nutzen.


Voraussetzungen für die Integration in Windows Server Active Directory auf Azure-VMs:

* Eine [Organisationseinheit](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) muss erstellt werden, in der die HDInsight-Cluster-VMs und die vom Cluster verwendeten Dienstprinzipale platziert werden.
* Für die Kommunikation mit AD müssen [Lightweight Directory Access-Protokolle](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) eingerichtet werden. Das Zertifikat zum Einrichten von LDAPS muss ein reales Zertifikat (kein selbst signiertes Zertifikat) sein.
* In der Domäne müssen für den IP-Adressbereich des HDInsight-Subnetzes (z.B. 10.2.0.0/24 in der obigen Abbildung) Reverse-DNS-Zonen erstellt werden.
* Ein Dienstkonto oder ein Benutzerkonto wird benötigt. Mit diesem Konto erstellen Sie den HDInsight-Cluster. Dieses Konto muss die folgenden Berechtigungen besitzen:

    - Berechtigungen zum Erstellen von Dienstprinzipalobjekten und Computerobjekten innerhalb der Organisationseinheit
    - Berechtigungen zum Erstellen von Reverse-DNS-Proxyregeln
    - Berechtigungen zum Einbinden von Computern in die Active Directory-Domäne


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](apache-domain-joined-configure.md).
* Informationen zum Verwalten von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwalten von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](apache-domain-joined-manage.md).
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung (Vorschau)](apache-domain-joined-run-hive.md).
* Informationen zum Ausführen von Hive-Abfragen mit SSH für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
