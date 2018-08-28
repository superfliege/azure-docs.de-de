---
title: Automatisieren der Azure Stack-Überprüfung mit PowerShell | Microsoft-Dokumentation
description: Sie können die Azure Stack-Überprüfung mit PowerShell automatisieren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 1cb4b1a7cfd72ea302676244a53af58e77215aa9
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234454"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Automatisieren der Azure Stack-Überprüfung mit PowerShell 

Validation-as-a-Service (VaaS) bietet die Möglichkeit, den Start von Tests mithilfe des Skripts **LaunchVaaSTests.ps1** zu automatisieren.

Sie können PowerShell für den folgenden Workflow verwenden:

- Workflow für den Testdurchlauf

Dieses Skript umfasst die vier Elemente eines Workflows:

- Installieren der erforderlichen Komponenten
- Installieren und Starten des lokalen Agents.
- Starten einer Kategorie von Tests, etwa Integrations-, Funktions- oder Zuverlässigkeitstests
- Melden der Ergebnisse der einzelnen Testdurchläufe zur Überwachung und Berichtsdateierstellung

## <a name="launch-the-test-pass-workflow"></a>Starten des Workflows für den Testdurchlauf

1. Öffnen Sie eine PowerShell-Eingabeaufforderung mit erhöhten Rechten.

2. Führen Sie das folgende Skript aus, um das Automatisierungsskript herunterzuladen:

    ````PowerShell  
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://vaastestpacksprodeastus.blob.core.windows.net/packages/Microsoft.VaaS.Scripts.3.0.0.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ````

3. Führen Sie das folgende Skript mit Ihren Werten aus:

    ````PowerShell  
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>"  -AsPlainText -Force)
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<ServiceAdminUser>", (ConvertTo-SecureString "<ServiceAdminPassword>" -AsPlainText -Force)
    $TenantAdminCreds = New-Object System.Management.Automation.PSCredential "<TenantAdminUser>", (ConvertTo-SecureString "<TenantAdminPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
        -VaaSAccountTenantId <VaaSAccountTenantId> `
        -VaaSSolutionName <VaaSSolutionName> `
        -VaaSTestPassName <VaaSTestPassName> `
        -VaaSTestCategories Integration,Functional `
        -MaxScriptWaitTimeInHours 12 `
        -ServiceAdminCreds $ServiceAdminCreds `
    ````

    **Parameter**

    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | VaaSUserld | Ihre VaaS-Benutzer-ID | 
    | VaaSUserPassword | Ihr VaaS-Kennwort |
    | ServiceAdminUser | Ihr Azure Stack-Dienstadministratorkonto  |
    | ServiceAdminPassword | Ihr Kennwort für den Azure Stack-Dienst  |
    | TenantAdminUser | Der Administrator für den primären Mandanten  |
    | TenantAdminPassword | Das Kennwort für den primären Mandanten  |
    | FunctionalCategory| Integration, Funktion oder Zuverlässigkeit. Wenn Sie mehrere Werte verwenden, trennen Sie diese durch ein Komma.  |

4. Sehen Sie sich die Ergebnisse des Tests an. Weitere Informationen zum Lesen von Testergebnissen finden Sie unter [Überwachen eines Tests mit Azure Stack Validation-as-a-Service](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Nächste Schritte

 - Lesen Sie mehr über [Azure Stack Validation-as-a-Service](https://docs.microsoft.com/azure/azure-stack/partner).
 - Weitere Informationen zu PowerShell in Azure Stack finden Sie in der Referenz zum [Azure Stack-Modul](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0).