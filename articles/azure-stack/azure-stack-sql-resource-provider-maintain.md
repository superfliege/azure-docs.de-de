---
title: Verwalten des SQL-Ressourcenanbieters in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den SQL-Ressourcenanbieterdienst in Azure Stack verwalten können.
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
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e7ddbe1235b3957a1e0cb7693ee728bfdbf9db6b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295658"
---
# <a name="maintenance-operations"></a>Wartungsvorgänge 
Der SQL-Ressourcenanbieter ist ein gesperrter virtueller Computer. Die Aktualisierung der Sicherheit einer Ressourcenanbieter-VM kann über den PowerShell-JEA-Endpunkt (Just Enough Administration) _DBAdapterMaintenance_ erfolgen. Das Ressourcenanbieter-Installationspaket enthält ein Skript für diese Vorgänge.

## <a name="patching-and-updating"></a>Patchen und Aktualisieren
Der SQL-Ressourcenanbieter wird nicht als Teil von Azure Stack gewartet, da es sich um eine Add-On-Komponente handelt. Microsoft stellt bei Bedarf Updates für den SQL-Ressourcenanbieter bereit. Der SQL-Ressourcenanbieter wird auf einem virtuellen _Benutzercomputer_ im Standardabonnement des Anbieters instanziiert. Aus diesem Grund ist es erforderlich, Windows-Patches, Antivirensignaturen usw. bereitzustellen. Die Windows-Updatepakete werden im Rahmen des Patch- und Updatezyklus bereitgestellt und können zum Anwenden eines Updates auf die Windows-VM verwendet werden. Bei der Veröffentlichung eines aktualisierten Adapters wird ein Skript zum Anwenden des Updates bereitgestellt. Dieses Skript erstellt eine neue Ressourcenanbieter-VM und migriert jeden beliebigen bereits verfügbaren Zustand.

 ## <a name="backuprestoredisaster-recovery"></a>Sicherung/Wiederherstellung/Notfallwiederherstellung
 Der SQL-Ressourcenanbieter wird nicht als Teil des Sicherungs-/Notfallwiederherstellungsprozesses von Azure Stack gesichert, da es sich um eine Add-On-Komponente handelt. Skripts werden für Folgendes bereitgestellt:
- Sichern der erforderlichen Zustandsinformationen (in einem Azure Stack-Speicherkonto gespeichert)
- Wiederherstellen des Ressourcenanbieters bei einer vollständigen Stack-Wiederherstellung.
Vor dem Ressourcenanbieter müssen die Datenbankserver (sofern erforderlich) wiederhergestellt werden.

## <a name="updating-sql-credentials"></a>Aktualisieren von SQL-Anmeldeinformationen
Sie sind für das Erstellen und Verwalten von Systemadministratorkonten auf Ihren SQL Server-Instanzen verantwortlich. Der Ressourcenanbieter benötigt ein Konto mit diesen Berechtigungen zum Verwalten von Datenbanken im Auftrag von Benutzern – er benötigt keinen Zugriff auf die Daten in diesen Datenbanken. Wenn Sie die Systemadministratorkennwörter auf Ihren SQL Server-Instanzen ändern müssen, können Sie die Updatefunktion der Administratorschnittstelle des Ressourcenanbieters verwenden, um das gespeicherte Kennwort, das vom Ressourcenanbieter verwendet wird, zu ändern. Diese Kennwörter werden in einem Schlüsseltresor in Ihrer Azure Stack-Instanz gespeichert.

Um die Einstellungen zu ändern, klicken Sie auf **Durchsuchen** &gt; **VERWALTUNGSRESSOURCEN** &gt; **SQL-Hostserver** &gt; **SQL-Anmeldungen**, und wählen Sie einen Anmeldenamen aus. Die Änderung muss zuerst auf der SQL-Instanz vorgenommen werden (und ggf. auf allen Replikaten). Klicken Sie im Bereich **Einstellungen** auf **Kennwort**.

![Aktualisieren des Administratorkennworts](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>Geheimnisrotation 
*Diese Anweisungen gelten nur für integrierte Azure Stack-Systeme ab Version 1804. Versuchen Sie nicht, die Geheimnisrotation mit älteren Azure Stack-Versionen (vor 1804) zu verwenden.*

Bei Verwendung der Ressourcenanbieter SQL und MySQL mit integrierten Azure Stack-Systemen können Sie folgende Infrastrukturgeheimnisse (Bereitstellungsgeheimnisse) rotieren:
- Das [während der Bereitstellung angegebene](azure-stack-pki-certs.md) externe SSL-Zertifikat
- Das während der Bereitstellung angegebene Kennwort des lokalen Administratorkontos für den virtuellen Computer des Ressourcenanbieters
- Das Kennwort des Diagnosebenutzers (dbadapterdiag) für den Ressourcenanbieter

### <a name="powershell-examples-for-rotating-secrets"></a>PowerShell-Beispiele für die Rotation von Geheimnissen

**Gleichzeitiges Ändern aller Geheimnisse**
```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  `
    -VMLocalCredential $localCreds
```

**Ausschließliches Ändern des Kennworts für den Diagnosebenutzer**
```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd 
```

**Ändern des Kennworts des lokalen Administratorkontos für den virtuellen Computer**
```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds
```

**Ändern des SSL-Zertifikats**
```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd 
```

### <a name="secretrotationsqlproviderps1-parameters"></a>Parameter für „SecretRotationSQLProvider.ps1“

|Parameter|BESCHREIBUNG|
|-----|-----|
|AzCredential|Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto|
|CloudAdminCredential|Anmeldeinformationen für das Domänenkonto des Azure Stack-Cloudadministrators|
|PrivilegedEndpoint|Privilegierter Endpunkt für den Zugriff auf „Get-AzureStackStampInformation“|
|DiagnosticsUserPassword|Kennwort des Diagnosebenutzers|
|VMLocalCredential|Das lokale Administratorkonto des virtuellen MySQLAdapter-Computers|
|DefaultSSLCertificatePassword|Kennwort für das SSL-Standardzertifikat (*.pfx)|
|DependencyFilesLocalPath|Lokaler Pfad der Abhängigkeitsdateien|
|     |     |

### <a name="known-issues"></a>Bekannte Probleme
**Problem:** Die Protokolle für die Geheimnisrotation werden nicht automatisch erfasst, wenn beim Ausführen des benutzerdefinierten Skripts für die Geheimnisrotation Fehler auftreten.

**Problemumgehung:** Verwenden Sie das Cmdlet Get-AzsDBAdapterLogs, um alle Ressourcenanbieterprotokolle, einschließlich „AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log“, unter „C:\Logs“ zu erfassen.

## <a name="update-the-virtual-machine-operating-system"></a>Aktualisieren des Betriebssystems für den virtuellen Computer
Es gibt mehrere Möglichkeiten, die Windows Server-VM zu aktualisieren:
- Installieren des aktuellen Ressourcenanbieterpakets über ein gepatchtes Windows Server 2016 Core-Image
- Installieren eines Windows Update-Pakets während der Installation oder des Updates des Ressourcenanbieters

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Aktualisieren der Windows Defender-Definitionen für den virtuellen Computer
Führen Sie diese Schritte aus, um die Defender-Definitionen zu aktualisieren:

1. Laden Sie das Update der Windows Defender-Definitionen von der Seite [Windows Defender Definition](https://www.microsoft.com/en-us/wdsi/definitions) herunter.

    Laden Sie auf dieser Seite unter „Manually download and install the definitions“ (Definitionen manuell herunterladen und installieren) die 64-Bit-Datei „Windows Defender Antivirus for Windows 10 and Windows 8.1“ herunter. 
    
    Direkter Link: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Erstellen Sie eine PowerShell-Sitzung mit dem VM-Wartungsendpunkt des SQL-Ressourcenanbieteradapters.
3. Kopieren Sie die Updatedatei für die Definitionen auf den DB-Adaptercomputer, indem Sie die Wartungsendpunkt-Sitzung verwenden.
4. Rufen Sie in der PowerShell-Wartungssitzung den Befehl _Update-DBAdapterWindowsDefenderDefinitions_ auf.
5. Es wird empfohlen, nach der Installation die verwendete Updatedatei für die Definitionen zu entfernen. Sie können sie in der Wartungssitzung mit dem Befehl _Remove-ItemOnUserDrive_ entfernen.


Hier ist ein Beispielskript zum Aktualisieren der Defender-Definitionen angegeben (ersetzen Sie die Adresse bzw. den Namen des virtuellen Computers durch den tatsächlichen Wert):

```powershell
# Set credentials for the RP VM local admin user
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Public IP Address of the DB adapter machine
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions. 
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' `
    -Outfile $localPathToDefenderUpdate 

# Create session to the maintenance endpoint
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance
# Copy defender update file to the db adapter machine
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\"
# Install the update file
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession 
```


## <a name="collect-diagnostic-logs"></a>Erfassen von Diagnoseprotokollen
Der SQL-Ressourcenanbieter ist ein gesperrter virtueller Computer. Wenn es notwendig ist, Protokolle vom virtuellen Computer zu erfassen, wird zu diesem Zweck der PowerShell-JEA-Endpunkt (Just Enough Administration) mit dem Namen _DBAdapterDiagnostics_ bereitgestellt. Über diesen Endpunkt sind zwei Befehle verfügbar:

- **Get-AzsDBAdapterLog**. Dient zum Vorbereiten eines ZIP-Pakets mit Ressourcenanbieter-Diagnoseprotokollen und zum Ablegen des Pakets auf dem Benutzerdatenträger der Sitzung. Der Befehl kann ohne Parameter aufgerufen werden und sammelt die Protokolle für die letzten vier Stunden.
- **Remove-AzsDBAdapterLog**. Dient zum Bereinigen vorhandener Protokollpakete auf dem virtuellen Computer des Ressourcenanbieters

Während der Bereitstellung bzw. des Updates des Ressourcenanbieters wird ein Benutzerkonto mit dem Namen **dbadapterdiag** erstellt, um für die Extraktion von Ressourcenanbieterprotokollen eine Verbindung mit dem Diagnoseendpunkt herzustellen. Das Kennwort dieses Kontos entspricht dem Kennwort, das während der Bereitstellung bzw. des Updates für das lokale Administratorkonto angegeben wird.

Zum Nutzen dieser Befehle müssen Sie eine PowerShell-Remotesitzung mit dem virtuellen Computer des Ressourcenanbieters herstellen und den Befehl aufrufen. Optional können Sie die Parameter „FromDate“ und „ToDate“ angeben. Wenn Sie nicht einen oder beide Parameter angeben, liegt „FromDate“ vier Stunden vor dem aktuellen Zeitpunkt, und „ToDate“ ist der aktuelle Zeitpunkt.

In diesem Beispielskript wird die Verwendung dieser Befehle veranschaulicht:

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
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

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen von SQL Server-Hostservern](azure-stack-sql-resource-provider-hosting-servers.md)
