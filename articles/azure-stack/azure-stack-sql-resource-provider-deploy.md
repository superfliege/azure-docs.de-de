---
title: Verwenden von SQL-Datenbanken in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in wenigen Schritten SQL-Datenbanken als Dienst in Azure Stack und den SQL Server-Ressourcenanbieteradapter bereitstellen können.
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: jeffgo
ms.openlocfilehash: d0b287eb61087e90c898aad5273ab5be8c1f98b2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Verwenden von SQL-Datenbanken in Microsoft Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Verwenden Sie den SQL Server-Ressourcenanbieteradapter, um SQL-Datenbanken als Dienst in [Azure Stack](azure-stack-poc.md) verfügbar zu machen. Nachdem Sie den Ressourcenanbieter installiert und mit mindestens einer SQL Server-Instanz verbunden haben, können Sie und Ihre Benutzer damit Folgendes erstellen:
- Datenbanken für native Cloud-Apps.
- Websites, die auf SQL basieren.
- Workloads, die auf SQL basieren.
Sie müssen nicht jedes Mal einen virtuellen Computer bereitstellen, der SQL Server hostet.

Der Ressourcenanbieter unterstützt nicht alle Datenbankverwaltungsfunktionen von [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/). Pools für elastische Datenbanken und die automatische Skalierung der Datenbankleistung nach oben oder unten sind beispielsweise nicht verfügbar. Der Ressourcenanbieter unterstützt jedoch ähnliche Vorgänge für das Erstellen, Lesen, Aktualisieren und Löschen. Die API ist nicht mit SQL-Datenbank kompatibel.

## <a name="sql-resource-provider-adapter-architecture"></a>Architektur des SQL-Ressourcenanbieteradapters
Der Ressourcenanbieter besteht aus drei Komponenten:

- **Dem virtuellen Computer des SQL-Ressourcenanbieteradapters**, einem virtuellen Windows-Computer, der die Anbieterdienste ausführt.
- **Dem Ressourcenanbieter selbst**, der die Bereitstellungsanforderungen verarbeitet und Datenbankressourcen verfügbar macht
- **Servern, die SQL Server hosten**, die Kapazität für Datenbanken bereitstellen und als „Hostserver“ bezeichnet werden.

Sie müssen mindestens eine SQL Server-Instanz erstellen bzw. den Zugriff auf externe SQL Server-Instanzen bereitstellen.

> [!NOTE]
> Hostserver, die auf integrierten Azure Stack-Systemen installiert werden, müssen mit einem Mandantenabonnement erstellt werden. Sie können nicht mit dem Standardabonnement des Anbieters erstellt werden. Sie müssen im Mandantenportal oder einer PowerShell-Sitzung mit einer geeigneten Anmeldung erstellt werden. Alle Hostserver sind abrechenbare VMs, die entsprechende Lizenzen benötigen. Der Dienstadministrator kann Besitzer des Mandantenabonnements sein.

## <a name="deploy-the-resource-provider"></a>Bereitstellen des Ressourcenanbieters

1. Wenn nicht bereits geschehen, registrieren Sie Ihr Development Kit und laden das Image „Windows Server 2016 Datacenter Core“ über die Marketplace-Verwaltung herunter. Sie müssen ein Windows Server 2016 Core-Image verwenden. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden. (Achten Sie darauf, dass die Option „Core“ ausgewählt ist.)

2. Melden Sie sich bei einem Host an, der Zugriff auf die VM mit dem privilegierten Endpunkt hat.

    - Melden Sie sich in Azure Stack Development Kit-Installationen beim physischen Host an.

    - In Systemen mit mehreren Knoten muss der Host ein System sein, das auf den privilegierten Endpunkt zugreifen kann.
    
    >[!NOTE]
    > Das System, in dem das Skript ausgeführt wird, *muss* ein Windows 10- oder Windows Server 2016-System mit der aktuellen Version der .NET-Laufzeit sein. Andernfalls tritt bei der Installation ein Fehler auf. Das Azure Stack SDK erfüllt dieses Kriterium.


3. Laden Sie die Binärdatei des SQL-Ressourcenanbieters herunter. Führen Sie das selbstextrahierende Programm aus, um die Inhalte in ein temporäres Verzeichnis zu extrahieren.

    >[!NOTE] 
    > Der Ressourcenanbieter verfügt über einen entsprechenden Mindestbuild für Azure Stack. Vergewissern Sie sich, dass Sie die richtige Binärdatei für die ausgeführte Azure Stack-Version herunterladen.

    | Azure Stack-Build | Installationsprogramm für SQL-Ressourcenanbieter |
    | --- | --- |
    | 1802: 1.0.180302.1 | [SQL RP Version 1.1.18.0](https://aka.ms/azurestacksqlrp1802) |
    | 1712: 1.0.180102.3, 1.0.180103.2 oder 1.0.180106.1 (mehrere Knoten) | [SQL RP Version 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1711: 1.0.171122.1 | [SQL RP Version 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1710: 1.0.171028.1 | [SQL RP Version 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. Für das Azure Stack SDK wird im Rahmen dieses Prozesses ein selbstsigniertes Zertifikat erstellt. Bei integrierten Systemen müssen Sie ein entsprechendes Zertifikat bereitstellen.

   Um Ihr eigenes Zertifikat bereitzustellen, platzieren Sie wie folgt eine PFX-Datei in **DependencyFilesLocalPath**:

    - Ein Platzhalterzertifikat für „\*.dbadapter.\<Region\>.\<externer FQDN\>“ oder ein einzelnes Zertifizierungsstellenzertifikat mit einem allgemeinen Namen wie „sqladapter.dbadapter.\<Region\>.\<externer FQDN\>“.

    - Dem Zertifikat muss vertraut werden. Das bedeutet, die Vertrauenskette muss vorhanden sein, ohne dass Zwischenzertifikate erforderlich sind.

    - In dem Verzeichnis, auf das mit dem Parameter „DependencyFilesLocalPath“ verwiesen wird, kann nur eine Zertifikatdatei vorhanden sein.

    - Der Dateiname darf keine Sonderzeichen enthalten.


5. Öffnen Sie eine **neue** PowerShell-Konsole mit erhöhten Rechten (Administrator), und wechseln Sie zu dem Verzeichnis, in dem Sie die Dateien extrahiert haben. Verwenden Sie ein neues Fenster, um Probleme aufgrund von falschen PowerShell-Modulen zu vermeiden, die bereits in Ihrem System geladen sind.

6. [Installieren Sie Azure PowerShell, Version 1.2.11](azure-stack-powershell-install.md).

7. Führen Sie das Skript „DeploySqlProvider.ps1“ aus, das folgende Aufgaben erledigt:

    - Lädt die Zertifikate und andere Artefakte in ein Azure Stack-Speicherkonto hoch.
    - Veröffentlicht Katalogpakete, damit Sie SQL-Datenbanken über den Katalog bereitstellen können.
    - Veröffentlicht ein Katalogpaket für die Bereitstellung von Hostservern.
    - Stellt einen virtuellen Computer mithilfe des in Schritt 1 erstellten Windows Server 2016-Images bereit, und installiert dann den Ressourcenanbieter.
    - Registriert einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
    - Registriert Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz (Benutzer und Administrator).
    - Installiert während der Installation des Ressourcenanbieters optional ein einzelnes Windows-Update.

8. Wir empfehlen Ihnen, das aktuelle Windows Server 2016 Core-Image aus der Marketplace-Verwaltung herunterzuladen. Wenn Sie ein Update installieren müssen, können Sie unter dem lokalen Abhängigkeitspfad ein einzelnes MSU-Paket anordnen. Falls mehr als eine MSU-Datei gefunden wird, schlägt die Ausführung des Skripts fehl.


Hier finden Sie ein Beispiel, das Sie an der PowerShell-Eingabeaufforderung ausführen können. (Achten Sie darauf, die Kontoinformationen und Kennwörter wie erforderlich zu ändern.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>Parameter „DeploySqlProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** | Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _Erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** | Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals

> [!NOTE]
>  Nachdem das Installationsskript vollständig ausgeführt wurde, müssen Sie das Portal aktualisieren, damit das Blatt „Administrator“ angezeigt wird.


1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an.

2. Überprüfen Sie, ob die Bereitstellung erfolgreich war. Wechseln Sie zu **Ressourcengruppen**. Wählen Sie dann die Ressourcengruppe **system.\<Speicherort\>.sqladapter** aus. Überprüfen Sie, ob alle vier Bereitstellungen erfolgreich ausgeführt wurden.

      ![Überprüfen der Bereitstellung des SQL-Ressourcenanbieters](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Aktualisieren des SQL-Ressourcenanbieteradapters (nur mehrere Knoten, Build 1710 und höher)
Unter Umständen wird ein neuer SQL-Ressourcenanbieteradapter veröffentlicht, wenn Azure Stack-Builds aktualisiert werden. Der vorhandene Adapter funktioniert zwar weiterhin, aber es ist ratsam, so schnell wie möglich das Update auf den aktuellen Build durchzuführen. Updates müssen in der richtigen Reihenfolge installiert werden. Es ist nicht möglich, Versionen zu überspringen (siehe Tabelle in Schritt 3 unter [Bereitstellen des Ressourcenanbieters](#deploy-the-resource-provider)).

Verwenden Sie zum Aktualisieren des Ressourcenanbieters das Skript *UpdateSQLProvider.ps1*. Der Prozess ähnelt dem Prozess zum Installieren eines Ressourcenanbieters, wie im Abschnitt [Bereitstellen des Ressourcenanbieters](#deploy-the-resource-provider) dieses Artikels beschrieben. Das Skript ist im Download des Ressourcenanbieters enthalten.

Mit dem Skript *UpdateSQLProvider.ps1* wird eine neue VM mit dem aktuellen Ressourcenanbietercode erstellt, und die Einstellungen werden von der alten VM zur neuen VM migriert. Zu den migrierten Einstellungen gehören Datenbank- und Hostserverinformationen und der erforderliche DNS-Eintrag.

Für das Skript ist die Verwendung der Argumente erforderlich, die für das Skript „DeploySqlProvider.ps1“ beschrieben wurden. Geben Sie hier auch das Zertifikat an. 

Wir empfehlen Ihnen, das aktuelle Windows Server 2016 Core-Image aus der Marketplace-Verwaltung herunterzuladen. Wenn Sie ein Update installieren müssen, können Sie unter dem lokalen Abhängigkeitspfad ein einzelnes MSU-Paket anordnen. Falls mehr als eine MSU-Datei gefunden wird, schlägt die Ausführung des Skripts fehl.

Hier ist ein Beispiel für das Skript *UpdateSQLProvider.ps1* angegeben, das Sie an der PowerShell-Eingabeaufforderung ausführen können. Achten Sie darauf, die Kontoinformationen und Kennwörter wie erforderlich zu ändern: 

> [!NOTE]
> Der Updateprozess gilt nur für integrierte Systeme.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>Parameter in „UpdateSQLProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** | Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** |Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |


## <a name="collect-diagnostic-logs"></a>Erfassen von Diagnoseprotokollen
Der SQL-Ressourcenanbieter ist ein gesperrter virtueller Computer. Wenn es notwendig ist, Protokolle vom virtuellen Computer zu erfassen, wird zu diesem Zweck der PowerShell-JEA-Endpunkt (Just Enough Administration) mit dem Namen _DBAdapterDiagnostics_ bereitgestellt. Über diesen Endpunkt sind zwei Befehle verfügbar:

* Get-AzsDBAdapterLog: Dient zum Vorbereiten eines ZIP-Pakets mit Ressourcenanbieter-Diagnoseprotokollen und zum Ablegen des Pakets auf dem Benutzerdatenträger der Sitzung. Der Befehl kann ohne Parameter aufgerufen werden und sammelt die Protokolle für die letzten vier Stunden.
* Remove-AzsDBAdapterLog: Dient zum Bereinigen vorhandener Protokollpakete auf der Ressourcenanbieter-VM.

Während der Bereitstellung bzw. des Updates des Ressourcenanbieters wird ein Benutzerkonto mit dem Namen _dbadapterdiag_ erstellt, um für die Extraktion von Ressourcenanbieterprotokollen eine Verbindung mit dem Diagnoseendpunkt herzustellen. Das Kennwort dieses Kontos entspricht dem Kennwort, das während der Bereitstellung bzw. des Updates für das lokale Administratorkonto angegeben wird.

Zum Nutzen dieser Befehle müssen Sie eine PowerShell-Remotesitzung mit dem virtuellen Computer des Ressourcenanbieters herstellen und den Befehl aufrufen. Optional können Sie die Parameter „FromDate“ und „ToDate“ angeben. Wenn Sie nicht einen oder beide Parameter angeben, liegt „FromDate“ vier Stunden vor dem aktuellen Zeitpunkt, und „ToDate“ ist der aktuelle Zeitpunkt.

In diesem Beispielskript wird die Verwendung dieser Befehle veranschaulicht:

```
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<see above>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, $diagnosticsUserPassword)
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
        -ConfigurationName DBAdapterDiagnostics

# Sample captures logs from the previous one hour
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup logs
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session
$session | Remove-PSSession
```

## <a name="maintenance-operations-integrated-systems"></a>Wartungsvorgänge (integrierte Systeme)
Der SQL-Ressourcenanbieter ist ein gesperrter virtueller Computer. Die Aktualisierung der Sicherheit einer Ressourcenanbieter-VM kann über den PowerShell-JEA-Endpunkt (Just Enough Administration) _DBAdapterMaintenance_ erfolgen.

Das Ressourcenanbieter-Installationspaket enthält ein Skript für diese Vorgänge.

### <a name="update-the-virtual-machine-operating-system"></a>Aktualisieren des Betriebssystems für den virtuellen Computer
Es gibt mehrere Möglichkeiten, die Windows Server-VM zu aktualisieren:
* Installieren des aktuellen Ressourcenanbieterpakets über ein gepatchtes Windows Server 2016 Core-Image
* Installieren eines Windows Update-Pakets während der Installation oder des Updates des Ressourcenanbieters


### <a name="update-the-virtual-machine-windows-defender-definitions"></a>Aktualisieren der Windows Defender-Definitionen für den virtuellen Computer

Führen Sie diese Schritte aus, um die Defender-Definitionen zu aktualisieren:

1. Laden Sie das Update der Windows Defender-Definitionen von der Seite [Windows Defender Definition](https://www.microsoft.com/en-us/wdsi/definitions) (Windows Defender-Definition) herunter.

    Laden Sie auf dieser Seite unter „Manually download and install the definitions“ (Definitionen manuell herunterladen und installieren) die 64-Bit-Datei „Windows Defender Antivirus for Windows 10 and Windows 8.1“ herunter. 
    
    Direkte Verknüpfung: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Erstellen Sie eine PowerShell-Sitzung mit dem VM-Wartungsendpunkt des SQL-Ressourcenanbieteradapters.
3. Kopieren Sie die Updatedatei für die Definitionen auf den DB-Adaptercomputer, indem Sie die Wartungsendpunkt-Sitzung verwenden.
4. Rufen Sie in der PowerShell-Wartungssitzung den Befehl _Update-DBAdapterWindowsDefenderDefinitions_ auf.
5. Es wird empfohlen, nach der Installation die verwendete Updatedatei für die Definitionen zu entfernen. Sie können sie in der Wartungssitzung mit dem Befehl _Remove-ItemOnUserDrive_ entfernen.


Hier ist ein Beispielskript zum Aktualisieren der Defender-Definitionen angegeben (ersetzen Sie die Adresse bzw. den Namen des virtuellen Computers durch den tatsächlichen Wert):

```
# Set credentials for the diagnostic user
$diagPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$diagCreds = New-Object System.Management.Automation.PSCredential `
    ("dbadapterdiag", $vmLocalAdminPass)$diagCreds = Get-Credential

# Public IP Address of the DB adapter machine
$databaseRPMachine  = "XX.XX.XX.XX"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"
 
# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions. 
Invoke-WebRequest -Uri https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64 `
    -Outfile $localPathToDefenderUpdate 

# Create session to the maintenance endpoint
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $diagCreds -ConfigurationName DBAdapterMaintenance
# Copy defender update file to the db adapter machine
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\mpam-fe.exe"
# Install the update file
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinitions -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession
```

## <a name="remove-the-sql-resource-provider-adapter"></a>Entfernen des SQL-Ressourcenanbieteradapters

Zum Entfernen des Ressourcenanbieters müssen unbedingt zuerst alle Abhängigkeiten entfernt werden.

1. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des SQL-Ressourcenanbieteradapters heruntergeladen haben.

2. Alle Benutzerdatenbanken müssen aus dem Ressourcenanbieter gelöscht werden. (Die Daten werden dabei nicht gelöscht.) Diese Aufgabe sollte von den Benutzern selbst ausgeführt werden.

3. Der Administrator muss die Hostserver aus dem SQL-Ressourcenanbieteradapter löschen.

4. Der Administrator muss alle Pläne löschen, die auf den SQL-Ressourcenanbieteradapter verweisen.

5. Der Administrator muss alle SKUs und Kontingente löschen, die dem SQL-Ressourcenanbieteradapter zugewiesen sind.

6. Führen Sie das Bereitstellungsskript mit den folgenden Elementen erneut aus:
    - Parameter „-Uninstall“
    - Azure Resource Manager-Endpunkte
    - DirectoryTenantID
    - Anmeldeinformationen für das Dienstadministratorkonto


## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Hostservern](azure-stack-sql-resource-provider-hosting-servers.md) und [Erstellen von Datenbanken](azure-stack-sql-resource-provider-databases.md).

Probieren Sie andere [PaaS-Dienste](azure-stack-tools-paas-services.md) wie den [MySQL Server-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md) und den [App Services-Ressourcenanbieter](azure-stack-app-service-overview.md) aus.
