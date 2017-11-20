---
title: "Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das Einrichten und Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 2c844ce8aec04c74a9c2dbecdd1b3effb286df97
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)

Erfahren Sie, wie man Azure HDInsight-Cluster mit Azure Active Directory (Azure AD) und [Apache Ranger](http://hortonworks.com/apache/ranger/) einrichtet, und profitieren Sie von strengen Richtlinien für Authentifizierung und Rollenbasierte Zugriffssteuerung (role-based access control, RBAC).  In die Domäne eingebundenes HDInsight kann nur unter Linux-basierten Clustern konfiguriert werden. Weitere Informationen finden Sie unter [Introduce Domain-joined HDInsight clusters (Einführung in HDInsight-Cluster mit Domänenverknüpfung)](apache-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie ist auf HDInsight mit Domänenverknüpfung nicht aktiviert.

Dieser Artikel ist das erste Tutorial einer Reihe:

* Erstellen eines HDInsight-Clusters mit Verbindung zu Azure AD (über die Eigenschaft „Azure Directory Domain Services“) mit Apache Ranger aktiviert.
* Erstellen und Anwenden von Hive-Richtlinien durch Apache Ranger, und Erteilen der Erlaubnis an Nutzer (z.B. Datenanalysten), sich mithilfe von ODBC-basierten Tools, z.B. Excel, Tableau usw. mit Hive zu verbinden. Microsoft arbeitet daran, schon bald andere Workloads, wie z.B. HBase und Storm zu in HDInsight mit Domänenverknüpfung hinzuzufügen.

Namen für Azure-Dienste müssen global eindeutig sein. In diesem Tutorial werden die folgenden Namen verwendet. Contoso ist ein fiktiver Name. Sie müssen *contoso* durch einen anderen Namen ersetzen, wenn Sie das Tutorial durcharbeiten. 

**Namen:**

| Eigenschaft | Wert |
| --- | --- |
| Azure AD-Verzeichnis |contosoaaddirectory |
| Name Ihrer Azure AD-Domäne |contoso (contoso.onmicrosoft.com) |
| HDInsight VNet |contosohdivnet |
| HDInsight VNet-Ressourcengruppe |contosohdirg |
| HDInsight-Cluster |contosohdicluster |

In diesem Tutorial finden Sie die Schritte zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters. Jeder Abschnitt enthält Links zu anderen Artikeln mit weiteren Hintergrundinformationen.

## <a name="prerequisite"></a>Voraussetzung:
* Machen Sie sich mit [Azure AD-Domänendiensten](https://azure.microsoft.com/services/active-directory-ds/) und deren [Preis](https://azure.microsoft.com/pricing/details/active-directory-ds/)-Struktur vertraut.
* Stellen Sie sicher, dass sich Ihre Abonnement auf der Zulassungsliste befindet. Dazu können Sie eine E-Mail mit Ihrer Abonnement-ID an hdipreview@microsoft.com senden.
* Ein SSL-Zertifikat, das von einer Signierstelle oder einem selbstsignierten Zertifikat für Ihre Domäne signiert wurde. Es wird ein Zertifikat für sicheres LDAP benötigt.

## <a name="procedures"></a>Prozeduren
1. Erstellen Sie ein HDInsight-VNet im Ressourcenverwaltungsmodus von Azure.
2. Erstellen und konfigurieren Sie Azure AD und Azure AD DS.
3. Erstellen Sie ein HDInsight-Cluster.

> [!NOTE]
> In diesem Lernprogramm wird davon ausgegangen, dass Sie noch nicht über Azure AD verfügen. Wenn Sie bereits darüber verfügen, können Sie diesen Teil überspringen.
> 
> 

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Erstellen Sie ein Resource Manager-VNet für HDInsight-Cluster
In diesem Abschnitt erstellen Sie ein Azure Ressource Manager-VNet, das für HDInsight-Cluster verwendet wird. Weitere Informationen zum Erstellen eines Azure-VNet mit anderen Methoden finden Sie unter [Erstellen eines virtuellen Netzwerks](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

Nach dem Erstellen des VNet konfigurieren Sie die Azure AD DS zur Verwendung dieses virtuellen Netzwerks.

**Erstellen eines Resource Manager-VNET**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu**, **Netzwerk**, und dann auf **Virtuelles Netzwerk**. 
3. Wählen Sie unter **Bereitstellungsmodell auswählen** die Option **Resource Manager**, und klicken Sie auf **Erstellen**.
4. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * **Name**: contosohdivnet
   * **Adressraum**: 10.0.0.0/16.
   * **Subnetzname**: Subnet1
   * **Subnetzadressbereich**: 10.0.0.0/24
   * **Abonnement**: (Wählen Sie Ihr Azure-Abonnement aus.)
   * **Ressourcengruppe**: contosohdirg
   * **Speicherort**: (Wählen Sie den gleichen Speicherort wie für das Azure AD-VNet aus, z. B. contosoaadvnet.)
5. Klicken Sie auf **Erstellen**.

**Konfigurieren von DNS für das Resource Manager-VNET**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Weitere Dienste** > **Virtuelle Netzwerke**. Achten Sie darauf, dass Sie nicht auf **Virtuelle Netzwerke (klassisch)** klicken.
2. Klicken Sie auf **contosohdivnet**.
3. Klicken Sie auf der linken Seite des neuen Blatts auf **DNS-Server**.
4. Klicken Sie auf **Benutzerdefiniert**, und geben Sie die folgenden Werte ein:
   
   * 10.0.0.4
   * 10.0.0.5     
     
5. Klicken Sie auf **Speichern**.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Erstellen und Konfigurieren von Azure AD DS für Azure AD
In diesem Abschnitt werden Sie:

1. Azure AD erstellen.
2. Azure AD-Benutzer erstellen. Diese Benutzer sind Domänenbenutzer. Sie verwenden den ersten Benutzer, um den HDInsight-Cluster mit Azure AD zu erstellen.  Die anderen beiden Benutzer sind in diesem Tutorial optional. Sie werden in [Configure Hive policies for Domain-joined HDInsight clusters (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster)](apache-domain-joined-run-hive.md) verwendet, wenn Sie Apache Ranger-Richtlinien erstellen.
3. Eine AAD DC Administratorengruppe erstellen und den Azure AD-Benutzer zur Gruppe hinzufügen. Sie verwenden diesen Benutzer zum Erstellen der Organisationseinheit.
4. Azure AD Domain Services (Azure AD DS) für Azure AD Aktivieren.
5. LDAPS für Azure AD konfigurieren. Das Lightweight Directory Access Protocol (LDAP) wird zum Lesen und Schreiben in Azure AD verwendet.

Wenn Sie lieber ein vorhandenes Azure AD verwenden möchten, können Sie die Schritte 1 und 2 überspringen.

**Erstellen von Azure AD**

1. Öffnen Sie das [klassische Azure-Portal](https://manage.windowsazure.com), klicken Sie auf **Neu** > **App Services** > **Active Directory** > **Directory** > **Benutzerdefiniert erstellen**. 
2. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * **Name**: contosoaaddirectory
   * **Domänenname**: contoso.  Dieser Name muss global eindeutig sein.
   * **Land oder Region**: Wählen Sie Ihr Land oder Ihre Region aus.
3. Klicken Sie auf **Fertig stellen**.

**Einen Azure AD-Benutzer erstellen**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Azure Active Directory** > **contosoaaddirectory** > **Benutzer und Gruppen**. 
2. Klicken Sie im Menü auf **Alle Benutzer**.
3. Klicken Sie auf **Neuer Benutzer**.
4. Geben Sie den **Namen** und **Benutzernamen** ein, und klicken Sie dann auf **Weiter**. 
5. Konfigurieren Sie das Benutzerprofil. Wählen Sie **Globaler Administrator** in **Rolle**, und klicken Sie dann auf **Weiter**.  Die Rolle „Globaler Administrator“ ist notwendig, um Organisationseinheiten zu erstellen.
6. Erstellen Sie eine Kopie des temporären Kennworts.
7. Klicken Sie auf **Erstellen**. Weiter unten in diesem Tutorial verwenden Sie diesen globalen Administratorbenutzer zum Erstellen des HDInsight-Clusters.

Verfahren Sie auf das gleiche Verfahren, um zwei weitere Benutzer mit der Rolle **Benutzer** hinzuzufügen: hiveuser1 und hiveuser2. Die folgenden Benutzer werden in [Configure Hive policies for Domain-joined HDInsight clusters (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster)](apache-domain-joined-run-hive.md) verwendet.

**Erstellen einer AAD DC-Administratorengruppe und Hinzufügen eines Azure AD-Benutzers**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Azure Active Directory** > **contosoaaddirectory** > **Benutzer und Gruppen**. 
2. Klicken Sie im Menü am oberen Rand auf **Alle Gruppen**.
3. Klicken Sie auf **Neue Gruppe**.
4. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * **Name**: AAD DC Administrators.  Ändern Sie nicht den Gruppennamen.
   * **Mitgliedschaftstyp**: Zugewiesen.
5. Klicken Sie auf **Auswählen**.
6. Klicken Sie auf **Mitglieder**.
7. Wählen Sie den im vorherigen Schritt erstellten Benutzer aus, und klicken Sie auf **Auswählen**.
8. Führen Sie die gleichen Schritte erneut aus, um eine weitere Gruppe mit dem Namen **HiveUsers** zu erstellen, und fügen Sie die beiden Hive-Benutzer zur Gruppe hinzu.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Domain Services – Erstellen der Administratorengruppe für Azure AD-Domänencontroller](../../active-directory-domain-services/active-directory-ds-getting-started.md).

**Aktivieren von Azure AD DS für Azure AD**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressource erstellen** > **Sicherheit + Identität** > **Azure AD Domain Services** > **Hinzufügen**. 
2. Geben Sie folgende Werte ein bzw. wählen diese aus:
   * **Verzeichnisname**: contosoaaddirectory
   * **DNS-Domänenname**: Zeigt den standardmäßigen DNS-Namen des Azure-Verzeichnis an. Zum Beispiel contoso.onmicrosoft.com.
   * **Standort**: Wählen Sie Ihre Region aus.
   * **Netzwerk**: Wählen Sie das zuvor erstellte virtuelle Netzwerk und Subnetz aus, z. B. **contosohdivnet**.
3. Klicken Sie auf der Seite „Zusammenfassung“ auf **OK**. Unter den Benachrichtigungen wird **Bereitstellung wird ausgeführt...** angezeigt.
4. Warten Sie, bis **Bereitstellung wird ausgeführt...** ausgeblendet wurde und **IP-Adresse** aufgefüllt wird. Zwei IP-Adressen werden aufgefüllt. Dies sind die IP-Adressen der Domänencontroller, die von den Domänendiensten bereitgestellt werden. Jede IP-Adresse wird angezeigt, sobald der entsprechende Domänencontroller bereitgestellt wurde und bereit ist. Notieren Sie sich die beiden IP-Adressen. Diese werden später benötigt werden.

Weitere Informationen finden Sie unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../../active-directory-domain-services/active-directory-ds-getting-started.md).

**Synchronisieren Ihres Kennworts**

Wenn Sie Ihre eigene Domäne verwenden, müssen Sie das Kennwort synchronisieren. Weitere Informationen finden Sie unter [Aktivieren der Kennwortsynchronisierung für Azure AD-Domänendienste](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**Konfigurieren von LDAPS für Azure AD**

1. Rufen Sie für Ihre Domäne ein SSL-Zertifikat ab, das von einer Signierstelle signiert wurde.
2. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Azure AD Domain Services** > **contoso.onmicrosoft.com**. 
3. Aktivieren Sie **Secure LDAP**.
6. Folgen Sie den Anweisungen, um die Zertifikatsdatei und das Kennwort anzugeben.  
7. Warten Sie, bis **Secure LDAP-Zertifikat** aufgefüllt ist. Das kann 10 Minuten oder mehr dauern.

> [!NOTE]
> Wenn gerade Hintergrundaufgaben unter Azure AD DS ausgeführt werden, erhalten Sie möglicherweise eine Fehlermeldung beim Hochladen des Zertifikats – <i>There is an operation being performed for this tenant. Please try again later (Für diesen Mandanten wird gerade eine Operation ausgeführt.Versuchen Sie es später erneut)</i>.  Für den Fall, dass dieser Fehler auftritt, versuchen Sie es bitte später noch einmal. Bei der zweiten Domänencontroller-IP kann es bis zu 3 Stunden dauern, bis diese bereitgestellt wird.
> 
> 

Weitere Informationen finden Sie unter [Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-hdinsight-cluster"></a>Erstellen des HDInsight-Clusters
In diesem Abschnitt erstellen Sie einen Linux-basierten Hadoop-Cluster in HDInsight, entweder mithilfe des Azure-Portals oder einer [Azure Resource Manager-Vorlage](../../azure-resource-manager/resource-group-template-deploy.md). Andere Methoden zur Erstellung von Clustern und Informationen zu den Einstellungen finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Weitere Informationen zur Verwendung einer Resource Manager-Vorlage zum Erstellen von Hadoop-Clustern in HDInsight finden Sie unter [Erstellen von Hadoop-Clustern in HDInsight mithilfe von Resource Manager-Vorlagen](../hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**Erstellen eines in die Domäne eingebundenen HDInsight-Clusters mithilfe des Azure-Portals**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu**, **Intelligence + Analyse**, und klicken Sie dann auf **HDInsight**.
3. Geben Sie auf dem Blatt **Neuer HDInsight-Cluster** die folgenden Werte ein, oder wählen Sie diese aus:
   
   * **Clustername**: Geben Sie einen neuen Namen für den in die Domäne eingebundenen HDInsight-Cluster ein.
   * **Abonnement** : Wählen Sie ein Azure-Abonnement aus, das für die Erstellung dieses Clusters verwendet wird.
   * **Clusterkonfiguration**:
     
     * **Clustertyp**: Hadoop. HDInsight mit Domänenverknüpfung wird derzeit nur auf Hadoop-, Spark- und Interactive Query-Clustern unterstützt.
     * **Betriebssystem**: Linux.  HDInsight mit Domänenverknüpfung wird derzeit nur auf Linux-basierten HDInsight-Clustern unterstützt.
     * **Version:** HDI 3.6. HDInsight mit Domänenverknüpfung wird derzeit nur auf HDInsight-Clustern, Version 3.6 unterstützt.
     * **Clustertyp**: PREMIUM
       
       Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
   * **Anmeldeinformationen**: Konfigurieren Sie die Anmeldeinformationen für den Benutzer des Clusters und den Benutzer von SSH.
   * **Datenquelle**: Erstellen Sie ein neues Speicherkonto oder verwenden Sie ein vorhandenes Speicherkonto, das als Standardspeicherkonto für den HDInsight-Cluster verwendet werden kann. Der Standort muss dem Standort der beiden VNets entsprechen.  Dieser Standort muss auch dem Standort des HDInsight-Clusters entsprechen.
   * **Preise**: Wählen Sie die Anzahl der Workerknoten Ihres Clusters aus.
   * **Erweiterte Konfiguration**: 
     
     * **Domain-joining &amp; Vnet/Subnet (Domänenbeitritt &amp; VNET-Subnetz)**: 
       
       * **Domäneneinstellungen**: 
         
         * **Domänenname**: contoso.onmicrosoft.com
         * **Domänenbenutzername**: Geben Sie einen Domänenbenutzernamen ein. Diese Domäne muss über die folgenden Berechtigungen verfügen: Hinzufügen von Computern zur Domäne, und Platzieren dieser Computer in der während der Clustererstellung festgelegten Organisationseinheit; Erstellen von Dienstprinzipalen innerhalb der während der Clustererstellung festgelegten Organisationseinheit; Erstellen von Reverse-DNS-Einträgen. Dieser Benutzer der Domäne wird der Administrator dieses in die Domäne eingebundenen HDInsight-Clusters.
         * **Domänenkennwort**: Geben Sie das Benutzerkennwort für die Domäne ein.
         * **Organisationseinheit**: Geben Sie den Distinguished Name der Organisationseinheit ein, die Sie mit dem HDInsight-Cluster verwenden möchten. Beispiel: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com. Wenn diese Organisationseinheit nicht vorhanden ist, versucht der HDInsight-Cluster, die Organisationseinheit zu erstellen. Stellen Sie sicher, dass die Organisationseinheit bereits vorhanden ist, oder dass das Domänenkonto berechtigt ist, eine neue Organisationseinheit zu erstellen. Wenn Sie das zu „AADDC-Administratoren“ gehörende Domänenkonto verwenden, verfügt es über die erforderlichen Berechtigungen zum Erstellen der Organisationseinheit.
         * **LDAPS URL**: ldaps://contoso.onmicrosoft.com:636
         * **Access user group** (Zugriff auf die Benutzergruppe): Geben Sie die Sicherheitsgruppe an, deren Benutzer Sie mit dem Cluster synchronisieren möchten. Beispiel: HiveUsers.
           
           Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
           
           ![Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuelles Netzwerk**: contosohdivnet
       * **Subnetz**: Subnet1
         
         Klicken Sie zum Speichern der Änderungen auf **Auswählen**.        
         Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
   * **Ressourcengruppe**: Wählen Sie die Ressourcengruppe aus, die für das HDInsight VNet verwendet wird (contosohdirg).
4. Klicken Sie auf **Erstellen**.  

Eine weitere Möglichkeit in die Domäne eingebundene HDInsight-Cluster zu erstellen ist es, eine Azure Resource Management-Vorlage zu verwenden. Im Folgenden sehen Sie, wie das funktioniert:

**Erstellen eines in die Domäne eingebundenen HDInsight-Clusters mithilfe einer Resource Management-Vorlage**

1. Klicken Sie auf das folgende Bild, um eine Resource Manager-Vorlage im Azure-Portal zu öffnen. Die Resource Manager-Vorlage befindet sich in einem öffentlichen Blobcontainer. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Geben Sie auf dem Blatt **Parameter** folgende Werte ein:
   
   * **Abonnement**: (Wählen Sie Ihr Azure-Abonnement aus).
   * **Ressourcengruppe**: Klicken Sie auf **Vorhandene Verwenden**, und geben Sie die gleiche Ressourcengruppe an wie die, die Sie verwendet haben.  Beispiel: contosohdirg. 
   * **Standort**: Geben Sie den Standort einer Ressourcengruppe an.
   * **Clustername**: Geben Sie einen Namen für den Hadoop-Cluster ein, den Sie erstellen möchten. Beispiel: contosohdicluster.
   * **Clustertyp**: Wählen Sie einen Clustertyp aus.  Der Standardwert lautet **hadoop**.
   * **Standort**: Wählen Sie einen Standort für den Cluster aus.  Das standardmäßige Speicherkonto verwendet den gleichen Speicherort.
   * **Cluster Worker Node count (Anzahl von Workerknoten im Cluster)**: Wählen Sie die Anzahl der Workerknoten aus.
   * **Cluster-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet **admin**.
   * **SSH-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet **sshuser**.  Sie können auch einen anderen Namen festlegen. 
   * **Virtual Network-ID**: /subscriptions/&lt;SubscriptionID&gt;/resourceGroups/&lt;ResourceGroupName&gt;/providers/Microsoft.Network/virtualNetworks/&lt;VNetName&gt;
   * **Virtuelles Netzwerk/Subnetz**: /subscriptions/&lt;SubscriptionID&gt;/resourceGroups/&lt;ResourceGroupName&gt;/providers/Microsoft.Network/virtualNetworks/&lt;VNetName&gt;/subnets/Subnet1
   * **Domänenname**: contoso.onmicrosoft.com
   * **Organization Unit DN (Organisationseinheit DN)**: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com
   * **Clusterbenutzergruppen-DNs**: [\"HiveUsers\"]
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (Geben Sie den Benutzernamen des Administrators der Domäne ein)
   * **DomainAdminPassword**: (Geben Sie das Kennwort des Administrators der Domäne ein)
   * **Ich stimme den oben genannten Geschäftsbedingungen zu**: (aktivieren)
   * **An Dashboard anheften**: (aktivieren)
3. Klicken Sie auf **Kaufen**. Es wird eine neue Kachel mit dem Titel **Deploying Template deployment (Vorlagenbereitstellung bereitstellen)** angezeigt. Das Erstellen eines Clusters dauert ca. 20 Minuten. Wenn der Cluster erstellt wurde, öffnen Sie ihn, indem Sie im Portal auf das Clusterblatt klicken.

Löschen Sie den Cluster, wenn Sie das Tutorial beendet haben. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen. Anweisungen zum Löschen eines Clusters finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](apache-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Verwenden von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

