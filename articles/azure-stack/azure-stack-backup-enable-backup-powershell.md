---
title: Aktivieren der Sicherung für Azure Stack mit PowerShell | Microsoft-Dokumentation
description: Aktivieren Sie den Dienst für die Infrastruktursicherung mit Windows PowerShell, sodass Azure Stack bei einem Fehler wiederhergestellt werden kann.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 08/16/2018
ms.openlocfilehash: 10d7303c4323305e177cf006b9a259a817dc695e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247475"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Aktivieren der Sicherung für Azure Stack mit PowerShell

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Aktivieren Sie den Infrastructure Backup-Dienst mit Windows PowerShell, um regelmäßige Sicherungen von Folgendem zu erstellen:
 - Interner Identitätsdienst und Stammzertifikat
 - Benutzertarife, -angebote und -abonnements
 - KeyVault-Geheimnisse
 - RBAC-Rollen und -Richtlinien für Benutzer

Sie können auf die PowerShell-Cmdlets zugreifen, um über den Endpunkt der Bedienerverwaltung die Sicherung zu aktivieren und zu starten sowie Sicherungsinformationen abzurufen.

## <a name="prepare-powershell-environment"></a>Vorbereiten der PowerShell-Umgebung

Anweisungen zum Konfigurieren der PowerShell-Umgebung finden Sie unter [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md). Weitere Informationen zur Anmeldung bei Azure Stack finden Sie unter [Konfigurieren der Betreiberumgebung und Anmelden bei Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Angeben der Sicherungsfreigabe, der Anmeldeinformationen und des Verschlüsselungsschlüssels zum Aktivieren der Sicherung

Bearbeiten Sie in der gleichen PowerShell-Sitzung das folgende PowerShell-Skript durch Hinzufügen der Variablen für Ihre Umgebung. Führen Sie das aktualisierte Skript aus, um die Sicherungsfreigabe, die Anmeldeinformationen und den Verschlüsselungsschlüssel für den Infrastructure Backup-Dienst anzugeben.

| Variable        | BESCHREIBUNG   |
|---              |---                                        |
| $username       | Geben Sie den **Benutzernamen** ein, und verwenden Sie dabei die Domäne und den Benutzernamen für den Speicherort des freigegebenen Laufwerks mit ausreichendem Zugriff zum Lesen und Schreiben von Dateien. Beispiel: `Contoso\backupshareuser`. |
| $password       | Geben Sie das **Kennwort** für den Benutzer ein. |
| $sharepath      | Geben Sie den Pfad zu dem **Sicherungsspeicherort** ein. Für den Pfad zu einer auf einem separaten Gerät gehosteten Dateifreigabe müssen Sie eine UNC-Zeichenfolge (Universal Naming Convention) verwenden. Eine UNC-Zeichenfolge gibt den Speicherort von Ressourcen (z.B. freigegebene Dateien oder Geräte) an. Zur Sicherstellung der Verfügbarkeit der Sicherungsdaten sollte sich das Gerät an einem gesonderten Speicherort befinden. |
| $frequencyInHours | Die Häufigkeit in Stunden bestimmt, wie oft Sicherungen erstellt werden. Der Standardwert ist 12. Scheduler unterstützt maximal 12 und mindestens 4.|
| $retentionPeriodInDays | Die Vermerkdauer in Tagen bestimmt, wie viele Tage Sicherungen am externen Speicherort beibehalten werden. Der Standardwert ist 7. Scheduler unterstützt maximal 14 und mindestens 2. Nach Ablauf ihrer Vermerkdauer werden Sicherungen automatisch aus dem externen Speicherort gelöscht.|
|     |     |

   ```powershell
    # Example username:
    $username = "domain\backupadmin"
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"
   
    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString
    
    # The encryption key is generated using the New-AzsEncryptionKeyBase64 cmdlet provided in Azure Stack PowerShell.
    # Make sure to store your encryption key in a secure location after it is generated.
    $Encryptionkey = New-AzsEncryptionKeyBase64
    $key = ConvertTo-SecureString -String ($Encryptionkey) -AsPlainText -Force

    Set-AzsBackupShare -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $key
   ```
   
##  <a name="confirm-backup-settings"></a>Überprüfen der Sicherungseinstellungen

Führen Sie in der gleichen PowerShell-Sitzung die folgenden Befehle aus:

   ```powershell
    Get-AzsBackupLocation | Select-Object -Property Path, UserName
   ```

Das Ergebnis sollte der folgenden Beispielausgabe ähneln:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
   ```

## <a name="update-backup-settings"></a>Aktualisieren der Sicherungseinstellungen
In der gleichen PowerShell-Sitzung können Sie die Standardwerte für die Vermerkdauer und die Häufigkeit von Sicherungen aktualisieren. 

   ```powershell
    #Set the backup frequency and retention period values.
    $frequencyInHours = 10
    $retentionPeriodInDays = 5

    Set-AzsBackupShare -BackupFrequencyInHours $frequencyInHours -BackupRetentionPeriodInDays $retentionPeriodInDays
    Get-AzsBackupLocation | Select-Object -Property Path, UserName, AvailableCapacity, BackupFrequencyInHours, BackupRetentionPeriodInDays
   ```

Das Ergebnis sollte der folgenden Beispielausgabe ähneln:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
    BackupFrequencyInHours      : 10
    BackupRetentionPeriodInDays : 5
   ```

## <a name="next-steps"></a>Nächste Schritte

 - Informationen zum Ausführen einer Sicherung finden Sie unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).  
 - Informationen zum Überprüfen, ob eine Sicherung durchgeführt wurde, finden Sie unter [Bestätigen der erfolgreichen Sicherung im Verwaltungsportal](azure-stack-backup-back-up-azure-stack.md ).
