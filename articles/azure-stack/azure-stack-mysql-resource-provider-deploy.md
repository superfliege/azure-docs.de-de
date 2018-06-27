---
title: Verwenden von MySQL-Datenbank-Instanzen in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in wenigen Schritten MySQL-Datenbank-Instanzen als Dienst in Azure Stack und den MySQL Server-Ressourcenanbieteradapter bereitstellen können.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 061d9431890622dafafa8c180027b8df735c100e
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296012"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Verwenden von MySQL-Datenbanken in Microsoft Azure Stack
Verwenden Sie den MySQL Server-Ressourcenanbieter von Azure Stack, um MySQL-Datenbanken als Dienst in Azure Stack verfügbar zu machen. Der MySQL-Ressourcenanbieterdienst wird auf der VM des MySQL-Ressourcenanbieters ausgeführt, was ein virtueller Computer unter Windows Server Core ist.

## <a name="deploy-the-resource-provider"></a>Bereitstellen des Ressourcenanbieters 
1. Wenn nicht bereits geschehen, registrieren Sie Ihr Development Kit und laden das Image „Windows Server 2016 Datacenter Core“ über die Marketplace-Verwaltung herunter. Sie müssen ein Windows Server 2016 Core-Image verwenden. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden. (Achten Sie darauf, dass die Option „Core“ ausgewählt ist.) 

2. Melden Sie sich bei einem Host an, der Zugriff auf die VM mit dem privilegierten Endpunkt hat:
    - Melden Sie sich bei Azure SDK-Installationen beim physischen Host an.  
    - In integrierten Systemen muss der Host ein System sein, das auf den privilegierten Endpunkt zugreifen kann. 
    
    >[!NOTE] 
    > Das System, in dem das Skript ausgeführt wird, *muss* ein Windows 10- oder Windows Server 2016-System mit der neuesten Version der .NET-Laufzeit sein. Andernfalls tritt bei der Installation ein Fehler auf. Das Azure Stack SDK erfüllt dieses Kriterium. 
    
3. Laden Sie die Binärdatei des MySQL-Ressourcenanbieters herunter. Führen Sie das selbstextrahierende Programm aus, um die Inhalte in ein temporäres Verzeichnis zu extrahieren. 
    >[!NOTE]  
    > Der Ressourcenanbieter verfügt über einen entsprechenden Mindestbuild für Azure Stack. Vergewissern Sie sich, dass Sie die richtige Binärdatei für die ausgeführte Azure Stack-Version herunterladen.

    | Azure Stack-Version | MySQL RP Version| 
    | --- | --- | 
    | Version 1804 (1.0.180513.1)|[MySQL RP Version 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) | 
    | Version 1802 (1.0.180302.1) | [MySQL RP Version 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) | 
    | Version 1712 (1.0.180102.3 oder 1.0.180106.1 (integrierte Systeme)) | [MySQL RP Version 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) | 

4.  Für das ASDK wird im Rahmen dieses Prozesses ein selbstsigniertes Zertifikat erstellt. Bei integrierten Systemen müssen Sie ein entsprechendes Zertifikat bereitstellen. Wenn Sie Ihr eigenes Zertifikat bereitstellen müssen, platzieren Sie eine PFX-Datei in **DependencyFilesLocalPath**, die folgende Kriterien erfüllt: 
    - Ein Platzhalterzertifikat für „\*.dbadapter.\<Region\>.\<externer FQDN\>“ oder ein einzelnes Zertifizierungsstellenzertifikat mit einem allgemeinen Namen wie „mysqladapter.dbadapter.\<Region\>.\<externer FQDN\>“. 
    - Dem Zertifikat muss vertraut werden. Das bedeutet, die Vertrauenskette muss vorhanden sein, ohne dass Zwischenzertifikate erforderlich sind. 
    - Unter „DependencyFilesLocalPath“ ist nur eine Zertifikatdatei vorhanden. 
    - Der Dateiname darf keine Sonderzeichen oder Leerzeichen enthalten. 

5. Öffnen Sie eine **neue** PowerShell-Administratorkonsole mit erhöhten Rechten. Wechseln Sie dann zu dem Verzeichnis, in das Sie die Dateien extrahiert haben. Verwenden Sie ein neues Fenster, um Probleme aufgrund von falschen PowerShell-Modulen zu vermeiden, die bereits in Ihrem System geladen sind. 

6. Führen Sie das Skript **DeployMySqlProvider.ps1** aus. Mit diesem Skript werden folgende Schritte ausgeführt: 
    - Lädt die Binärdatei des MySQL-Connectors herunter (diese kann offline bereitgestellt werden). 
    - Lädt die Zertifikate und andere Artefakte in ein Azure Stack-Speicherkonto hoch. 
    - Veröffentlicht Katalogpakete, damit Sie SQL-Datenbanken über den Katalog bereitstellen können. 
    - Veröffentlicht ein Katalogpaket für die Bereitstellung von Hostservern. 
    - Stellt einen virtuellen Computer mit einem Windows Server 2016 Azure Stack Marketplace-Image bereit, und installiert den Ressourcenanbieter. 
    - Registriert einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird. 
    - Registriert Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz (Mandant und Administrator). 

    Sie können die erforderlichen Parameter in der Befehlszeile angeben oder das Skript ohne Parameter ausführen und diese dann bei Aufforderung eingeben. 

    Hier finden Sie ein Beispiel, das Sie an der PowerShell-Eingabeaufforderung ausführen können. Achten Sie darauf, die Kontoinformationen und Kennwörter wie erforderlich zu ändern: 

    ```powershell 
    # Install the AzureRM.Bootstrapper module and set the profile. 
    Install-Module -Name AzureRm.BootStrapper -Force 
    Use-AzureRmProfile -Profile 2017-03-09-profile 

    # Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time. 
    $domain = "AzureStack"  

    # For integrated systems, use the IP address of one of the ERCS virtual machines 
    $privilegedEndpoint = "AzS-ERCS01" 

    # Point to the directory where the resource provider installation files were extracted. 
    $tempDir = 'C:\TEMP\MYSQLRP' 

    # The service admin account (can be Azure Active Directory or Active Directory Federation Services). 
    $serviceAdmin = "admin@mydomain.onmicrosoft.com" 
    $AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
    $AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass) 

    # Set the credentials for the new resource provider VM local administrator account 
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
    $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds ` 
        -VMLocalCredential $vmLocalAdminCreds ` 
        -CloudAdminCredential $cloudAdminCreds ` 
        -PrivilegedEndpoint $privilegedEndpoint ` 
        -DefaultSSLCertificatePassword $PfxPass ` 
        -DependencyFilesLocalPath $tempDir\cert ` 
        -AcceptLicense 
    ``` 

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1-Parameter 
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

> [!NOTE] 
> Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst eine Datenbank erstellen, wenn die SKU erstellt wurde. 

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals 
> [!NOTE] 
>  Nachdem das Installationsskript vollständig ausgeführt wurde, müssen Sie das Portal aktualisieren, damit das Blatt „Administrator“ angezeigt wird. 

1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an. 
2. Überprüfen Sie, ob die Bereitstellung erfolgreich war. Wechseln Sie zu **Ressourcengruppen**, und wählen Sie die Ressourcengruppe **system.\<Speicherort\>.mysqladapter** aus. Überprüfen Sie, ob alle vier Bereitstellungen erfolgreich ausgeführt wurden:

      ![Überprüfen der Bereitstellung des MySQL-Ressourcenanbieters](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png) 


## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen von Hostservern](azure-stack-mysql-resource-provider-hosting-servers.md)
