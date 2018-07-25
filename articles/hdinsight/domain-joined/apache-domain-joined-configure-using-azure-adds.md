---
title: Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters mit Azure AD DS
description: Erfahren Sie mehr über das Einrichten und Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters mit Azure Active Directory Domain Services.
services: hdinsight
author: omidm1
ms.author: omidm
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 07/17/2018
ms.openlocfilehash: 45cb9590e6dd0d8260f6e63b80caeca894f0fd44
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126033"
---
# <a name="configure-a-domain-joined-hdinsight-cluster-by-using-azure-active-directory-domain-services"></a>Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters mit Azure Active Directory Domain Services

In die Domäne eingebundene Cluster ermöglichen den Zugriff auf Azure HDInsight-Cluster durch mehrere Benutzer. In die Domäne eingebundene HDInsight-Cluster sind mit einer Domäne verbunden, sodass Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren beim Cluster verwenden und Big Data-Aufträge ausführen können. 

In diesem Artikel erfahren Sie, wie Sie einen in die Domäne eingebundenen HDInsight-Cluster mit Azure Active Directory Domain Services (Azure AD DS) einrichten und konfigurieren.

## <a name="enable-azure-ad-ds"></a>Aktivieren von Azure AD DS

Das Aktivieren von Azure AD DS ist eine Voraussetzung zum Erstellen eines in eine Domäne eingebundenen HDInsight-Clusters. Weitere Informationen finden Sie unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Nur Mandantenadministratoren verfügen über die Berechtigungen zum Erstellen einer Azure AD DS-Instanz. Stellen Sie bei Verwendung von Azure Data Lake Storage Gen1 als Standardspeicher für HDInsight sicher, dass der Azure AD-Standardmandant für Data Lake Storage Gen1 mit der Domäne für den HDInsight-Cluster identisch ist. Da Hadoop mit Kerberos und Standardauthentifizierung arbeitet, muss die mehrstufige Authentifizierung für Benutzer deaktiviert werden, die auf den Cluster zugreifen sollen.

Nachdem Sie die Azure AD DS-Instanz bereitgestellt haben, erstellen Sie in Azure Active Directory (Azure AD) ein Dienstkonto mit den erforderlichen Berechtigungen. Wenn dieses Dienstkonto bereits vorhanden ist, setzen Sie sein Kennwort zurück, und warten Sie, bis es mit Azure AD DS synchronisiert wurde. Dieses Zurücksetzen führt zur Erstellung des Kerberos-Kennworthashes. Die Synchronisierung mit Azure AD DS kann bis zu 30 Minuten dauern. 

Das Dienstkonto muss über die folgenden Berechtigungen verfügen:

- Einbinden von Computern in die Domäne und Platzieren von Computerprinzipalen in der Organisationseinheit, die Sie während der Clustererstellung angeben.
- Erstellen von Dienstprinzipalen in der Organisationseinheit, die Sie während der Clustererstellung angeben.

> [!NOTE]
> Da Apache Zeppelin den Domänennamen zum Authentifizieren des administrativen Dienstkontos verwendet, *muss* das Dienstkonto den gleichen Domänennamen wie das UPN-Suffix für Apache Zeppelin aufweisen, damit es ordnungsgemäß funktioniert.

Weitere Informationen zu Organisationseinheiten und ihrer Verwaltung finden Sie unter [Erstellen einer Organisationseinheit (OE) in einer durch Azure AD Domain Services verwalteten Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

Secure LDAP ist für eine von Azure AD DS verwaltete Domäne erforderlich. Weitere Informationen finden Sie unter [Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Erstellen eines in die Domäne eingebundenen HDInsight-Clusters

Der nächste Schritt besteht darin, den HDInsight-Cluster mithilfe von Azure AD DS und des im vorherigen Abschnitt angelegten Dienstkontos zu erstellen.

Es ist einfacher, die Azure AD DS-Instanz und den HDInsight-Cluster im gleichen virtuellen Azure-Netzwerk zu platzieren. Falls Sie sie in verschiedene virtuelle Netzwerke einbinden möchten, müssen Sie für diese virtuellen Netzwerke ein Peering so einrichten, dass HDInsight-VMs eine direkte Verbindung mit dem Domänencontroller haben, um die VMs zu verbinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../../virtual-network/virtual-network-peering-overview.md).

Zum Erstellen eines in die Domäne eingebundenen HDInsight-Clusters müssen Sie die folgende Parameter angeben:

- **Domänenname**: Der Domänenname, der Azure AD DS zugeordnet ist. Beispiel: contoso.onmicrosoft.com.
- **Domänenbenutzername**: Das Dienstkonto in der vom Azure AD DS-Domänencontroller verwalteten Domäne, die im vorherigen Abschnitt erstellt wurde. Ein Beispiel ist hdiadmin@contoso.onmicrosoft.com. Dieser Domänenbenutzer wird der Administrator dieses HDInsight-Clusters.
- **Domänenkennwort**: Das Kennwort des Dienstkontos.
- **Organisationseinheit**: Der Distinguished Name der Organisationseinheit, die Sie mit dem HDInsight-Cluster verwenden möchten. Beispiel: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com. Wenn diese Organisationseinheit nicht vorhanden ist, versucht der HDInsight-Cluster, sie mithilfe der Berechtigungen zu erstellen, über die das Dienstkonto verfügt. Wenn sich das Dienstkonto beispielsweise in der Gruppe „Azure AD DS-Administratoren“ befindet, hat es die Berechtigung zum Erstellen einer Organisationseinheit. Andernfalls müssen Sie möglicherweise zuerst die Organisationseinheit anlegen und dem Dienstkonto Vollzugriff auf diese Organisationseinheit gewähren. Weitere Informationen finden Sie unter [Erstellen einer Organisationseinheit (OE) in einer durch Azure AD Domain Services verwalteten Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).

    > [!IMPORTANT]
    > Geben Sie alle Domänencontroller durch ein Komma getrennt hinter der Organisationseinheit an (z.B. OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com).

- **LDAPS URL**: Beispiel: ldaps://contoso.onmicrosoft.com:636.

    > [!IMPORTANT]
    > Geben Sie die vollständige URL einschließlich ldaps:// und Portnummer (:636) ein.

- **Access user group** (Zugriff auf die Benutzergruppe): Die Sicherheitsgruppe, deren Benutzer Sie mit dem Cluster synchronisieren möchten. Beispiel: HiveUsers. Wenn Sie mehrere Benutzergruppen angeben möchten, trennen Sie diese durch Semikolons (,).
 
Der folgende Screenshot zeigt die Konfigurationen im Azure-Portal:

![Konfiguration der in die Domäne eingebundenen Azure HDInsight Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png)zu erstellen und zu verwalten.


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Konfigurieren von Hive-Richtlinien für in die Domäne eingebundenen HDInsight-Clustern](apache-domain-joined-run-hive.md).
* Informationen zum Verwenden von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

