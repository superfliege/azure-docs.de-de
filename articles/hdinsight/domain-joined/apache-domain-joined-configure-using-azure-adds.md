---
title: Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit AAD DS
description: Erfahren Sie mehr über das Einrichten und Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services.
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 8064940e0d0f035010a2521752d6f32f3f9ccd9f
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849829"
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services

In die Domäne eingebundene Cluster ermöglichen den Zugriff auf HDInsight-Cluster durch mehrere Benutzer. In die Domäne eingebundene HDInsight-Cluster sind mit einer Domäne verbunden, sodass Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren beim Cluster verwenden und Big Data-Aufträge ausführen können. 

In diesem Artikel erfahren Sie, wie Sie in die Domäne eingebundene HDInsight-Cluster mit Azure Active Directory Domain Services einrichten und konfigurieren.

## <a name="create-azure-adds"></a>Erstellen von Azure AD DS

Das Aktivieren von Azure Active Directory Domain Services (AAD DS) ist eine Voraussetzung zum Erstellen eines in eine Domäne eingebundenen HDInsight-Clusters. Informationen zum Aktivieren einer AAD DS-Instanz finden Sie unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Nur die Mandantenadministratoren verfügen über die Berechtigungen zum Erstellen einer AAD DS-Instanz. Stellen Sie bei Verwendung von Azure Data Lake Store (ADLS) als Standardspeicher für HDInsight sicher, dass der Azure AD-Standardmandant für ADLS mit der Domäne für den HDInsight-Cluster identisch ist. Da Hadoop mit Kerberos und Standardauthentifizierung arbeitet, muss die mehrstufige Authentifizierung für Benutzer deaktiviert werden, die Zugriff auf den Cluster haben sollen.

Nachdem die AAD DS-Instanz bereitgestellt wurde, müssen Sie ein Dienstkonto mit den benötigten Berechtigungen in AAD anlegen (das mit AAD DS synchronisiert wird). Wenn dieses Dienstkonto bereits vorhanden ist, müssen Sie sein Kennwort zurücksetzen und warten, bis es mit AAD DS synchronisiert ist. (Dieses Zurücksetzen führt zur Erstellung des Kerberos-Kennworthashs, und die Synchronisierung mit AAD DS kann bis zu 30 Minuten dauern.) Dieses Dienstkonto muss über die folgenden Berechtigungen verfügen:

- Einbinden von Computern in die Domäne und Platzieren von Computerprinzipalen in der Organisationseinheit, die Sie während der Clustererstellung angeben.
- Erstellen von Dienstprinzipalen in der Organisationseinheit, die Sie während der Clustererstellung angeben.

> [!NOTE]
> Da Apache Zeppelin den Domänennamen zum Authentifizieren des administrativen Dienstkontos verwendet, MUSS das Dienstkonto den gleichen Domänennamen wie das UPN-Suffix für Apache Zeppelin aufweisen, damit es ordnungsgemäß funktioniert.

Weitere Informationen zu Organisationseinheiten und ihrer Verwaltung finden Sie unter [Erstellen einer Organisationseinheit (OE) in einer durch Azure AD Domain Services verwalteten Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

Secure LDAP ist für die von AAD DS verwaltete Domäne erforderlich. Informationen zum Aktivieren von Secure LDAP finden Sie unter [Konfigurieren von sicherem LDAP (LDAPS) für eine über Azure AD Domain Services verwaltete Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Erstellen eines in die Domäne eingebundenen HDInsight-Clusters

Der nächste Schritt besteht darin, den HDInsight-Cluster mithilfe von AAD DS und des im vorherigen Abschnitt angelegten Dienstkontos zu erstellen.

Es ist einfacher, AAD DS und den HDInsight-Cluster im gleichen virtuellen Azure-Netzwerk (VNet) zu platzieren. Falls Sie sie in verschiedene VNets einbinden möchten, müssen Sie für diese VNets ein Peering so einrichten, dass HDI-VMs eine direkte Verbindung mit dem Domänencontroller haben, um die VMs zu verbinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../../virtual-network/virtual-network-peering-overview.md).

Zum Erstellen eines in die Domäne eingebundenen HDInsight-Clusters müssen Sie die folgende Parameter angeben:

- **Domänenname**: Der Domänenname, der AAD DS zugeordnet ist. Beispiel: contoso.onmicrosoft.com
- **Domänenbenutzername**: Das Dienstkonto in der verwalteten Domäne, die im vorherigen Abschnitt erstellt wurde. Beispiel: hdiadmin@contoso.onmicrosoft.com. Dieser Domänenbenutzer wird der Administrator dieses HDInsight-Clusters.
- **Domain password** (Domänenkennwort): das Kennwort des Dienstkontos.
- **Organization Unit** (Organisationseinheit): der Distinguished Name der Organisationseinheit, die Sie mit dem HDInsight-Cluster verwenden möchten. Beispiel: OU=HDInsightOU,DC=contoso,DC=onmicrosohift,DC=com. Wenn diese Organisationseinheit nicht vorhanden ist, versucht der HDInsight-Cluster, sie mithilfe der Berechtigungen zu erstellen, über die das Dienstkonto verfügt. (Wenn sich das Dienstkonto beispielsweise in der Gruppe „AAD DS-Administratoren“ befindet, hat es die Berechtigung zum Erstellen einer Organisationseinheit. Andernfalls müssen Sie möglicherweise zuerst die Organisationseinheit anlegen und dem Dienstkonto Vollzugriff auf diese Organisationseinheit gewähren. Siehe [Erstellen einer Organisationseinheit (OE) in einer durch Azure AD Domain Services verwalteten Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)).

    > [!IMPORTANT]
    > Es ist wichtig, alle Domänencontroller durch ein Komma getrennt hinter der Organisationseinheit anzugeben (z.B. OU=HDInsightOU,DC=contoso,DC=onmicrosohift,DC=com).

- **LDAPS URL**: z. B. „ldaps://contoso.onmicrosoft.com:636“

    > [!IMPORTANT]
    > Geben Sie die vollständige URL einschließlich ldaps:// und Portnummer (:636) ein.

- **Access user group** (Zugriff auf die Benutzergruppe): die Sicherheitsgruppe, deren Benutzer Sie mit dem Cluster synchronisieren möchten. Beispiel: HiveUsers. Wenn Sie mehrere Benutzergruppen angeben möchten, trennen Sie diese durch Kommas (,).
 
Der folgende Screenshot zeigt die Konfigurationen im Azure-Portal:

![Konfiguration der in die Domäne eingebundenen Azure HDInsight Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png)zu erstellen und zu verwalten.


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](apache-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Verwenden von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

