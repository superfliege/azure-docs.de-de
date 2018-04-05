---
title: Schützen von Daten, die im Azure Data Lake Store gespeichert sind | Microsoft Docs
description: Informationen zum Schützen von Daten im Azure Data Lake-Speicher mithilfe von Gruppen und Zugriffssteuerungslisten
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 4d926ee08da593e590aa77a2ca09d8d1e1f6bb46
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Schützen von Daten, die im Azure Data Lake-Speicher gespeichert sind
Das Schützen von Daten im Azure Data Lake-Speicher ist ein Ansatz, der drei Schritte umfasst.  Die rollenbasierte Zugriffssteuerung (RBAC) sowie die Zugriffssteuerungslisten (ACLs) müssen so festgelegt werden, dass der Zugriff auf Daten für Benutzer und Sicherheitsgruppen vollständig aktiviert ist.

1. Beginnen Sie, indem Sie in Azure Active Directory (AAD) Sicherheitsgruppen erstellen. Diese Sicherheitsgruppen werden verwendet, um die rollenbasierte Zugriffssteuerung im Azure-Portal zu implementieren. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Microsoft Azure](../active-directory/role-based-access-control-configure.md).
2. Weisen Sie die AAD-Sicherheitsgruppen dem Azure Data Lake-Speicherkonto zu. Hiermit werden der Zugriff auf das Data Lake-Speicherkonto über das Portal und die Verwaltungsvorgänge über das Portal oder APIs gesteuert.
3. Weisen Sie die AAD-Sicherheitsgruppen als Zugriffssteuerungslisten im Data Lake-Speicher-Dateisystem zu.
4. Darüber hinaus können Sie auch einen IP-Adressbereich für Clients festlegen, die auf die Daten im Data Lake-Speicher zugreifen können.

Dieser Artikel enthält eine Anleitung zur Verwendung des Azure-Portals für die oben genannten Aufgaben. Ausführliche Informationen zur Sicherheitsimplementierung auf Konto- und Datenebene durch Data Lake-Speicher finden Sie unter [Security in Azure Data Lake Store](data-lake-store-security-overview.md)(Sicherheit im Azure Data Lake-Speicher). Ausführliche Informationen zur Implementierung von ACLs im Azure Data Lake Store finden Sie unter [Übersicht über die Zugriffssteuerung in Data Lake Store](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Azure Data Lake Store-Konto**. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit dem Azure Data Lake-Speicher](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Erstellen von Sicherheitsgruppen in Azure Active Directory
Eine Anleitung zum Erstellen von AAD-Sicherheitsgruppen und zum Hinzufügen von Benutzern zur Gruppe finden Sie unter [Verwalten von Sicherheitsgruppen in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

> [!NOTE] 
> Sie können im Azure-Portal sowohl Benutzer als auch andere Gruppen zu einer Gruppe in Azure AD hinzufügen. Verwenden Sie zum Hinzufügen eines Dienstprinzipals zu einer Gruppe jedoch das [PowerShell-Modul von Azure AD](../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Zuweisen von Benutzern oder Sicherheitsgruppen zu Azure Data Lake-Speicherkonten
Wenn Sie Benutzer oder Sicherheitsgruppen Azure Data Lake-Speicherkonten zuweisen, steuern Sie den Zugriff auf die Verwaltungsvorgänge des Kontos, indem Sie das Azure-Portal und die Azure Resource Manager-APIs verwenden. 

1. Öffnen Sie ein Azure Data Lake-Speicherkonto. Klicken Sie im linken Bereich auf **Alle Ressourcen** und dann auf dem Blatt „Alle Ressourcen“ auf den Namen des Kontos, dem Sie einen Benutzer oder eine Sicherheitsgruppe zuweisen möchten.

2. Klicken Sie auf dem Blatt Ihres Data Lake Store-Kontos auf **Zugriffssteuerung (IAM)**. Auf dem Blatt werden standardmäßig die Abonnementbesitzer als Besitzer aufgeführt.
   
    ![Zuweisen einer Sicherheitsgruppe zum Azure Data Lake Store-Konto](./media/data-lake-store-secure-data/adl.select.user.icon.png "Zuweisen einer Sicherheitsgruppe zum Azure Data Lake Store-Konto")

3. Klicken Sie auf dem Blatt **Zugriffssteuerung (IAM)** auf **Hinzufügen**, um das Blatt **Berechtigungen hinzufügen** zu öffnen. Wählen Sie auf dem Blatt **Berechtigungen hinzufügen** eine **Rolle** für den Benutzer bzw. die Gruppe aus. Suchen Sie nach der Sicherheitsgruppe, die Sie zuvor in Azure Active Directory erstellt haben, und wählen Sie sie aus. Wenn Sie über viele Benutzer und Gruppen verfügen, in denen Sie suchen können, können Sie das Textfeld **Auswählen** zum Filtern nach dem Gruppennamen verwenden. 
   
    ![Hinzufügen einer Rolle für den Benutzer](./media/data-lake-store-secure-data/adl.add.user.1.png "Hinzufügen einer Rolle für den Benutzer")
   
    Mit der Rolle **Besitzer** und **Mitwirkender** wird Zugriff auf viele verschiedene Verwaltungsfunktionen des Data Lake-Kontos gewährt. Benutzer, die mit Daten im Data Lake interagieren, aber dennoch Informationen zur Kontoverwaltung anzeigen müssen, können Sie der Rolle **Leser** hinzufügen. Der Umfang dieser Rollen ist auf die Verwaltungsvorgänge beschränkt, die sich auf das Azure Data Lake-Speicherkonto beziehen.
   
    Für Datenvorgänge wird mithilfe von individuellen Dateisystemberechtigungen definiert, welche Möglichkeiten Benutzer haben. Aus diesem Grund kann ein Benutzer mit der Rolle „Leser“ nur Verwaltungseinstellungen anzeigen, die dem Konto zugeordnet sind. Potenziell kann er aber basierend auf den zugewiesenen Dateisystemberechtigungen Daten lesen und schreiben. Die Dateisystemberechtigungen des Data Lake Store sind unter [Zuweisen von Sicherheitsgruppen als Zugriffssteuerungslisten zum Azure Data Lake Store-Dateisystem](#filepermissions) beschrieben.

    > [!IMPORTANT]
    > Nur in der Rolle **Besitzer** wird der Dateisystemzugriff automatisch aktiviert. Für alle anderen Rollen wie **Mitwirkender** oder **Leser** sind Zugriffssteuerungslisten erforderlich, um die Ebenen des Zugriffs auf Ordner und Dateien zu aktivieren.  Die Rolle **Besitzer** umfasst Administratorrechte für Dateien und Ordner, die über Zugriffssteuerungslisten nicht überschrieben werden können. Weitere Informationen zum Zuordnen von RBAC-Richtlinien zum Datenzugriff finden Sie unter [RBAC für die Kontoverwaltung](data-lake-store-security-overview.md#rbac-for-account-management).

4. Wenn Sie eine Gruppe oder einen Benutzer hinzufügen möchten, die bzw. der auf dem Blatt **Berechtigungen hinzufügen** nicht aufgeführt ist, können Sie sie einladen, indem Sie die entsprechende E-Mail-Adresse im Textfeld **Auswählen** eingeben und sie dann in der Liste auswählen.
   
    ![Hinzufügen einer Sicherheitsgruppe](./media/data-lake-store-secure-data/adl.add.user.2.png "Hinzufügen einer Sicherheitsgruppe")
   
5. Klicken Sie auf **Speichern**. Die Sicherheitsgruppe sollte wie unten dargestellt hinzugefügt werden.
   
    ![Sicherheitsgruppe hinzugefügt](./media/data-lake-store-secure-data/adl.add.user.3.png "Sicherheitsgruppe hinzugefügt")

6. Der Benutzer bzw. die Sicherheitsgruppe verfügt jetzt über Zugriff auf den Azure Data Lake-Speicher. Wenn Sie bestimmten Benutzern Zugriff gewähren möchten, können Sie sie der Sicherheitsgruppe hinzufügen. Wenn Sie den Zugriff für einen Benutzer widerrufen möchten, können Sie sie auch aus der Sicherheitsgruppe entfernen. Sie können einem Konto auch mehrere Sicherheitsgruppen zuweisen. 

## <a name="filepermissions"></a>Zuweisen von Benutzern oder Sicherheitsgruppen als Zugriffssteuerungslisten zum Azure Data Lake Store-Dateisystem
Indem Sie dem Azure Data Lake-Dateisystem Benutzer oder Sicherheitsgruppen zuweisen, legen Sie die Zugriffssteuerung für die im Azure Data Lake-Speicher gespeicherten Daten fest.

1. Klicken Sie auf dem Blatt Ihres Data Lake-Speicherkontos auf **Daten-Explorer**.
   
    ![Anzeigen von Daten über Daten-Explorer](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Anzeigen von Daten über Daten-Explorer")
2. Klicken Sie auf dem Blatt **Daten-Explorer** auf den Ordner, für den Sie die Zugriffssteuerungsliste konfigurieren möchten, und klicken Sie dann auf **Zugriff**. Um einer Datei Zugriffssteuerungslisten zuzuweisen, müssen Sie zunächst auf die Datei klicken, um sie in der Vorschau anzuzeigen, und dann auf dem Blatt **Dateivorschau** auf **Zugriff** klicken.
   
    ![Festlegen von Zugriffssteuerungslisten für das Data Lake-Dateisystem](./media/data-lake-store-secure-data/adl.acl.1.png "Festlegen von Zugriffssteuerungslisten für das Data Lake-Dateisystem")
3. Auf dem Blatt **Zugriff** sind die Besitzer und zugewiesenen Berechtigungen aufgeführt, die dem Stamm bereits zugewiesen wurden. Klicken Sie auf das Symbol **Hinzufügen**, um zusätzliche Zugriffs-ACLs hinzuzufügen.
    > [!IMPORTANT]
    > Durch Festlegen von Zugriffsberechtigungen für eine einzelne Datei wird einem Benutzer oder einer Gruppe nicht zwingend der Zugriff auf diese Datei gewährt. Der zugewiesene Benutzer bzw. die zugewiesene Gruppe muss Zugriff auf den Pfad zu der Datei haben. Weitere Informationen und Beispiele finden Sie unter [Allgemeine Szenarien im Zusammenhang mit Berechtigungen](data-lake-store-access-control.md#common-scenarios-related-to-permissions).
   
    ![Auflisten von Standardzugriff und benutzerdefiniertem Zugriff](./media/data-lake-store-secure-data/adl.acl.2.png "Auflisten von Standardzugriff und benutzerdefiniertem Zugriff")
   
   * **Besitzer** und **Beliebige andere Person** bieten Zugriff im UNIX-Stil, bei dem Sie Lesen, Schreiben und Ausführen (read, write, execute – rwx) für drei unterschiedliche Benutzerklassen angeben: Besitzer, Gruppe und andere.
   * **Zugewiesene Berechtigungen** entspricht den POSIX-Zugriffssteuerungslisten, bei denen Sie Berechtigungen nicht nur für den Besitzer oder die Gruppe der Datei, sondern für speziell benannte Benutzer oder Gruppen festlegen können. 
     
     Weitere Informationen finden Sie unter [HDFS-Zugriffssteuerungslisten](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)(in englischer Sprache). Weitere Informationen zur Implementierung von ACLs in Data Lake Store finden Sie unter [Zugriffssteuerung in Data Lake Store](data-lake-store-access-control.md).
4. Klicken Sie auf das Symbol **Hinzufügen**, um das Blatt **Berechtigungen zuweisen** zu öffnen. Klicken Sie auf diesem Blatt auf **Benutzer oder Gruppe auswählen**, und suchen Sie dann auf dem Blatt **Benutzer oder Gruppe auswählen** nach der Sicherheitsgruppe, die Sie zuvor in Azure Active Directory erstellt haben. Wenn Sie über viele Gruppen verfügen, in denen Sie suchen können, können Sie das Textfeld oben zum Filtern nach dem Gruppennamen verwenden. Klicken Sie auf die Gruppe, die Sie hinzufügen möchten, und klicken Sie dann auf **Auswählen**.
   
    ![Hinzufügen einer Gruppe](./media/data-lake-store-secure-data/adl.acl.3.png "Hinzufügen einer Gruppe")
5. Klicken Sie auf **Berechtigungen auswählen**, wählen Sie die Berechtigungen aus, und legen Sie fest, ob die Berechtigungen rekursiv angewendet werden sollen und ob sie als Zugriffs-ACL und/oder Standard-ACL zugewiesen werden sollen. Klicken Sie auf **OK**.
   
    ![Zuweisen von Berechtigungen zu einer Gruppe](./media/data-lake-store-secure-data/adl.acl.4.png "Zuweisen von Berechtigungen zu einer Gruppe")
   
    Weitere Informationen zu Berechtigungen in Data Lake Store und zu Standard- und Zugriffs-ACLs finden Sie unter [Zugriffssteuerung in Data Lake Store](data-lake-store-access-control.md).
6. Nach dem Klicken auf **OK** auf dem Blatt **Berechtigungen auswählen** werden die neu hinzugefügte Gruppe und die zugehörigen Berechtigungen auf dem Blatt **Zugriff** aufgeführt.
   
    ![Zuweisen von Berechtigungen zu einer Gruppe](./media/data-lake-store-secure-data/adl.acl.5.png "Zuweisen von Berechtigungen zu einer Gruppe")
   
   > [!IMPORTANT]
   > Im aktuellen Release können Sie bis zu 28 Einträge unter **Zugewiesene Berechtigungen** hinzufügen. Wenn Sie mehr als 28 Benutzer hinzufügen möchten, sollten Sie Sicherheitsgruppen erstellen, die Benutzer den Sicherheitsgruppen hinzufügen und für das Data Lake Store-Konto den Zugriff auf diese Sicherheitsgruppen gewähren.
   > 
   > 
7. Falls erforderlich, können Sie die Zugriffsberechtigungen auch ändern, nachdem Sie die Gruppe hinzugefügt haben. Deaktivieren oder aktivieren Sie das Kontrollkästchen für jeden Berechtigungstyp (Lesen, Schreiben, Ausführen) in Abhängigkeit davon, ob Sie die Berechtigung für die Sicherheitsgruppe entfernen oder zuweisen möchten. Klicken Sie auf **Speichern**, um die Änderungen zu speichern, oder auf **Verwerfen**, um die Änderungen rückgängig zu machen.

## <a name="set-ip-address-range-for-data-access"></a>Festlegen des IP-Adressbereichs für den Datenzugriff
Mit dem Azure Data Lake-Speicher können Sie den Zugriff auf Ihren Datenspeicher auf Netzwerkebene noch weiter einschränken. Sie können die Firewall aktivieren, eine IP-Adresse angeben oder einen IP-Adressbereich für Ihre vertrauenswürdigen Clients definieren. Nach der Aktivierung können nur Clients, deren IP-Adressen innerhalb des definierten Bereichs liegen, eine Verbindung mit dem Speicher herstellen.

![Firewall-Einstellungen und IP-Zugriff](./media/data-lake-store-secure-data/firewall-ip-access.png "Firewalleinstellungen und IP-Adresse")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Entfernen von Sicherheitsgruppen für ein Azure Data Lake-Speicherkonto
Wenn Sie Sicherheitsgruppen aus Azure Data Lake-Speicherkonten entfernen, ändern Sie lediglich den Zugriff auf die Verwaltungsvorgänge des Kontos, indem Sie das Azure-Portal und die Azure Resource Manager-APIs verwenden.  

Der Zugriff auf Daten ist unverändert und wird weiterhin über die ACLs verwaltet.  Eine Ausnahme bilden Benutzer oder Gruppen in der Rolle „Besitzer“.  Benutzer oder Gruppen, die aus der Rolle „Besitzer“ entfernt wurden, sind keine Administratoren mehr, und ihr Zugriff beschränkt sich auf den Zugriff auf ACL-Einstellungen. 

1. Klicken Sie auf dem Blatt Ihres Data Lake Store-Kontos auf **Zugriffssteuerung (IAM)**. 
   
    ![Zuweisen einer Sicherheitsgruppe zum Azure Data Lake-Konto](./media/data-lake-store-secure-data/adl.select.user.icon.png "Zuweisen einer Sicherheitsgruppe zum Azure Data Lake-Konto")
2. Klicken Sie auf dem Blatt **Zugriffssteuerung (IAM)** auf die Sicherheitsgruppe, die Sie entfernen möchten. Klicken Sie auf **Entfernen**.
   
    ![Sicherheitsgruppe entfernt](./media/data-lake-store-secure-data/adl.remove.group.png "Sicherheitsgruppe entfernt")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Entfernen der Zugriffssteuerungslisten von Sicherheitsgruppen aus dem Azure Data Lake-Speicher-Dateisystem
Wenn Sie Zugriffssteuerungslisten von Sicherheitsgruppen aus dem Azure Data Lake Store-Dateisystem entfernen, ändern Sie den Zugriff auf die Daten im Data Lake Store.

1. Klicken Sie auf dem Blatt Ihres Data Lake-Speicherkontos auf **Daten-Explorer**.
   
    ![Erstellen von Verzeichnissen im Data Lake-Konto](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Erstellen von Verzeichnissen im Data Lake-Konto")
2. Klicken Sie auf dem Blatt **Daten-Explorer** auf den Ordner, für den Sie die Zugriffssteuerungsliste entfernen möchten, und klicken Sie dann auf **Zugriff**. Um Zugriffssteuerungslisten für eine Datei zu entfernen, müssen Sie zunächst auf die Datei klicken, um sie in der Vorschau anzuzeigen, und dann auf dem Blatt **Dateivorschau** auf **Zugriff** klicken. 
   
    ![Festlegen von Zugriffssteuerungslisten für das Data Lake-Dateisystem](./media/data-lake-store-secure-data/adl.acl.1.png "Festlegen von Zugriffssteuerungslisten für das Data Lake-Dateisystem")
3. Klicken Sie auf dem Blatt **Zugriff** auf die Sicherheitsgruppe, die Sie entfernen möchten. Klicken Sie auf dem Blatt **Zugriffsdetails** auf **Entfernen**.
   
    ![Zuweisen von Berechtigungen zu einer Gruppe](./media/data-lake-store-secure-data/adl.remove.acl.png "Zuweisen von Berechtigungen zu einer Gruppe")

## <a name="see-also"></a>Weitere Informationen
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
* [Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher](data-lake-store-copy-data-azure-storage-blob.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Erste Schritte mit Data Lake-Speicher mithilfe von PowerShell](data-lake-store-get-started-powershell.md)
* [Erste Schritte mit Data Lake-Speicher mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store](data-lake-store-diagnostic-logs.md)

