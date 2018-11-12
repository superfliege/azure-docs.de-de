---
title: Ausführen eines Validierungstests in Azure Stack | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Protokolldateien für die Diagnose in Azure Stack sammeln.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 11/02/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: af601005c7c8bd8fa7fe335879991caa34187927
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236974"
---
# <a name="run-a-validation-test-for-azure-stack"></a>Ausführen eines Validierungstests für Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*
 
Sie können den Status Ihrer Azure Stack-Instanz prüfen. Wenn ein Problem vorliegt, wenden Sie sich an den Microsoft-Support. Der Support fordert Sie auf, das Cmdlet **Test-AzureStack** auf Ihrem Verwaltungsknoten auszuführen. Der Validierungstest isoliert den Fehler. Der Support kann dann die detaillierten Protokolle analysieren, sich auf den Bereich konzentrieren, in dem der Fehler aufgetreten ist, und mit Ihnen bei der Lösung des Problems zusammenarbeiten.

## <a name="run-test-azurestack"></a>Ausführen von „Test-AzureStack“

Wenn ein Problem vorliegt, wenden Sie sich an den Microsoft-Support, und führen Sie anschließend **Test-AzureStack** aus.

1. Sie haben ein Problem.
2. Wenden Sie sich an den Microsoft-Support.
3. Führen Sie **Test-AzureStack** auf dem privilegierten Endpunkt aus.
    1. Greifen Sie auf den privilegierten Endpunkt zu. Anweisungen finden Sie unter [Verwenden des privilegierten Endpunkts in Azure Stack](azure-stack-privileged-endpoint.md). 
    2. Melden Sie sich auf dem ASDK als **AzureStack\CloudAdmin** beim Verwaltungshost an.  
    In einem integrierten System müssen Sie die IP-Adresse für den privilegierten Endpunkt für die Verwaltung verwenden, der von Ihrem OEM-Hardwarehersteller bereitgestellt wird.
    3. Starten Sie PowerShell als Administrator.
    4. Führen Sie diesen Befehl aus: `Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint`
    5. Führen Sie diesen Befehl aus: `Test-AzureStack`
4. Wenn bei einem Test **FAIL** gemeldet wird, führen Sie `Get-AzureStackLog -FilterByRole SeedRing -OutputSharePath “<path>” -OutputShareCredential $cred` aus. Das Cmdlet sammelt die Protokolle von Test-AzureStack. Weitere Informationen zu Diagnoseprotokollen finden Sie unter [Azure Stack-Diagnosetools](azure-stack-diagnostics.md). Sie sollten keine Protokolle sammeln und sich nicht an den Microsoft-Support wenden, wenn bei Tests **WARN** gemeldet wird.
5. Senden Sie die **SeedRing**-Protokolle an den Microsoft-Support. Der Microsoft-Support arbeitet mit Ihnen zusammen, um das Problem zu beheben.

## <a name="reference-for-test-azurestack"></a>Referenz für „Test-AzureStack“

Dieser Abschnitt enthält eine Übersicht über das Cmdlet „Test-AzureStack“ und eine Zusammenfassung des Validierungsberichts.

### <a name="test-azurestack"></a>Test-AzureStack

Überprüft den Status von Azure Stack. Das Cmdlet meldet den Status Ihrer Hardware und Software für Azure Stack. Supportmitarbeiter können diesen Bericht verwenden, um die Zeit für die Bearbeitung von Azure Stack-Supportvorgängen zu verkürzen.

> [!Note]  
> **Test-AzureStack** kann Fehler erkennen, die nicht zu Cloudausfällen führen, wie z.B. einen einzelnen ausgefallenen Datenträger oder den Ausfall eines einzelnen physischen Hostknotens.

#### <a name="syntax"></a>Syntax

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parameter

| Parameter               | Wert           | Erforderlich | Standard |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | Zeichenfolge    | Nein        | FALSE   |
| DoNotDeployTenantVm     | SwitchParameter | Nein        | FALSE   |
| AdminCredential         | PSCredential    | Nein        | Nicht verfügbar      |
| Auflisten                    | SwitchParameter | Nein        | FALSE   |
| Ignorieren                  | Zeichenfolge          | Nein        | Nicht verfügbar      |
| Include                 | Zeichenfolge          | Nein        | Nicht verfügbar      |
| BackupSharePath         | Zeichenfolge          | Nein        | Nicht verfügbar      |
| BackupShareCredential   | PSCredential    | Nein        | Nicht verfügbar      |


Dieses Cmdlet unterstützt diese gängigen Parameter: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable und OutVariable. Weitere Informationen finden Sie unter [about_Commonparameters](https://go.microsoft.com/fwlink/?LinkID=113216). 

### <a name="examples-of-test-azurestack"></a>Beispiele von „Test-AzureStack“

Die folgenden Beispiele gehen davon aus, dass Sie als **CloudAdmin** angemeldet sind und auf den privilegierten Endpunkt (PEP) zugreifen. Anweisungen finden Sie unter [Verwenden des privilegierten Endpunkts in Azure Stack](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Interaktives Ausführen von „Test-AzureStack“ ohne Cloudszenarien

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Ausführen von „Test-AzureStack“ mit Cloudszenarien

Mit **Test-AzureStack** können Sie Cloudszenarien für Ihre Azure Stack-Instanz durchspielen. Zu diesen Szenarien gehören:

 - Erstellen von Ressourcengruppen
 - Erstellen von Plänen
 - Erstellen von Angeboten
 - Erstellen von Speicherkonten
 - Erstellen eines virtuellen Computers
 - Ausführen von Blobvorgängen mit dem im Testszenario angelegten Speicherkonto
 - Ausführen von Warteschlangenvorgängen mit dem im Testszenario angelegten Speicherkonto
 - Ausführen von Tabellenvorgängen mit dem im Testszenario angelegten Speicherkonto

Für die Cloudszenarien sind Anmeldeinformationen eines Cloudadministrators erforderlich. 
> [!Note]  
> Sie können die Cloudszenarien nicht mit AD FS-Anmeldeinformationen (Active Directory Federation Services, Active Directory-Verbunddienste) ausführen. Auf das Cmdlet **Test AzureStack** kann nur auf dem PEP zugegriffen werden. Der PEP unterstützt jedoch keine AD FS-Anmeldeinformationen.

Geben Sie den Benutzernamen des Cloudadministrators im UPN-Format serviceadmin@contoso.onmicrosoft.com (Azure AD) ein. Geben Sie bei Aufforderung das Kennwort für das Cloudadministratorkonto ein.

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Ausführen von „Test-AzureStack“ ohne Cloudszenarien

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Listen Sie verfügbare Testszenarien auf:

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Ausführen eines angegebenen Tests

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

So schließen Sie bestimmte Tests aus

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="run-test-azurestack-to-test-infrastructure-backup-settings"></a>Ausführen von „Test-AzureStack“ zum Testen der Einstellungen für die Infrastruktursicherung

Vor der Konfiguration der Infrastruktursicherung können Sie mit dem Test **AzsBackupShareAccessibility** den Sicherungsfreigabepfad und die Sicherungsanmeldeinformationen testen.

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
````
Nach der Konfiguration der Sicherung können Sie AzsBackupShareAccessibility ausführen, um zu überprüfen, ob über das ERCS mit Ausführung einer PEP-Sitzung auf die Freigabe zugegriffen werden kann:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility
````

So testen Sie neue Anmeldeinformationen mit der konfigurierten Sicherungsfreigabe über die Ausführung einer PEP-Sitzung

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential <PSCredential for backup share>
````

### <a name="validation-test"></a>Validierungstest

In der folgenden Tabelle sind die Validierungstests zusammengefasst, die von **Test-AzureStack** ausgeführt werden.

| NAME                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Zusammenfassung der Cloudhostinginfrastruktur für Azure Stack                                                                                  |
| Zusammenfassung der Azure Stack-Speicherdienste                                                                                              |
| Zusammenfassung der Infrastrukturrolleninstanz von Azure Stack                                                                                  |
| Nutzung der Cloudhostinginfrastruktur für Azure Stack                                                                              |
| Kapazität der Azure Stack-Infrastruktur                                                                                               |
| Zusammenfassung von Azure Stack-Portal und API                                                                                                |
| Zusammenfassung der Azure Resource Manager-Zertifikate für Azure Stack                                                                                               |
| Infrastrukturverwaltungscontroller und die Infrastrukturrollen „Netzwerkcontroller“, „Speicherdienste“ und „Privilegierter Endpunkt“          |
| Infrastrukturverwaltungscontroller und die Infrastrukturrolleninstanzen „Netzwerkcontroller“, „Speicherdienste“ und „Privilegierter Endpunkt“ |
| Zusammenfassung der Infrastrukturrollen von Azure Stack                                                                                           |
| Azure Stack Cloud Service Fabric Services                                                                                         |
| Leistung der Infrastrukturrolleninstanz von Azure Stack                                                                              |
| Zusammenfassung der Cloudhostleistung von Azure Stack                                                                                        |
| Zusammenfassung der Ressourcennutzung von Azure Stack-Diensten                                                                                  |
| Azure Stack-Skalierungseinheit für kritische Ereignisse (letzte 8 Stunden)                                                                             |
| Zusammenfassung der physischen Datenträger für Azure Stack-Speicherdienste                                                                               |
|Zusammenfassung der Verfügbarkeit von Azure Stack-Sicherungsfreigaben                                                                                     |

## <a name="next-steps"></a>Nächste Schritte

 - Weitere Informationen zu Azure Stack-Diagnosetools und zur Protokollierung von Problemen finden Sie unter [Azure Stack-Diagnosetools ](azure-stack-diagnostics.md).
 - Weitere Informationen zur Problembehandlung finden Sie unter [Problembehandlung von Microsoft Azure Stack](azure-stack-troubleshooting.md).
