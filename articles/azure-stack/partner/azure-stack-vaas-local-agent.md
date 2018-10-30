---
title: Bereitstellen des lokalen Agents | Microsoft-Dokumentation
description: Stellen Sie den lokalen Agent für Validation-as-a-Service in Azure Stack bereit.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 6a7d1f886bd3a8457b6cad2e9fea11820805e7c9
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49651943"
---
# <a name="deploy-the-local-agent"></a>Bereitstellen des lokalen Agents

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Hier erfahren Sie, wie Sie den lokalen VaaS-Agent (Validation-as-a-Service) zum Überprüfen Ihrer Hardware verwenden. Der lokale Agent muss vor dem Ausführen von Validierungstests in der zu überprüfenden Azure Stack-Lösung bereitgestellt werden.

> [!Note]  
> Stellen Sie sicher, dass die ausgehende Internetverbindung des Computers, auf dem der lokale Agent ausgeführt wird, nicht unterbrochen wird. Auf diesen Computer dürfen nur Benutzer Zugriff haben, die Sie dazu autorisiert haben, VaaS im Auftrag Ihres Mandanten zu verwenden.

Das Bereitstellen des lokalen Agents umfasst folgende Schritte:

1. Installieren Sie den lokalen Agent.
2. Führen Sie Integritätsprüfungen durch.
3. Führen Sie den lokalen Agent aus.

## <a name="download-and-start-the-local-agent"></a>Herunterladen und Starten des lokalen Agents

Laden Sie den Agent auf einen geeigneten Computer in Ihrem Datencenter herunter, der nicht Teil des Azure Stack-Systems ist, aber Zugriff auf alle Azure Stack-Endpunkte hat.

### <a name="machine-prerequisites"></a>Voraussetzungen für Computer

Vergewissern Sie sich, dass Ihr Computer die folgenden Kriterien erfüllt:

- Zugriff auf alle Azure Stack-Endpunkte
- Installation von .NET 4.6 und PowerShell 5.0
- Mindestens 8 GB RAM
- Prozessoren mit mindestens acht Kernen
- Mindestens 200 GB Speicherplatz
- Stabile Verbindung mit dem Internet

Azure Stack ist das getestete System. Der Computer darf nicht Teil von Azure Stack sein oder in der Azure Stack-Cloud gehostet werden.

### <a name="download-and-install-the-agent"></a>Herunterladen und Installieren des Agents

1. Öffnen Sie auf dem Computer, den Sie zum Ausführen der Tests verwenden möchten, Windows PowerShell in einer Eingabeaufforderung mit erhöhten Rechten.
2. Führen Sie den folgenden Befehl aus, um den lokalen Agent herunterzuladen:

    ```PowerShell
    Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.latest.nupkg" -outfile "OnPremAgent.zip"
    Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent -Force
    Set-Location VaaSOnPremAgent\lib\net46
    ```

3. Führen Sie den folgenden Befehl aus, um die Abhängigkeiten des lokalen Agents zu installieren:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

    **Parameter**

    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | aadServiceAdminUser | Der globale Administratorbenutzer für Ihren Azure AD-Mandanten. Beispiel: vaasadmin@contoso.onmicrosoft.com |
    | aadServiceAdminPassword | Das Kennwort für den globalen Administratorbenutzer |
    | AadTenantId | Azure AD-Mandanten-ID für das bei Validation-as-a-Service registrierte Azure-Konto |
    | ExternalFqdn | Sie finden den vollqualifizierten Domänennamen in der Konfigurationsdatei. Anweisungen hierzu finden Sie unter [Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack](azure-stack-vaas-parameters.md). |
    | Region | Die Region des Azure AD-Mandanten |

Der Befehl lädt ein PIR-Image (Betriebssystem-VHD) und eine Kopie aus einem Azure-Blobspeicher in Ihren Azure Stack-Speicher herunter.

![Herunterladen der erforderlichen Komponenten](media/installingprereqs.png)

> [!Note]
> Wenn die Geschwindigkeit beim Herunterladen dieser Images niedrig ist, laden Sie sie einzeln auf eine lokale Dateifreigabe herunter, und geben Sie den Parameter **-LocalPackagePath** *FileShareOrLocalPath* an. Weitere Informationen zum PIR-Download finden Sie unter [Problembehandlung für Validation-as-a-Service](azure-stack-vaas-troubleshoot.md) im Abschnitt [Behandeln langsamer Netzwerkverbindung](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity).

## <a name="checks-before-starting-the-tests"></a>Prüfungen vor Beginn der Tests

Die Tests führen Remoteaktionen aus. Der Computer, auf dem die Tests ausgeführt werden, muss Zugriff auf die Azure Stack-Endpunkte haben. Andernfalls funktionieren die Tests nicht. Verwenden Sie bei Verwendung des lokalen VaaS-Agents den Computer, auf dem der Agent ausgeführt wird. Sie können überprüfen, ob Ihr Computer Zugriff auf die Azure Stack-Endpunkte hat. Führen Sie hierzu die folgenden Schritte aus:

1. Prüfen Sie, ob der Basis-URI erreicht werden kann. Öffnen Sie eine CMD-Eingabeaufforderung oder Bash-Shell, und führen Sie den folgenden Befehl aus. Ersetzen Sie dabei `<EXTERNALFQDN>` durch den externen vollqualifizierten Domänennamen Ihrer Umgebung:

    ```bash
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Öffnen Sie einen Webbrowser, und navigieren Sie zu `https://adminportal.<EXTERNALFQDN>`, um zu prüfen, ob das MAS-Portal erreichbar ist.

3. Melden Sie sich mit den Werten für den Azure AD-Dienstadministratornamen und das entsprechende Kennwort an, die beim Erstellen des Testdurchlaufs bereitgestellt wurden.

4. Überprüfen Sie die Systemintegrität mithilfe des PowerShell-Cmdlets **Test-AzureStack**, wie in [Ausführen eines Validierungstests für Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test) beschrieben. Beheben Sie vor dem Starten von Tests alle Warnungen und Fehler.

## <a name="run-the-agent"></a>Ausführen des Agents

1. Öffnen Sie Windows PowerShell in einer Eingabeaufforderung mit erhöhten Rechten.

2. Führen Sie den folgenden Befehl aus:

    ````PowerShell
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ````

      **Parameter**  
    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | VaaSUserId | Benutzer-ID zur Anmeldung beim VaaS-Portal (etwa UserName@Contoso.com) |
    | VaaSTenantId | Azure AD-Mandanten-ID für das bei Validation-as-a-Service registrierte Azure-Konto |

    > [!Note]  
    > Beim Ausführen des Agents muss als aktuelles Arbeitsverzeichnis der Speicherort der ausführbaren Datei für den Task-Engine-Host **Microsoft.VaaSOnPrem.TaskEngineHost.exe** verwendet werden.

Werden keine Fehler angezeigt, war der lokale Agent erfolgreich. Der folgende Beispieltext wird im Konsolenfenster angezeigt:

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Gestarteter Agent](media/startedagent.png)

Ein Agent wird anhand seines Namens eindeutig identifiziert. Standardmäßig wird der vollqualifizierte Domänenname (Fully Qualified Domain Name, FQDN) des Computers verwendet, auf dem er gestartet wurde. Sie müssen das Fenster minimieren, um versehentliche Klicks im Fenster zu vermeiden, da durch eine Fokusänderung alle anderen Aktionen angehalten werden.

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung für Validation-as-a-Service](azure-stack-vaas-troubleshoot.md)
- [Validation-as-a-Service – wichtige Begriffe](azure-stack-vaas-key-concepts.md)
- [Planen des ersten Tests](azure-stack-vaas-schedule-test-pass.md)