---
title: "Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services – Azure| Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das Einrichten und Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2017
ms.author: saurinsh
ms.openlocfilehash: cf8532334f03f72691fa09e8dbdd02b78072cf38
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services

In die Domäne eingebundene Cluster stellen Sicherheitsfunktionen für Unternehmen mit zahlreichen Benutzern auf HDInsight bereit. In die Domäne eingebundene HDInsight-Cluster sind mit Active Directory-Domänen verbunden, sodass Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren beim Cluster verwenden und Big Data-Aufträge ausführen können. 

Es gibt drei Möglichkeiten, einen Domänencontroller so einzurichten, dass ein in die Domäne eingebundener HDInsight die Verbindung herstellen kann:

- Azure Active Directory Domain Services (Azure AD DS)
- Lokales Active Directory
- Active Directory-Domänencontroller auf Azure IaaS-VMs

In diesem Artikel erfahren Sie, wie Sie in die Domäne eingebundene HDInsight-Cluster mit Azure Active Directory Domain Services einrichten und konfigurieren.

## <a name="create-azure-adds"></a>Erstellen von Azure AD DS

Sie müssen einen Azure AD DS erstellen, um einen HDInsight-Cluster erstellen zu können. Informationen zum Erstellen eines Azure AD DS finden Sie unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Nur die Mandantenadministratoren verfügen über Berechtigungen zum Erstellen von Domänendiensten. 

Nachdem der Domänendienst bereitgestellt wurde, müssen Sie ein Dienstkonto in der Gruppe **Azure AD DC-Administratoren** anlegen, um den HDInsight-Cluster zu erstellen. Das Dienstkonto muss ein globaler Administrator im Azure AD sein.

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Erstellen eines in die Domäne eingebundenen HDInsight-Clusters

Der nächste Schritt besteht darin, den HDInsight-Clusters mithilfe des Azure AD DS und des im vorherigen Abschnitt angelegten Dienstkontos zu erstellen.

Es ist einfacher, die Azure AD-Domänendienst und den HDInsight-Cluster in das gleiche virtuelle Azure-Netzwerk (VNet) zu platzieren. Falls sie sich in unterschiedlichen VNets befinden, müssen Sie beide VNets durch Peering verbinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../../virtual-network/virtual-network-peering-overview.md).

Zum Erstellen eines in die Domäne eingebundenen HDInsight-Clusters müssen Sie die folgende Parameter angeben:

- **Domain name** (Domänenname): der Domänenname, der dem Azure AD DS zugeordnet ist. Beispiel: contoso.onmicrosoft.com
- **Domain user name** (Domänenbenutzername): das Dienstkonto in der Gruppe der Azure AD DC-Administratoren, das im vorherigen Abschnitt erstellt wurde. Beispiel: hdiadmin@contoso.onmicrosoft.com. Dieser Benutzer der Domäne ist der Administrator dieses in die Domäne eingebundenen HDInsight-Clusters.
- **Domain password** (Domänenkennwort): das Kennwort des Dienstkontos.
- **Organization Unit** (Organisationseinheit): der Distinguished Name der Organisationseinheit, die Sie mit dem HDInsight-Cluster verwenden möchten. Beispiel: OU=HDInsightOU,DC=contoso,DC=onmicrosohift,DC=com. Wenn diese Organisationseinheit nicht vorhanden ist, versucht der HDInsight-Cluster, die Organisationseinheit zu erstellen. 
- **LDAPS URL**: z. B. „ldaps://contoso.onmicrosoft.com:636“
- **Access user group** (Zugriff auf die Benutzergruppe): die Sicherheitsgruppe, deren Benutzer Sie mit dem Cluster synchronisieren möchten. Beispiel: HiveUsers. Wenn Sie mehrere Benutzergruppen angeben möchten, trennen Sie diese durch Kommas (,).
 
Der folgende Screenshot zeigt die Konfigurationen im Azure-Portal:

![Konfiguration der in die Domäne eingebundenen Azure HDInsight Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](apache-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Verwenden von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

