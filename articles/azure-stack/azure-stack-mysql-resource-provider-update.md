---
title: Aktualisieren des MySQL-Ressourcenanbieters in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den MySQL-Ressourcenanbieter in Azure Stack aktualisieren können.
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
ms.date: 09/04/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 86e72787347cddd399fbdde4cd943b86ba48375f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697831"
---
# <a name="update-the-mysql-resource-provider"></a>Aktualisieren des MySQL-Ressourcenanbieters 

*Gilt für: Integrierte Azure Stack-Systeme*

Unter Umständen wird ein neuer SQL-Ressourcenanbieteradapter veröffentlicht, wenn Azure Stack-Builds aktualisiert werden. Der vorhandene Adapter funktioniert zwar weiterhin, aber es ist ratsam, so schnell wie möglich das Update auf den aktuellen Build durchzuführen. 

>[!IMPORTANT]
>Sie müssen die Updates in der Reihenfolge installieren, in der sie veröffentlicht werden. Es können keine Versionen ausgelassen werden. Informationen hierzu finden Sie in der Versionsliste unter [Bereitstellen des Ressourcenanbieters – Voraussetzungen](.\azure-stack-mysql-resource-provider-deploy.md#prerequisites).

## <a name="update-the-mysql-resource-provider-adapter-integrated-systems-only"></a>Update des MySQL-Ressourcenanbieteradapters (nur für integrierte Systeme)

Unter Umständen wird ein neuer SQL-Ressourcenanbieteradapter veröffentlicht, wenn Azure Stack-Builds aktualisiert werden. Der vorhandene Adapter funktioniert zwar weiterhin, aber es ist ratsam, so schnell wie möglich das Update auf den aktuellen Build durchzuführen.  
 
Verwenden Sie zum Aktualisieren des Ressourcenanbieters das Skript **UpdateMySQLProvider.ps1**. Der Prozess ähnelt dem Prozess zum Installieren eines Ressourcenanbieters, wie im Abschnitt [Bereitstellen des Ressourcenanbieters](#deploy-the-resource-provider) dieses Artikels beschrieben. Das Skript ist im Download des Ressourcenanbieters enthalten. 

Mit dem Skript **UpdateMySQLProvider.ps1** wird eine neue VM mit dem aktuellen Ressourcenanbietercode erstellt, und die Einstellungen werden von der alten VM zur neuen VM migriert. Zu den migrierten Einstellungen gehören Datenbank- und Hostserverinformationen und der erforderliche DNS-Eintrag. 

>[!NOTE]
>Wir empfehlen Ihnen, das aktuelle Windows Server 2016 Core-Image aus der Marketplace-Verwaltung herunterzuladen. Wenn Sie ein Update installieren müssen, können Sie unter dem lokalen Abhängigkeitspfad ein **einzelnes** MSU-Paket platzieren. Beim Skript tritt ein Fehler auf, wenn an diesem Speicherort mehrere MSU-Dateien vorhanden sind.

Für das Skript ist die Verwendung der Argumente erforderlich, die für das Skript „DeployMySqlProvider.ps1“ beschrieben wurden. Geben Sie hier auch das Zertifikat an.  

Hier ist ein Beispiel für das Skript *UpdateMySQLProvider.ps1* angegeben, das Sie an der PowerShell-Eingabeaufforderung ausführen können. Achten Sie darauf, die Kontoinformationen und Kennwörter wie erforderlich zu ändern:  

> [!NOTE] 
> Der Updateprozess gilt nur für integrierte Systeme. 

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
$tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds ` 
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
| **AzureEnvironment** | Die zum Bereitstellen von Azure Stack verwendete Azure-Umgebung des Dienstadministratorkontos. Nur erforderlich, wenn sie nicht AD FS ist. Unterstützte Umgebungsnamen sind **AzureCloud**, **AzureUSGovernment** oder für Azure Active Directory für China **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | Ihre Zertifikatdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ (_obligatorisch_ bei mehreren Knoten) | 
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _Erforderlich_ | 
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 | 
| **RetryDuration** | Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 | 
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  | 
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  | 
| **AcceptLicense** | Überspringt die Aufforderung zum Akzeptieren der GPL-Lizenz.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 
 

## <a name="next-steps"></a>Nächste Schritte
[Warten des MySQL-Ressourcenanbieters](azure-stack-mysql-resource-provider-maintain.md)
