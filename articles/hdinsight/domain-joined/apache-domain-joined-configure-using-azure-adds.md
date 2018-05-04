---
title: Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit AAD-DS
description: Erfahren Sie mehr über das Einrichten und Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services.
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: omidm
ms.openlocfilehash: 060ca8040f514ec1df48c2ca4568cbbb2a529267
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services

In die Domäne eingebundene Cluster stellen Sicherheitsfunktionen für Unternehmen mit zahlreichen Benutzern auf HDInsight bereit. In die Domäne eingebundene HDInsight-Cluster sind mit Active Directory-Domänen verbunden, sodass Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren beim Cluster verwenden und Big Data-Aufträge ausführen können. 

In diesem Artikel erfahren Sie, wie Sie in die Domäne eingebundene HDInsight-Cluster mit Azure Active Directory Domain Services einrichten und konfigurieren.

> [!NOTE]
> Active Directory auf Azure IaaS-VMs wird nicht mehr unterstützt.

## <a name="create-azure-adds"></a>Erstellen von Azure AD DS

Sie müssen einen Azure AD DS erstellen, um einen HDInsight-Cluster erstellen zu können. Informationen zum Erstellen eines Azure AD DS finden Sie unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Nur die Mandantenadministratoren verfügen über Berechtigungen zum Erstellen von Domänendiensten. Stellen Sie bei Verwendung von Azure Data Lake Store (ADLS) als Standardspeicher für HDInsight sicher, dass der Azure AD-Standardmandant für ADLS mit der Domäne für den HDInsight-Cluster identisch ist. Damit dieses Setup mit Azure Data Lake Store funktioniert, muss die mehrstufige Authentifizierung für Benutzer, die Zugriff auf den Cluster haben sollen, deaktiviert werden.

Nachdem der AAD-Domänendienst bereitgestellt wurde, müssen Sie ein Dienstkonto mit den richtigen Berechtigungen in AAD anlegen (welches mit AAD-DS synchronisiert wird), um den HDInsight-Cluster zu erstellen. Wenn dieses Dienstkonto bereits vorhanden ist, müssen Sie sein Kennwort zurücksetzen und warten, bis es mit AAD-DS synchronisiert ist. (Dieses Zurücksetzen führt zur Erstellung des Kerberos-Kennworthashs und kann bis zu 30 Minuten dauern.) Dieses Dienstkonto sollte über die folgenden Berechtigungen verfügen:

- Einbinden von Computern in die Domäne und Platzieren von Computerprinzipalen in der Organisationseinheit, die Sie während der Clustererstellung angeben.
- Erstellen von Dienstprinzipalen in der Organisationseinheit, die Sie während der Clustererstellung angeben.

Für durch Azure AD Domain Services verwaltete Domänen müssen Sie Secure LDAP konfigurieren. Informationen zum Aktivieren von Secure LDAP finden Sie unter [Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Erstellen eines in die Domäne eingebundenen HDInsight-Clusters

Der nächste Schritt besteht darin, den HDInsight-Clusters mithilfe des Azure AD DS und des im vorherigen Abschnitt angelegten Dienstkontos zu erstellen.

Es ist einfacher, die Azure AD-Domänendienst und den HDInsight-Cluster in das gleiche virtuelle Azure-Netzwerk (VNet) zu platzieren. Falls sie sich in unterschiedlichen VNets befinden, müssen Sie beide VNets durch Peering verbinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../../virtual-network/virtual-network-peering-overview.md).

Zum Erstellen eines in die Domäne eingebundenen HDInsight-Clusters müssen Sie die folgende Parameter angeben:

- **Domain name** (Domänenname): der Domänenname, der dem Azure AD DS zugeordnet ist. Beispiel: contoso.onmicrosoft.com
- **Domain user name** (Domänenbenutzername): das Dienstkonto in dem Azure AD DC, der im vorherigen Abschnitt erstellt wurde. Beispiel: hdiadmin@contoso.onmicrosoft.com. Dieser Benutzer der Domäne wird der Administrator dieses in die Domäne eingebundenen HDInsight-Clusters.
- **Domain password** (Domänenkennwort): das Kennwort des Dienstkontos.
- **Organization Unit** (Organisationseinheit): der Distinguished Name der Organisationseinheit, die Sie mit dem HDInsight-Cluster verwenden möchten. Beispiel: OU=HDInsightOU,DC=contoso,DC=onmicrosohift,DC=com. Wenn diese Organisationseinheit nicht vorhanden ist, versucht der HDInsight-Cluster, die Organisationseinheit zu erstellen. 
- **LDAPS URL**: z. B. „ldaps://contoso.onmicrosoft.com:636“
- **Access user group** (Zugriff auf die Benutzergruppe): die Sicherheitsgruppe, deren Benutzer Sie mit dem Cluster synchronisieren möchten. Beispiel: HiveUsers. Wenn Sie mehrere Benutzergruppen angeben möchten, trennen Sie diese durch Kommas (,).
 
> [!NOTE]
> Da Apache Zeppelin den Domänennamen zum Authentifizieren des administrativen Dienstkontos verwendet, MUSS das Dienstkonto den gleichen Domänennamen wie das UPN-Suffix für Apache Zeppelin aufweisen, damit es ordnungsgemäß funktioniert.
 
Der folgende Screenshot zeigt die Konfigurationen im Azure-Portal:

![Konfiguration der in die Domäne eingebundenen Azure HDInsight Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png)zu erstellen und zu verwalten.


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](apache-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Verwenden von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

