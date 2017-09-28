---
title: Verwenden von SQL-Datenbanken in Azure Stack | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie in wenigen Schritten SQL-Datenbanken als Dienst in Azure Stack und den SQL Server-Ressourcenanbieteradapter bereitstellen können."
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: JeffGo
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 018d556d52aa1a1f436460d9811c43f9b45bd440
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Verwenden von SQL-Datenbanken in Microsoft Azure Stack


Verwenden Sie den SQL Server-Ressourcenanbieteradapter, um SQL-Datenbanken als Dienst in Azure Stack verfügbar zu machen. Nach der Installation des Ressourcenanbieters und des Herstellens einer Verbindung desselben mit einer oder mehreren SQL Server-Instanzen können Sie und Ihre Benutzer Datenbanken für native Cloud-Apps, auf SQL basierende Websites und Workloads erstellen, ohne jeweils einen virtuellen Computer bereitstellen zu müssen, der SQL Server hostet.

Der Ressourcenanbieter unterstützt nicht alle Datenbankverwaltungsfunktionen von [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/). Beispielsweise werden Pools für elastische Datenbanken und das Skalieren der Datenbankleistung nicht unterstützt. Die API ist nicht mit SQL DB kompatibel.


## <a name="sql-server-resource-provider-adapter-architecture"></a>Architektur des SQL Server-Ressourcenanbieteradapters
Der Ressourcenanbieter basiert nicht auf allen Datenverwaltungsfunktionen von Azure SQL-Datenbank und bietet nicht alle davon an. Pool für elastische Datenbanken und das automatische Skalieren der Datenbankleistung nach oben oder unten sind beispielsweise nicht verfügbar. Der Ressourcenanbieter unterstützt jedoch ähnliche Vorgänge für das Erstellen, Lesen, Aktualisieren und Löschen.

Der Ressourcenanbieter besteht aus drei Komponenten:

- **Dem virtuellen Computer des SQL-Ressourcenanbieteradapters**, ein virtueller Windows-Computer, der die Anbieterdienste ausführt
- **Dem Ressourcenanbieter selbst**, der die Bereitstellungsanforderungen verarbeitet und Datenbankressourcen verfügbar macht
- **Servern, die SQL Server hosten**, die Kapazität für Datenbanken bereitstellen und als „Hostserver“ bezeichnet werden 

In diesem Release wird keine SQL-Instanz mehr erstellt. Sie müssen eine oder mehrere erstellen und/oder Zugriff auf externe SQL-Instanzen bereitstellen. Eine Reihe von Optionen ist für Sie verfügbar, einschließlich der Vorlagen im [Azure Stack-Schnellstartkatalog](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) und der Marketplace-Elemente. 

>[!NOTE]
Wenn Sie SQL Marketplace-Elemente heruntergeladen haben, versichern Sie sich, dass Sie auch die SQL-IaaS-Erweiterung herunterladen. Andernfalls werden diese nicht bereitgestellt.


## <a name="deploy-the-resource-provider"></a>Bereitstellen des Ressourcenanbieters

1. Wenn nicht bereits geschehen, registrieren Sie Ihr Development Kit und laden das Image „Windows Server 2016 EVAL“ über die Marketplace-Verwaltung herunter. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden. Die .NET 3.5-Laufzeit ist nicht mehr erforderlich.

2. [Laden Sie die Binärdatei des SQL-Ressourcenanbieters herunter](https://aka.ms/azurestacksqlrp), und extrahieren Sie sie auf dem Development Kit-Host.

3. Melden Sie sich beim Development Kit-Host an, und extrahieren Sie die SQL RP-Installationsdatei in ein temporäres Verzeichnis.

4. Das Azure Stack-Stammzertifikat wird abgerufen, und im Rahmen dieses Prozesses wird ein selbstsigniertes Zertifikat erstellt. 

    __Optional:__ Wenn Sie eigene Zertifikate bereitstellen müssen, bereiten Sie diese vor, und kopieren Sie sie in ein lokales Verzeichnis, wenn Sie die Zertifikate (die an das Installationsskript übergeben werden) anpassen möchten. Sie benötigen folgende Zertifikate:

    a. Ein Platzhalterzertifikat für *.dbadapter.\<Region\>.\<external fqdn\>. Diesem Zertifikat muss so vertraut werden, als würde es z.B. von einer Zertifizierungsstelle ausgestellt werden (d.h. die Vertrauenskette muss vorhanden sein, ohne dass Zwischenzertifikate erforderlich sind). Ein einzelnes Sitezertifikat kann mit dem expliziten Namen des virtuellen Computers verwendet werden, den Sie während der Installation bereitstellen.

    b. Das von Azure Resource Manager für Ihre Azure Stack-Instanz verwendete Stammzertifikat. Wenn es nicht gefunden wird, wird das Stammzertifikat abgerufen.


5. Öffnen Sie eine **neue** PowerShell-Konsole mit erhöhten Rechten, und wechseln Sie zu dem Verzeichnis, in dem Sie die Dateien extrahiert haben. Verwenden Sie ein neues Fenster, um Probleme aufgrund von falschen PowerShell-Modulen, die bereits in Ihrem System geladen sind, zu vermeiden.

6. Wenn Sie andere Versionen der AzureRm- oder AzureStack-PowerShell-Module als 1.2.9 oder 1.2.10 installiert haben, werden Sie aufgefordert, diese zu entfernen, ansonsten wird die Installation nicht fortgesetzt. Dies schließt Versionen ab 1.3 ein.

7. Führen Sie das Skript „DeploySqlProvider.ps1“ mit den unten aufgeführten Parametern aus.

Mit diesem Skript werden folgende Schritte ausgeführt:

* Laden Sie bei Bedarf eine kompatible Version von Azure PowerShell herunter.
* Laden Sie die Zertifikate und andere Artefakte in Ihr Azure Stack-Speicherkonto hoch.
* Veröffentlichen Sie Katalogpakete, damit Sie SQL-Datenbanken über den Katalog bereitstellen können.
* Stellen Sie einen virtuellen Computer mithilfe des in Schritt 1 erstellten Windows Server 2016-Images bereit, und installieren Sie den Ressourcenanbieter.
* Registrieren Sie einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
* Registrieren Sie Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz (Mandant und Administrator).

> [!NOTE]
> Wenn die Installation länger als 90 Minuten dauert, schlägt sie möglicherweise fehl, und auf dem Bildschirm sowie in der Protokolldatei wird eine Fehlermeldung angezeigt. Die Bereitstellung wird jedoch ab dem fehlgeschlagenen Schritt wiederholt. Systeme, die den empfohlenen Arbeitsspeicher- und Core-Spezifikationen nicht entsprechen, können den SQL-Ressourcenanbieter möglicherweise nicht bereitstellen.
>

Hier ist ein Beispiel, das Sie über die PowerShell-Eingabeaufforderung ausführen können (ändern Sie jedoch die Kontoinformationen und Portalendpunkte entsprechend):

```
# Install the AzureRM.Bootstrapper module
Install-Module -Name AzureRm.BootStrapper -Force

# Installs and imports the API Version Profile required by Azure Stack into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.10 -Force

# Download the Azure Stack Tools from GitHub and set the environment
cd c:\
Invoke-Webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip -OutFile master.zip
Expand-Archive master.zip -DestinationPath . -Force

# This endpoint may be different for your installation
Import-Module C:\AzureStack-Tools-master\Connect\AzureStack.Connect.psm1
Add-AzureRmEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external" 

# For AAD, use the following
$tenantID = Get-AzsDirectoryTenantID -AADTenantName "<your directory name>" -EnvironmentName AzureStackAdmin

# For ADFS, replace the previous line with
# $tenantID = Get-AzsDirectoryTenantID -ADFS -EnvironmentName AzureStackAdmin

$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ("admin@mydomain.onmicrosoft.com", $AdminPass)

# change this as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files 
# and adjust the endpoints
<extracted file directory>\DeploySQLProvider.ps1 -DirectoryTenantID $tenantID -AzCredential $AdminCreds -VMLocalCredential $vmLocalAdminCreds -ResourceGroupName "SqlRPRG" -VmName "SqlVM" -ArmEndpoint "https://adminmanagement.local.azurestack.external" -TenantArmEndpoint "https://management.local.azurestack.external" -DefaultSSLCertificatePassword $PfxPass
 ```

### <a name="deploysqlproviderps1-parameters"></a>Parameter „DeploySqlProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn sie nicht festgelegt werden oder eine Parameterüberprüfung fehlschlägt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | Beschreibung | Kommentar oder Standardwert |
| --- | --- | --- |
| **DirectoryTenantID** | Die Azure- oder ADFS-Verzeichnis-ID (GUID) | _erforderlich_ |
| **AzCredential** | Geben Sie die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto an. Sie müssen die gleichen Anmeldeinformationen wie für das Bereitstellen von Azure Stack verwenden. | _erforderlich_ |
| **VMLocalCredential** | Definieren Sie die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. Das Kennwort wird ebenfalls für das SQL-**sa**-Konto verwendet. | _erforderlich_ |
| **ResourceGroupName** | Definieren Sie einen Namen für eine Ressourcengruppe, in der die von diesem Skript erstellten Elemente gespeichert werden sollen. Zum Beispiel *SqlRPRG*. |  _erforderlich_ |
| **VmName** | Definieren Sie den Namen für den virtuellen Computer, auf dem der Ressourcenanbieter installiert werden soll. Zum Beispiel *SqlVM*. |  _erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatdateien müssen ebenfalls in diesem Verzeichnis abgelegt werden. | _optional_ |
| **DefaultSSLCertificatePassword** | Kennwort für das PFX-Zertifikat | _erforderlich_ |
| **MaxRetryCount** | Definieren Sie, wie oft Sie jeden Vorgang wiederholen möchten, wenn ein Fehler vorliegt.| 2 |
| **RetryDuration** | Definieren Sie das Timeout zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernen Sie den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein |
| **DebugMode** | Verhindert die automatische Bereinigung bei einem Fehler | Nein |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals

> [!NOTE]
>  Nachdem das Installationsskript abgeschlossen ist, müssen Sie das Portal aktualisieren, damit das Blatt „Administrator“ angezeigt wird.


1. Klicken Sie auf dem Desktop der VM-Konsole auf **Microsoft Azure Stack-Portal**, und melden Sie sich als Dienstadministrator beim Portal an.

2. Überprüfen Sie, ob die Bereitstellung erfolgreich war. Klicken Sie auf die **Ressourcengruppen** &gt;, die Sie verwendet haben, und versichern Sie sich dann, dass der Bereich „Zusammenfassung“ des Blatts **_Datum_ (Erfolgreich)** anzeigt.

      ![Überprüfen der Bereitstellung des SQL-Ressourcenanbieters](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="provide-capacity-by-connecting-to-a-hosting-sql-server"></a>Bereitstellen von Kapazität durch Herstellen einer Verbindung mit einem SQL-Hostserver

1. Melden Sie sich als Dienstadministrator beim Azure Stack-Portal an.

2. Erstellen Sie einen virtuellen SQL-Computer, wenn Sie nicht bereits über einen verfügen. Die Marketplace-Verwaltung bietet einige Optionen für das Bereitstellen von SQL-VMs.

3. Klicken Sie auf **Ressourcenanbieter** &gt; **SQLAdapter** &gt; **Hostserver** &gt; **+Add** (+Hinzufügen).

    Auf dem Blatt **SQL Hosting Servers** (SQL-Hostserver) können Sie den SQL-Server-Ressourcenanbieter mit tatsächlichen SQL-Serverinstanzen verbinden, die als Back-End des Ressourcenanbieters dienen.

    ![Hostserver](./media/azure-stack-sql-rp-deploy/sqlrp-hostingserver.PNG)

4. Füllen Sie das Formular mit den Verbindungsdetails für Ihre SQL Server-Instanz aus.

    ![Neue Hostserver](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.PNG)

    > [!NOTE]
    > Sofern die SQL-Instanz für den Azure Resource Manager-Mandanten und -Administrator zugänglich ist, kann sie vom Ressourcenanbieter gesteuert werden. Die SQL-Instanz __muss__ ausschließlich dem Ressourcenanbieter zugeordnet sein.

5. Wenn Sie Server hinzufügen, müssen Sie diese einer neuen oder vorhandenen SKU hinzufügen, um Dienstangebote zu unterscheiden. Beispielsweise können Sie über eine SQL Enterprise-Instanz verfügen, die Datenbankkapazität und automatische Sicherung bereitstellt, Hochleistungsserver für einzelne Abteilungen reservieren usw. Der SKU-Name muss die Eigenschaften beschreiben, damit Mandanten ihre Datenbanken entsprechend platzieren können, und alle Hostserver in einer SKU müssen denselben Funktionsumfang aufweisen.

    Beispiel:

    ![SKUs](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

>[!NOTE]
Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst eine Datenbank erstellen, wenn die SKU erstellt wurde.


## <a name="create-your-first-sql-database-to-test-your-deployment"></a>Erstellen Ihrer ersten SQL-Datenbank zum Testen der Bereitstellung

1. Melden Sie sich als Dienstadministrator beim Azure Stack-Verwaltungsportal an.

2. Klicken Sie auf **+ New** (+Neu) &gt;**Daten und Speicher** &gt; **SQL Server Database (Preview)** (SQL Server-Datenbank (Vorschauversion)) &gt; **Hinzufügen**.

3. Füllen Sie das Formular mit den Details der Datenbank aus, einschließlich eines **Datenbanknamens** und der **maximalen Größe**, und ändern Sie die anderen Parameter bei Bedarf. Sie werden aufgefordert, eine SKU für Ihre Datenbank auszuwählen. Wenn Hostserver hinzugefügt werden, werden diese einer SKU zugewiesen, und Datenbanken werden in dem Pool von Hostservern erstellt, aus denen die SKU besteht.

    ![Neue Datenbank](./media/azure-stack-sql-rp-deploy/newsqldb.png)


4. Füllen Sie die Anmeldeeinstellungen aus: **Datenbankanmeldung** und **Kennwort**. Dabei handelt es sich um Anmeldeinformationen für die SQL-Authentifizierung, die nur für Ihren Zugriff auf diese Datenbank erstellt werden. Der Benutzername für die Anmeldung muss global eindeutig sein. Erstellen Sie entweder eine neue Anmeldeeinstellung, oder wählen Sie eine vorhandene aus. Sie können die Anmeldeeinstellungen für andere Datenbanken wiederverwenden, die dieselbe SKU verwenden.

    ![Erstellen einer neuen Datenbankanmeldung](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. Übermitteln Sie das Formular, und warten Sie, bis die Bereitstellung abgeschlossen ist.

    Beachten Sie im resultierenden Blatt das Feld „Verbindungszeichenfolge“. Sie können diese Zeichenfolge in jeder Anwendung (z.B. eine Web-App) in Azure Stack verwenden, die den Zugriff auf SQL Server erfordert.

    ![Abrufen der Verbindungszeichenfolge](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="add-capacity"></a>Hinzufügen einer Kapazität

Fügen Sie die Kapazität hinzu, indem Sie zusätzliche SQL-Hosts im Azure Stack-Portal hinzufügen und diese der entsprechenden SKU zuordnen. Wenn Sie eine andere Instanz von SQL statt der verwenden möchten, die auf virtuellen Computer mit dem Ressourcenanbieter installiert ist, klicken Sie auf **Ressourcenanbieter** &gt; **SQLAdapter** &gt; **SQL Hosting Servers** (SQL-Hostserver) &gt; **+Add** (+Hinzufügen).

## <a name="making-sql-databases-available-to-tenants"></a>Verfügbarmachen von SQL-Datenbanken für Mandanten

Erstellen Sie Pläne und Angebote, um SQL-Datenbanken für Mandanten zur Verfügung zu stellen. Sie müssen einen Plan erstellen, den Dienst „Microsoft.SqlAdapter“ zum Plan hinzufügen und ein vorhandenes Kontingent hinzufügen oder ein neues erstellen. Wenn Sie ein Kontingent erstellen, können Sie die Kapazität angeben, die Sie dem Mandanten gewähren möchten.
    ![Erstellen von Plänen und Angeboten, die Datenbanken umfassen](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="tenant-usage-of-the-resource-provider"></a>Mandantennutzung des Ressourcenanbieters

Self-Service-Datenbanken werden über das Mandantenportal bereitgestellt.

## <a name="removing-the-sql-adapter-resource-provider"></a>Entfernen des SQL-Adapter-Ressourcenanbieters

Zum Entfernen des Ressourcenanbieters müssen unbedingt zuerst alle Abhängigkeiten entfernt werden.

1. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des Ressourcenanbieters heruntergeladen haben.

2. Alle Mandantendatenbanken müssen aus dem Ressourcenanbieter gelöscht werden (die Daten werden dabei nicht gelöscht). Diese Aktion muss von den Mandanten selbst ausgeführt werden.

3. Der Administrator muss die Hostserver aus dem SQL-Adapter löschen.

4. Der Administrator muss alle Pläne löschen, die auf den SQL-Adapter verweisen.

5. Der Administrator muss alle Kontingente löschen, die dem SQL-Adapter zugewiesen sind.

6. Führen Sie das Bereitstellungsskript mit dem Parameter „-Uninstall“, Azure Resource Manager-Endpunkten, DirectoryTenantID und Anmeldeinformationen für das Dienstadministratorkonto erneut aus.



## <a name="next-steps"></a>Nächste Schritte


Probieren Sie andere [PaaS-Dienste](azure-stack-tools-paas-services.md) wie den [MySQL Server-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md) und den [App Services-Ressourcenanbieter](azure-stack-app-service-overview.md) aus.

