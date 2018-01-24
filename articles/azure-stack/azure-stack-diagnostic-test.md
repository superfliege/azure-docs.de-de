---
title: "Ausführen eines Validierungstests in Azure Stack | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Protokolldateien für die Diagnose in Azure Stack sammeln."
services: azure-stack
author: mattbriggs
manager: femila
cloud: azure-stack
ms.assetid: D44641CB-BF3C-46FE-BCF1-D7F7E1D01AFA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: mabrigg
ms.openlocfilehash: 53ef19628b40c4a008143c867c9e7867ac91854d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="run-a-validation-test-for-azure-stack"></a>Ausführen eines Validierungstests für Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*
 
Sie können den Status Ihrer Azure Stack-Instanz prüfen. Wenn ein Problem vorliegt, wenden Sie sich an den Microsoft-Support. Der Support fordert Sie auf, das Cmdlet „Test-AzureStack“ auf Ihrem Verwaltungsknoten auszuführen. Der Validierungstest isoliert den Fehler. Der Support kann dann die detaillierten Protokolle analysieren, sich auf den Bereich konzentrieren, in dem der Fehler aufgetreten ist, und mit Ihnen bei der Lösung des Problems zusammenarbeiten.

## <a name="run-test-azurestack"></a>Ausführen von „Test-AzureStack“

Wenn ein Problem vorliegt, wenden Sie sich an den Microsoft-Support, und führen Sie anschließend **Test-AzureStack** aus.

1. Sie haben ein Problem.
2. Wenden Sie sich an den Microsoft-Support.
3. Führen Sie **Test-AzureStack** auf dem privilegierten Endpunkt aus.
    1. Greifen Sie auf den privilegierten Endpunkt zu. Anweisungen finden Sie unter [Verwenden des privilegierten Endpunkts in Azure Stack](azure-stack-privileged-endpoint.md). 
    2. Melden Sie sich auf dem Verwaltungshost als **AzureStack\CloudAdmin** an.
    3. Starten Sie PowerShell als Administrator.
    4. Führen Sie diesen Befehl aus: `Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint`
    5. Führen Sie diesen Befehl aus: `Test-AzureStack`
4. Wenn bei einem Test Fehler gemeldet werden, führen Sie `Get-AzureStackLog -FilterByRole SeedRing -OutputPath <Log output path>` aus. Das Cmdlet sammelt die Protokolle von „Test-AzureStack“. Weitere Informationen zu Diagnoseprotokollen finden Sie unter [Azure Stack-Diagnosetools](azure-stack-diagnostics.md).
5. Senden Sie die **SeedRing**-Protokolle an den Microsoft-Support. Der Microsoft-Support arbeitet mit Ihnen zusammen, um das Problem zu beheben.

## <a name="reference-for-test-azurestack"></a>Referenz für „Test-AzureStack“

Dieser Abschnitt enthält eine Übersicht über das Cmdlet „Test-AzureStack“ und eine Zusammenfassung des Validierungsberichts.

### <a name="test-azurestack"></a>Test-AzureStack

Überprüft den Status von Azure Stack. Das Cmdlet meldet den Status Ihrer Hardware und Software für Azure Stack. Supportmitarbeiter können diesen Bericht verwenden, um die Zeit für die Bearbeitung von Azure Stack-Supportvorgängen zu verkürzen.

> [!Note]  
> „Test-AzureStack“ kann Fehler erkennen, die nicht zu Cloudausfällen führen, wie z.B. einen einzelnen ausgefallenen Datenträger oder einen einzelnen Ausfall eines physischen Hostknotens.

#### <a name="syntax"></a>Syntax

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parameter

| Parameter               | Wert           | Erforderlich | Standard |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | PSCredential    | Nein        | FALSE   |
| DoNotDeployTenantVm     | SwitchParameter | Nein        | FALSE   |
| AdminCredential         | PSCredential    | Nein        | Nicht verfügbar      |
| StorageConnectionString | Zeichenfolge          | Nein        | Nicht verfügbar      |
| Auflisten                    | SwitchParameter | Nein        | FALSE   |
| Ignorieren                  | Zeichenfolge          | Nein        | Nicht verfügbar      |
| Include                 | Zeichenfolge          | Nein        | Nicht verfügbar      |

Dieses Cmdlet unterstützt diese gängigen Parameter: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable und OutVariable. Weitere Informationen finden Sie unter [about_Commonparameters](http://go.microsoft.com/fwlink/?LinkID=113216). 

### <a name="examples-of-test-azurestack"></a>Beispiele von „Test-AzureStack“

Die folgenden Beispiele gehen davon aus, dass Sie als **CloudAdmin** angemeldet sind und auf den privilegierten Endpunkt (PEP) zugreifen. Anweisungen finden Sie unter [Verwenden des privilegierten Endpunkts in Azure Stack](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Interaktives Ausführen von „Test-AzureStack“ ohne Cloudszenarien

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
      Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Ausführen von „Test-AzureStack“ mit Cloudszenarien

Mit „Test-AzureStack“ können Sie Cloudszenarien für Ihre Azure Stack-Instanz durchspielen. Zu diesen Szenarien gehören:

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
> Sie können die Cloudszenarien nicht mit AD FS-Anmeldeinformationen (Active Directory Federated Services, Active Directory-Verbunddienste) ausführen. Auf das Cmdlet **Test AzureStack** kann nur auf dem PEP zugegriffen werden. Der PEP unterstützt jedoch keine AD FS-Anmeldeinformationen.

Geben Sie den Benutzernamen des Cloudadministrators im UPN-Format serviceadmin@contoso.onmicrosoft.com (AAD) ein. Geben Sie bei Aufforderung das Kennwort für das Cloudadministratorkonto ein.

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Ausführen von „Test-AzureStack“ ohne Cloudszenarien

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Listen Sie verfügbare Testszenarien auf:

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Ausführen eines angegebenen Tests

Führen Sie in einer PEP-Sitzung Folgendes aus:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

So schließen Sie bestimmte Tests aus

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="validation-test"></a>Validierungstest

In der folgenden Tabelle sind die Validierungstests zusammengefasst, die von „Test AzureStack“ ausgeführt werden.

| Name                                                                                                                              |
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

## <a name="next-steps"></a>Nächste Schritte

 - Weitere Informationen zu Azure Stack-Diagnosetools und zur Protokollierung von Problemen finden Sie unter [Azure Stack-Diagnosetools ](azure-stack-diagnostics.md).
 - Weitere Informationen zur Problembehandlung finden Sie unter [Problembehandlung von Microsoft Azure Stack](azure-stack-troubleshooting.md).