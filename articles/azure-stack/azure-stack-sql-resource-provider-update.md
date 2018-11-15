---
title: Aktualisieren des SQL-Ressourcenanbieters in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den SQL-Ressourcenanbieter in Azure Stack aktualisieren können.
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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: d1bb7da3ad3862e5b15f6440b4c607143b3c5b2a
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683222"
---
# <a name="update-the-sql-resource-provider"></a>Aktualisieren des SQL-Ressourcenanbieters

*Gilt für: Integrierte Azure Stack-Systeme*

Unter Umständen wird ein neuer SQL-Ressourcenanbieter veröffentlicht, wenn Azure Stack auf einen neuen Build aktualisiert wird. Der vorhandene Adapter funktioniert zwar weiterhin, aber es ist ratsam, so schnell wie möglich das Update auf den aktuellen Build durchzuführen.

>[!IMPORTANT]
>Sie müssen die Updates in der Reihenfolge installieren, in der sie veröffentlicht werden. Es können keine Versionen ausgelassen werden. Informationen hierzu finden Sie in der Versionsliste unter [Bereitstellen des Ressourcenanbieters – Voraussetzungen](.\azure-stack-sql-resource-provider-deploy.md#prerequisites).

## <a name="overview"></a>Übersicht

Verwenden Sie zum Aktualisieren des Ressourcenanbieters das Skript *UpdateSQLProvider.ps1*. Das Skript ist im Download des neuen SQL-Ressourcenanbieters enthalten. Der Updateprozess ähnelt dem Prozess zum [Bereitstellen des Ressourcenanbieters](.\azure-stack-sql-resource-provider-deploy.md). Das Updateskript verwendet die gleichen Argumente wie das Skript „DeploySqlProvider.ps1“, und Sie müssen Zertifikatinformationen angeben.

### <a name="update-script-processes"></a>Aktualisieren von Skriptprozessen

Das Skript *UpdateSQLProvider.ps1* erstellt einen neuen virtuellen Computer (VM) mit dem neuesten Ressourcenanbietercode.

> [!NOTE]
> Wir empfehlen Ihnen, das aktuelle Windows Server 2016 Core-Image aus der Marketplace-Verwaltung herunterzuladen. Wenn Sie ein Update installieren müssen, können Sie unter dem lokalen Abhängigkeitspfad ein **einzelnes** MSU-Paket platzieren. Beim Skript tritt ein Fehler auf, wenn an diesem Speicherort mehrere MSU-Dateien vorhanden sind.

Nachdem das Skript *UpdateSQLProvider.ps1* eine neue VM erstellt hat, werden die folgenden Einstellungen aus der alten Anbieter-VM migriert:

* Datenbankinformationen
* Hostserverinformationen
* Erforderlicher DNS-Eintrag

### <a name="update-script-powershell-example"></a>Beispiel für ein PowerShell-Aktualisierungsskript

Das folgende Skript können Sie über eine PowerShell ISE mit erhöhten Rechten bearbeiten und ausführen. 

Denken Sie daran, die Kontoinformationen und Kennwörter den Anforderungen Ihrer Umgebung entsprechend zu ändern.

> [!NOTE]
> Dieser Updateprozess gilt nur für integrierte Azure Stack-Systeme.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.5.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported environment names are AzureCloud, AzureUSGovernment, or AzureChinaCloud. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service administrator account (this can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential required for privileged endpoint access.
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
  -AzureEnvironment $AzureEnvironment `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `

 ```

## <a name="updatesqlproviderps1-parameters"></a>Parameter in „UpdateSQLProvider.ps1“

Wenn Sie das Skript ausführen, können Sie die folgenden Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** | Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **AzureEnvironment** | Die zum Bereitstellen von Azure Stack verwendete Azure-Umgebung des Dienstadministratorkontos. Nur für Azure AD-Bereitstellungen erforderlich. Unterstützte Umgebungsnamen sind **AzureCloud**, **AzureUSGovernment** oder für ein chinesisches Azure AD **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | Ihre Zertifikatdatei (.pfx) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional für einzelne Knoten, aber obligatorisch für mehrere Knoten_ |
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _Erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** |Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen. | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |

## <a name="next-steps"></a>Nächste Schritte

[Wartungsvorgänge](azure-stack-sql-resource-provider-maintain.md)
