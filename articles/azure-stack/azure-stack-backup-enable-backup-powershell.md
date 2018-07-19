---
title: Aktivieren der Sicherung für Azure Stack mit PowerShell | Microsoft-Dokumentation
description: Aktivieren Sie den Dienst für die Infrastruktursicherung mit Windows PowerShell, sodass Azure Stack bei einem Fehler wiederhergestellt werden kann.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/10/2018
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 4fb40904e59e78e416d4598472a6adeb498e49f4
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968883"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Aktivieren der Sicherung für Azure Stack mit PowerShell

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

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
| $key            | Geben Sie den **Verschlüsselungsschlüssel** ein, der zum Verschlüsseln der Sicherungen verwendet wird. |
| $password       | Geben Sie das **Kennwort** für den Benutzer ein. |
| $sharepath      | Geben Sie den Pfad zu dem **Sicherungsspeicherort** ein. Für den Pfad zu einer auf einem separaten Gerät gehosteten Dateifreigabe müssen Sie eine UNC-Zeichenfolge (Universal Naming Convention) verwenden. Eine UNC-Zeichenfolge gibt den Speicherort von Ressourcen (z.B. freigegebene Dateien oder Geräte) an. Zur Sicherstellung der Verfügbarkeit der Sicherungsdaten sollte sich das Gerät an einem gesonderten Speicherort befinden. |

   ```powershell
    $username = "domain\backupadmin"
   
    $Secure = Read-Host -Prompt ("Password for: " + $username) -AsSecureString
    $Encrypted = ConvertFrom-SecureString -SecureString $Secure
    $password = ConvertTo-SecureString -String $Encrypted
    
    $BackupEncryptionKeyBase64 = ""
    $tempEncryptionKeyString = ""
    foreach($i in 1..64) { $tempEncryptionKeyString += -join ((65..90) + (97..122) | Get-Random | % {[char]$_}) }
    $tempEncryptionKeyBytes = [System.Text.Encoding]::UTF8.GetBytes($tempEncryptionKeyString)
    $BackupEncryptionKeyBase64 = [System.Convert]::ToBase64String($tempEncryptionKeyBytes)
    $BackupEncryptionKeyBase64
    
    $Securekey = ConvertTo-SecureString -String $BackupEncryptionKeyBase64 -AsPlainText -Force
    $Encryptedkey = ConvertFrom-SecureString -SecureString $Securekey
    $key = ConvertTo-SecureString -String $Encryptedkey
    
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    Set-AzSBackupShare -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $key
   ```
   
##  <a name="confirm-backup-settings"></a>Überprüfen der Sicherungseinstellungen

Führen Sie in der gleichen PowerShell-Sitzung die folgenden Befehle aus:

   ```powershell
    Get-AzsBackupLocation | Select-Object -Property Path, UserName, AvailableCapacity
   ```

Das Ergebnis sollte der folgenden Ausgabe ähneln:

   ```powershell
    Path                        : \\serverIP\AzSBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
   ```

## <a name="next-steps"></a>Nächste Schritte

 - Informationen zum Ausführen einer Sicherung finden Sie unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).  
 - Informationen zum Überprüfen, ob eine Sicherung durchgeführt wurde, finden Sie unter [Bestätigen der erfolgreichen Sicherung im Verwaltungsportal](azure-stack-backup-back-up-azure-stack.md ).
