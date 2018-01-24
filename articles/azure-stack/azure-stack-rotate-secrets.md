---
title: Rotieren von Geheimnissen in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Geheimnisse in Azure Stack rotieren.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: mabrigg
ms.openlocfilehash: e2e9d93af3889714ade1d0364a6f747c184e6d75
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Rotieren von Geheimnissen in Azure Stack

*Gilt für: In Azure Stack integrierte Systeme*

Aktualisieren Sie Ihre Kennwörter für Azure Stack-Komponenten in einem regelmäßigen Rhythmus.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Aktualisieren der Kennwörter für Baseboard-Verwaltungscontroller (BMC)

Die Baseboard-Verwaltungscontroller (BMCs) überwachen den physischen Zustand Ihrer Server. Die Spezifikationen und Anweisungen zur Aktualisierung des Kennworts des BMC variieren je nach OEM (Original Equipment Manufacturer, Originalgerätehersteller).

1. Aktualisieren Sie den BMC auf den physischen Azure Stack-Server gemäß den Anweisungen des OEM. Das Kennwort für jeden BMC in Ihrer Umgebung muss identisch sein.
2. Öffnen Sie in Azure Stack einen privilegierten Endpunkt. Anweisungen finden Sie unter [Verwenden des privilegierten Endpunkts in Azure Stack](azure-stack-privileged-endpoint.md).
3. Nachdem die PowerShell-Eingabeaufforderung sich je nach Umgebung in **[IP-Adresse oder ERCS-VM-Name]: PS>** oder in **[azs-ercs01]: PS>** geändert hat, führen Sie `Set-BmcPassword` aus, indem Sie `invoke-command` ausführen. Übergeben Sie die Sitzungsvariable des privilegierten Endpunkts als Parameter. Beispiel: 

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    ```
    
    Sie können auch die statische PowerShell-Version mit den Kennwörtern als Codezeilen verwenden:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Sicherheit und Azure Stack finden Sie unter [Sicherheitsstatus der Azure Stack-Infrastruktur](azure-stack-security-foundations.md).
