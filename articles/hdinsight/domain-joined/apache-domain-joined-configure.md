---
title: "Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das Einrichten und Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: cgronlun
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/15/2018
ms.author: saurinsh
ms.openlocfilehash: b4d71eeb0aab75e67e851f867f194ed7578d0d1c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="configure-domain-joined-hdinsight-sandbox-environment"></a>Konfigurieren einer in die Domäne eingebundenen HDInsight-Sandboxumgebung

Erfahren Sie, wie Sie Azure HDInsight-Cluster mit einem eigenständigen Active Directory-Verzeichnis und [Apache Ranger](http://hortonworks.com/apache/ranger/) einrichten, und profitieren Sie von einer strengen Authentifizierung und umfassenden Richtlinien für die rollenbasierte Zugriffssteuerung (RBAC). Weitere Informationen finden Sie unter [Introduce Domain-joined HDInsight clusters (Einführung in HDInsight-Cluster mit Domänenverknüpfung)](apache-domain-joined-introduction.md). 

> [!IMPORTANT]
> Standardmäßig kann dieses Setup nur mit Azure Storage-Konten verwendet werden. Für die Verwendung mit Azure Data Lake Store müssen Sie Active Directory mit einem neuen Azure Active Directory synchronisieren.

Ohne in die Domäne eingebundene HDInsight-Cluster kann jeder Cluster nur ein Hadoop-HTTP-Benutzerkonto und ein SSH-Benutzerkonto verwenden.  Die Authentifizierung mehrere Benutzer kann folgendermaßen erreicht werden:

-   Eigenständiges Active Directory-Verzeichnis, ausgeführt in Azure-IaaS
-   Azure Active Directory

In diesem Artikel wird die Verwendung eines eigenständigen Active Directory-Verzeichnisses in Azure-IaaS behandelt. Es handelt sich um die einfachste Architektur, die ein Benutzer implementieren kann, um in HDInsight mehrere Benutzer zu unterstützen. Dieser Artikel beschreibt zwei Ansätze für diese Konfiguration:

- Option 1: Verwenden einer Azure-Ressourcenverwaltungsvorlage, um sowohl das eigenständige Active Directory-Verzeichnis als auch den HDInsight-Cluster zu erstellen.
- Option 2: Unterteilung des Prozesses in die folgenden zwei Schritte:
    - Erstellen eines Active Directory-Verzeichnisses mithilfe einer Vorlage
    - Einrichten von LDAPS
    - Erstellen von AD-Benutzern und -Gruppen
    - Erstellen des HDInsight-Clusters

> [!IMPORTANT]
> 
> Oozie ist auf HDInsight mit Domänenverknüpfung nicht aktiviert.

## <a name="prerequisite"></a>Voraussetzung
* Azure-Abonnement

## <a name="option-1-one-step-approach"></a>Option 1: Ansatz mit einem Schritt
In diesem Abschnitt öffnen Sie eine Azure-Ressourcenverwaltungsvorlage aus dem Azure-Portal. Die Vorlage wird verwendet, um ein eigenständiges Active Directory-Verzeichnis und einen HDInsight-Cluster zu erstellen. Aktuell können Sie Hadoop-, Spark-, und Interactive Query-Cluster mit Domäneneinbindung erstellen.

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen: Die Vorlage finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/).
   
    So erstellen Sie einen Spark-Cluster

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fspark%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    So erstellen Sie einen Interactive Query-Cluster

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Finteractivequery%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    So erstellen Sie einen Hadoop-Cluster

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fhadoop%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Geben Sie die Werte ein, aktivieren Sie die Optionen **Ich stimme den oben genannten Geschäftsbedingungen zu** und **An Dashboard anheften**, und klicken Sie anschließend auf **Kaufen**. Zeigen Sie mit dem Mauszeiger auf das Erläuterungssymbol neben den Feldern, um die Beschreibungen anzuzeigen. Die meisten der Werte wurden aufgefüllt. Sie können entweder die Standardwerte oder Ihre eigenen Werte verwenden.

    - **Ressourcengruppe**: Geben Sie einen Namen für die Ressourcengruppe ein.
    - **Standort**: Wählen Sie einen Standort in Ihrer Nähe aus.
    - **Name des neuen Speicherkontos**: Geben Sie den Namen eines Azure Storage-Kontos ein. Dieses neue Speicherkonto wird vom primären Domänencontroller, dem Sicherungsdomänencontroller und dem HDInsight-Cluster als Standardspeicherkonto verwendet.
    - **Administratorbenutzername**: Geben Sie den Benutzernamen des Domänenadministrators ein.
    - **Administratorkennwort**: Geben Sie das Kennwort des Domänenadministrators ein.
    - **Domänenname**: Der Standardname lautet *contoso.com*.  Wenn Sie den Domänennamen ändern, müssen Sie auch die Werte in den Feldern **Zertifikat für sicheres LDAP** und **DN der Organisationseinheit** aktualisieren.
    - **DNS-Präfix**: Geben Sie das DNS-Präfix für die von Load Balancer verwendete öffentliche IP-Adresse ein.
    - **Clustername**: Geben Sie den Namen des HDInsight-Clusters ein.
    - **Clustertyp**: Ändern Sie diesen Wert nicht. Wenn Sie den Clustertyp ändern möchten, verwenden Sie die entsprechende Vorlage im letzten Schritt.
    - **Secure LDAP-Zertifikatkennwort**: Verwenden Sie den Standardwert, es sei denn, Sie ändern das Feld „Secure LDAP-Zertifikat“.
    Einige der Werte sind in der Vorlage hartcodiert, beispielsweise ist die Anzahl von Workerknoteninstanzen auf 2 festgelegt.  Klicken Sie zum Ändern der hartcodierten Werte auf **Vorlage bearbeiten**.

    ![Vorlage zum Bearbeiten von HDInsight-Clustern mit Domäneneinbindung](./media/apache-domain-joined-configure/hdinsight-domain-joined-edit-template.png)

Nachdem die Vorlage vollständig ausgefüllt wurde, werden 23 Ressourcen in der Ressourcengruppe erstellt.

## <a name="option-2-multi-step-approach"></a>Option 2: Ansatz mit mehreren Schritten

Dieser Abschnitt umfasst vier Schritte:

1. Erstellen eines Active Directory-Verzeichnisses mithilfe einer Vorlage
2. Einrichten von LDAPS
3. Erstellen von AD-Benutzern und -Gruppen
4. Erstellen des HDInsight-Clusters

### <a name="create-an-active-directory"></a>Erstellen eines Active Directory-Verzeichnisses

Azure Resource Manager-Vorlagen erleichtern das Erstellen von Azure-Ressourcen. In diesem Abschnitt verwenden Sie eine [Azure-Schnellstartvorlage](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/), um eine neue Gesamtstruktur und eine Domäne mit zwei virtuellen Computern zu erstellen. Die zwei virtuellen Computer fungieren als primärer Domänencontroller und als Sicherungsdomänencontroller.

**So erstellen Sie eine Domäne mit zwei Domänencontrollern**

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Factive-directory-new-domain-ha-2-dc%2Fazuredeploy.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Die Vorlage sieht so aus:

    ![HDInsight mit Domäneneinbindung: Erstellung von Gesamtstruktur, Domäne, virtuellen Computern](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-arm-template.png)

2. Geben Sie die folgenden Werte ein:

    - **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    - **Name der Ressourcengruppe**: Geben Sie den Namen einer Ressourcengruppe ein.  Eine Ressourcengruppe wird verwendet, um Ihre Azure-Ressourcen für ein Projekt zu verwalten.
    - **Standort**: Wählen Sie einen Azure-Standort in Ihrer Nähe aus.
    - **Administratorbenutzername**: Dies ist der Benutzernamen des Domänenadministrators. Dieser Benutzer ist nicht das HTTP-Benutzerkonto Ihres HDInsight-Clusters. Dies ist das Konto, das Sie während des Tutorials verwenden.
    - **Administratorkennwort**: Geben Sie das Kennwort für den Domänenadministrator ein.
    - **Domänenname**: Der Name der Domäne muss ein zweiteiliger Name sein. Beispiele: contoso.com, contoso.local oder hdinsight.test.
    - **DNS-Präfix**: Geben Sie ein DNS-Präfix ein.
    - **PDC-RDP-Port**: (Verwenden Sie für dieses Tutorial den Standardwert.)
    - **BDC-RDP-Port**: (Verwenden Sie für dieses Tutorial den Standardwert.)
    - **Speicherort für Artefakte**: (Verwenden Sie für dieses Tutorial den Standardwert.)
    - **SAS-Token für Artefaktspeicherort**: (Lassen Sie diesen Wert für dieses Tutorial leer.)

Das Erstellen der Ressourcen dauert ca. 20 Minuten.

### <a name="setup-ldaps"></a>Einrichten von LDAPS

Das Lightweight Directory Access Protocol (LDAP) wird zum Lesen und Schreiben in Active Directory verwendet.

**So stellen Sie per Remotedesktop eine Verbindung mit dem Cluster her**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Öffnen Sie die Ressourcengruppe, und öffnen Sie dann den virtuellen Computer des primären Domänencontrollers (PDC). Der Standname des primären Domänencontrollers lautet „adPDC“. 
3. Klicken Sie auf **Verbinden**, um per Remotedesktop eine Verbindung mit dem PDC herzustellen.

    ![HDInsight mit Domäneneinbindung: Verbindungsherstellung mit dem PDC über Remotedesktop](./media/apache-domain-joined-configure/hdinsight-domain-joined-remote-desktop-pdc.png)


**So fügen Sie die Active Directory-Zertifikatdienste hinzu**

4. Öffnen Sie den **Server-Manager**, sofern noch nicht geschehen.
5. Klicken Sie auf **Verwalten** und anschließend auf **Rollen und Features hinzufügen**.

    ![HDInsight mit Domäneneinbindung: Hinzufügen von Rollen und Features](./media/apache-domain-joined-configure/hdinsight-domain-joined-add-roles.png)
5. Klicken Sie auf der Seite „Vorbereitungen“ auf **Weiter**.
6. Wählen Sie **Rollenbasierte oder featurebasierte Installation** und klicken Sie auf **Weiter**.
7. Wählen Sie den PDC aus, und klicken Sie auf **Weiter**.  Der Standname des primären Domänencontrollers lautet „adPDC“.
8. Wählen Sie **Active Directory-Zertifikatdienste** aus.
9. Klicken Sie im Popupdialogfeld auf **Features hinzufügen**.
10. Folgen Sie den Anweisungen des Assistenten, und verwenden Sie für die verbleibenden Schritte des Verfahrens die Standardeinstellungen.
11. Klicken Sie auf **Schließen** , um den Assistenten zu schließen.

**So konfigurieren Sie ein AD-Zertifikat**

1. Klicken Sie im Server-Manager auf das gelbe Benachrichtigungssymbol, und klicken Sie dann auf **Active Directory-Zertifikatdienste konfigurieren**.

    ![HDInsight mit Domäneneinbindung: Konfigurieren des Active Directory-Zertifikats](./media/apache-domain-joined-configure/hdinsight-domain-joined-configure-ad-certificate.png)

2. Klicken Sie links auf **Rollendienste**, wählen Sie **Zertifizierungsstelle** aus, und klicken Sie dann auf **Weiter**.
3. Folgen Sie den Anweisungen des Assistenten, und verwenden Sie für die verbleibenden Schritte des Verfahrens die Standardeinstellungen (klicken Sie im letzten Schritt auf **Konfigurieren**).
4. Klicken Sie auf **Schließen** , um den Assistenten zu schließen.

### <a name="optional-create-ad-users-and-groups"></a>(Optional) Erstellen von AD-Benutzern und -Gruppen

**So erstellen Sie Benutzer und Gruppen in Active Directory**
1. Herstellen einer Verbindung mit dem PDC über Remotedesktop
1. Öffnen Sie **Active Directory-Benutzer und -Computer**.
2. Wählen Sie im linken Bereich Ihren Domänennamen aus.
3. Klicken Sie im Menü oben auf das Symbol **Neuen Benutzer im aktuellen Container erstellen**.

    ![HDInsight mit Domäneneinbindung: Erstellen von Benutzern](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-ad-user.png)
4. Folgen Sie den Anweisungen, um einige Benutzer zu erstellen. Beispiele: hiveuser1 und hiveuser2.
5. Klicken Sie im Menü oben auf das Symbol **Neue Gruppe im aktuellen Container erstellen**.
6. Folgen Sie den Anweisungen, um eine Gruppe namens **HDInsightUsers** zu erstellen.  Diese Gruppe wird verwendet, wenn Sie später in diesem Tutorial einen HDInsight-Cluster erstellen.

> [!IMPORTANT]
> Sie müssen die PDC-VM neu starten, bevor Sie einen HDInsight-Cluster mit Domäneneinbindung erstellen.

### <a name="create-an-hdinsight-cluster-in-the-vnet"></a>Erstellen eines HDInsight-Clusters im VNET

In diesem Abschnitt verwenden Sie das Azure-Portal, um einen HDInsight-Cluster zu dem virtuellen Netzwerk hinzuzufügen, das Sie zuvor in diesem Tutorial mithilfe der Resource Manager-Vorlage erstellt haben. In diesem Artikel werden nur die spezifischen Informationen für die Konfiguration eines Clusters mit Domäneneinbindung abgedeckt.  Allgemeine Informationen finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](../hdinsight-hadoop-create-linux-clusters-portal.md).  

**So erstellen Sie einen in die Domäne eingebundenen HDInsight-Cluster**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Öffnen Sie die Ressourcengruppe, die Sie zuvor in diesem Tutorial mithilfe der Resource Manager-Vorlage erstellt haben.
3. Fügen Sie einen HDInsight-Cluster zur Ressourcengruppe hinzu.
4. Wählen Sie die Option **Benutzerdefiniert** aus:

    ![HDInsight mit Domäneneinbindung: Option für die benutzerdefinierte Erstellung](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-custom-configuration-option.png)

    Es gibt sechs Abschnitte bei Verwendung der Option für die benutzerdefinierte Konfiguration: Grundeinstellungen, Speicher, Anwendung, Clustergröße, erweiterte Einstellungen, Zusammenfassung.
5. Im Abschnitt **Grundeinstellungen**:

    - Clustertyp: Wählen Sie **Sicherheitspaket für Unternehmen** aus. Aktuell kann das Sicherheitspaket für Unternehmen nur für die folgenden Clustertypen aktiviert werden: Hadoop, Interactive Query und Spark.

        ![HDInsight mit Domäneneinbindung: Sicherheitspaket für Unternehmen](./media/apache-domain-joined-configure/hdinsight-creation-enterprise-security-package.png)
    - Benutzername für Clusteranmeldung: Dies ist der Hadoop-HTTP-Benutzer. Dieses Konto unterscheidet sich vom Domänenadministratorkonto.
    - Ressourcengruppe: Wählen Sie die Ressourcengruppe aus, die Sie zuvor in diesem Tutorial mithilfe der Resource Manager-Vorlage erstellt haben.
    - Standort: Der Standort muss mit demjenigen übereinstimmen, den Sie beim Erstellen des virtuellen Netzwerks und der Domänencontroller mithilfe der Resource Manager-Vorlage verwendet haben.

6. Im Abschnitt **Erweiterte Einstellungen**:

    - Domäneneinstellungen:

        ![HDInsight mit Domäneneinbindung: erweiterte Domäneneinstellungen](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-advanced-domain-settings.png)
        
        - Domänenname: Geben Sie den Domänennamen ein, den Sie in [Active Directory erstellen](#create-an-active-directory) verwendet haben.
        - Domänenbenutzername: Geben Sie den Benutzernamen des AD-Administrators ein, den Sie in [Active Directory erstellen](#create-an-active-directory) verwendet haben.
        - Organisationseinheit: Siehe Beispiel im Screenshot.
        - LDAPS-URL: Siehe Beispiel im Screenshot.
        - Zugriff auf Benutzergruppe: Geben Sie den Namen der Benutzergruppe ein, die Sie in [AD-Benutzer und -Gruppen erstellen](#optionally-createad-users-and-groups) erstellt haben.
    - Virtuelles Netzwerk: Wählen Sie das in [Active Directory erstellen](#create-an-active-directory) erstellte virtuelle Netzwerk aus. Der in der Vorlage verwendete Standardname lautet **adVNET**.
    - Subnetz: Der in der Vorlage verwendete Standardname lautet **adSubnet**.



Löschen Sie den Cluster, wenn Sie das Tutorial beendet haben. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen. Anweisungen zum Löschen eines Clusters finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](apache-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Verwenden von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

