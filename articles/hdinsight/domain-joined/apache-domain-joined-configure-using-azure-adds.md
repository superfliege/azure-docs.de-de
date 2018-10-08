---
title: Konfigurieren eines HDInsight-Clusters mit Enterprise-Sicherheitspaket(Enterprise Security Package, ESP) mithilfe von Azure AD DS
description: Erfahren Sie mehr über das Einrichten und Konfigurieren eines HDInsight-Clusters Enterprise-Sicherheitspaket mithilfe von Azure Active Directory Domain Services.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: eb24aa0471604696de99f4878baef764cfef0a8b
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408353"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Konfigurieren eines HDInsight-Clusters mit Enterprise-Sicherheitspaket (Enterprise Security Package, ESP) mithilfe von Azure Active Directory Domain Services

ESP-Cluster ermöglichen Mehrbenutzerzugriff in Azure HDInsight-Clustern. HDInsight-Cluster mit ESP sind mit einer Domäne verbunden, sodass Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren beim Cluster verwenden und Big Data-Aufträge ausführen können. 

In diesem Artikel erfahren Sie, wie Sie einen HDInsight-Cluster mit ESP mit Azure Active Directory Domain Services (Azure AD DS) einrichten und konfigurieren.

>[!NOTE]
>ESP ist in HDI 3.6 für Spark, Interactive und Hadoop allgemein verfügbar. Für HBase- und Kafka-Clustertypen ist ESP in der Vorschau.

## <a name="enable-azure-ad-ds"></a>Aktivieren von Azure AD DS

Das Aktivieren von Azure AD DS ist eine Voraussetzung zum Erstellen eines HDInsight-Clusters mit ESP. Weitere Informationen finden Sie unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

Wenn Azure AD DS aktiviert ist, beginnen alle Benutzer und Objekte standardmäßig mit der Synchronisierung von Azure Active Directory (AAD) zu Azure AD DS. Die Dauer des Synchronisierungsvorgangs hängt von der Anzahl der Objekte in AAD ab. Die Synchronisierung kann bei Hunderttausenden von Objekten einige Tage dauern. 

Kunden können wählen, ob sie nur die Gruppen synchronisieren möchten, die Zugriff auf die HDInsight-Cluster benötigen. Diese Option, nur bestimmte Gruppen zu synchronisieren, wird als *bereichsbezogene Synchronisierung* bezeichnet. Anweisungen finden Sie unter [Konfigurieren der bereichsbezogenen Synchronisierung von Azure AD mit Ihrer verwalteten Domäne](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-scoped-synchronization).

> [!NOTE]
> Nur Mandantenadministratoren verfügen über die Berechtigungen zum Erstellen einer Azure AD DS-Instanz. Die mehrstufige Authentifizierung muss nur für Benutzer deaktiviert werden, die auf den Cluster zugreifen.

Wenn Sie sicheres LDAP aktivieren, geben Sie den Domänennamen in den Betreffnamen oder den alternativen Betreffnamen im Zertifikat ein. Wenn Ihr Domänenname beispielsweise *contoso.com* lautet, stellen Sie sicher, dass der genaue Name in Ihrem Betreffnamen oder alternativen Betreffnamen des Zertifikats vorhanden ist. Weitere Informationen finden Sie unter [Konfigurieren von sicherem LDAP für eine durch Azure AD DS verwaltete Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="check-aad-ds-health-status"></a>Überprüfen des AAD DS-Integritätsstatus

Zeigen Sie den Integritätsstatus Ihrer Azure Active Directory Domain Services an, indem Sie **Integrität** unter der Kategorie **Verwalten** auswählen. Stellen Sie sicher, dass der Status von AAD DS grün (wird ausgeführt) und die Synchronisierung abgeschlossen ist.

![Integrität von Azure Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="add-managed-identity"></a>Hinzufügen einer verwalteten Identität

Nachdem Sie Azure AD DS aktiviert haben, erstellen Sie eine vom Benutzer zugewiesene verwaltete Identität, und weisen Sie sie der Rolle **HDInsight Domain Services Contributor** in der Azure AD DS-Zugriffssteuerung zu.

![Azure Active Directory Domain Services-Zugriffssteuerung](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Die Zuweisung einer verwalteten Identität an die Rolle **HDInsight Domain Services Contributor** stellt sicher, dass die Identität über den richtigen Zugriff verfügt, um bestimmte Domänendienstvorgänge in der AAD DS-Domäne durchzuführen. Weitere Informationen finden Sie im unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-hdinsight-cluster-with-esp"></a>Erstellen eines HDInsight-Clusters mit ESP

Der nächste Schritt besteht darin, den HDInsight-Cluster mit aktiviertem ESP mithilfe von Azure AD DS zu erstellen.

Es ist einfacher, die Azure AD DS-Instanz und den HDInsight-Cluster im gleichen virtuellen Azure-Netzwerk zu platzieren. Falls Sie sie in verschiedene virtuelle Netzwerke einbinden möchten, müssen Sie für diese virtuellen Netzwerke ein Peering so einrichten, dass HDInsight-VMs eine direkte Verbindung mit dem Domänencontroller haben, um die VMs zu verbinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../../virtual-network/virtual-network-peering-overview.md). Um zu testen, ob das Peering korrekt durchgeführt wird, verbinden Sie eine VM mit dem HDInsight-VNET/Subnetz, und pingen Sie den Domänennamen, oder führen Sie **ldp.exe** aus, um auf die AAD DS-Domäne zuzugreifen.

Beim Erstellen eines HDInsight-Clusters können Sie das Enterprise-Sicherheitspaket auf der Registerkarte „Benutzerdefiniert“ aktivieren. 

![Azure HDInsight – Sicherheit und Netzwerk](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

Sobald Sie ESP aktivieren, werden häufige Fehlkonfigurationen im Zusammenhang mit Azure AD DS automatisch erkannt und validiert.

![Azure HDInsight Enterprise-Sicherheitspaket – Domänenüberprüfung](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Die Früherkennung spart Zeit, da Sie Fehler vor der Erstellung des Clusters beheben können.

![Azure HDInsight Enterprise-Sicherheitspaket – fehlgeschlagene Domänenüberprüfung](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Zum Erstellen eines HDInsight-Clusters mit ESP müssen Sie die folgende Parameter angeben:

- **Clusteradministrator**: Wählen Sie einen Administrator für Ihren Cluster aus Ihrem synchronisierten Azure AD DS aus. Dieses Konto muss bereits synchronisiert und in AAD DS verfügbar sein.

- **Clusterzugriffsgruppen**: Die Sicherheitsgruppen, deren Benutzer Sie mit dem Cluster synchronisieren möchten, sollten in Azure AD DS verfügbar sein. Zum Beispiel die Gruppe HiveUsers. Weitere Informationen dazu finden Sie in [Erstellen einer Gruppe in Azure Active Directory und Hinzufügen von Mitgliedern](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **LDAPS URL**: Beispiel: ldaps://contoso.com:636.

Der folgende Screenshot zeigt eine erfolgreiche Konfigurationen im Azure-Portal:

![Konfiguration von Active Directory Domain Services für Azure HDInsight ESP](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

Die von Ihnen erstellte verwaltete Identität kann beim Erstellen eines neuen Clusters aus dem Dropdownmenü für die vom Benutzer zugewiesene verwaltete Identität ausgewählt werden.

![Konfiguration von Active Directory Domain Services für Azure HDInsight ESP](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung](apache-domain-joined-run-hive.md).
* Informationen zum Verwenden von HDInsight-Clustern mit ESP finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).