---
title: Überprüfen einer Azure Stack-Sicherung mit dem ASDK | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Azure Stack-Sicherung für integrierte Systeme mit dem ASDK überprüfen.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: 12d5c2448bc6a5241f6aa4e283acdf9c5a0f602a
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265650"
---
# <a name="use-the-asdk-to-validate-an-azure-stack-backup"></a>Verwenden des ASDK zum Überprüfen der Azure Stack-Sicherung
Nach der Bereitstellung von Azure Stack und von Benutzerressourcen wie Angeboten, Plänen, Kontingenten und Abonnements sollten Sie [Azure Stack-Infrastruktursicherung aktivieren](../azure-stack-backup-enable-backup-console.md). Das Planen und Ausführen regelmäßiger Infrastruktursicherungen stellt sicher, dass Daten zur Infrastrukturverwaltung nicht verloren gehen, wenn ein Hardware- oder Dienstausfall auftritt.

> [!TIP]
> Es wird empfohlen, [eine bedarfsgesteuerte Sicherung auszuführen](../azure-stack-backup-back-up-azure-stack.md), bevor Sie dieses Verfahren beginnen, um sicherzustellen, dass Sie über eine Kopie der neuesten Infrastrukturdaten verfügen. Stellen Sie sicher, die Sicherungs-ID zu erfassen, nachdem eine Sicherung erfolgreich abgeschlossen wurde. Diese ID wird bei der Cloudwiederherstellung benötigt. 

Azure Stack-Infrastruktursicherungen enthalten wichtige Daten zu Ihrer Cloud, die während einer erneuten Bereitstellung von Azure Stack wiederhergestellt werden können. Sie können das ASDK verwenden, um diese Sicherungen ohne Auswirkungen auf den Schutz Ihrer Produktionscloud zu überprüfen. 

Das Überprüfen von Sicherungen im ASDK wird für die folgenden Szenarien unterstützt:

|Szenario|Zweck|
|-----|-----|
|Überprüfen von Infrastruktursicherungen einer integrierten Lösung|Kurze Überprüfung, ob die Daten in der Sicherung gültig sind.|
|Informieren über den vollständigen Wiederherstellungsworkflow|Verwenden Sie das ASDK zum Überprüfen der gesamten Sicherung und Wiederherstellung.|
|     |     |

Das folgende Szenario wird beim Überprüfen von Sicherungen auf dem ASDK **nicht** unterstützt:

|Szenario|Zweck|
|-----|-----|
|ASDK zum Erstellen von Sicherung und Wiederherstellung|Stellen Sie Daten aus einer früheren Version des ASDK in einer neueren Version wieder her.|
|     |     |


## <a name="cloud-recovery-deployment"></a>Bereitstellung von Cloudwiederherstellungen
Infrastruktursicherungen aus der Bereitstellung integrierter Systeme können durch Ausführen einer Bereitstellung von Cloudwiederherstellungen des ASDK überprüft werden. Bei dieser Art der Bereitstellung werden bestimmte Dienstdaten aus einer Sicherung wiederhergestellt, nachdem das ASDK auf dem Hostcomputer installiert wurde.



### <a name="cloud-recovery-prerequisites"></a>Voraussetzungen für die Cloudwiederherstellung
Vor dem Starten einer Bereitstellung für die Cloudwiederherstellung des ASDK benötigen Sie die folgenden Informationen:

|Voraussetzung|BESCHREIBUNG|
|-----|-----|
|Freigabepfad der Sicherung|Der UNC-Dateifreigabepfad der neuesten Azure Stack-Sicherung, die zum Wiederherstellen von Informationen zur Azure Stack-Infrastruktur verwendet werden. Diese lokale Freigabe wird während des Bereitstellungsprozesses für Cloudwiederherstellungen erstellt.|
|Schlüssel der Sicherungsverschlüsselung|Der Verschlüsselungsschlüssel, der zum Planen der Infrastruktursicherung im Azure Stack-Verwaltungsportal verwendet wurde.|
|Sicherungs-ID für die Wiederherstellung|Die Sicherungs-ID im alphanumerischen Format „xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx“, die die Sicherung bezeichnet, für die eine Cloudwiederherstellung durchgeführt werden soll.|
|Zeitserver-IP-Adresse|Eine gültige Zeitserver IP-Adresse (z.B. 132.163.97.2) ist für Azure Stack-Bereitstellungen erforderlich.|
|Kennwort für externes Zertifikat|Das Kennwort für das externe Zertifikat, das von Azure Stack verwendet werden soll. Die Sicherung der Zertifizierungsstelle enthält externe Zertifikate, die mit diesem Kennwort wiederhergestellt werden müssen.|
|     |     | 

## <a name="prepare-the-host-computer"></a>Vorbereiten des Hostcomputers 
Wie bei einer normalen ASDK-Bereitstellung muss die ASDK-Hostsystemumgebung für die Installation vorbereitet werden. Nach der Vorbereitung des Development Kit-Hostcomputers startet der Computer von der Festplatte des virtuellen Computers (CloudBuilder.vhdx), sodass die ASDK-Bereitstellung beginnen kann.

Laden Sie auf dem ASDK-Hostcomputer die zur gesicherten Version von Azure Stack gehörige Datei „cloudbuilder.vhdx“ herunter, und befolgen Sie die Anweisungen für das [Vorbereiten des ASDK-Hostcomputers](asdk-prepare-host.md).

Nach dem Neustart des Hostservers aus der Datei „cloudbuilder.vhdx“ müssen Sie eine Dateifreigabe erstellen und Ihre Sicherungsdaten in diese kopieren. Die Dateifreigabe muss für das Konto, unter dem das Setup ausgeführt wird, zugänglich sein. In diesen PowerShell-Beispielbefehlen ist dies das Administratorkonto: 

```powershell
$shares = New-Item -Path "c:\" -Name "Shares" -ItemType "directory"
$azsbackupshare = New-Item -Path $shares.FullName -Name "AzSBackups" -ItemType "directory"
New-SmbShare -Path $azsbackupshare.FullName -FullAccess ($env:computername + "\Administrator")  -Name "AzSBackups"
```

Kopieren Sie als Nächstes Ihre aktuellsten Azure Stack-Sicherungsdateien in die neu erstellte Freigabe. Die Ordnerstruktur in der Freigabe sollte `\\<ComputerName>\AzSBackups\MASBackup\<BackupID>\` lauten.

## <a name="deploy-the-asdk-in-cloud-recovery-mode"></a>Bereitstellen des ASDK im Cloudwiederherstellungsmodus
Das Skript **InstallAzureStackPOC.ps1** dient zum Initiieren der Cloudwiederherstellung. 

> [!IMPORTANT]
> Für die ASDK-Installation wird genau eine Netzwerkschnittstellenkarte (NIC) für das Netzwerk unterstützt. Wenn Sie mehrere Netzwerkadapter nutzen, sollten Sie vor dem Ausführen des Bereitstellungsskripts sicherstellen, dass nur einer aktiviert ist (und alle anderen deaktiviert).

Passen Sie die folgenden PowerShell-Befehle an Ihre Umgebung an, und führen Sie sie aus, um das ASDK im Cloudwiederherstellungsmodus bereitzustellen:

```powershell
cd C:\CloudDeployment\Setup     
$adminPass = Get-Credential Administrator
$key = ConvertTo-SecureString "<Your backup encryption key>" -AsPlainText -Force ` 
$certPass = Read-Host -AsSecureString  

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
-BackupEncryptionKeyBase64 $key -BackupStoreCredential $adminPass -BackupId "<Backup ID to restore>" `
-TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

## <a name="restore-infrastructure-data-from-backup"></a>Wiederherstellen von Infrastrukturdaten aus einer Sicherung
Nach einer erfolgreichen Bereitstellung für die Cloudwiederherstellung müssen Sie die Wiederherstellung mit dem Cmdlet **Restore-AzureStack** abschließen. 

Nach der Anmeldung als Azure Stack-Operator [installieren Sie Azure Stack PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) und führen dann den folgenden Befehl aus (ersetzen Sie den Parameter `Name` durch Ihre Sicherungs-ID):

```powershell
Restore-AzsBackup -Name "<BackupID>"
```
Warten Sie nach dem Aufrufen dieses Cmdlets 60 Minuten, bevor Sie die Überprüfung der Sicherungsdaten im ASDK, das in der Cloud wiederhergestellt wurde, starten.

## <a name="next-steps"></a>Nächste Schritte
[Registrieren von Azure Stack](asdk-register.md)

