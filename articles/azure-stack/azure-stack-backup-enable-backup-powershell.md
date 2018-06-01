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
ms.openlocfilehash: 4faa6930c37f9d491a3efa4b34519dbb13761a9d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34074931"
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

Anweisungen zum Konfigurieren der PowerShell-Umgebung finden Sie unter [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md).

## <a name="generate-a-new-encryption-key"></a>Generieren eines neuen Verschlüsselungsschlüssels

PowerShell muss installiert und für Azure Stack und die Azure Stack-Tools konfiguriert sein.
 - Weitere Informationen finden Sie unter [Einrichten von PowerShell in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).
 - Weitere Informationen finden Sie unter [Herunterladen von Azure Stack-Tools von GitHub](azure-stack-powershell-download.md).

Öffnen Sie Windows PowerShell mit erhöhten Rechten, und führen Sie die folgenden Befehle aus:
   
   ```powershell
    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
   ```
   
Führen Sie in der gleichen PowerShell-Sitzung die folgenden Befehle aus:

   ```powershell
   $encryptionkey = New-EncryptionKeyBase64
   ```

> [!Warning]  
> Sie müssen die AzureStack-Tools zum Generieren des Schlüssels verwenden.

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Angeben der Sicherungsfreigabe, der Anmeldeinformationen und des Verschlüsselungsschlüssels zum Aktivieren der Sicherung

Bearbeiten Sie in der gleichen PowerShell-Sitzung das folgende PowerShell-Skript durch Hinzufügen der Variablen für Ihre Umgebung. Führen Sie das aktualisierte Skript aus, um die Sicherungsfreigabe, die Anmeldeinformationen und den Verschlüsselungsschlüssel für den Infrastructure Backup-Dienst anzugeben.

| Variable        | BESCHREIBUNG   |
|---              |---                                        |
| $username       | Geben Sie den **Benutzernamen** mit der Domäne und dem Benutzernamen für den Speicherort des freigegebenen Laufwerks ein. Beispiel: `Contoso\administrator`. |
| $password       | Geben Sie das **Kennwort** für den Benutzer ein. |
| $sharepath      | Geben Sie den Pfad zu dem **Sicherungsspeicherort** ein. Für den Pfad zu einer auf einem separaten Gerät gehosteten Dateifreigabe müssen Sie eine UNC-Zeichenfolge (Universal Naming Convention) verwenden. Eine UNC-Zeichenfolge gibt den Speicherort von Ressourcen (z.B. freigegebene Dateien oder Geräte) an. Zur Sicherstellung der Verfügbarkeit der Sicherungsdaten sollte sich das Gerät an einem gesonderten Speicherort befinden. |

   ```powershell
    $username = "domain\backupoadmin"
    $password = "password"
    $credential = New-Object System.Management.Automation.PSCredential($username, ($password| ConvertTo-SecureString -asPlainText -Force))  
    $location = Get-AzsLocation
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"
    
    Set-AzSBackupShare -Location $location.Name -Path $sharepath -UserName $credential.UserName -Password $credential.GetNetworkCredential().password -EncryptionKey $encryptionkey
   ```
   
##  <a name="confirm-backup-settings"></a>Überprüfen der Sicherungseinstellungen

Führen Sie in der gleichen PowerShell-Sitzung die folgenden Befehle aus:

   ```powershell
   Get-AzsBackupLocation | Select-Object -ExpandProperty externalStoreDefault | Select-Object -Property Path, UserName, Password | ConvertTo-Json
   ```

Das Ergebnis sollte der folgenden JSON-Ausgabe ähneln:

   ```json
      {
    "ExternalStoreDefault":  {
        "Path":  "\\\\serverIP\\AzSBackupStore\\contoso.com\\seattle",
        "UserName":  "domain\backupoadmin",
        "Password":  null
       }
   } 
   ```

## <a name="next-steps"></a>Nächste Schritte

 - Informationen zum Ausführen einer Sicherung finden Sie unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).  
 - Informationen zum Überprüfen, ob eine Sicherung durchgeführt wurde, finden Sie unter [Bestätigen der erfolgreichen Sicherung im Verwaltungsportal](azure-stack-backup-back-up-azure-stack.md ).