---
title: Verwenden von MySQL-Datenbanken als PaaS in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den MySQL-Ressourcenanbieter bereitstellen und MySQL-Datenbanken als Dienst in Azure Stack einrichten.
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: mabrigg
ms.openlocfilehash: 3273f435cb65411c85e3a22369682d51e7a12baf
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Verwenden von MySQL-Datenbanken in Microsoft Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können einen MySQL-Ressourcenanbieter in Azure Stack bereitstellen. Nach der Bereitstellung des Ressourcenanbieters können Sie mithilfe von Azure Resource Manager-Bereitstellungsvorlagen MySQL-Server und -Datenbanken erstellen. Sie können auch MySQL-Datenbanken als Dienst bereitstellen. 

MySQL-Datenbanken, die häufig auf Websites verwendet werden, unterstützen zahlreiche Website-Plattformen. Beispielsweise können Sie nach der Bereitstellung des Ressourcenanbieters WordPress-Websites über die Web-Apps-Plattform als Dienst-Add-On (PaaS) für Azure Stack erstellen.

Zum Bereitstellen des MySQL-Anbieters in einem System ohne Internetzugriff kopieren Sie die Datei [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) in eine lokale Freigabe. Geben Sie anschließend den Namen dieser Freigabe an, wenn Sie dazu aufgefordert werden. Sie müssen die PowerShell-Module für Azure und Azure Stack installieren.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>Architektur des MySQL Server-Ressourcenanbieteradapters

Der Ressourcenanbieter besteht aus drei Komponenten:

- **Dem virtuellen Computer des MySQL-Ressourcenanbieteradapters**, einem virtuellen Windows-Computer, der die Anbieterdienste ausführt.

- **Dem Ressourcenanbieter selbst**, der die Bereitstellungsanforderungen verarbeitet und Datenbankressourcen verfügbar macht

- **Servern, die MySQL Server hosten**, die Kapazität für Datenbanken bereitstellen und als „Hostserver“ bezeichnet werden.

In diesem Release werden keine MySQL-Instanzen mehr erstellt. Das bedeutet, dass Sie die Instanzen selbst erstellen und/oder Zugriff auf externe SQL-Instanzen bereitstellen müssen. Im [Azure Stack-Schnellstartkatalog](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) finden Sie eine Beispielvorlage mit folgenden Funktionen:
- Erstellen eines MySQL-Servers
- Herunterladen und Bereitstellen eines MySQL-Servers aus dem Azure Marketplace

> [!NOTE]
> Hostserver, die in einer Azure Stack-Implementierung mit mehreren Knoten installiert werden, müssen mit einem Mandantenabonnement erstellt werden. Sie können nicht mit dem Standardabonnement des Anbieters erstellt werden. Sie müssen im Mandantenportal oder einer PowerShell-Sitzung mit einer geeigneten Anmeldung erstellt werden. Alle Hostserver sind abrechenbare VMs, die entsprechende Lizenzen benötigen. Der Dienstadministrator kann Besitzer des Mandantenabonnements sein.

### <a name="required-privileges"></a>Erforderliche Berechtigungen
Das Systemkonto muss über die folgenden Berechtigungen verfügen:

1.  Datenbank: erstellen, löschen
2.  Anmeldung: erstellen, festlegen, löschen, gewähren, widerrufen

## <a name="deploy-the-resource-provider"></a>Bereitstellen des Ressourcenanbieters

1. Wenn nicht bereits geschehen, registrieren Sie Ihr Development Kit und laden das Image „Windows Server 2016 Datacenter Core“ über die Marketplace-Verwaltung herunter. Sie müssen ein Windows Server 2016 Core-Image verwenden. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden. (Achten Sie darauf, dass die Option „Core“ ausgewählt ist.) Die .NET 3.5-Laufzeit ist nicht mehr erforderlich.


2. Melden Sie sich bei einem Host an, der Zugriff auf die VM mit dem privilegierten Endpunkt hat.

    - Melden Sie sich bei Azure SDK-Installationen beim physischen Host an. 
    - In Systemen mit mehreren Knoten muss der Host ein System sein, das auf den privilegierten Endpunkt zugreifen kann.
    
    >[!NOTE]
    > Das System, in dem das Skript ausgeführt wird, *muss* ein Windows 10- oder Windows Server 2016-System mit der neuesten Version der .NET-Laufzeit sein. Andernfalls tritt bei der Installation ein Fehler auf. Der Azure SDK-Host erfüllt diese Kriterien.
    

3. Laden Sie die Binärdatei des MySQL-Ressourcenanbieters herunter. Führen Sie das selbstextrahierende Programm aus, um die Inhalte in ein temporäres Verzeichnis zu extrahieren.

    >[!NOTE] 
    > Der Build des Ressourcenanbieters entspricht den Azure Stack-Builds. Vergewissern Sie sich, dass Sie die richtige Binärdatei für die ausgeführte Azure Stack-Version herunterladen.

    | Azure Stack-Build | MySQL RP-Installationsprogramm |
    | --- | --- |
    | 1.0.180102.3 oder 1.0.180106.1 (mehrere Knoten) | [MySQL RP Version 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |
    | 1.0.171122.1 | [MySQL RP Version 1.1.12.0](https://aka.ms/azurestackmysqlrp1711) |
    | 1.0.171028.1 | [MySQL RP Version 1.1.8.0](https://aka.ms/azurestackmysqlrp1710) |

4.  Das Azure Stack-Stammzertifikat wird vom privilegierten Endpunkt abgerufen. Für das Azure SDK wird im Rahmen dieses Prozesses ein selbstsigniertes Zertifikat erstellt. Bei mehreren Knoten müssen Sie ein entsprechendes Zertifikat bereitstellen.

    Wenn Sie Ihr eigenes Zertifikat bereitstellen müssen, platzieren Sie eine PFX-Datei in **DependencyFilesLocalPath**, die folgende Kriterien erfüllt:

    - Ein Platzhalterzertifikat für „\*.dbadapter.\<Region\>.\<externer FQDN\>“ oder ein einzelnes Zertifizierungsstellenzertifikat mit einem allgemeinen Namen wie „mysqladapter.dbadapter.\<Region\>.\<externer FQDN\>“.

    - Dem Zertifikat muss vertraut werden. Das bedeutet, die Vertrauenskette muss vorhanden sein, ohne dass Zwischenzertifikate erforderlich sind.

    - Unter „DependencyFilesLocalPath“ ist nur eine Zertifikatdatei vorhanden.
    
    - Der Dateiname darf keine Sonderzeichen oder Leerzeichen enthalten.


5. Öffnen Sie eine **neue** PowerShell-Administratorkonsole mit erhöhten Rechten. Wechseln Sie dann zu dem Verzeichnis, in das Sie die Dateien extrahiert haben. Verwenden Sie ein neues Fenster, um Probleme aufgrund von falschen PowerShell-Modulen zu vermeiden, die bereits in Ihrem System geladen sind.

6. [Installieren Sie Azure PowerShell, Version 1.2.11](azure-stack-powershell-install.md).

7. Führen Sie das Skript `DeployMySqlProvider.ps1` aus.

Mit diesem Skript werden folgende Schritte ausgeführt:

* Lädt die Binärdatei des MySQL-Connectors herunter (diese kann offline bereitgestellt werden).
* Lädt die Zertifikate und andere Artefakte in ein Azure Stack-Speicherkonto hoch.
* Veröffentlicht Katalogpakete, damit Sie SQL-Datenbanken über den Katalog bereitstellen können.
* Veröffentlicht ein Katalogpaket für die Bereitstellung von Hostservern.
* Stellt einen virtuellen Computer mithilfe des in Schritt 1 erstellten Windows Server 2016-Images bereit. Installiert auch den Ressourcenanbieter.
* Registriert einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
* Registriert Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz (Mandant und Administrator).


Ihre Möglichkeiten:
- Geben Sie in der Befehlszeile die erforderlichen Parameter an.
- Starten Sie die Ausführung ohne Parameter, und geben Sie die Parameter ein, wenn Sie dazu aufgefordert werden.

Hier finden Sie ein Beispiel, das Sie an der PowerShell-Eingabeaufforderung ausführen können. Achten Sie darauf, die Kontoinformationen und Kennwörter wie erforderlich zu ändern:


```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure SDK, the default is AzureStack, and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files.
# Find the ERCS01 IP address first, and make sure the certificate
# file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```


### <a name="deploysqlproviderps1-parameters"></a>Parameter „DeploySqlProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** | Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem MySQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **DependencyFilesLocalPath** | Pfad zu einer lokalen Freigabe, die [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi) enthält. Wenn Sie einen dieser Pfade angeben, muss die Zertifikatdatei ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _Erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** | Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |
| **AcceptLicense** | Überspringt die Aufforderung zum Akzeptieren der GPL-Lizenz.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |



Je nach Systemleistung und Downloadgeschwindigkeiten kann die Installation zwischen 20 Minuten und mehreren Stunden dauern. Wenn das **MySQLAdapter**-Blatt nicht verfügbar ist, aktualisieren Sie das Administratorportal.

> [!NOTE]
> Wenn die Installation mehr als 90 Minuten dauert, tritt möglicherweise ein Fehler auf. In diesem Fall wird auf dem Bildschirm und in der Protokolldatei eine Fehlermeldung angezeigt. Der Bereitstellungsvorgang wird ab dem fehlgeschlagenen Schritt wiederholt. Systeme, die den empfohlenen Arbeitsspeicher- und Kernspezifikationen nicht entsprechen, können den MySQL-Ressourcenanbieter möglicherweise nicht bereitstellen.



## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals

> [!NOTE]
>  Nachdem das Installationsskript vollständig ausgeführt wurde, müssen Sie das Portal aktualisieren, damit das Blatt „Administrator“ angezeigt wird.


1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an.

2. Überprüfen Sie, ob die Bereitstellung erfolgreich war. Wechseln Sie zu **Ressourcengruppen**, und wählen Sie die Ressourcengruppe **system.\<Speicherort\>.mysqladapter** aus. Überprüfen Sie, ob alle vier Bereitstellungen erfolgreich ausgeführt wurden.

      ![Überprüfen der Bereitstellung des MySQL-Ressourcenanbieters](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Bereitstellen von Kapazität durch Verbindung mit einem MySQL-Hostserver

1. Melden Sie sich als Dienstadministrator beim Azure Stack-Portal an.

2. Wählen Sie **VERWALTUNGSRESSOURCEN** > **MySQL-Hostserver** > **+ Hinzufügen** aus.

    Auf dem Blatt **MySQL-Hostserver** können Sie den MySQL-Server-Ressourcenanbieter mit tatsächlichen MySQL-Serverinstanzen verbinden, die als Back-End des Ressourcenanbieters dienen.

    ![Hostserver](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Geben Sie die Verbindungsdetails für Ihre MySQL Server-Instanz an. Geben Sie den vollqualifizierten Domänennamen (FQDN) oder eine gültige IPv4-Adresse an, nicht den Kurznamen des virtuellen Computers. Diese Installation stellt keine standardmäßige MySQL-Instanz mehr bereit. Die angegebene Größe hilft dem Ressourcenanbieter bei der Verwaltung der Datenbankkapazität. Diese sollte in etwa der physischen Kapazität des Datenbankservers entsprechen.

    > [!NOTE]
    >Wenn die MySQL-Instanz für den Azure Resource Manager-Mandanten und -Administrator zugänglich ist, kann sie vom Ressourcenanbieter gesteuert werden. Die MySQL-Instanz *muss* ausschließlich dem Ressourcenanbieter zugeordnet sein.

4. Wenn Sie Server hinzufügen, müssen Sie diese einer neuen oder vorhandenen SKU hinzufügen, um die Differenzierung von Dienstangeboten zu ermöglichen.
  Beispielsweise können Sie über eine Enterprise-Instanz Folgendes bereitstellen:
    - Datenbankkapazität
    - Automatische Sicherung
    - Reservieren von Hochleistungsservern für einzelne Abteilungen
 

Der SKU-Name sollte die Eigenschaften widerspiegeln, damit Mandanten ihre Datenbanken entsprechend platzieren können. Alle Hostserver in einer SKU sollten die gleichen Funktionen aufweisen.

![Erstellen einer MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
> Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst eine Datenbank erstellen, wenn die SKU erstellt wurde.


## <a name="test-your-deployment-by-creating-your-first-mysql-database"></a>Testen der Bereitstellung durch Erstellen der ersten MySQL-Datenbank


1. Melden Sie sich als Dienstadministrator beim Azure Stack-Portal an.

2. Wählen Sie **+ Neu** > **Daten + Speicher** > **MySQL-Datenbank** aus.

3. Geben Sie die Datenbankdetails an.

    ![Erstellen einer MySQL-Testdatenbank](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Wählen Sie eine SKU aus.

    ![SKU auswählen](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Erstellen Sie eine Einstellung für die Anmeldung. Sie können eine vorhandene Anmeldeeinstellung verwenden oder eine neue erstellen. Diese Einstellung enthält den Benutzernamen und das Kennwort für die Datenbank.

    ![Erstellen einer neuen Datenbankanmeldung](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    Die Verbindungszeichenfolge enthält den echten Namen des Datenbankservers. Kopieren Sie ihn aus dem Portal.

    ![Abrufen der Verbindungszeichenfolge für die MySQL-Datenbank](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> Benutzernamen dürfen in MySQL 5.7 nicht länger als 32 Zeichen sein. In früheren Versionen durften sie 16 Zeichen nicht überschreiten.


## <a name="add-capacity"></a>Hinzufügen einer Kapazität

Fügen Sie Kapazität hinzu, indem Sie im Azure Stack-Portal weitere MySQL-Server hinzufügen. Zusätzliche Server können einer neuen oder vorhandenen SKU hinzugefügt werden. Stellen Sie sicher, dass die Servereigenschaften identisch sind.


## <a name="make-mysql-databases-available-to-tenants"></a>Verfügbarmachen von MySQL-Datenbanken für Mandanten
Erstellen Sie Pläne und Angebote, um MySQL-Datenbanken für Mandanten zur Verfügung stellen. Fügen Sie z.B. den Dienst „Microsoft.MySqlAdapter“ hinzu, fügen Sie ein Kontingent hinzu usw.

![Erstellen von Plänen und Angeboten, die Datenbanken umfassen](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="update-the-administrative-password"></a>Aktualisieren des Administratorkennworts
Sie ändern das Kennwort, indem Sie es zunächst auf der MySQL Server-Instanz ändern. Wählen Sie **VERWALTUNGSRESSOURCEN** > **MySQL-Hostserver** aus. Wählen Sie dann den Hostserver aus. Klicken Sie im Bereich **Einstellungen** auf **Kennwort**.

![Aktualisieren des Administratorkennworts](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="update-the-mysql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Aktualisieren des MySQL-Ressourcenanbieteradapters (nur mehrere Knoten, Build 1710 und höher)
Bei jeder Aktualisierung des Azure Stack-Builds wird ein neuer MySQL-Ressourcenanbieteradapter veröffentlicht. Der vorhandene Adapter funktioniert möglicherweise weiterhin. Es wird jedoch empfohlen, nach der Aktualisierung des Azure Stack-Builds den Adapter so schnell wie möglich ebenfalls zu aktualisieren. 

Der Updateprozess ähnelt dem oben beschriebenen Installationsvorgang. Sie erstellen einen neuen virtuellen Computer mit dem neuesten Ressourcenanbietercode. Dann migrieren Sie Einstellungen auf diese neue Instanz, einschließlich der Informationen zu Datenbank und Hostserver. Sie migrieren auch den erforderlichen DNS-Eintrag.

Verwenden Sie das Skript „UpdateMySQLProvider.ps1“ mit den gleichen Argumenten wie oben beschrieben. Geben Sie hier auch das Zertifikat an.

> [!NOTE]
> Das Update wird nur auf Systemen mit mehreren Knoten unterstützt.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense
 ```

### <a name="updatemysqlproviderps1-parameters"></a>Parameter für „UpdateMySQLProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** |Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Kennwort für das PFX-Zertifikat | _Erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** | Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |
| **AcceptLicense** | Überspringt die Aufforderung zum Akzeptieren der GPL-Lizenz.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |

## <a name="remove-the-mysql-resource-provider-adapter"></a>Entfernen des MySQL-Ressourcenanbieteradapters

Zum Entfernen des Ressourcenanbieters müssen unbedingt zuerst alle Abhängigkeiten entfernt werden.

1. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des Ressourcenanbieters heruntergeladen haben.

2. Alle Mandantendatenbanken müssen aus dem Ressourcenanbieter gelöscht werden. (Die Daten werden dabei nicht gelöscht.) Diese Aufgabe sollte von den Mandanten selbst ausgeführt werden.

3. Mandanten müssen die Namespace-Registrierung aufheben.

4. Der Administrator muss die Hostserver aus dem MySQL-Adapter löschen.

5. Der Administrator muss alle Pläne löschen, die auf den MySQL-Adapter verweisen.

6. Der Administrator muss alle Kontingente löschen, die dem MySQL-Adapter zugewiesen sind.

7. Führen Sie das Bereitstellungsskript mit den folgenden Elementen erneut aus:
    - Parameter „-Uninstall“
    - Azure Resource Manager-Endpunkte
    - DirectoryTenantID
    - Anmeldeinformationen für das Dienstadministratorkonto
