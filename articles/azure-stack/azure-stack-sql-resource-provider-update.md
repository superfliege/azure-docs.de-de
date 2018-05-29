---
title: Verwenden von SQL-Datenbanken in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in wenigen Schritten SQL-Datenbanken als Dienst in Azure Stack und den SQL Server-Ressourcenanbieteradapter bereitstellen können.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 9154f509f9019c28515970869678aa6633d16163
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33206174"
---
# <a name="update-the-sql-resource-provider-adapter"></a>Aktualisieren des SQL-Ressourcenanbieteradapters
Unter Umständen wird ein neuer SQL-Ressourcenanbieteradapter veröffentlicht, wenn Azure Stack-Builds aktualisiert werden. Der vorhandene Adapter funktioniert zwar weiterhin, aber es ist ratsam, so schnell wie möglich das Update auf den aktuellen Build durchzuführen. Updates müssen in der richtigen Reihenfolge installiert werden. Es ist nicht möglich, Versionen zu überspringen (siehe Liste der Versionen in [Verwenden von SQL-Datenbanken in Microsoft Azure Stack](.\azure-stack-sql-resource-provider-deploy.md#prerequisites)).

Verwenden Sie zum Aktualisieren des Ressourcenanbieters das Skript *UpdateSQLProvider.ps1*. Der Prozess ähnelt dem Prozess zum Installieren eines Ressourcenanbieters, wie im Artikel [Bereitstellen des Ressourcenanbieters](.\azure-stack-sql-resource-provider-deploy.md) beschrieben. Das Skript ist im Download des Ressourcenanbieters enthalten.

Mit dem Skript *UpdateSQLProvider.ps1* wird eine neue VM mit dem aktuellen Ressourcenanbietercode erstellt, und die Einstellungen werden von der alten VM zur neuen VM migriert. Zu den migrierten Einstellungen gehören Datenbank- und Hostserverinformationen und der erforderliche DNS-Eintrag.

Für das Skript ist die Verwendung der Argumente erforderlich, die für das Skript „DeploySqlProvider.ps1“ beschrieben wurden. Geben Sie hier auch das Zertifikat an. 

Wir empfehlen Ihnen, das aktuelle Windows Server 2016 Core-Image aus der Marketplace-Verwaltung herunterzuladen. Wenn Sie ein Update installieren müssen, können Sie unter dem lokalen Abhängigkeitspfad ein einzelnes MSU-Paket anordnen. Falls mehr als eine MSU-Datei gefunden wird, schlägt die Ausführung des Skripts fehl.

Hier ist ein Beispiel für das Skript *UpdateSQLProvider.ps1* angegeben, das Sie an der PowerShell-Eingabeaufforderung ausführen können. Achten Sie darauf, die Kontoinformationen und Kennwörter wie erforderlich zu ändern: 

> [!NOTE]
> Der Updateprozess gilt nur für integrierte Systeme.

```powershell
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

## <a name="updatesqlproviderps1-parameters"></a>Parameter in „UpdateSQLProvider.ps1“
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


## <a name="next-steps"></a>Nächste Schritte

[Wartungsvorgänge](azure-stack-sql-resource-provider-maintain.md)
