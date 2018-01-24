---
title: "Aktivieren der Sicherung für Azure Stack mit PowerShell | Microsoft-Dokumentation"
description: Aktivieren Sie den Infrastructure Backup-Dienst mit Windows PowerShell, sodass Azure Stack bei einem Fehler wiederhergestellt werden kann.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 7DFEFEBE-D6B7-4BE0-ADC1-1C01FB7E81A6
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: e0be5f1916ddb653550e6428201356290560c00e
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Aktivieren der Sicherung für Azure Stack mit PowerShell

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Aktivieren Sie den Infrastructure Backup-Dienst mit Windows PowerShell, sodass Azure Stack bei einem Fehler wiederhergestellt werden kann. Sie können auf die PowerShell-Cmdlets zugreifen, um über den Endpunkt der Bedienerverwaltung die Sicherung zu aktivieren und zu starten sowie Sicherungsinformationen abzurufen.

## <a name="download-azure-stack-tools"></a>Herunterladen der Azure Stack-Tools

PowerShell muss installiert und für Azure Stack und die Azure Stack-Tools konfiguriert sein. Weitere Informationen finden Sie unter [Einrichten von PowerShell in Azure Stack](https://review.docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-configure-quickstart).

##  <a name="load-the-connect-and-infrastructure-modules"></a>Laden der Connect- und Infrastructure-Module

Öffnen Sie Windows PowerShell mit erhöhten Rechten, und führen Sie die folgenden Befehle aus:

   ```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1
    
    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
   ```

##  <a name="setup-rm-environment-and-log-into-the-operator-management-endpoint"></a>Einrichten der RM-Umgebung und Anmelden beim Endpunkt der Bedienerverwaltung

Bearbeiten Sie in der gleichen PowerShell-Sitzung das folgende PowerShell-Skript durch Hinzufügen der Variablen für Ihre Umgebung. Führen Sie das aktualisierte Skript aus, um die RM-Umgebung einzurichten und sich beim Endpunkt der Bedienerverwaltung anzumelden.

| Variable    | BESCHREIBUNG |
|---          |---          |
| $TenantName | Azure Active Directory-Mandantenname |
| Operator account name        | Name Ihres Azure Stack-Bedienerkontos |
| Azure Resource Manager Endpoint | URL für Azure Resource Manager |

   ```powershell
   # Specify Azure Active Directory tenant name
    $TenantName = "contoso.onmicrosoft.com"
    
    # Set the module repository and the execution policy
    Set-PSRepository `
      -Name "PSGallery" `
      -InstallationPolicy Trusted
    
    Set-ExecutionPolicy RemoteSigned `
      -force
    
    # Configure the Azure Stack operator’s PowerShell environment.
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint "https://adminmanagement.seattle.contoso.com"
    
    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience "https://graph.windows.net/"
    
    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName $TenantName `
      -EnvironmentName AzureStackAdmin
    
    # Sign-in to the operator's console.
    Login-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID 
    
   ```
## <a name="generate-a-new-encryption-key"></a>Generieren eines neuen Verschlüsselungsschlüssels

Führen Sie in der gleichen PowerShell-Sitzung die folgenden Befehle aus:

   ```powershell
   $encryptionkey = New-EncryptionKeyBase64
   ```

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

Set-AzSBackupShare -Location $location -Path $path -UserName $credential.UserName -Password $credential.GetNetworkCredential().password -EncryptionKey $encryptionkey 

   ```
##  <a name="confirm-backup-settings"></a>Überprüfen der Sicherungseinstellungen

Führen Sie in der gleichen PowerShell-Sitzung die folgenden Befehle aus:

   ```powershell
   Get-AzsBackupLocation | Select-Object -Property Path, UserName, Password | ConvertTo-Json 
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