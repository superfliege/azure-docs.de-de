---
title: Verwenden von MySQL-Datenbanken als PaaS in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den MySQL-Ressourcenanbieter bereitstellen und MySQL-Datenbanken als Dienst auf Azure Stack bereitstellen.
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
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: badaefb4986f573362babea81d704bf2be067d6b
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2017
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Verwenden von MySQL-Datenbanken in Microsoft Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können einen MySQL-Ressourcenanbieter in Azure Stack bereitstellen. Nach der Bereitstellung des Ressourcenanbieters können Sie MySQL-Server und Datenbanken über Azure Resource Manager-Bereitstellungsvorlagen erstellen und MySQL-Datenbanken als Dienst bereitstellen. MySQL-Datenbanken, die häufig auf Websites verwendet werden, unterstützen zahlreiche Website-Plattformen. Beispielsweise können Sie nach der Bereitstellung des Ressourcenanbieters WordPress-Websites über die Azure-Web-Apps-Plattform als Dienst (PaaS)-Add-On für Azure Stack erstellen.

Zum Bereitstellen des MySQL-Anbieters in einem System ohne Internetzugriff können Sie die Datei [mysql-connector-net-6.9.9.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.9.9.msi) in eine lokale Freigabe kopieren. Geben Sie anschließend den Freigabenamen an, wenn Sie dazu aufgefordert werden. Sie müssen auch die PowerShell-Module für Azure und Azure Stack installieren.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>Architektur des MySQL Server-Ressourcenanbieteradapters

Der Ressourcenanbieter besteht aus drei Komponenten:

- **Dem virtuellen Computer des MySQL-Ressourcenanbieteradapters**, ein virtueller Windows-Computer, der die Anbieterdienste ausführt.
- **Dem Ressourcenanbieter selbst**, der die Bereitstellungsanforderungen verarbeitet und Datenbankressourcen verfügbar macht.
- **Servern, die MySQL Server hosten**, die Kapazität für Datenbanken bereitstellen und als „Hostserver“ bezeichnet werden.

In diesem Release wird keine MySQL-Instanz mehr erstellt. Sie müssen diese erstellen und/oder Zugriff auf externe SQL-Instanzen bereitstellen. Im [Azure Stack-Schnellstartkatalog](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) finden Sie eine Beispielvorlage mit folgenden Funktionen:
- Erstellen eines MySQL-Servers
- Herunterladen und Bereitstellen eines MySQL-Servers aus dem Marketplace

![NOTE] Hostserver, die in Azure Stack mit mehreren Knoten installiert werden, müssen mit einem Mandantenabonnement erstellt werden. Sie können nicht mit dem Standardabonnement des Anbieters erstellt werden. Das heißt, sie müssen über das Mandantenportal oder eine PowerShell-Sitzung mit einer entsprechenden Anmeldung erstellt werden. Alle Hostserver sind abrechenbare VMs, die entsprechende Lizenzen benötigen. Der Dienstadministrator kann der Besitzer dieses Abonnements sein.

### <a name="required-privileges"></a>Erforderliche Berechtigungen
Das Systemkonto muss über die folgenden Berechtigungen verfügen:

1.  Datenbank: erstellen, ablegen
2.  Anmeldung: erstellen, festlegen, ablegen, gewähren, widerrufen

## <a name="deploy-the-resource-provider"></a>Bereitstellen des Ressourcenanbieters

1. Wenn nicht bereits geschehen, registrieren Sie Ihr Development Kit und laden das Image „Windows Server 2016 Datacenter Core“ über die Marketplace-Verwaltung herunter. Sie müssen ein Windows Server 2016 Core-Image verwenden. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden – achten Sie aber darauf, die Option „Core“ auszuwählen. Die .NET 3.5-Laufzeit ist nicht mehr erforderlich.


2. Melden Sie sich auf einem Host an, der Zugriff auf die VM mit dem privilegierten Endpunkt hat.

    a. Melden Sie sich bei Installationen mit dem Azure Stack Development Kit auf dem physischen Host an.

    b. Auf Systemen mit mehreren Knoten muss der Host ein System sein, das auf den privilegierten Endpunkt zugreifen kann.

3. [Laden Sie die Binärdatei des MySQL-Ressourcenanbieters herunter](https://aka.ms/azurestackmysqlrp), und führen Sie Self-Extractor aus, um den Inhalt in einem temporären Verzeichnis zu extrahieren.

4.  Das Azure Stack-Stammzertifikat wird vom privilegierten Endpunkt abgerufen. Für ASDK wird im Rahmen dieses Prozesses ein selbstsigniertes Zertifikat erstellt. Bei mehreren Knoten müssen Sie ein entsprechendes Zertifikat bereitstellen.

    Wenn Sie ein eigenes Zertifikat bereitstellen müssen, benötigen Sie folgende Zertifikate:

    Ein Platzhalterzertifikat für \*.dbadapter.\<Region\>.\<externer FQDN\>. Diesem Zertifikat muss so vertraut werden, als würde es z.B. von einer Zertifizierungsstelle ausgestellt werden. Das bedeutet, die Vertrauenskette muss vorhanden sein, ohne dass Zwischenzertifikate erforderlich sind. Ein einzelnes Websitezertifikat kann mit dem expliziten Namen des virtuellen Computers [mysqladapter] verwendet werden, den Sie während der Installation verwenden.


5. Öffnen Sie eine **neue** PowerShell-Konsole mit erhöhten Rechten (Administrator), und wechseln Sie zu dem Verzeichnis, in dem Sie die Dateien extrahiert haben. Verwenden Sie ein neues Fenster, um Probleme aufgrund von falschen PowerShell-Modulen, die bereits in Ihrem System geladen sind, zu vermeiden.

6. [Installieren Sie Azure PowerShell, Version 1.2.11](azure-stack-powershell-install.md).

7. Führen Sie das Skript „DeploySqlProvider.ps1“ aus.

Mit diesem Skript werden folgende Schritte ausgeführt:

* Laden Sie die Binärdatei des MySQL-Connectors herunter (diese kann offline bereitgestellt werden).
* Laden Sie die Zertifikate und andere Artefakte in Ihr Azure Stack-Speicherkonto hoch.
* Veröffentlichen Sie Katalogpakete, damit Sie SQL-Datenbanken über den Katalog bereitstellen können.
* Veröffentlichen eines Katalogpakets für die Bereitstellung von Hostservern
* Stellen Sie einen virtuellen Computer mithilfe des in Schritt 1 erstellten Windows Server 2016-Images bereit, und installieren Sie den Ressourcenanbieter.
* Registrieren Sie einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
* Registrieren Sie Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz (Mandant und Administrator).


Ihre Möglichkeiten:
- Geben Sie mindestens die erforderlichen Parameter an der Befehlszeile an.
- Wenn Sie die Ausführung ohne Parameter vornehmen, werden Sie zur Eingabe aufgefordert.

Hier ist ein Beispiel, das Sie über die PowerShell-Eingabeaufforderung ausführen können (ändern Sie jedoch die Kontoinformationen und Kennwörter entsprechend):


```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack
$domain = 'AzureStack'
# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privleged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files
# Find the ERCS01 IP address first and make sure the certificate
# file is in the specified directory
.$tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint '10.10.10.10' `
  -DefaultSSLCertificatePassword $PfxPass -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1-Parameter

Sie können diese Parameter in der Befehlszeile angeben. Wenn sie nicht festgelegt werden oder eine Parameterüberprüfung fehlschlägt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | Beschreibung | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator sind für den Zugriff auf den privilegierten Endpunkt erforderlich. | _erforderlich_ |
| **AzCredential** | Geben Sie die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto an. Verwenden Sie die gleichen Anmeldeinformationen wie für das Bereitstellen von Azure Stack. | _erforderlich_ |
| **VMLocalCredential** | Definieren Sie die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem MySQL-Ressourcenanbieter. | _erforderlich_ |
| **PrivilegedEndpoint** | Geben Sie die IP-Adresse oder den DNS-Namen des privilegierten Endpunkts an. |  _erforderlich_ |
| **DependencyFilesLocalPath** | Pfad zu einer lokalen Freigabe, die [mysql-connector-net-6.9.9.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.9.9.msi) enthält. Wenn Sie eine Zertifikatsdatei bereitstellen, muss diese ebenfalls in diesem Verzeichnis abgelegt werden. | _optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Kennwort für das PFX-Zertifikat | _erforderlich_ |
| **MaxRetryCount** | Definieren Sie, wie oft Sie jeden Vorgang wiederholen möchten, wenn ein Fehler vorliegt.| 2 |
| **RetryDuration** | Definieren Sie das Timeout zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernen Sie den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein |
| **DebugMode** | Verhindert die automatische Bereinigung bei einem Fehler | Nein |
| **AcceptLicense** | Überspringt die Aufforderung zum Akzeptieren der GPL-Lizenz (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |


Je nach Systemleistung und Downloadgeschwindigkeiten kann die Installation zwischen 20 Minuten und mehreren Stunden dauern. Wenn das MySQLAdapter-Blatt nicht verfügbar ist, aktualisieren Sie das Administratorportal.

> [!NOTE]
> Wenn die Installation länger als 90 Minuten dauert, schlägt sie u.U. fehl, und auf dem Bildschirm sowie in der Protokolldatei wird eine Fehlermeldung angezeigt. Der Bereitstellungsvorgang wird ab dem fehlgeschlagenen Schritt wiederholt. Systeme, die den empfohlenen Arbeitsspeicher- und Core-Spezifikationen nicht entsprechen, können den MySQL-Ressourcenanbieter möglicherweise nicht bereitstellen.



## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals

> [!NOTE]
>  Nachdem das Installationsskript abgeschlossen ist, müssen Sie das Portal aktualisieren, damit das Blatt „Administrator“ angezeigt wird.


1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an.

2. Überprüfen Sie, ob die Bereitstellung erfolgreich war. Suchen Sie nach **Ressourcengruppen** &gt;, klicken Sie auf die Ressourcengruppe **system.\<location\>.mysqladapter**, und überprüfen Sie, ob alle vier Bereitstellungen erfolgreich waren.

      ![Überprüfen der Bereitstellung des MySQL-Ressourcenanbieters](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Bereitstellen von Kapazität durch Verbindung mit einem MySQL-Hostserver

1. Melden Sie sich als Dienstadministrator beim Azure Stack-Portal an.

2. Navigieren Sie zu **VERWALTUNGSRESSOURCEN** &gt; **MySQL-Hostserver** &gt; **+ Hinzufügen**.

    Auf dem Blatt **MySQL-Hostserver** können Sie den MySQL-Server-Ressourcenanbieter mit tatsächlichen MySQL-Serverinstanzen verbinden, die als Back-End des Ressourcenanbieters dienen.

    ![Hostserver](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Füllen Sie das Formular mit den Verbindungsdetails für Ihre MySQL Server-Instanz aus. Geben Sie den vollqualifizierten Domänennamen (FQDN) oder eine gültige IPv4-Adresse an, nicht den Kurznamen des virtuellen Computers. Diese Installation stellt keine standardmäßige MySQL-Instanz mehr bereit. Die angegebene Größe hilft dem Ressourcenanbieter bei der Verwaltung der Datenbankkapazität. Diese sollte in etwa der physischen Kapazität des Datenbankservers entsprechen.

    > [!NOTE]
    > Sofern die MySQL-Instanz für den Azure Resource Manager-Mandanten und -Administrator zugänglich ist, kann sie vom Ressourcenanbieter gesteuert werden. Die MySQL-Instanz __muss__ ausschließlich dem Ressourcenanbieter zugeordnet sein.

4. Wenn Sie Server hinzufügen, müssen Sie diese einer neuen oder vorhandenen SKU hinzufügen, um die Differenzierung von Dienstangeboten zu ermöglichen.
  Beispielsweise können Sie über eine Enterprise-Instanz Folgendes bereitstellen:
    - Datenbankkapazität
    - Automatische Sicherungen
    - Reservieren von Hochleistungsservern für einzelne Abteilungen
    - usw.
    Der SKU-Name sollte die Eigenschaften widerspiegeln, damit Mandanten ihre Datenbanken entsprechend platzieren können. Alle Hostserver in einer SKU sollten die gleichen Funktionen aufweisen.

    ![Erstellen einer MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst eine Datenbank erstellen, wenn die SKU erstellt wurde.


## <a name="to-test-your-deployment-create-your-first-mysql-database"></a>Erstellen Ihrer ersten MySQL-Datenbank zum Testen der Bereitstellung


1. Melden Sie sich als Dienstadministrator beim Azure Stack-Portal an.

2. Klicken Sie auf die Schaltfläche **+ Neu** &gt; **Daten und Speicher** &gt; **MySQL-Datenbank**.

3. Füllen Sie das Formular mit den Datenbankdetails aus.

    ![Erstellen einer MySQL-Testdatenbank](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Wählen Sie eine SKU aus.

    ![SKU auswählen](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Erstellen Sie eine Einstellung für die Anmeldung. Als Einstellung für die Anmeldung kann eine vorhandene wiederverwendet oder eine neue erstellt werden. Diese Einstellung enthält den Benutzernamen und das Kennwort für die Datenbank.

    ![Erstellen einer neuen Datenbankanmeldung](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    Die Verbindungszeichenfolge enthält den echten Namen des Datenbankservers. Kopieren Sie ihn aus dem Portal.

    ![Abrufen der Verbindungszeichenfolge für die MySQL-Datenbank](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> Benutzernamen dürfen bei MySQL 5.7 höchstens 32 Zeichen und bei früheren Editionen höchstens 16 Zeichen aufweisen.


## <a name="add-capacity"></a>Hinzufügen einer Kapazität

Fügen Sie Kapazität hinzu, indem Sie im Azure Stack-Portal weitere MySQL-Server hinzufügen. Zusätzliche Server können einer neuen oder vorhandenen SKU hinzugefügt werden. Stellen Sie sicher, dass die Servereigenschaften identisch sind.


## <a name="making-mysql-databases-available-to-tenants"></a>Verfügbarmachen von MySQL-Datenbanken für Mandanten
Erstellen Sie Pläne und Angebote, um MySQL-Datenbanken für Mandanten zur Verfügung stellen. Fügen Sie den Dienst „Microsoft.MySqlAdapter“ hinzu, fügen Sie ein Kontingent hinzu usw.

![Erstellen von Plänen und Angeboten, die Datenbanken umfassen](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="updating-the-administrative-password"></a>Aktualisieren des Administratorkennworts
Sie ändern das Kennwort, indem Sie es zunächst auf der MySQL Server-Instanz ändern. Navigieren Sie zu **VERWALTUNGSRESSOURCEN** &gt; **MySQL-Hostserver** &gt;, und klicken Sie auf den Hostserver. Klicken Sie im Bereich „Einstellungen“ auf „Kennwort“.

![Aktualisieren des Administratorkennworts](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="removing-the-mysql-adapter-resource-provider"></a>Entfernen des MySQL-Adapter-Ressourcenanbieters

Zum Entfernen des Ressourcenanbieters müssen unbedingt zuerst alle Abhängigkeiten entfernt werden.

1. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des Ressourcenanbieters heruntergeladen haben.

2. Alle Mandantendatenbanken müssen aus dem Ressourcenanbieter gelöscht werden (die Daten werden dabei nicht gelöscht). Diese Aktion muss von den Mandanten selbst ausgeführt werden.

3. Mandanten müssen die Namespace-Registrierung aufheben.

4. Der Administrator muss die Hostserver aus dem MySQL-Adapter löschen.

5. Der Administrator muss alle Pläne löschen, die auf den MySQL-Adapter verweisen.

6. Der Administrator muss alle Kontingente löschen, die dem MySQL-Adapter zugewiesen sind.

7. Führen Sie das Bereitstellungsskript mit dem Parameter „-Uninstall“, Azure Resource Manager-Endpunkten, DirectoryTenantID und Anmeldeinformationen für das Dienstadministratorkonto erneut aus.




## <a name="next-steps"></a>Nächste Schritte


Probieren Sie andere [PaaS-Dienste](azure-stack-tools-paas-services.md) wie den [SQL Server-Ressourcenanbieter](azure-stack-sql-resource-provider-deploy.md) und den [App Services-Ressourcenanbieter](azure-stack-app-service-overview.md) aus.
