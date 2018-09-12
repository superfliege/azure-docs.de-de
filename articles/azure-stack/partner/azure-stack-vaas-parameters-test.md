---
title: Testparameter für Validation-as-a-Service in Azure Stack | Microsoft-Dokumentation
description: Themenbereich zu Konfigurationsdatei und Parameter für den Testdurchlauf für Validation-as-a-Service in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 9f042779e3463f0d75dc6327b3553156edbf162a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162222"
---
# <a name="test-parameters-for-validation-as-a-service-azure-stack"></a>Testparameter für Validation-as-a-Service in Azure Stack

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Wählen Sie vor dem Ausführen einer Testsammlung aus Validation-as-a-Service (VaaS) eine Lösung aus, und erstellen Sie einen Testdurchlauf.

- Melden Sie sich mit Ihren registrierten VaaS-Mandantenanmeldeinformationen an.
- Erstellen Sie eine neue Lösung (indem Sie **Lösung hinzufügen**), oder wählen Sie eine vorhandene Lösung aus.
- Wählen Sie die Schaltfläche **Start** (Starten) auf der Workflowkachel **Test Passes** (Testdurchläufe) aus.

> [!Note]  
> Erstellen Sie für jede eindeutige Computergruppe/Hardwarekonfiguration, die Sie überprüfen, einen neuen Lösungseintrag und für jede Buildbereitstellung in dieser Computergruppe einen neuen Testdurchlauf.

Sie müssen die zum Ausführen der Tests erforderlichen Parameter auf der Seite **Test Pass Information** (Informationen zum Testdurchlauf) eingeben. Geben Sie diese Parameter ein, wenn Sie einen neuen Test- oder Prüfdurchlauf starten. Die meisten Parameter weisen die Werte auf, die Sie bei der Bereitstellung von Azure Stack angegeben haben.

Sie können die in der [Testparametertabelle](#test-parameters) aufgeführten Werte manuell eingeben oder die Konfigurationsdatei für die Bereitstellung hochladen, die alle Azure Stack-Stempelinformationen enthält. Nachdem Sie die Datei hochgeladen haben, werden die Werte durch das Portal aus der Datei geladen.

> [!Note]  
> Sie können nach den Testparameterwerten suchen und sie eingeben, indem Sie die Konfigurationsdatei suchen und in das Portal laden.

## <a name="export-the-test-parameters-using-powershell"></a>Exportieren der Testparameter mithilfe von PowerShell

1. Melden Sie sich beim DVM-Computer oder einem anderen Computer mit Zugriff auf die Azure Stack-Umgebung an.
2. Öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und führen Sie Folgendes aus:

    ````PowerShell  
        $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation'
        ConvertTo-Json $params > stampinfoproperties.json
    ````

3. Laden Sie **stampinfoproperties.json** in das VaaS-Portal hoch.

## <a name="find-the-test-parameters-in-the-configuration-file"></a>Suchen nach den Testparametern in der Konfigurationsdatei

Die Testparameterwerte finden Sie auch, indem Sie die ECE-**Konfigurationsdatei** öffnen. Die Datei befindet sich im folgenden Pfad auf dem DVM-Computer:  
`C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787`

## <a name="test-parameters"></a>Testparameter 

| Parameter    | BESCHREIBUNG |
|-------------|-----------------|
| Azure Stack-Build                      | Nummer des bereitgestellten Azure Stack-Builds oder der bereitgestellten Version, Beispiel: 1.0.170330.0 | 
| Mandanten-ID                              | Azure Active Directory-Mandant, der bei der Bereitstellung von Azure Stack angegeben wurde. Suchen Sie in der Konfigurationsdatei nach **AADTenant**, und wählen Sie den Wert **GUID** im `Id`-Tag aus. | 
| Region                                 | Die Region der Azure Stack-Bereitstellung. Suchen Sie in der Konfigurationsdatei nach **Region**. | 
| Azure Resource Manager                | Azure Resource Manager-Endpunkt des Mandanten, z.B. `https://management.<ExternalFqdn>` | 
| Resource Manager-Administrator                 | Azure Resource Manager-Endpunkt des Administrators. Beispiel: `https://adminmanagement.<ExternalFqdn>` | 
| Externer FQDN                          | Externer FQDN der Umgebung. Suchen Sie in der Konfigurationsdatei nach **ExternalFqdn**. | 
| Mandantenbenutzer                            | Der Azure Active Directory-Mandantenadministrator, der entweder bereits bereitgestellt wurde oder im Azure AD-Verzeichnis vom Dienstadministrator bereitgestellt werden muss. Weitere Informationen zur Bereitstellung von Mandantenkonten finden Sie unter [Hinzufügen neuer Azure Stack-Mandantenkonten in Azure Active Directory](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-new-user-aad). Dieser Wert wird von dem Test zur Durchführung von Vorgängen auf Mandantenebene verwendet, wie z.B. zum Bereitstellen von Vorlagen für die Bereitstellung von Ressourcen (VMs, Speicherkonten usw.) und die Ausführung von Workloads. | 
| Dienstadministratorbenutzer             | Der Azure Active Directory-Administrator des Mandanten des Azure AD-Verzeichnisses, der während der Azure Stack-Bereitstellung angegeben wird. Suchen Sie in der Konfigurationsdatei nach **AADTenant**, und wählen Sie den Wert im `UniqueName`-Tag in der Konfigurationsdatei aus. | 

## <a name="checks-before-starting-the-tests"></a>Prüfungen vor Beginn der Tests

Im Rahmen der Tests werden Remotevorgänge durchgeführt. Der Computer, auf dem die Tests durchgeführt werden, müssen Zugriff auf die Azure Stack-Endpunkte haben. Andernfalls funktioniert der Test nicht. Wenn Sie den lokalen VaaS-Agent verwenden, verwenden Sie den Computer, auf dem der Agent ausgeführt wird. Sie können sicherstellen, dass Ihr Computer Zugriff auf Azure Stack-Punkte hat, indem Sie Folgendes prüfen.

1. Prüfen Sie, ob der Basis-URI erreicht werden kann. Öffnen Sie eine Eingabeaufforderung oder eine Bash-Shell, und führen Sie den folgenden Befehl aus:

    ```bash  
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Öffnen Sie einen Webbrowser, und navigieren Sie zu `https://adminportal.<EXTERNALFQDN>`, um zu prüfen, ob das MAS-Portal erreicht werden kann.

3. Melden Sie sich mit den Werten für den Azure AD-Dienstadministratornamen und das entsprechende Kennwort an, die beim Erstellen des Testdurchlaufs bereitgestellt wurden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Validation-as-a-Service in Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
